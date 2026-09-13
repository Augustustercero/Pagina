# PASO 3 — Dirección visual: color y tipografía

Documento de contexto del proyecto. Versión corregida y ampliada.

**Estado de este paso.** La versión original de este documento era un brief exploratorio: nada estaba cerrado. En conversación se probaron variantes reales sobre pantalla y se tomaron decisiones. **Los valores de este documento ya no son exploratorios: son la especificación de trabajo.**

Siguen siendo reemplazables — todo se implementa como variables, nunca escrito a mano en cada pantalla — pero el equipo debe construir con estos valores, no proponer otros.

---

## 1. El problema de diseño que se resolvió

La marca vive en la tensión entre dos polos (ver Paso 2): seriedad con los números más cercanía humana real. Traducido a lo visual:

- **Demasiado institucional** (azules corporativos sobre blanco, negro, gris, alta densidad tipo terminal Bloomberg) → se siente banco, frío, impersonal. Pierde la cercanía que es el diferencial.
- **Demasiado cálido/informal** (pasteles, ilustraciones, tipografías redondeadas, mucho color) → se siente app de finanzas personales para principiantes. Pierde la credibilidad que necesita un producto que vende análisis a gente que ya invierte.

La solución que se encontró: **fondos cálidos de baja saturación con un color de marca institucional encima.** El fondo aporta la cercanía y la comodidad de lectura; el azul aporta la seriedad. Ninguno de los dos solo lograba el equilibrio.

Un aprendizaje del proceso, que conviene tener presente para futuras decisiones: **los colores de fondo se juzgan a pantalla completa, no en muestras.** El marrón carbón y el arena original se veían bien como rectángulo chico y resultaron pesados llenando una pantalla. Toda propuesta nueva de fondo debe evaluarse aplicada, no como swatch.

## 2. Requisitos funcionales que la paleta cumple

1. **Lectura larga cómoda.** La plataforma tiene análisis de hasta 2.500 palabras. Nunca blanco puro sobre negro puro. El resultado buscado y confirmado en prueba real es el de un modo lectura por defecto: con el brillo del celular al máximo, no molesta la vista.
2. **Verde y rojo como utilitarios, no como marca.** Saturación media: que contrasten y se distingan, sin gritar. No los verdes y rojos fluorescentes tipo semáforo de las apps de trading.
3. **Soporta densidad de datos.** Tablas, gráficos, mapas de calor, paneles de números, además del modo artículo editorial.
4. **Soporta el estado bloqueado/difuminado**, que se repite en todo el producto.
5. **Estatus sin ostentación.** Sin dorados, sin brillos, sin efectos premium.
6. **Accesible.** Todo texto con contraste suficiente. El color nunca es el único portador de información.

## 3. Paleta base — valores definidos

| Rol | Modo claro | Modo oscuro |
|---|---|---|
| Fondo de página | `#F9F5EC` | `#1A1917` |
| Superficie de tarjeta | `#FEFCF8` | `#232220` |
| Borde | `#EAE4D6` | `#332F2A` |
| **Color de marca** | `#0D2438` | `#2A5F85` |
| Texto principal | `#262420` | `#E8E4DA` |
| Texto secundario | `#6B6350` | `#9A9284` |
| Ganancia | `#2F7A4E` | `#6BAF6B` |
| Pérdida | `#B84A3F` | `#D2726A` |
| Tag de ticker — fondo / texto | `#F3EBD5` / `#6B5A28` | `#332C1E` / `#D6C08A` |
| Tag de sector — fondo / texto | `#EFEADD` / `#6B6350` | `#2C2A26` / `#A8A093` |

### Notas sobre estos valores

**El fondo claro es "crema apenas".** Se probaron cuatro niveles desde el arena original hacia el crema y se eligió el primer escalón: más luminoso que el arena, pero todavía claramente cálido. Los niveles más claros perdían carácter y hacían que la tarjeta dejara de despegarse del fondo.

**El fondo oscuro es gris cálido neutro.** Se descartó el marrón carbón porque a pantalla completa se vuelve pesado, y se descartó el gris azulado frío por alejarse de la calidez que define la marca. El gris cálido no compromete ninguna decisión de color de marca.

