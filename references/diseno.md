# Sistema visual

## La idea

Pizarrón y marcadores. Fondo de papel cuadriculado tenue, tinta gris petróleo, y los colores de marcador con **significado fijo** en todo el documento. La página se parece al material del que salió, y eso hace que se sienta propia en lugar de genérica.

## Color del resaltador

El resaltador de la página **coincide con el que la persona usó en el apunte**. Si el cuaderno está resaltado en verde, `--hl` es verde. Si es rosa, rosa. Es un detalle de dos caracteres en el CSS que hace que la página se sienta la continuación de su carpeta y no una plantilla.

```css
--hl:#FBEB8A;   /* amarillo */
--hl:#BEEEB3;   /* verde */
--hl:#F6BEDF;   /* rosa */
--hl:#E0D5F5;   /* violeta */
```

Si el apunte usa dos colores con significados distintos (amarillo para definiciones, rosa para fórmulas), se replican los dos: `<mark>` y `<mark class="p">`.

## Significado de los colores

Se mantiene constante en todo el documento. Cuando el tema tiene su propia convención de color, gana la del tema (en cinética: rojo para desaparición, verde para aparición).

| Color | Variable | Uso |
|---|---|---|
| Rojo | `--red` | advertencias, lo que baja o se consume, límites |
| Azul | `--blue` | definiciones, lo estable, lo que se conserva |
| Verde | `--green` | aciertos, lo que sube o aparece |
| Violeta | `--violet` | actividades, estados intermedios |
| Ámbar | `--amber` | avisos sobre el material (páginas faltantes), casos especiales |

## Tipografía

- **Archivo** para títulos, botones e interfaz. Grotesca, con peso, tracking apenas negativo.
- **Source Serif 4** para el cuerpo, a 18px y línea 1.65. Es texto denso de estudio que se lee durante media hora seguida: una serif se banca mejor esa distancia que una sans.
- **IBM Plex Mono** para fórmulas, datos y valores. En material científico la monoespaciada separa visualmente el dato de la prosa, que es justo lo que se quiere.

Siempre con fallback de sistema, para que el archivo sirva sin internet.

## Bloques de contenido

| Clase | Para qué | Marca visual |
|---|---|---|
| `.def` | definición formal, casi textual del apunte | borde azul izquierdo |
| `.warn` | advertencia, limitación, error frecuente | borde rojo izquierdo |
| `.gap` | aviso de material faltante en el escaneo | borde ámbar izquierdo |
| `.eq` | fórmulas y desarrollos | recuadro, mono, centrado |
| `.card` | agrupar contenido relacionado | borde suave |
| `.card.act` | actividad | borde violeta izquierdo + chapa |
| `.hint` | acotación, imagen mental, dato de color | gris, más chico |

Los bloques son la estructura visual del resumen: una página donde todo el texto se ve igual no se puede escanear con la vista, y en época de parcial se lee salteado.

## Movimiento

**Un solo momento de animación por página**, en el hero: una curva que se dibuja sola, órbitas que giran lento, moléculas que flotan. Nada más se mueve salvo lo que la persona mueve.

Siempre dentro de `@media (prefers-reduced-motion: reduce)` para desactivarlo.

## Lo que no va

- Emojis como iconos de sección.
- Gradientes decorativos, sombras grandes, bordes redondeados de más.
- Barras de colores de lado a lado.
- Tarjetas idénticas repetidas donde bastaba una lista.
- Fondos oscuros: es material para leer largo rato, con buena luz y también en el celular a la noche.

## Serie de páginas

Si la persona ya tiene páginas de otras unidades, la nueva **mantiene el mismo sistema** y solo cambia el color del resaltador y la ilustración del hero. Un conjunto coherente se siente un apunte; cinco diseños distintos se sienten cinco archivos sueltos.
