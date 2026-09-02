# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/14-child-profile/parent-child.screen-time-extension.spec.ts >> Screen-time extension round trip >> CHD-006 / CR-010 / PAR-007 - child requests extra time, parent approves it @CHD-006 @CR-010 @PAR-007 @destructive
- Location: e2e/specs/smoke-testing/14-child-profile/parent-child.screen-time-extension.spec.ts:126:7

# Error details

```
Error: no pending screen-time request found for "Alnf Child" at 37 minutes.

expect(locator).toBeVisible() failed

Locator: locator('div').filter({ has: getByText('Alnf Child') }).filter({ has: getByText(/\b37\s+minutes\b/) }).first()
Expected: visible
Timeout: 5000ms
Error: element(s) not found

Call log:
  - no pending screen-time request found for "Alnf Child" at 37 minutes. with timeout 5000ms
  - waiting for locator('div').filter({ has: getByText('Alnf Child') }).filter({ has: getByText(/\b37\s+minutes\b/) }).first()


Call Log:
- Timeout 30000ms exceeded while waiting on the predicate

Diagnostic dump at failure time:
  tabs: Pending2 | Approved30 | Denied23
  visible "Requested: N minutes" lines: []
```

# Page snapshot

```yaml
- generic:
  - generic:
    - complementary:
      - generic:
        - button
      - navigation:
        - link:
          - /url: /dashboard
        - link:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar
        - link:
          - /url: /dashboard/messages
        - link:
          - /url: /progress
        - link:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/library?standalone=true
        - link:
          - /url: /support
        - link:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/curricula
        - link:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/user-payments
        - link:
          - /url: /dashboard/myClub
    - banner:
      - generic:
        - generic:
          - generic:
            - link:
              - /url: /dashboard
              - generic:
                - generic: Martial Apps
                - generic: · Automation Club
          - generic:
            - generic:
              - button
              - link:
                - /url: /dashboard/messages
              - generic:
                - button:
                  - generic: 99+
            - generic:
              - button:
                - generic: A
    - main:
      - generic:
        - button
        - generic:
          - button
        - generic:
          - generic:
            - heading [level=2]: Your Household
        - generic:
          - button:
            - generic:
              - paragraph: 2 pending screen time requests
              - paragraph: Tap to review and approve or deny
            - generic: "2"
          - generic:
            - generic:
              - generic:
                - generic:
                  - generic:
                    - heading [level=2]: Alnf Family
                    - paragraph: Household
                - generic:
                  - generic:
                    - paragraph: Total Members
                    - paragraph: "6"
                  - generic:
                    - paragraph: Total Children
                    - paragraph: "5"
              - generic:
                - generic:
                  - generic:
                    - heading [level=2]: Child Profile
                    - paragraph: Create or switch to a child profile
                - button: Switch to a Child Profile
              - generic:
                - generic:
                  - heading [level=3]: Household Settings
                - generic:
                  - button: Edit Household Name
                  - button: Manage Members
                  - button:
                    - generic: Screen Time Requests
                    - generic: "2"
            - generic:
              - generic:
                - generic:
                  - generic:
                    - heading [level=3]: Family Members
                    - paragraph: Adult members & child profiles
                - generic:
                  - paragraph: Alnf's Parents
                  - paragraph: Household Owner
                - generic:
                  - generic:
                    - generic: "["
                    - generic:
                      - generic:
                        - paragraph: "[e2e] PinCheck mtkh32nc"
                        - generic: "5"
                      - generic: No clubs
                    - generic:
                      - button
                      - button
                  - button:
                    - generic: Add and Manage Clubs
                - generic:
                  - generic:
                    - generic: "["
                    - generic:
                      - generic:
                        - paragraph: "[e2e] PinStrength mtk4utmg"
                        - generic: "5"
                      - generic: No clubs
                    - generic:
                      - button
                      - button
                  - button:
                    - generic: Add and Manage Clubs
                - generic:
                  - generic:
                    - generic: "["
                    - generic:
                      - generic:
                        - paragraph: "[e2e] PinStrength mthcmyy6"
                        - generic: "5"
                      - generic: No clubs
                    - generic:
                      - button
                      - button
                  - button:
                    - generic: Add and Manage Clubs
                - generic:
                  - generic:
                    - generic: "["
                    - generic:
                      - generic:
                        - paragraph: "[e2e] PinStrength mtgwibvy"
                        - generic: "5"
                      - generic: No clubs
                    - generic:
                      - button
                      - button
                  - button:
                    - generic: Add and Manage Clubs
                - generic:
                  - generic:
                    - generic: A
                    - generic:
                      - generic:
                        - paragraph: Alnf Child
                        - generic: "0"
                      - generic: 1 Club
                    - generic:
                      - button
                      - button
                  - button:
                    - generic: Add and Manage Clubs
                - generic:
                  - button: Add Child Profile
                  - button: Invite Partner
  - alert
  - dialog [ref=f4e2]:
    - heading "Dialog" [level=2] [ref=f4e3]
    - button [ref=f4e4] [cursor=pointer]
    - generic [ref=f4e8]:
      - heading "Screen Time Requests" [level=2] [ref=f4e9]
      - paragraph [ref=f4e10]: Manage your children's screen time extension requests
    - generic [ref=f4e12]:
      - generic [ref=f4e13]:
        - button "Pending 2" [active] [ref=f4e14] [cursor=pointer]:
          - text: Pending
          - generic [ref=f4e18]: "2"
        - button "Approved 30" [ref=f4e19] [cursor=pointer]:
          - text: Approved
          - generic [ref=f4e23]: "30"
        - button "Denied 23" [ref=f4e24] [cursor=pointer]:
          - text: Denied
          - generic [ref=f4e29]: "23"
      - generic [ref=f4e30]:
        - generic [ref=f4e31]:
          - generic [ref=f4e32]: Start Date
          - textbox [ref=f4e33]: 2026-08-03
        - generic [ref=f4e34]:
          - generic [ref=f4e35]: End Date
          - textbox [ref=f4e36]: 2026-09-02
      - generic [ref=f4e37]:
        - paragraph [ref=f4e40]: No requests found
        - paragraph [ref=f4e41]: There are no pending screen time requests
```