**El color de marca cambia de valor entre modos, y es obligatorio que así sea.** El azul institucional `#0D2438` es casi tan oscuro como el fondo del modo oscuro: sin adaptar, el header y los botones desaparecen. La versión aclarada `#2A5F85` mantiene el reconocimiento de marca con la presencia necesaria. Esto es normal en productos con dos modos y no debe interpretarse como una inconsistencia.

**El azul institucional se había descartado en la versión original** de este documento por sentirse "demasiado banco". Se revirtió: contra el fondo crema pierde la frialdad que tenía contra un fondo blanco, y se lee más como papel de informe que como sucursal bancaria. El azul pesa visualmente sobre el crema y eso está aceptado y buscado.

**Los tags salieron del verde.** La propuesta original los tenía en verde salvia, lo que generaba tres verdes distintos en pantalla (marca, tags, ganancia). Como la marca terminó siendo azul, el verde quedó reservado exclusivamente para ganancia, y los tags pasaron a un amarillo suave de bajo contraste. Los tags de sector usan un neutro arena para diferenciarse de los de ticker.

**Contraste corregido.** El texto secundario de la propuesta original (`#7A7057`) no alcanzaba el mínimo de 4,5:1 sobre el fondo. Los valores de esta tabla lo cumplen.

## 4. Ganancia y pérdida — reglas

- **Saturación media**, no alta. El portfolio es donde el usuario ve su propia plata: es el peor lugar para meter adrenalina.
- **El signo `+` o `−` está siempre presente.** El color nunca es el único indicador. Esto es lo que hace que la información siga siendo legible incluso para quien no sepa que existe el modo daltonismo o no lo haya activado.
- **Modo daltonismo:** un interruptor cambia el par verde-rojo por **azul-naranja**, que se distingue bien en las tres formas más comunes de daltonismo. Un solo modo alcanza; no hacen falta modos separados por tipo.
- Los valores concretos del par azul-naranja, en ambos modos, están pendientes de definir.

## 5. Tipografía — decisiones tomadas

| Rol | Familia | Notas |
|---|---|---|
| **Títulos y subtítulos** | **Literata** | Serif diseñada específicamente para lectura prolongada en pantalla. Bajo contraste entre trazos, formas abiertas. |
| **Cuerpo de texto** | **IBM Plex Sans** | Humanista, formas abiertas, cálida. |
| **Datos y números** | **IBM Plex Sans** con cifras tabulares (`font-feature-settings: 'tnum'`) | Misma familia que el cuerpo. |

Las tres son gratuitas. **No se paga por tipografía**, decisión cerrada: quedan descartadas Canela, GT Sectra y cualquier otra licencia comercial.

### Por qué estas y no las de la propuesta original

**Literata reemplaza a Fraunces.** Fraunces gustó y tiene más personalidad, pero tiene mucho contraste entre trazos gruesos y finos, lo que exige más del ojo. El criterio decisivo fue el contexto de lectura: el usuario típico lee volviendo del trabajo o recién levantado, con la vista cansada. Literata está diseñada exactamente para eso. También se evaluaron Newsreader, Petrona y Bitter.

**IBM Plex Sans reemplaza a Karla.** Karla es cálida y humanista, pero tiene formas más cerradas y menor altura de letra proporcional, lo que la vuelve más exigente con vista cansada. IBM Plex Sans conserva el carácter humanista (el fundamento original de descartar Inter sigue vigente: las geométricas puras se sienten frías y hechas por máquina) y suma dos ventajas concretas:

- **Unifica cuerpo y datos en una sola familia**, así el producto entero usa dos tipografías en total.
- **Al no ser monoespaciada**, un número dentro de un párrafo no rompe el ritmo de la línea, cosa que sí pasa con las mono.

Para datos se evaluaron también IBM Plex Mono, JetBrains Mono, Roboto Mono e Inter. Se descartaron las mono por leerse demasiado "terminal", que es explícitamente lo que la marca no quiere ser.

### Parámetros de composición

- **Cuerpo de texto: 17px, interlineado 1,75.** Estos valores no son decorativos: en lectura larga el interlineado y el tamaño pesan más que la elección de tipografía. Un texto bien compuesto en cualquier sans decente cansa menos que un texto apretado en la mejor tipografía.
- **Ancho de lectura angosto: 60 a 75 caracteres por renglón** (aproximadamente 10 a 12 palabras). Igual en noticia estándar y en ampliada. El ancho no se ensancha para las notas largas: el largo del renglón es lo que cansa el ojo, no el largo de la nota, así que en la ampliada el ancho angosto ayuda todavía más.
- **Modo oscuro: el cuerpo nunca en blanco puro.** El texto va en blanco crudo (`#E8E4DA`), porque el blanco puro sobre fondo oscuro produce halo y fatiga.
- **Logo/wordmark derecho, no en itálica.** Se probó itálica y se prefirió la versión recta.

