# Cartera · Auditoría de diseño y plan maestro

*Noviembre de 2026 · sobre la versión publicada v706 · ningún cambio de código hecho todavía*

---

## 0. Cómo leer este documento

**Qué se ha revisado.**
- **El código, entero.** `cartera.html` tiene 817 KB y 6.794 líneas: estilos 8-3018, estructura 3019-3233 y JavaScript 3237-6794. Se leyó en cuatro revisiones independientes (estilos de escritorio, móvil, primera y segunda mitad del JavaScript), más el script que construye la página publicada.
- **Más de 50 capturas propias** de la app con datos reales, en el simulador:
  - escritorio a 1440 px (las cinco vistas, la ficha, la búsqueda, el menú y el modo oscuro) y a 820 px;
  - iPhone de 402 px (las cinco pestañas, seis fichas y el final de página), en claro y en oscuro.

**Qué no se ha podido comprobar.**
- El iPhone de verdad: la apertura, la barra de la hora y el tacto real de los gestos. Lo que depende de eso está marcado como «por confirmar en el iPhone».
- Las afirmaciones de las revisiones que no se sostenían al comprobarlas se han quitado. Por ejemplo, que «Inter Display no se carga»: sí se carga, la incluye el script de construcción.

**Referencias.** Los números de línea son de `cartera.html`; en la página publicada (`index.html`) son 20 más. Este repositorio es público, así que el documento no lleva importes, edades ni nombres de las empresas de la cartera.

**Prioridades.**
- **P0**: error visible o dato que engaña. Se arregla ya.
- **P1**: falta de sistema; produce incoherencias en toda la app.
- **P2**: replanteamiento de calado.
- **P3**: pulido.

**Cómo se aprueba.** Nada de esto se aplica sin tu permiso. El plan de la sección 11 está pensado para aprobarlo por bloques.

---

## 1. Veredicto en una página

**Lo que ya tiene alma y hay que proteger.**
- **Una voz.** Cada vista del escritorio abre con una frase en segunda persona que resume el estado. Es raro en finanzas y es lo más tuyo de la app.
- **Una paleta cálida y propia**: marfil, tinta verde casi negra y un menta de acento. Los grises son cálidos, no los de serie.
- **Una firma**: el logotipo «Cartera.», con su punto menta repetido en los iconos de las pestañas.
- **Obsesión por que nada salte**: pintado sin parpadeos, cifras que cuentan del valor viejo al nuevo, logos ajustados a píxel entero.
- **Piezas de primera**:
  - la matriz de valor razonable de la ficha;
  - la regla de precio con el abanico de valor;
  - la calculadora con las cuentas en cascada;
  - los hitos anotados en el gráfico de Movimientos.

**Lo que impide que parezca un producto de autor.**
1. **No hay sistema, hay sedimento.**
   - Tres sistemas de componentes superpuestos en los estilos de escritorio (el original con variables, el «gx» y el «zf/zcp»).
   - 125 `!important` y 177 etiquetas de parche.
   - Un tercio de los estilos son comentarios que narran cambios pasados.
   - 42 verdes y 24 rojos distintos, y 41 sombras distintas.
   - Cada pantalla nueva ha traído su propia paleta privada.
2. **El escritorio y el móvil son dos productos.**
   - El escritorio es editorial: frases grandes, mapas, gráficos.
   - El móvil es una lista: rótulos de una palabra, filas de dos líneas y ni un solo gráfico de línea.
   - Ni siquiera comparten los valores de color: el móvil copia los del escritorio con diferencias de 1 a 8 unidades que nadie decidió, y su modo oscuro es otro (verdoso frente a neutro).
3. **La fórmula se ha comido a la voz.** Las frases de dos líneas con la segunda en gris aparecen en todas las vistas y en las seis secciones de cada ficha. Vistas seguidas suenan a generador, y además repiten las cifras que hay justo debajo.
4. **Algunos gráficos no son honestos.** Varios prometen «área = peso» y no lo cumplen, el mismo color significa cuatro cosas distintas según el gráfico, y hay un fallo de datos en los dividendos.
5. **La narrativa responde al «cómo voy», pero no al «qué hago».** Ninguna vista termina en una decisión. El Universo, que es lo más cercano, se ordena por defecto por industria y en orden alfabético.

**Los diez movimientos que más cambian la app**, por orden:

| # | Movimiento | Tipo |
|---|---|---|
| 1 | Arreglar los errores visibles: franja verde al final del móvil, dividendos del mes en curso, mapas que inflan el área | P0 |
| 2 | Un solo sistema de diseño: valores de color, letra, espacio, radio, sombra y movimiento compartidos por escritorio y móvil | P1 |
| 3 | Una gramática de gráficos: un color, un significado | P1 |
| 4 | Un único módulo de formato: cifras, fechas, signos y vocabulario | P1 |
| 5 | La voz: menos frases, mejor elegidas y sin repetir lo de debajo | P2 |
| 6 | Arquitectura narrativa: «Cómo voy → Por qué → Qué viene → Qué hago» | P2 |
| 7 | El móvil como app de autor: gráfico protagonista, título grande, estados pulsados, transición fila → ficha | P2 |
| 8 | La ficha de escritorio fluida, no un lienzo de 1600 × 980 encogido | P2 |
| 9 | Accesibilidad: foco, contraste del menta, objetivos táctiles, VoiceOver | P1 |
| 10 | Limpieza: borrar el registro de cambios del código, lo muerto y los parches | P1 |

---

## 2. La historia que cuenta el diseño

### 2.1 Escritorio: hay narrativa, pero es un recitado

Orden actual de cada vista:

| Vista | Abre con | Después |
|---|---|---|
| Resumen | Frase: cómo fue la sesión y cuánto ganas desde el inicio | Tarjeta oscura de la sesión con mapa · tarjeta clara de evolución · tabla de posiciones de 14 columnas |
| Análisis | Frase: en cuántas industrias te repartes | Muro de logos por industria · dividendos · cascada del retorno · trayectoria |
| Movimientos | Frase: cuántos movimientos y cuánto has aportado | Barras mensuales con hitos · tabla con filtros |
| Calendario | Frase: cobros hasta fin de año | Dos meses en rejilla · agenda de 58 filas |
| Universo | Frase: cuántas subieron y bajaron | Dos mapas · mapa de calor de industrias · columnas de exigencia · tabla de 117 filas |

**Problemas.**
- **Se cuentan temas, no una historia.** Análisis va de diversificación a rentas, de ahí al retorno y luego al futuro. Lo natural sería al revés: cuánto he ganado → de dónde sale → qué rentas produce → cómo de concentrado estoy → adónde voy.
- **Repetición.** La ganancia total aparece en la frase de Resumen y otra vez en la tarjeta de Evolución, a 200 px. El número de empresas que suben o bajan está en la frase y otra vez en la leyenda del mapa. El valor de la cartera sale en Resumen y otra vez en la frase de Movimientos. Una historia no repite el dato: lo amplía.
- **La frase de Posiciones rota cada día** entre cuatro variantes según el día del año (`dia%fr.length`, 5546-5548), no según lo que importa. Un día habla de tu peor empresa y al siguiente de cuántas tienes, aunque no haya cambiado nada. Es variedad artificial.
- **No hay «qué hago».** Ninguna vista termina en una acción. El Universo ordena su tabla por industria (`psort=["ind",1]`, 4034) en lugar de por margen o por «por revisar».
- **El ritmo de superficies es arbitrario.**
  - En Resumen hay una tarjeta oscura de 570 px al lado de una clara de 800 px.
  - En Análisis se alternan sección sobre blanco sin tarjeta, banda crema con tarjeta, blanco sin tarjeta y crema con tarjeta.
  - El cambio de fondo no significa nada: no separa capítulos, separa el orden en que se hicieron los bloques.

