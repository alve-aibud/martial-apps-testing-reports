# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/05-viewport-layout/responsive-nav.spec.ts >> XC-002 - the layout re-arranges at the 768px boundary >> XC-002 - desktop shows the full sidebar @XC-002
- Location: e2e/specs/smoke-testing/05-viewport-layout/responsive-nav.spec.ts:143:7

# Error details

```
Error: the sidebar offers all 8 destinations (home, calendar, messages, progress, library, support, payment, myClub).

expect(locator).toHaveCount(expected) failed

Locator:  locator('[data-track="nav:home"]:visible, [data-track="nav:calendar"]:visible, [data-track="nav:messages"]:visible, [data-track="nav:progress"]:visible, [data-track="nav:library"]:visible, [data-track="nav:support"]:visible, [data-track="nav:payment"]:visible, [data-track="nav:myClub"]:visible')
Expected: 8
Received: 7
Timeout:  10000ms

Call log:
  - the sidebar offers all 8 destinations (home, calendar, messages, progress, library, support, payment, myClub). with timeout 10000ms
  - waiting for locator('[data-track="nav:home"]:visible, [data-track="nav:calendar"]:visible, [data-track="nav:messages"]:visible, [data-track="nav:progress"]:visible, [data-track="nav:library"]:visible, [data-track="nav:support"]:visible, [data-track="nav:payment"]:visible, [data-track="nav:myClub"]:visible')
    23 × locator resolved to 7 elements
       - unexpected value "7"

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
        - link "Progress" [ref=e15] [cursor=pointer]:
          - /url: /progress
        - link "Library" [ref=e17] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/library?standalone=true
        - link "Support" [ref=e19] [cursor=pointer]:
          - /url: /support
        - link "Curriculum" [ref=e21] [cursor=pointer]:
          - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/curricula
        - link "My Club" [ref=e23] [cursor=pointer]:
          - /url: /dashboard/myClub
    - banner [ref=e25]:
      - generic [ref=e26]:
        - generic [ref=e27]:
          - link "Martial Apps Logo Martial Apps · Automation Club" [ref=e29] [cursor=pointer]:
            - /url: /dashboard
            - img "Martial Apps Logo" [ref=e30]
            - generic [ref=e31]:
              - generic [ref=e32]: Martial Apps
              - generic [ref=e33]: · Automation Club
          - generic [ref=e34]:
            - generic [ref=e35]:
              - button "Send feedback" [ref=e36] [cursor=pointer]
              - link "3" [ref=e46] [cursor=pointer]:
                - /url: /dashboard/messages
              - button "52" [ref=e51] [cursor=pointer]
            - button "A" [ref=e57] [cursor=pointer]
        - generic [ref=e64]:
          - button "Overview" [ref=e65] [cursor=pointer]
          - button "Sites" [ref=e66] [cursor=pointer]
    - main [ref=e67]:
      - generic [ref=e68]:
        - generic [ref=e70]:
          - button [ref=e71] [cursor=pointer]:
            - generic [ref=e72]:
              - heading "My Clubs" [level=2] [ref=e73]
              - paragraph [ref=e74]: Manage & switch clubs
          - generic [ref=e78]:
            - generic [ref=e79]: 🥋
            - generic:
              - paragraph: Automation Club
              - paragraph: 7 members
          - generic [ref=e84]:
            - button "Create" [ref=e85] [cursor=pointer]
            - button "Join" [ref=e88] [cursor=pointer]
        - generic [ref=e91]:
          - generic [ref=e92]: 🥋
          - generic [ref=e99]:
            - heading "Automation Club" [level=1] [ref=e101]
            - button "Switch club" [ref=e103] [cursor=pointer]:
              - img "Switch" [ref=e104]
              - text: Switch club
          - generic [ref=e107]:
            - generic [ref=e108]:
              - heading "Attendance" [level=3] [ref=e110]
              - link [ref=e111] [cursor=pointer]:
                - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/scan-attendance
                - generic [ref=e114]:
                  - paragraph [ref=e115]: Scan Attendance
                  - paragraph [ref=e116]: Mark your attendance for today's class
            - generic [ref=e119]:
              - heading "Training & Progress" [level=3] [ref=e121]
              - generic [ref=e122]:
                - link [ref=e123] [cursor=pointer]:
                  - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/progress
                  - img "My Progress" [ref=e124]
                  - generic [ref=e125]:
                    - heading "My Progress" [level=3] [ref=e126]
                    - paragraph [ref=e127]: Track your training progress
                - link [ref=e130] [cursor=pointer]:
                  - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/calendar
                  - img "Class Schedule" [ref=e131]
                  - generic [ref=e132]:
                    - heading "Class Schedule" [level=3] [ref=e133]
                    - paragraph [ref=e134]: View upcoming classes
                - link [ref=e137] [cursor=pointer]:
                  - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/library
                  - img "Video Library" [ref=e138]
                  - generic [ref=e139]:
                    - heading "Video Library" [level=3] [ref=e140]
                    - paragraph [ref=e141]: View, learn and practice moves
                - link [ref=e144] [cursor=pointer]:
                  - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/curricula
                  - img "View Curriculum" [ref=e145]
                  - generic [ref=e146]:
                    - heading "View Curriculum" [level=3] [ref=e147]
                    - paragraph [ref=e148]: Browse and assign yourself to available curriculum
            - generic [ref=e151]:
              - heading "Belts & Activity" [level=3] [ref=e153]
              - link [ref=e155] [cursor=pointer]:
                - /url: /dashboard/myClub/cms4gce000003pkwujx5570uv/belts
                - img "Belt System" [ref=e156]
                - generic [ref=e157]:
                  - heading "Belt System" [level=3] [ref=e158]
                  - paragraph [ref=e159]: View club belt hierarchy
            - generic [ref=e162]:
              - heading "Your Status" [level=3] [ref=e164]
              - generic [ref=e165]:
                - generic [ref=e166]:
                  - generic [ref=e167]: Your Role
                  - generic [ref=e168]: Member
                - generic [ref=e169]:
                  - generic [ref=e170]: Current Belt
                  - generic [ref=e171]: 9th Kyu White-Yellow Belt
  - alert [ref=e174]
```

