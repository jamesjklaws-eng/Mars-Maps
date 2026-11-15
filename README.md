# Mars Survival and Navigation Guide

A route planner for astronauts walking on the surface of Mars. It covers the western half of Jezero Crater, where NASA's Perseverance rover landed, and tells the crew before they step outside what a walk will cost in time, oxygen and risk.

The whole project is one file, `index.html`. The page inside it is titled "Jezero Marswalk Navigator".

## Run it

Open `index.html` in a web browser. There is nothing to install and no server to start.

The 3D view loads the three.js library from a public CDN, so it needs an internet connection the first time. Without one, or on a device without WebGL, the page falls back to a flat map and everything else still works.

To publish it as a website, turn on GitHub Pages for this repository (Settings → Pages → deploy from the main branch).

## What it does

- **Plans walking routes.** Pick a start and a destination by clicking the map, or choose one of four science destinations: Three Forks, Hawksbill Gap, the rim of Belva Crater and the Kodiak butte.
- **Offers three routes for each walk:** fastest, safest and most science, as a return trip or one way.
- **Costs every route** in walking time, distance, climb, steepest slope, oxygen used and radiation dose.
- **Scores the risk from 1 to 100**, where 1 is the lowest risk and 100 means the oxygen runs out. The page lists what makes up each score.
- **Forecasts the weather** for the chosen departure time: temperature, wind and humidity through the Martian day, with warnings for darkness, extreme cold, dust-devil hours and dust storms.
- **Gives turn-by-turn directions** and an elevation profile, with a playback of the walk.
- **Shows four map layers:** the NASA orbital image, slope hazard, an ESA topographic map and ground type.
- **Works on phones**, with the map pinned to the top of the screen and the details in tabs.

## How it works

| Part | Method |
|---|---|
| Route finding | A* search over a grid of 100 m cells, minimising walking time rather than distance |
| Walking speed | 2.7 km/h on flat ground, reduced on slopes by Tobler's hiking function, by 18% on sand and by 40% among boulders |
| Safest route | Adds a penalty for steep ground and doubles the cost of sand and boulders |
| Energy | Working effort (300 W by default) plus the work of lifting crew and suit against Mars gravity of 3.71 m/s², at 25% muscle efficiency |
| Oxygen | One litre per 20.1 kJ of energy, about 77 g an hour at 300 W, checked against a 30-minute reserve |
| Radiation | 0.64 mSv per day |
| Weather | An hour-by-hour curve fitted between recorded ranges of temperature, wind and humidity |
| Risk score | Mostly the oxygen margin, with points added for steep ground, soft ground, long walks and bad weather |

Suit mass, working effort and oxygen tank size are adjustable, and every figure recalculates.

## Data and credits

- **Surface image:** Mars 2020 CTX orthomosaic of Jezero Crater, 6 m per pixel, from the Context Camera on Mars Reconnaissance Orbiter. Credit: NASA/JPL-Caltech/MSSS, mosaic by USGS Astrogeology.
- **Topographic map:** Mars Express HRSC elevation map of Jezero Crater. © ESA/DLR/FU Berlin, licensed [CC BY-SA 3.0 IGO](https://creativecommons.org/licenses/by-sa/3.0/igo/). It is cropped to match the NASA image.
- **Walking speed:** the planning figure for long walkbacks recorded in NASA's [Apollo Lunar Surface Journal](https://www.nasa.gov/wp-content/uploads/static/history/alsj/a11/a11.gaits.html).
- **Radiation:** the surface dose measured by the RAD instrument on NASA's Curiosity rover.
- **Weather ranges:** a summary of readings from the MEDA weather station on NASA's Perseverance rover.
- **Landing site:** Octavia E. Butler Landing, 18.4447°N 77.4508°E.

## Limitations

This is a prototype, and several parts are estimates.

- **Terrain heights are not measured.** The crater wall, the delta, Kodiak, Belva Crater and the sand fields were traced by eye from the images and given estimated heights, calibrated against the contour values on the ESA map. Slopes, climbs and routes come from that traced model. Loading NASA's HiRISE or CTX elevation data would replace it.
- **Some place positions are approximate**, in particular Three Forks, Hawksbill Gap and Cape Nukshak.
- **The oxygen tank size is an assumption** (550 g by default), as are the sand and boulder factors, the stop times and the working effort.
- **Walking speeds come from the Moon.** Nobody has walked on Mars, where gravity is more than twice as strong.
- **The weather is a forecast from recorded ranges, not live data.** The weather figures came from an AI-generated summary of MEDA data and have not been checked against NASA's archive. Sunrise and sunset times and the dust-storm adjustment are estimates.
- **The risk score is this project's own scale**, not a NASA standard.
- **The image covers the western half of the crater only**, about 30 by 30 km.

## Built with

- JavaScript, HTML and CSS in a single file
- [three.js](https://threejs.org/) r128 for the 3D terrain
- The Canvas 2D API for the map layers, charts and the flat fallback map
- Python, used only to crop and prepare the two map images before embedding them

## Use of AI

Claude, made by Anthropic, was used for research, data gathering and writing the code. The weather summary was produced with Google Gemini.

## Licence

The topographic map keeps its CC BY-SA 3.0 IGO licence, which requires the ESA/DLR/FU Berlin credit to stay with it. NASA imagery is credited as above.