### 2.2 Móvil: ordenado, pero sin historia

- **Cada pestaña sigue el mismo esquema**: bloque oscuro con una cifra grande y dos pares a la derecha, y debajo listas de filas de dos líneas en tarjetas blancas. Es coherente, pero es uniformidad de plantilla, no de sistema: la misma forma valga o no para el contenido.
- **La voz editorial desaparece.** Los títulos son rótulos de una palabra a 13 px («Reparto», «Historial»). Solo sobrevive en la ficha, a 13 px.
- **Resumen no cuenta cómo vas.** Hay tres cifras y una lista alfabética: la mayor posición no va primero y lo que más se ha movido hoy queda escondido. El orden alfabético se decidió en la v635; aquí se propone revisarlo (sección 12).
- **No hay ni un gráfico de línea.** No hay evolución del valor ni cotización en la ficha. Para una app de inversión es la ausencia más grave.

### 2.3 Propuesta: una sola historia en cuatro capítulos, en las dos plataformas

| Capítulo | Pregunta | Dónde |
|---|---|---|
| **Cómo voy** | ¿Gano o pierdo, hoy y desde el principio? | Resumen: una frase, la cifra y el gráfico de evolución como protagonista |
| **Por qué** | ¿Qué lo explica? | Resumen (lo de hoy: quién movió la cartera) y Análisis (cascada del retorno, concentración) |
| **Qué viene** | ¿Qué cobro y qué pasa en los próximos meses? | Calendario: cobros, resultados y fechas que te tocan |
| **Qué hago** | ¿Compro, vendo, reviso? | Universo y ficha: «por revisar» primero, margen frente a lo exigido |

**Reglas de la historia.**
- Una sola frase por vista, no por bloque. Los bloques llevan un título corto.
- La frase dice lo que no está en los números de debajo: una comparación, una causa o un cambio. Si solo repetiría una cifra visible, se omite.
- La frase se elige por relevancia (un movimiento grande, una posición nueva, un cobro próximo, un objetivo alcanzado), nunca por el día del año.
- Cada vista termina en algo que se puede hacer, como abrir la ficha que hay que revisar.

---

## 3. Coherencia total

### 3.1 Valores de diseño (las variables de color, letra, espacio y radio)

- **En el escritorio:**
  - Hay 301 variables. 77 no se usan nunca: todo `--glass-*` (1349), todo `--g-*` (1497), `--ih-*` (26) y otras.
  - **De la guía de letra (38 · 28 · 17 · 13 · 12), `--fs-38` no existe y `--fs-28` no se usa.** Los dos se escriben a mano (2224-2225, 2260).
  - El radio real de las tarjetas, 20 px, no tiene variable: se escribe a mano 10 veces.
  - Los colores base claros están en la línea 1282, 1.200 líneas después de sus versiones oscuras (49, 63).
- **En el móvil:**
  - Hay unas 40 variables `--m-*` con color propio (2372-2379), en lugar de remitir a las del escritorio:

| Móvil | Valor | Escritorio | Valor |
|---|---|---|---|
| `--m-bg` | #F4F2ED | `--canvas` | #F3F1EC |
| `--m-line` | #E7E4DD | `--line` | #E5E2DB |
| `--m-tpos` | #8FD3AE | `--h-pos` | #86D3AA |
| `--m-tneg` | #F0A3A3 | `--h-neg` | #F2A097 |
| oscuro `--m-bg` | #0F1311 (verdoso) | oscuro `--canvas` | #0F0F0E (neutro) |
| oscuro `--m-card` | #171C19 | oscuro `--card` | #262624 |

- **Propuesta (P1).** Una sola capa de valores de diseño en tres niveles:
  - **Base**: la paleta en bruto.
  - **Con significado**: `--texto`, `--texto-2`, `--superficie`, `--tarjeta`, `--sube`, `--baja`, `--acento`, `--previsto`…
  - **De componente**: solo si hace falta.
  - El móvil usa los mismos nombres con significado: `--m-*` desaparece o pasa a ser un alias.
  - El modo oscuro se escribe una vez, con `light-dark()` o con una única capa. Hoy hay 39 pares de reglas duplicadas, una para el oscuro automático y otra para el forzado.

### 3.2 Tipografía

- **Familias.** Inter para todo e Inter Display en las cifras grandes. Inter es una buena letra, pero es la letra por defecto de 2020-2026: por sí sola, no da autoría.
  - La letra Newsreader (con serifa) se distribuye con la web y no se usa en ninguna parte.
  - `--f-mono` es Inter, así que no es monoespaciada (1282).
- **Tamaños realmente usados en el escritorio** (194 declaraciones):
  - 13 px ×67, 12 ×51, 11 ×24, 17 ×18;
  - 12,5 ×9 y 15 ×9;
  - 20 ×3, 14 ×2, 16 ×2, 24 ×1, 28 ×1;
  - y dos tamaños fluidos (26-38 y 20-28) que producen tamaños intermedios.
- **En el móvil**: 13 ×28, 12 ×29, 11 ×9, 10 ×4, 9 ×1, 15 ×2, 16 ×1, 17 ×4 y 28 ×1.
- **El parche G-02 (2280-2305) no corrige el origen**: añade 26 sobrescrituras encima. Ejemplo: la navegación vale 13 en la 637, 14 en la 1395 y otra vez 13 en la 2285.
- **Jerarquía invertida en el Resumen del móvil.** Las filas van a 15 px y el título de la sección «Posiciones» a 13 (la «prueba B» de la v671, 2818-2822, que sigue publicada). Las demás pestañas van a 13/11, así que hay dos densidades en la misma app.
- **Peso.** El 600 se usa 42 veces en el escritorio y 26 en el móvil: pesan igual la cifra grande, los totales, las pastillas y la pestaña activa. Cuando todo pesa, nada destaca.
- **Cifras tabulares.** No hay una regla global. Las cifras grandes de Resumen usan el atajo `font:` (2224-2225), que anula `font-variant-numeric`, así que no son tabulares.
- **Versalitas.** Los títulos de sección van en versalitas de 11 px fuera de Resumen y en minúscula de 17 px dentro (1934).

**Propuesta (P1).** Una escala tipográfica con nombre, aplicada en el origen y la misma en las dos plataformas:

| Nivel | Escritorio | Móvil | Uso |
|---|---|---|---|
| Frase de vista | 38 | 17 (o 28 si es la cifra) | la única frase por vista |
| Cifra protagonista | 28 (Inter Display, tabular) | 28 | valor de la cartera, cifra de la ficha |
| Título de bloque | 17 | 17 | un título por bloque, nunca dos líneas |
| Texto y cifras de lista | 13 | 13 (15 si se decide mantener la prueba B en todo) | filas, tablas |
| Rótulo y secundaria | 12 | 12 | ejes, notas, detalle |
| Excepción declarada | 11 | 11 | solo ejes densos, y diciéndolo |

Dos pesos para jerarquizar, 400 y 600, más el 500 solo en la navegación. Cifras tabulares en toda cifra.

**Propuesta de autoría (P2, para decidir).** Dar a la frase de cada vista una letra con carácter: una serifa de texto (Newsreader ya está en el proyecto) o un corte Display con ligaduras y espaciado propios. Es la forma más barata de que la app deje de parecer «Inter sobre marfil». Habría que probarlo en una sola vista antes de extenderlo.

### 3.3 Color

- **Verdes de ganancia como texto:**
  - claro: #346F57, #2F6650, #86D3AA y #8FD3AE;
  - oscuro: #71B595 y #7CC49F.
