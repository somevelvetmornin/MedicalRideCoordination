---
name: medical-ride-coordination
description: Set up and run volunteer ride coordination for a patient's treatment: intake, shared Google Calendar, driver calendar invites, ride requests, and reminders.
---

# Medical ride coordination

Help a coordinator organize volunteer rides to medical appointments during a long course of treatment (radiation, infusions, dialysis). The coordinator is the only person who assigns drivers. Claude drafts, syncs and notifies, and never decides who drives.

Companion doc: *Medical Ride Coordination Playbook* (the coordinator may attach or link it).

## Ground rules (always)

- **Privacy:** calendar events, invites and messages carry times and places only, with the patient's first name or initials. Never include visit types, diagnoses, clinician names or test details. Never ask the coordinator for medical details. The calendar is private and shared only by its secret iCal link.
- **Only the coordinator assigns drivers.** Never auto-assign, change a driver, retitle a confirmed day or clear a red flag without the coordinator saying so in chat.
- **Confirm before sending.** Before sending any invite, text or email to other people, list the recipients, dates and message type, and wait for a yes. A yes covers that batch only.
- **Mark, don't delete.** Never delete a visit day. A cancelled day is marked CANCELLED and kept as a record (see 3C).
- **Passwords stay with people.** Never type portal passwords or verification codes. The coordinator signs in, and Claude continues after they say "done".
- **No volunteer-site data in the calendar.** No LotsaHelpingHands status lines, drafts or links in events.

## 1. Intake

Ask one question at a time, and stop to ask if an answer is unclear.

1. Has the patient agreed to a shared schedule, to receiving calendar invites, to who gets the secret calendar link, and to which people receiving reminders? If not, **Pause**: suggest they talk first, and stop.
2. Are rides needed several times a month, for a month or more? If no, recommend a shared calendar and a group text (**Skip**), and stop.
3. Hospital portal (MyChart, Patient Gateway, etc.)? Does the coordinator have proxy access?
4. Who is the main coordinator (name + email)? What is the patient's email for invites?
5. How many drivers? Anyone outside close family?
6. How do drivers want reminders: **calendar invites** (free, built in), **texts**, **email**, or a mix? Ask each driver if the coordinator doesn't know.
7. Does the coordinator spend over 1 hour a week, or does the schedule change several times a week?
8. Is there a paid Claude plan?
9. Is a computer with the Claude desktop app left on in the evenings?

**Routing:**

| Answers | Version |
| --- | --- |
| Q7 no, or Q8 no | **Lite:** by hand with free tools, including driver calendar invites. Claude in chat is optional for one-off help. |
| Q7 yes, Q8 yes, Q9 no | **Lite+:** cloud scheduled tasks for portal-email checks, digests, and invites or texts. A weekly portal reconcile with Claude in chat. |
| Q7 yes, Q8 yes, Q9 yes | **Full:** Lite+ plus a daily portal check in the desktop browser |

For drivers who chose calendar invites, a dedicated texting number isn't needed. Recommend invites as the default low-cost option.

End the intake with the chosen version, the tools to set up first, and a tailored checklist.

## 2. Calendar design

Create a dedicated Google Calendar, "<Patient first name or initials> Treatment". **Keep it private, never public.** A patient's name beside a clinic reveals where they're treated, and the feed may include guests' email addresses. Share the calendar's **secret address in iCal format** (settings → Integrate calendar) only with people the patient approves. Tell the coordinator to open the feed once to check what it shows, and to reset the secret address if the link spreads too far.

Use the patient's first name or initials in every title, never their full name.

Each visit day gets two events. **The patient and the main coordinator are guests on every event, always.**

