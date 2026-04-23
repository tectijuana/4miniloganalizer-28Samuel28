# 🌐 Cómo usar Mockaroo para generar tu `logs.txt`

## Paso 1 — Configura el schema en mockaroo.com

Ve a [mockaroo.com](https://mockaroo.com) y configura **una sola columna** así:

| Field Name | Type | Opciones |
|---|---|---|
| `status_code` | **Custom List** | `200, 201, 204, 301, 400, 401, 403, 404, 500, 502, 503, 504` |

Luego:
- **Rows:** `100`
- **Format:** `Custom` (para que no ponga header)
- **Header:** ❌ desactivado
- En el campo de formato personalizado escribe solo: `{status_code}`

Así cada línea tendrá **únicamente el número**, que es lo que lee tu `analyzer.s`.

---

## Paso 2 — Descarga y renombra

Descarga el archivo generado y renómbralo a `logs.txt`. Súbelo a tu repo de GitHub.

---

## Paso 3 — Mejora el Makefile para incluir más tests

Actualiza tu `Makefile` con casos de prueba que usen ese `logs.txt` real.

---

## 📊 Tu nuevo `logs.txt` (estilo Mockaroo)

100 líneas con distribución **realista** de un sistema cloud:

| Familia | Códigos | Cantidad |
|---|---|---|
| 2xx exitosos | 200, 201, 204 | 58 registros |
| 3xx redirecciones | 301, 304 | 7 registros |
| 4xx errores cliente | 400, 401, 403, 404 | 20 registros |
| 5xx errores servidor | 500, 502, **503**, 504 | 14 registros |

El **primer 503 está en la línea 27** — tu `analyzer.s` debe imprimir exactamente eso.

---

## 🔧 Cómo usar Mockaroo manualmente (para tu evidencia)

Si tu docente quiere que uses Mockaroo directamente desde la web:

1. Ve a **mockaroo.com**
2. Borra todas las columnas predeterminadas → agrega una columna llamada `status_code`
3. Tipo: **Custom List** → valores: `200, 201, 204, 301, 400, 404, 500, 502, 503, 504`
4. Rows: `100` · Format: **Text** · Header: **desactivado**
5. Formato personalizado: solo escribe `{status_code}`
6. Click en **Download Data** → renombra a `logs.txt`

---

## ✅ Estado final de tu repo

| Archivo | Estado |
|---|---|
| `analyzer.s` | ✅ Código ARM64 completo |
| `Makefile` | ✅ 8 tests unitarios + test Mockaroo |
| `logs.txt` | ✅ 100 filas estilo Mockaroo |
| `README.md` | ✅ Documentación completa |

Con `make test` obtendrás evidencia de los 8 casos unitarios **más** el análisis del archivo de 100 filas generado con Mockaroo.
