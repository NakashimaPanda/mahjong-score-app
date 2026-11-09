# Mahjong Score App

Version 1.0.0

[Japanese version](README.ja.md)

An app for managing results in four-player mahjong. It handles everything in one place: registering players, creating matchups, entering scores for each game (hanchan), displaying the standings, and exporting results as CSV.

The app itself is a single HTML file (`mahjong-score-app.html`). No server or installation is needed — just open the file in a browser. It also works offline.

## Features

- Member registration (including import from a text file)
- Automatic creation of matchups (table assignments)
- Score entry per game and table, with automatic rank calculation
- Standings (with totals and ranks), plus a matchups and schedule display
- CSV export of results
- Japanese / English display switching (member names you enter are shown as-is)
- Dark mode / light mode switching

## How to use

### 1. Open the file

Open `mahjong-score-app.html` in your browser. No internet connection is required.

### 2. Register members

On the Members screen, enter the participants' names.

- Type a name and press Enter or Tab to move to the next name field. Pressing Enter in the last field adds a new field.
- You can also load participants from a text file (one person per line). Loading via the "Import Participants from Text File" button adds to the current list, while dragging and dropping a file onto the Members screen registers the participants as new, replacing all current participants. Shift_JIS files are also supported.
- At least 4 members must be registered to play.

### 3. Set the time schedule

On the Time Settings screen, enter the event date, start time, end time, and the duration of one game. If you want a lunch break, you can also set its target start time and length.

- The lunch break starts at the game boundary closest to the target start time, and lasts for the set length.
- Game durations are a guideline for running the event. Small shifts in the actual schedule on the day are fine.

### 4. Set the table rules

On the Table Settings screen, set the starting points, the return points, the uma (small and large), and the "Handling of 100 Points". See "Scoring" below for details.

### 5. Create the matchups

On the main screen (the standings), press "Create Matchups" to create a match schedule for each game.

Matchups are decided mechanically: the same members and the same number of games always produce the same schedule. On top of that, the schedule is adjusted to avoid the following biases:

- The same players playing each other repeatedly, as far as possible
- Sitting out being concentrated on particular players (players with the fewest sit-outs sit out first)
- Sitting at the same table three times in a row (a sit-out in between breaks the streak; this is relaxed only when it cannot be avoided, such as when there is only one table)

Recreating the matchups erases any results already entered.

### 6. Enter scores

On the Score Entry screen, select a game and a table, then enter the raw scores of the four players.

- Raw scores can be entered as "25300" or "25.3"; both are treated as 25300. Entry is by keyboard only.
- Enter a raw score and press Enter or Tab to move the cursor to the next player's field.
- Seat winds are fixed from the top as East, South, West, North. To change a player's seat wind, drag their name onto the row of the seat wind you want and drop it there. Any raw score and rank already entered move with that player.
- Once three scores are entered, the fourth is filled in automatically as "Starting Points × 4 − the total of the other three". An automatically filled value can still be edited by hand.
- In the case of a tie, the player closer to East ranks higher. Ranks are calculated automatically and can also be changed by hand afterwards.
- If the four scores do not add up to starting points × 4, a notice appears at the bottom of the screen (a confirmation also appears when saving).

How the Score Entry screen first appears depends on how you opened it:

- Opened by clicking a row in the matchups list: that table is selected, and any saved data for it is shown.
- Opened from the "Score Entry" button at the top, or the "Go to Score Entry" button on the main screen: it opens in its initial state (the first game, the first table, and empty score fields).

Note that if you save from the screen opened in its initial state via a button, it will overwrite any saved data for that table. To edit a table that already has scores, open it from its row in the matchups list.

### 7. Check the standings and export as CSV

The standings on the main screen show each member's final score, rank, and raw score for every game.

- Cell colours indicate the table (in the same game, the same colour means the same table). Sit-outs are shown in grey and marked "Rest".
- Click a cell in the standings to jump to the member details for that game and table.
- Click the "Total" or "Rank" header to sort by rank (1st on top); click again to return to the original registration order.
- Press "Export Results as CSV" to save the results as a CSV file.

## Scoring

The final score is calculated with the following formula:

```text
Final score = (raw score − return points) / 1000 + uma + (oka, 1st place only)
```

- The uma is: 1st place +large, 2nd +small, 3rd −small, 4th −large. For example, for uma 5-10, enter small = 5 and large = 10.
- The oka is added to 1st place automatically.
- Tobi (going bust) and yakitori are not handled (scores are based on raw scores).

"Handling of 100 Points" sets how the fraction, in units of 100 points, from (raw score − return points) / 1000 is treated. You can choose from three methods, shown with the labels used in the app:

| Method | How the fraction is handled |
| --- | --- |
| 5 down / 6 up | Discard a fraction of 5 or less, and round up at 6 or more (by absolute value) |
| Down if plus, up if minus | Discard the fraction if plus, round it up if minus — rounding toward zero either way |
| Keep raw scores as-is | Keep the fraction to one decimal place, unchanged |

With a rounding method (5 down / 6 up, or down if plus / up if minus), 2nd to 4th place are rounded first, and 1st place absorbs the resulting difference. Because of this, the four players' final scores always add up to 0 under every scoring method.

## Data storage

Everything you enter is saved automatically in your browser on this device (localStorage). Data is not shared with other browsers or other devices.

Because clearing your browser's site data also erases this data, export a CSV at key points to keep a backup. CSV is the only export format.

Pressing "Delete All Data" on the main screen deletes all members, settings, matchups, and results. This cannot be undone.

## Notes

- This app is for four-player mahjong only.
- Re-saving the member list can leave it inconsistent with existing matchups and results. If that happens, recreate the matchups.
- Saving the time settings can change the number of games, which erases existing matchups and results.

## About development

This app was created to manage mahjong results with friends. The requirements and specifications were decided by the developer, while a generative AI (Muse) was used for the code implementation and debugging.

There may be shortcomings in the code structure or security. Suggestions for improvement and pull requests are welcome.

## License

MIT License — see the [LICENSE](LICENSE) file.

© 2026 NakashimaPanda