- **Rojos de pérdida**: #9C4848, #924343, #F2A097, #F0A3A3, #D88282 y #E39393.
- **En total, 42 verdes y 24 rojos**, contando las escalas `--lp1-5` y `--ln1-5`, que además están duplicadas como `--rlv-*`.
- **El menta (#86D3AA) tiene cuatro nombres** (`--h-pos`, `--gx-id`, `--zsr`, `--m-gid`) y **cuatro significados**:
  - dividendo previsto;
  - aportación positiva en la cascada;
  - ganancia en la trayectoria;
  - empresa que supera lo exigido.
  - Además, en el móvil es el color neutro de las barras de Reparto y Pagadores.
- **La tinta (#1A231E) significa**: cobrado, aportado, retorno total y entradas.
- **Escalas de color de rendimiento: cuatro, con umbrales distintos.**
  - Mapa y losetas: 0,5 / 1 / 2,5 / 5 %.
  - Industrias, para todos los periodos, «Hoy» incluido: 5 / 15 / 40 / 100 %. Un 3 % en un día casi no tiene color, mientras que en el mapa ese mismo 3 % es el escalón 4 de 5.
  - Matriz de valoración: 5 / 10 / 20 / 35 %.
  - `retLevel` de 10 niveles y `tirLv` de 8.
- **Modo oscuro.**
  - En el escritorio, la barra superior es carbón neutro (Revisión 52); en el móvil, verde.
  - En el mapa oscuro del escritorio, las casillas que bajan quedan en un granate terroso de poco contraste con el fondo.
- **Cada componente nuevo trae su paleta**: la ficha (`.zf`) unas 20 variables, la calculadora 9 y los avisos `--nt-*`.

**Propuesta (P1): un diccionario de color de una página.**

| Significado | Claro | Oscuro | Nunca para |
|---|---|---|---|
| Sube / ganancia (texto) | un verde | un verde | barras neutras, previsto |
| Baja / pérdida (texto) | un rojo | un rojo | — |
| Escala de subida, 5 niveles | `--sube-1…5` | ídem | — |
| Escala de bajada, 5 niveles | `--baja-1…5` | ídem | — |
| Neutro de gráfico (reparto, peso) | tinta al 80 % | tinta clara | — |
| Realizado / cobrado | tinta | tinta clara | — |
| Previsto / estimado | tinta con trama o al 35 % | ídem | menta |
| Acento de interfaz (selección, foco) | menta oscuro con contraste ≥ 3:1 | menta | datos |

Y dos escalas de rendimiento, no cuatro: una para movimientos diarios y otra para periodos largos y distancias de valoración.

### 3.4 Espacio, retícula y puntos de corte

- **Espaciado.** Solo el 69 % de los márgenes y rellenos son múltiplos de 4. El 6 es el intruso: 47 veces.
- **Rellenos de tarjeta distintos**: 26 / 30 (409), 48 / 48 / 44, 40 / 48 y 32 / 36 / 28.
- **Unos 21 puntos de corte**, algunos escritos de dos maneras (700,99 y 701). Hay solapes reales a 1000 y a 1200 px, donde se aplican a la vez reglas de «hasta» y de «desde».
- **Ancho máximo.** La página mide 1440 px, aunque el comentario de la línea 30 todavía dice 1180.
- **Márgenes del móvil.**
  - A la izquierda, 16 px (bloque oscuro y títulos), 12 (tarjetas), 22 (logos), 26 (notas) y 62 (texto de fila).
  - **A la derecha, las cifras terminan en tres sitios: x = 386, 380 y 376.** Son 10 px de baile entre cifras que deberían caer en la misma línea.

**Propuesta (P1).**
- Una escala de 4: 4 · 8 · 12 · 16 · 24 · 32 · 48 · 64.
- Un único relleno de tarjeta por plataforma.
- Cinco puntos de corte (701 · 761 · 1001 · 1201 · 1441) escritos de una sola forma.
- En el móvil, una sola línea izquierda (16) y una sola línea derecha para las cifras (16 desde el borde).

### 3.5 Componentes

- **Radios de contenedor**: 12, 20, 18, 16 y 14. El menú «···» ha pasado por 14 → 18 → 20, con tres sombras distintas.
- **Alturas de botón**: 28, 30, 32, 36, 38, 40 y 44.
- **Alturas de fila**: en el escritorio unos 44, 39, 33, 34, 40, 46 y 28; en el móvil 52, 57, 40, 44, 48, 60 y 64.
- **Pestañas en cuatro estilos**: segmentado, texto subrayado, lista lateral y palabra subrayada con flecha.
- **Elevación**: 41 sombras distintas en el escritorio y 5 sin variable en el móvil.

**Propuesta (P1): una biblioteca corta**, escrita una sola vez y usada por las dos plataformas:
- tarjeta (2 variantes);
- fila de lista (1 altura por plataforma);
- tabla;
- control segmentado;
- palabra con menú;
- botón (2 tamaños);
- pastilla de cifra;
- hoja y modal;
- aviso;
- 3 niveles de elevación.

### 3.6 Movimiento

- **Escritorio**: 11 transiciones con la curva por defecto; ninguna animación usa curvas propias. Cada pieza entra de una forma: la ficha 32 px en 0,18 s, la calculadora 12 px en 0,18 s, el modal 8 px en 0,16 s y la vista con un fundido de 0,15 s.
- **Móvil**: 10 curvas distintas (entre ellas una de 49 puntos) y 12 duraciones.
  - Hay una regla global `#ma>:not(.m-bar){transition:transform .55s…}` (2488) que ya pisó a la ficha y obligó a un parche.
- **Lo bueno**: todas las animaciones tienen su versión para quien prefiere menos movimiento.

**Propuesta (P1).** Tres duraciones (rápida 160 ms, media 280 ms, larga 420 ms), tres curvas con nombre (salida, entrada, muelle) y una sola receta de entrada para hojas, modales y menús.

### 3.7 Formato de cifras, fechas y vocabulario

- **Unas 20 funciones de formato** para lo mismo.
- **El espacio antes de «€» y «%» se mezcla en la misma pantalla**: fino en `eur`, `pct` y `maP`, normal en `gxE`, `gxP`, `gxPS`, `maPct` y las marcas de los ejes. En la ficha móvil, «Esperas» y el porcentaje de al lado usan espacios distintos (6251).
- **El cero.** `pct` y `seur` devuelven vacío, de ahí 20 parches `||"0,0 %"`. Puede salir «−0,0 %».
- **Seis formatos de fecha**: «05/11/26» en las tablas del Universo, «5 nov», «17 jul 2025», «5 nov 2026», «5 de noviembre de 2026» y «mar 4 nov».
- **Millones**: «1,2 millones €» en el eje y «1,23 millones de €» en la frase.
- **Vocabulario.**
  - **«TIR» significa dos cosas**: realizada en Posiciones y esperada en el Universo.
  - **Un mismo conjunto, cuatro nombres**: Universo, Radar, Preselección, Candidatas.
  - **El valor razonable, seis nombres**: «Valor razonable», «Para ti vale», «abanico de valor razonable», «Precio objetivo», «podría valer», «Tu proyección».
  - **Jerga sin traducir**: «LTM», «CAGR», «FCF», «M€», «PM», «Call/Put», «Δ». Al mismo tiempo, la ficha dice «Media de 200 sesiones» en vez de «MM200»: unas veces se traduce y otras no.
  - **«Modo noche»** en los menús y **«Modo oscuro»** en el botón.
  - **La columna «Dividendo»** de Posiciones muestra la rentabilidad por dividendo, no el dividendo.
  - **Una voz en primera persona** se cuela en los avisos: «si se repite, avísame», «No localizo tu script». ¿Quién es ese «yo»?

**Propuesta (P1).**
- Un único módulo de formato, con espacio fino siempre, «0,0 %» explícito y sin «−0».
- Fechas: «5 nov» en listas, «5 nov 2026» si el año no es el actual y «miércoles 5 de noviembre» en frases.
- Y un glosario corto que se aplique en todo el código:

| Concepto | Nombre único |
|---|---|
| TIR conseguida | TIR |
| TIR que esperas | TIR esperada |
| Lo que exiges | Exigida |
| Las empresas que sigues | Universo (y «Radar» desaparece) |
| Lo que crees que vale | Valor razonable |
| Rentabilidad por dividendo | Rent. dividendo |
| LTM / CAGR / FCF | Últ. 12 meses / Crec. anual / Flujo de caja |
| Tema | Modo oscuro |

---

## 4. Lo que delata plantilla o IA (sin filtros)

1. **La combinación** de Inter, fondo marfil, acento menta, tarjetas de 20 px y botones redondeados es hoy la firma por defecto de las herramientas de IA para webs financieras «elegantes». Nada de eso es malo por separado; junto, es reconocible al instante.
2. **La frase de dos líneas con la segunda en gris**, repetida en todas partes y siempre con la estructura «cifra; coletilla»:
   - «Tus N empresas se reparten en M industrias distintas.»
   - «Arriba, X (+5 %); abajo, Y (−3 %).»
   - «En tu radar tienes N empresas; en tu cartera, M.»
3. **Frases de relleno**: «Toca revisar si tu tesis sigue en pie» en todos los resultados de todas las empresas, «Nada a la vista», «Las cifras de tu cartera y los datos de cada pestaña dicen lo mismo en las dos».
4. **El tono de asistente**: «Con la que más pierdes es con…», «avísame». Es la voz de un chat metida en la app.
5. **La paleta de comandos ⌘K** con desenfoque y teclas grises. Está bien hecha, pero es la de Raycast/Linear/Vercel.
6. **El punto «en vivo» con halo pulsante**, el esqueleto de carga con brillo y la sombra pequeña de 1 a 2 px por defecto (39).
7. **Los iconos de trazo 1,5 con extremos redondeados**, del estilo de Lucide y Feather (24 en la estructura de la página).
8. **El «vidrio líquido» copiado de iOS** en la barra de pestañas del móvil: un muelle simulado con constantes a mano (5863-5915) y comentarios que hablan de «ingeniería inversa». Al lado del sistema real siempre parece una copia, y sus constantes no coinciden con sus propios comentarios (el código usa un ancho de 80; el comentario dice 88 y 73,2).
9. **El código como registro de cambios.**
   - Un tercio de los estilos son comentarios.
   - Hay 90 etiquetas de versión en 1.500 líneas de JavaScript y frases como «vídeo de las 22:50: de 55 a 102 de brillo».
   - 238 comentarios ya no tienen ninguna regla debajo.
10. **Perfeccionismo sin sistema.** Hay un algoritmo de Bézier para colocar un punto donde la curva cruza un umbral y ajustes de logo en múltiplos de 1/64 px. Al mismo tiempo, conviven cuatro escalas de color, dos espacios antes de «%» y barras sin eje ni cifras.
11. **El logotipo «Cartera.» es un PNG en base64** a cinco densidades dentro de los estilos (2191-2192, unos 30 KB), en vez de texto o SVG.

**Lo que falta para sentirse producto de autor en 2027.**
- Una idea gráfica propia y reconocible: un tipo de gráfico, una forma de anotar o una manera de marcar «tu» dato, que se repita en todas partes.
- Una sola voz con criterio editorial, que sepa callarse.
- Movimiento con intención: la fila se convierte en la ficha y la cifra viaja de la lista a la cabecera.
- Silencio: menos bloques, más aire en los que quedan y una sola cosa protagonista por pantalla.

---

## 5. Errores concretos (P0)

Todos comprobados en el código o en captura.

| # | Error | Dónde | Comprobado | Qué hacer |
|---|---|---|---|---|
| E1 | **Al llegar al final de cualquier página del móvil, los últimos 110 px (tras la barra de pestañas) salen verde oscuro en vez de crema.** Viene de la v700 (`#ma` en verde y el crema solo en cada página); el relleno inferior de `#ma` queda verde. | `#ma` 2627, relleno inferior 110 px | Captura del final de Resumen | Pasar el relleno inferior a la página o pintar el crema también ahí |
| E2 | **Dividendos de 24 meses: lo ya cobrado en el mes en curso no aparece nunca.** La parte pasada va de −12 a −1 y la futura empieza en el mes en curso con `paid:0`. | 3575-3578 y 4699-4700 | Código, y en captura el mes en curso solo enseña lo pendiente | Incluir lo cobrado del mes en curso en su columna |
| E3 | **El mapa de Resumen infla el área de las posiciones pequeñas** hasta el 20 % de la mayor para que quepa su porcentaje. Una del 0,5 % puede ocupar varias veces su peso. El grupo «Otras» que promete el comentario es código muerto. | 5494-5495 | Código | Agrupar de verdad en «Otras (n)» lo que no cabe |
| E4 | **El muro de industrias de Análisis** (escritorio) tiene un ancho mínimo de 76 px por logo: con muchas posiciones se rompe «ancho = peso». Alto fijo de tres baldas aunque baste una. Los bordes derechos de las filas quedan en tres sitios distintos. | 4457-4471 | Código y captura | Mismo criterio que E3 y alto según el contenido |
| E5 | **Leyenda de dividendos del móvil**: «Previsto este año» lleva la muestra de «solo previsto», pero suma cobrado + previsto. El gráfico enseña dos años a medias con letras que se repiten (E F M A M J J A S O N D E F…) sin separar años. | 6129-6135 | Código y captura | Que la leyenda sume lo que pinta y separar los años |
| E6 | **«Hoy» o «última sesión» se decide de tres maneras** (reloj, variación de tus posiciones y variación del Universo). A la misma hora, una vista dice «hoy» y otra «última sesión». El reloj no sabe de festivos ni de la apertura de EE. UU.; un martes a las 23:00 sigue diciendo «hoy». | 6583, 5693, 6068 | Código | Un solo criterio, por bolsa |
| E7 | **La cabecera de cada mes en Movimientos** pinta las entradas en verde, cuando la regla de esa misma vista dice que el dinero que entra o sale va sin color. | 3990 (frente a 3910-3912) | Código | Quitar el color |
| E8 | **Dividendos en la tabla**: los cobrados van primero y los previstos al final, detrás de varias páginas de «Ver 50 más». El separador «Cobrados / Previstos» es código muerto. | 3977, 3989 | Código | Previstos arriba y separador visible |
| E9 | **Las columnas de exigencia** (Universo) recortan el eje al 40 %: lo que pasa de ahí queda en el borde sin marca. Las candidatas por debajo de lo exigido van al 12 % de tinta y casi no se ven. | 4632-4651 | Código y captura | Marcar lo recortado y codificar «tuya / candidata» con la forma, no con la opacidad |
| E10 | **Vacío equivocado**: con el filtro «Por revisar» sale «Ninguna valoración tiene más de un año», pero «por revisar» incluye también las empresas con resultados posteriores a la valoración. | 4156 | Código | Texto que cubra los dos casos |
| E11 | **⌘K, destinos muertos**: «Industrias», «Países», «Divisas» y «Concentración» abren Análisis arriba sin cambiar nada; «Concentración» ya no existe. | 4304 | Código | Cada destino lleva a su sitio, o se quita |
| E12 | **El Cuadre antiguo sigue vivo**: se construye y se oculta en cada lectura y aún puede mostrarse junto al nuevo. Son dos cuadres con formatos distintos. | 3824, 6748 | Código | Borrarlo |
| E13 | **Signos**: «Llevas perdido» enseña la cifra en rojo sin signo, mientras que el resto de pérdidas lleva «−». | ficha móvil | Captura | Un criterio: siempre signo, o nunca con verbo de pérdida |
| E14 | **Filas de importe 0,00 €** («1 acción a 0,00 €») se pintan como una compra más. | Movimientos | Captura | Marcarlas (regalo, ajuste…) o avisar de que el dato está incompleto |
| E15 | **El menú «···» del escritorio** tiene cinco escuchas de clic y dos reglas de colocación que se contradicen (5670 y 6767); gana la primera por ir un fotograma después. | 5471, 5670, 6594, 6767, 6770 | Código | Un solo manejador y una regla |

---

## 6. Revisión pantalla por pantalla

### 6.1 Escritorio · Resumen
- **La frase (38 px)** es lo mejor de la vista, pero repite la ganancia que sale 200 px más abajo.
- **Tarjetas desiguales.** La oscura de la sesión mide 570 px y la clara de evolución 800: dos tratamientos opuestos que compiten. **Propuesta**: la evolución como protagonista a todo el ancho y la sesión como banda o columna estrecha subordinada.
- **Mapa.**
  - Los logos tienen pesos ópticos muy distintos: unos rellenos y negros, otros finos.
  - La casilla sin logo con solo «−0,5 %» no dice qué es.
  - La leyenda «N suben · N bajan» repite la frase. Ver también E3.
- **Evolución.**
  - Bien resuelta: el relleno en verde por encima y en rojo por debajo de lo aportado, y los ejes con pasos redondos.
  - Los círculos blancos en dos puntos de la curva (los cruces de umbrales redondos) no se explican.
  - Hay dos entradas animadas encadenadas: el trazo de 1,8 s y el conteo de las cifras.
- **Tabla de posiciones (14 columnas).**
  - Densa y bien alineada.
  - Las pastillas de color de «Rentabilidad» compiten con las barras de peso, y «TIR» va sin color mientras que en el Universo va en pastilla.
  - **Propuesta**: tres columnas protagonistas (valor, peso, rentabilidad) y el resto en gris medio o plegable.
  - La frase de la tabla rota cada día (sección 2.1).
- **El pie** queda separado del contenido por una banda vacía de unos 60 px.

### 6.2 Escritorio · Análisis
- **El orden de los bloques es temático**, no narrativo (sección 2.3).
- **El muro de industrias** es más decorativo que informativo: el logo manda sobre el peso. **Propuesta**: barras horizontales apiladas por industria con el logo como etiqueta, o un mapa honesto (E4).
- **Dividendos.**
  - Las barras de cobrado (tinta) y previsto (menta), separadas por la línea de hoy, están bien.
  - Ver E2.
  - La variación del año en curso compara cobrado + previsto con el año anterior entero sin decirlo.
- **«Quién te paga más»**: barras menta finas sobre carril gris. El menta aquí es neutro, otra vez el color con varios significados.
- **La cascada del retorno es correcta**, pero:
  - su paleta (menta, rojo al 70 %, tinta) no es la de la cascada de la ficha (salvia, rojo, negro);
  - recorta los negativos acumulados (`Math.max(0,v)`, 4733);
  - sus cifras van a 15 px, un tamaño único en toda la app.
- **La trayectoria** (deslizadores y barras apiladas) es una de las piezas con más alma. Mejora pendiente: marcar en las barras dónde está «hoy» con más fuerza que una línea de puntos.

### 6.3 Escritorio · Movimientos
- **Las barras mensuales** no tienen eje, ni cifras, ni leyenda. Las de arriba (tinta) y las de abajo (arena) no dicen qué son. El texto emergente solo da el mes, no el importe.
- **Los hitos anotados son muy buenos**: es la idea gráfica más propia de la app y debería extenderse (sección 9.3).
- **Tabla.**
  - La columna «Resultado» está vacía en casi todas las filas (solo la tienen las ventas).
  - En las aportaciones, el nombre del bróker sale dos veces (como «empresa» y como «bróker»).
  - El buscador va a 17 px y la tabla a 13: el campo pesa más que los datos.

### 6.4 Escritorio · Calendario
- **Dos meses en rejilla con casillas grises** ocupan media pantalla para enseñar nueve cobros: mucha tinta para poca información. **Propuesta**: una línea de tiempo horizontal de 90 días con los cobros como marcas proporcionales al importe, o la rejilla sin el fondo de las casillas vacías.
- **Agenda.**
  - La columna «Cobro» repite la misma palabra en las 58 filas.
  - El separador de filas empieza a otra sangría que el título del mes.

### 6.5 Escritorio · Universo
- **Los dos mapas de suben y bajan** repiten el lenguaje de Resumen, y eso es bueno.
- **El mapa de calor de industrias** está bien, pero «Hoy» usa la escala de periodos largos y casi no tiene color.
- **Columnas de exigencia**: la idea es buena y el tipo de gráfico es el adecuado, pero no se entiende sin explicación (ver E9). **Propuesta**: título del eje («TIR esperada») y una frase debajo de cada columna («exiges 10 %»).
- **La tabla de 117 filas** mide unas 8 pantallas sin agrupar.
  - Las banderas en círculo son un recurso de plantilla.
  - Las fechas van en «31/07/26», distinto del resto de la app.
  - **Propuesta**: ordenar por defecto por «por revisar» y luego por margen, y agrupar.

### 6.6 Escritorio · Ficha de empresa
- **Es lo más trabajado**: frase, regla de precio, posición, cascada, matriz de valor y «lo que viene».
- **Problema principal**: es un lienzo de 1600 × 980 que se encoge con `zoom` hasta 0,5 (4793).
  - A 1440 × 900 se ve al 87 %, y los textos de 12 px quedan en 10,4 px.
  - En un portátil de 1280 × 720 queda en torno al 70 %, con textos de unos 8,4 px.
  - Pasa lo mismo con el detalle de un movimiento (1600 × 561) y el Cuadre (1360 × 980).
- **Construye las seis secciones** aunque solo se vea una, y las rehace con cada flecha, cada cambio de ancho y cada lectura de la hoja.
- **Propuesta (P2)**: rejilla fluida con texto a tamaño real, una sección pintada a la vez y la misma estructura que la ficha del móvil.

### 6.7 Escritorio · Búsqueda, menú, calculadora
- **⌘K** está bien resuelto: resultados agrupados y ayuda de teclado. Ver E11.
- **El menú**:
  - mezcla acciones (calculadora, cuadre), preferencias (ocultar importes, modo noche) y una acción destructiva en rojo;
  - **propuesta**: separar «Herramientas» de «Preferencias» y llevar la acción destructiva a una confirmación.
- **La calculadora** es lo mejor resuelto de la zona: cuentas en cascada, aviso al vaciar, «Vender todas». Su código es casi idéntico en el escritorio y en el móvil, copiado.

### 6.8 Móvil · Resumen
- **Bloque oscuro**: valor a 28 px, con Hoy y Total a la derecha. Limpio.
- **La lista alfabética** esconde lo importante (sección 12).
  - **El «a X» de cada fila es ambiguo**: es tu precio medio en «Hoy» y la cotización en «Total», y ninguna etiqueta lo dice.
- **Ver E1** al final de la página.
- **No hay estado pulsado en las filas**: al tocar una empresa no pasa nada hasta que la ficha empieza a subir. Solo los botones del menú tienen `:active`.
- **Propuesta (P2)**:
  - bajo el bloque oscuro, el gráfico de evolución (alto 160 px, con lectura al mantener pulsado);
  - después «Hoy te ha movido» (las tres posiciones con más impacto en euros);
  - y luego la lista ordenable.

### 6.9 Móvil · Análisis
- Reparto con barras normalizadas a la mayor y no al 100 %, así que se pierde la idea de parte de un todo.
- Dividendos: ver E5.
- El retorno en barras de valor absoluto no es una cascada; la fila total solo lleva color si es negativa.
- La trayectoria es una lista de años, sin gráfico.

### 6.10 Móvil · Movimientos y Calendario
- **Movimientos.**
  - Las filas son claras.
  - Los textos largos se cortan («Ajuste FX 2026, provisional hasta el extra…») sin manera de leerlos enteros.
- **Calendario.**
  - La rejilla repite el problema del escritorio: casillas grises, logos de 9 a 12 px ilegibles.
  - «Próximo X, el 17» no dice el mes.

### 6.11 Móvil · Universo
- Las losetas de suben y bajan son lo más vistoso del móvil.
- **El Radar pinta todas las empresas de golpe**: cientos de filas con logo, sin paginar.
- «TIR 6,5 % (−3,5 pp)» en cada fila es denso; el margen debería ser el dato principal.

### 6.12 Móvil · Ficha
- **Subpestañas.** Las seis, en palabras subrayadas con desplazamiento horizontal: «Tesis» queda cortada y «Cotización» no se ve. La primera se llama «Resumen», igual que la pestaña principal.
- **Cotización** es una lista de rentabilidades y una «escalera de precios» en filas, sin gráfico.
- **No hay pila de navegación**: al tocar otra empresa dentro de la ficha se pierde la anterior.
- **Dos gestos para cerrar la misma hoja**: hacia abajo y desde el borde.
- **Las frases se sacan construyendo la ficha entera del escritorio** dentro de una plantilla y leyéndola (6227-6234). El propio código lo estima en un segundo de CPU, y si cambia el marcado del escritorio la ficha del móvil se queda sin texto, en silencio.
- **Repite datos.** En «Resumen», la posición y el valor ya están en el bloque oscuro, y «Tu proyección · 2030» pone un año donde se espera una cifra.

### 6.13 Móvil · Navegación, carga y gestos
- **No hay título de página**: dónde estás solo lo dice la barra de abajo.
- **Calculadora y Cuadre** se superponen a la pestaña actual sin título ni botón de volver, y la barra de abajo sigue marcando la pestaña de debajo.
- **Buscar** cuesta dos toques (menú → Buscar).
- **Tirar para actualizar** pone textos («Desliza para actualizar», «Suelta para actualizar»), un patrón antiguo de Android; iOS no pone texto. Se propone solo el anillo.
- **El cambio de pestaña es instantáneo, sin transición**: se siente web, no app.
- **Lo bueno**:
  - recuerda la posición de cada pestaña;
  - corta la inercia al cambiar;
  - pinta las demás pestañas por detrás sin estorbar al dedo;
  - las cifras cuentan al actualizar.

---

## 7. Gráficos: una gramática

### 7.1 Inventario
- Evolución (Chart.js).
- Mapas de Resumen y Universo.
- Muro de industrias.
- Barras de Movimientos.
- Dividendos (meses, años y pagadores).
- Cascada del retorno.
- Trayectoria.
- Mapa de calor de industrias.
- Columnas de exigencia.
- En la ficha: regla de precio, cascada, matriz de valor, compras en el tiempo, rentabilidad frente a la industria y escalera de precios.
- En el móvil: losetas, barras de 6 px, columnas de dividendos y la matriz.

### 7.2 Problemas de conjunto
- **Un color, varios significados** (sección 3.3).
- **Siete discontinuas distintas** (2-3, 2-4, 2-5, 3-3, 4-4, 5-4 en SVG y 3-3 en canvas).
- **Letra de los ejes**: 11 px en Chart.js y 12 en SVG. Hay 12,5 px nueve veces, 15 una vez y la ficha cambia el titular de 28 a 20 px según quepa.
- **Honestidad**: E3, E4 y E9.
- **Barras sin escala**: Movimientos y dividendos del móvil.

### 7.3 Gramática propuesta
1. **Un color, un significado** (diccionario de la sección 3.3).
2. **Previsto = la misma tinta que lo real, con trama o al 35 %**, nunca otro tono. Así se lee «lo mismo, pero aún no».
3. **Área y longitud siempre proporcionales.** Lo que no cabe se agrupa en «Otras (n)», nunca se infla.
4. **Toda barra tiene escala**: eje, rejilla mínima o cifra encima.
5. **Una línea de «hoy» única**: tinta al 40 %, discontinua 2-3, con el rótulo «hoy» arriba. La misma en dividendos, trayectoria, calendario y cotización.
6. **Anotar en lugar de leyendas.** Los hitos de Movimientos son el modelo: nombrar en el propio gráfico el punto que importa (máximo, cruce, compra) y quitar leyendas.
7. **Tipografía de gráfico**: 12 px para ejes y rótulos y 13 para cifras sobre barras. Sin 11 ni 12,5.
8. **Una sola cascada** para el retorno de la cartera y el de cada empresa.
9. **Entrada animada**: una sola, de 420 ms, y solo la primera vez.

### 7.4 La idea gráfica propia (P2)
Hace falta un rasgo que solo tenga esta app. Una propuesta concreta: **«tu marca» en todos los gráficos**.
- Un pequeño triángulo o punto menta que señala siempre «lo tuyo»: tu precio medio en la regla, tu casilla en la matriz, tu cartera frente al índice, tu compra en la cotización, tu objetivo en la trayectoria.
- Ya existe en germen: el punto del logotipo y la casilla destacada de la matriz.
- Convertirlo en sistema da identidad sin añadir adornos.

---

## 8. Accesibilidad

- **Foco con teclado** (escritorio). El anillo global está bien (85), pero se anula en 8 sitios:
  - la navegación (1914);
  - los iconos de la barra (2155, solo cambian de color);
  - `[data-tip]` (691), que le gana al anillo por ir después.
- **Contraste.**
  - El menta sobre blanco da 1,76:1 y sobre crema 1,56:1, pero es el único marcador de la sección activa y de las palabras que se pueden elegir. Un indicador gráfico pide 3:1.
  - El ámbar como texto da 3,47:1.
  - Los días pasados del calendario dan 2,09:1.
- **Objetivos táctiles.**
  - Escritorio: iconos de la barra de 16 × 36 px, «···» de 14 px de ancho, «i» de 18 × 18 y quitar operación de 16 × 16.
  - Móvil: opciones a 36 × 36 (y es el único acceso a Buscar, Calculadora y Ajustes), aspa de la ficha a 40 × 40, quitar operación a 24 × 32, casillas de la calculadora a 32-34.
- **VoiceOver.**
  - Las filas y losetas del móvil son `div` sin `role="button"`; las losetas llevan además `role="img"` aunque abren la ficha.
  - No hay `h1`.
  - La ficha es un diálogo pero no recibe el foco al abrirse.
  - El único `role="status"` está dentro del escritorio, que la app oculta: en el móvil nada anuncia «Actualizando…».
- **Zoom.** `maximum-scale=1` (3 y 5855) impide ampliar con los dedos. No se responde al tamaño de texto del sistema, y el bloque oscuro mide 71 px exactos con el desbordamiento oculto (2920).

**Propuesta (P1)**: arreglar todo lo anterior. Mejora el diseño, no solo la norma: el estado pulsado y el foco son también la respuesta que hoy le falta a la app.

---

## 9. Visión de futuro: replanteamientos de calado (P2)

### 9.1 Un sistema de diseño de verdad, «Cartera DS»
- Un único bloque de valores de diseño, la biblioteca de componentes de la sección 3.5, el diccionario de color, la escala tipográfica y la de movimiento.
- Escrito una vez y usado por escritorio y móvil.
- Todo lo demás (parches, `!important`, `html body`, `:not(#_)`, paletas privadas) desaparece al reescribir cada pantalla sobre él.
- Se hace pantalla a pantalla, no de golpe: el riesgo se controla comparando capturas antes y después.

### 9.2 Arquitectura de navegación
**Propuesta**, para decidir; no es la única posible. Mantener cinco destinos pero reordenados según la historia:

| Hoy | Propuesta | Por qué |
|---|---|---|
| Resumen | **Resumen** («Cómo voy») | igual, con la evolución de protagonista |
| Análisis | **Análisis** («Por qué») | reordenado: retorno → origen → rentas → concentración → trayectoria |
| Movimientos | **Movimientos** | igual |
| Calendario | **Agenda** («Qué viene») | cobros, resultados y fechas de revisión, en línea de tiempo |
| Universo | **Universo** («Qué hago») | por defecto «por revisar» y margen, no orden alfabético |

Además:
- Calculadora y Cuadre como hojas con título y botón de cerrar, no como páginas superpuestas.
- Buscar a un toque en el móvil: un icono en la barra superior, junto a las opciones.
- Ajustes como una hoja propia (tema, ocultar importes, conexión), separada de las herramientas.

### 9.3 Resumen como portada editorial
Escritorio y móvil con la misma estructura:
1. **La frase única** (sección 2.3).
2. **El gráfico de evolución, protagonista**, con hitos anotados como los de Movimientos (primera aportación, cada umbral redondo, máximo, caída mayor…) y «tu marca» al final de la curva.
3. **«Hoy te ha movido»**: las tres a cinco posiciones con más impacto en euros, con su barra de contribución. Sustituye al mapa como lectura principal del día; el mapa queda en el Universo o como vista alternativa.
4. **La lista o tabla de posiciones**, ordenable, con el orden por defecto según el modo: impacto en «Hoy», peso en «Total».

### 9.4 El móvil como app de autor
- **Título grande por pestaña** que se pliega al desplazar, con la cifra del bloque oscuro integrada.
- **El gráfico de evolución en Resumen** y el de cotización en la ficha, con lectura al mantener pulsado.
- **Transición compartida fila → ficha** con View Transitions, ya disponible en Safari: el logo y la cifra viajan de la fila a la cabecera de la ficha.
- **Estado pulsado** en filas y losetas: fondo al 4 % y escala de 0,98, en 120 ms.
- **Pila de navegación en la ficha** (volver a la empresa anterior) y un solo gesto para cerrar.
- **Barra de pestañas**: valorar si compensa mantener la imitación del vidrio de iOS con muelle propio, frente a una barra sobria propia de la app (sólida, con el punto menta como indicador). Es una decisión de identidad: la imitación siempre pierde al lado del sistema real.
- **Respuesta al tamaño de texto del sistema** y sin cajas de alto fijo.
- **Respuesta háptica**: Safari en iOS no da acceso a la vibración desde la web. Se renuncia a ella y se compensa con el estado pulsado.

### 9.5 La ficha, una sola en dos tamaños
- La misma estructura de datos y las mismas frases para escritorio y móvil, generadas desde los datos y no leyendo el HTML de la otra.
- En el escritorio, rejilla fluida a tamaño real y la navegación lateral que ya tiene. En el móvil, las mismas secciones con un control segmentado de cuatro como mucho:
  - **Resumen** (incluye la posición);
  - **Valoración** (incluye la proyección);
  - **Tesis**;
  - **Cotización** (con el gráfico).

### 9.6 La voz
- **Una frase por vista y una por ficha**, elegida por relevancia según una tabla de prioridades (cambio grande, novedad, próximo cobro, objetivo alcanzado, alerta de revisión).
- **Prohibido repetir una cifra** que esté visible debajo.
- **Sin primera persona** en avisos y errores: «No se ha podido leer la hoja. Vuelve a intentarlo».
- **Estados vacíos con intención**, que digan qué falta y qué hacer: «Aún no hay cobros previstos para este mes».
- **Revisar la norma de las dos líneas.** La frase de las vistas está en dos líneas por diseño, y conviene confirmar si eso choca con «las cabeceras nunca tienen dos líneas» (sección 12).

### 9.7 Rendimiento percibido
- **Pintar solo lo visible.** La ficha de escritorio pinta las seis secciones, el Radar pinta cientos de filas y cada logo analizado vuelve a pintar entera la portada de Análisis.
- **Sustituir los arreglos de píxel después de pintar** (cabeceras centradas con `translateX`, sombras medidas con rangos) por columnas de ancho fijo en CSS. Quita observadores, mediciones y saltos: hoy hay unos 20 `MutationObserver` y, solo en la primera mitad del JavaScript, 8 escuchas de cambio de tamaño.
- **Que `innerHTML` vuelva a ser `innerHTML`.** Hoy está redefinido en 19 elementos para comparar y cambiar solo lo distinto. Funciona, pero quien lea el código no lo sabe. Mejor una función con nombre (`pintaLista`).

---

## 10. Limpieza del código (P1, sin cambio visible)

1. Borrar el registro de cambios de los comentarios: unos 67 KB solo en los estilos de escritorio. La historia ya está en los commits; el comentario debe decir para qué sirve una regla, no cuándo se cambió.
2. Borrar las 77 variables sin uso, las 238 cabeceras huérfanas, el Cuadre antiguo, la ficha lateral antigua, el latido de carga muerto, los ajustes de subpíxel calibrados con capturas, los duplicados (`isOpt` y `esOpcion`, `fold` y `fo`, cuatro listas de meses, cuatro formas de unir con «y») y las funciones envueltas sobre funciones.
3. Fundir las capas de parches en su estado final: `.top` (unas 14 generaciones), `.duo` (7), flechas de orden (5) y el relleno de las tarjetas de Resumen (5).
4. Quitar los 125 `!important`, los 58 `html body` y los 11 `:not(#_)` a medida que se ordena la cascada.
5. Logotipo en SVG o texto en lugar de 30 KB de PNG en base64.
6. Quitar de la web la letra Newsreader, que no se usa, o usarla (sección 3.2).

**Riesgo**: bajo si se hace por bloques y se comparan las 27 pantallas antes y después, como en las últimas versiones.

---

## 11. Plan por fases

Cada fase se aprueba por separado. Dentro de cada fase, cada punto es una versión publicable con comparación de capturas.

**Fase 0 · Errores (P0).** Poco riesgo, efecto inmediato.
- E1 franja verde del móvil · E2 dividendos del mes · E5 leyenda del móvil · E6 «hoy / última sesión» · E7 color de los meses · E8 orden de dividendos · E10 vacío · E11 ⌘K · E12 cuadre antiguo · E13 signos · E14 filas a 0,00 € · E15 menú.
- E3, E4 y E9 (gráficos que engañan) van aquí si se acepta agrupar en «Otras».

**Fase 1 · Sistema (P1).** Base de todo lo demás.
1. Valores de diseño únicos y modo oscuro escrito una vez. Decidir antes un solo oscuro (sección 12).
2. Escala tipográfica aplicada en el origen; quitar G-02 como parche.
3. Diccionario de color y dos escalas de rendimiento.
4. Módulo de formato y glosario.
5. Escala de espacio, márgenes del móvil y puntos de corte.
6. Movimiento: tres duraciones y tres curvas.
7. Accesibilidad (sección 8).
8. Limpieza (sección 10), en paralelo.

**Fase 2 · Replanteamientos (P2).** Pantalla a pantalla, en este orden:
1. Resumen como portada editorial, en escritorio y móvil, con el gráfico protagonista.
2. Ficha única y fluida, con gráfico de cotización en el móvil.
3. Análisis reordenado con la gramática de gráficos.
4. Universo orientado a «qué hago».
5. Agenda en línea de tiempo.
6. Movimientos con escala y hitos.
7. Voz: una frase por vista, por relevancia.
8. Móvil: título grande, transición fila → ficha, estado pulsado, pila en la ficha y decisión sobre la barra de pestañas.

**Fase 3 · Pulido (P3).** La idea gráfica propia (sección 7.4) en todos los gráficos, la tipografía con carácter para la frase si se aprueba la prueba, y la revisión final de las 27 pantallas en claro y oscuro.

---

## 12. Decisiones que necesito de ti

Estas propuestas contradicen algo que ya decidiste, o dependen de tu gusto:

1. **Las frases de dos líneas** de cada vista: ¿cuentan como «cabeceras» para la norma de no tener nunca dos líneas? Si cuentan, la propuesta de una sola frase por vista lo resuelve.
2. **La lista del Resumen del móvil en orden alfabético** (v635): ¿la ordenamos por impacto en «Hoy» y por peso en «Total», con la alfabética como opción?
3. **Un solo modo oscuro**: ¿carbón neutro, como el escritorio desde la Revisión 52, o verdoso, como el móvil?
4. **La «prueba B»** del Resumen del móvil, con filas a 15 px: ¿se queda y se extiende a todo, o vuelve a 13 como el resto?
5. **La barra de pestañas imitando el vidrio de iOS**: ¿se mantiene o pasamos a una barra propia?
6. **Una letra con carácter** para la frase de cada vista (por ejemplo, Newsreader): ¿la probamos en una vista?
7. **El mapa como lectura principal del día en Resumen**: ¿lo sustituimos por «Hoy te ha movido», con el mapa en el Universo?

---

## Anexo A · Datos de partida (medidos)

| Medida | Valor |
|---|---|
| Tamaño de `cartera.html` | 817 KB · 6.794 líneas |
| Estilos de escritorio | 234 KB · 4.645 declaraciones · 1.602 selectores |
| Comentarios en los estilos de escritorio | 66,7 KB (33 %) · 238 sin regla debajo |
| `!important` (escritorio) | 125, el 82 % entre las líneas 1900 y 2380 |
| Variables | 301 distintas · 77 sin uso |
| Colores dentro de las variables | 166 hexadecimales y 55 rgba distintos |
| Verdes / rojos distintos | 42 / 24 |
| Sombras distintas (escritorio) | 41 |
| Radios distintos (escritorio) | 30 valores en 116 declaraciones; 43 con variable |
| Puntos de corte | unos 21 umbrales en 180 `@media` |
| Curvas de movimiento (móvil) | 10 · duraciones 12 |
| Funciones de formato de cifras | unas 20 |
| Formatos de fecha | 6 |
| Escalas de color de rendimiento | 4 (más dos de niveles) |
| `MutationObserver` | unos 20 |
| Final de página del móvil en verde | 110 px (relleno inferior de `#ma`) |
| Ficha de escritorio a 1440 × 900 | se ve al 87 %: 12 px → 10,4 px |
