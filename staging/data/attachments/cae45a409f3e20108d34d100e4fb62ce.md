# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/04-navigation-core-ui/secretary.blocked.spec.ts >> Secretary is blocked from owner-only screens >> SEC-043 - secretary blocked from Club Settings @SEC-043
- Location: e2e/specs/smoke-testing/04-navigation-core-ui/secretary.blocked.spec.ts:65:7

# Error details

```
Error: expected an access-denied screen on /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/settings, but none appeared

expect(locator).toBeVisible() failed

Locator: getByRole('heading', { name: /Access (Restricted|Denied)|Acc[eè]s (restreint|refus[eé])|\baccess(Restricted|Denied)\b/i })
Expected: visible
Timeout: 30000ms
Error: element(s) not found

Call log:
  - expected an access-denied screen on /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/settings, but none appeared with timeout 30000ms
  - waiting for getByRole('heading', { name: /Access (Restricted|Denied)|Acc[eè]s (restreint|refus[eé])|\baccess(Restricted|Denied)\b/i })

```

```yaml
- complementary:
  - button "Expand sidebar"
  - navigation:
    - link "Home":
      - /url: /dashboard
    - link "Calendar":
      - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/calendar
    - link "Message":
      - /url: /dashboard/messages
    - link "Members":
      - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/members
    - link "Progress":
      - /url: /progress
    - link "Library":
      - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/library?standalone=true
    - link "Support":
      - /url: /support
    - link "Payment":
      - /url: /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/payments
    - link "My Club":
      - /url: /dashboard/myClub
    - button "Share"
- banner:
  - link "Martial Apps Logo Martial Apps · Automation Club":
    - /url: /dashboard
    - img "Martial Apps Logo"
    - text: Martial Apps · Automation Club
  - button "Send feedback"
  - link "Messages":
    - /url: /dashboard/messages
  - button "28"
  - button "A"
  - button "Overview"
  - button "Members"
  - button "Sites"
- main:
  - heading "Club Settings" [level=1]
  - paragraph: Manage your club preferences and information
  - button "General"
  - button "Billing"
  - button "QR code"
  - heading "Update Club Logo" [level=4]
  - paragraph: Upload your club logo or emblem
  - text: 🥋
  - button
  - heading "Club Information" [level=4]
  - paragraph: Edit club name, motto & dates
  - text: Club Name
  - textbox "Enter club name": Automation Club
  - text: Website URL
  - textbox "https://yourclub.com"
  - text: Federation Affiliation
  - textbox "e.g., International Martial Arts Federation"
  - heading "Club Address" [level=4]
  - paragraph: Physical location and business registration
  - text: House Number
  - textbox "e.g., 12A"
  - text: Street Address
  - textbox "123 Main Street": 705-315 Holmwood Avenue Ottawa, ON K1S 2R2, Canada
  - text: City / Town
  - textbox "Toronto": Ottawa
  - text: Zip / Postal Code
  - textbox "M5V 1A1": K1S 2R2
  - text: Province / State
  - textbox "Ontario": "ON"
  - text: Country *
  - combobox: Canada
  - text: Company Registration Number
  - textbox "Optional — business number"
  - heading "Social Media Links" [level=4]
  - paragraph: Add links to your club's social media profiles
  - button "Add social link"
  - heading "Branding Theme" [level=4]
  - paragraph: Customize your club's colors
  - text: Primary Color
  - textbox "#FF5733": "#BC002D"
  - text: Secondary Color
  - textbox "#33FF57": "#6B6B6B"
  - button "Save Changes"
- alert
```

# Test source

