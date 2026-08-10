import requests
import json
from datetime import datetime, timezone

BBOX = {"lamin": 41, "lomin": -5, "lamax": 51, "lomax": 10}  # France

response = requests.get("https://opensky-network.org/api/states/all", params=BBOX, timeout=30)
response.raise_for_status()
data = response.json()

features = []
for state in data.get("states") or []:
    icao24, callsign, origin_country, time_position, last_contact, lon, lat, baro_alt, on_ground, velocity, true_track, vertical_rate = state[:12]

    if lon is None or lat is None:
        continue  # plane with no recent location / ignore and continue

    features.append({
        "type": "Feature",
        "geometry": {"type": "Point", "coordinates": [lon, lat]},
        "properties": {
            "icao24": icao24,
            "callsign": (callsign or "").strip(),
            "origin_country": origin_country,
            "altitude_m": baro_alt,
            "velocity_ms": velocity,
            "heading": true_track,
            "on_ground": on_ground
        }
    })

geojson = {
    "type": "FeatureCollection",
    "generated_at": datetime.now(timezone.utc).isoformat(),
    "features": features
}

with open("docs/data/flights.geojson", "w", encoding="utf-8") as f:
    json.dump(geojson, f, ensure_ascii=False)

print(f"{len(features)} avions écrits dans data/flights.geojson")