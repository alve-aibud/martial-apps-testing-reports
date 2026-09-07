# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/11-exams-grading-belts/owner.exam-grading-promotion.spec.ts >> Owner schedules an exam, grades it, and the promotion sticks >> OWP-019 - the owner can schedule an exam and grade candidates @OWP-019 @destructive
- Location: e2e/specs/smoke-testing/11-exams-grading-belts/owner.exam-grading-promotion.spec.ts:134:7

# Error details

```
Error: could not reset "[e2e] OWP-019 Exam" for a fresh grading pass via POST .../exams/cmtr1r3oj00ttygace7fdtcd8/reset-for-testing.

expect(received).toBeTruthy()

Received: false
```

# Test source

```ts
  73  |  * and on staging that is byte-identical to a route invented on the spot,
  74  |  * while `GET /exams` returns 200 on both (so it is not auth, and not the
  75  |  * router being down). The route exists in `develop` with no environment gate,
  76  |  * so staging is simply behind.
  77  |  *
  78  |  * The failure is ORDER-DEPENDENT, which is what makes it confusing:
  79  |  *
  80  |  *   run 1 on a fresh staging club  -> PASSES. No `[e2e] OWP-019 Exam` yet, so
  81  |  *                                     the `else` branch schedules it through
  82  |  *                                     the real form and never calls reset.
  83  |  *   run 2 onward                   -> `OWP-019` fails at the reset above.
  84  |  *                                     `OWP-020` and `CR-014` fall with it -
  85  |  *                                     they never call the endpoint themselves
  86  |  *                                     but both read state `OWP-019` produces
  87  |  *                                     (the exam on the promotions page, and
  88  |  *                                     the candidate's promoted belt).
  89  |  *
  90  |  * Nothing is left wedged when it fails: `OWP-019` dies BEFORE promoting, so
  91  |  * the candidate's belt is never changed and the `finally` restore is a no-op.
  92  |  *
  93  |  * So a staging run is "correct" with SIX failures - the three cross-env reds
  94  |  * (`SEC-043`, `SEN-008`, `XC-001`) plus these three - until the endpoint
  95  |  * ships there. On `dev` it is deployed and all three pass.
  96  |  *
  97  |  * `mode: 'default'`, not left to `fullyParallel` scheduling - `OWP-020` and
  98  |  * `CR-014` both depend on `OWP-019` having ALREADY finalized the exam.
  99  |  *
  100 |  * ============================================================================
  101 |  * THE WHOLE FILE HOLDS THE MEMBER-ACCOUNT LOCK, ACQUIRED IN `beforeAll` AND
  102 |  * RELEASED IN `afterAll` - not `withMemberAccountLock` around one test.
  103 |  * ============================================================================
  104 |  *
  105 |  * `ROLE_MEMBER` is promoted the moment `OWP-019` finalizes and stays promoted
  106 |  * until this file's OWN `afterAll` restores them, three tests and however
  107 |  * long they take later. `withMemberAccountLock` cannot express that: it
  108 |  * acquires and releases within a single callback, so wrapping only `OWP-019`
  109 |  * would free the lock the instant that one test finished - leaving the
  110 |  * member promoted but UNPROTECTED for `OWP-020`, `CR-014`, and the restore
  111 |  * itself, which is exactly the window `MEM-003`/`MEM-004` (this club's other
  112 |  * belt-promoting test on this same shared account) can land in under
  113 |  * `npm run test:destructive:dev`'s full parallel load. Found live,
  114 |  * 2026-08-30 - this file had never taken the lock at all until then. See
  115 |  * `acquireMemberAccountLock`'s own header in `member-account-lock.ts`.
  116 |  */
  117 | 
  118 | test.use(asRole('ownerPrimary'));
  119 | test.describe.configure({ mode: 'default' });
  120 | 
  121 | const EXAM_NAME = '[e2e] OWP-019 Exam';
  122 | const EXAM_DAYS_AHEAD = 20;
  123 | 
  124 | let candidateMemberId: string | undefined;
  125 | let candidateOriginalBeltId: string | undefined;
  126 | let examId: string | undefined;
  127 | let releaseMemberLock: (() => void) | undefined;
  128 | 
  129 | test.beforeAll(async () => {
  130 |   releaseMemberLock = await acquireMemberAccountLock();
  131 | });
  132 | 
  133 | test.describe('Owner schedules an exam, grades it, and the promotion sticks', () => {
  134 |   test('OWP-019 - the owner can schedule an exam and grade candidates @OWP-019 @destructive', async ({
  135 |     page,
  136 |     sitesPage,
  137 |     gradingPage,
  138 |     clubId,
  139 |     ownerApi,
  140 |   }, testInfo) => {
  141 |     testInfo.annotations.push({
  142 |       type: 'manual-scenario',
  143 |       description: 'manual-qa/role-owner-primary.md#OWP-019 (steps 1-9)',
  144 |     });
  145 | 
  146 |     const candidate = await findMemberIdByEmail(ownerApi, clubId, ROLES.member.email);
  147 |     expect(candidate, `${ROLES.member.email} was not found on the roster.`).toBeTruthy();
  148 |     candidateMemberId = candidate?.memberId;
  149 | 
  150 |     const detail = await ownerApi.get(`${env.apiURL}/clubs/${clubId}/members/${candidateMemberId}`, {
  151 |       headers: { 'x-app-id': env.appId },
  152 |     });
  153 |     const detailBody = await detail.json().catch(() => null);
  154 |     candidateOriginalBeltId = detailBody?.data?.belt?.id ?? detailBody?.data?.beltId;
  155 |     expect(candidateOriginalBeltId, "could not read the candidate's current belt before promoting them.").toBeTruthy();
  156 | 
  157 |     const existing = await findExam(ownerApi, clubId, EXAM_NAME);
  158 |     let examiners: string[];
  159 | 
  160 |     if (existing) {
  161 |       // The exam already exists - RESET it rather than skip re-grading, no
  162 |       // matter whether an earlier run finalized it or crashed before getting
  163 |       // that far. `resetExamForTesting` clears its results and puts it back
  164 |       // to `scheduled`; its date, examiners and candidate are untouched (see
  165 |       // the helper's own header in `scratch-site.ts`), so grading can proceed
  166 |       // directly without re-running the create form at all.
  167 |       examId = existing.examId;
  168 |       const reset = await resetExamForTesting(ownerApi, clubId, examId);
  169 |       expect(
  170 |         reset,
  171 |         `could not reset "${EXAM_NAME}" for a fresh grading pass via ` +
  172 |           `POST .../exams/${examId}/reset-for-testing.`,
> 173 |       ).toBeTruthy();
      |         ^ Error: could not reset "[e2e] OWP-019 Exam" for a fresh grading pass via POST .../exams/cmtr1r3oj00ttygace7fdtcd8/reset-for-testing.
  174 | 
  175 |       testInfo.annotations.push({
  176 |         type: 'create-path-not-exercised',
  177 |         description:
  178 |           `"${EXAM_NAME}" already existed, so this run reset it via the ` +
  179 |           `test-only endpoint and re-graded it, rather than re-scheduling ` +
  180 |           `through the create form (that path is exercised on the first ` +
  181 |           `run in a given environment).`,
  182 |       });
  183 | 
  184 |       const participants = await examParticipants(ownerApi, clubId, examId);
  185 |       examiners = participants.examiners;
  186 |       expect(
  187 |         examiners,
  188 |         `the reset exam "${EXAM_NAME}" came back with fewer than 2 examiners - ` +
  189 |           `reset-for-testing should not have touched them.`,
  190 |       ).toHaveLength(2);
  191 |     } else {
  192 |       // Steps 1-3: schedule, in the exam form the room offers.
  193 |       const room = await ensureExamFixturesRoom(ownerApi, clubId, 'OWP-019');
  194 |       const opened = await sitesPage.openCreateEventForm(
  195 |         room.roomRoute,
  196 |         'exam',
  197 |         new Date(Date.now() + 9 * 86_400_000).toISOString().slice(0, 10),
  198 |         '14:00',
  199 |       );
  200 |       expect(opened, 'the exam form did not open from the exam-fixtures room.').toBe(true);
  201 | 
  202 |       const belt = await sitesPage.selectExamTargetBelt();
  203 |       expect(belt, 'a target belt was clicked but it had no label.').not.toEqual('');
  204 | 
  205 |       await sitesPage.pickExamDate(EXAM_DAYS_AHEAD);
  206 |       examiners = await sitesPage.selectExamPeople('Examiners *', 2);
  207 |       expect(examiners, 'two distinct examiners should have been ticked.').toHaveLength(2);
  208 |       expect(new Set(examiners).size, `the same examiner was ticked twice: ${examiners.join(', ')}`).toBe(2);
  209 | 
  210 |       await sitesPage.selectExamPersonByName('Candidates *', candidate?.name ?? '');
  211 | 
  212 |       expect(
  213 |         await sitesPage.examValidationErrors(),
  214 |         'the exam form is already complaining before submit.',
  215 |       ).toEqual([]);
  216 | 
  217 |       examId = await sitesPage.createSimpleEvent(EXAM_NAME, 'exams');
  218 |       expect(examId, 'the exam was submitted but came back without an id.').toBeTruthy();
  219 | 
  220 |       // Move it into the gradable past - see this file's header and
  221 |       // `GradingPage`'s for why yesterday, not the create form's own picker.
  222 |       const moved = await moveExamToPast(ownerApi, clubId, examId as string, 1);
  223 |       expect(moved, 'could not move the exam into the past via the API.').toBeTruthy();
  224 |     }
  225 | 
  226 |     // Steps 5-6: the grading screen, examiners present.
  227 |     //
  228 |     // NOT just `markExaminersPresent(examiners)` - on the RESET path the exam
  229 |     // may already have both examiners marked present, left over from an
  230 |     // earlier run (`reset-for-testing` clears results but not
  231 |     // `ExamExaminer.isPresent`; see `examinersNeedingPresence`'s own header
  232 |     // in `scratch-site.ts`). The toggle FLIPS, so clicking an already-present
  233 |     // examiner would un-mark them. Re-reading who still needs it right before
  234 |     // clicking makes this correct regardless of what state the reset exam
  235 |     // happens to be in - including the CREATE path, where every examiner
  236 |     // needs it and this is a no-op difference.
  237 |     await gradingPage.open(clubId, examId as string);
  238 |     const needPresence = await examinersNeedingPresence(ownerApi, clubId, examId as string);
  239 |     await gradingPage.markExaminersPresent(needPresence);
  240 | 
  241 |     // Step 7: pass the candidate.
  242 |     await gradingPage.passCandidate(candidate?.name ?? '');
  243 | 
  244 |     // Steps 8-9: finalize.
  245 |     const finalized = await gradingPage.finalize();
  246 |     expect(finalized, 'submitting the finalize confirmation sent no request.').toBeDefined();
  247 |     expect(finalized?.ok, 'POST .../grading/finalize was refused.').toBeTruthy();
  248 |     expect(finalized?.passed, 'exactly one candidate should have been marked passed.').toBe(1);
  249 | 
  250 |     // The promotion itself - the candidate's belt actually changed.
  251 |     const promoted = await ownerApi.get(`${env.apiURL}/clubs/${clubId}/members/${candidateMemberId}`, {
  252 |       headers: { 'x-app-id': env.appId },
  253 |     });
  254 |     const promotedBody = await promoted.json().catch(() => null);
  255 |     const newBeltId = promotedBody?.data?.belt?.id ?? promotedBody?.data?.beltId;
  256 |     expect(
  257 |       newBeltId,
  258 |       "finalizing should have changed the candidate's belt away from what it was before.",
  259 |     ).not.toBe(candidateOriginalBeltId);
  260 |   });
  261 | 
  262 |   /**
  263 |    * `OWP-020` steps 1-3. Largely a read/navigation proof - the actual
  264 |    * mutation is `OWP-019`'s finalize above; this scenario is about the
  265 |    * PROMOTIONS SCREEN surfacing that, not a second promotion. `promotions/
  266 |    * page.tsx` has no mutation of its own; its "Start Grading" / "Edit
  267 |    * Details" buttons are `router.push` into the exams/grading flow already
  268 |    * covered.
  269 |    */
  270 |   test('OWP-020 - the promotions workflow page reflects the exam @OWP-020 @destructive', async ({
  271 |     page,
  272 |     clubId,
  273 |   }, testInfo) => {
```