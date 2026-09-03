# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/12-profile/member.settings-writes.spec.ts >> The settings page saves real changes and reverses them >> MEM-019 - password mismatch blocks submit, privacy and push toggles round-trip @MEM-019 @destructive
- Location: e2e/specs/smoke-testing/12-profile/member.settings-writes.spec.ts:61:7

# Error details

```
Error: expected /settings to render for this role, but its content never appeared

expect(locator).toBeVisible() failed

Locator: getByRole('heading', { name: /^(Settings|Param[èe]tres)$/i }).first()
Expected: visible
Timeout: 30000ms
Error: element(s) not found

Call log:
  - expected /settings to render for this role, but its content never appeared with timeout 30000ms
  - waiting for getByRole('heading', { name: /^(Settings|Param[èe]tres)$/i }).first()
    - waiting for navigation to finish...
    - navigated to "https://dev-testing.martialapps.com/login?session=expired"

```

```yaml
- navigation:
  - img "Martial Apps"
  - text: MartialApps
  - link "Join Us":
    - /url: /register
- paragraph: Welcome back! Manage your club and track progress.
- text: Email Address
- textbox "you@example.com"
- text: Password
- textbox "********"
- button "Show password"
- button "Log In"
- link "Forgot password?":
  - /url: /forgot-password
- paragraph:
  - text: Don't have an account?
  - link "Sign up":
    - /url: /register
- contentinfo:
  - navigation:
    - link "Privacy Policy":
      - /url: /privacy
    - link "Terms of Service":
      - /url: /terms
  - paragraph: © 2026 Martial Apps. All rights reserved.
- alert
```

# Test source

