# PASO 4 — Estructura de planes y matriz de permisos

Documento de contexto del proyecto. Versión corregida y ampliada.

**Qué es este documento.** Define exactamente qué puede hacer cada usuario según su nivel de acceso. Es la referencia de permisos del producto: ante cualquier duda de "¿este usuario puede hacer X?", la respuesta está acá.

**Cómo leerlo.** Está escrito para que no haga falta interpretar nada. Donde hay una regla, está escrita como regla. Donde algo no está definido, dice explícitamente que no está definido, en vez de dejarlo implícito. Si al implementar aparece un caso que este documento no cubre, **hay que preguntar, no asumir**.

**Advertencia sobre la versión anterior.** La primera versión de este documento tenía varias cosas que quedaron sin efecto. Están marcadas a lo largo del texto con el rótulo "Corrección". Si el equipo tiene una copia vieja circulando, **esta versión la reemplaza por completo**.

---

## 1. Los cinco niveles de acceso

| # | Nivel | Precio | Requiere cuenta |
|---|---|---|---|
| 0 | Anónimo | — | No |
| 1 | Gratuito | USD 0 | Sí |
| 2 | Nivel 15 | USD 15/mes | Sí |
| 3 | Nivel 30 | USD 30/mes | Sí |
| 4 | Nivel 60 | USD 60/mes | Sí |

**Los nombres de los planes no están definidos.** En este documento se los llama por el precio para evitar ambigüedad. En la interfaz van a llevar nombre propio, que se define más adelante (ver Paso 2, sección 6). **El código no debe hardcodear ni el nombre ni el precio como identificador del nivel** — se usa un identificador interno estable (por ejemplo `tier_0` a `tier_4`) y el nombre y el precio se leen de configuración. Cambiar un precio o un nombre no puede requerir tocar la lógica de permisos.

**Los niveles son acumulativos.** Cada nivel incluye todo lo del nivel anterior, más lo propio. No hay ninguna funcionalidad que exista en un nivel bajo y desaparezca en uno alto. Esta regla no tiene excepciones y sirve como verificación: si al implementar aparece algo que un nivel alto no puede hacer y uno bajo sí, es un error.

**La mentoría 1:1 no es un nivel.** No aparece en esta escala, no se compara con estos planes, y no requiere ninguno de ellos. Ver sección 12.

---

## 2. Matriz completa de permisos

Esta tabla es la fuente de verdad. El resto del documento la explica en detalle.

| Funcionalidad | Anónimo | Gratuito | $15 | $30 | $60 |
|---|---|---|---|---|---|
| **CONTENIDO** | | | | | |
| Home y listado de noticias | Sí | Sí | Sí | Sí | Sí |
| Noticia estándar (500-750 palabras) | **Cortada** | Completa | Completa | Completa | Completa |
| Daily en texto (300-400 palabras, 2 temas) | No | Sí | Sí | Sí | Sí |
| Daily narrado en audio | No | Sí | Sí | Sí | Sí |
| Podcast mensual (texto y audio) | No | Sí | Sí | Sí | Sí |
| Audio de cada nota individual | No | No | Sí | Sí | Sí |
| Noticia ampliada (2.000-2.500 palabras) | No | No | No | Sí | Sí |
| Glosario financiero | No | Sí | Sí | Sí | Sí |
| Avisos "cuando salga el dato" (por mail) | No | Sí | Sí | Sí | Sí |
| **PORTFOLIO** | | | | | |
| Portfolio tracker | Difuminado | Sí | Sí | Sí | Sí |
| Cantidad de portfolios | — | 1 | 1 | Ilimitados | Ilimitados |
| Tope de activos vivos por portfolio | — | 6 | 6 | Sin tope | Sin tope |
| Contabilidad por lotes | — | Sí | Sí | Sí | Sí |
| Vista simple y vista por lotes | — | Sí | Sí | Sí | Sí |
| Ganancia realizada y no realizada separadas | — | Sí | Sí | Sí | Sí |
| Calculadora de comisiones | — | Sí | Sí | Sí | Sí |
| Calculadora de impuestos | — | Sí | Sí | Sí | Sí |
| Historial de posiciones cerradas | — | Sí | Sí | Sí | Sí |
| **HERRAMIENTAS** | | | | | |
| Calendario económico | Difuminado | Sí | Sí | Sí | Sí |
| Simulador de escenarios históricos | Difuminado | Sí | Sí | Sí | Sí |
| Correlación entre activos | Difuminado | No | No | No | Sí |
| Trading de congresistas | Difuminado | No | No | No | Sí |
| Dark pool | Difuminado | No | No | No | Sí |
| Flujo de opciones | Difuminado | No | No | No | Sí |
| Mapa de calor de liquidaciones | Difuminado | No | No | No | Sí |
| Los 3 portfolios del autor con benchmark | Difuminado | No | No | No | Sí |
| **ALERTAS** | | | | | |
| Alertas de trading por WhatsApp | No | No | No | No | Sí |
| **OTROS** | | | | | |
| Chatbot | Difuminado | No | No | No | Sí |
| Modo oscuro | Sí | Sí | Sí | Sí | Sí |
| Modo daltonismo | Sí | Sí | Sí | Sí | Sí |
| Selector de idioma | Sí | Sí | Sí | Sí | Sí |

