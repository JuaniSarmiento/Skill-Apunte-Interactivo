# Leer el material

## Primero: ¿el contenido ya está en contexto?

Si el archivo se ve en el contexto (markdown, txt, csv, o un PDF chico que el sistema ya renderizó), no hace falta tocar el disco. Si solo aparece la ruta en `/mnt/user-data/uploads/`, hay que leerlo.

## PDFs de cuaderno escaneado

Casi siempre vienen de fotos hechas con el celular. Son enormes (100-400 MB), sin capa de texto, y con las páginas en cualquier orden.

### Inventario

```bash
pdfinfo archivo.pdf          # cuántas páginas, tamaño
pdffonts archivo.pdf | head  # si no lista fuentes → es imagen pura, no hay texto que extraer
```

Si `pdffonts` no devuelve nada, `pdftotext` no sirve. Hay que rasterizar y mirar.

### Rasterizar

```bash
cd /tmp && pdftoppm -jpeg -r 20 /mnt/user-data/uploads/archivo.pdf /tmp/pg
```

20 DPI suena bajísimo pero alcanza para leer texto impreso de apunte y es lo que permite mirar 24 páginas sin quemar el contexto. Si una página específica no se lee (letra manuscrita chica, foto movida, contraste bajo), rehacé **solo esa** a 40-50 DPI:

```bash
pdftoppm -jpeg -r 45 -f 7 -l 7 archivo.pdf /tmp/pg7
```

### Mirar todas las páginas

Una por una, con `view`. Sin excepción, aunque sean 24. Saltear páginas significa perder un ejemplo resuelto o media unidad del programa: eso se nota en el resultado final.

Mientras se mira, anotar mentalmente:

| Qué buscar | Por qué importa |
|---|---|
| Número de página del cuaderno | Detecta hojas faltantes o dadas vuelta |
| Texto resaltado (cualquier color) | Es lo que va a tomar el profesor |
| Anotaciones manuscritas al margen | La explicación de clase, el truco, el ejemplo del profe |
| Ejemplos resueltos con números | Materia prima de las calculadoras guiadas |
| Tablas y gráficos | Van reproducidos, no descriptos |
| Cambios de tema | Un PDF suele traer varias unidades |
| Nombre en la portada | Es de quién es el cuaderno, no necesariamente de quien lo manda |

### Páginas desordenadas o faltantes

Muy común: la numeración del cuaderno salta (…3, 4, 7, 8…) o dos páginas están invertidas.

- **Desordenadas** → se reordenan silenciosamente al armar la página, siguiendo la numeración del cuaderno.
- **Faltantes** → nunca rellenar con conocimiento general como si estuviera en el apunte. Se hace lo siguiente:
  1. Avisar en el chat qué hojas faltan y qué contenido iría ahí.
  2. Dejar en la página un bloque de aviso visible (`.gap`, borde ámbar) diciendo qué falta y sugiriendo conseguir esas hojas.
  3. Si hace falta un dato de la hoja faltante para que el resto se entienda (una constante que se usa después), se puede mencionar mínimamente, aclarando de dónde sale.

Esto importa: la persona estudia para un examen sobre *ese* programa. Contenido inventado con buena intención le hace estudiar cosas que no entran y le tapa el hueco real.

## Fotos sueltas (JPG, PNG)

Si vienen como imágenes en el contexto, se leen directamente sin tocar el disco. Suelen ser fotos de pizarrón y son **más valiosas que el apunte impreso**: muestran el método exacto que el profesor enseñó, con su notación y sus atajos. Cuando el pizarrón y el apunte difieren en la forma de resolver, gana el pizarrón, y conviene marcarlo así en la página.

## Documentos con texto

Word, PDFs digitales, markdown: se extrae el texto directamente. El resto del flujo es igual, pero suele faltar la capa de resaltado y anotaciones — en ese caso, el criterio de qué destacar lo pone el contenido (definiciones, fórmulas, conclusiones).
