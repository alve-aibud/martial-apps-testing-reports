# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: specs/smoke-testing/09-calendar-scheduling/sensei.teaching-filter.spec.ts >> SEN-011 - the staff teaching filter >> an unassigned class >> OWP-041 - an unassigned class is hidden from a sensei's Teaching view but still on the owner's calendar @OWP-041 @destructive
- Location: e2e/specs/smoke-testing/09-calendar-scheduling/sensei.teaching-filter.spec.ts:571:9

# Error details

```
Error: "[e2e] owp041-unassigned mtrhkuwr" has no teacher assigned, so a sensei's Teaching view (which defaults to the "Mine" filter) must NOT show it - it belongs to nobody. If this appears, the fallback removed in frontend commit 32de1b3e is back.

expect(locator).toBeHidden() failed

Locator:  getByText('[e2e] owp041-unassigned mtrhkuwr').filter({ visible: true }).first()
Expected: hidden
Received: visible
Timeout:  15000ms

Call log:
  - "[e2e] owp041-unassigned mtrhkuwr" has no teacher assigned, so a sensei's Teaching view (which defaults to the "Mine" filter) must NOT show it - it belongs to nobody. If this appears, the fallback removed in frontend commit 32de1b3e is back. with timeout 15000ms
  - waiting for getByText('[e2e] owp041-unassigned mtrhkuwr').filter({ visible: true }).first()
    34 × locator resolved to <div class="truncate text-[11px] text-tertiary leading-tight">[e2e] owp041-unassigned mtrhkuwr</div>
       - unexpected value "visible"

```

```yaml
- text: "[e2e] owp041-unassigned mtrhkuwr"
```

# Test source

