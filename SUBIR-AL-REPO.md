# Cómo subir este paquete a GitHub

Pasos exactos, desde el navegador. No hace falta instalar nada.

---

## Antes de empezar

1. Descomprime `tarjetas-nfc-sedema.zip`.
2. **Saca la carpeta `apps-script/` fuera** de la carpeta descomprimida.
   No va al repositorio: su contenido se pega en el editor de Apps Script.

Al terminar debes ver exactamente estos siete elementos:

```
assets                    (carpeta)
_plantilla                (carpeta)
liber-saltijeral          (carpeta)
README.md
SUBIR-AL-REPO.md
CONTRATO-PLANTILLA.md
NOTAS-DE-REVISION.md
```

Más `.nojekyll`, que el explorador esconde por empezar con punto.

---

## 1. Vaciar el repositorio actual

El repositorio `SedemaOficina/tarjetas` quedó con los archivos sueltos en la
raíz y un `index (1).html` duplicado. Tiene un solo commit, así que lo más
limpio es borrarlo y volver a crearlo.

1. Entra al repositorio → **Settings**.
2. Hasta abajo, zona roja → **Delete this repository**.
3. Escribe `SedemaOficina/tarjetas` para confirmar.

---

## 2. Crearlo otra vez

1. Organización **SedemaOficina** → **Repositories** → **New repository**.
2. Nombre: `tarjetas` · Descripción: «Tarjetas de presentación digitales NFC».
3. Visibilidad: **Public**. GitHub Pages no publica desde repositorios privados
   en planes gratuitos.
4. **No marcar** «Add a README», ni `.gitignore`, ni licencia.
5. **Create repository**.

---

## 3. Subir — el paso donde falló la vez pasada

En la pantalla del repositorio vacío, clic en **«uploading an existing file»**.

**No uses el botón «choose your files».** Ese solo acepta archivos sueltos: es
lo que aplanó la estructura y provocó que los dos `index.html` chocaran.

**Arrastra las tres carpetas cerradas** —`assets`, `_plantilla`,
`liber-saltijeral`— junto con los cuatro `.md`, todo de un jalón, sobre la zona
punteada. Sin abrirlas.

### Verificación obligatoria antes de confirmar

Mira la lista de archivos por subir. Tiene que mostrar **rutas con diagonal**:

```
assets/styles.css
assets/logo.png
assets/corazon.png
assets/patron_rojo.jpg
assets/latido.mp3
_plantilla/index.html
liber-saltijeral/index.html
liber-saltijeral/foto.jpg
liber-saltijeral/contacto.vcf
README.md
SUBIR-AL-REPO.md
CONTRATO-PLANTILLA.md
NOTAS-DE-REVISION.md
```

Si ves nombres sueltos sin diagonal, **cancela y vuelve a arrastrar**. No
confirmes el commit.

Mensaje del commit: «Estructura inicial del monorepo de tarjetas» →
**Commit changes**.

### Si el arrastre no funciona

Safari no maneja bien la carga de carpetas: usa Chrome o Edge. Y si aun así se
resiste, dentro del repositorio presiona la tecla **`.`** (punto): se abre el
editor de VS Code en el navegador, arrastras las carpetas al árbol de la
izquierda y confirmas todo desde el panel de Source Control. Ahí la estructura
se respeta siempre, porque la ves mientras la armas.

---

## 4. Crear `.nojekyll`

Empieza con punto, el explorador lo esconde y el arrastre no lo sube.

1. **Add file → Create new file**.
2. Nombre: `.nojekyll`.
3. Contenido vacío. Si el navegador no deja guardar vacío, escribe un guion.
4. **Commit changes**.

Sin este archivo, GitHub Pages procesa el sitio con Jekyll, que ignora toda
carpeta que empiece con guion bajo. `_plantilla` es una de ellas.

---

## 5. Activar GitHub Pages

1. **Settings** → menú lateral **Pages**.
2. Source: **Deploy from a branch**.
3. Branch: `main` · Carpeta: `/ (root)` → **Save**.
4. La compilación tarda de uno a tres minutos. Se sigue en la pestaña
   **Actions**.

La tarjeta queda en:

```
https://sedemaoficina.github.io/tarjetas/liber-saltijeral/
```

Provisional, para probar. No se graba en ningún chip: eso espera al dominio
propio.

---

## 6. Comprobar en el teléfono

Con datos móviles, no con WiFi de oficina:

- [ ] La página carga completa
- [ ] **Guardar Contacto** abre la agenda en iOS
- [ ] **Guardar Contacto** abre la agenda en Android
- [ ] En el contacto importado, «Asesor» aparece como cargo
- [ ] Los acentos se ven bien: «Secretaría», no `SecretarÃ­a`
- [ ] WhatsApp, Correo, Llamar y LinkedIn abren donde deben
- [ ] El corazón late y suena
- [ ] El zoom con dos dedos funciona

---

## Después

La hoja de control y el generador de Apps Script vienen en el `README.md`,
sección 3.3. Una vez instalados, no vuelves a subir archivos a mano: capturas
un renglón y publicas desde el menú.
