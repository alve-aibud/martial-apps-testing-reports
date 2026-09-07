# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/11-exams-grading-belts/sensei.exam-grading-promotion.spec.ts >> Sensei schedules an exam, grades it, and the promotion sticks >> SEN-004 - a sensei can schedule an exam and grade candidates @SEN-004 @destructive
- Location: e2e/specs/smoke-testing/11-exams-grading-belts/sensei.exam-grading-promotion.spec.ts:52:7

# Error details

```
Error: could not move the exam into the past via the API.

expect(received).toBeTruthy()

Received: false
```

# Test source

```ts
  24  |  * **The candidate here is `ROLE_PARENT`, deliberately NOT `ROLE_MEMBER`** -
  25  |  * the owner-side file already uses `ROLE_MEMBER`, and running both files in
  26  |  * parallel must not mean two exams racing to promote and restore the SAME
  27  |  * account's belt.
  28  |  *
  29  |  * **Holds `acquireParentAccountLock` for the whole file** (`beforeAll` to
  30  |  * `afterAll`), the same shape `owner.exam-grading-promotion.spec.ts` uses for
  31  |  * `ROLE_MEMBER` and for the same reason - the promotion made in `SEN-004`
  32  |  * must survive untouched through `SEN-005` and the final restore, so nothing
  33  |  * that also writes `ROLE_PARENT`'s belt can run concurrently for that whole
  34  |  * span, not just for one test.
  35  |  */
  36  | 
  37  | test.use(asRole('sensei'));
  38  | test.describe.configure({ mode: 'default' });
  39  | 
  40  | const EXAM_NAME = '[e2e] SEN-004 Exam';
  41  | const EXAM_DAYS_AHEAD = 21;
  42  | 
  43  | let candidateMemberId: string | undefined;
  44  | let candidateOriginalBeltId: string | undefined;
  45  | let releaseParentLock: (() => void) | undefined;
  46  | 
  47  | test.beforeAll(async () => {
  48  |   releaseParentLock = await acquireParentAccountLock();
  49  | });
  50  | 
  51  | test.describe('Sensei schedules an exam, grades it, and the promotion sticks', () => {
  52  |   test('SEN-004 - a sensei can schedule an exam and grade candidates @SEN-004 @destructive', async ({
  53  |     sitesPage,
  54  |     gradingPage,
  55  |     clubId,
  56  |     ownerApi,
  57  |   }, testInfo) => {
  58  |     testInfo.annotations.push({
  59  |       type: 'manual-scenario',
  60  |       description: 'manual-qa/role-sensei.md#SEN-004',
  61  |     });
  62  | 
  63  |     const candidate = await findMemberIdByEmail(ownerApi, clubId, ROLES.parent.email);
  64  |     expect(candidate, `${ROLES.parent.email} was not found on the roster.`).toBeTruthy();
  65  |     candidateMemberId = candidate?.memberId;
  66  | 
  67  |     const detail = await ownerApi.get(`${env.apiURL}/clubs/${clubId}/members/${candidateMemberId}`, {
  68  |       headers: { 'x-app-id': env.appId },
  69  |     });
  70  |     const detailBody = await detail.json().catch(() => null);
  71  |     candidateOriginalBeltId = detailBody?.data?.belt?.id ?? detailBody?.data?.beltId;
  72  |     expect(candidateOriginalBeltId, "could not read the candidate's current belt before promoting them.").toBeTruthy();
  73  | 
  74  |     const existing = await findExam(ownerApi, clubId, EXAM_NAME);
  75  |     if (existing?.isFinalized) {
  76  |       testInfo.annotations.push({
  77  |         type: 'create-path-not-exercised',
  78  |         description:
  79  |           `"${EXAM_NAME}" already exists and is finalized from an earlier run, and a ` +
  80  |           `graded exam cannot be deleted, so this run did not re-schedule or re-grade.`,
  81  |       });
  82  |       return;
  83  |     }
  84  |     if (existing && !existing.isFinalized) {
  85  |       // An earlier run created it but never got as far as finalizing (e.g. a
  86  |       // mid-run failure). Still future-dated at this point, so still
  87  |       // deletable - clear it rather than leave a second create colliding
  88  |       // with it.
  89  |       await ownerApi
  90  |         .delete(`${env.apiURL}/clubs/${clubId}/exams/${existing.examId}`, {
  91  |           headers: { 'x-app-id': env.appId },
  92  |         })
  93  |         .catch(() => undefined);
  94  |     }
  95  | 
  96  |     const room = await ensureExamFixturesRoom(ownerApi, clubId, 'SEN-004');
  97  |     const opened = await sitesPage.openCreateEventForm(
  98  |       room.roomRoute,
  99  |       'exam',
  100 |       new Date(Date.now() + 9 * 86_400_000).toISOString().slice(0, 10),
  101 |       '15:00',
  102 |     );
  103 |     expect(opened, 'the exam form did not open from the exam-fixtures room.').toBe(true);
  104 | 
  105 |     const belt = await sitesPage.selectExamTargetBelt();
  106 |     expect(belt, 'a target belt was clicked but it had no label.').not.toEqual('');
  107 | 
  108 |     await sitesPage.pickExamDate(EXAM_DAYS_AHEAD);
  109 |     const examiners = await sitesPage.selectExamPeople('Examiners *', 2);
  110 |     expect(examiners, 'two distinct examiners should have been ticked.').toHaveLength(2);
  111 |     expect(new Set(examiners).size, `the same examiner was ticked twice: ${examiners.join(', ')}`).toBe(2);
  112 | 
  113 |     await sitesPage.selectExamPersonByName('Candidates *', candidate?.name ?? '');
  114 | 
  115 |     expect(
  116 |       await sitesPage.examValidationErrors(),
  117 |       'the exam form is already complaining before submit.',
  118 |     ).toEqual([]);
  119 | 
  120 |     const examId = await sitesPage.createSimpleEvent(EXAM_NAME, 'exams');
  121 |     expect(examId, 'the exam was submitted but came back without an id.').toBeTruthy();
  122 | 
  123 |     const moved = await moveExamToPast(ownerApi, clubId, examId as string, 1, { startTime: '16:00', endTime: '18:00' });
> 124 |     expect(moved, 'could not move the exam into the past via the API.').toBeTruthy();
      |                                                                         ^ Error: could not move the exam into the past via the API.
  125 | 
  126 |     await gradingPage.open(clubId, examId as string);
  127 |     await gradingPage.markExaminersPresent(examiners);
  128 |     await gradingPage.passCandidate(candidate?.name ?? '');
  129 | 
  130 |     const finalized = await gradingPage.finalize();
  131 |     expect(finalized, 'submitting the finalize confirmation sent no request.').toBeDefined();
  132 |     expect(finalized?.ok, 'POST .../grading/finalize was refused for a sensei.').toBeTruthy();
  133 |     expect(finalized?.passed, 'exactly one candidate should have been marked passed.').toBe(1);
  134 | 
  135 |     const promoted = await ownerApi.get(`${env.apiURL}/clubs/${clubId}/members/${candidateMemberId}`, {
  136 |       headers: { 'x-app-id': env.appId },
  137 |     });
  138 |     const promotedBody = await promoted.json().catch(() => null);
  139 |     const newBeltId = promotedBody?.data?.belt?.id ?? promotedBody?.data?.beltId;
  140 |     expect(
  141 |       newBeltId,
  142 |       "finalizing should have changed the candidate's belt away from what it was before.",
  143 |     ).not.toBe(candidateOriginalBeltId);
  144 |   });
  145 | 
  146 |   /**
  147 |    * `SEN-005` steps mirror `OWP-020` - see that test's comment for why this
  148 |    * is a read/navigation proof, not a second mutation.
  149 |    */
  150 |   test('SEN-005 - the promotions workflow page reflects the exam @SEN-005 @destructive', async ({
  151 |     page,
  152 |     clubId,
  153 |   }, testInfo) => {
  154 |     testInfo.annotations.push({
  155 |       type: 'manual-scenario',
  156 |       description: 'manual-qa/role-sensei.md#SEN-005',
  157 |     });
  158 | 
  159 |     await page.goto(`/dashboard/myClub/${clubId}/promotions`, { waitUntil: 'domcontentloaded' });
  160 |     await expect(
  161 |       page.getByText(/access denied|accès refusé/i),
  162 |       'the promotions page should not deny access to a sensei.',
  163 |     ).toHaveCount(0, { timeout: 15_000 });
  164 | 
  165 |     await expect(
  166 |       page.getByText(EXAM_NAME, { exact: true }).filter({ visible: true }).first(),
  167 |       `"${EXAM_NAME}" should appear in the promotions page's Recent Results ` +
  168 |         'once finalized.',
  169 |     ).toBeVisible({ timeout: 15_000 });
  170 |   });
  171 | 
  172 |   /**
  173 |    * `ownerApi`, NOT the file's own `request` fixture - `PUT
  174 |    * .../members/:memberId/info` is gated to `club_owner_primary`,
  175 |    * `club_owner` and `secretary` only (`clubRoutes.js` line 873); a sensei
  176 |    * cannot call it. Since `test.use(asRole('sensei'))` at the top of this
  177 |    * file makes the bare `request` fixture sensei-authenticated too, using it
  178 |    * here silently 403'd on every attempt - confirmed the restore never
  179 |    * actually happened despite `afterAll` reporting a "CLEANUP FAILED" each
  180 |    * time, rather than the (mistaken) belt-change assumption behind that
  181 |    * message.
  182 |    */
  183 |   // Wrapped in try/finally so the parent-account lock releases even when
  184 |   // there is nothing to restore or the restore itself throws - see the
  185 |   // identical note in owner.exam-grading-promotion.spec.ts.
  186 |   test.afterAll(async ({ ownerApi, clubId }) => {
  187 |     try {
  188 |       if (!candidateMemberId || !candidateOriginalBeltId) return;
  189 | 
  190 |       // See the identical note in owner.exam-grading-promotion.spec.ts -
  191 |       // skips a redundant PUT that the API refuses rather than no-ops.
  192 |       const current = await ownerApi.get(`${env.apiURL}/clubs/${clubId}/members/${candidateMemberId}`, {
  193 |         headers: { 'x-app-id': env.appId },
  194 |       });
  195 |       const currentBody = await current.json().catch(() => null);
  196 |       const currentBeltId = currentBody?.data?.belt?.id ?? currentBody?.data?.beltId;
  197 |       if (currentBeltId === candidateOriginalBeltId) return;
  198 | 
  199 |       const restored = await ownerApi
  200 |         .put(`${env.apiURL}/clubs/${clubId}/members/${candidateMemberId}/info`, {
  201 |           headers: { 'x-app-id': env.appId },
  202 |           data: { beltId: candidateOriginalBeltId },
  203 |         })
  204 |         .then((r) => r.ok())
  205 |         .catch(() => false);
  206 | 
  207 |       if (!restored) {
  208 |         // eslint-disable-next-line no-console
  209 |         console.error(
  210 |           `CLEANUP FAILED: could not restore ${ROLES.parent.email}'s original ` +
  211 |             `belt (${candidateOriginalBeltId}). Fix by hand: PUT ` +
  212 |             `.../clubs/${clubId}/members/${candidateMemberId}/info with ` +
  213 |             `beltId=${candidateOriginalBeltId}.`,
  214 |         );
  215 |       }
  216 |     } finally {
  217 |       releaseParentLock?.();
  218 |     }
  219 |   });
  220 | });
  221 | 
```