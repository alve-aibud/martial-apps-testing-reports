# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/09-calendar-scheduling/owner.calendar-controls.spec.ts >> Owner edits an event on the calendar >> OWP-009 - a recurring occurrence offers a scope picker that applies @OWP-009 @destructive
- Location: e2e/specs/smoke-testing/09-calendar-scheduling/owner.calendar-controls.spec.ts:490:7

# Error details

```
Error: the recurring scope picker should offer 2 choices. If this count has changed, the positional indexing in this page object is now pointing at the wrong buttons - fix it before trusting a result.

expect(locator).toHaveCount(expected) failed

Locator:  getByRole('button').filter({ hasText: /classCancellation\.scope(This|Future|All)|^(This class|This and future|All classes)/i }).filter({ visible: true })
Expected: 2
Received: 1
Timeout:  30000ms

Call log:
  - the recurring scope picker should offer 2 choices. If this count has changed, the positional indexing in this page object is now pointing at the wrong buttons - fix it before trusting a result. with timeout 30000ms
  - waiting for getByRole('button').filter({ hasText: /classCancellation\.scope(This|Future|All)|^(This class|This and future|All classes)/i }).filter({ visible: true })
    63 × locator resolved to 1 element
       - unexpected value "1"

```

# Page snapshot

```yaml
- generic [active] [ref=f1e1]:
  - generic [ref=f1e2]:
    - complementary [ref=f1e3]:
      - button "Expand sidebar" [ref=f1e5] [cursor=pointer]
      - navigation [ref=f1e8]:
        - link "Home" [ref=f1e9] [cursor=pointer]:
          - /url: /dashboard
        - link "Calendar" [ref=f1e11] [cursor=pointer]:
          - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/calendar
        - link "Message" [ref=f1e13] [cursor=pointer]:
          - /url: /dashboard/messages
        - link "Members" [ref=f1e15] [cursor=pointer]:
          - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/members
        - link "Progress" [ref=f1e17] [cursor=pointer]:
          - /url: /progress
        - link "Library" [ref=f1e19] [cursor=pointer]:
          - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/library?standalone=true
        - link "Support" [ref=f1e21] [cursor=pointer]:
          - /url: /support
        - link "Payment" [ref=f1e23] [cursor=pointer]:
          - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/payments
        - link "My Club" [ref=f1e25] [cursor=pointer]:
          - /url: /dashboard/myClub
        - button "Share" [ref=f1e27] [cursor=pointer]
    - banner [ref=f1e29]:
      - generic [ref=f1e30]:
        - generic [ref=f1e31]:
          - link "Martial Apps Logo Martial Apps · Automation Club" [ref=f1e33] [cursor=pointer]:
            - /url: /dashboard
            - img "Martial Apps Logo" [ref=f1e34]
            - generic [ref=f1e35]:
              - generic [ref=f1e36]: Martial Apps
              - generic [ref=f1e37]: · Automation Club
          - generic [ref=f1e38]:
            - generic [ref=f1e39]:
              - button "Send feedback" [ref=f1e40] [cursor=pointer]
              - link "Messages" [ref=f1e50] [cursor=pointer]:
                - /url: /dashboard/messages
              - button "21" [ref=f1e54] [cursor=pointer]
            - button "A" [ref=f1e60] [cursor=pointer]
        - generic [ref=f1e67]:
          - button "Overview" [ref=f1e68] [cursor=pointer]
          - button "Members" [ref=f1e69] [cursor=pointer]
          - button "Sites" [ref=f1e70] [cursor=pointer]
          - button "Settings" [ref=f1e71] [cursor=pointer]
    - main [ref=f1e72]:
      - generic [ref=f1e73]:
        - generic [ref=f1e76]:
          - generic [ref=f1e77]:
            - button "Collapse sidebar" [ref=f1e79] [cursor=pointer]
            - button "Create" [ref=f1e83] [cursor=pointer]
            - generic:
              - generic:
                - button "September 2026" [ref=f1e87] [cursor=pointer]
                - generic [ref=f1e91]:
                  - button [ref=f1e92] [cursor=pointer]
                  - button [ref=f1e95] [cursor=pointer]
              - generic:
                - generic [ref=f1e98]: S
                - generic [ref=f1e99]: M
                - generic [ref=f1e100]: T
                - generic [ref=f1e101]: W
                - generic [ref=f1e102]: T
                - generic [ref=f1e103]: F
                - generic [ref=f1e104]: S
              - generic:
                - generic [ref=f1e105]: "30"
                - generic [ref=f1e106]: "31"
                - button "1" [ref=f1e107] [cursor=pointer]
                - button "2" [ref=f1e108] [cursor=pointer]
                - button "3" [ref=f1e109] [cursor=pointer]
                - button "4" [ref=f1e110] [cursor=pointer]
                - button "5" [ref=f1e111] [cursor=pointer]
                - button "6" [ref=f1e112] [cursor=pointer]
                - button "7" [ref=f1e113] [cursor=pointer]
                - button "8" [ref=f1e114] [cursor=pointer]
                - button "9" [ref=f1e115] [cursor=pointer]
                - button "10" [ref=f1e116] [cursor=pointer]
                - button "11" [ref=f1e117] [cursor=pointer]
                - button "12" [ref=f1e118] [cursor=pointer]
                - button "13" [ref=f1e119] [cursor=pointer]
                - button "14" [ref=f1e120] [cursor=pointer]
                - button "15" [ref=f1e121] [cursor=pointer]
                - button "16" [ref=f1e122] [cursor=pointer]
                - button "17" [ref=f1e123] [cursor=pointer]
                - button "18" [ref=f1e124] [cursor=pointer]
                - button "19" [ref=f1e125] [cursor=pointer]
                - button "20" [ref=f1e126] [cursor=pointer]
                - button "21" [ref=f1e127] [cursor=pointer]
                - button "22" [ref=f1e128] [cursor=pointer]
                - button "23" [ref=f1e129] [cursor=pointer]
                - button "24" [ref=f1e130] [cursor=pointer]
                - button "25" [ref=f1e131] [cursor=pointer]
                - button "26" [ref=f1e132] [cursor=pointer]
                - button "27" [ref=f1e133] [cursor=pointer]
                - button "28" [ref=f1e134] [cursor=pointer]
                - button "29" [ref=f1e135] [cursor=pointer]
                - button "30" [ref=f1e136] [cursor=pointer]
                - generic [ref=f1e137]: "1"
                - generic [ref=f1e138]: "2"
                - generic [ref=f1e139]: "3"
            - generic:
              - generic:
                - button [ref=f1e140] [cursor=pointer]:
                  - heading "Curricula" [level=3] [ref=f1e141]
                - button "View all →" [ref=f1e144] [cursor=pointer]
              - generic:
                - generic: "[e2e] Automation"
                - generic: "[e2e] OWP-017 Curriculum"
                - generic: "[e2e] owp032-empty-curriculum mtr1pab8"
                - generic: "[e2e] owp032-empty-curriculum mtr1pqjl"
                - generic: "[e2e] owp032-empty-curriculum mtrck649"
                - generic: "[e2e] owp032-empty-curriculum mtrckhqy"
                - generic: "[e2e] owp032-empty-curriculum mtrcktik"
                - generic: "[e2e] owp032-empty-curriculum mtrhquw9"
                - generic: "[e2e] owp032-empty-curriculum mtrhr7pi"
                - generic: "[e2e] owp032-empty-curriculum mtrhrk5f"
                - generic: "[e2e] SEN-006 Curriculum"
                - generic: "[e2e] SEN-011 Target Curriculum"
            - generic:
              - generic:
                - button [ref=f1e157] [cursor=pointer]:
                  - heading "Sites" [level=3] [ref=f1e158]
                - button "View all →" [ref=f1e161] [cursor=pointer]
              - generic:
                - button "All Sites" [ref=f1e162] [cursor=pointer]
                - button "Automation Club - Main Site" [ref=f1e167] [cursor=pointer]
                - button "[e2e] Canada TZ - AB (Mountain)" [ref=f1e172] [cursor=pointer]
                - button "[e2e] Canada TZ - BC (Pacific)" [ref=f1e177] [cursor=pointer]
                - button "[e2e] Canada TZ - MB (Central)" [ref=f1e182] [cursor=pointer]
                - button "[e2e] Canada TZ - NL (Newfoundland)" [ref=f1e187] [cursor=pointer]
                - button "[e2e] Canada TZ - NS (Atlantic)" [ref=f1e192] [cursor=pointer]
                - button "[e2e] Canada TZ - ON (Eastern)" [ref=f1e197] [cursor=pointer]
                - button "[e2e] Exam Fixtures Site" [ref=f1e202] [cursor=pointer]
                - button "[e2e] site-owp009 mtv7sf4h" [ref=f1e207] [cursor=pointer]
            - generic:
              - generic:
                - button [ref=f1e212] [cursor=pointer]:
                  - heading "Event Types" [level=3] [ref=f1e213]
                - button "Create" [ref=f1e216] [cursor=pointer]
              - generic:
                - generic: Classes
                - generic: Exams
                - generic: Workshops
                - generic: Socials
                - generic: Tournaments
          - button "Expand sidebar" [ref=f1e234] [cursor=pointer]
          - generic [ref=f1e238]:
            - generic [ref=f1e239]:
              - generic [ref=f1e240]:
                - button "Today" [ref=f1e241] [cursor=pointer]
                - generic [ref=f1e242]:
                  - button [ref=f1e243] [cursor=pointer]
                  - button [ref=f1e246] [cursor=pointer]
                - generic [ref=f1e249]:
                  - heading "Thursday, September 10, 2026" [level=2] [ref=f1e250]
                  - generic [ref=f1e251]: Attending · 1 event
              - generic [ref=f1e252]:
                - generic [ref=f1e253]:
                  - button "All teachers" [ref=f1e254] [cursor=pointer]
                  - generic [ref=f1e257]:
                    - button "Teaching" [ref=f1e258] [cursor=pointer]
                    - button "Attending" [ref=f1e259] [cursor=pointer]
                - generic [ref=f1e260]:
                  - button "Day" [ref=f1e261] [cursor=pointer]
                  - button "Week" [ref=f1e262] [cursor=pointer]
                  - button "Month" [ref=f1e263] [cursor=pointer]
            - generic [ref=f1e266]:
              - generic [ref=f1e267]:
                - generic [ref=f1e268]: 6 AM
                - generic [ref=f1e270]: 7 AM
                - generic [ref=f1e272]: 8 AM
                - generic [ref=f1e274]: 9 AM
                - generic [ref=f1e276]: 10 AM
                - generic [ref=f1e278]: 11 AM
                - generic [ref=f1e280]: 12 PM
                - generic [ref=f1e282]: 1 PM
                - generic [ref=f1e284]: 2 PM
                - generic [ref=f1e286]: 3 PM
                - generic [ref=f1e288]: 4 PM
                - generic [ref=f1e290]: 5 PM
                - generic [ref=f1e292]: 6 PM
                - generic [ref=f1e294]: 7 PM
                - generic [ref=f1e296]: 8 PM
                - generic [ref=f1e298]: 9 PM
                - generic [ref=f1e300]: 10 PM
              - generic [ref=f1e320] [cursor=pointer]:
                - generic [ref=f1e321]:
                  - generic [ref=f1e322]: 10:37
                  - generic [ref=f1e323]: + Main sensei
                  - generic [ref=f1e324]: + 2nd
                - generic [ref=f1e325]: "[e2e] owp009-series mtv7sf4h"
                - generic [ref=f1e326]: 10:37 – 11:37 am
                - generic [ref=f1e327]: "[e2e] site-owp009 mtv7sf4h · [e2e] room mtv7sf8f"
        - button [ref=f1e328] [cursor=pointer]
        - generic [ref=f1e331]:
          - generic [ref=f1e333]:
            - generic [ref=f1e339]:
              - heading "Change Class" [level=1] [ref=f1e340]
              - paragraph [ref=f1e341]: "[e2e] owp009-series mtv7sf4h"
            - button [ref=f1e342] [cursor=pointer]
          - generic [ref=f1e346]:
            - generic [ref=f1e347]:
              - generic [ref=f1e351]:
                - paragraph [ref=f1e352]: Thu, Sep 10 · 10:37 AM–11:37 AM
                - paragraph [ref=f1e353]: "[e2e] room mtv7sf8f"
              - generic [ref=f1e354]: Starts in 2h 59m
            - generic [ref=f1e358]:
              - generic [ref=f1e359]: Recurring class — apply to
              - generic [ref=f1e365]:
                - button "This class" [ref=f1e366] [cursor=pointer]
                - button "This & future" [ref=f1e367] [cursor=pointer]
              - paragraph [ref=f1e368]: Only this class moves to the new time.
            - generic [ref=f1e370]:
              - generic [ref=f1e371]: Class Name *
              - textbox [ref=f1e372]: "[e2e] owp009-series mtv7sf4h"
            - generic [ref=f1e373]:
              - generic [ref=f1e374]: Description
              - textbox [ref=f1e375]
            - generic [ref=f1e376]:
              - generic [ref=f1e377]: New date & time
              - generic [ref=f1e378]:
                - generic [ref=f1e379]:
                  - generic [ref=f1e380]: Date
                  - button "10 Sep 2026" [ref=f1e381] [cursor=pointer]
                - generic [ref=f1e385]:
                  - generic [ref=f1e386]: Start time
                  - button "10:37 AM" [ref=f1e387] [cursor=pointer]
                - generic [ref=f1e392]:
                  - generic [ref=f1e393]: Duration (mins)
                  - generic [ref=f1e394]:
                    - spinbutton [ref=f1e395]: "60"
                    - generic [ref=f1e396]: min
              - paragraph [ref=f1e397]:
                - text: 10:37 AM – 11:37 AM
                - generic [ref=f1e401]: (60 min)
              - generic [ref=f1e402]: No room on this class — availability can't be checked
            - generic [ref=f1e406]:
              - generic [ref=f1e407]:
                - text: From
                - paragraph [ref=f1e408]: Thu, Sep 10 · 10:37 AM – 11:37 AM
              - generic [ref=f1e411]:
                - text: To
                - paragraph [ref=f1e412]: Thu, Sep 10 · 10:37 AM – 11:37 AM
            - generic [ref=f1e413]:
              - generic [ref=f1e414]:
                - text: Reason
                - generic [ref=f1e415]: (optional)
              - textbox "e.g. Room conflict, instructor schedule change..." [ref=f1e416]
            - generic [ref=f1e417]:
              - generic [ref=f1e422]:
                - paragraph [ref=f1e423]: Notify Members
                - paragraph [ref=f1e424]: Send notification to all club members about this change
              - button [ref=f1e425] [cursor=pointer]
          - generic [ref=f1e427]:
            - button "Keep class" [ref=f1e428] [cursor=pointer]
            - button "Save Changes" [ref=f1e429] [cursor=pointer]
  - alert [ref=f1e432]
```

