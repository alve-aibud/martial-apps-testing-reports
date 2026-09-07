# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/14-child-profile/child.write-refused.spec.ts >> CHD-040 - a child session can send a direct message >> CHD-040 - a switched-in child CAN send a direct message @CHD-040 @destructive
- Location: e2e/specs/smoke-testing/14-child-profile/child.write-refused.spec.ts:157:7

# Error details

```
Error: a switched-in child session should be able to send a direct message - product has confirmed this is intended (see this file's header). Got 403.

expect(received).toBeTruthy()

Received: false
```

# Page snapshot

```yaml
- generic [ref=f1e1]:
  - generic [ref=f1e2]:
    - complementary [ref=f1e3]:
      - button "Expand sidebar" [ref=f1e5] [cursor=pointer]
      - navigation [ref=f1e8]:
        - link "Home" [ref=f1e9] [cursor=pointer]:
          - /url: /dashboard
        - link "Message" [ref=f1e11] [cursor=pointer]:
          - /url: /dashboard/messages
        - link "Progress" [ref=f1e13] [cursor=pointer]:
          - /url: /progress
        - link "Library" [ref=f1e15] [cursor=pointer]:
          - /url: /library
    - banner [ref=f1e17]:
      - generic [ref=f1e19]:
        - link "Martial Apps Logo Martial Apps" [ref=f1e21] [cursor=pointer]:
          - /url: /dashboard
          - img "Martial Apps Logo" [ref=f1e22]
          - generic [ref=f1e23]: Martial Apps
        - generic [ref=f1e25]:
          - generic [ref=f1e26]:
            - button "Send feedback" [ref=f1e27] [cursor=pointer]
            - link "Messages" [ref=f1e37] [cursor=pointer]:
              - /url: /dashboard/messages
          - button "A" [active] [ref=f1e41] [cursor=pointer]
    - main [ref=f1e48]:
      - generic [ref=f1e49]:
        - generic [ref=f1e53]:
          - heading "Welcome!" [level=1] [ref=f1e55]
          - paragraph [ref=f1e57]: Your journey to mastery continues.
        - generic [ref=f1e59]:
          - generic [ref=f1e60]:
            - generic [ref=f1e62]:
              - generic [ref=f1e63]: 0%
              - generic [ref=f1e68]:
                - heading "None" [level=2] [ref=f1e69]
                - paragraph [ref=f1e70]: Progress to next rank
              - button "View my path" [ref=f1e71] [cursor=pointer]
            - generic [ref=f1e72]:
              - generic [ref=f1e73]:
                - heading "0" [level=3] [ref=f1e77]
                - paragraph [ref=f1e78]: Classes this month
              - generic [ref=f1e79]:
                - heading "30h 53m" [level=3] [ref=f1e83]
                - paragraph [ref=f1e84]: Hours in app
            - generic [ref=f1e85]:
              - generic [ref=f1e86]:
                - heading "My Clubs" [level=2] [ref=f1e89]
                - paragraph [ref=f1e93]: No clubs joined yet.
              - generic [ref=f1e94]:
                - heading "My Household" [level=2] [ref=f1e97]
                - button "AH Automation house 2 members · 1 children" [ref=f1e98] [cursor=pointer]:
                  - generic [ref=f1e99]: AH
                  - generic [ref=f1e100]:
                    - paragraph [ref=f1e101]: Automation house
                    - paragraph [ref=f1e102]: 2 members · 1 children
          - generic [ref=f1e105]:
            - generic [ref=f1e106]:
              - heading "Club Announcements" [level=2] [ref=f1e110]
              - paragraph [ref=f1e114]: Join a club to see announcements and stay connected with your community.
            - generic [ref=f1e115]:
              - heading "Recent Activity" [level=2] [ref=f1e118]
              - paragraph [ref=f1e122]: No recent activity.
  - alert [ref=f1e123]
```

# Test source

