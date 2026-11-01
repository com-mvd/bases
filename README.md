# bases

Datos que leen las páginas de [com](https://com-mvd.github.io/com/) y [dashboard](https://com-mvd.github.io/dashboard/).
Se publican en `https://com-mvd.github.io/bases/<archivo>`.

**Este repo no guarda historial.** Cada vez que alguien toca **Actualizar** en la portada de com, el n8n de la PC
del COM baja los últimos 90 días del Grafana y reemplaza el único commit de `main` por uno nuevo. Por eso el repo
pesa siempre lo mismo, aunque las bases se actualicen muchas veces por día.

## Archivos

| Archivo | Qué es | Quién lo cambia |
|---|---|---|
| `agenda.json`, `zona-limpia.json`, `reclamos.json`, `bolsones.json` | Base pública de cada servicio (sin teléfonos, cédulas ni emails) | n8n |
| `<servicio>-privado.json` | Textos completos (descripciones, teléfonos), cifrados con la contraseña del sitio | n8n |
| `llave.json` | Sirve para comprobar la contraseña; no la contiene | n8n |
| `actualizado.json` | Hora de la última subida de cada servicio | n8n |
| `bolsones-calendario.json` | Qué municipio se atiende cada día, su planta y los vaciados fijos | **a mano** |

## Editar el calendario de Bolsones

Desde github.com: abrir `bolsones-calendario.json` → lápiz (Edit) → cambiar → **Commit changes**.
No usar GitHub Desktop con este repo: como n8n reemplaza el commit, una copia local queda desfasada enseguida.
n8n conserva el calendario cuando sube las bases (solo reemplaza sus propios archivos).

## Formato de agenda, zona-limpia y reclamos

```
{ "formato": 2, "generado": "2026-10-30T20:48", "columnas": [...], "privadas": [...],
  "fechas": [...], "dicc": { "<columna>": [...] }, "registros": [[...], ...] }
```

- Cada registro es una fila con un valor por columna, en el orden de `columnas` (las del Grafana).
- Si la columna está en `dicc`, el valor es la posición del texto en esa lista (los textos que se repiten van una sola vez).
- Si la columna está en `fechas`, el valor son segundos desde 1970 del reloj de Montevideo, sin zona horaria.
- Las columnas `privadas` van vacías: su contenido está en `<servicio>-privado.json`.

Para leerlas sin pensar en esto, dashboard tiene `assets/bases.js` (`await BasesCOM.filas('reclamos')`).
`bolsones.json` tiene el formato propio de la página Supervisión de Bolsones.
