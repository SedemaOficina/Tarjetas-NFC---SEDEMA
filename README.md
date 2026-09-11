# Tarjetas digitales NFC — Oficina de la Secretaría, SEDEMA

Monorepo de las tarjetas de presentación NFC del personal. Un repositorio,
un dominio, una carpeta por persona y un generador que crea cada tarjeta
desde un renglón de Google Sheets.

---

## 1. Qué cambió respecto al esquema anterior

| | Antes | Ahora |
|---|---|---|
| Repositorios | Uno por persona | Uno solo |
| Cambiar el diseño | Editar N repositorios | Editar `_plantilla/index.html` y republicar |
| Peso de la tarjeta | **5.0 MB** | **156 KB** en total, y los assets se cachean entre tarjetas |
| Alta de una persona | A mano, copiando carpetas | Un renglón en la hoja y un clic en el menú |
| Botón sin dato | Quedaba visible y muerto | El bloque desaparece del HTML publicado |
| Sin fotografía | Recuadro roto | Iniciales sobre fondo guinda |
| Redes sociales | Solo LinkedIn, fijo en el código | LinkedIn, Facebook, Instagram y X, opcionales una por una |

### Optimización de assets aplicada

| Archivo | Antes | Ahora |
|---|---|---|
| `logo.png` | 1 643 KB | 11 KB |
| `corazon.png` | 467 KB | 31 KB |
| `patron_rojo.jpg` | 246 KB | 27 KB |
| `latido.mp3` | 2 512 KB (64 s, estéreo, 320 kbps) | 6 KB (un ciclo cardiaco, mono, 48 kbps) |
| `foto.jpg` | 57 KB | 27 KB |

El audio además pasó a `preload="none"`: antes se descargaban 2.5 MB
aunque nadie tocara el corazón.

---

## 2. Estructura

```
tarjetas/
├── CNAME                    dominio propio (crear al comprarlo)
├── .nojekyll                impide que GitHub ignore carpetas con _
├── index.html               directorio interno, lo genera el script
├── assets/                  compartido por todas las tarjetas
│   ├── styles.css
│   ├── logo.png
│   ├── corazon.png
│   ├── patron_rojo.jpg
│   └── latido.mp3
├── _plantilla/
│   └── index.html           LA plantilla. Aquí se edita el diseño.
└── liber-saltijeral/        una carpeta por persona
    ├── index.html           generado
    ├── contacto.vcf         generado
    └── foto.jpg             generado desde Drive
```

Regla: **nunca se edita a mano el `index.html` de una persona.** Si hay que
cambiar algo, se cambia la plantilla o el renglón de la hoja y se republica.

---

## 3. Instalación (una sola vez)

### 3.1 Repositorio

Los pasos con clics están en **`SUBIR-AL-REPO.md`**. En resumen:

1. Crear `SedemaOficina/tarjetas` en GitHub, **público**.
2. Subir el contenido de este paquete **sin la carpeta `apps-script/`**
   (esa carpeta va al editor de Apps Script, no al repositorio), arrastrando
   las carpetas cerradas para que la estructura se conserve.
3. `Settings → Pages → Deploy from a branch → main / (root)`.
4. Cuando exista el dominio propio: crear el archivo `CNAME` con el dominio
   dentro y activar `Enforce HTTPS`.

### 3.2 Token de GitHub

1. GitHub → `Settings → Developer settings → Personal access tokens →
   Fine-grained tokens → Generate new token`.
2. Repository access: **Only select repositories** → `SedemaOficina/tarjetas`.
3. Permisos: **Contents → Read and write**. Nada más.
4. Vigencia: 1 año, con recordatorio en el calendario.
5. Copiar el token. Solo se muestra una vez.

### 3.3 Hoja y script

1. Crear una hoja de cálculo nueva llamada *Tarjetas NFC SEDEMA*.
2. `Extensiones → Apps Script`.
3. Crear un archivo por cada `.gs` de la carpeta `apps-script/`, con el mismo
   nombre, y pegar el contenido.