# Test source

```ts
  140 |   }
  141 | 
  142 |   /**
  143 |    * Click Save Changes and wait for whichever call the app decides to make.
  144 |    *
  145 |    * TWO endpoints are accepted, because the dialog picks between them: a
  146 |    * one-off edit goes to `updateClassApi` (`PUT .../classes/:id`) while some
  147 |    * branches use `rescheduleClassApi` (`POST .../reschedule`) instead
  148 |    * (`calendar/page.tsx` ~lines 842-870). Waiting on only the PUT times out
  149 |    * against a save that in fact succeeded.
  150 |    *
  151 |    * The enabled-check first is not defensive padding: the submit is disabled
  152 |    * while `!newDate || !newStartTime || !newEndTime` (`CancelClassModal.tsx`
  153 |    * line 495), so a half-filled dialog produces a silent no-op followed by a
  154 |    * timeout that says nothing about the cause.
  155 |    */
  156 |   private async save(): Promise<{ ok: boolean; status: number }> {
  157 |     const submit = calendarLocators.saveChanges(this.page);
  158 |     await expect(
  159 |       submit,
  160 |       'the Change Class dialog will not submit - Save Changes is disabled, ' +
  161 |         'which means date, start time or end time is unset.',
  162 |     ).toBeEnabled({ timeout: RENDER_TIMEOUT });
  163 | 
  164 |     const pending = this.page.waitForResponse(
  165 |       (r) => {
  166 |         const path = new URL(r.url()).pathname;
  167 |         const method = r.request().method();
  168 |         return (
  169 |           (/\/classes\/[^/]+$/.test(path) && method === 'PUT') ||
  170 |           (path.endsWith('/reschedule') && method === 'POST')
  171 |         );
  172 |       },
  173 |       { timeout: RENDER_TIMEOUT },
  174 |     );
  175 |     await submit.click();
  176 |     const res = await pending;
  177 |     return { ok: res.ok(), status: res.status() };
  178 |   }
  179 | 
  180 |   /**
  181 |    * Open the cancel dialog and report whether confirming is blocked.
  182 |    *
  183 |    * `OWP-009` step 6: a ONE-OFF class inside the two-hour window cannot be
  184 |    * soft-cancelled. The guard is frontend-only - `LATE_CANCEL_THRESHOLD_HOURS
  185 |    * = 2` and `cancelBlocked = mode === 'cancel' && isLateCancel &&
  186 |    * !isRecurring` (`CancelClassModal.tsx` lines 10 and 111). The backend's
  187 |    * `cancelClass` has no such check, which matches the sheet: it describes
  188 |    * what the tester sees.
  189 |    */
  190 |   async openCancelDialogAndCheckBlocked(): Promise<boolean> {
  191 |     const cancel = calendarLocators.eventCancel(this.page);
  192 |     await expect(
  193 |       cancel,
  194 |       'the event detail popup should offer "Cancel" for a class.',
  195 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  196 |     await cancel.click();
  197 | 
  198 |     const confirm = calendarLocators.confirmCancelClass(this.page);
  199 |     await expect(
  200 |       confirm,
  201 |       'the cancel dialog should show its confirm button.',
  202 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  203 | 
  204 |     return confirm.isDisabled();
  205 |   }
  206 | 
  207 |   /**
  208 |    * `OWP-009` step 5 - the live countdown near the top of the cancel dialog.
  209 |    * Returns whatever it says, so the spec can assert the shape rather than a
  210 |    * specific number that depends on when the suite ran.
  211 |    */
  212 |   async readCountdown(): Promise<string> {
  213 |     const label = calendarLocators.cancelCountdown(this.page);
  214 |     await expect(
  215 |       label,
  216 |       'the cancel dialog should carry a live countdown to the class start ' +
  217 |         '("Commence dans ...", "Starts in ...") or an ongoing/ended label.',
  218 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  219 |     return ((await label.textContent()) ?? '').trim();
  220 |   }
  221 | 
  222 |   /**
  223 |    * Pin the scope picker's shape before anything indexes into it.
  224 |    *
  225 |    * The options are addressed by POSITION (0 = this, 1 = future, 2 = all)
  226 |    * because for the French owner they carry no accessible name at all - only
  227 |    * raw i18n key paths. Position is safe exactly as long as the option count
  228 |    * is what this suite thinks it is, so the count is asserted first, the same
  229 |    * guard `expectCreateFormReady` gives the positional site fields.
  230 |    *
  231 |    * Reschedule mode offers two options and cancel mode three
  232 |    * (`CancelClassModal.tsx` line 228).
  233 |    */
  234 |   async expectScopePicker(expected: number): Promise<void> {
  235 |     await expect(
  236 |       calendarLocators.scopeOptions(this.page),
  237 |       `the recurring scope picker should offer ${expected} choices. If this ` +
  238 |         `count has changed, the positional indexing in this page object is ` +
  239 |         `now pointing at the wrong buttons - fix it before trusting a result.`,
> 240 |     ).toHaveCount(expected, { timeout: RENDER_TIMEOUT });
      |       ^ Error: the recurring scope picker should offer 2 choices. If this count has changed, the positional indexing in this page object is now pointing at the wrong buttons - fix it before trusting a result.
  241 |   }
  242 | 
  243 |   /** Choose a scope: 0 = this occurrence, 1 = this and future, 2 = all. */
  244 |   async chooseScope(index: 0 | 1 | 2): Promise<void> {
  245 |     await calendarLocators.scopeOptions(this.page).nth(index).click();
  246 |   }
  247 | 
  248 |   /**
  249 |    * `OWP-009` step 7 - the series-wide settings that appear once the scope is
  250 |    * wider than one occurrence.
  251 |    */
  252 |   async expectSeriesSettings(): Promise<void> {
  253 |     await expect(
  254 |       calendarLocators.seriesSettings(this.page),
  255 |       'choosing a scope beyond this single occurrence should reveal the ' +
  256 |         'series settings - the sheet expects the name, description and the ' +
  257 |         "series' end date or occurrence count to be editable from this same " +
  258 |         'dialog.',
  259 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  260 | 
  261 |     await expect(
  262 |       calendarLocators.seriesLimitToggle(this.page),
  263 |       'the series settings should offer both ways of bounding the series - ' +
  264 |         'an end date and a number of classes.',
  265 |     ).toHaveCount(2, { timeout: RENDER_TIMEOUT });
  266 |   }
  267 | 
  268 |   /**
  269 |    * `OWP-009` step 9 - open the teacher picker and report who it offers.
  270 |    *
  271 |    * The sheet asks for two things here: that the control exists on the popup
  272 |    * at all (rather than making you reopen the full edit form), and that it
  273 |    * offers REAL club members rather than placeholder names. This returns the
  274 |    * names so the spec can check them against the roster - a placeholder check
  275 |    * written as a pattern would be guesswork, since one of the seeded staff is
  276 |    * genuinely called "Sensei alnf".
  277 |    */
  278 |   async openTeacherPicker(): Promise<string[]> {
  279 |     const trigger = calendarLocators.teacherPicker(this.page);
  280 |     await expect(
  281 |       trigger,
  282 |       'the event detail popup should carry a teacher control - the sheet ' +
  283 |         'expects a quick reassignment picker here rather than a trip back ' +
  284 |         'through the full edit form.',
  285 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  286 |     await trigger.click();
  287 | 
  288 |     const options = calendarLocators.teacherOptions(this.page);
  289 |     await expect(
  290 |       options.first(),
  291 |       'the teacher picker opened with nobody in it.',
  292 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  293 | 
  294 |     return (await options.allTextContents()).map((t) => t.trim().replace(/\s+/g, ' '));
  295 |   }
  296 | 
  297 |   /** Pick a teacher from the open picker and report the server's answer. */
  298 |   async assignTeacher(name: string): Promise<{ ok: boolean; status: number }> {
  299 |     const pending = this.page.waitForResponse(
  300 |       (r) =>
  301 |         /\/classes\/[^/]+$/.test(new URL(r.url()).pathname) &&
  302 |         r.request().method() === 'PUT',
  303 |       { timeout: RENDER_TIMEOUT },
  304 |     );
  305 |     await calendarLocators
  306 |       .teacherOptions(this.page)
  307 |       .filter({ hasText: name })
  308 |       .first()
  309 |       .click();
  310 |     const res = await pending;
  311 |     return { ok: res.ok(), status: res.status() };
  312 |   }
  313 | 
  314 |   /**
  315 |    * `OWP-009` step 8 - confirm the cancellation and report the server's answer.
  316 |    *
  317 |    * `POST /clubs/:clubId/classes/:classId/cancel` (`classController.js` line
  318 |    * 544). A soft cancel, not a delete: the class stays on the calendar marked
  319 |    * cancelled, which is why the caller still cleans it up afterwards.
  320 |    */
  321 |   async confirmCancel(): Promise<{ ok: boolean; status: number }> {
  322 |     const pending = this.page.waitForResponse(
  323 |       (r) =>
  324 |         new URL(r.url()).pathname.endsWith('/cancel') &&
  325 |         r.request().method() === 'POST',
  326 |       { timeout: RENDER_TIMEOUT },
  327 |     );
  328 |     await calendarLocators.confirmCancelClass(this.page).click();
  329 |     const res = await pending;
  330 |     return { ok: res.ok(), status: res.status() };
  331 |   }
  332 | }
  333 | 
```