# Test source

```ts
  69  |     });
  70  | 
  71  |     await page.setViewportSize(WIDTHS.phone);
  72  |     await clubSection('overview').gotoAndAwaitClubRole(clubRoute(clubId, 'overview'));
  73  | 
  74  |     await expect(
  75  |       navLocators.bottomNav(page),
  76  |       'at 390px the phone bottom bar should be on screen (BottomNavigation is md:hidden).',
  77  |     ).toBeVisible();
  78  | 
  79  |     await expect(
  80  |       navLocators.desktopSidebar(page),
  81  |       'at 390px the desktop sidebar should be hidden (it is `hidden md:flex`). ' +
  82  |         'Both navs on screen at once is the classic responsive regression.',
  83  |     ).toBeHidden();
  84  | 
  85  |     await expect(
  86  |       navLocators.allLinks(page),
  87  |       `a plain member's phone bar offers ${BOTTOM_NAV_KEYS_MEMBER.length} ` +
  88  |         `destinations (${BOTTOM_NAV_KEYS_MEMBER.join(', ')}) against the ` +
  89  |         `sidebar's ${NAV_KEYS.length}. If this reads 6, check the role - ` +
  90  |         `Support is owner-gated on this bar (BottomNavigation.tsx line 146), ` +
  91  |         `not responsive.`,
  92  |     ).toHaveCount(BOTTOM_NAV_KEYS_MEMBER.length);
  93  | 
  94  |     // Named individually rather than left to the count: a count alone would
  95  |     // still pass if one of these appeared and a different item vanished.
  96  |     for (const key of SIDEBAR_ONLY_KEYS) {
  97  |       await expect(
  98  |         navLocators.link(page, key),
  99  |         `"${key}" is a sidebar-only destination - the phone bar drops it ` +
  100 |           `entirely, for every role.`,
  101 |       ).toBeHidden();
  102 |     }
  103 |   });
  104 | 
  105 |   test('XC-002 - the swap happens exactly at 768px @XC-002', async ({
  106 |     page,
  107 |     clubSection,
  108 |     clubId,
  109 |   }, testInfo) => {
  110 |     testInfo.annotations.push({
  111 |       type: 'manual-scenario',
  112 |       description: 'manual-qa/cross-cutting-checks.md#XC-002 (step 2, the boundary)',
  113 |     });
  114 | 
  115 |     await clubSection('overview').gotoAndAwaitClubRole(clubRoute(clubId, 'overview'));
  116 | 
  117 |     // One pixel below: still the phone layout.
  118 |     await page.setViewportSize(WIDTHS.belowBoundary);
  119 |     await expect(
  120 |       navLocators.bottomNav(page),
  121 |       'at 767px - one pixel below the breakpoint - the bottom bar should still be there.',
  122 |     ).toBeVisible();
  123 |     await expect(
  124 |       navLocators.desktopSidebar(page),
  125 |       'at 767px the sidebar should not have arrived yet. If it has, the ' +
  126 |         'breakpoint is not where `md:` puts it.',
  127 |     ).toBeHidden();
  128 | 
  129 |     // At 768 exactly: the sidebar takes over.
  130 |     await page.setViewportSize(WIDTHS.boundary);
  131 |     await expect(
  132 |       navLocators.desktopSidebar(page),
  133 |       'at 768px exactly the sidebar should appear - `md:` is min-width:768px, ' +
  134 |         'so the breakpoint is inclusive.',
  135 |     ).toBeVisible();
  136 |     await expect(
  137 |       navLocators.bottomNav(page),
  138 |       'at 768px the bottom bar should be gone. Both at once means the two ' +
  139 |         'breakpoints have drifted apart.',
  140 |     ).toBeHidden();
  141 |   });
  142 | 
  143 |   test('XC-002 - desktop shows the full sidebar @XC-002', async ({
  144 |     page,
  145 |     clubSection,
  146 |     clubId,
  147 |   }, testInfo) => {
  148 |     testInfo.annotations.push({
  149 |       type: 'manual-scenario',
  150 |       description: 'manual-qa/cross-cutting-checks.md#XC-002 (step 4)',
  151 |     });
  152 | 
  153 |     await page.setViewportSize(WIDTHS.desktop);
  154 |     await clubSection('overview').gotoAndAwaitClubRole(clubRoute(clubId, 'overview'));
  155 | 
  156 |     await expect(
  157 |       navLocators.desktopSidebar(page),
  158 |       'at 1280px the sidebar should be on screen.',
  159 |     ).toBeVisible();
  160 | 
  161 |     await expect(
  162 |       navLocators.bottomNav(page),
  163 |       'at 1280px the phone bottom bar should be hidden.',
  164 |     ).toBeHidden();
  165 | 
  166 |     await expect(
  167 |       navLocators.allLinks(page),
  168 |       `the sidebar offers all ${NAV_KEYS.length} destinations (${NAV_KEYS.join(', ')}).`,
> 169 |     ).toHaveCount(NAV_KEYS.length);
      |       ^ Error: the sidebar offers all 8 destinations (home, calendar, messages, progress, library, support, payment, myClub).
  170 |   });
  171 | });
  172 | 
  173 | /**
  174 |  * Steps 3, 5 and 6 are not automated here.
  175 |  *
  176 |  * Step 3 ("no column squashed, no chip cut off at ~768") is a human eye
  177 |  * judgement - there is no assertion for "unreadably squashed" that is not
  178 |  * really a screenshot diff, which this suite does not do.
  179 |  *
  180 |  * Steps 5 and 6 (Messages two-panel on desktop, collapsing to one panel below
  181 |  * 768) ARE automatable and worth adding. They are left out only because the
  182 |  * seeded member has no conversations, so the two-panel state has nothing to
  183 |  * render into - the same blocker `MEM-006` hits on the calendar. They unblock
  184 |  * with the two-session fixture that group 16 needs anyway.
  185 |  */
  186 | 
```