**Modo oscuro, daltonismo e idioma nunca se cobran.** Son accesibilidad y preferencia, no funcionalidad premium. Disponibles incluso para el anónimo.

---

## 3. Nivel 0 — Anónimo

Alguien que entra al sitio sin cuenta. Típicamente llega desde Google o desde un link compartido.

### 3.1 Qué ve

- **La home completa**, con el listado de noticias, títulos, bajadas y tags. Sin ningún candado.
- **Las noticias, cortadas.** Ver 3.2.
- **Todas las herramientas presentes en la interfaz, difuminadas**, con dato real borroso de fondo. No se esconden ni se sacan del menú.

### 3.2 El corte de la noticia

Este mecanismo es central y tiene reglas estrictas.

**Comportamiento:**

1. La **primera pantalla se ve entera y sin ningún candado**. El usuario no percibe que hay un muro cuando llega.
2. Al scrollear, el texto **se desvanece progresivamente hasta desaparecer por completo**.
3. En el punto donde el texto ya no se ve, aparece la invitación a registrarse gratis.
4. A partir de ahí no hay más contenido: o se registra, o no sigue leyendo.

**Reglas de implementación, todas obligatorias:**

- **El corte se define por posición en la estructura del texto, nunca por altura fija en píxeles.** Una altura fija haría que en desktop se vea casi toda la nota y en celular tres párrafos. El corte lo marca el autor al escribir, en un punto de la estructura editorial.
- **El corte se aplica a todas las notas por igual.** No existe el esquema de "una nota gratis y el resto bloqueadas". No hay contador de notas, no hay cookies de "ya leíste tu nota del mes".
- **No hay ícono de candado.** El tratamiento es únicamente el desvanecido del texto. Un candado grita "pagá"; el texto que se desvanece genera ganas de seguir. Esta decisión está cerrada.
- **REQUISITO TÉCNICO CRÍTICO: el texto bloqueado no puede estar cargado en la página.** No se puede servir la nota completa en el HTML y taparla con CSS. Tiene que servirse recién después del registro, desde el servidor.

  Motivo: si está en el HTML, cualquiera lo lee desactivando el estilo desde las herramientas del navegador, y los buscadores lo indexan como contenido público. Las dos cosas anulan el muro por completo. **Este punto no es negociable y hay que verificarlo explícitamente en el testing.**

**Fundamento de la decisión, por si hace falta explicarla:** es más honesto (el usuario ve el límite desde el primer momento y lo que leyó ya le dio valor real, así que no se lee como clickbait) y deja contenido público indexable en cada nota, lo cual atrae tráfico de buscadores. Tapar notas enteras dejaría a Google sin nada que mostrar.

### 3.3 Qué NO tiene el anónimo

Daily, glosario, portfolio, ninguna herramienta usable, ningún audio, ninguna alerta.

---

## 4. Nivel 1 — Gratuito

> **Corrección respecto de la versión original.** La versión anterior describía el portfolio del gratuito como una versión recortada, con la calculadora de impuestos y la contabilidad por lotes reservadas al plan de $30. **Eso quedó sin efecto.** El portfolio del gratuito es funcionalmente idéntico al del plan más caro. La única diferencia es de escala.

### 4.1 Qué suma respecto del anónimo

- **Noticia estándar completa**, sin corte.
- **Daily**, en texto y en audio narrado.
- **Podcast mensual**, en texto y en audio.
- **Glosario financiero completo.**
- **Portfolio tracker**, con tope de 1 portfolio y 6 activos vivos. Funcionalidad completa dentro de ese tope.
- **Calendario económico**, completo y sin límite. No se cobra.
- **Simulador de escenarios históricos**, completo y sin límite. No se cobra.
- **Avisos por mail** de "avisame cuando salga este dato".

### 4.2 Qué NO tiene

- Audio de las notas individuales.
- Noticia ampliada.
- Datos institucionales (congresistas, dark pool, opciones, mapa de calor).
- Los 3 portfolios del autor con benchmark.
- Alertas por WhatsApp.
- Más de 1 portfolio o más de 6 activos.

### 4.3 Condición obligatoria del nivel gratuito

El acceso gratuito **requiere aceptar el checkbox de publicidad**. Si el usuario no lo acepta, el único camino es pagar el plan de $15. El detalle completo de este mecanismo (los dos checkboxes separados, el modal de explicación, el copy y el motivo regulatorio) está en el Paso 1, sección 6, y no se repite acá.

---

## 5. Nivel 2 — $15/mes

### 5.1 Qué suma

**Todo el audio del producto.** El audio narrado de cada nota individual, además del daily y el podcast que ya venían del gratuito.

### 5.2 Qué NO suma

**Nada más.** Este plan no cambia el portfolio, no agrega herramientas y no da acceso a la noticia ampliada.

Esto está decidido a propósito y hay que dejarlo escrito para que no se "arregle" por iniciativa propia: **el plan de $15 tiene dos funciones distintas de la de vender features.**

1. Es la salida para quien no quiere aceptar publicidad. Es el precio de la privacidad, no de las funcionalidades.
2. Es el escalón de entrada al pago. Convierte a un usuario gratuito en un usuario que paga, que es el salto psicológico más difícil de todos.