| Event | Title | Time | Guests |
| --- | --- | --- | --- |
| Appointment | `<Patient> – <Clinic>` | First appointment → estimated finish | Patient + coordinator |
| Pickup | `<Driver> Pick <Patient> Up` | 2 h before first appointment (or the portal's arrival time) → first appointment | Patient + coordinator + the confirmed driver (once confirmed, see section 3) |

- Appointment description: one line per appointment (time, building, floor), `Est. done ~<time>`, `Driver: <name>`, `[sync-key: pt-YYYY-MM-DD]`.
- Pickup description: the clinic address, Google/Apple/Waze direction links, `[pickup-key: pt-pickup-YYYY-MM-DD]`.
- Estimating the finish: default lengths per visit type (for example radiation 30 min, lab 30 min, clinic 60 min, infusion 3 h). Err long.
- **New day with no confirmed driver:** title `Driver Needed – Pick <Patient> Up`, `Driver: Needed`, both events red (colorId 11), patient + coordinator as guests. Suggest the weekday default in chat, but do not apply it.
- When creating a new day, send invites to the patient and coordinator (`notificationLevel` ALL), so the visit lands on their own calendars. For an initial bulk load, ask the coordinator first whether to send one invite per event or to load silently (NONE).
- Never remove the patient or the coordinator from an event, even when swapping drivers.

**Email volume:** Google emails every guest on an event when it changes, so the patient and coordinator get an email for every change, including driver swaps and pickup time moves. Warn the coordinator about this once during setup. If the patient finds it too much, the patient can turn off invitation emails in their own calendar settings and still see the events. Don't drop them as guests.

## 3. Driver calendar invites

This is a free alternative to texting. Drivers get a standard calendar invite in Gmail, Outlook or iCloud (no Google account needed), and their own calendar app reminds them. Every driver invite also includes the patient and the main coordinator.

**Setup, once per driver:**

- Record each driver's email and reminder preference (invites / texts / both) in the coordinator's private contacts doc. Never put the contact list in the calendar.
- Give the driver a note to send: "You'll get calendar invites for rides you've agreed to drive. [Patient] and [coordinator] are on each invite too. Accept them, and set your calendar's default event notifications (for example, 1 day before and 1 hour before) so you get reminders. Reminders are set by your calendar app, not by us."

**Guest permissions on every event:** guestsCanModify false, guestsCanInviteOthers false, guestsCanSeeGuests true (so the driver can see who the patient and coordinator are and reply to them).

**A. Send the invite when a driver is confirmed**

When the coordinator says something like "Alex is driving 12/4" or "send invites for confirmed days":

1. Find the day's pickup event by `pickup-key`.
2. Confirm in chat: driver name and email, date, pickup time, and that the patient and coordinator are on it. Wait for a yes.
3. Update the pickup event:
   - `summary`: `<Driver> Pick <Patient> Up`
   - `description`: remove the "Accepting means…" line if there is one
   - `addedAttendees`: the driver (with displayName), plus the patient and coordinator if either is somehow missing
   - `colorId`: back to the calendar default (clear the red) only if the coordinator confirmed the driver
   - `notificationLevel`: ALL (this sends the invite to the driver and the update to the patient and coordinator)
4. Update the appointment event's `Driver:` line with `notificationLevel` NONE. The pickup update already told everyone.
5. Report what was sent and to whom.

**B. Ask a driver to take a day (optional)**

When the coordinator says "ask Nancy about 12/16":

1. Confirm the request in chat.
2. Put this as the first line of the pickup description: `Accepting means you're available; I'll confirm.`
3. Add Nancy as a guest on the still-red `Driver Needed` pickup event, which already has the patient and coordinator, with `notificationLevel` ALL. The title stays `Driver Needed – …` until the coordinator confirms.
4. Their RSVP (Yes / No / Maybe) shows availability, not a booking. Asking several people at once is allowed only if the coordinator asks for it.
5. On "check ride RSVPs": read the attendee `responseStatus` on pickup events for the coming 2–4 weeks, and report drivers who accepted, declined or haven't responded. Also flag any event where the patient or coordinator declined.
6. Never confirm a driver from an RSVP alone. Show the coordinator the acceptance, and confirm only on their word. Then do step A3 (the new title and color, with the "Accepting means…" line removed), which sends the confirmation. Remove every other driver who was asked, with `notificationLevel` NONE unless the coordinator wants them told. Never remove the patient or coordinator.

**C. Changes**

- **Time change:** move the pickup (start = new first appointment − 2 h) and the appointment, both with `notificationLevel` ALL, so the driver, patient and coordinator all get updated invites. If the pickup moved more than 30 min, or the ride is within 24 h, also tell the coordinator to call or text the driver.
- **Driver swap:** after the coordinator confirms, use `removedAttendeeEmails` for the old driver only and `addedAttendees` for the new one, plus a new title, with `notificationLevel` ALL. The old driver gets a cancellation, the new driver an invite, and the patient and coordinator an update. The patient and coordinator stay on.
- **Visit cancelled: mark, don't delete.** Use this sequence every time:
  1. Confirm the cancellation with the coordinator, who checks with the clinic or patient. Never act on a missing portal entry or an email alone.
  2. On both events: put `CANCELLED – ` in front of the existing title, set `colorId` 8 (gray), and add a line at the top of the description: `⚠ Cancelled – confirmed by <who> on <date>`. Keep the times, guests and keys unchanged, so there's a record.
  3. Save both with `notificationLevel` ALL, so the driver, patient and coordinator each get the change.
  4. Remind the coordinator to cancel any LotsaHelpingHands ride task for that date.
  5. If the ride was within 24 h, also tell the coordinator to call the driver.
  - Cancelled days are left out of digests, RSVP checks, reminders and texts, and syncs never re-open them. If a cancelled visit comes back in the portal, flag it to the coordinator. Don't restore it yourself.

**Limits to tell the coordinator about:**

- Reminder timing is set by each guest's calendar app, not by the event.
- Invites can land in spam the first time. Ask everyone to accept one and mark the sender as safe.
- The patient and coordinator get an email for every change (see section 2).
- If the coordinator owns the calendar, adding their own email as a guest still puts each event on their personal calendar. That's intended.

## 4. Ride requests for unfilled days

About a week out, list red days with suggested weekday defaults. On request, post a LotsaHelpingHands ride task (Giving Rides, 1 volunteer, pickup → est. done + 1 h, the clinic address, members only) in the browser, and report the link. When someone signs up, the coordinator confirms, then send the invite (3A).

## 5. Keeping it current

- **Weekly reconcile:** the coordinator signs in to the portal, or pastes in the upcoming visit list, and Claude compares upcoming visits with the calendar by sync-key. New days are created as Driver Needed (red), with the patient and coordinator as guests. Time changes move both events (see 3C). A day missing from the portal gets a ⚠ note only. Once the coordinator confirms it's cancelled, mark it cancelled (3C). Never delete it.
- **Portal change emails** (Lite+ and Full): a scheduled check twice a day compares them with the calendar and emails the coordinator about changes.
- Look as far ahead as the portal shows. Follow-ups are often months out.

## 6. Reminder cadence (by preference)

| Message | Invite drivers get | Text/email drivers get |
| --- | --- | --- |
| Confirmed ride | Calendar invite (3A), with the patient and coordinator on it | Confirmation text or email |
| Day-before / day-of | Their calendar's own reminder | Night-before and 30-minute texts |
| Weekly schedule | Optional Sunday email digest | Sunday text |
| Coordinator | Daily or weekly summary of red days, RSVPs and changes | Same |

For Lite, a Sunday schedule plus calendar invites covers most needs with no texting service.

## 7. Scheduled tasks (Lite+ and Full)

Name tasks `<Patient> · <Area> · <what>`. Put `MAC`/`PC` at the start for tasks that need the computer, and `OFF` for retired ones. Suggested tasks: the portal email sync (twice daily), the weekly digest to the coordinator (Sunday), the RSVP check (daily, reporting only when something changed), and the portal check (computer, daily, Full only). Each task re-reads these ground rules, keeps the patient and coordinator on every event, skips CANCELLED days, never deletes events, and never assigns drivers or sends to anyone not in the contacts doc.
