# Catálogo de actividades

Todas usan los helpers de `assets/plantilla.html`. Cada actividad llama a `done('id')` cuando se completa, para que avance la barra de progreso.

Contenido:
1. Simulador de variables
2. Calculadora guiada paso a paso
3. Elegir el camino antes de calcular
4. Clasificador
5. Emparejar
6. Ordenar una secuencia
7. Comparador con selector
8. Constructor encadenado
9. Simulación con partículas
10. Verdadero o falso
11. Opción múltiple suelta
12. Flashcards
13. Examen final

---

## 1. Simulador de variables

**Para qué**: relaciones entre magnitudes (leyes físicas, curvas, diagramas de energía). Es la actividad que más rinde: enseña de un tirón lo que tres párrafos no logran.

**Cómo**: uno o dos sliders redibujan un SVG en tiempo real. El texto de feedback debajo **cambia según el valor**, describiendo el régimen en el que está.

```javascript
function drawE(){
  const ea=+document.getElementById('ea').value;
  // ... calcular geometría a partir de ea
  let s = '<line .../>';                       // ejes
  s += '<path d="'+curva(...)+'" fill="none" stroke="var(--ink)" stroke-width="2.6"/>';
  document.getElementById('svg').innerHTML = s;
  const fb=document.getElementById('fb');
  fb.className='fb '+(exo?'good':'info')+' show';
  fb.innerHTML = exo ? '<b>Exotérmica.</b> Los productos quedan más abajo…'
                     : '<b>Endotérmica.</b> Los productos guardaron energía…';
  if(algoQueDemuestraQueExploro) done('sim');
}
```

Detalles que lo hacen bueno:
- Checkboxes para mostrar u ocultar cada anotación del gráfico (cada magnitud marcada).
- El feedback nombra el fenómeno, no describe el dibujo.
- `done()` se dispara cuando exploró de verdad (probó los tres procesos, tildó el catalizador), no al primer movimiento.

## 2. Calculadora guiada paso a paso

**Para qué**: procedimientos de varios pasos con cuentas. Reemplaza al ejercicio resuelto del apunte por uno que la persona resuelve.

**Cómo**: una lista de pasos, cada uno con input, botón "Comprobar" y botón "Pista". Tolerancia relativa al comparar (2 %). Botón abajo para regenerar con números nuevos.

```javascript
const st=[{q:'1. Velocidad de desaparición de B (M·s⁻¹)', v:()=>P.vb,
           hint:'V<sub>B</sub> = −([B]<sub>f</sub> − [B]<sub>i</sub>) / t. El menos adelante hace que dé positivo.'}];
// al comprobar:
if(Math.abs(val-tgt) <= Math.max(tgt*0.02, 0.0005)) { /* correcto */ }
else if(Math.abs(Math.abs(val)-tgt) <= tgt*0.02) { /* número bien, signo mal → mensaje específico */ }
```

El caso "número bien, signo mal" merece su propio mensaje. Ese tipo de feedback quirúrgico es lo que diferencia una actividad útil de un formulario.

## 3. Elegir el camino antes de calcular

**Para qué**: cuando lo difícil no es la cuenta sino **decidir qué hacer**. Es la actividad más valiosa de todas y la que menos se le ocurre a nadie.

**Cómo**: se muestran los datos (tabla de experiencias, enunciado) y primero se elige *qué comparar o qué método aplicar*. Recién cuando la elección es correcta se habilita la parte de calcular. Si elige mal, se explica **por qué ese camino no lleva a ningún lado**.

```javascript
if(sameB && !sameA){
  fb.innerHTML='Ese es el par. En exp '+i+' y exp '+j+' la <b>[B] es la misma</b>, '+
               'así que al dividir se anula y te queda una sola incógnita.';
  document.getElementById('paso2').style.display='block';
} else {
  fb.innerHTML='Ahí cambian las dos concentraciones a la vez: te quedan dos incógnitas '+
               'en una sola ecuación y no se puede despejar.';
}
```

## 4. Clasificador

**Para qué**: casos que hay que asignar a una categoría (qué ley aplica, qué factor es, metal o no metal, qué tipo de fuerza).