Si alguien del equipo nota que "el de $15 tiene muy poco", la respuesta es que es correcto y deliberado.

---

## 6. Nivel 3 — $30/mes

### 6.1 Qué suma

**Noticia ampliada.** El análisis extendido, de 2.000 a 2.500 palabras, con el mecanismo causal completo: por qué pasó lo que pasó, a qué sectores y activos afecta, y cómo se conecta con lo anterior.

> **Corrección respecto de la versión original.** El documento anterior ubicaba la noticia ampliada en el plan de $60. **Corresponde al de $30.**

**Portfolios ilimitados, activos sin tope.** El usuario puede crear todos los portfolios que quiera, cada uno con su nombre propio, y cargar todos los activos que quiera en cada uno.

### 6.2 Qué NO suma

Datos institucionales, portfolios del autor, alertas por WhatsApp.

---

## 7. Nivel 4 — $60/mes

### 7.1 Qué suma

**Los datos institucionales completos:**

- Trading de congresistas de Estados Unidos.
- Dark pool.
- Flujo de opciones.
- Mapa de calor de liquidaciones.

**Correlación entre activos.**

**Chatbot.** Con una excepción importante, ver 7.3.

**Los 3 portfolios del autor con benchmark.** Ver sección 10.

**Alertas de trading por WhatsApp.** Ver sección 11.

### 7.2 Por qué estos datos van juntos en el nivel más alto

Son los datos que el usuario no consigue en ningún otro lado sin pagar mucho más. Es el argumento de valor del nivel, y también el que naturalmente conecta con la mentoría: quien paga por ver el flujo institucional es quien más probablemente quiera que alguien se lo interprete.

### 7.3 El chatbot es exclusivo de este nivel, sin excepciones

El chatbot **no funciona en ningún otro nivel, para ninguna función**. Esto incluye el autocompletado de comisiones del portfolio (sección 9.8).

Consecuencia, aceptada a conciencia: **los usuarios de los niveles gratuito, $15 y $30 cargan el porcentaje de comisiones a mano.** No se les propone ningún valor, no hay búsqueda automática, no hay sugerencia. Un campo vacío que completan ellos.

Esto significa que el portfolio del gratuito **no es exactamente idéntico** al del nivel de $60: le falta esa comodidad. Es una diferencia menor y está aceptada. El principio de la sección 9.1 sigue vigente en lo que importa — el motor de cálculo, la precisión de los números y todas las vistas son iguales en todos los niveles. Lo único que cambia es quién escribe el porcentaje.

---

## 8. El módulo de contenido en detalle

### 8.1 Tipos de contenido y sus formatos

| Tipo | Extensión | Frecuencia | Nivel mínimo |
|---|---|---|---|
| Daily | 300-400 palabras, 2 temas | Diaria | Gratuito |
| Noticia estándar | 500-750 palabras | Varias por día | Gratuito (cortada para el anónimo) |
| Noticia ampliada | 2.000-2.500 palabras | Según el tema | $30 |
| Podcast mensual | Formato largo | Mensual | Gratuito |

### 8.2 Reglas del daily

- Exactamente **dos temas**, no tres ni cinco.
- **300 a 400 palabras** en total. Corto, para leer rápido.
- Va siempre acompañado de su **audio narrado**, con la voz clonada del autor.
- **El daily con audio es gratuito.** Esto es deliberado: es la primera vez que el usuario escucha la voz del autor, y la cercanía por audio no se cobra. Lo que se cobra es el volumen y la profundidad.

### 8.3 Reglas de la noticia estándar

- **500 a 750 palabras.**
- **No lleva TL;DR.** El resumen arriba existe solo en la ampliada. Poner un resumen en una nota de 600 palabras invita a no leerla.
- Lleva **tags de ticker y de sector** (ver 8.7).
- Termina con la **firma del autor** (ver Paso 3, sección 7).

### 8.4 Reglas de la noticia ampliada

- **2.000 a 2.500 palabras.**
- **Lleva TL;DR arriba.**
- **Índice compacto arriba**, con los subtítulos de la nota, para saltar a una sección.
- **Encabezado fijo con indicador de progreso** de lectura mientras se scrollea.
- **NO lleva secciones colapsables.** Esta decisión está cerrada y tiene fundamento: esconder el mecanismo causal detrás de un desplegable le da permiso al lector para saltear justamente la parte que justifica el precio del plan.
- **El ancho de lectura es el mismo que en la nota estándar** (60-75 caracteres por renglón). No se ensancha porque la nota sea más larga. Lo que cansa el ojo es el largo del renglón, no el largo de la nota, así que en la ampliada el ancho angosto ayuda todavía más.

Hay un dato del que conviene estar advertido al diseñar esta pantalla: entre las notas largas, economía y negocios es la categoría que **menos retiene al lector**. La navegación interna (índice, progreso, subtítulos frecuentes) no es decoración: es lo que hace que la nota se termine de leer.

### 8.5 Las glosas de términos técnicos

Cuando aparece un término técnico, se explica **en prosa, dentro del texto corrido**.

