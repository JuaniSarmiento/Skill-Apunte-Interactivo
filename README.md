# apunte-interactivo

Skill de Claude que convierte material de estudio —un PDF escaneado de un cuaderno,
fotos del pizarrón, un documento— en **una sola página HTML autocontenida** que sirve
a la vez de resumen y de herramienta de práctica.

El resultado no es un resumen que se lee: es una página donde la persona **hace cosas**.
Mueve variables y ve qué pasa, resuelve ejercicios que se autocorrigen, juega a
clasificar, y rinde un examen final que le dice qué repasar.

## Qué produce

Un único archivo `.html`, sin dependencias ni build, con:

- El **resumen completo** del tema, en 8 a 12 capítulos
- **Ocho actividades** repartidas dentro del texto: simuladores, calculadoras guiadas,
  juegos de clasificación, flashcards
- Un **examen final** autocorregido que señala qué capítulo repasar

## Los dos criterios que la definen

**Lo resaltado va resaltado.** Lo que la persona marcó con resaltador es lo que el
profesor tomó, y las anotaciones a mano en los márgenes suelen ser la explicación que
dio en clase. Eso vale más que el texto impreso.

**Nada de resumir de más.** Van las definiciones textuales, las tablas completas y los
ejemplos con sus números. Un resumen que omite la mitad obliga a tener el PDF abierto
al lado, y entonces fracasó.

## Instalación

```bash
git clone https://github.com/JuaniSarmiento/Skill-Apunte-Interactivo.git \
  ~/.claude/skills/apunte-interactivo
```

## Estructura

| Archivo | Qué es |
|---|---|
| `SKILL.md` | El flujo y las reglas |
| `references/lectura-material.md` | Cómo leer un PDF escaneado sin perderse páginas |
| `references/actividades.md` | Catálogo de actividades, con el código listo para usar |
| `references/diseno.md` | El sistema visual |
| `assets/plantilla.html` | El molde del que parte cada página |

## Nota

Está escrita para **Claude.ai**: entrega con `present_files` y evita
`localStorage`/`sessionStorage`, porque los artifacts del navegador no los soportan.
El estado vive en variables JavaScript durante la sesión.

## Licencia

Apache-2.0