```ts
  1   | import { expect, type Page, type Locator } from '@playwright/test';
  2   | import { BasePage, RENDER_TIMEOUT } from './base.page';
  3   | import { personalLocators } from '../locators/personal.locators';
  4   | import type { NamedControl } from './club-section.page';
  5   | 
  6   | /** Pulls the first run of digits out of a string, or `undefined` if none. */
  7   | function firstNumber(text: string): number | undefined {
  8   |   const match = text.match(/\d+/);
  9   |   return match ? Number(match[0]) : undefined;
  10  | }
  11  | 
  12  | export type PersonalSection =
  13  |   | 'belts'
  14  |   | 'progress'
  15  |   | 'settings'
  16  |   | 'notifications'
  17  |   | 'household';
  18  | 
  19  | /**
  20  |  * Any PERSONAL screen - one with no club id in its URL.
  21  |  *
  22  |  * One object rather than a class per page, for the same reason ClubSectionPage
  23  |  * gives: these pages differ only by which marker proves they rendered. What
  24  |  * differs from ClubSectionPage is the question being asked. There is no role
  25  |  * guard here and no denial screen to expect: every authenticated account
  26  |  * reaches these pages, and the test is what they are OFFERED once there.
  27  |  *
  28  |  * DashboardPage stays its own object - it is the personal home screen with its
  29  |  * own cards, and it predates this.
  30  |  */
  31  | export class PersonalSectionPage extends BasePage {
  32  |   constructor(page: Page, private readonly section: PersonalSection) {
  33  |     super(page);
  34  |   }
  35  | 
  36  |   private marker(): Locator {
  37  |     return personalLocators[this.section](this.page);
  38  |   }
  39  | 
  40  |   /**
  41  |    * Navigate and assert the real page rendered.
  42  |    *
  43  |    * `gotoAndAwaitClubRole`, not `goto`, even though these routes are not
  44  |    * club-scoped: their contents are still gated on data from
  45  |    * `GET /users/clubs` - the belts page picks its club from it
  46  |    * (belts/page.tsx ~line 45) and decides the Eligible Students control from
  47  |    * the role it reports (~line 130). Judge either before it answers and the
  48  |    * result describes the loading frame, not the role.
  49  |    *
  50  |    * `/settings` does not read the club role for its TAB BAR - that is gated on
  51  |    * the profile type - but it is covered by the same wait because the
  52  |    * authenticated layout issues `GET /users/clubs` on every page beneath it,
  53  |    * so the response arrives here too and the settings page's own club-scoped
  54  |    * content (the privacy toggles) depends on it. If a future personal route
  55  |    * ever loads WITHOUT that call, the wait falls through on its timeout rather
  56  |    * than failing - correct, but it would cost the full CLUB_ROLE_TIMEOUT, so
  57  |    * give that route a plain `goto` instead.
  58  |    */
  59  |   async expectLoaded(route: string): Promise<void> {
  60  |     await this.gotoAndAwaitClubRole(route);
  61  | 
  62  |     await expect(
  63  |       this.marker(),
  64  |       `expected ${route} to render for this role, but its content never appeared`,
> 65  |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
      |       ^ Error: expected /settings to render for this role, but its content never appeared
  66  |   }
  67  | 
  68  |   /** Load, then assert further markers the sheet expects on the page. */
  69  |   async expectMarkers(route: string, markers: NamedControl[]): Promise<void> {
  70  |     await this.expectLoaded(route);
  71  | 
  72  |     for (const { label, locator } of markers) {
  73  |       await expect(
  74  |         locator(this.page).first(),
  75  |         `"${label}" is missing from ${route}`,
  76  |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  77  |     }
  78  |   }
  79  | 
  80  |   /**
  81  |    * Load, then assert controls this role must not be offered.
  82  |    *
  83  |    * Waits for the page to render first, or the control is trivially absent
  84  |    * because nothing has loaded yet.
  85  |    */
  86  |   async expectControlsAbsent(route: string, controls: NamedControl[]): Promise<void> {
  87  |     await this.expectLoaded(route);
  88  | 
  89  |     for (const { label, locator } of controls) {
  90  |       await expect(
  91  |         locator(this.page),
  92  |         `"${label}" is visible on ${route} for a role that must not have it`,
  93  |       ).toHaveCount(0);
  94  |     }
  95  |   }
  96  | 
  97  |   /**
  98  |    * Both directions on one screen, in a single visit - see the twin on
  99  |    * ClubSectionPage. The belts page waits on `GET /users/clubs` before it can
  100 |    * decide either question, so asking them separately doubles the wait.
  101 |    */
  102 |   async expectControls(
  103 |     route: string,
  104 |     { present, absent }: { present: NamedControl[]; absent: NamedControl[] },
  105 |   ): Promise<void> {
  106 |     await this.expectLoaded(route);
  107 | 
  108 |     for (const { label, locator } of present) {
  109 |       await expect(
  110 |         locator(this.page).first(),
  111 |         `"${label}" did not appear on ${route} for a role that must have it`,
  112 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  113 |     }
  114 | 
  115 |     for (const { label, locator } of absent) {
  116 |       await expect(
  117 |         locator(this.page),
  118 |         `"${label}" is visible on ${route} for a role that must not have it`,
  119 |       ).toHaveCount(0);
  120 |     }
  121 |   }
  122 | 
  123 |   /**
  124 |    * The mirror image, and the reason the absence checks mean anything: prove
  125 |    * the very same selectors DO match for a role that should have the controls.
  126 |    * See the false-green rule in README section 7.
  127 |    */
  128 |   async expectControlsPresent(route: string, controls: NamedControl[]): Promise<void> {
  129 |     await this.expectLoaded(route);
  130 | 
  131 |     for (const { label, locator } of controls) {
  132 |       await expect(
  133 |         locator(this.page).first(),
  134 |         `"${label}" did not appear on ${route} for a role that must have it - ` +
  135 |           `the matching absence test elsewhere therefore proves nothing`,
  136 |       ).toBeVisible({ timeout: RENDER_TIMEOUT });
  137 |     }
  138 |   }
  139 | 
  140 |   /**
  141 |    * `MEM-003` step 3. Filters `/progress` down to one curriculum and reads
  142 |    * that curriculum's own stats-card total.
  143 |    *
  144 |    * **This is the only reachable "total" on this page without opening a
  145 |    * modal.** The sheet's step 1 describes a visible "classes attended versus
  146 |    * required" figure on load - but `classesAttendedProgress`
  147 |    * (`BeltProgressionPath.tsx` line ~369) only renders INSIDE a belt node's
  148 |    * detail popup (`node.status === 'current'`), reached by clicking a
  149 |    * specific node in the belt journey path. Found the hard way: an earlier
  150 |    * version of this method targeted that line directly and it was never
  151 |    * visible on a plain page load. The curriculum-filtered stats card
  152 |    * (`progress/page.tsx` line ~235) is the one number this page shows
  153 |    * without any click into a modal, so it stands in for both "the total"
  154 |    * and "the per-curriculum breakdown" - a fair substitution when, as here,
  155 |    * only one curriculum has any real attendance at all.
  156 |    */
  157 |   async readCurriculumTotal(curriculumName: string): Promise<number> {
  158 |     await personalLocators.progressCurriculumPill(this.page, curriculumName).click();
  159 | 
  160 |     const total = personalLocators.progressCurriculumStatsTotal(this.page);
  161 |     await expect(
  162 |       total,
  163 |       `selecting "${curriculumName}" should have revealed its own stats card.`,
  164 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
  165 |     const text = (await total.textContent()) ?? '';
```