- **Extensión: 2 o 3 oraciones.** Qué es el término y por qué importa en este caso puntual.
- **La única marca visual es la cursiva en el término.**
- **NO va en tooltip. NO va en recuadro lateral. NO va en nota al pie.** La glosa es prosa, no un elemento de interfaz.
- La explicación exhaustiva del término vive en el **glosario**, que es una sección aparte.

Este punto se malinterpreta fácil, así que se dice de otra forma: si el desarrollador está construyendo un componente para mostrar glosas, se equivocó. No hay componente. Es texto.

### 8.6 El hilo entre notas y las notas parecidas

Son dos cosas distintas y **no deben mezclarse en el mismo bloque**.

**El hilo** es el argumento del autor en desarrollo a lo largo de varias notas. Es curado por el autor, tiene orden y tiene veredicto.

- Va **arriba y al final de la nota**, con tratamiento visual propio y visible.
- Muestra la posición dentro de la serie: "nota 5 de 5".
- Muestra el **veredicto de las notas anteriores**, no solo el título.

**Las notas parecidas** son sugerencias automáticas por tema o ticker.

- Van **más abajo, en un bloque separado y visualmente secundario**.

Motivo de la separación: si van juntas, el hilo pierde peso y queda como una recomendación más. Una cosa es la tesis del autor y otra es "más contenido".

### 8.7 Tags

- Cada nota lleva tags de **ticker** y de **sector**.
- **Función principal: son internos.** Alimentan el sistema de recomendaciones y el chatbot.
- **Función secundaria: filtrado.** Tiene que existir un lugar en la sección de noticias donde el usuario pueda filtrar por tag. El diseño concreto de ese filtro está pendiente.
- Los dos tipos de tag tienen tratamiento visual distinto (ver Paso 3, sección 3).

### 8.8 "Avisame cuando salga el dato"

Al final de cada nota hay una opción para que el sistema avise cuando se publique el dato mencionado.

- **El aviso va por mail, para todos los planes.**
- **NO va por WhatsApp**, en ningún nivel, ni siquiera en el de $60.

Motivo: WhatsApp se reserva para las alertas de trading y los avisos de campaña y vencimiento. Si el canal se satura, el usuario reporta el número, y unos pocos reportes alcanzan para que WhatsApp lo bloquee. Perder el número sería perder el canal entero.

### 8.9 Guardar posición de lectura

- El sistema guarda la posición del scroll cuando el usuario se va de una nota.
- Al volver, aparece un cartel arriba: **"Volver donde quedaste"**. Con un tap baja hasta ahí.
- Si no lo toca, sigue leyendo desde arriba con normalidad.
- **Con cuenta iniciada, funciona incluso si cambia de dispositivo.**

### 8.10 El glosario

- Disponible desde el **nivel gratuito**. Es una decisión de marca: el autor quiere que la gente aprenda de finanzas le pague o no.
- Contiene la explicación **exhaustiva** de cada término, a diferencia de las glosas en las notas, que son breves y contextuales.

**Cómo se alimenta (requiere pantalla nueva):**

1. El sistema detecta términos técnicos en cada nota nueva.
2. Los cruza contra la lista de términos **ya aprobados** y contra la de **ya rechazados**.
3. Le presenta al autor **solo los términos nuevos**, con una definición propuesta.
4. El autor aprueba o rechaza uno por uno.
5. Los rechazados se recuerdan, para no volver a proponerlos.

Esto implica un **panel de desarrollador / administración** que no estaba en el inventario original de pantallas. Ver Paso 5.

---

## 9. El módulo de portfolio en detalle

Es el módulo más complejo del producto y el que más reglas tiene. Conviene leer esta sección entera antes de implementar cualquier parte.

### 9.1 Principio general

**Hay un solo motor de portfolio.** No existe una versión degradada para el gratuito y una completa para los pagos. Todos los niveles usan el mismo motor, con la misma precisión de cálculo.

**La única diferencia entre niveles es de escala:**

| | Gratuito y $15 | $30 y $60 |
|---|---|---|
| Cantidad de portfolios | 1 | Ilimitados |
| Activos vivos por portfolio | 6 | Sin tope |

Todo lo demás es idéntico, con **una sola excepción menor**: el autocompletado de comisiones por chatbot funciona únicamente en el nivel de $60 (ver 7.3 y 9.8). En los demás niveles el usuario escribe el porcentaje a mano. El cálculo resultante es exactamente el mismo.

**Por qué está decidido así, para que no se cambie por iniciativa propia:** un portfolio que calcula mal, o que muestra un número bruto sin descontar comisiones, no genera ganas de pagar. Genera desconfianza en los números. Y como el producto vende criterio y rigor con los datos, esa desconfianza se contagia al resto del contenido, que es el activo central de la marca. Un portfolio que funciona impecable y se topa con un techo de tamaño convierte mucho mejor que uno mutilado.

**Explícitamente prohibido:** degradar cálculos, esconder el neto, o introducir errores a propósito en el nivel gratuito para forzar la conversión. Esto se evaluó y se descartó.

### 9.2 Contabilidad por lotes

**Los lotes se registran siempre, en todos los niveles, sin excepción.** Cada compra de un activo es un lote con su propia fecha y su propio precio.

Esto es cierto incluso si el usuario está mirando la vista simple. **La vista simple esconde los lotes, no los desactiva.** El registro por debajo es siempre el mismo.

