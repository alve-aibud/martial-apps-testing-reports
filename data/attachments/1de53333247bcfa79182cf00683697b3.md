# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/09-calendar-scheduling/member.read-only.spec.ts >> Member views the club calendar >> MEM-006 - the calendar renders its empty state, not an error @MEM-006
- Location: e2e/specs/smoke-testing/09-calendar-scheduling/member.read-only.spec.ts:26:7

# Error details

```
Error: "empty-state copy" did not appear on /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar for a role that must have it - the matching absence test elsewhere therefore proves nothing

expect(locator).toBeVisible() failed

Locator: locator('p:visible').filter({ hasText: /^(No events scheduled|Aucun [ée]v[ée]nement pr[ée]vu)$/i }).first()
Expected: visible
Timeout: 30000ms
Error: element(s) not found

Call log:
  - "empty-state copy" did not appear on /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar for a role that must have it - the matching absence test elsewhere therefore proves nothing with timeout 30000ms
  - waiting for locator('p:visible').filter({ hasText: /^(No events scheduled|Aucun [ée]v[ée]nement pr[ée]vu)$/i }).first()

```

```yaml
- complementary:
  - button "Expand sidebar"
  - navigation:
    - link "Home":
      - /url: /dashboard
    - link "Calendar":
      - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar
    - link "Message":
      - /url: /dashboard/messages
    - link "Progress":
      - /url: /progress
    - link "Library":
      - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/library?standalone=true
    - link "Support":
      - /url: /support
    - link "Curriculum":
      - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/curricula
    - link "My Payments":
      - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/user-payments
    - link "My Club":
      - /url: /dashboard/myClub
- banner:
  - link "Martial Apps Logo Martial Apps · Automation Club":
    - /url: /dashboard
    - img "Martial Apps Logo"
    - text: Martial Apps · Automation Club
  - button "Send feedback"
  - link "2":
    - /url: /dashboard/messages
  - button "52"
  - button "A"
  - button "Overview"
  - button "Sites"
- main:
  - button "Collapse sidebar"
  - button "September 2026"
  - button
  - button
  - text: S M T W T F S 30 31
  - button "1"
  - button "2"
  - button "3"
  - button "4"
  - button "5"
  - button "6"
  - button "7"
  - button "8"
  - button "9"
  - button "10"
  - button "11"
  - button "12"
  - button "13"
  - button "14"
  - button "15"
  - button "16"
  - button "17"
  - button "18"
  - button "19"
  - button "20"
  - button "21"
  - button "22"
  - button "23"
  - button "24"
  - button "25"
  - button "26"
  - button "27"
  - button "28"
  - button "29"
  - button "30"
  - text: 1 2 3
  - button "Curricula":
    - heading "Curricula" [level=3]
  - button "View all →"
  - text: "[e2e] Automation [e2e] OWP-017 Curriculum [e2e] SEN-006 Curriculum [e2e] SEN-011 Target Curriculum"
  - button "Sites":
    - heading "Sites" [level=3]
  - button "View all →"
  - button "All Sites"
  - button "Automation Club - Main Site"
  - button "[e2e] Canada TZ - AB (Mountain)"
  - button "[e2e] Canada TZ - BC (Pacific)"
  - button "[e2e] Canada TZ - MB (Central)"
  - button "[e2e] Canada TZ - NL (Newfoundland)"
  - button "[e2e] Canada TZ - NS (Atlantic)"
  - button "[e2e] Canada TZ - ON (Eastern)"
  - button "[e2e] Exam Fixtures Site"
  - button "[e2e] site-mem003 mtkhpi55"
  - button "[e2e] site-postexam mtk7rac1"
  - button "[e2e] site-postexam mtk7ugy0"
  - button "[e2e] site-postexam mtk7zp0j"
  - button "[probe] tzfmt v1 mtahm16u"
  - button "[probe] tzfmt v2 mtahm2bh"
  - button "[probe] tzfmt v3 mtahm308"
  - button "Event Types":
    - heading "Event Types" [level=3]
  - text: Classes Exams Workshops Socials Tournaments
  - button "Expand sidebar"
  - button "Today"
  - button
  - button
  - heading "Wednesday, September 2, 2026" [level=2]
  - button "Day"
  - button "Week"
  - button "Month"
  - text: 6 AM 7 AM 8 AM 9 AM 10 AM 11 AM 12 PM 1 PM 2 PM 3 PM 4 PM 5 PM 6 PM 7 PM 8 PM 9 PM 10 PM 9:00 · [e2e] mem003-attend mtkhpik1 [e2e] mem003-attend mtkhpik1 9:00 – 10:00 am [e2e] site-mem003 mtkhpi55 · [e2e] room mtkhpig6
- alert
```

# Test source