4. En `00_Config.gs` ajustar `OWNER`, `REPO` y `DOMINIO`.
5. Guardar, recargar la hoja: aparece el menú **Tarjetas NFC**.
6. `Tarjetas NFC → Preparar hoja (encabezados)`.
7. `Tarjetas NFC → Configurar token de GitHub` y pegar el token.
8. `Tarjetas NFC → Verificar conexión`.

---

## 4. Alta de una tarjeta

1. Llenar un renglón en la hoja `Tarjetas`.
2. Dejar el cursor en ese renglón.
3. `Tarjetas NFC → Publicar la fila seleccionada`.

El script sube en **un solo commit**: `index.html`, `contacto.vcf` y la
fotografía. Si algo falla, no queda nada a medio publicar.

### Columnas

| Col | Campo | Obligatorio | Nota |
|---|---|---|---|
| A | Slug | no | Se autocompleta desde el nombre. Se puede acortar a mano. |
| B | Nombre completo | **sí** | Usa `\|` para forzar el salto de línea del título: `Jorge Liber\|Saltijeral Giles`. |
| C | Nombre de pila | **sí** | Para la vCard y las iniciales. |
| D | Apellidos | **sí** | Para la vCard y las iniciales. |
| E | Cargo | **sí** | Etiqueta dorada y campo `TITLE:` de la vCard. |
| F | Área | **sí** | |
| G | Teléfono | no | 10 dígitos. |
| H | WhatsApp | no | 10 dígitos. |
| I | Correo | no | |
| J | LinkedIn | no | URL completa, o `in/usuario`. |
| K | Facebook | no | `usuario`, `@usuario` o la URL completa. |
| L | Instagram | no | `usuario`, `@usuario` o la URL completa. |
| M | X | no | `usuario`, `@usuario` o la URL completa. |
| N | Foto (Drive) | no | Enlace o ID del archivo. |
| O | Estado | **sí** | `Activa` o `Baja`. |
| P | URL | auto | |
| Q | Publicado | auto | |
| R | Notas | no | |

**Toda columna opcional vacía hace desaparecer su botón.** No queda un enlace
muerto ni un hueco en la maquetación:

- Sin fotografía → círculo guinda con las iniciales.
- Sin correo, sin teléfono o sin WhatsApp → ese botón no se genera y los demás
  se reacomodan en la rejilla elástica.
- Sin ninguna red social → desaparece la fila completa, incluido el título
  «También me encuentras en».
- Con una, dos, tres o cuatro redes → aparecen solo esas, sin celdas vacías:
  una se centra a media anchura, dos y cuatro se acomodan en dos columnas, y
  tres caben en un solo renglón.
- Con un número impar de botones de contacto, el último ocupa el ancho
  completo en vez de dejar media celda en blanco.

Las redes se pueden capturar como el usuario a secas, con arroba o con la URL
completa: el generador arma el enlace correcto en los tres casos.

**Fotografías:** cuadradas, 300×300 px, menos de 150 KB. El archivo de Drive
debe estar compartido con la cuenta que corre el script.

---

## 5. Baja de una persona

Cambiar la columna **Estado** a `Baja` y publicar. El script sustituye su
`index.html` por una redirección al sitio institucional. La URL sigue viva:
las tarjetas en circulación no dan 404 y ya no exponen datos de alguien que
ya no está.

**Nunca se borra la carpeta.**

---

## 6. Cambiar el diseño de todas las tarjetas

1. Editar `_plantilla/index.html` o `assets/styles.css`.
2. `Tarjetas NFC → Publicar todas las pendientes`.

Cambiar de dominio es el mismo movimiento: se ajusta `CONFIG.DOMINIO`, se
republica todo y las URL internas quedan actualizadas.

---

## 7. Pendientes

- [ ] Comprar el dominio y crear el archivo `CNAME`.
- [ ] Autoalojar Cabin y Roboto en `assets/fonts/` para eliminar la petición
      a Google Fonts (hoy es el único recurso externo de la tarjeta).
- [ ] Convertir las imágenes a WebP con respaldo en PNG/JPG.
- [ ] Migrar las tarjetas existentes a la hoja y publicarlas.