Consecuencia práctica para el desarrollador: el modelo de datos es siempre por lotes. La vista es una capa de presentación, no un modo de funcionamiento distinto.

### 9.3 Las dos vistas

> **Corrección respecto de la versión original.** El documento anterior hablaba de un "modo simple" y un "modo avanzado" que cambiaban el comportamiento de la herramienta. **Eso quedó sin efecto.** No son dos modos: son dos formas de mirar los mismos datos.

**Vista por lotes (la vista por defecto).**

Es a la que se entra siempre la primera vez. Muestra:

- Cada lote por separado, con su fecha, su precio de compra y su ganancia o pérdida propia.
- **Ganancia realizada y no realizada separadas.**
- **Dos totales distintos:** uno que suma todo, y otro que suma solo lo realizado.
- Comisiones acumuladas y ganancia neta.
- Impuesto estimado.

**Vista simple.**

- Sin desglose por lote.
- Venta con criterio **FIFO automático** (se vende primero lo que se compró primero).
- Ganancia total, sin separar realizada de no realizada.

**Reglas de las vistas:**

- Se puede **ir y volver libremente** entre una y otra, cuantas veces se quiera.
- Cambiar de vista **no altera ningún dato**. Solo cambia lo que se muestra.
- **Arriba de todo va una explicación** de cómo funciona el portfolio por lotes y para qué sirve. Esa explicación está disponible siempre, en las dos vistas, para que quien arrancó en la vista simple pueda entender y cambiarse.
- **Los nombres de las dos vistas están pendientes.** "Simple" y "por lotes" son descripciones, no nombres definitivos. Ojo: no llamarlas "Básico" y "Pro", porque esas palabras se van a usar para los planes y se generaría confusión.

### 9.4 El nombre del portfolio

- **Se pide siempre al crear un portfolio**, incluso en el nivel gratuito, que solo puede tener uno.
- **Se puede cambiar en cualquier momento**, sin restricciones y sin confirmación. El nombre es una etiqueta; los activos no cambian.
- Motivo de pedirlo también en el gratuito: si el día de mañana el usuario paga y crea más portfolios, el primero ya viene nombrado y no hay que pedirle nada retroactivamente.

### 9.5 Agregar, vender y eliminar

Hay **dos acciones distintas y visualmente separadas**. Esto es importante porque el usuario las va a confundir si no se distinguen bien.

**VENDER.** Botón propio.

- Registra la operación de venta.
- Calcula la **ganancia realizada**.
- La posición pasa a **cerrada**.
- **Libera el cupo** dentro del tope de 6.
- **No borra nada.** Todo el historial se conserva.

**ELIMINAR.** Botón propio, con ícono de tacho de basura.

- Borra el activo entero: todos sus lotes, todo su historial, toda su ganancia realizada.
- **Es para corregir errores de carga**, no para salir de una posición.
- Está sujeto a la regla de los 7 días (ver 9.6).

**Regla de aviso:** si el activo que se va a eliminar **tiene ventas registradas**, hay que advertir antes de borrar, indicando que eso también borra la ganancia realizada que está contada en el cálculo de impuestos. Si el activo nunca tuvo ventas, se borra sin ninguna advertencia.

### 9.6 La regla de los 7 días (antiabuso)

**Un activo solo se puede eliminar dentro de los 7 días de haber sido cargado.**

Pasados los 7 días, el botón de eliminar deja de estar disponible para ese activo. La única salida es vender, que registra la operación y deja el rastro.

**Fundamento:** un error de carga se detecta en días, no en meses. Alguien que cargó mal un ticker se da cuenta enseguida. Alguien que quiere borrar un activo de hace tres meses no está corrigiendo un error: está reciclando el cupo de 6, o está borrando un historial que no le conviene.

**Consecuencia aceptada:** un activo cargado mal hace más de una semana queda ahí para siempre, ocupando uno de los seis lugares del gratuito. La única salida es venderlo, lo cual le mete una operación falsa en el cálculo. Es un costo real, es chico, y está aceptado a conciencia.

**Regla derivada, para cerrar la puerta de atrás:**

**No se puede eliminar un portfolio que contenga activos vivos de más de 7 días.**

Sí se puede eliminar:
- Un portfolio vacío.
- Un portfolio donde todos los activos vivos fueron cargados dentro de los últimos 7 días.

Sin esta regla derivada, cualquiera podría saltarse la regla de los 7 días borrando el portfolio entero y creando uno nuevo. Es la misma regla aplicada al conjunto.

**Explícitamente descartado:** penalizar al usuario con una espera de 3 meses para cargar un activo nuevo después de eliminar uno. Se evaluó y se descartó porque castiga al que se equivocó, que es justamente el caso que la regla de los 7 días busca permitir.

### 9.7 Qué cuenta contra el tope de 6

> **Corrección respecto de la versión original.** El documento anterior decía que el tope contaba contra el "total histórico" de activos cargados. **Eso quedó sin efecto.**

- **Cuenta: activos vivos en simultáneo.** Seis activos distintos abiertos al mismo tiempo.
- **NO cuenta: los lotes.** Tres compras de AAPL son **un** activo, no tres.
- **NO cuentan: las posiciones cerradas.** Un activo que se vendió por completo sale del conteo y su historial se conserva aparte.
- Vender o eliminar **libera el lugar inmediatamente**.