# Test source

```ts
  167 |    * see `editChildTrigger`'s comment for why.
  168 |    */
  169 |   async openEditChildForm(childName: string): Promise<void> {
  170 |     await this.gotoAndAwaitClubRole('/dashboard/household');
  171 | 
  172 |     const trigger = householdLocators.editChildTrigger(this.page, childName);
  173 |     await expect(
  174 |       trigger,
  175 |       `no edit control found for "${childName}"'s row on /dashboard/household.`,
  176 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  177 |     await trigger.click();
  178 | 
  179 |     await expect(
  180 |       householdLocators.editChildScreenTime(this.page),
  181 |       'the edit-child dialog did not open with its screen-time field.',
  182 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  183 |   }
  184 | 
  185 |   /**
  186 |    * Overwrites the screen-time field and submits, returning whether the
  187 |    * server accepted it. Assumes `openEditChildForm` already left the dialog
  188 |    * open - every other field arrives prefilled by the component, so nothing
  189 |    * else needs touching for `PAR-003`.
  190 |    */
  191 |   async setScreenTimeLimit(minutes: string): Promise<{ ok: boolean } | undefined> {
  192 |     await householdLocators.editChildScreenTime(this.page).fill(minutes);
  193 | 
  194 |     const pending = this.page
  195 |       .waitForResponse(
  196 |         (r) =>
  197 |           /\/child-profiles\/[^/]+$/.test(new URL(r.url()).pathname) &&
  198 |           r.request().method() === 'PUT',
  199 |         { timeout: 10_000 },
  200 |       )
  201 |       .catch(() => undefined);
  202 | 
  203 |     await householdLocators.editChildSubmit(this.page).click();
  204 |     const res = await pending;
  205 |     return res ? { ok: res.ok() } : undefined;
  206 |   }
  207 | 
  208 |   /**
  209 |    * `PAR-007` / `PAR-008`. Opens the Screen Time Requests modal and its
  210 |    * Pending tab (the modal opens on Pending by default - clicked anyway, so
  211 |    * a rerun that leaves the tab elsewhere still lands where these need it).
  212 |    */
  213 |   async openScreenTimeRequests(): Promise<void> {
  214 |     await this.gotoAndAwaitClubRole('/dashboard/household');
  215 | 
  216 |     await screenTimeLocators.requestsModalTrigger(this.page).click();
  217 |     await expect(
  218 |       screenTimeLocators.requestsModalHeading(this.page),
  219 |       'the Screen Time Requests modal did not open.',
  220 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  221 | 
  222 |     await screenTimeLocators.pendingTab(this.page).click();
  223 |   }
  224 | 
  225 |   /**
  226 |    * Approves the pending request for `childName` at `requestedMinutes`,
  227 |    * granting `approvedMinutes`. Card is anchored on BOTH the child's name and
  228 |    * the requested minutes - see `screenTimeLocators.requestCard`.
  229 |    *
  230 |    * `rejectedAttempts` (optional, `PAR-007` steps 3-4): values tried FIRST,
  231 |    * in order, against the SAME open panel, each expected to be refused with
  232 |    * an inline error and to leave the request pending - a too-high value hits
  233 |    * the server (`ValidationError('Approved minutes cannot exceed requested
  234 |    * minutes')`), while `<= 0` or `> 1440` is refused client-side
  235 |    * (`validateApprovedMinutes`, `ScreenTimeRequestCard.tsx` ~line 43-47) with
  236 |    * no network round trip either way. `expect(...).toBeVisible()` waiting on
  237 |    * the inline error covers both cases without needing to know which is
  238 |    * which. Only ONE "Approve" click ever happens - clicking it again once the
  239 |    * panel is open would find nothing, since the button is replaced by the
  240 |    * input the moment it opens.
  241 |    */
  242 |   async approveScreenTimeRequest(
  243 |     childName: string,
  244 |     requestedMinutes: number,
  245 |     approvedMinutes: number,
  246 |     opts: { rejectedAttempts?: number[] } = {},
  247 |   ): Promise<{ ok: boolean } | undefined> {
  248 |     const card = screenTimeLocators.requestCard(this.page, childName, requestedMinutes);
  249 | 
  250 |     // Same `toPass` retry as `expectRequestApproved`/`expectRequestDenied`
  251 |     // below, and for the same reason (see that pair's header) - this is the
  252 |     // ONE lookup in this file that was still a single-shot `toBeVisible`,
  253 |     // and it raced the exact same modal refetch live on GitHub Actions,
  254 |     // 2026-09-03: "no pending screen-time request found ... at 37 minutes."
  255 |     // Re-clicking Pending each pass forces a fresh refetch, same as
  256 |     // re-clicking Approved/Denied does for those two methods.
  257 |     try {
  258 |       await expect(async () => {
  259 |         await screenTimeLocators.pendingTab(this.page).click();
  260 |         await expect(
  261 |           card,
  262 |           `no pending screen-time request found for "${childName}" at ` +
  263 |             `${requestedMinutes} minutes.`,
  264 |         ).toBeVisible({ timeout: 5_000 });
  265 |       }).toPass({ timeout: RENDER_TIMEOUT });
  266 |     } catch (err) {
> 267 |       throw new Error(`${(err as Error).message}\n\n${await this.dumpRequestsModalState()}`);
      |             ^ Error: no pending screen-time request found for "Alnf Child" at 37 minutes.
  268 |     }
  269 | 
  270 |     await screenTimeLocators.approveButton(card).click();
  271 | 
  272 |     for (const bad of opts.rejectedAttempts ?? []) {
  273 |       await screenTimeLocators.approvedMinutesField(card).fill(String(bad));
  274 |       await screenTimeLocators.confirmApproveButton(card).click();
  275 |       await expect(
  276 |         screenTimeLocators.approveErrorMessage(card),
  277 |         `approving ${bad} minutes against a ${requestedMinutes}-minute request ` +
  278 |           'was not refused - expected an inline validation error.',
  279 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  280 |     }
  281 | 
  282 |     await screenTimeLocators.approvedMinutesField(card).fill(String(approvedMinutes));
  283 | 
  284 |     const pending = this.page
  285 |       .waitForResponse(
  286 |         (r) => /\/screen-time-requests\/[^/]+\/approve\b/.test(r.url()) && r.request().method() === 'PUT',
  287 |         { timeout: 10_000 },
  288 |       )
  289 |       .catch(() => undefined);
  290 | 
  291 |     await screenTimeLocators.confirmApproveButton(card).click();
  292 |     const res = await pending;
  293 |     return res ? { ok: res.ok() } : undefined;
  294 |   }
  295 | 
  296 |   /**
  297 |    * Denies the pending request for `childName` at `requestedMinutes`, with
  298 |    * `reason`.
  299 |    *
  300 |    * `rejectedReasons` (optional, `PAR-008` steps 2-3): reasons tried FIRST
  301 |    * against the same open panel - a blank reason ("required") and a reason
  302 |    * under 10 characters ("minimum length"), both refused CLIENT-side
  303 |    * (`validateDenialReason`, `ScreenTimeRequestCard.tsx` ~line 77-82; the
  304 |    * backend's own `denialReason` is merely `.optional()`, no length rule -
  305 |    * this app enforces the requirement only in the form). Same one-Deny-click
  306 |    * shape as the approve method above, for the same reason.
  307 |    */
  308 |   async denyScreenTimeRequest(
  309 |     childName: string,
  310 |     requestedMinutes: number,
  311 |     reason: string,
  312 |     opts: { rejectedReasons?: string[] } = {},
  313 |   ): Promise<{ ok: boolean } | undefined> {
  314 |     const card = screenTimeLocators.requestCard(this.page, childName, requestedMinutes);
  315 | 
  316 |     // Same `toPass` retry as `approveScreenTimeRequest` above - the same
  317 |     // modal-refetch race applies to this lookup too. See that method's
  318 |     // comment.
  319 |     try {
  320 |       await expect(async () => {
  321 |         await screenTimeLocators.pendingTab(this.page).click();
  322 |         await expect(
  323 |           card,
  324 |           `no pending screen-time request found for "${childName}" at ` +
  325 |             `${requestedMinutes} minutes.`,
  326 |         ).toBeVisible({ timeout: 5_000 });
  327 |       }).toPass({ timeout: RENDER_TIMEOUT });
  328 |     } catch (err) {
  329 |       throw new Error(`${(err as Error).message}\n\n${await this.dumpRequestsModalState()}`);
  330 |     }
  331 | 
  332 |     await screenTimeLocators.denyButton(card).click();
  333 | 
  334 |     for (const bad of opts.rejectedReasons ?? []) {
  335 |       await screenTimeLocators.denialReasonField(card).fill(bad);
  336 |       await screenTimeLocators.confirmDenyButton(card).click();
  337 |       await expect(
  338 |         screenTimeLocators.denyErrorMessage(card),
  339 |         `denying with reason ${JSON.stringify(bad)} was not refused - expected ` +
  340 |           'an inline validation error.',
  341 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  342 |     }
  343 | 
  344 |     await screenTimeLocators.denialReasonField(card).fill(reason);
  345 | 
  346 |     const pending = this.page
  347 |       .waitForResponse(
  348 |         (r) => /\/screen-time-requests\/[^/]+\/deny\b/.test(r.url()) && r.request().method() === 'PUT',
  349 |         { timeout: 10_000 },
  350 |       )
  351 |       .catch(() => undefined);
  352 | 
  353 |     await screenTimeLocators.confirmDenyButton(card).click();
  354 |     const res = await pending;
  355 |     return res ? { ok: res.ok() } : undefined;
  356 |   }
  357 | 
  358 |   /**
  359 |    * Pushes the modal's End Date filter 2 days past the machine's own clock -
  360 |    * see `screenTimeLocators.endDateFilter`'s header for the exact boundary
  361 |    * bug this dodges. Called before searching the Approved/Denied tabs for a
  362 |    * request that was JUST resolved, never before - widening it earlier would
  363 |    * do nothing useful and only adds an extra fill.
  364 |    */
  365 |   private async widenEndDateFilter(): Promise<void> {
  366 |     const safeEnd = new Date(Date.now() + 2 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
  367 |     await screenTimeLocators.endDateFilter(this.page).fill(safeEnd as string);
```