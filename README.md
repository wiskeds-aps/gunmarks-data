# gunmarks-data

Weekly-refreshed snapshot of real per-vehicle "Marks of Excellence" damage
thresholds (65/85/95/100%) for the EU realm, consumed by
[hangar_stats](https://github.com/wiskeds-aps/hangar_stats)' "Прогресс до
3 отметок" widget.

## Why this exists

WG's own per-tank mark thresholds (the "Big Three" baseline damageRating
is computed against) aren't published as a fetchable dataset. `poliroid.me/
gunmarks/` computes and shows these real thresholds through their own
public, unauthenticated API. Rather than have the game mod itself depend
on that undocumented third-party API live (which the mod's client would
otherwise need internet access for during every hangar session, and which
would break the mod if that site ever goes down or changes shape), this
repo pulls a snapshot into `data/eu.json` once a week via GitHub Actions
(`.github/workflows/update.yml`) and the mod fetches ITS OWN copy from
here instead (`raw.githubusercontent.com/.../data/eu.json`).

## Data source & disclaimer

Source: `https://poliroid.me/gunmarks/api/v2/data/eu/vehicles/65,85,95,100`
(public, no authentication). This project is not affiliated with
poliroid.me or Wargaming. Data is refreshed weekly (Mondays, 04:00 UTC) --
see `tools/fetch_poliroid.py`. Run it manually with `python3
tools/fetch_poliroid.py` from the repo root; it overwrites `data/eu.json`.

## `data/eu.json` shape

```json
{
  "meta": {"source": "...", "realm": "eu", "fetched_at": "...", "vehicle_count": 813},
  "thresholds": {
    "<compactDescr>": {"65": 1823, "85": 2658, "95": 3295, "100": 3781}
  }
}
```

`<compactDescr>` is the vehicle's own WoT client compactDescr int (the
same int `g_currentVehicle.intCD` / `vInfo.vehicleType.compactDescr`
already is in-client) -- not a poliroid-internal id. See
`tools/fetch_poliroid.py`'s docstring for how that was confirmed.
