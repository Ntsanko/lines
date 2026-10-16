# BB's Lines

Everything BB says on the desktop comes from the Line Packs in this repo. BB checks here when she starts and once a day, and downloads any pack that's new or has changed. Nobody needs an app update for her to get new material.

This repo is public on purpose. BB reads it anonymously, so the app ships with no password or token. That's safe because a Line Pack is only text and conditions. It never holds code, and never anything about anyone's PC.

## Adding Lines

1. Fork this repo, or make a branch if you have access.
2. Add your Lines to a pack in `packs/`, or start a new pack file there. If you start a new file:
   - Add its path to `manifest.json`.
   - Give the pack a `name` no other pack uses; a good habit is to match the file, like `packs/friends.json` for `"name": "friends"`.
   - Use only letters, digits, `-` and `_` in the path, ending in `.json`, like `packs/rins-lines.json`. BB ignores any other path.
   - Keep each pack under 1 MB, and the repo to at most 50 packs.
3. Open a pull request. Once it's merged, everyone's BB picks it up within a day, or the next time they start her.

If a pack has a mistake in it, BB skips the whole pack. She keeps using the last good copy she downloaded, so nothing breaks. Your Lines just won't show up until the mistake is fixed. Check the rules below before opening the PR.

Typical mistakes:
- A misspelled field: `"programm"` instead of `"program"`. BB treats any field she doesn't know as a mistake, so a typo can never quietly make a Line apply everywhere.
- An unknown trigger, kind or blank.
- The same `id` twice, or a Line with no `id`.

## A Line

```json
{ "id": "poked-again", "trigger": "poked", "text": "Poke me again and see what happens~" }
```

Every Line needs these:

- **`id`**: unique within its pack. BB uses it to remember when she last said the Line.
- **`trigger`**: the moment she says it (see below).
- **`text`**: what she says.

Everything else is optional and narrows down when she may say it:

| Field | Meaning |
|---|---|
| `program` | Only while Senpai is using this program, by process name: `chrome`, `discord`, `notepad`. Not "Google Chrome". |
| `minStage` | Only once BB has reached this Stage: `curious`, `smug`, `possessive` or `yandere`. On a `stageChanged` Line, it announces exactly that Stage. |
| `habitKind` | Only once BB has noticed this Habit: `nightOwl`, `marathon`, `compulsiveSwitching`, `dailyRitual` or `neglect`. |
| `secretKind` | Only once BB knows a Secret of this kind: `duplicateHoard` or `shameGame`. |
| `about` | With a `habitKind` or `secretKind`: only about this particular program or game, like `"about": "excel"`. |
| `timeOfDay` | Only at this time of day, 24-hour, and it may run past midnight: `{ "from": "22:00", "to": "04:00" }`. |
| `cooldownMinutes` | How long before she may say it again. |
| `voicesSecret` | `true` if saying it gives away something BB found out. She never says those in Safe Mode, while Senpai might be streaming. |

When several Lines fit a moment, BB picks one of the most specific: a Line for `discord` beats a general one.

### Triggers

**Things Senpai does to her:**
- `poked`
- `petted`
- `thrown`
- `returnedFromDismissal`: Senpai sent her away, and she's back.

**Things she notices:**
- `programSwitched`
- `lingering`: Senpai has been in one program for a long while.
- `timeOfDay`: on the hour.
- `returnedFromIdle`: Senpai is back at the PC.
- `ambient`: idle chatter.

**Things she learns:**
- `habitLearned`
- `secretHuntStarted`
- `secretFound`
- `secretHuntFruitless`
- `stageChanged`
- `forgotten`: Senpai made her forget everything.

**Her Prank taunts:**
- `xButtonMenace`
- `peek`
- `cursorSit`
- `windowShove`
- `fakeCrack`
- `earthquake`

### Blanks

A Line's text can have blanks in curly brackets, filled in when she says it. A Line whose blanks can't be filled at that moment isn't said.

**What's going on:**
- `{program}`: the program Senpai is using. In a Habit Line, it's that Habit's program. In a Prank taunt, it's the program whose window she's pranking (for `cursorSit`, the one Senpai is using).
- `{time}`: a Daily Ritual's usual time, like `21:00`.
- `{hours}`: how long a Habit usually lasts, or a Shame Game's hours.
- `{otherProgram}`: the second program in a Compulsive Switching Habit.

**Secrets.** Mark any Line using these with `"voicesSecret": true`:
- `{file_name}`
- `{count}`
- `{folder}`
- `{game_name}`
- `{oldest_download}`

**The PC:**
- `{uptime}`
- `{disk_used}`
- `{recycle_bin}`

## A pack

```json
// Comments like this are fine.
{
  "name": "friends",
  "minAppVersion": "0.2.0",
  "lines": [
    { "id": "...", "trigger": "...", "text": "..." }
  ]
}
```

- **`name`**: what the pack is called, unique across the repo. A pack here with the same name as one bundled with the app replaces the bundled one. That's how `packs/starter.json` updates the Lines BB ships with.
- **`minAppVersion`** (optional): the oldest version of BB the pack is written for. If you use a trigger, kind, blank or field that's new in some version, set this to that version. Older copies of BB then skip just the Lines they don't understand. Without the bump, they'd reject the whole pack as broken. Even then, every Line still needs a unique `id`.

## House rules

- Stay in character. BB is a smug, teasing kouhai who bullies Senpai, hands out terrible advice, and gets more possessive at later Stages.
- Keep it fun for everyone who runs her. Nothing truly cruel, and nothing about real people.
- Only text, never links.