```ts
  108 |   }
  109 | 
  110 |   /**
  111 |    * Arrive at this screen the way a user does - by following a route that
  112 |    * redirects here - and assert both where we landed and what is offered.
  113 |    *
  114 |    * `/dashboard/myClub` is the case this exists for. The sheets call it a club
  115 |    * picker; it is not. It redirects to the first club the account belongs to
  116 |    * (myClub/page.tsx ~line 13), so the thing to assert is the landing URL, not
  117 |    * a list of clubs to choose from.
  118 |    *
  119 |    * Controls are checked on the same load, because getting here costs a full
  120 |    * club-role resolution and these screens are slow.
  121 |    */
  122 |   async expectReachedVia(
  123 |     from: string,
  124 |     expectedRoute: string,
  125 |     controls: { present: NamedControl[]; absent: NamedControl[] } = { present: [], absent: [] },
  126 |   ): Promise<void> {
  127 |     const escaped = expectedRoute.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
  128 |     await this.gotoExpectingRedirect(from, new RegExp(`${escaped}/?$`));
  129 | 
  130 |     await expect(
  131 |       this.marker(),
  132 |       `${from} redirected to ${expectedRoute}, but the page never rendered for this role`,
  133 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  134 | 
  135 |     await this.expectNotBlocked(expectedRoute);
  136 | 
  137 |     for (const { label, locator } of controls.present) {
  138 |       await expect(
  139 |         locator(this.page).first(),
  140 |         `"${label}" did not appear on ${expectedRoute} for a role that must have it`,
  141 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  142 |     }
  143 | 
  144 |     for (const { label, locator } of controls.absent) {
  145 |       await expect(
  146 |         locator(this.page),
  147 |         `"${label}" is visible on ${expectedRoute} for a role that must not have it`,
  148 |       ).toHaveCount(0);
  149 |     }
  150 |   }
  151 | 
  152 |   /**
  153 |    * Both directions on one screen, in a single visit.
  154 |    *
  155 |    * The "you get these, but not those" scenarios - a staff role that is offered
  156 |    * the Members tab and withheld the Settings tab, for instance. Calling
  157 |    * expectControlsPresent and expectControlsAbsent in turn would load the page
  158 |    * twice, and these screens take 5-8s just to resolve their club role.
  159 |    *
  160 |    * The present list also does the absence list's positive-control work: it
  161 |    * proves the page reached the state where controls render at all, on the same
  162 |    * load, for the same role.
  163 |    */
  164 |   async expectControls(
  165 |     route: string,
  166 |     { present, absent }: { present: NamedControl[]; absent: NamedControl[] },
  167 |   ): Promise<void> {
  168 |     await this.gotoAndAwaitClubRole(route);
  169 | 
  170 |     await expect(
  171 |       this.marker(),
  172 |       `expected ${route} to render for this role, but its content never appeared`,
  173 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  174 | 
  175 |     for (const { label, locator } of present) {
  176 |       await expect(
  177 |         locator(this.page).first(),
  178 |         `"${label}" did not appear on ${route} for a role that must have it`,
  179 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  180 |     }
  181 | 
  182 |     for (const { label, locator } of absent) {
  183 |       await expect(
  184 |         locator(this.page),
  185 |         `"${label}" is visible on ${route} for a role that must not have it`,
  186 |       ).toHaveCount(0);
  187 |     }
  188 |   }
  189 | 
  190 |   /**
  191 |    * The mirror image, and the reason the absence checks mean anything: prove
  192 |    * the very same selectors DO match for a role that should have the controls.
  193 |    * See the false-green rule in README section 7.
  194 |    */
  195 |   async expectControlsPresent(route: string, controls: NamedControl[]): Promise<void> {
  196 |     await this.gotoAndAwaitClubRole(route);
  197 | 
  198 |     await expect(
  199 |       this.marker(),
  200 |       `expected ${route} to render for this role, but its content never appeared`,
  201 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  202 | 
  203 |     for (const { label, locator } of controls) {
  204 |       await expect(
  205 |         locator(this.page).first(),
  206 |         `"${label}" did not appear on ${route} for a role that must have it - ` +
  207 |           `the matching absence test elsewhere therefore proves nothing`,
> 208 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
      |         ^ Error: "empty-state copy" did not appear on /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar for a role that must have it - the matching absence test elsewhere therefore proves nothing
  209 |     }
  210 |   }
  211 | 
  212 |   /**
  213 |    * `MEM-004` step 2. Reads the "Classes at Current Belt" card's own total
  214 |    * on the club-scoped progress page, already open - the number directly
  215 |    * comparable to `/progress`'s total, since both come from the same
  216 |    * `useBeltProgression` hook (`progress/page.tsx` vs
  217 |    * `dashboard/myClub/[clubID]/progress/page.tsx`).
  218 |    */
  219 |   async readClassesAtCurrentBelt(): Promise<number> {
  220 |     const count = clubSectionLocators.classesAtCurrentBeltCount(this.page);
  221 |     await expect(count, 'the "Classes at Current Belt" number did not render.').toBeVisible({
  222 |       timeout: RENDER_TIMEOUT,
  223 |     });
  224 |     const text = (await count.textContent()) ?? '';
  225 |     const n = Number(text.trim());
  226 |     expect(Number.isFinite(n), `could not parse a number out of "${text}".`).toBeTruthy();
  227 |     return n;
  228 |   }
  229 | }
  230 | 
```