---
name: weather
description: Get the current weather and forecast for Longchamps, Buenos Aires, Argentina. Use when the user asks about the weather, temperature, rain, wind, or forecast for their local area ("clima", "qué tiempo hace", "va a llover").
---

# Local weather: Longchamps, Buenos Aires

Fetch live data from the free Open-Meteo API (no API key needed).

Location: Longchamps, Almirante Brown partido, Buenos Aires province, Argentina
- Latitude: -34.8667
- Longitude: -58.3833
- Timezone: America/Argentina/Buenos_Aires

## Steps

1. Run this command (works in Bash and PowerShell):

```
curl -s "https://api.open-meteo.com/v1/forecast?latitude=-34.8667&longitude=-58.3833&current=temperature_2m,apparent_temperature,relative_humidity_2m,precipitation,weather_code,wind_speed_10m,wind_direction_10m&daily=weather_code,temperature_2m_max,temperature_2m_min,precipitation_probability_max,precipitation_sum&timezone=America%2FArgentina%2FBuenos_Aires&forecast_days=3"
```

2. Parse the JSON and summarize it for the user:
   - **Now**: temperature, feels-like, humidity, wind (km/h and direction), precipitation.
   - **Next 3 days**: min/max temperature, chance of rain, and a short condition description.

3. Translate `weather_code` (WMO) to plain text:
   - 0 clear sky; 1-3 mainly clear / partly cloudy / overcast
   - 45, 48 fog
   - 51-57 drizzle; 61-67 rain; 71-77 snow
   - 80-82 rain showers; 95 thunderstorm; 96, 99 thunderstorm with hail

## Output format

- Respond in English, concise (a few lines).
- Use °C and km/h.
- Mention a practical tip only if relevant (umbrella, strong wind, heat).
- If the user asks about a different place, use Open-Meteo's geocoding API
  (`https://geocoding-api.open-meteo.com/v1/search?name=<place>`) to get its coordinates and reuse the same call.
- If the request fails, say so plainly; don't invent data.