```ts
  523 |    * Added 2026-09-05, after frontend commit `32de1b3e` deliberately stopped
  524 |    * showing unassigned classes to every teacher. The behaviour is worth
  525 |    * pinning because it is the only thing standing between a club and a class
  526 |    * nobody can see: if a class is created with no teacher, SOMEONE has to
  527 |    * still find it, and that someone is whoever is looking at "All".
  528 |    *
  529 |    * **THE OWNER DOES NOT SEE IT IN TEACHING MODE EITHER.** That was this
  530 |    * test's first guess and it is wrong; measured live 2026-09-05 rather than
  531 |    * reasoned about, after the first version failed:
  532 |    *
  533 |    *   Attending (the landing view) | label "Attending · 1 event" | VISIBLE
  534 |    *   Teaching                     | label "(none)"              | HIDDEN
  535 |    *
  536 |    * Two things in `ClubCalendar.tsx` conspire, and reading only the first one
  537 |    * gives the wrong answer:
  538 |    *
  539 |    *   1. `teachingFilter`'s `useState` INITIALIZER (line 192-195) picks
  540 |    *      `'all'` for owner roles and `'mine'` for everyone else - but it runs
  541 |    *      once, at first render, when `clubRole` is still `''` (the layout
  542 |    *      sets it only after `GET /users/clubs` resolves, `layout.tsx` line
  543 |    *      311). Nothing re-syncs it afterwards, so that role-based default
  544 |    *      never actually applies to anyone. Same shape as the hazard CLAUDE.md
  545 |    *      rule 6 describes.
  546 |    *   2. It would not matter anyway: the Teaching toggle's own onClick is
  547 |    *      `setTeachingMode('teaching'); setTeachingFilter('mine')` (line 1161,
  548 |    *      and again at 1651). **Clicking Teaching forces "mine" for every
  549 |    *      role, owner included.**
  550 |    *
  551 |    * So an unassigned class is hidden in Teaching mode from EVERYONE. What
  552 |    * keeps it findable is the ATTENDING view - which is where every role
  553 |    * lands, since `teachingMode` initialises to `'attending'` for all roles.
  554 |    * That is what this test pins: hidden from the sensei's Teaching view,
  555 |    * still present on the owner's calendar as they actually find it.
  556 |    *
  557 |    * Both halves in one test, on purpose: the sensei's "cannot see it" is an
  558 |    * absence check, and CLAUDE.md rule 3 wants a positive control proving the
  559 |    * same locator matches for a role that SHOULD have it. Two separate tests
  560 |    * could drift apart or race each other over the same scratch class.
  561 |    *
  562 |    * Recorded, not pinned red: the "widen to All teachers" escape hatch could
  563 |    * not be driven at this suite's 1280px viewport - the teacher-filter
  564 |    * trigger resolved to ZERO nodes in Teaching mode during the same
  565 |    * measurement. Whether that control is reachable at all for an owner is an
  566 |    * open question, written up in `COVERAGE.md` §3b rather than asserted here.
  567 |    */
  568 |   test.describe('an unassigned class', () => {
  569 |     test.use(asRole('sensei'));
  570 | 
  571 |     test('OWP-041 - an unassigned class is hidden from a sensei\'s Teaching view but still on the owner\'s calendar @OWP-041 @destructive', async ({
  572 |       page,
  573 |       browser,
  574 |       clubSection,
  575 |       sitesPage,
  576 |       clubId,
  577 |       ownerApi,
  578 |     }, testInfo) => {
  579 |       testInfo.annotations.push({
  580 |         type: 'manual-scenario',
  581 |         description: 'manual-qa/role-owner-primary.md#OWP-041',
  582 |       });
  583 | 
  584 |       await ensureAutomationCurriculum(ownerApi, clubId);
  585 | 
  586 |       const className = scratchName('owp041-unassigned');
  587 |       let siteId: string | undefined;
  588 |       let classId: string | undefined;
  589 | 
  590 |       try {
  591 |         siteId = await createScratchSite(ownerApi, clubId, scratchName('site-owp041'));
  592 |         const roomRes = await ownerApi.post(
  593 |           `${env.apiURL}/clubs/${clubId}/sites/${siteId}/rooms`,
  594 |           {
  595 |             headers: { 'x-app-id': env.appId },
  596 |             data: { name: scratchName('room'), capacity: 10, roomsize: '30', floor: '1' },
  597 |           },
  598 |         );
  599 |         const roomBody = await roomRes.json().catch(() => null);
  600 |         const roomId: string | undefined = roomBody?.data?.id ?? roomBody?.data?.roomId;
  601 | 
  602 |         // Created by the SENSEI, and left with no teacher - the whole point.
  603 |         // `createClass` assigns nobody, and `[e2e] Automation` carries no
  604 |         // `defaultSenseiIds` for the service to fall back on, so the class
  605 |         // genuinely belongs to no one.
  606 |         await sitesPage.openCreateClassForm(
  607 |           fillRoute('room', { clubId, siteId, roomId: roomId as string }),
  608 |           todayIsoUtc(),
  609 |           '11:00',
  610 |         );
  611 |         classId = await sitesPage.createClass(className, AUTOMATION_CURRICULUM_NAME);
  612 | 
  613 |         // --- the sensei's own default view: Teaching + "Mine" ---
  614 |         await clubSection('calendar').gotoAndAwaitClubRole(clubRoute(clubId, 'calendar'));
  615 |         await calendarLocators.teachingToggle(page).click();
  616 | 
  617 |         await expect(
  618 |           calendarLocators.eventChip(page, className),
  619 |           `"${className}" has no teacher assigned, so a sensei's Teaching view ` +
  620 |             `(which defaults to the "Mine" filter) must NOT show it - it ` +
  621 |             `belongs to nobody. If this appears, the fallback removed in ` +
  622 |             `frontend commit 32de1b3e is back.`,
