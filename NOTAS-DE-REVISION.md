# Revisión de la tarjeta modelo

Hallazgos sobre `Liber_Saltijeral-main` y qué se hizo con cada uno.

## Corregidos en esta versión

**1. Peso: 5.0 MB.** En una red de evento son decenas de segundos. Assets
optimizados a 156 KB en total (detalle en el README). El `latido.mp3` eran
64 segundos en estéreo a 320 kbps para reproducir un latido: se recortó a un
ciclo cardiaco en mono.

**2. El patrón del fondo nunca se animaba.** `.hero` declara
`animation: moveBackground` y la clase `.animate-in`, definida después en la
hoja, declara `animation: fadeIn`. Al ser ambos selectores de una clase, gana
el último: `moveBackground` quedaba anulado. Ahora las dos animaciones se
declaran juntas en la misma regla.

**3. La tarjeta podía abrirse en blanco.** Todos los elementos partían de
`opacity: 0` y solo los revelaba la animación. Si el navegador la ignoraba
—`prefers-reduced-motion`, un fallo de carga del CSS—, no se veía nada. El
estado base ahora es visible y las animaciones viven dentro de
`@media (prefers-reduced-motion: no-preference)`.

**4. Zoom bloqueado.** `maximum-scale=1, user-scalable=no` impedía acercar la
pantalla. Retirado.

**5. Audio de 2.5 MB con `preload="auto"`.** Se descargaba siempre, tocara o
no el corazón. Ahora `preload="none"`.

**6. Correo sin sustituir.** El botón apuntaba a
`mailto:tucorreo@sedema.cdmx.gob.mx`. Es exactamente el error que el
generador ya no permite: un marcador sin resolver detiene la publicación.

**7. vCard con el cargo en el campo equivocado.**
`ORG:Asesor. Secretaria Del Medio Ambiente…` mezclaba cargo y dependencia en
un solo campo, y "Secretaría" iba sin acento. Ahora `ORG:` lleva la
dependencia y el área, y el cargo va en `TITLE:`, que es donde las agendas de
iOS y Android lo muestran.

**8. Teléfonos con espacios en la vCard.** `55 5345 8184` no siempre marca
bien desde la agenda. Ahora en formato internacional: `+525553458184`.

**9. Faltaban `noindex`, `theme-color`, `canonical` y Open Graph.** Los datos
de contacto del personal eran indexables, y al compartir el enlace por
WhatsApp aparecía la URL sin previsualización. Resuelto.

**10. Sin respaldo cuando no hay fotografía.** Se agregó el bloque de
iniciales sobre fondo guinda.

**11. Accesibilidad.** Se agregaron `alt` descriptivos, `aria-hidden` en los
íconos decorativos, `width`/`height` en las imágenes —evita el salto de
maquetación mientras cargan— y foco visible para navegación por teclado.

## Pendiente, requiere decisión

**Fuentes de Google.** Cabin y Roboto se siguen cargando desde
`fonts.googleapis.com`: es el único recurso externo que queda. Autoalojarlas
en `assets/fonts/` elimina dos peticiones y hace la tarjeta inmune a redes
que filtren Google. No se hizo aquí porque este entorno no tiene salida a ese
dominio; se descargan en local y se copian a `assets/fonts/`.

**Correo institucional.** La vCard de la tarjeta modelo trae
`liber.sedema@gmail.com`. Para tarjetas institucionales conviene el correo de
dominio de la Secretaría.