Ejemplo, para que no quede duda: un usuario gratuito tiene AAPL (comprada en tres tandas distintas), MELI, GGAL, KO, VIST y BTC. Eso son **6 activos vivos**, está en el tope. Si vende toda su posición de KO, pasa a tener 5 vivos y puede cargar uno nuevo. El historial de KO sigue visible en la lista de posiciones cerradas.

### 9.8 Calculadora de comisiones

**Disponible en todos los niveles, incluido el gratuito.**

> **Corrección respecto de la versión original.** El documento anterior la ubicaba en el plan de $30. **Quedó sin efecto.**

**Cómo funciona:**

- El usuario carga **un porcentaje único, a grosso modo**, que incluye comisión del bróker + derechos de mercado + IVA, todo junto. **No es una tabla desglosada por tipo de operación.** Un solo campo.
- El P&L se calcula **neto de comisiones**, no solo bruto.
- Existe una **vista acumulada**: cuánto pagó de comisiones en el año, o en el período que elija.

**Autocompletado por chatbot — solo en el nivel de $60.** En los demás niveles el usuario escribe el porcentaje a mano, sin sugerencia previa (ver 7.3).

Cuando está disponible:

1. El usuario indica cuál es su bróker.
2. El chatbot busca las comisiones vigentes.
3. **Se las propone al usuario.** No las carga solo.
4. El usuario **confirma o corrige a mano**.

La confirmación explícita es obligatoria: así el número es responsabilidad del usuario. El copy debe reforzarlo, con algo del tipo "verificá que coincida con lo que te cobra tu bróker".

**Aviso de antigüedad del dato:**

Las comisiones cambian y el dato puede quedar viejo sin que nadie se entere.

- Se guarda la **fecha de carga** del porcentaje.
- Se muestra un aviso: *"Comisiones cargadas en marzo. Revisá si siguen siendo las mismas."*
- El aviso va **dentro del portfolio, cerca de donde se muestra el P&L neto**. No en configuración. Si está enterrado, nadie lo revisa nunca y el número queda viejo igual. Como el usuario ya está mirando su rendimiento ahí mismo, es el momento natural para acordarse.
- El aviso se implementa como un **ícono de información chiquito** junto a las comisiones acumuladas. Al abrirlo muestra la fecha de carga y el recordatorio.
- **El ícono debe abrirse tanto por hover como por tap.** El hover no existe en celular; si solo funciona con cursor, en móvil el aviso es invisible.

**Mantener el dato actualizado queda por cuenta del usuario.** No es responsabilidad del sistema.

### 9.9 Calculadora de impuestos

**Disponible en todos los niveles, incluido el gratuito.** Misma corrección que la de comisiones.

- El usuario define la **alícuota** que le corresponde.
- El sistema calcula el impuesto estimado sobre la ganancia realizada.
- **Disclaimer obligatorio y explícito:** es una estimación aproximada, no es asesoramiento contable, y conviene consultar con un contador. Este texto no es opcional ni se puede esconder.

### 9.10 Posiciones cerradas

- Se conservan siempre, en todos los niveles.
- **Fuera del conteo de 6.**
- Se muestran en una **lista aparte**, para que el usuario vea todo su recorrido sin que le ocupe cupo.

---

## 10. Los 3 portfolios del autor con benchmark

> **Corrección respecto de la versión original.** El documento anterior los describía de forma ambigua, dando a entender en algunos pasajes que eran del usuario. **No lo son.**

**Son tres carteras armadas por el autor, no por el usuario.** Es contenido, no una herramienta.

- Cada una se compara contra un **benchmark distinto**: inflación, S&P 500 y Nasdaq.
- **Visibles únicamente en el plan de $60.**
- Función doble: demostración pública del criterio del autor, y puente natural hacia la mentoría. Quien paga el nivel más alto es quien más probablemente quiera que ese criterio se aplique a su caso.

**Advertencia legal a tener presente:** mostrar rendimientos de carteras propias está cerca del terreno del asesoramiento financiero, y el autor todavía no tiene matrícula. La presentación tiene que ser descriptiva de lo que pasó, sin promesas ni proyecciones. El copy de esta sección conviene revisarlo con criterio conservador.

---

## 11. Alertas por WhatsApp

**Exclusivas del plan de $60.**

Tipos de alerta:

- Zonas de precio.
- Dark pool.
- Dividendos.
- Splits.

**Regla de canal, que aplica a todo el producto:**

WhatsApp se usa **únicamente** para:

1. Alertas de trading (esta sección).
2. Avisos de campaña.
3. Avisos de vencimiento de suscripción.

**No se usa para nada más.** En particular, no se usa para el aviso de "avisame cuando salga el dato", que va por mail.

Motivo: si el canal se satura deja de ser especial y pasa a ser molesto. El usuario que recibe demasiados mensajes reporta el número, y eso es lo único que puede tumbar el canal entero.

**Baja de mensajes:** el detalle completo (baja por tipo de mensaje, dos canales independientes) está en el Paso 1, sección 6.3.

---

## 12. La mentoría 1:1

