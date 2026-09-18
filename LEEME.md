# Horarios KS — cómo mantener y publicar el sitio

Sitio estático: no hay servidor, base de datos ni panel de administración.
La página lee directamente los Excel que pongas en la carpeta `datos/`.

```
index.html              la página completa (diseño + lógica)
404.html                página de "no encontrado" (déjala; evita un problema en Cloudflare Pages)
lib/xlsx.mini.min.js    librería que lee Excel en el navegador (SheetJS 0.18.5)
datos/2026-07.xlsx      un Excel por mes, con el nombre AAAA-MM.xlsx
datos/2026-09.xlsx
datos/cumpleanos.xlsx   Nombre | Apellido | Equipo | Día | Mes
```

## Cada mes (2 minutos)

1. Recibes la rotación en Excel → renómbrala `2026-10.xlsx` (año-mes).
2. Abre el sitio, abajo a la derecha: **Probar un Excel antes de publicarlo**. Te muestra cómo
   quedará y te avisa de equipos nuevos (típico error de tipeo) o días en la columna equivocada.
3. Sube el archivo a la carpeta `datos/` de tu repositorio (pasos detallados más abajo). No hay que tocar `index.html`.

Si la rotación llega como **foto**: pásala al mismo formato de Excel (copia `2026-09.xlsx` como plantilla,
o pídele a Claude que transcriba la foto a ese formato) y sigue los mismos pasos.

## Formato del Excel del mes (el mismo que ya usan)

- Una fila con `Lunes … Viernes`, y debajo una celda por día.
- Dentro de cada celda, una línea por dato (Alt+Enter):
  - 1ª línea: el número del día.
  - Líneas que empiezan con `Equipo …`: equipos que van a **oficina** ese día. Los demás quedan como remoto.
  - Cualquier otra línea es una etiqueta: `FERIADO`, `RECUPERACIÓN`, `★ HAPPY HOUR`, etc.
  - Un día con `FERIADO` y sin equipos se pinta como feriado.
- Los nombres de equipo deben escribirse siempre igual (tildes y mayúsculas no importan).
  Un equipo nuevo aparece solo en los filtros en cuanto figura en un Excel.

## Cumpleaños

`datos/cumpleanos.xlsx`, hoja "Cumpleaños": `Nombre | Apellido | Equipo | Día | Mes`.
Equipo y Mes tienen lista desplegable; si aparece un equipo nuevo, agrégalo en la hoja "Listas".
La fila de ejemplo (nombre con "(ejemplo)") es ignorada por la página. No se guarda año de nacimiento.

## Publicar en GitHub Pages por primera vez (gratis, ~15 minutos)

1. **Crea tu cuenta** en https://github.com/signup (correo, contraseña y un nombre de usuario).
   Ojo: el usuario aparece en la dirección final: `https://USUARIO.github.io/horarios-ks/`. Verifica tu correo.
2. **Crea el repositorio**: arriba a la derecha, botón **+** → **New repository**.
   - Repository name: `horarios-ks`
   - Visibilidad: **Public** (obligatorio en el plan gratuito para usar Pages)
   - Activa **Add README** → **Create repository**.
3. **Descomprime** `horarios-ks.zip` en tu PC y abre la carpeta `horarios-ks`.
4. **Sube los archivos**: en el repositorio, **Add file → Upload files**.
   Selecciona TODO lo que hay *dentro* de la carpeta (`index.html`, `404.html`, `LEEME.md`, y las carpetas `datos` y `lib`)
   y **arrástralo** a la zona de carga. Tiene que ser arrastrando: el botón "choose your files" no acepta carpetas.
   Espera a que termine la lista, baja y pulsa **Commit changes**.
   Comprueba que `index.html` quedó en la raíz del repositorio (no dentro de otra carpeta).
5. **Activa Pages**: pestaña **Settings** → menú izquierdo **Pages** → en "Build and deployment":
   Source = **Deploy from a branch**, Branch = **main**, carpeta **/(root)** → **Save**.
6. Espera 1–3 minutos (la primera vez puede tardar hasta 10), recarga esa misma página y aparecerá
   "Your site is live at…" con el botón **Visit site**. Esa es la dirección para compartir.

## Actualizar cada mes desde GitHub (2 minutos)

1. Renombra el Excel del mes a `AAAA-MM.xlsx` (p. ej. `2026-10.xlsx`).
2. En el repositorio entra a la carpeta **datos** (haz clic en ella).
3. **Add file → Upload files** → arrastra el Excel → **Commit changes**.
4. En 1–2 minutos el mes nuevo aparece en el sitio.

Para **corregir** un mes o actualizar los cumpleaños: sube un archivo con el **mismo nombre** en la misma
carpeta y GitHub lo reemplaza. Si no ves el cambio, recarga con Ctrl+F5.

**Dominio propio (opcional):** lo más barato es pedirle a TI un subdominio del dominio de la empresa
(p. ej. `horarios.tuempresa.com`): es un registro DNS tipo CNAME hacia `USUARIO.github.io`,
y luego se escribe ese dominio en Settings → Pages → Custom domain.

## Detalles útiles

- Enlace directo por equipo: `…/index.html#equipo=converse` (ideal para fijarlo en el grupo de WhatsApp de cada marca).
  El navegador también recuerda el último equipo elegido.
- `index.html` trae julio y septiembre 2026 como **datos de muestra** incrustados, para que funcione
  al abrir el archivo suelto en tu PC. Con el sitio publicado mandan siempre los Excel de `datos/`.
  Cuando quieras, reemplaza esa línea por `const MUESTRA = {equipos:[],meses:{},cumples:[]};`
- Abrir `index.html` con doble clic NO lee los Excel (los navegadores lo bloquean en archivos locales).
  Para probar en tu PC: `python -m http.server` dentro de la carpeta y abre http://localhost:8000
