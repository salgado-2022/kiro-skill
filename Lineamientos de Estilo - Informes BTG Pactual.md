# Lineamientos de estilo — Informes PDF corporativos BTG Pactual

Este documento describe, de forma completa y autocontenida, el sistema de diseño usado para
generar el **"Informe de casos — Beneficiarios Finales"**. Sirve para que cualquier IA (Claude,
ChatGPT, Gemini, etc.) o cualquier desarrollador reproduzca el mismo estilo en un informe nuevo,
sin necesidad de ver el documento original.

Puedes pegar este archivo completo como contexto/prompt para pedir un nuevo informe con este estilo.

---

## 1. Filosofía de diseño

- **Es un informe interno serio, no una pieza de marketing.** Denso en datos, pero fácil de escanear.
- **Fidelidad absoluta a los datos de origen.** Nunca se "arregla", redondea ni reinterpreta una cifra
  del documento fuente. Si el material original tiene una inconsistencia numérica, se reproduce tal
  cual — no es tarea del diseño corregir el contenido.
- **Jerarquía visual clara**: portada → resumen ejecutivo con métricas → cuerpo detallado →
  anexo numérico. El lector debe poder entender el 80 % del mensaje solo con la portada y el resumen.
- **Todo lo repetible se convierte en componente**: si un patrón (una tarjeta, una tabla, un badge de
  estado) aparece más de una vez, se define una sola vez en CSS y se reutiliza. Nunca estilos "a mano"
  caso por caso.
- **Precisión antes que decoración**: gráficos y tablas existen para transmitir un número exacto, no
  para llenar espacio. Si un dato ya se entiende en una frase, no necesita gráfico.

---

## 2. Paleta de colores — identidad BTG Pactual

Colores extraídos de la identidad de marca real de BTG Pactual (azul marino + azul cian).
**No usar otros colores de acento** salvo los semánticos de estado definidos abajo.

| Token | Hex | Uso |
|---|---|---|
| `--navy` | `#1E2A52` | Fondo de portada, encabezados de fichas (group-head), texto de máxima jerarquía |
| `--navy-2` | `#2F3D72` | Color secundario de marca; bordes de sección, encabezados de tabla, subtítulos |
| `--azure` | `#00A0E3` | Acento primario; números clave, iconografía, barras de gráfico principales, bordes de callout |
| `--azure-light` | `#EAF7FD` | Fondos de callout / tags |
| `--azure-mid` | `#CBEBF9` | Bordes de tags |
| `--gray-50` | `#F7F8FA` | Fondo de tarjetas neutras, filas alternas de tabla |
| `--gray-100` | `#EEF1F5` | Líneas divisorias suaves |
| `--gray-300` | `#D5DAE1` | Bordes de tarjetas y tablas |
| `--gray-500` | `#8A93A3` | Texto terciario / metadatos |
| `--gray-700` | `#5B6472` | Texto de cuerpo secundario |
| `--ink` | `#1B222C` | Texto de cuerpo principal |

**Colores semánticos de estado** (para badges de estatus — no cambiar su significado):

| Estado | Fondo | Texto | Significado |
|---|---|---|---|
| Verde `--green` `#1E8E5A` / bg `#E4F5EC` | Completado, Cerrada, cifras positivas |
| Ámbar `--amber` `#D98C00` / bg `#FCF1DD` | Pending, en curso, advertencia media |
| Rojo `--red` `#C0392B` / bg `#FBE7E4` | Abierta/urgente, riesgo, escalado |
| Gris `--gray-700` / bg `#EEF0F3` | Cancelado, no aplica, neutro |

> Regla dura: el color **nunca** es solo decorativo en un badge de estado — siempre codifica el
> mismo significado en todo el documento (verde = resuelto, rojo = abierto/urgente, ámbar = en curso,
> gris = cancelado/neutro).

---

## 3. Tipografía

- Fuente: **Liberation Sans** (métricamente compatible con Arial/Helvetica; disponible sin conexión
  a internet, lo que la hace segura para entornos sandboxed). Si no está disponible, usar DejaVu Sans.
- No usar fuentes decorativas ni serif en el cuerpo — el tono es financiero/corporativo, no editorial.
- Tamaños base (en pt, para render A4 a 96–110 dpi vía WeasyPrint):
  - Cuerpo de texto: 9.4–9.6pt, line-height 1.5
  - H1 de sección: 16–17pt, bold, con borde inferior de 2.4px en `--navy-2`
  - H2 subsección: 11.5–12pt, bold, color `--navy-2`
  - H3: 10–10.5pt, bold
  - Texto pequeño / metadatos: 7.4–8.4pt
  - Números grandes en tarjetas de estadística: 16–17pt bold
  - Título de portada: 28–30pt bold
