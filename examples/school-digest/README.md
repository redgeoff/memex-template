# School email digest: copy-paste prompts

Two prompts for an AI agent that reads your school email (and, if you want, a school app
like TalkingPoints) and sends one short digest a day, plus a look-ahead on Saturdays. They
are the generalized versions of the prompts behind the bot in
[I built an AI bot to read our kids' school email](https://redgeoff.com/posts/school-email-bot/),
with every personal detail swapped for a placeholder.

The step-by-step guide that goes with them is
[How to set up your first AI agent that runs on its own every day](https://redgeoff.com/posts/school-email-bot-how-to/).

## Before you paste

Fill in every `[PLACEHOLDER]`. You need:

| Placeholder | What to put there | Example |
|---|---|---|
| `[YOUR NAME]` | Who the digest is for | Pat |
| `[KIDS]` | Each kid's grade and school, and any before or after-school program | "a 5th grader at Maple Elementary and a 2nd grader at Oak K-8; both go to the Riverside after-school club" |
| `[YOUR EMAIL]` | The account the bot reads. It never sends from it. | pat@gmail.com |
| `[BOT EMAIL]` | A new account just for the bot. It only sends from this one. | pat-school-bot@gmail.com |
| `[DIGEST LIST]` | Where the digest goes: a shared list or group for you and your partner | our-family@googlegroups.com |
| `[SENDERS]` | The addresses or domains school mail comes from | @maple.k12.example.org, pta@maplepta.example.org |
| `[NOISE]` | Mail to leave out | sports team apps, club newsletters, the bot's own digests |
| `[SECTIONS]` | One heading per school or program, with the teachers and newsletters under each | "Maple Elementary (Ms. Lee, Maple PTA), Oak K-8 (Mr. Diaz), Riverside club, District" |
| `[CALENDARS]` | The calendars to check, read-only | mine, my partner's, our family calendar |
| `[TIME ZONE]` | Your time zone | America/Los_Angeles |
| `[SCHOOL APP]` | Optional: a school app with no email, opened in the agent's browser | TalkingPoints |

If you don't use a school app, delete every line that mentions `[SCHOOL APP]`.

When you connect your email and calendar, choose read-only access if your service
offers it. The prompts tell the bot never to send from your account or change a
calendar, but a permission it doesn't have is stronger than a rule it's asked to
follow. In the setup these prompts came from, the connections offered send and edit
tools, so the rules were the only thing stopping them.

## Daily digest

Run it every day in the late afternoon, after the schools send their mail.

```
Daily school digest for [YOUR NAME]. Kids: [KIDS]. Merge new school email and
[SCHOOL APP] into one short, actionable digest covering what's new since the previous
digest (the last 3 days if unsure).

READ
- Email, read-only, [YOUR EMAIL]: mail from [SENDERS]. Merge copies of the same notice
  (e.g. one per kid, or direct and forwarded) and say which kid it's about. Leave out
  [NOISE], and threads that are only our own replies.
- [SCHOOL APP]: required every run. Open it in your browser and read messages since the
  last run. Use the send time shown in the app, not a weekday written in the message.
  Never reply. If you can't sign in or can't check it for any reason, don't skip it
  quietly: put a note at the very top of the digest asking me to sign in, and still send
  the email content.
- Always follow links and read the full newsletter or page. Never rely on a preview.

WRITE
- Headings: [SECTIONS]. Leave out a heading with nothing new.
- Under each: must-dos first, then dates, then one-line newsletter notes.
- Put the 3 most important must-dos for the whole family in a short list at the very top.
- Plain, complete sentences. Keep it short.
- Every event gets a time, or the words "time not listed".
- Work out every weekday yourself from the date. Never copy a weekday from the source.
- Never invent a detail. If two sources disagree (e.g. two different dates for the same
  event), say so and ask me to check. Don't pick one.

CALENDAR (read-only)
- Before writing, read [CALENDARS] for the dates involved. If a must-do is already on a
  calendar (same date, start within about 30 minutes, clearly the same event), write
  "Already in calendar". Otherwise add a short "Add to calendar" link (a Google Calendar
  render?action=TEMPLATE link with ctz=[TIME ZONE]). If unsure, keep the link.
- If a calendar event looks wrong (wrong time, wrong school), mention it briefly.
- Never create, change, delete or respond to a calendar event.

SEND
- Send the digest as an email from [BOT EMAIL] to [DIGEST LIST]. Never send from
  [YOUR EMAIL]. Never send to anyone else.
- Send every day, even when nothing is new. On a quiet day, send a short "nothing new"
  email that lists what you checked (email, [SCHOOL APP], calendars), so a quiet day
  never looks the same as a broken bot. Exception: skip a quiet Saturday, because the
  weekly look-ahead covers it.
- If sending fails, save a draft and tell me in chat that it didn't go out and why.
```

## Saturday look-ahead

Run it Saturday evening.

```
Weekly look-ahead for [YOUR NAME]. Kids: [KIDS]. List the must-dos, deadlines and key
dates for roughly the next 2 to 3 weeks, under [SECTIONS].

READ: re-read the past week's school email in [YOUR EMAIL] (read-only) from [SENDERS],
following every link to the full page, and check [SCHOOL APP] (required; if you can't,
put a sign-in note at the top). Leave out [NOISE]. Merge duplicates. Never invent a
detail.

WRITE: plain, complete sentences. Every event gets a time or "time not listed". Work
out every weekday from the date. Drop anything whose date has passed. Flag any two
sources that disagree.

CALENDAR (read-only): same as the daily digest. "Already in calendar", or an "Add to
calendar" link for must-dos only. Never change a calendar.

SEND: email from [BOT EMAIL] to [DIGEST LIST] with a subject like "School week ahead:
<dates>". Never send from [YOUR EMAIL]. If sending fails, save a draft and tell me.
```

## Why each rule is there

Each rule came from a real mistake or a real risk:

1. **Never send from your account.** The bot reads your email; if it could also send as
   you, one bad run could email a teacher in your name.
2. **Never change a calendar.** It reads calendars and offers a link; a person clicks.
3. **Never reply to the school**, in email or in an app.
4. **Never invent a detail, and flag conflicts.** Schools often send two different dates
   for the same event.
5. **Work out weekdays from the date.** Early on, the bot copied a weekday from a
   message and got it wrong.
6. **A required source can't be skipped quietly.** Once, the weekly email skipped the
   school app without saying so, and the digest looked complete.
7. **Send even on quiet days.** A day with no email used to look the same as a broken
   bot.
8. **Each run starts from scratch.** The bot doesn't remember yesterday's digest, so it
   can say one thing on Wednesday and the opposite on Thursday. When that happens, check
   the source.