## 6. Dos densidades

La instrucción original era "calma y densidad a la vez". Se resolvió separando por zona:

- **Zona de lectura** (noticias, daily, análisis, glosario): más aire. Interlineado generoso, párrafos separados, márgenes amplios, ancho angosto. El scroll no se percibe como problema si el texto fluye.
- **Zona de herramientas** (portfolio, tablas, calendario, congresistas, mapas): compacta. El usuario quiere ver muchos números de un vistazo y comparar. El aire estorba.

No es una contradicción: son dos modos del mismo producto.

## 7. La firma del autor

Al final de cada nota, no arriba. **No va byline de tipo "Por [nombre]" en el encabezado.**

- Nombre y apellido reales del autor, **en cursiva**.
- Al lado o debajo, un **sello circular estilo lacre**, con el logo de la marca en el medio.
- La lógica: cerrar como una carta. El lector termina de leer y recién ahí se encuentra con quién se lo escribió. Es más personal que un byline de diario.

**Pendientes de este elemento:**

- La tipografía exacta de la firma (una cursiva o script, distinta de las tres familias del sistema).
- **El color del lacre.** El lacre tradicional es rojo, pero el rojo ya está asignado a pérdida en este producto. Un sello rojo al pie de una nota podría leerse como señal negativa. Alternativas a evaluar: lacre en el azul de marca, o en un burdeos suficientemente distinto del rojo de pérdida.

## 8. Lo que la dirección visual NO debe hacer

- **No imitar identidades de medios existentes.** Nada que se lea como copia de NYT, WSJ, Bloomberg o Financial Times.
- **No usar la estética de app de trading.** Nada de fondos negros con verdes y rojos fluorescentes, gráficos de velas como decoración, ni sensación de terminal.
- **No usar señales políticas.** Se descartó la tipografía asociada a la bandera de Malvinas por su carga política.
- **No caer en clichés de premium.** Sin dorados, sin degradados brillantes, sin efectos de vidrio.
- **No sobrecargar visualmente.** El usuario debe sentir calma. Espacios de respiro, jerarquía clara.
- **No escribir colores a mano en las pantallas.** Todo color, tamaño y espaciado va como variable. Cambiar la identidad debe ser cambiar valores en un solo lugar, nunca recorrer pantallas.

## 9. Fundamento de la dirección humanista

Vale que el equipo entienda el porqué, no solo el qué. Existe una corriente actual de reacción humanista en diseño — tipografías con trazos visibles, leves imperfecciones ópticas, curvas cálidas, paletas tierra no saturadas — que surge precisamente porque todo se siente hecho por máquina y la gente busca lo que se siente hecho por una mano humana. Esto encaja exactamente con la propuesta de valor del proyecto: hay una persona real con criterio del otro lado, no un agregador automático.

Además, las paletas cálidas y no saturadas reducen la fatiga visual, lo cual importa mucho en un producto de lectura larga.

## 10. Cómo validar

El autor tiene criterio de mercados, no de diseño, y lo sabe. Antes de fijar definitivamente la identidad conviene **probar dos o tres direcciones con gente real** — aunque sean cinco conocidos —, porque esto es percepción y no tiene respuesta matemática. Está previsto hacerlo.

Esto no invalida los valores de este documento: son la base sobre la que se construye. Si la validación con usuarios sugiere ajustes, se cambian los valores de las variables sin tocar la estructura.

---

## 11. Pendientes de este paso

- **Valores del par azul-naranja** del modo daltonismo, en ambos modos.
- **Tipografía de la firma** del autor.
- **Color del lacre** del sello (ver 7).
- **Tratamiento visual del estado bloqueado/difuminado**: opacidad, nivel de desenfoque y color del overlay. Se define al diseñar las pantallas donde aparece.
- **Peso visual del header.** El azul institucional sobre crema pesa y eso está aceptado, pero queda por definir la altura de la barra y si el azul se usa también en otros elementos o queda reservado al header y a los botones principales.
