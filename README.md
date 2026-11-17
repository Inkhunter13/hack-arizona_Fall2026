<p align="center"><img src="web/icons/logo.svg" alt="SprayWait logo" width="240"></p>

# SprayWait

**Should I spray pesticide right now?** SprayWait answers **Go**, **Caution**, or **Stop** for orchard applicators, with the reasons in plain language. It reads weather at two heights in the orchard, so it catches drift risks that a single ground-level weather station misses.

**[Try the live app →](https://spraywait.onrender.com)** (free hosting; the first load can take up to a minute)

<table>
  <tr>
    <td align="center"><img src="docs/screenshots/demo-inversion.png" alt="Hidden inversion: ground station says GO, SprayWait says STOP" width="260"></td>
    <td align="center"><img src="docs/screenshots/station.png" alt="Farm station page charting temperature and wind at 5 ft and 30 ft" width="260"></td>
    <td align="center"><img src="docs/screenshots/log.png" alt="Spray log with time-stamped decisions and triggers" width="260"></td>
  </tr>
  <tr>
    <td align="center"><sub>A ground station alone says GO.<br>The 30 ft sensor makes it STOP.</sub></td>
    <td align="center"><sub>Station readings at both heights,<br>and spray status over the day.</sub></td>
    <td align="center"><sub>Time-stamped log of every decision<br>and what triggered it.</sub></td>
  </tr>
</table>

## 5th of 30 teams at the AgX Arizona Summit hackathon

Built in 6 hours at the [AgX Arizona Summit](https://hack.arizona.edu/hack-arizona-agx-hacking-future-agriculture), hosted by [Hack Arizona](https://hack.arizona.edu/) on November 15, 2026. Scored **18.5/20**.

- **Zach Keyes:** app UI, station, forecast, log, alerts, Render deployment, demo, and docs. [LinkedIn](https://www.linkedin.com/in/zkey/) · [Website](https://mnestic.space)
- **Joy Hewang:** pesticide data, science research, and app UI features. [LinkedIn](https://www.linkedin.com/in/lunjoyhewang/)
- **Leo Edgin:** inversion and wind-shear models and the core decision engine. [LinkedIn](https://www.linkedin.com/in/leo-edgin-31a50724b/)

## Why it matters

Pesticide that drifts off target harms neighboring crops, workers, and communities. The riskiest condition is a **temperature inversion**: cool air trapped under warmer air, which holds spray in a shallow layer that can travel laterally for long distances. It feels like a calm, perfect evening at ground level, so a single weather station reports it as safe. Comparing the temperature at 5 ft and 30 ft reveals it.

## What it does

- **Go / Caution / Stop right now**, with every reason shown using the measured numbers: inversion, wind too low or too high, wind changing speed or direction with height, and the pesticide's heat and volatility limits.
- **Alerts** on screen and as a phone notification when conditions get worse.
- **Plans ahead:** a 24-hour forecast strip shows the next safe spray window, or warns before conditions turn bad.
- **Keeps a record:** a time-stamped log of every reading, decision, and trigger, downloadable as CSV.
- **Fits the job:** 47 pesticides with label limits and volatility, and a nozzle picker that sets droplet size.
- **Demo mode** ([/demo](https://spraywait.onrender.com/demo)): 8 scenarios, each isolating one check, side by side with what a ground station alone would say.

## Engineering highlights

- **Fail-safe by design:** missing, stale, or single-height data can never produce GO. Rules can only add caution, never remove it.
- **Real data:** the live station replays a recorded day of Oklahoma Mesonet readings in real time, including a real evening inversion.
- **One engine, two uses:** the 24-hour forecast runs the same checks as the live decision, so the two never disagree.
- **Hardware-ready:** stations post readings to an authenticated API; a real sensor mast only replaces the data source.
- **Sourced rules:** every threshold cites UC ANR or NDSU Extension guidance, or is a stated design choice that leans toward caution ([LOGIC.md](docs/LOGIC.md)).
- **Tested:** 185 pytest tests, 99% coverage.

## How it works

```
Farm station (5 ft + 30 ft)  ─┐
Pesticide + nozzle           ─┼─► Checks ─► GO / CAUTION / STOP + reasons ─► Alert, Log
Hourly forecast (24 h)       ─┘            (same checks per hour) ─► Next spray window
```

Five checks run on every reading: data quality, inversion, wind speed, wind shear, and pesticide limits. The worst result wins, and every reason is shown. The research is in [docs/SCIENCE.md](docs/SCIENCE.md), and every rule with its source in [docs/LOGIC.md](docs/LOGIC.md).

## Tech stack

Python and FastAPI (rule engine and API), plain HTML/JS installable web app (PWA), Open-Meteo forecasts, pytest, hosted on Render. Setup and code layout are in [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md).

## What's next

- Real station hardware.
- Alerts to a closed app (Web Push), and text alerts.
- Watching volatile products after spraying.
- Calibrating inversion strength against measured drift.

> SprayWait supports the applicator's decision. It does not replace pesticide labels, regulations, or professional judgment.

## License

[MIT](LICENSE)
