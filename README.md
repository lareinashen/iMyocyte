# iMyocyte

A React-based interactive application for visualizing and simulating cardiomyocyte behavior and signal propagation.

## Overview

iMyocyte is a web application built with React, TypeScript, and Vite that provides an interactive interface for exploring cardiomyocyte properties and signal propagation patterns. The application features multiple visualization modes including ring diagrams, neighbor modes, and location-based views.

## Features

- **Multiple Visualization Modes**
  - Ring Diagram visualization
  - Neighbor Mode
  - Neighbor Ring Mode
  - Location Mode
  
- **Real-time Signal Propagation**
  - Fire signal simulation
  - Real-time updates using Ably
  
- **Interactive UI**
  - Material-UI components
  - Bootstrap styling
  - Responsive design

## Tech Stack

- **Frontend Framework:** React 18
- **Language:** TypeScript
- **Build Tool:** Vite
- **UI Libraries:** 
  - Material-UI (MUI)
  - React Bootstrap
  - Emotion (CSS-in-JS)
- **Real-time Communication:** Ably
- **Testing:** Vitest, React Testing Library
- **Linting:** ESLint

## Prerequisites

- Node.js (v16 or higher recommended)
- npm or yarn package manager

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/lareinashen/iMyocyte 
   cd iMyocyte
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Set up Ably API Key:
   - Create a free account at [https://ably.com](https://ably.com)
   - Navigate to your dashboard and create a new app (or use an existing one)
   - Copy your API key from the "API Keys" tab
   - Open `src/data/Constants.ts` in your project
   - Replace `'***'` in the `ABLY_API_KEY` constant with your actual API key:
     ```typescript
     export const ABLY_API_KEY = 'your-actual-api-key-here';
     ```

4. Run the development server:
   ```bash
   npm run dev
   ```
   Open [http://localhost:5173](http://localhost:5173) to view the application in your browser.

## Available Scripts

### Development

```bash
npm run dev
```
Runs the app in development mode. Open [http://localhost:5173](http://localhost:5173) to view it in the browser.

### Build

```bash
npm run build
```
Builds the app for production to the `dist` folder. It compiles TypeScript and optimizes the build for best performance.

### Preview

```bash
npm run preview
```
Preview the production build locally.

### Lint

```bash
npm run lint
```
Runs ESLint to check code quality and style.

### Deploy

```bash
npm run deploy
```
Deploys the application to GitHub Pages.

## Project Structure

```
iMyocyte/
├── src/
│   ├── Pages/           # Page components
│   │   ├── ChooseMode.tsx
│   │   ├── FireSignal.tsx
│   │   ├── LocationMode.tsx
│   │   ├── NeighbourMode.tsx
│   │   ├── NeighbourRingMode.tsx
│   │   ├── RingDiagram.tsx
│   │   ├── RunningInfo.tsx
│   │   └── Steps.tsx
│   ├── Services/        # Utility functions
│   ├── assets/          # Static assets
│   ├── data/            # Data models and constants
│   │   ├── Constants.ts
│   │   ├── CountContext.tsx
│   │   ├── Enums.ts
│   │   ├── GlobalVariables.ts
│   │   └── myocyteProperties.json
│   ├── App.tsx          # Main app component
│   └── main.tsx         # Entry point
├── public/              # Public assets
├── dist/                # Build output
└── package.json         # Dependencies and scripts
```

## Configuration

The application uses Ably for real-time communication. See step 3 in the Installation section above for instructions on setting up your Ably API key.

## For Instructors: Running iMyocyte in Your Classroom

This section is a quick guide for instructors who want to use iMyocyte as a
teaching tool for cardiac reentry and excitable-media dynamics. It assumes
you have already completed the installation steps above.

### Overview of a classroom session

iMyocyte is designed around a simple model: one **ring leader** (typically
the instructor or a designated student) builds a circular topology from
**participant devices**, triggers an initial stimulus, and manipulates
session parameters while the class watches the wave propagate in real time.

On the student side, participation is intentionally frictionless: students
simply open the session URL in any modern browser (phone, tablet, or laptop)
— no installation, account creation, or app download is required.

### Before class: one-time instructor setup

1. Complete the installation steps above (clone the repo, `npm install`,
   configure your Ably API key).
2. Start the development server with `npm run dev`.
3. Note the URL your server is hosted at (by default,
   `http://localhost:5173`). For classroom use, you will usually want to
   deploy the app to a publicly accessible host (e.g., GitHub Pages,
   Vercel, Netlify) so student devices on any network can reach it. See
   the *Deploy* section above.
4. Share the deployed URL with your class (via slide, QR code, or LMS).

> **Tip:** Before the first class, do a dry run with a few devices
> (e.g., your phone + laptop + a colleague's device) to confirm that the
> ring forms correctly and that firing events propagate.

### Running a live session

1. **Have students join.** Students navigate to the same URL. As each
   student joins, they appear in each other's list of
   participants. 
3. **Build the ring.** Designate a student (or yourself!) as the leader, who
   must select participants one at a time to add them to the ring.
   A minimum of 6 cells is required so
   that the circuit time exceeds the refractory period — this is what
   allows reentry to occur.
5. **Adjust session parameters (optional).** Before or during the
   demonstration, the leader can toggle:
   - **Exercise level** (Rest / Light / Moderate / Vigorous) — modulates
     propagation delay and refractory period across all cells.
   - **Sympathetic activation** (on/off) — applies a uniform shift to
     propagation delay and refractory period.
   - **Cell health status** — toggle a specific cell between healthy
     and unhealthy. By default, the cell positioned
   next to the leader in the ring would be designated as the unhealthy cell.
7. **Fire the initial stimulus.** The leader triggers the first
   activation (S1), which launches a bidirectional wavefront around the
   ring. In a healthy ring, waves collide and self-terminate. In a ring
   with an unhealthy cell, a timed second stimulus (S2) can trigger
   unidirectional block and initiate sustained reentry.
8. **Explore parameter effects.** Once participants understand the
   baseline behaviour, vary exercise level, sympathetic activation, or
   the location of the unhealthy cell to demonstrate how these factors
   shift whether the wave self-terminates or sustains.

### Ending a session

- To end the demonstration cleanly, the ring leader refreshes their
  browser. This clears the ring state and removes all participants from
  the topology.
- If the ring leader accidentally disconnects, the ring dissolves
  automatically. Other participants can then rejoin once a new leader
  joins.
- If a non-leader participant disconnects, the topology automatically
  rebuilds around them.

### Running multiple parallel rings (for larger classes)

For classes larger than roughly 15–20 students, we recommend organizing
participants into **subgroups of 6–12 students**, each with their own
ring leader. This reduces the likelihood of topology conflicts and keeps
each ring small enough for students to visually follow the wave.

In the current version, all participants join the same session URL, and
each ring leader selects their own subset of students from the
participant list. **Important limitation:** the current implementation
does not prevent a single student from being selected as a neighbour
in more than one ring simultaneously. To avoid merged or chained rings,
coordinate with other leaders beforehand (e.g., by assigning non-overlapping
student rosters to each ring) and avoid re-adding participants who are
already in another ring.

Topology hardening (e.g., locking rings once formed, enforcing a strict
two-neighbour cap) is planned for a future release.

### Known limitations

- Participants can occasionally be assigned to more than one ring if
  multiple leaders select the same student. Coordinate with co-leaders
  in larger deployments.
- Network conditions (e.g., students on mobile data with variable
  signal) can introduce timing jitter. The simulation is robust to
  small jitter but may occasionally produce unexpected propagation
  patterns on unreliable networks.
- The demonstration is designed to illustrate qualitative dynamics; it
  is not a biophysically calibrated simulator.

### Troubleshooting

| Issue | Likely cause | Fix |
|---|---|---|
| Students don't appear in leader's list | Students not on the same URL, or Ably connection failed | Confirm URL; check that the Ably API key is valid and not rate-limited |
| Ring won't form | Fewer than 6 participants in the session | Add more participants before building the ring |
| Wave doesn't propagate | Leader hasn't triggered the initial stimulus, or a cell is stuck in refractory | Refresh the leader's browser to reset the ring |
| Multiple rings merge into a chain | A student was added as a neighbour in more than one ring | Rebuild rings with non-overlapping student rosters |

### Pedagogical tips

- Run the healthy-ring demonstration **first** so students see clean
  wave collision and self-termination before introducing reentry.
- After the first reentry demonstration, invite students to predict
  what will happen when exercise level or sympathetic activation is
  changed, then run the simulation to confirm.
- The same session can illustrate related concepts: refractoriness,
  conduction block, and the dependence of reentry on pathway length.

For the full pedagogical framing and learning objectives, see the
accompanying manuscript (Shen et al., *The Biophysicist*).


## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Deployment

The application is configured for deployment to GitHub Pages at: https://github.com/lareinashen/iMyocyte 

## Contact

For questions or support, please open an issue in the repository.

