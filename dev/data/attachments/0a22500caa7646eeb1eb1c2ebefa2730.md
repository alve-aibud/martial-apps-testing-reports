# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/07-join-requests/staff.join-requests.spec.ts >> Secretary decides a join request >> SEC-002 - a secretary can approve a pending applicant @SEC-002 @destructive
- Location: e2e/specs/smoke-testing/07-join-requests/staff.join-requests.spec.ts:179:7

# Error details

```
TimeoutError: locator.click: Timeout 15000ms exceeded.
Call log:
  - waiting for locator('[data-track="nav:mem_tab"][data-track-target="members"], button[aria-label="Active Members"], button[aria-label="Membres actifs"]').filter({ visible: true }).first()

```

# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - generic [ref=e2]:
    - complementary [ref=e3]:
      - button "Expand sidebar" [ref=e5] [cursor=pointer]
      - navigation [ref=e8]:
        - link "Home" [ref=e9] [cursor=pointer]:
          - /url: /dashboard
        - link "Calendar" [ref=e11] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar
        - link "Message" [ref=e13] [cursor=pointer]:
          - /url: /dashboard/messages
        - link "Members" [ref=e15] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/members
        - link "Progress" [ref=e17] [cursor=pointer]:
          - /url: /progress
        - link "Library" [ref=e19] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/library?standalone=true
        - link "Support" [ref=e21] [cursor=pointer]:
          - /url: /support
        - link "Curriculum" [ref=e23] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/curricula
        - link "My Household" [ref=e25] [cursor=pointer]:
          - /url: "#"
        - link "Payment Dashboard" [ref=e27] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/payments
        - link "My Club" [ref=e29] [cursor=pointer]:
          - /url: /dashboard/myClub
        - button "Share" [ref=e31] [cursor=pointer]
    - banner [ref=e33]:
      - generic [ref=e34]:
        - generic [ref=e35]:
          - link "Martial Apps Logo Martial Apps · Automation Club" [ref=e37] [cursor=pointer]:
            - /url: /dashboard
            - img "Martial Apps Logo" [ref=e38]
            - generic [ref=e39]:
              - generic [ref=e40]: Martial Apps
              - generic [ref=e41]: · Automation Club
          - generic [ref=e42]:
            - generic [ref=e43]:
              - button "Send feedback" [ref=e44] [cursor=pointer]
              - link "Messages" [ref=e54] [cursor=pointer]:
                - /url: /dashboard/messages
              - button "51" [ref=e58] [cursor=pointer]
            - button "A" [ref=e64] [cursor=pointer]
        - generic [ref=e71]:
          - button "Overview" [ref=e72] [cursor=pointer]
          - button "Members" [ref=e73] [cursor=pointer]
          - button "Sites" [ref=e74] [cursor=pointer]
    - main [ref=e75]:
      - generic [ref=e78]:
        - button "Back to roster" [ref=e80] [cursor=pointer]
        - heading "Pending Members(0)" [level=4] [ref=e85]
        - generic [ref=e88]:
          - paragraph [ref=e94]: No pending members
          - paragraph [ref=e95]: No pending requests at this time
  - alert [ref=e96]
