# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/07-join-requests/cross-role.join-request.spec.ts >> Join request round trip >> CR-002 - applicant asks to join, owner approves, applicant lands on the roster @CR-002 @destructive
- Location: e2e/specs/smoke-testing/07-join-requests/cross-role.join-request.spec.ts:61:7

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
          - button "Settings" [ref=e75] [cursor=pointer]
    - main [ref=e76]:
      - generic [ref=e79]:
        - button "Back to roster" [ref=e81] [cursor=pointer]
        - heading "Pending Members(0)" [level=4] [ref=e86]
        - generic [ref=e89]:
          - paragraph [ref=e95]: No pending members
          - paragraph [ref=e96]: No pending requests at this time
  - alert [ref=e97]
```

# Test source

```ts
  88  |         const clubName = await clubDisplayName(ownerApi, clubId);
  89  | 
  90  |         // ---- APPLICANT SIDE -------------------------------------------------
  91  |         // Find the club by hand and ask to join.
  92  |         await applicantPage.goto(`${test.info().project.use.baseURL ?? ''}${routes.clubsDiscover}`);
  93  |         await clubsDiscoverLocators.search(applicantPage).fill(clubName);
  94  |         await clubsDiscoverLocators.searchSubmit(applicantPage).click();
  95  | 
  96  |         /**
  97  |          * FIND-AND-CLICK IN ONE `toPass`, not a visible-then-click pair.
  98  |          *
  99  |          * Found live on GitHub Actions, 2026-09-06:
  100 |          *
  101 |          *   locator resolved to <button ...>Request to Join</button>
  102 |          *   attempting click action
  103 |          *   element was detached from the DOM, retrying
  104 |          *
  105 |          * The button is there and the locator is right - the CARD LIST is
  106 |          * re-rendering underneath it. The search results arrive after the
  107 |          * submit click, and the discover grid replaces its nodes when they
  108 |          * land, so a button resolved from the pre-response render is detached
  109 |          * before the click completes. Playwright retries a detached click on
  110 |          * its own, but the locator ends in `.last()` over cards filtered by
  111 |          * name, so each retry can resolve to a DIFFERENT node and the 15s
  112 |          * actionability budget runs out.
  113 |          *
  114 |          * Re-resolving the locator inside the retry is what fixes it: every
  115 |          * pass looks the list up again, so once the grid settles the click
  116 |          * lands on the settled node. Same reasoning as
  117 |          * `household.page.ts`'s screen-time lookups - retry the whole
  118 |          * find-then-act sequence, not just the final assertion.
  119 |          */
  120 |         await expect(async () => {
  121 |           const joinButton = clubsDiscoverLocators.requestToJoin(applicantPage, clubName);
  122 |           await expect(
  123 |             joinButton,
  124 |             `No enabled "Request to Join" on the card for "${clubName}". If the ` +
  125 |               `card shows "Request Sent" or "View Club" instead, the applicant was ` +
  126 |               `left in the club by an earlier run.`,
  127 |           ).toBeVisible({ timeout: 5_000 });
  128 |           await joinButton.click({ timeout: 5_000 });
  129 |         }).toPass({ timeout: 30_000 });
  130 | 
  131 |         // Pick a belt - the dialog will not send without one.
  132 |         await clubsDiscoverLocators.beltPicker(applicantPage).click();
  133 |         await clubsDiscoverLocators.firstBeltOption(applicantPage).click();
  134 |         await clubsDiscoverLocators
  135 |           .joinMessage(applicantPage)
  136 |           .fill('[e2e] CR-002 automated round trip');
  137 | 
  138 |         const submitted = applicantPage.waitForResponse(
  139 |           (r) => /\/join-requests$/.test(r.url()) && r.request().method() === 'POST',
  140 |         );
  141 |         await clubsDiscoverLocators.sendRequest(applicantPage).click();
  142 |         const submitRes = await submitted;
  143 | 
  144 |         expect(
  145 |           submitRes.ok(),
  146 |           `The join request the applicant sent through the UI failed ` +
  147 |             `(${submitRes.status()}).\n${await submitRes.text().catch(() => '')}`,
  148 |         ).toBeTruthy();
  149 | 
  150 |         // The applicant's own view now says the request is out.
  151 |         await expect(
  152 |           clubsDiscoverLocators.requestSent(applicantPage, clubName),
  153 |           'The club card did not flip to "Request Sent" after a successful submit.',
  154 |         ).toBeVisible({ timeout: 20_000 });
  155 | 
  156 |         // ---- OWNER SIDE -----------------------------------------------------
  157 |         await membersPage.gotoMembers(clubRoute(clubId, 'members'));
  158 |         await membersLocators.pendingTab(page).click();
  159 | 
  160 |         const card = membersLocators.pendingCardToggle(page, applicant.email);
  161 |         await expect(
  162 |           card,
  163 |           `The owner does not see <${applicant.email}> under Pending, although the ` +
  164 |             `applicant's request was accepted. This is the hand-off the scenario exists ` +
  165 |             `to prove.`,
  166 |         ).toBeVisible({ timeout: 20_000 });
  167 |         await card.click();
  168 | 
  169 |         await membersLocators.approveJoinRequest(page).click();
  170 |         await expect(membersLocators.confirmBeltApproval(page)).toBeVisible({
  171 |           timeout: 15_000,
  172 |         });
  173 | 
  174 |         const approval = page.waitForResponse(
  175 |           (r) =>
  176 |             /\/join-requests\/[^/]+\/approve$/.test(r.url()) && r.request().method() === 'PUT',
  177 |         );
  178 |         await membersLocators.confirmBeltApproval(page).click();
  179 |         const approvalRes = await approval;
  180 | 
  181 |         expect(
  182 |           approvalRes.ok(),
  183 |           `The owner's approval failed (${approvalRes.status()}).\n` +
  184 |             `${await approvalRes.text().catch(() => '')}`,
  185 |         ).toBeTruthy();
  186 | 
  187 |         // The member appears on the roster, in the owner's UI.