- Números de caso / IDs (ej. `COL-123456`) siempre en fuente monoespaciada (Liberation Mono),
  bold, color `--navy-2` — para que sean escaneables como identificadores técnicos.

---

## 4. Retícula y página

- Tamaño A4, márgenes ~2.0cm arriba / 1.7cm laterales / 1.95cm abajo.
- Encabezado de página (todas las páginas excepto portada):
  - Arriba-izquierda: nombre corto del informe, gris tenue, 7.6pt
  - Arriba-derecha: "BTG Pactual — Confidencial" en `--navy-2` bold, 7.6pt
- Pie de página:
  - Abajo-izquierda: proyecto / área, gris muy tenue
  - Abajo-derecha: "Página X de Y"
- Cada sección numerada (1., 2., 3.…) empieza en página nueva (`page-break-before: always`),
  salvo la primera después de la portada.
- Las tablas largas repiten el encabezado (`<thead>`) en cada página nueva.
- Las tarjetas (group-card, pend, rec) llevan `page-break-inside: avoid` siempre que quepan en una
  página; si son más largas que una página completa, se deja que fluyan (no forzar cortes raros).

---

## 5. Portada

Estructura fija:
1. Fondo degradado `--navy` → `--navy-2` diagonal, con 2-3 arcos circulares sutiles (`border: 1px solid
   rgba(255,255,255,0.1–0.35)`) y un resplandor circular azul (`radial-gradient`) en la esquina
   inferior derecha — da profundidad sin ser ruidoso.
2. Marca arriba: círculo pequeño con gradiente azul + wordmark "BTG **Pactual**" (la palabra "Pactual"
   en azul acento, "BTG" en blanco).
