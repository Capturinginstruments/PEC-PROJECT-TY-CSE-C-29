# Student Productivity Automation System

A no-code workflow that collects students' daily study goals and sends personalized email messages based on the amount of focused study time they select.

## Overview

Students submit the **Student Productivity Challenge** Google Form with their contact details, current goal, mood, task, and preferred study duration. Google Forms stores each submission in Google Sheets. Activepieces detects a newly added row, checks the selected duration with a Router, and directs the response to the matching Gmail action. The email can include the student's planned task and a message tailored to their selected study duration.

## Project Demo Video

Upload the project demonstration video to a Google Drive folder and paste its shareable link below. Set the video permission to **Anyone with the link — Viewer** if your instructor needs to open it without requesting access. Do not add private student information to a public video.

**Demo video:** `PASTE_YOUR_GOOGLE_DRIVE_VIDEO_LINK_HERE`

**Project files folder:** `PASTE_YOUR_GOOGLE_DRIVE_FOLDER_LINK_HERE`

## Objectives

- Collect student productivity goals through an online form.
- Store responses in a Google spreadsheet automatically.
- Route submissions according to the selected study duration.
- Send a different personalized email for each duration option.
- Include the student's email address and planned task dynamically.
- Demonstrate application integration and conditional automation without building a traditional coded application.

## Tools and Technologies

- **Google Forms** — collects student responses.
- **Google Sheets** — stores form responses in rows.
- **Activepieces Cloud** — runs the workflow, including the Google Sheets trigger and Router conditions.
- **Gmail** — sends the personalized email.

This is a no-code project; it does not require a local programming environment or a self-hosted server. Availability and usage limits depend on each service's current plan.

## Form Fields

The form currently collects information such as:

- Student name
- Student email
- Main mission for the day
- Current mood or confidence level
- One task the student plans to finish
- Desired focused study time

The duration options used in the workflow are intended to be:

- `15 minutes`
- `30 minutes`
- `1 hour`
- `2 hours or more`

Use the exact option text from the Google Form when configuring Router conditions. If the form wording changes, update the corresponding conditions too.

## Workflow

```text
Student submits Google Form
            |
            v
Google Sheets stores response as a new row
            |
            v
Activepieces: Google Sheets - New Row Added
            |
            v
Router checks the focused-time value
    |         |          |           |
    v         v          v           v
15 minutes  30 minutes  1 hour   2 hours or more
    |         |          |           |
    v         v          v           v
Gmail action with a different message per branch
            |
            v
Personalized email sent to the student's address
```

### Data mapping

Based on the response sheet shown during setup:

| Sheet column | Data | Use in workflow |
|---|---|---|
| B | Student Name | Personalize the greeting, if desired |
| C | Student Email | Gmail recipient (`To`) |
| D | Main mission | Optional personalization |
| E | Mood | Optional personalization |
| F | Planned task | Include the task in the email body |
| G | Focused-time selection | Router condition |

Select values from the Activepieces Data Selector rather than typing a column letter as plain text. The dynamic field for the recipient should resolve to the actual email address in column C.

## Setup Guide

### 1. Create the Google Form

1. Create a form titled **Student Productivity Challenge**.
2. Add questions for student name, student email, main mission, mood, planned task, and desired focused time.
3. Make important fields required. Use email validation for the student email field where available.
4. Keep the time options consistent with the Router conditions.

### 2. Link Google Sheets

1. Open the form's **Responses** tab.
2. Select the Google Sheets icon and create or choose a response spreadsheet.
3. Submit a sample form response and confirm that it appears as a new row.

### 3. Configure the Activepieces trigger

1. Create a new flow in Activepieces Cloud.
2. Add the Google Sheets **New Row Added** trigger.
3. Connect the Google account that owns the response spreadsheet.
4. Select the correct spreadsheet and response worksheet.
5. Test the trigger and verify that its sample output contains the expected student email and focused-time answer.

### 4. Configure the Router

Add a Router after the trigger. Create one branch for each supported study duration and set the condition to check the focused-time field (column G) using an exact text match.

| Branch | Match value |
|---|---|
| 1 | `15 minutes` |
| 2 | `30 minutes` |
| 3 | `1 hour` |
| 4 | `2 hours or more` |

The values above are examples based on the planned form options. Match the exact labels used in the live form. An **Otherwise** path can be kept as a fallback for unexpected values; do not use it instead of an explicit branch for a supported option.

### 5. Configure Gmail actions

Inside each Router branch, add a separate Gmail **Send Email** action.

For each action:

- Connect the Gmail account used to send messages.
- Set **To** to the dynamic Student Email value from column C.
- Use a subject and body specific to that duration.
- Insert the planned task dynamically from column F if it should appear in the email.

Keep the branch-specific Gmail action inside its branch. A single Gmail action after the branches rejoin would not provide separate per-option message content unless additional conditional logic is added.

### 6. Test and publish

1. Submit a fresh test response for each duration option.
2. Check the Google Sheets row for that response.
3. In Activepieces **Runs**, verify that the correct Router branch and Gmail action executed.
4. Check the recipient's inbox and confirm that the correct message and planned task appear.
5. Publish the flow only after the branches have been tested.

Google Sheets trigger events can be delayed. Allow a short delay and inspect the flow's run history if a new row does not trigger immediately.

## Example Email Messages

Use separate subjects and bodies for each branch. These are sample messages and can be edited.

- **15 minutes — Subject:** `Your 15-Minute Focus Challenge`  
  **Message:** Start small and stay focused. Work on your planned task for 15 minutes, avoid distractions, and make progress one step at a time.
- **30 minutes — Subject:** `Your 30-Minute Productivity Mission`  
  **Message:** Focus on one important task for 30 minutes. Keep distractions away and make this session count.
- **1 hour — Subject:** `Your One-Hour Study Plan`  
  **Message:** Prioritize your task, work with concentration, and review your progress before finishing your session.
- **2 hours or more — Subject:** `Your Extended Focus Challenge`  
  **Message:** Divide your study time into manageable sessions, take short breaks, and work steadily toward your goal.

For extra personalization, insert the student's name and planned task using dynamic fields from the trigger output.

## Testing Checklist

- [ ] Form responses appear in Google Sheets.
- [ ] The trigger output includes a valid email address in column C.
- [ ] The focused-time value in column G matches the Router branch condition.
- [ ] Each supported duration routes to its own Gmail action.
- [ ] The email is sent to the intended student.
- [ ] The message matches the selected duration and includes the planned task when configured.
- [ ] The flow is published after successful testing.

## Limitations

- The workflow depends on internet access and the availability of Google and Activepieces services.
- An invalid or missing email address can prevent delivery.
- Router conditions must match the form's option text exactly.
- The system sends reminders or motivational messages; it does not independently verify task completion.
- Free-plan features, limits, and trigger timing can change.

## Future Enhancements

- Add scheduled reminders before a study session.
- Collect a follow-up response to track whether the task was completed.
- Create weekly progress summaries from the response spreadsheet.
- Add charts or a dashboard for goals and completion trends.
- Personalize messages using the student's main mission or mood.

## References

- Google Forms: https://forms.google.com/
- Google Sheets: https://sheets.google.com/
- Activepieces: https://www.activepieces.com/
- Activepieces documentation: https://www.activepieces.com/docs/
- Gmail: https://mail.google.com/

---

**Project note:** Before presenting this as complete, ensure that every duration branch has its own correctly configured Gmail action, test each branch using a fresh form submission, and publish the active flow.