```ts
  85  |    * default (Header.tsx ~line 1207 says so explicitly, to avoid a flash of
  86  |    * owner chrome).
  87  |    *
  88  |    * That default is why waiting matters in BOTH directions. An absence check
  89  |    * run before the response passes for every role including the owner - a
  90  |    * false green. A presence check run before it fails for the owner - a false
  91  |    * red, which is how this was found.
  92  |    *
  93  |    * The response promise is created BEFORE navigating, or a fast answer would
  94  |    * arrive before anyone is listening. If it never comes we fall through
  95  |    * rather than fail here: the assertion that follows gives a far better
  96  |    * message than a bare wait timeout would.
  97  |    */
  98  |   async gotoAndAwaitClubRole(route: string): Promise<void> {
  99  |     const clubRoleResolved = this.page
  100 |       .waitForResponse((res) => /\/users\/clubs\b/.test(res.url()), {
  101 |         timeout: CLUB_ROLE_TIMEOUT,
  102 |       })
  103 |       .catch(() => null);
  104 | 
  105 |     await this.goto(route);
  106 |     await clubRoleResolved;
  107 |   }
  108 | 
  109 |   /**
  110 |    * Navigate somewhere that is SUPPOSED to redirect, and assert where it lands.
  111 |    *
  112 |    * `goto` treats any redirect as a failure, which is right for a page that
  113 |    * should stay put and wrong for the handful that route onwards by design.
  114 |    * `/dashboard/myClub` is the live case: it is not a club picker, it pushes to
  115 |    * the first club the account belongs to (myClub/page.tsx ~line 13).
  116 |    *
  117 |    * The named traps still apply - a session problem lands on /login and must be
  118 |    * reported as a session problem, not as "the redirect went somewhere odd".
  119 |    */
  120 |   async gotoExpectingRedirect(from: string, expected: RegExp): Promise<void> {
  121 |     const clubRoleResolved = this.page
  122 |       .waitForResponse((res) => /\/users\/clubs\b/.test(res.url()), {
  123 |         timeout: CLUB_ROLE_TIMEOUT,
  124 |       })
  125 |       .catch(() => null);
  126 | 
  127 |     await this.page.goto(from, { waitUntil: 'domcontentloaded' });
  128 |     await clubRoleResolved;
  129 | 
  130 |     // The redirect is client-side - a router.push inside an effect - so it
  131 |     // happens after the response awaited above. Poll for the URL rather than
  132 |     // reading it once.
  133 |     try {
  134 |       await expect(this.page).toHaveURL(expected, { timeout: RENDER_TIMEOUT });
  135 |     } catch {
  136 |       // Landing somewhere unexpected is far more often a session problem than a
  137 |       // routing one, and "toHaveURL failed" hides that. Name the trap instead.
  138 |       const current = new URL(this.page.url()).pathname;
  139 |       throw new Error(
  140 |         `Expected ${from} to redirect to ${expected} but it is on ${current}.\n` +
  141 |           `Likely cause: ${trapReason(current)}.`,
  142 |       );
  143 |     }
  144 |   }
  145 | 
  146 |   /**
  147 |    * The authenticated layout force-redirects in two cases:
  148 |    *   - mustChangePassword          -> /change-password
  149 |    *   - profile incomplete + >14d   -> /complete-profile
  150 |    * Freshly seeded demo accounts hit these often. Without an explicit check the
  151 |    * failure reads "heading not found" with no clue why, so detect it and say
  152 |    * what actually happened.
  153 |    */
  154 |   private async assertNotRedirectedAway(expectedPath: string): Promise<void> {
  155 |     const current = new URL(this.page.url()).pathname;
  156 | 
  157 |     /**
  158 |      * Compare PATHS, ignoring any query string on the expected side.
  159 |      *
  160 |      * Some screens are reached with params that carry state rather than
  161 |      * identity - the room page's `?eventType=class&date=…&startTime=…`, which
  162 |      * is what the calendar routes to when creating a class. The app is free
  163 |      * to consume those and tidy them out of the URL, and it does. Without
  164 |      * this, `goto` read that as "redirected away" and failed a navigation
  165 |      * that had gone exactly where it was told.
  166 |      *
  167 |      * Callers that pass no query are unaffected: this only makes the
  168 |      * comparison less brittle, never more permissive about the trap cases
  169 |      * below, which are all bare paths.
  170 |      */
  171 |     const expected = expectedPath.split('?')[0];
  172 |     if (current === expected) return;
  173 | 
  174 |     throw new Error(
  175 |       `Expected to stay on ${expectedPath} but landed on ${current}.\n` +
  176 |         `Likely cause: ${trapReason(current)}.`,
  177 |     );
  178 |   }
  179 | 
  180 |   /** Assert the access-denied screen is showing. */
  181 |   async expectBlocked(route: string): Promise<void> {
  182 |     await expect(
  183 |       commonLocators.denialHeading(this.page),
  184 |       `expected an access-denied screen on ${route}, but none appeared`,
> 185 |     ).toBeVisible({ timeout: RENDER_TIMEOUT });
      |       ^ Error: expected an access-denied screen on /dashboard/myClub/cmtqw2tih005nmp7kgabxyl0y/settings, but none appeared
  186 |   }
  187 | 
  188 |   /** Assert the denial screen is NOT showing. Never sufficient on its own. */
  189 |   async expectNotBlocked(route: string): Promise<void> {
  190 |     await expect(
  191 |       commonLocators.denialHeading(this.page),
  192 |       `${route} rendered an access-denied screen for a role that should be allowed`,
  193 |     ).toBeHidden();
  194 |   }
  195 | }
  196 | 
```