> 623 |         ).toBeHidden({ timeout: 15_000 });
      |           ^ Error: "[e2e] owp041-unassigned mtrhkuwr" has no teacher assigned, so a sensei's Teaching view (which defaults to the "Mine" filter) must NOT show it - it belongs to nobody. If this appears, the fallback removed in frontend commit 32de1b3e is back.
  624 | 
  625 |         /**
  626 |          * --- the positive control: the OWNER's calendar as they land on it ---
  627 |          *
  628 |          * A whole separate browser context rather than a second spec file, so
  629 |          * both roles look at the SAME scratch class in the same run - the two
  630 |          * halves cannot disagree about what was on the calendar.
  631 |          *
  632 |          * NOT switched to Teaching, deliberately - see this describe's header.
  633 |          * Every role lands on Attending, and that is the view in which an
  634 |          * unassigned class is still reachable. Clicking Teaching here would
  635 |          * hide it for the owner too, and asserting THAT would just re-prove
  636 |          * the sensei half against a second account.
  637 |          */
  638 |         const ownerCtx = await browser.newContext({
  639 |           storageState: env.authFile('ownerPrimary'),
  640 |         });
  641 |         try {
  642 |           const ownerPage = await ownerCtx.newPage();
  643 |           const ownerCalendar = new ClubSectionPage(ownerPage, 'calendar');
  644 |           await ownerCalendar.gotoAndAwaitClubRole(clubRoute(clubId, 'calendar'));
  645 | 
  646 |           await expect(
  647 |             calendarLocators.eventChip(ownerPage, className),
  648 |             `the owner lands on the Attending view, and an unassigned class ` +
  649 |               `MUST still be visible there - it is the only view that shows ` +
  650 |               `it at all. If this is hidden, a class belonging to nobody is ` +
  651 |               `visible to nobody and can never be found to assign or clean up.`,
  652 |           ).toBeVisible({ timeout: 15_000 });
  653 |         } finally {
  654 |           await ownerCtx.close();
  655 |         }
  656 |       } finally {
  657 |         if (classId) {
  658 |           await deleteScratchClass(ownerApi, clubId, classId);
  659 |         }
  660 |         if (siteId) {
  661 |           await deleteScratchSite(ownerApi, clubId, siteId);
  662 |         }
  663 |       }
  664 |     });
  665 |   });
  666 | 
  667 |   test.describe('as a plain member', () => {
  668 |     test.use(asRole('member'));
  669 | 
  670 |     /**
  671 |      * The sheet's edge case, and the negative half of the pair above. A plain
  672 |      * member's calendar is attendance-only, so neither side of the toggle
  673 |      * should exist for them.
  674 |      */
  675 |     test('SEN-011 - a plain member is not offered the filter at all @SEN-011', async ({
  676 |       page,
  677 |       clubSection,
  678 |       clubId,
  679 |     }, testInfo) => {
  680 |       testInfo.annotations.push({
  681 |         type: 'manual-scenario',
  682 |         description: 'manual-qa/role-sensei.md#SEN-011 (edge case: member sees no toggle)',
  683 |       });
  684 | 
  685 |       await clubSection('calendar').gotoAndAwaitClubRole(clubRoute(clubId, 'calendar'));
  686 | 
  687 |       await expect(
  688 |         calendarLocators.teachingToggle(page),
  689 |         'a plain member should never see the Teaching filter - it is wrapped in ' +
  690 |           'isStaffRole (ClubCalendar.tsx line 1063) and their calendar is ' +
  691 |           'attendance-only.',
  692 |       ).toBeHidden({ timeout: 15_000 });
  693 | 
  694 |       await expect(
  695 |         calendarLocators.attendingToggle(page),
  696 |         'nor the Attending side - the toggle is one control and both halves are ' +
  697 |           'gated together.',
  698 |       ).toBeHidden({ timeout: 15_000 });
  699 |     });
  700 |   });
  701 | });
  702 | 
  703 | /**
  704 |  * NOT AUTOMATED from this scenario.
  705 |  *
  706 |  * **Steps 1, 3 and 4 are all covered now**, by the third and fourth tests
  707 |  * above - the "Teaching · N events" label, the name-derived teacher
  708 |  * dropdown, and the staff "Add Students" enrolment updating the calendar
  709 |  * without a reload (see that test's own header for the full derivation,
  710 |  * including why the earlier "a sensei has no way to enrol at all" note was
  711 |  * wrong - it had only checked the plain self-enrol button, not the
  712 |  * staff-side "Add" modal).
  713 |  *
  714 |  * The sheet's "the list is SCOPED for a sensei, not the full club roster" is
  715 |  * the one piece still unasserted: proving a list is narrower than the
  716 |  * roster needs a known expected subset, and the sheet does not say what it
  717 |  * should contain.
  718 |  */
  719 | 
```