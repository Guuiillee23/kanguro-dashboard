# Kanguro · Dashboard Meta Ads + Google Ads

Web: `https://<usuario>.github.io/<este-repo>/` (GitHub Pages, rama `main`, carpeta raíz).

## Cómo funciona

| Rama | Qué hay | Quién la toca |
|---|---|---|
| `main` | `index.html` (la web) y este README | **Kanguro** — se puede modificar libremente |
| `datos` | `datos.json` cifrado | **Maktiva**, automático cada hora. No editar: se reemplaza entera |

1. Maktiva descarga los datos de Meta Ads y Google Ads (Apps Script), los cifra con el usuario y la contraseña del dashboard y los sube a la rama `datos` cada hora.
2. `index.html` descarga `datos.json` desde `https://raw.githubusercontent.com/<usuario>/<repo>/datos/datos.json`, pide usuario y contraseña, lo descifra en el navegador y llama a `iniciar(D0)`.
3. Sin la contraseña los datos no se pueden leer, aunque el repo sea público.

Para probar en local: sirve la carpeta (`npx serve .` o `python3 -m http.server`) y pon en `DATOS_URL` (arriba de `index.html`) la URL raw completa de `datos.json`.

**No tocar** el bloque `ACCESO` del final de `index.html` (descifrado). Todo lo demás (estilos, secciones, gráficas) se puede cambiar dentro de `iniciar(D0)`.

## Estructura de los datos (`D0`)

Fechas como número de días desde `D0.desde` (`'2026-06-01'` = 0). Importes en € (sin IVA, lo que factura Meta/Google).

| Campo | Contenido |
|---|---|
| `actualizado` | Fecha ISO de la última actualización |
| `hoy` | Día de hoy (Europe/Madrid), `AAAA-MM-DD` |
| `desde` | Primer día con datos |
| `paises` | `['ES','PT','FR']` (el resto llega como `'OT'`) |
| `avisos` | Lista de textos si algo no se ha podido descargar |
| `conj` | Conjuntos de anuncios de Meta: `[nombre, campaña, tipo, idCampaña, idConjunto]`. `tipo`: `'f'` = lead form (cuenta clientes potenciales), `'l'` = lead landing (cuenta contactos) |
| `ads` | Anuncios de Meta: `[nombre, índice en conj, url imagen, url anuncio, idAnuncio]` |
| `m` | Meta por día, conjunto y país: `[día, índice conj, país, inversión, leads form, leads landing]` |
| `a` | Meta por día, anuncio y país: `[día, índice ads, país, inversión, leads form, leads landing]` |
| `gc` | Nombres de campañas de Google Ads |
| `g` | Google Ads por día, campaña y país: `[día, índice gc, país, inversión, conversiones, impresiones, clics]` |

Reglas de leads (Meta): los conjuntos «Lead Landing» cuentan solo **contactos**; los «Lead Ads» / «Formulario Meta» solo **clientes potenciales**. Atribución 7 días clic + 1 día visualización.

Si necesitáis un dato que no está aquí (otra métrica de Meta, otro desglose…), pedídselo a Maktiva: se añade en el Apps Script y aparece en `D0`.
