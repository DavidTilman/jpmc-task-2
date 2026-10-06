# JPMC Task 2 — Use J.P. Morgan Chase Frameworks

Task 2 of the J.P. Morgan Chase Software Engineering virtual experience (Forage).
A React + TypeScript web app streams quotes from a mock exchange server and plots
them live with JPMC's open-source [Perspective](https://perspective.finos.org/)
library.

## What was implemented

- **`src/App.tsx`** — after clicking *Start Streaming Data*, the app polls the
  server every 100 ms (up to 1000 times) and keeps appending data, instead of fetching
  a single snapshot. The graph is only shown once streaming starts.
- **`src/Graph.tsx`** — the `perspective-viewer` is configured as a continuous
  line chart:
  - `view: y_line`
  - `column-pivots: ["stock"]` — one line each for `ABC` and `DEF`
  - `row-pivots: ["timestamp"]` — time on the x-axis
  - `aggregates` — duplicate rows for the same stock and timestamp are averaged,
    so each timestamp yields a single point

## Running

Requires Python 3 and Node.js.

```sh
# terminal 1: mock exchange on port 8080
pip install -r requirements.txt
python server.py

# terminal 2: web app on http://localhost:3000
npm install
npm start
```

The `start` script passes `--openssl-legacy-provider`, which is needed by the
pinned `react-scripts` 2.x on Node 17+.

## Files

| File | Description |
|------|-------------|
| `server.py` | Mock exchange that replays `test.csv` |
| `src/DataStreamer.ts` | Fetches quotes from the server |
| `src/App.tsx` | App shell and streaming loop |
| `src/Graph.tsx` | Perspective table and chart configuration |