```ts
  90  |      * matching `ONB-020`'s own "profile incomplete" edge case: a child's
  91  |      * profile is never marked complete, so it 403s the same way any
  92  |      * incomplete adult profile would.
  93  |      */
  94  |     const householdRes = await page.request.post(`${env.apiURL}/households`, {
  95  |       headers: { 'x-app-id': env.appId },
  96  |       data: { householdName: '[e2e] CHD-040 should not exist' },
  97  |     });
  98  |     expect(
  99  |       householdRes.status(),
  100 |       'a child session should be refused household creation with a clear ' +
  101 |         `403, not a silent no-op or a 2xx. Got ${householdRes.status()}.`,
  102 |     ).toBe(403);
  103 | 
  104 |     // Step 4 - creating a support ticket. Confirmed refused - no cleanup
  105 |     // needed since nothing was created (and `supportRoutes.js` has no
  106 |     // DELETE for tickets at all if it ever were).
  107 |     const ticketRes = await page.request.post(`${env.apiURL}/tickets`, {
  108 |       headers: { 'x-app-id': env.appId },
  109 |       data: {
  110 |         category: 'help',
  111 |         description: `[e2e] CHD-040 child-session support ticket attempt ${Date.now().toString(36)}`,
  112 |       },
  113 |     });
  114 |     expect(
  115 |       ticketRes.ok(),
  116 |       `a child session should not be able to create a support ticket. Got ${ticketRes.status()}.`,
  117 |     ).toBeFalsy();
  118 |   });
  119 | });
  120 | 
  121 | /**
  122 |  * ============================================================================
  123 |  * CORRECTED 2026-08-25 - a switched-in child session sending a direct
  124 |  * message to an adult club member is intended, not a safety gap.
  125 |  * ============================================================================
  126 |  *
  127 |  * Until 2026-08-20 this was pinned red on the belief that hiding Messages
  128 |  * from the child nav (`CHD-001`) meant a child should not be able to message
  129 |  * an adult unsupervised at all, and that the send succeeding anyway was the
  130 |  * hiding being "cosmetic only" - a real bug worth blocking on. That premise
  131 |  * came from the sheet's own (then-uncorrected) expectation for `CHD-001`.
  132 |  * Product has since confirmed the opposite for `CHD-001`: a child account is
  133 |  * meant to have Messaging access, not have it hidden. This test's rationale
  134 |  * was built entirely on the assumption `CHD-001` just retracted, so it is
  135 |  * retracted with it - not because the app changed, but because the
  136 |  * requirement it was checking against did.
  137 |  *
  138 |  * **What actually happens, unchanged:** `clubMembershipMiddleware.js`'s
  139 |  * `checkClubMembership` admits a switched-in child as a genuine, active
  140 |  * `ClubMember` (its own comment: "when a child is switched in, their token
  141 |  * has their own personId and the ClubMember record has both personId +
  142 |  * childProfileId set"), and neither `directMessageService.js`'s route
  143 |  * (`messageRoutes.js`, `POST /:clubId/messages/direct`) nor the service
  144 |  * function itself refuse a switched-in child sender. That is now asserted as
  145 |  * the intended behaviour rather than treated as an unchecked gap.
  146 |  *
  147 |  * The message this test sends is still cleaned up
  148 |  * (`DELETE .../messages/direct/:messageId`) regardless of outcome.
  149 |  */
  150 | test.describe('CHD-040 - a child session can send a direct message', () => {
  151 |   test.beforeEach(async ({ profileSwitchPage }) => {
  152 |     const { name, pin } = assertChildProfilePresent();
  153 |     await profileSwitchPage.switchToChild(name, pin);
  154 |     await profileSwitchPage.closeAccountMenu();
  155 |   });
  156 | 
  157 |   test('CHD-040 - a switched-in child CAN send a direct message @CHD-040 @destructive', async ({
  158 |     page,
  159 |     ownerApi,
  160 |     clubId,
  161 |   }, testInfo) => {
  162 |     testInfo.annotations.push({
  163 |       type: 'manual-scenario',
  164 |       description: 'manual-qa/role-child.md#CHD-040 (step 2)',
  165 |     });
  166 | 
  167 |     const sensei = await findMemberIdByEmail(ownerApi, clubId, ROLES.sensei.email);
  168 |     expect(sensei, `${ROLES.sensei.email} was not found on the roster.`).toBeTruthy();
  169 | 
  170 |     let sentMessageId: string | undefined;
  171 |     try {
  172 |       const messageRes = await page.request.post(`${env.apiURL}/clubs/${clubId}/messages/direct`, {
  173 |         headers: { 'x-app-id': env.appId },
  174 |         data: {
  175 |           receiverMemberId: sensei?.memberId,
  176 |           content: `[e2e] CHD-040 child-session message attempt ${Date.now().toString(36)}`,
  177 |         },
  178 |       });
  179 | 
  180 |       if (messageRes.ok()) {
  181 |         const messageBody = await messageRes.json().catch(() => null);
  182 |         sentMessageId = messageBody?.data?.messageId ?? messageBody?.data?.id;
  183 |       }
  184 | 
  185 |       expect(
  186 |         messageRes.ok(),
  187 |         `a switched-in child session should be able to send a direct message ` +
  188 |           `- product has confirmed this is intended (see this file's header). ` +
  189 |           `Got ${messageRes.status()}.`,
> 190 |       ).toBeTruthy();
      |         ^ Error: a switched-in child session should be able to send a direct message - product has confirmed this is intended (see this file's header). Got 403.
  191 |     } finally {
  192 |       if (sentMessageId) {
  193 |         await ownerApi
  194 |           .delete(`${env.apiURL}/clubs/${clubId}/messages/direct/${sentMessageId}`, {
  195 |             headers: { 'x-app-id': env.appId },
  196 |           })
  197 |           .catch(() => undefined);
  198 |       }
  199 |     }
  200 |   });
  201 | });
  202 | 
```