**Cómo**: cada caso es una frase con N botones debajo. Al elegir, se bloquean todos, se pinta el correcto en verde y el elegido en rojo si falló, y se explica. `done()` al completar todos.

Los casos salen del apunte. Los que el profesor usó como ejemplo son los que van a estar en el parcial.

## 5. Emparejar

**Para qué**: personas con descubrimientos, términos con definiciones, símbolos con nombres.

**Cómo**: dos columnas, la derecha barajada. Se toca uno de la izquierda, después el de la derecha. Acierto → los dos quedan verdes y fijos. Error → parpadeo rojo y sigue. Es tolerante a propósito: el objetivo es que termine sabiéndolos, no medirlo.

## 6. Ordenar una secuencia

**Para qué**: rankings (radios iónicos, energías de enlace, orden de llenado, cronologías).

**Cómo**: dos zonas. Arriba los elementos barajados, abajo el orden que va armando. Cada toque mueve uno de arriba a abajo. Al completarse se juzga y se muestra la secuencia correcta con los valores reales.

## 7. Comparador con selector

**Para qué**: varias cosas del mismo tipo que se contrastan entre sí (tres tipos de rayos, cuatro modelos atómicos, tres estados).

**Cómo**: una fila de botones tipo `.sw`; cada uno redibuja un SVG y cambia el texto explicativo. `done()` cuando los vio todos:

```javascript
if(!window._vistos) window._vistos=new Set();
window._vistos.add(i);
if(window._vistos.size===total) done('comp');
```

Obliga suavemente a recorrerlos todos, que es exactamente lo que se quiere.

## 8. Constructor encadenado

**Para qué**: sistemas de valores donde cada elección restringe la siguiente (números cuánticos es el caso perfecto).

**Cómo**: fila de chips por cada nivel. Al elegir el primero, se **generan** las opciones válidas del segundo y solo esas. Lo que no se puede elegir, no aparece. La restricción se aprende usándola, sin memorizar la regla.

Cada vez que se elige algo, el feedback dice qué implica esa elección ("el nivel n = 3 tiene 3 subniveles y hasta 2n² = 18 electrones").

## 9. Simulación con partículas

**Para qué**: experimentos históricos y fenómenos estadísticos (dispersión de partículas, choques, difusión).

**Cómo**: `requestAnimationFrame`, partículas como objetos con posición y velocidad, redibujando el SVG entero cada cuadro. Un contador acumula resultados y, al pasar un mínimo razonable de repeticiones, aparece la conclusión con los porcentajes obtenidos.

```javascript
function step(){
  parts.forEach(p=>{ p.x+=p.vx; p.y+=p.vy; if(p.x>590) p.done=true; });
  document.getElementById('svg').innerHTML = base() + parts.filter(p=>!p.done).map(dibujar).join('');
  parts=parts.filter(p=>!p.done);
  if(parts.length) raf=requestAnimationFrame(step); else raf=null;
}
```

Lo potente es que la conclusión **la saca de sus propios números**, no de un texto. Con 40 tiros ve que la enorme mayoría pasa derecho y eso le queda.

## 10. Verdadero o falso

**Para qué**: desarmar confusiones típicas. Cada afirmación falsa tiene que ser una que la gente realmente cree.

**Cómo**: dos botones por afirmación, explicación siempre, sea acierto o error.

## 11. Opción múltiple suelta

**Para qué**: una pregunta puntual insertada en medio de un capítulo, para cortar la lectura pasiva. Usar `mcq()` o `mcqList()` de la plantilla.

## 12. Flashcards

**Para qué**: datos de memoria pura: constantes, valores, equivalencias, fórmulas de una línea.

**Cómo**: grilla de tarjetas que giran al tocarlas, pregunta adelante y respuesta atrás. Diez o doce. No cuentan para el progreso: son de repaso libre.

## 13. Examen final

12 a 15 preguntas de opción múltiple que cubran todos los capítulos, con puntos indicadores arriba que se van pintando. Al terminar, un cierre que **cambia según el puntaje** y dice qué capítulos o actividades conviene rehacer. Ese cierre es lo que convierte el examen en un diagnóstico.

Las opciones incorrectas tienen que ser **errores plausibles**, no rellenos obvios: la confusión clásica entre dos conceptos parecidos, el signo cambiado, la variable equivocada.
