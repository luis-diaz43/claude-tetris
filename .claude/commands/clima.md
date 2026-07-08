# Skill: clima

Obtén el clima actual para una ubicación usando el servicio público wttr.in (no requiere API key).

## Cómo usar

El usuario puede invocar esta skill con o sin argumentos:
- `/clima` — muestra el clima de La Vega, República Dominicana (ubicación por defecto)
- `/clima Ciudad` — clima para una ciudad específica (ej: `/clima Madrid`)
- `/clima Ciudad,País` — más preciso (ej: `/clima "Buenos Aires,AR"`)

## Instrucciones

1. Lee el argumento `$ARGUMENTS` para saber la ubicación solicitada.
   - Si está vacío, usa `La+Vega,Dominican+Republic` como ubicación por defecto.
   - Si tiene valor, úsalo como la ciudad/ubicación.

2. Construye la URL: `https://wttr.in/{UBICACION}?format=j1&lang=es`
   - Reemplaza `{UBICACION}` con el argumento (URL-encoded si tiene espacios).
   - Si la ubicación está vacía, usa `https://wttr.in/La+Vega,Dominican+Republic?format=j1&lang=es`

3. Usa la herramienta `WebFetch` para obtener el JSON de esa URL.

4. Del JSON resultante, extrae y muestra de forma clara:
   - **Ubicación**: `nearest_area[0].areaName[0].value`, `nearest_area[0].country[0].value`
   - **Condición actual**: `current_condition[0].lang_es[0].value` (descripción en español)
   - **Temperatura**: `current_condition[0].temp_C` °C / `current_condition[0].temp_F` °F
   - **Sensación térmica**: `current_condition[0].FeelsLikeC` °C
   - **Humedad**: `current_condition[0].humidity` %
   - **Viento**: `current_condition[0].windspeedKmph` km/h, dirección `current_condition[0].winddir16Point`
   - **Visibilidad**: `current_condition[0].visibility` km
   - **Pronóstico para hoy**: del array `weather[0]`:
     - Máxima: `maxtempC` °C, Mínima: `mintempC` °C
     - Amanecer: `astronomy[0].sunrise`, Atardecer: `astronomy[0].sunset`

5. Presenta la información en un formato limpio con secciones bien organizadas. Usa iconos de texto apropiados para el clima (sol, nubes, lluvia, etc.) basándote en la descripción.

6. Si el fetch falla o la ubicación no se encuentra, informa al usuario y sugiere intentar con una ciudad más específica.