**No es un plan y no aparece en esta escala.**

Reglas que afectan a este documento:

- **No requiere ningún plan pago.** Un usuario del nivel gratuito puede contratarla. Es indiferente al sistema de suscripción.
- **No aparece como una columna más en la tabla comparativa de planes.** Compararla feature por feature daría una impresión equivocada de lo que es.
- **Vive dentro del sitio**, con el mismo nombre de marca, como una sección propia con tratamiento visual distinto.
- **El precio no se publica nunca.** Se conversa con cada persona y depende del alcance del trabajo.

**Los dos alcances que se perfilan** (todavía sin precio):

1. **Armado puntual.** Se arma la cartera, se explica el criterio, y después la persona sigue sola.
2. **Seguimiento continuo.** Además del armado, el autor avisa qué conviene comprar o vender a lo largo del tiempo.

**Flujo de contacto:**

1. Contacto inicial por **WhatsApp**.
2. Se agenda una **llamada de diagnóstico por Zoom o Meet**.
3. Duración: **una hora y media. Gratuita.** No se arma ninguna cartera en esa llamada: se habla de mercados, del método del autor y de en qué se basa para decidir.
4. Los turnos son los **fines de semana**. El formulario para pedir turno se abre los **lunes**.
5. Antes de agendar, la persona completa un **formulario de ocho campos** (ver 12.1).

**La sección de mentoría incluye un "sobre mí"**, enfocado en la transformación y no en el currículum: de dónde viene la persona y a dónde llega — su portfolio, su nivel de riesgo, su entendimiento de qué mueve sus posiciones. El texto no está escrito todavía.

### 12.1 El formulario de pre-selección

Ocho campos exactos. Todos con opciones desplegables o botones, salvo el séptimo.

1. **Edad.** Rango: menos de 30 / 30-45 / 45-60 / más de 60.
2. **Hace cuánto invierte.** Menos de 1 año / 1-3 / 3-10 / más de 10.
3. **Tamaño del portfolio.** Rangos en dólares, con opción "prefiero no decirlo".
4. **En qué está invertido hoy.** Selección múltiple: acciones argentinas / CEDEARs / acciones USA / bonos / ETFs / cripto / plazo fijo o dólar / opciones o derivados / nada todavía.
5. **En qué se basa para decidir.** Técnico / fundamental / los dos / recomendaciones de terceros / todavía no tengo un método.
6. **Objetivo.** Crecer capital a largo plazo / generar ingreso periódico / proteger de la inflación / aprender a decidir solo / una meta concreta con fecha.
7. **Qué te trae acá.** **Texto libre, dos renglones, obligatorio.** Es el campo más valioso: da el problema en las palabras de la persona.
8. **Tiempo disponible por semana.** Menos de 1 hora / 2-5 horas / más de 5 horas.

**Explícitamente excluidos del formulario:**

- **Tolerancia al riesgo.** Todo el mundo se declara moderado y no aporta información. Se evalúa en la llamada.
- **Rendimiento del último año.** La mayoría no lo sabe, y el que lo sabe lo redondea para arriba.
- **Si trabajó antes con un asesor.** Se evaluó y se descartó.

---

## 13. El estado bloqueado

Aplica a cualquier funcionalidad que el usuario no tiene según su nivel.

### 13.1 Reglas generales

- **Se muestra, no se esconde.** La herramienta aparece en la interfaz, difuminada, con el dato real borroso de fondo. **No se saca del menú ni se reemplaza por una caja vacía.**
- **Debe invitar, no frustrar.** El mensaje es "esto está acá esperándote", no "no tenés permiso".
- **Debe quedar claro qué plan lo desbloquea**, sin obligar al usuario a ir a buscar la tabla de precios.
- Ver algo real genera más deseo de upgrade que ver un ícono genérico de candado sobre un espacio en blanco.

### 13.2 Para el texto de las notas

Desvanecido progresivo hasta desaparecer. **Sin candado.** Ver 3.2.

### 13.3 Para las herramientas

Difuminado con dato real de fondo. El nivel exacto de desenfoque y opacidad está pendiente y se define al diseñar las pantallas.

### 13.4 Requisito técnico transversal

**Ningún contenido ni dato bloqueado puede viajar al navegador del usuario.** Vale para el texto de las notas, para los datos de las herramientas y para cualquier otra cosa que el nivel del usuario no habilite.

Si el dato está en el navegador y solo está tapado visualmente, cualquiera lo ve inspeccionando la página. **La validación de permisos se hace en el servidor, siempre.** La interfaz refleja los permisos; no los impone.

---

## 14. Cambios de nivel

### 14.1 Subir de nivel

- El acceso a lo nuevo es **inmediato**.
- Si el usuario tenía activos archivados de una baja anterior, **se restauran automáticamente**.

### 14.2 Bajar de nivel o cancelar

El usuario vuelve al nivel gratuito, con el tope de 1 portfolio y 6 activos.

**Al momento de bajar, el usuario elige.** No se elige solo ni se posterga:

1. El sistema le muestra **todos sus portfolios con sus nombres**.
2. El usuario **elige uno**, que queda activo.
3. Dentro de ese portfolio, **elige con qué 6 activos se queda**.
4. Todo lo demás queda **guardado durante 2 meses**.

