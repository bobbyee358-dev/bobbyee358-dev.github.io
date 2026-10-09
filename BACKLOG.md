# Backlog

## In progress
- Helper (Cloudflare Worker): background NWS alert notifications for the saved location.
  Waiting on: Cloudflare account, API token and account ID stored as repo secrets.

## Radar ideas
- Check the filtered legend's top-end colors against IEM's official MRMS scale (mrms_lcref.png).
- Future radar (HRRR forecast reflectivity, available from the same IEM tile service).
- Satellite layer (GOES, IEM tile service).
- Sharper single-radar view when zoomed in.
- Air quality and marine/surf tabs (Open-Meteo; check browser access first).

## Weekly health check
- GitHub Actions job that checks each data source still responds.

## App ideas (not yet chosen)
- Grocery list and meal planner, recipe box, habit tracker, budget log, calculators, timers.
- Needs the helper: news reader, podcast player.

## Done
- Oct 8 2026: Nimbus Radar v1 live; fixed midnight label overlap and duplicate NWS alert cards.
- Oct 8 2026: Radar defaults to filtered NOAA MRMS (false bird/insect bloom gone, confirmed on phone); raw NEXRAD toggle; per-mode legend.
- Oct 8 2026: Hub site live with launcher; radar moved to /radar/; old /Nimbus/ address forwards there.
