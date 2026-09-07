# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/11-exams-grading-belts/owner.delete-curriculum.spec.ts >> Owner deletes a curriculum >> OWP-032 - a curriculum with real history is refused, not silently deleted @OWP-032 @destructive
- Location: e2e/specs/smoke-testing/11-exams-grading-belts/owner.delete-curriculum.spec.ts:159:7

# Error details

```
Error: deleting a curriculum with real classes/exams/enrollments against it should be refused with a 409 ("Archive it instead"), not silently accepted. Got 404.

expect(received).toBe(expected) // Object.is equality

Expected: 409
Received: 404
```

# Test source

```ts
  78  |     });
  79  | 
  80  |     const name = scratchName('owp032-empty-curriculum');
  81  |     let classDefinitionId: string | undefined;
  82  |     let deleted = false;
  83  | 
  84  |     try {
  85  |       const created = await ownerApi.post(`${env.apiURL}/clubs/${clubId}/class-definitions`, {
  86  |         headers: { 'x-app-id': env.appId },
  87  |         data: {
  88  |           name,
  89  |           description: 'OWP-032 scratch curriculum - never used, safe to genuinely delete.',
  90  |         },
  91  |       });
  92  |       expect(created.ok(), `could not create the OWP-032 scratch curriculum (${created.status()}).`).toBeTruthy();
  93  |       const createdBody = await created.json().catch(() => null);
  94  |       classDefinitionId = createdBody?.data?.classDefinitionId ?? createdBody?.data?.id;
  95  |       expect(classDefinitionId, 'curriculum created but no id came back.').toBeTruthy();
  96  | 
  97  |       await clubSection('curricula').gotoAndAwaitClubRole(clubRoute(clubId, 'curricula'));
  98  | 
  99  |       // Step 2 - a Delete control is offered, distinct from Archive.
  100 |       const deleteTrigger = curriculaLocators.deleteTrigger(page, name);
  101 |       await expect(
  102 |         deleteTrigger,
  103 |         `the "${name}" card should offer a Delete control alongside Edit and Archive.`,
  104 |       ).toBeVisible({ timeout: 15_000 });
  105 |       await deleteTrigger.click();
  106 | 
  107 |       await expect(
  108 |         curriculaLocators.confirmDeleteDialogTitle(page),
  109 |         'clicking Delete should open the "Delete curriculum?" confirmation dialog.',
  110 |       ).toBeVisible({ timeout: 10_000 });
  111 | 
  112 |       // Step 3 - confirm, and watch the real DELETE response.
  113 |       const deleteCall = page.waitForResponse(
  114 |         (r) =>
  115 |           new URL(r.url()).pathname.endsWith(`/class-definitions/${classDefinitionId}`) &&
  116 |           r.request().method() === 'DELETE',
  117 |         { timeout: 15_000 },
  118 |       );
  119 |       await curriculaLocators.confirmDeleteButton(page).click();
  120 |       deleted = true; // set before awaiting - same reasoning as MEM-015
  121 |       const deleteRes = await deleteCall;
  122 |       expect(
  123 |         deleteRes.ok(),
  124 |         `DELETE .../class-definitions/:id was refused (${deleteRes.status()}) for a curriculum ` +
  125 |           'that was never used - it should have been eligible.',
  126 |       ).toBeTruthy();
  127 | 
  128 |       // Confirm it is a REAL delete, not merely hidden by an active-only
  129 |       // filter: the unfiltered list should not find it at all, archived or not.
  130 |       const stillThere = await findAnyStatus(ownerApi, clubId, name);
  131 |       expect(
  132 |         stillThere,
  133 |         'the deleted curriculum should not appear in the unfiltered list at all - ' +
  134 |           'a real delete removes the row, an archive would leave it findable with status=all.',
  135 |       ).toBeFalsy();
  136 |     } finally {
  137 |       // Belt and braces: if the UI half died before the delete landed,
  138 |       // clean up through the API so an empty scratch curriculum is never
  139 |       // left behind - unlike every OTHER curriculum fixture in this suite,
  140 |       // this one is genuinely safe to remove for real.
  141 |       if (classDefinitionId && !deleted) {
  142 |         await ownerApi
  143 |           .delete(`${env.apiURL}/clubs/${clubId}/class-definitions/${classDefinitionId}`, {
  144 |             headers: { 'x-app-id': env.appId },
  145 |           })
  146 |           .catch(() => undefined);
  147 |       }
  148 |     }
  149 |   });
  150 | 
  151 |   /**
  152 |    * The negative half - attempted against the SHARED `AUTOMATION_CURRICULUM_NAME`
  153 |    * fixture, which is safe precisely because a refused DELETE changes
  154 |    * nothing. Driven at the API level: the point is the server's own refusal,
  155 |    * not a UI journey, and there is no UI reason to expect this curriculum's
  156 |    * Delete button to behave differently from any other curriculum's - the
  157 |    * server-side check is what actually matters here.
  158 |    */
  159 |   test('OWP-032 - a curriculum with real history is refused, not silently deleted @OWP-032 @destructive', async ({
  160 |     ownerApi,
  161 |     clubId,
  162 |   }, testInfo) => {
  163 |     testInfo.annotations.push({
  164 |       type: 'manual-scenario',
  165 |       description: 'manual-qa/role-owner-primary.md#OWP-032 (step 4)',
  166 |     });
  167 | 
  168 |     const classDefinitionId = await ensureAutomationCurriculum(ownerApi, clubId);
  169 | 
  170 |     const deleteRes = await ownerApi.delete(
  171 |       `${env.apiURL}/clubs/${clubId}/class-definitions/${classDefinitionId}`,
  172 |       { headers: { 'x-app-id': env.appId } },
  173 |     );
  174 |     expect(
  175 |       deleteRes.status(),
  176 |       'deleting a curriculum with real classes/exams/enrollments against it should be ' +
  177 |         `refused with a 409 ("Archive it instead"), not silently accepted. Got ${deleteRes.status()}.`,
> 178 |     ).toBe(409);
      |       ^ Error: deleting a curriculum with real classes/exams/enrollments against it should be refused with a 409 ("Archive it instead"), not silently accepted. Got 404.
  179 | 
  180 |     const body = await deleteRes.json().catch(() => null);
  181 |     expect(
  182 |       body?.errors?.code ?? body?.error?.code,
  183 |       'the refusal should carry the documented error code 5010.',
  184 |     ).toBe(5010);
  185 | 
  186 |     // Confirm nothing was actually removed - the shared fixture every other
  187 |     // write test depends on must still exist afterward.
  188 |     const stillThereRes = await ownerApi.get(
  189 |       `${env.apiURL}/clubs/${clubId}/class-definitions/${classDefinitionId}`,
  190 |       { headers: { 'x-app-id': env.appId } },
  191 |     );
  192 |     expect(
  193 |       stillThereRes.ok(),
  194 |       'the shared AUTOMATION_CURRICULUM_NAME fixture should still exist after a refused delete.',
  195 |     ).toBeTruthy();
  196 |   });
  197 | });
  198 | 
```