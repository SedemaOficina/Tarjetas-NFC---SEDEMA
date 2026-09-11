# Contrato de la plantilla

`_plantilla/index.html` es el único archivo donde vive el diseño. El generador
no sabe nada de HTML: solo sustituye marcadores y recorta bloques. Puedes
rediseñar la tarjeta entera siempre que respetes esta sintaxis.

## Sustitución

`{{CAMPO}}` se reemplaza por el valor del campo.

| Marcador | Valor de ejemplo |
|---|---|
| `{{NOMBRE}}` | `Jorge Liber Saltijeral Giles` |
| `{{NOMBRE_HTML}}` | `Jorge Liber<br>Saltijeral Giles` |
| `{{CARGO}}` | `Asesor` |
| `{{AREA}}` | `Oficina de la Secretaría` |
| `{{INICIALES}}` | `JS` |
| `{{TELEFONO}}` | `525553458184` |
| `{{WHATSAPP}}` | `525533312276` |
| `{{CORREO}}` | `nombre@sedema.cdmx.gob.mx` |
| `{{LINKEDIN}}` | URL completa, ya normalizada |
| `{{FACEBOOK}}` | URL completa, ya normalizada |
| `{{INSTAGRAM}}` | URL completa, ya normalizada |
| `{{X}}` | URL completa, ya normalizada |
| `{{SITIO}}` | `https://sedema.cdmx.gob.mx/` |
| `{{URL}}` | URL de la tarjeta, con barra al final |
| `{{ARCHIVO_VCF}}` | `jorge_liber_saltijeral_giles_SEDEMA.vcf` |

`REDES` es un campo calculado: trae valor si la persona tiene al menos una red
social. No se sustituye en ningún lado —solo existe para que
`<!--SI:REDES-->` pueda envolver la fila completa, con su título incluido, y
hacerla desaparecer cuando no hay ninguna. Si agregas una red nueva, súmala
también a esa condición en `02_Datos.gs`.

Los teléfonos llegan ya normalizados a formato internacional sin `+`, listos
para `wa.me/{{WHATSAPP}}` y `tel:{{TELEFONO}}`.

## Bloques condicionales

```html
<!--SI:LINKEDIN-->
  <a class="btn" href="{{LINKEDIN}}">LinkedIn</a>
<!--/SI:LINKEDIN-->

<!--NO:FOTO-->
  <div class="avatar-iniciales">{{INICIALES}}</div>
<!--/NO:FOTO-->
```

- `SI:` conserva el contenido si el campo trae valor.
- `NO:` lo conserva si el campo está vacío.

Es lo que garantiza que nadie reciba un botón visible y muerto: si la persona
no tiene LinkedIn, el enlace no se genera. Los bloques pueden anidarse en
cualquier parte del documento, incluido el `<head>` (así se omite `og:image`
cuando no hay fotografía).

## Reglas que el generador impone

1. Si queda un `{{MARCADOR}}` sin resolver, **la publicación se detiene** con
   un error que lo nombra. Ningún marcador llega a producción.
2. Si queda un bloque `<!--SI:...-->` sin su cierre, también se detiene.
3. Para agregar un campo nuevo: añadir la columna en `00_Config.gs` (`COL`),
   leerla en `leerFila_` de `02_Datos.gs` con la clave **en mayúsculas**, y
   usarla en la plantilla. El motor la reconoce sola.

## Reglas de diseño que no conviene romper

- **Rutas de assets:** siempre `../assets/…` — la plantilla se renderiza
  dentro de la carpeta de cada persona.
- **Estado base visible:** las animaciones de entrada viven dentro de
  `@media (prefers-reduced-motion: no-preference)`. Si se saca el
  `opacity: 0` de ahí y el navegador ignora la animación, la tarjeta se abre
  en blanco. Fue el riesgo de la versión anterior.
- **Sin `user-scalable=no`:** el zoom se dejó habilitado. Una tarjeta de
  contacto tiene que poder acercarse.
- **`noindex`:** los datos de contacto del personal no deben indexarse.
- **Rejilla elástica:** los botones secundarios van dentro de `.grid-auto`,
  no en una rejilla fija de dos columnas. Así, quitar uno no deja hueco.

## Íconos de las redes sociales

Las píldoras salen con el nombre en texto. Para ponerles el logotipo de cada
plataforma, el camino correcto es el kit oficial de marca de cada una, o un set
de íconos con licencia que ya los incluya.

**Tu tarjeta ya usa uno.** Los íconos de WhatsApp, correo, teléfono y del botón
de sitio web vienen de **Phosphor Icons** —se reconocen por el `viewBox="0 0 256
256"`—, que es de licencia MIT y trae las cuatro marcas que necesitas:

| Red | Nombre del ícono en Phosphor |
|---|---|
| LinkedIn | `linkedin-logo` |
| Facebook | `facebook-logo` |
| Instagram | `instagram-logo` |
| X | `x-logo` |

Cómo ponerlos, una vez por red (medio minuto cada una):

1. Entra a **phosphoricons.com**, busca el nombre de la tabla y elige el peso
   **Fill** para que combine con los demás botones.
2. Copia el SVG.
3. Pégalo dentro del bloque de esa red en `_plantilla/index.html`, donde dice
   «Ícono oficial opcional», y agrégale `class="social-icon"`.
4. Publica de nuevo desde la hoja: las tarjetas de todas las personas se
   actualizan de una vez.

```html
<!--SI:FACEBOOK-->
<a class="social-btn" href="{{FACEBOOK}}" target="_blank" rel="noopener">
  <svg class="social-icon" viewBox="0 0 256 256"><path d="…"/></svg>
  Facebook
</a>
<!--/SI:FACEBOOK-->
```

No hace falta quitarle el `fill` al SVG: la hoja de estilos fuerza el color
guinda del botón. Y el ancho de la píldora se ajusta solo, así que el ícono no
descuadra la rejilla.
