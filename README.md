# HomeAssistantGoodies

Blueprints, scripts and dashboard cards for Home Assistant.

```
blueprints/automation/   Automation blueprints (importable)
scripts/                 Scripts the blueprints call
dashboards/              Dashboard cards
```

## Daily Sprinkler

Waters once a day for a duration based on today's forecast maximum
temperature. Skips if rain has already fallen, rain is forecast, it's too
cool, or a sensor is unavailable. Each run writes a line such as
`Ran Mon 19 Oct, 06:00 for 10 min (forecast 27°C)` or
`Skipped Mon 19 Oct: 3.2 mm forecast today` to a text helper, which a
dashboard banner displays.

[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fandrew-savage%2FHomeAssistantGoodies%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fdaily_sprinkler.yaml)

### Setup

1. **Text helper.** Settings > Devices & services > Helpers > Create helper
   > Text. Name it `Sprinkler last run` and set max length to 255.
2. **Run script.** Create [`scripts/sprinkler_run.yaml`](scripts/sprinkler_run.yaml)
   as `script.sprinkler_run`, with your valve's MQTT topic and payload.
   Any script works as long as it accepts a `seconds` field.
3. **Blueprint.** Import it with the button above, then create an automation
   from it and pick your sensors, script and helper.
4. **Banner.** Add [`dashboards/sprinkler_status_card.yaml`](dashboards/sprinkler_status_card.yaml)
   as a manual card.

### Inputs

| Input | Default | Notes |
|---|---|---|
| Run time | 06:00 | Victoria only allows watering systems 6pm–10am |
| Forecast max temperature | — | e.g. BOM `temp_max_0` |
| Forecast rain amount | — | e.g. BOM `rain_amount_max_0` |
| Observed rain | — | A 24-hour sensor is better than daily rain, which resets at midnight |
| Skip if observed rain ≥ | 0.8 mm | |
| Skip if forecast rain ≥ | 2 mm | |
| Bands | 20/25/30/35 °C → 5/10/15/20 min | Each band applies from its temperature up to the next |

Below the first band, it doesn't water.