> 188 |         await membersLocators.membersTab(page).click();
      |                                                ^ TimeoutError: locator.click: Timeout 15000ms exceeded.
  189 |         await membersLocators.memberSearch(page).fill(applicant.email);
  190 |         await expect(
  191 |           membersLocators.rosterEntryByEmail(page, applicant.email),
  192 |           `<${applicant.email}> was approved but is not on the Members roster.`,
  193 |         ).toBeVisible({ timeout: 20_000 });
  194 | 
  195 |         // ---- APPLICANT SIDE AGAIN -------------------------------------------
  196 |         // The round trip is only closed when the APPLICANT can see it. Their
  197 |         // card flips from the disabled "Request Sent" to "View Club".
  198 |         await applicantPage.reload();
  199 |         await clubsDiscoverLocators.search(applicantPage).fill(clubName);
  200 |         await clubsDiscoverLocators.searchSubmit(applicantPage).click();
  201 | 
  202 |         await expect(
  203 |           clubsDiscoverLocators.requestSent(applicantPage, clubName),
  204 |           `The applicant's card still reads "Request Sent" after the owner approved ` +
  205 |             `them - the approval never reached the applicant's view.`,
  206 |         ).toBeHidden({ timeout: 20_000 });
  207 | 
  208 |         expect(
  209 |           await findActiveMember(ownerApi, clubId, personId),
  210 |           `The UI shows the approval but <${applicant.email}> is not an active member.`,
  211 |         ).toBeTruthy();
  212 |       } finally {
  213 |         await resetApplicant(ownerApi, clubId, personId);
  214 |         await ctx.close();
  215 |       }
  216 |     });
  217 |   });
  218 | 
  219 |   test('CR-003 - applicant asks to join, owner rejects, applicant is not a member @CR-003 @destructive', async ({
  220 |     page,
  221 |     browser,
  222 |     request,
  223 |     ownerApi,
  224 |     clubId,
  225 |   }, testInfo) => {
  226 |     testInfo.annotations.push({
  227 |       type: 'manual-scenario',
  228 |       description: 'manual-qa/role-cross-role.md#CR-003 (both sides, all UI)',
  229 |     });
  230 | 
  231 |     // Same reason as `CR-002` above - see `OWP-010` in owner.join-requests.spec.ts.
  232 |     testInfo.setTimeout(180_000);
  233 | 
  234 |     await withApplicantAccountLock(async () => {
  235 |       const { ctx, personId } = await applicantContext(browser, request);
  236 |       const applicantPage = await ctx.newPage();
  237 |       const membersPage = new MembersPage(page);
  238 | 
  239 |       try {
  240 |         await resetApplicant(ownerApi, clubId, personId);
  241 |         const clubName = await clubDisplayName(ownerApi, clubId);
  242 | 
  243 |         // ---- APPLICANT SIDE -------------------------------------------------
  244 |         await applicantPage.goto(`${test.info().project.use.baseURL ?? ''}${routes.clubsDiscover}`);
  245 |         await clubsDiscoverLocators.search(applicantPage).fill(clubName);
  246 |         await clubsDiscoverLocators.searchSubmit(applicantPage).click();
  247 | 
  248 |         const joinButton = clubsDiscoverLocators.requestToJoin(applicantPage, clubName);
  249 |         await expect(joinButton).toBeVisible({ timeout: 20_000 });
  250 |         await joinButton.click();
  251 | 
  252 |         await clubsDiscoverLocators.beltPicker(applicantPage).click();
  253 |         await clubsDiscoverLocators.firstBeltOption(applicantPage).click();
  254 |         await clubsDiscoverLocators
  255 |           .joinMessage(applicantPage)
  256 |           .fill('[e2e] CR-003 automated round trip');
  257 | 
  258 |         const submitted = applicantPage.waitForResponse(
  259 |           (r) => /\/join-requests$/.test(r.url()) && r.request().method() === 'POST',
  260 |         );
  261 |         await clubsDiscoverLocators.sendRequest(applicantPage).click();
  262 |         const submitRes = await submitted;
  263 |         expect(
  264 |           submitRes.ok(),
  265 |           `The join request failed (${submitRes.status()}).\n` +
  266 |             `${await submitRes.text().catch(() => '')}`,
  267 |         ).toBeTruthy();
  268 | 
  269 |         await expect(
  270 |           clubsDiscoverLocators.requestSent(applicantPage, clubName),
  271 |         ).toBeVisible({ timeout: 20_000 });
  272 | 
  273 |         // ---- OWNER SIDE -----------------------------------------------------
  274 |         await membersPage.gotoMembers(clubRoute(clubId, 'members'));
  275 |         await membersLocators.pendingTab(page).click();
  276 | 
  277 |         const card = membersLocators.pendingCardToggle(page, applicant.email);
  278 |         await expect(card).toBeVisible({ timeout: 20_000 });
  279 |         await card.click();
  280 | 
  281 |         const rejection = page.waitForResponse(
  282 |           (r) =>
  283 |             /\/join-requests\/[^/]+\/reject$/.test(r.url()) && r.request().method() === 'PUT',
  284 |         );
  285 |         await membersLocators.rejectJoinRequest(page).click();
  286 |         const rejectionRes = await rejection;
  287 | 
  288 |         expect(
```