**Durante esos 2 meses:**

- El usuario **opera con los 6 activos que eligió**, no con los que tenía antes.
- El resto está **congelado y no visible**.
- **Si vuelve a suscribirse dentro de la ventana, se restaura todo automáticamente** — todos los portfolios, todos los activos, todo el historial por lotes. El usuario no tiene que cargar nada de nuevo.

**Pasados los 2 meses sin resuscripción:** lo guardado se elimina de forma permanente.

**Fundamento de que elija en el momento y no después:** es más honesto. El usuario sabe desde el día uno con qué se quedó, en vez de descubrirlo dos meses más tarde.

### 14.3 Reembolsos

**10 días desde el pago**, y aplica **únicamente a la primera compra que el usuario hace en la plataforma**, sea el plan que sea.

Ejemplos, para que no quede duda:

- Usuario nuevo se suscribe al plan de $30. **Tiene 10 días** para pedir la devolución.
- Ese mismo usuario después pasa al de $60. **No tiene ventana de reembolso.**
- Usuario nuevo arranca en el de $15 y después salta al de $60. **La ventana la tuvo solo en el de $15.**

Es una sola vez en la vida de la cuenta. Las renovaciones mensuales no generan ventana nueva.

**Nota operativa:** con cripto la devolución es manual y tiene costo de red. Esto tiene que estar escrito en los términos y condiciones antes del primer reclamo.

---

## 15. Nivel futuro: Desarrollador

Está previsto un quinto nivel, **todavía sin definir y sin fecha**:

- Precio aproximado: **USD 350/mes**.
- Contenido: acceso a un MCP propio de la plataforma.

**Implicancia para el equipo:** la estructura de niveles y la disposición visual de la página de planes **deben poder acomodar un nivel más sin rehacerse**. Cuatro columnas hoy, cinco mañana.

---

## 16. Reglas técnicas transversales

Aplican a todo lo descrito arriba.

1. **Los permisos se validan en el servidor, siempre.** La interfaz refleja el nivel del usuario; no lo hace cumplir. Cualquier validación que viva solo en el navegador se puede saltear.

2. **El nivel se identifica con un identificador interno estable**, no con el precio ni con el nombre visible. Cambiar un precio o renombrar un plan no puede requerir tocar la lógica de permisos.

3. **Nada de contenido bloqueado viaja al navegador.** Ver 13.4.

4. **Responsive en todo, sin excepciones**, incluidas las herramientas pesadas (mapa de calor, dark pool, flujo de opciones). Desktop es la prioridad de diseño, pero nada puede quedar roto en celular. Se acepta que algunas herramientas no queden óptimas en pantalla chica; no se acepta que no funcionen.

5. **Aviso de pantalla grande** en las herramientas densas cuando se abren desde celular:
   - **Una vez por sesión**, por herramienta.
   - **Descartable.**
   - **Tono informativo neutro**, no de alarma. Nada de rojo.
   - **Arriba de la herramienta, sin taparla.**

6. **Consultas a las APIs externas centralizadas en el servidor.** El servidor consulta al proveedor, guarda el resultado unos segundos, y se lo sirve a todos los usuarios. Nunca una consulta por usuario.

   Detalle importante, porque se malinterpreta: **esto no es un archivo histórico.** Es una foto de unos 30 segundos que se pisa a sí misma. No se guarda historial de datos de mercado; las consultas de datos pasados van al proveedor en el momento.

   Con el volumen de usuarios previsto al inicio esto no es necesario para no quedarse sin cupo. Se hace desde el principio porque agregarlo después cuesta mucho más que ponerlo ahora.

7. **Todo color, tipografía y espaciado va como variable**, nunca escrito a mano en cada pantalla. Ver Paso 3.

---

## 17. Pendientes de este paso

**Bloquean la implementación de esas partes:**

- **Nombres de los planes.** Tres sets propuestos en el Paso 2, sección 6. Ninguno confirmado. Hasta entonces, identificadores internos.
- **Nombres de las dos vistas del portfolio.** Ver 9.3.

**No bloquean, se resuelven al diseñar:**

- **Dónde corta exactamente cada nota** para el anónimo. El criterio está definido; falta el mecanismo concreto con el que el autor marca el punto al escribir.
- **Cómo se ve el filtro por tags** en la sección de noticias.
- **Cómo se muestra visualmente el hilo** entre notas.
- **Nivel de desenfoque y opacidad** del estado bloqueado en las herramientas.

**Decisiones comerciales pendientes:**

- **Precio y alcances de la mentoría.** Nunca se publica; se define para la conversación.
- **Planes anuales con descuento.** Idea aceptada, postergada a propósito: al inicio los precios ya son bajos y el objetivo es recaudar lo mínimo indispensable. Cuando se implemente, el descuento anual no es solo precio: adelanta caja y elimina once oportunidades de baja por año.
- **Prueba temporal del portfolio sin tope** para usuarios gratuitos, durante un mes. Es la mejor candidata a prueba gratuita porque es la única herramienta donde el usuario deja trabajo propio: carga sus datos, se acostumbra, y al terminar tiene todo adentro. Falta definir qué pasa al terminar la prueba, presumiblemente el mismo mecanismo de la sección 14.2.
