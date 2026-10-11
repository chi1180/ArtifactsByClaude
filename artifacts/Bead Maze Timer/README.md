# Bead Maze Timer

A Pomodoro timer designed around the classic bead maze toy. Beads slide back and forth along colored wires, and every pomodoro you finish adds a new bead to the toy. Set a goal, fill the toy, and keep studying.

Built with Claude. Single HTML file, no dependencies, no build step.

Live version: https://chi1180.github.io/ArtifactsByClaude/artifacts/Bead%20Maze%20Timer/Bead%20Maze%20Timer.html

## How it works

1. You set a goal time (for example, 4 hours).
2. The app divides it into pomodoros: `round(goal / (focus + break))`, with a minimum of 1. With the defaults (240 / 25 / 5), that is 8 pomodoros.
3. Each pomodoro appears as a gray bead resting on the wooden base.
4. When a focus session ends, its bead turns colored, jumps onto a wire, and starts sliding.
5. When every bead is on the wires, the goal is reached.

Beads keep moving the whole time:

| State | Bead speed |
| --- | --- |
| Focus | Normal |
| Break | Faster |
| Paused | Slow |

If your system has "reduce motion" enabled, the beads stay still.

## Features

- Focus and break lengths with a configurable goal time
- Automatic switch between focus and break
- Sound alert (three short tones) at each phase change
- Browser notifications for pomodoro complete, break over, and goal reached
- Remaining time shown in the browser tab title while running
- Light and dark appearance that follows the system setting
- Settings saved in the browser

## Controls

- **Start / Pause**: starts or pauses the current phase.
- **Skip**: ends the current phase immediately. Skipping a focus session counts it as completed.
- **Reset**: clears progress and returns to the first focus session.
- **Again**: shown after the goal is reached; starts a fresh run.

## Settings

| Setting | Default | Notes |
| --- | --- | --- |
| Goal (min) | 240 | 10 or more |
| Focus (min) | 25 | 1 or more |
| Break (min) | 5 | 1 or more |

Changing any value resets the timer. Values are stored in the browser's `localStorage`.

## Notifications

The browser asks for notification permission the first time you press Start. If you decline, the sound alerts still play.

## Run locally

Open `Bead Maze Timer.html` in any modern browser. No server is needed.

## Known limitations

- The timer runs in the open page. Closing the tab stops it.
- Phase changes are checked on each animation frame, so browsers may delay the sound and notification while the tab is hidden or the phone screen is off. Keep the tab visible for accurate alerts.
- Progress is not saved between sessions. Only the settings are.

## Technical notes

- Plain HTML, CSS, and JavaScript in one file.
- The toy is an SVG. Beads follow the wire paths with `getPointAtLength`, moving back and forth along each path.
- The countdown uses timestamps rather than counting ticks, so the displayed time stays correct even if frames are skipped.
- Sound is generated with the Web Audio API; no audio files are used.
