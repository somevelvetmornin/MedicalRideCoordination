# Medical Ride Coordination

A playbook and a Claude skill for coordinating volunteer rides to medical appointments during a long course of treatment, such as daily radiation, infusions or dialysis.

One coordinator keeps a single private calendar of visits and rides. Volunteers fill driver slots, and everyone gets reminders without asking "what time tomorrow?" It was first used for a patient with several weeks of daily treatment at a major cancer center.

## What's here

| Path | What it is | For |
| --- | --- | --- |
| [`playbook.md`](playbook.md) | The full method: intake questions, calendar design, driver rules, calendar invites, cancellations, privacy, setup checklists, lessons learned | Anyone; no Claude needed |
| [`skills/medical-ride-coordination/`](skills/medical-ride-coordination/SKILL.md) | A Claude skill that runs the intake, builds the calendar, sends driver invites and keeps things in sync | Claude users |

## Which version do I need?

Start with the [intake questions](playbook.md#start-here-intake-questions). In short:

- **Lite:** by hand with free tools (Google Calendar, LotsaHelpingHands, calendar invites). Most families land here.
- **Lite+:** a paid Claude plan runs scheduled checks, digests and invites in the cloud.
- **Full:** Lite+ plus a daily patient-portal check from a computer running the Claude desktop app.

## Use the playbook

Read [`playbook.md`](playbook.md) and follow the Lite checklist under **Setup**. To have Claude walk you through it, paste the intake prompt from the playbook into a new Claude chat with the playbook attached.

## Install the Claude skill

1. Download this repo (**Code → Download ZIP**), or the skill zip from **Releases** if one is published.
2. Zip the `skills/medical-ride-coordination` folder, so the zip contains `medical-ride-coordination/SKILL.md`.
3. In Claude, open **Customize > Skills**, click **+**, choose **Create skill**, then **Upload a skill**, and select the zip.

Skills require code execution to be turned on. See [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude).

## Privacy ground rules

- Get the patient's agreement before sharing a schedule or sending reminders.
- Keep the calendar **private**, and share only its secret iCal link.
- Calendar events, invites and texts carry **times and places only**: no visit types, diagnoses or clinician names.
- Never store or type the patient's portal password; a person signs in.
- Never commit names, phone numbers, emails or calendar IDs to a fork of this repo.

## Contributing

Issues and pull requests are welcome, especially lessons from your own ride team. Please strip all personal and medical details first.

## License

[CC BY 4.0](LICENSE). You may share and adapt this material, including commercially, as long as you give credit to Sarah.

This is practical guidance from a family caregiver's experience, not medical or legal advice.