3. Kicker/etiqueta pequeña en mayúsculas dentro de una píldora con borde azul (ej. "SOPORTE IT
   COLOMBIA · ANÁLISIS DE CASOS").
4. Título grande (28–30pt) en dos líneas, la segunda línea en azul acento.
5. Subtítulo de una frase describiendo el alcance.
6. Fila de metadatos clave (proyecto, tipo, periodo, fecha) en formato label-arriba/valor-abajo.
7. Pie de portada: nota de confidencialidad a la izquierda + número destacado (el dato más importante
   del informe, ej. "86 casos revisados") a la derecha, separados por una línea sutil.

---

## 6. Componentes reutilizables

### 6.1 Tarjetas de estadística (`stat-card`)
Fila flexible de tarjetas con borde superior de 3px en azul (o rojo si es una alerta, o verde si es
positivo), fondo gris muy claro, número grande arriba y etiqueta pequeña abajo. Se usan para resumir
las cifras clave de una sección en 4-6 tarjetas máximo.

### 6.2 Callouts (recuadros de énfasis)
Cuatro variantes, todas con barra de color a la izquierda de 4px y fondo tenue del mismo color:
- **Callout azul** (`--azure`): el hallazgo o mensaje más importante de la sección.
- **Caja ámbar** (`--amber`): advertencia media / algo a vigilar.
- **Caja roja** (`--red`): riesgo, urgencia, escalamiento.
- **Caja gris neutra** (`--gray-500`): notas metodológicas o limitaciones del análisis.

### 6.3 Fichas de grupo/categoría (`group-card`)
Para agrupar hallazgos por categoría (ej. "P-01 — Fechas de entrada y salida"):
- Encabezado con fondo `--navy` sólido: badge con el ID en azul sobre fondo navy, título, y
  contador ("27 casos · 31 % del total") alineado a la derecha.
- Cuerpo blanco con: resumen en una frase, sub-hallazgos (título bold + descripción + lista
  monoespaciada pequeña de IDs relacionados), nota destacada opcional, y al final la lista completa
  de IDs incluidos en un bloque gris pequeño.
- Borde redondeado, `page-break-inside: avoid`.

### 6.4 Tags y badges
- **Tag** (etiqueta de categoría, ej. "P-02"): píldora pequeña, fondo `--azure-light`, borde
  `--azure-mid`, texto `--navy-2` bold, 7.2pt.
- **Badge de estado** (ej. "Completado", "Abierta"): píldora con esquinas muy redondeadas (10px),
  colores semánticos de la sección 2, 7.4pt bold, sin borde.

### 6.5 Tablas de datos
- Encabezado con fondo `--navy-2` sólido, texto blanco, bold, 7.3–7.8pt.
- Filas alternas con fondo `--gray-50` (zebra striping suave).
- Columna de ID de caso en monoespaciada bold color `--navy-2`.
- Columnas numéricas alineadas a la derecha con `tabular-nums`.
- Bordes solo horizontales (`border-bottom` en celdas), nunca grid completo — se ve menos denso.

### 6.6 Tarjetas de recomendación numerada (`rec`)
Fila horizontal: círculo grande con el número (fondo `--azure-light`, borde azul, número en navy),
luego título bold + descripción + una etiqueta pequeña verde de "impacto/beneficio esperado".
Se numeran en orden de prioridad (mayor impacto / menor esfuerzo primero).

### 6.7 Tarjetas de pendiente/urgencia (`pend`)
Borde izquierdo grueso (4px) en rojo (si está "Abierta") o ámbar (si está "Pending"), con el ID del
caso, un badge de estado y un contador de "días abierto" destacado en el mismo color del borde.

### 6.8 Gráficos (matplotlib)
- Fondo transparente siempre (`transparent=True` al guardar).
- Paleta: barras principales en `--navy-2` o `--azure` (alternar según si se quiere destacar un
  subconjunto); línea de tendencia en `--azure`; grid horizontal sutil en `--gray-300`, sin bordes
  superior/derecho/izquierdo (`spines` ocultos salvo el inferior).
- Tipografía Liberation Sans, mismo tamaño relativo que el cuerpo del documento.
- Preferir: barras horizontales para rankings de categorías, barras verticales + línea para series
  de tiempo, donut chart para composición de un total (con el total grande en el centro), barras
  horizontales cortas para distribuciones simples (ej. prioridad).
- Nunca usar 3D, sombras pesadas ni degradados llamativos en los gráficos — deben verse tan sobrios
  como el resto del documento.

---

## 7. Tono y estructura del contenido

- Se conserva la numeración de secciones del documento fuente (1. Resumen, 2. Metodología, …) tal
  cual, incluso si eso significa secciones muy cortas.
- Los datos van en `[stated]` — es decir, tal como los dio la fuente. No se interpreta, no se corrige,
  no se redondea "para que cuadre".
- Las citas textuales de comentarios (ej. de Jira) se mantienen entre comillas y en cursiva/gris para
  diferenciarlas de la prosa del informe.
- El pie del documento siempre incluye una nota de metodología/alcance en cursiva pequeña (qué se
  revisó, qué no se abrió/verificó).
- Todo caso, ticket o ID mencionado en el cuerpo se referencia igual (mismo formato) en el que aparece
  en la fuente — no se abrevia ni se cambia el prefijo.

---

## 8. Motor técnico recomendado

- **HTML + CSS → PDF con WeasyPrint** (Python). Es la ruta más flexible para lograr tablas
  complejas, `@page` con encabezado/pie corriendo, y `page-break` control — mucho más rico que
  dibujar con reportlab a mano.
- Los gráficos se generan aparte con **matplotlib** (fondo transparente, paleta BTG) y se insertan
  como imagen embebida en base64 (`<img src="data:image/png;base64,...">`), para no depender de
  archivos externos al mover el HTML.
- Los datos tabulares se separan en estructuras Python (listas de tuplas/diccionarios) ANTES de
  construir el HTML, para no transcribir tablas largas a mano dentro de strings HTML — reduce el
  riesgo de error de transcripción en documentos con muchas filas.
- El flujo es: **datos estructurados → CSS/plantilla de componentes → generación de HTML →
  render a PDF → control de calidad visual (renderizar páginas clave a imagen y revisarlas) → ajuste
  → entrega.**

---

## 9. Prompt listo para reutilizar con cualquier IA

Copia y pega esto (junto con tus datos/documento fuente) para pedir un nuevo informe con este estilo:

> Genera un PDF corporativo para BTG Pactual siguiendo exactamente estos lineamientos de estilo:
> [pega aquí el contenido completo de este documento]. Usa como base los datos del siguiente
> documento: [pega o adjunta tu documento fuente]. No inventes ni corrijas cifras — reproduce los
> datos tal como aparecen en la fuente. Genera portada, resumen ejecutivo con tarjetas de
> estadísticas, secciones numeradas iguales a las del documento fuente, tablas con badges de estado
> donde aplique, y un anexo numérico al final.
