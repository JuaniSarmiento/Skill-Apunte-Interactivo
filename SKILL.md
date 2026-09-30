---
name: apunte-interactivo
description: Convierte apuntes de clase, PDFs escaneados de cuadernos, fotos de pizarrón o cualquier material de estudio en una página web HTML interactiva de una sola pieza, con el resumen completo del tema más simuladores, calculadoras guiadas, juegos de clasificación, flashcards y un examen final autocorregido. Usala SIEMPRE que alguien suba un PDF, foto o documento de estudio y pida un resumen, un resumen interactivo, una página para estudiar, material para preparar un parcial o final, "algo para entender bien el tema", o pida convertir sus apuntes en algo con lo que pueda practicar. Usala también si piden "lo mismo que la vez pasada" sobre un apunte nuevo, o si piden ejercicios o actividades sobre el material que subieron. Aunque no digan la palabra "interactivo", si suben material de estudio y quieren repasarlo, esta skill es la respuesta.
license: Apache-2.0
metadata:
  author: juanisarmiento
  version: "1.0"
---

# Apunte interactivo

Convierte material de estudio (PDF escaneado, fotos, documentos) en **una sola página HTML autocontenida** que sirve como resumen completo y como herramienta de práctica.

El resultado no es un resumen que se lee: es una página donde la persona **hace cosas** — mueve variables y ve qué pasa, resuelve ejercicios que se autocorrigen, juega a clasificar, y rinde un examen final que le dice qué repasar.

## Flujo de trabajo

1. **Leer el material completo** → `references/lectura-material.md`
2. **Mapear el contenido** en capítulos y detectar qué está resaltado
3. **Elegir las actividades** → `references/actividades.md`
4. **Escribir la página** partiendo de `assets/plantilla.html`
5. **Entregar** el archivo con `present_files` y explicar en el chat qué actividades tiene

No saltear el paso 1. Leer el material entero antes de escribir una línea de HTML: el orden de los capítulos, los ejemplos numéricos y las frases resaltadas salen de ahí, no de conocimiento general del tema.

## 1. Leer el material

Leé `references/lectura-material.md` antes de tocar el PDF. Resume el procedimiento: rasterizar a baja resolución y mirar **todas** las páginas, una por una.

Puntos que siempre hay que chequear mientras se lee:

- **Qué está resaltado.** Lo que la persona marcó con resaltador es lo que el profesor tomó. Va resaltado en la página también, con `<mark>`.
- **Anotaciones a mano en los márgenes.** Suelen ser la explicación que dio el profesor en clase, o el truco para resolver. Valen oro y casi siempre son lo que falta en el texto impreso.
- **Páginas faltantes o desordenadas.** Los escaneos de cuaderno vienen con hojas dadas vuelta o ausentes. Verificá la numeración. Si falta algo, **decilo en el chat y dejá un aviso visible en la página** en lugar de rellenar el hueco con conocimiento propio.
- **Ejemplos numéricos resueltos.** Son la base de las calculadoras guiadas: hay que usar esos números, no inventar otros.
- **Más de un tema en el mismo PDF.** Es habitual. Se hace una sola página con todos, en el orden en que vienen.

## 2. Mapear el contenido

Antes de escribir, armá mentalmente (o en un borrador) la lista de capítulos. Criterios:

- **El orden del apunte manda**, salvo que la persona pida otro orden explícitamente.
- Entre 8 y 12 capítulos. Menos se siente pobre; más se vuelve inmanejable en el índice lateral.
- Cada capítulo lleva su número y un título corto para el índice.
- El último capítulo siempre es el **examen final**.

**Nada de resumir de más.** El objetivo es que la persona no tenga que volver al PDF: van las definiciones textuales resaltadas, las tablas completas, los ejemplos resueltos con sus números, las conclusiones. Un resumen que omite la mitad obliga a tener las dos cosas abiertas y fracasa.

Cómo tratar el texto del apunte:

- Las **definiciones formales** van casi textuales, dentro de un bloque `.def` y con `<mark>`.
- La **explicación de por qué** puede reescribirse en lenguaje llano, más directo que el original.
- Las **advertencias** ("ojo con", "no confundir", limitaciones de una ley) van en bloque `.warn`.
- Las **imágenes mentales** que dio el profesor (el globo que se aplasta, la olla a presión, la piedra en la lomita) se conservan: son lo que hace que el concepto se pegue.

## 3. Elegir las actividades

Leé `references/actividades.md` para el catálogo con código listo para usar.

**Ocho actividades** es el número que funciona: suficiente para cubrir el tema, poco para no agotar. Más el examen final.

Reglas para elegir:

- **Una actividad por concepto difícil**, no una por capítulo. Los capítulos descriptivos (historia, clasificaciones) se cubren con un juego de emparejar o clasificar; los capítulos con cuentas llevan calculadora guiada.
- **La actividad tiene que enseñar algo que el texto no puede.** Un slider que muestra cómo una curva cambia de forma enseña algo que tres párrafos no. Un botón que dice "correcto" sin explicar por qué, no.
- **Todo feedback explica.** Nunca "incorrecto" solo: siempre por qué, y qué regla se aplicaba. El feedback del error es donde se aprende.
- **Priorizá el paso donde la gente se traba.** Si en el apunte hay un método de varios pasos, la actividad tiene que hacerle practicar *el paso difícil* aislado, no el procedimiento entero. (Ejemplo real: en ley de velocidad lo difícil no es la cuenta, es elegir qué par de experiencias comparar. La actividad hace elegir el par primero y explica por qué el par equivocado no sirve.)
- **Los datos salen del apunte.** Los ejemplos resueltos de la clase son los mejores ejercicios porque la persona los puede contrastar con su carpeta.

Además de las ocho: **flashcards** para los datos de memorizar (constantes, valores, fórmulas cortas) y el **examen final** de 12 a 15 preguntas de opción múltiple que al terminar dice qué capítulos conviene repasar según el puntaje.

## 4. Escribir la página

Partí de `assets/plantilla.html`. Trae el sistema de diseño completo, la navegación lateral, la barra de progreso y las funciones JavaScript que usan todas las actividades. Se completa, no se reescribe.

### Estructura fija

```
hero (título + bajada + ilustración SVG animada)
índice lateral pegajoso + barra de progreso
capítulos 1..n
  tag de capítulo, h2, prosa, bloques .def/.warn/.eq/tablas
  tarjeta .card.act con la actividad cuando corresponde
examen final
bloque "tu recorrido" con el progreso
footer con la procedencia del material
```

### Reglas técnicas que no se negocian

- **Un solo archivo .html**, sin dependencias salvo Google Fonts. Tiene que funcionar abriéndolo con doble clic, sin internet y sin servidor. Si no hay conexión, las tipografías caen a las del sistema y todo lo demás sigue andando.
- **Nada de localStorage, sessionStorage ni window.storage.** El estado vive en variables JavaScript durante la sesión. Los artifacts de Claude.ai no soportan almacenamiento del navegador.
- **SVG inline** para todos los gráficos, dibujos y simuladores. Nada de imágenes externas ni librerías.
- **Responsive de verdad.** El índice lateral se convierte en una fila de chips horizontales arriba en pantallas angostas. Se estudia desde el celular.
- **Controles de 44×44 px como mínimo.** Todo botón, chip, opción de quiz, link del índice, slider o casilla tiene un área táctil de al menos 44×44 px, también a 380 px de ancho (la plantilla ya trae la regla; no la pises con alturas fijas menores).
- **Fórmulas en MathML, cada una envuelta en `<span class="mw">…</span>`** (la plantilla trae el CSS: se desplaza sola si es ancha). `<math>` solo no se comporta como contenedor de desplazamiento y desborda a 320 px.
- **Accesible.** `role="img"` y `aria-label` en cada SVG, `:focus-visible` visible, y respetar `prefers-reduced-motion` en toda animación.

### Sistema visual

Leé `references/diseno.md`. En resumen: estética de pizarrón y marcadores, donde los colores tienen significado fijo, y el color del resaltador de la página coincide con el que la persona usó en el apunte.

### Idioma

La página se escribe **en el idioma del apunte y de la persona**. Si el material está en español rioplatense, la página también: voseo, "tenés", "fijate", sin tuteo español. El registro es el de alguien explicándole a un compañero, no el de un manual.

## 5. Entregar

Guardá el archivo en el directorio de salida con un nombre descriptivo del tema (`cinetica-quimica.html`, no `resumen.html`) y presentalo con `present_files`.

En el chat, después del archivo:

- Listá las actividades en una línea cada una, diciendo **qué se hace** en cada una, no cómo se llama.
- Mencioná cualquier **hueco o problema del material** que hayas detectado (hojas faltantes, páginas desordenadas, un dato que no cierra).
- Si hay **algo del apunte que suele confundir** y quedó marcado en la página, señalalo. Es la información más útil que podés dar.

Nada de explicar el proceso ni la estructura del HTML. Lo que importa es qué puede hacer con la página.

## Si piden otra cosa a partir de lo mismo

- **"Cómo se la paso a alguien"** → el archivo se manda tal cual y anda con doble clic. La interactividad es JavaScript del navegador, no necesita servidor. Para un link: cualquier hosting estático, o el VPS propio si lo tienen.
- **"Agregale el tema nuevo"** → si el tema continúa al anterior, ofrecé una página nueva que se encadene, no una gigante. Cuatro páginas de 10 capítulos se estudian mejor que una de 40.
- **"Sacá las tipografías de internet"** → incrustar las fuentes en base64 dentro del archivo, o pasar a stack de sistema.