```

# Test source

```ts
  22  |  *
  23  |  * ============================================================================
  24  |  * WHY THESE ARE NOT JUST A COPY OF OWP-010/011
  25  |  * ============================================================================
  26  |  *
  27  |  * The backend admits three roles on both routes -
  28  |  * `checkClubAuthorization('club_owner_primary', 'club_owner', 'secretary')`
  29  |  * (`clubRoutes.js` ~line 679-696) - and the frontend gates the Pending tab on
  30  |  * its own `canManageMembers`. Those are two SEPARATE gates that can disagree,
  31  |  * and a role admitted by one but not the other looks exactly like "the
  32  |  * feature is broken" until someone checks which layer said no. That is the
  33  |  * whole content of these two rows: the same flow, proven for the roles the
  34  |  * owner tests never exercise.
  35  |  *
  36  |  * ⚠️ SEC-002 IS AUTOMATED HERE ONLY AS FAR AS STEP 4. Its steps 5-7 (edit a
  37  |  * member's info; confirm no role-change control is offered to a secretary)
  38  |  * are about the member-profile editor, not join requests - a different
  39  |  * surface, in a different smoke group. They are NOT covered by this file and
  40  |  * the row stays partial until they are. Do not mark SEC-002 fully automated
  41  |  * on the strength of this spec.
  42  |  *
  43  |  * Both tests share the one applicant account with the other two files in this
  44  |  * group, so both take `withApplicantAccountLock` - `describe.serial` orders
  45  |  * tests within a file and does nothing across files under `fullyParallel`.
  46  |  */
  47  | 
  48  | const applicant = assertJoinRequestApplicantPresent();
  49  | 
  50  | /**
  51  |  * One round trip: put a request in play, decide it as `page`'s role, and
  52  |  * assert the outcome through the interface.
  53  |  *
  54  |  * Shared by both roles because the flow genuinely IS identical - the variable
  55  |  * under test is the ROLE, so duplicating the steps per role would hide that.
  56  |  * The request itself is created over the API: who can DECIDE it is the
  57  |  * subject here, and the applicant's own journey is CR-002's.
  58  |  */
  59  | async function decideAsCurrentRole(
  60  |   { page, browser, request, ownerApi, clubId }: {
  61  |     page: import('@playwright/test').Page;
  62  |     browser: import('@playwright/test').Browser;
  63  |     request: import('@playwright/test').APIRequestContext;
  64  |     ownerApi: import('@playwright/test').APIRequestContext;
  65  |     clubId: string;
  66  |   },
  67  |   decision: 'approve' | 'reject',
  68  | ): Promise<void> {
  69  |   const { ctx, personId } = await applicantContext(browser, request);
  70  |   const membersPage = new MembersPage(page);
  71  | 
  72  |   try {
  73  |     // Cleanup uses the OWNER's context, not the staff role under test: a
  74  |     // secretary may not remove every member, and a cleanup that quietly 403s
  75  |     // would leave the account stuck for the next run. The role being tested
  76  |     // is the one that DECIDES, which is the part driven through the UI.
  77  |     await resetApplicant(ownerApi, clubId, personId);
  78  | 
  79  |     const beltId = await firstActiveBeltId(ownerApi, clubId);
  80  |     await submitJoinRequest(ctx.request, clubId, beltId);
  81  | 
  82  |     // Steps 1-2: the members page opens for this role, and offers Pending.
  83  |     await membersPage.gotoMembers(clubRoute(clubId, 'members'));
  84  |     await expect(
  85  |       membersLocators.pendingTab(page),
  86  |       'This role reached the members page but is not offered the Pending tab, ' +
  87  |         'so it cannot see join requests at all. That is a frontend gate ' +
  88  |         '(`canManageMembers`), separate from the backend role check.',
  89  |     ).toBeVisible({ timeout: 20_000 });
  90  |     await membersLocators.pendingTab(page).click();
  91  | 
  92  |     const card = membersLocators.pendingCardToggle(page, applicant.email);
  93  |     await expect(
  94  |       card,
  95  |       `<${applicant.email}> is not listed under Pending for this role.`,
  96  |     ).toBeVisible({ timeout: 20_000 });
  97  |     await card.click();
  98  | 
  99  |     if (decision === 'approve') {
  100 |       // Step 3.
  101 |       await membersLocators.approveJoinRequest(page).click();
  102 |       await expect(membersLocators.confirmBeltApproval(page)).toBeVisible({
  103 |         timeout: 15_000,
  104 |       });
  105 | 
  106 |       const approval = page.waitForResponse(
  107 |         (r) =>
  108 |           /\/join-requests\/[^/]+\/approve$/.test(r.url()) &&
  109 |           r.request().method() === 'PUT',
  110 |       );
  111 |       await membersLocators.confirmBeltApproval(page).click();
  112 |       const res = await approval;
  113 | 
  114 |       expect(
  115 |         res.ok(),
  116 |         `This role's approval was refused (${res.status()}). A 403 here means the ` +
  117 |           `frontend offered a control the backend does not allow.\n` +
  118 |           `${await res.text().catch(() => '')}`,
  119 |       ).toBeTruthy();
  120 | 
  121 |       // "...and appears in the Members roster."
> 122 |       await membersLocators.membersTab(page).click();
      |                                              ^ TimeoutError: locator.click: Timeout 15000ms exceeded.
  123 |       await membersLocators.memberSearch(page).fill(applicant.email);
  124 |       await expect(
  125 |         membersLocators.rosterEntryByEmail(page, applicant.email),
  126 |         `<${applicant.email}> was approved but is not on the roster.`,
  127 |       ).toBeVisible({ timeout: 20_000 });
  128 | 
  129 |       expect(
  130 |         await findActiveMember(ownerApi, clubId, personId),
  131 |         'The roster rendered the new member but the approval did not persist.',
  132 |       ).toBeTruthy();
  133 |     } else {
  134 |       // Step 4.
  135 |       const rejection = page.waitForResponse(
  136 |         (r) =>
  137 |           /\/join-requests\/[^/]+\/reject$/.test(r.url()) &&
  138 |           r.request().method() === 'PUT',
  139 |       );
  140 |       await membersLocators.rejectJoinRequest(page).click();
  141 |       const res = await rejection;
  142 | 
  143 |       expect(
  144 |         res.ok(),
  145 |         `This role's rejection was refused (${res.status()}).\n` +
  146 |           `${await res.text().catch(() => '')}`,
  147 |       ).toBeTruthy();
  148 | 
  149 |       await expect(
  150 |         card,
  151 |         `<${applicant.email}> is still under Pending after the rejection.`,
  152 |       ).toBeHidden({ timeout: 20_000 });
  153 | 
  154 |       // POSITIVE CONTROL for this absence check: the approve case above
  155 |       // drives the SAME two locators with the SAME email and requires them to
  156 |       // FIND the row. So an empty result here is "not on the roster", not a
  157 |       // selector that never matched.
  158 |       await membersLocators.membersTab(page).click();
  159 |       await membersLocators.memberSearch(page).fill(applicant.email);
  160 |       await expect(
  161 |         membersLocators.rosterEntryByEmail(page, applicant.email),
  162 |         `<${applicant.email}> was REJECTED but appears on the roster.`,
  163 |       ).toBeHidden({ timeout: 20_000 });
  164 | 
  165 |       expect(
  166 |         await findPendingRequest(ownerApi, clubId, personId),
  167 |         'The request is still pending after a successful rejection.',
  168 |       ).toBeFalsy();
  169 |     }
  170 |   } finally {
  171 |     await resetApplicant(ownerApi, clubId, personId);
  172 |     await ctx.close();
  173 |   }
  174 | }
  175 | 
  176 | test.describe.serial('Secretary decides a join request', () => {
  177 |   test.use(asRole('secretary'));
  178 | 
  179 |   test('SEC-002 - a secretary can approve a pending applicant @SEC-002 @destructive', async ({
  180 |     page,
  181 |     browser,
  182 |     request,
  183 |     ownerApi,
  184 |     clubId,
  185 |   }, testInfo) => {
  186 |     testInfo.annotations.push({
  187 |       type: 'manual-scenario',
  188 |       description: 'manual-qa/role-secretary.md#SEC-002 (steps 1-3; steps 5-7 are the member editor, not covered here)',
  189 |     });
  190 | 
  191 |     // Shares the join-request-applicant lock with 7 other tests across 3
  192 |     // files - see `OWP-010` in owner.join-requests.spec.ts for why 180s.
  193 |     testInfo.setTimeout(180_000);
  194 | 
  195 |     await withApplicantAccountLock(() =>
  196 |       decideAsCurrentRole({ page, browser, request, ownerApi, clubId }, 'approve'),
  197 |     );
  198 |   });
  199 | 
  200 |   test('SEC-002 - a secretary can reject a pending applicant @SEC-002 @destructive', async ({
  201 |     page,
  202 |     browser,
  203 |     request,
  204 |     ownerApi,
  205 |     clubId,
  206 |   }, testInfo) => {
  207 |     testInfo.annotations.push({
  208 |       type: 'manual-scenario',
  209 |       description: 'manual-qa/role-secretary.md#SEC-002 (step 4)',
  210 |     });
  211 | 
  212 |     testInfo.setTimeout(180_000);
  213 | 
  214 |     await withApplicantAccountLock(() =>
  215 |       decideAsCurrentRole({ page, browser, request, ownerApi, clubId }, 'reject'),
  216 |     );
  217 |   });
  218 | });
  219 | 
  220 | test.describe.serial('Co-owner decides a join request', () => {
  221 |   test.use(asRole('coOwner'));
  222 | 
```