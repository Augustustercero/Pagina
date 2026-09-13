# PASO 1 — Visión general, modelo de acceso y modelo de cobro

Documento de contexto del proyecto. Versión corregida y ampliada: incorpora todas las decisiones tomadas en conversación, incluidas las que contradicen la versión original.

Este documento está escrito para que lo lean tanto diseñadores como desarrolladores. Donde hay algo sin definir, está marcado como pendiente en vez de dejarlo implícito.

---

## 1. Qué es este proyecto

Una plataforma web de noticias y análisis financiero con modelo de suscripción por niveles, dirigida a inversores minoristas que ya tienen conocimiento previo de mercados. No es un medio generalista ni un producto para principiantes absolutos.

**La plataforma todavía no tiene nombre definido.** Para los diseños, usar un placeholder dejando claro que es provisorio, y que el logo/wordmark debe poder cambiarse fácilmente sin romper la composición del header. El nombre será inventado (no el nombre propio del autor) y se define al final del proceso de diseño, junto con la identidad visual.

## 2. A quién le habla

El lector objetivo es alguien que **ya invierte** y tiene algo de conocimiento de mercados, pero no es un profesional del sector. Sabe lo suficiente para valorar datos duros, terminología técnica y análisis riguroso — no necesita que le expliquen qué es una acción. Al mismo tiempo, no domina todo: puede perderse en jerga muy específica, y valora que se la traduzcan brevemente sin que lo traten como ignorante.

El objetivo emocional es que el lector sienta que **encontró algo distinto** al ruido habitual de los medios financieros: que hay criterio real detrás, que aprende algo cada vez que entra, y que eso le da una sensación de estatus por comprensión (no por dinero). Explícitamente **no** se busca generar ansiedad, urgencia falsa ni sensacionalismo.

## 3. Cuál es el corazón del negocio

El servicio central es la **mentoría 1:1**: el autor arma un portfolio sugerido según el perfil de riesgo del cliente, basado en su propio criterio y visión de mercado. Todo lo demás de la plataforma (noticias, audio, herramientas de datos, portfolio tracker) funciona como **vidriera y funnel** hacia ese servicio: demuestra el criterio del autor en público para que el usuario eventualmente quiera acceder a ese criterio de forma personalizada.

Esto tiene una implicancia de diseño importante: la plataforma no es un SaaS impersonal de herramientas. Es el escaparate de una persona con criterio propio. La presencia del autor (su voz en los audios, sus análisis firmados, su reporte mensual, sus tres portfolios con benchmark) es parte del producto, no un accesorio.

### La mentoría 1:1 no es un plan

Es un servicio de naturaleza distinta:

- Los planes ($15 / $30 / $60) venden **acceso a datos, contenido y herramientas** — el usuario consume información y la usa por su cuenta.
- La mentoría 1:1 vende **el trabajo directo del autor**: armado de portfolios personalizados, seguimiento y su visión aplicada al caso puntual de esa persona. No entrega datos ni desbloquea funcionalidades de la plataforma.

Reglas:

- Vive **dentro del sitio** como una sección propia, con tratamiento visual distinto al de los planes y su propio mecanismo de contacto.
- **No aparece como una columna más en la tabla comparativa de planes.** Compararla feature por feature daría una impresión equivocada de lo que es.
- **No requiere ningún plan pago.** Cualquiera puede contratarla, incluso un usuario del nivel gratuito. Es indiferente al sistema de suscripción.
- **Se paga dentro de la plataforma**, con los mismos medios de pago que el resto (ver sección 8).
- **No tiene precio público todavía.** No mostrar precio hasta que esté definido.

## 4. Modelo de acceso

> **Corrección respecto de la versión original.** La versión anterior de este documento decía que no había navegación anónima y que el login era únicamente con Google. Las dos cosas quedaron sin efecto.

### 4.1 Los tres estados de acceso

**Sin cuenta (anónimo).** Puede entrar al sitio y navegar. Ve:

- La home completa, con el listado de noticias.
- Las noticias, pero **cortadas** (ver 4.2).
- Todas las herramientas presentes en la interfaz, **difuminadas y bloqueadas**, con el dato real borroso de fondo.

No tiene daily, ni glosario, ni ninguna herramienta usable.

**Con cuenta gratuita.** Suma:

- Noticia completa.
- Daily (texto y audio).
- Glosario financiero.
- Portfolio tracker limitado a 1 portfolio y 6 activos (detalle en Paso 4).

**Planes pagos.** Requieren estar logueado sí o sí. Detalle de cada nivel en el Paso 4.

### 4.2 El corte de la noticia para el anónimo

Mecanismo:

- La **primera pantalla se ve entera y sin candados**. El usuario no percibe ningún muro al llegar.
- Al scrollear, el texto se **desvanece progresivamente** (mismo lenguaje visual que las herramientas bloqueadas) y aparece la invitación a registrarse gratis.
- El corte cae **justo antes de la parte más importante de la nota** — el momento en que se unen los puntos y el lector ya está enganchado.
- El corte se define **por posición en la estructura del texto, nunca por altura fija en píxeles**. Una altura fija haría que en desktop se vea casi toda la nota y en celular tres párrafos.
- Se aplica a **todas las notas por igual**. No existe el esquema de "una nota gratis y el resto bloqueadas".

Fundamento de esta decisión: es más honesto (el usuario ve el límite desde el primer momento y lo que leyó ya le dio valor real, así que no se lee como clickbait) y deja contenido público indexable en cada nota, lo cual atrae tráfico de buscadores. Tapar notas enteras dejaría a Google sin nada que mostrar.

**Requisito técnico obligatorio:** el texto bloqueado **no puede estar cargado en la página y solo tapado visualmente**. Tiene que servirse recién después del registro. Si está en el HTML, cualquiera lo lee desactivando el difuminado desde el navegador y los buscadores lo indexan como público, lo que anula el muro.

## 5. Registro, login y recuperación

### 5.1 Registro (una sola vez)

Campos obligatorios:

1. **Correo electrónico** — se envía un código de verificación que el usuario ingresa en la página.
2. **Contraseña**
3. **Número de teléfono** — se envía un código de verificación por SMS que el usuario ingresa en la página.

Ambas verificaciones son obligatorias para completar el alta. Una vez verificadas, **no se vuelven a pedir nunca más**.

### 5.2 Login (siempre)

Dos caminos, ambos disponibles:

- **Continuar con Google**
- **Mail + contraseña**

**Unificación de cuentas:** si un usuario se registró con mail y contraseña y después entra con Google usando ese mismo mail, es la misma cuenta. No se crean dos.

### 5.3 Recuperación de contraseña

Se implementan **las dos vías**:

- **Código por mail** para ingresar directamente a la plataforma.
- **Link por mail** para definir una contraseña nueva.

### 5.4 Pantallas y estados que se desprenden

- Landing / bienvenida
- Login (Google + mail y contraseña)
- Formulario de alta, con los dos checkboxes (ver 6)
- Modal de consentimiento de publicidad
- Pantalla de ingreso de código de mail
- Pantalla de ingreso de código de SMS
- Recuperación de contraseña (pedido, código, contraseña nueva)
- Estados de error: código incorrecto, código expirado, reenvío de código, límite de reintentos
- Vista de noticia cortada para el anónimo

## 6. Consentimiento de publicidad

El teléfono verificado es un activo central del negocio, no solo el canal de las alertas Pro. Sirve para enviar promociones, ofertas y avisos de campañas por mail y WhatsApp.

**Regla:** el checkbox de aceptación de publicidad es **obligatorio para acceder al nivel gratuito**. Si el usuario no lo acepta, el único camino es pagar el plan de $15.

### 6.1 Cómo se implementa

- Son **dos checkboxes distintos y visibles**: uno de términos y condiciones, otro de publicidad. Los dos obligatorios para el alta gratuita.
- El de publicidad **nunca va enterrado dentro de los términos y condiciones**. Esto no es una preferencia estética: WhatsApp Business exige opt-in explícito para mensajes de marketing, y si no hay constancia de consentimiento, unos pocos reportes de usuarios alcanzan para que bloqueen el número. Con el checkbox visible y marcado, hay registro.
- El consentimiento se acepta **una sola vez y queda para siempre**, incluso si el usuario después pasa a un plan pago. Como el 100% de los usuarios pasa por el alta gratuita antes de pagar, todos terminan aceptando.

### 6.2 Modal de explicación

Si el usuario intenta avanzar sin marcar el checkbox de publicidad, aparece un modal que explica la disyuntiva en lugar de dejarlo trabado sin entender por qué. Borrador de copy:

> **Para seguir gratis, necesitamos poder escribirte**
>
> El acceso gratuito se sostiene con las promociones y avisos que te mandamos por mail y WhatsApp. Son pocos, y ahí también avisamos los accesos gratuitos a planes pagos.
>
> Si preferís no recibirlos, podés acceder con el plan de $15/mes.
>
> `[ Acepto recibir promociones y sigo gratis ]` `[ Ver plan de $15 ]`

Criterios de diseño:

- El botón de aceptar es el **primario**; el de pagar, secundario. La mayoría va a elegir el gratuito y no conviene que el camino más probable parezca la opción de castigo.
- Aparece **solo al intentar avanzar**, no de entrada. Así se lee como aclaración y no como que el sitio empieza pidiendo algo.
- Al cerrarlo, el usuario vuelve al formulario con el checkbox sin marcar. No se lo expulsa.
- El tono explica el intercambio ("el gratuito se sostiene con esto"), no impone ("no podés seguir hasta que aceptes").

### 6.3 Baja de mensajes de WhatsApp

Cada mensaje de WhatsApp incluye una opción de respuesta para dejar de recibir ese tipo de mensaje. El usuario la selecciona, se envía una respuesta automática, y el sistema da de baja ese canal para ese usuario.

**La baja es por tipo de mensaje, no global.** Son dos canales independientes:

- **Promocionales** (ofertas, campañas, avisos comerciales).
- **Alertas de trading** (zonas de precio, dark pool, dividendos, splits — plan Pro).

Darse de baja de uno no afecta al otro. Si el usuario se da de baja de las promociones, sigue recibiendo sus alertas de trading, y viceversa.

## 7. Los cuatro pilares del producto

1. **Contenido editorial.** Noticias filtradas por IA (sin ruido ni relleno), reescritas desde cero con una metodología propia de análisis en capas, con tags de ticker y sector. Incluye el **daily** (300-400 palabras, dos temas) y análisis extendidos. La **noticia ampliada** — el análisis extendido con mecanismo causal completo — corresponde al plan de **$30**. Esto corrige la tabla del Paso 4, que la ubicaba en $60.

2. **Audio.** Narrado con la voz clonada del autor. El **daily con audio es gratuito**: es el gancho de cercanía y la primera vez que el usuario escucha su voz. Lo que se cobra es el volumen y la profundidad — el audio de cada nota individual es del plan $15, y el podcast mensual largo es del plan superior. No se cobra el acceso a su voz.

3. **Herramientas de datos.** Portfolio tracker con contabilidad por lotes (presente en **todos** los niveles, incluido el gratuito, con tope de activos), calculadora de impuestos y de comisiones, simulador de escenarios históricos, correlación entre activos, y datos institucionales (trading de congresistas, dark pool, flujo de opciones, mapa de calor de liquidaciones).

4. **Mentoría 1:1.** El servicio premium personal, fuera del sistema de suscripción mensual.

## 8. Medios de pago

Los precios están **en dólares**. Para Argentina se convierten a pesos automáticamente.

### 8.1 Medios habilitados

| Medio | Alcance | Cobro automático | Verificación |
|---|---|---|---|
| **Mercado Pago** | Argentina | **Sí** (débito recurrente con tarjeta) | Automática |
| **Transferencia bancaria** | Argentina | No | Manual / contra movimiento real |
| **Wise** | Exterior | No | Manual / contra movimiento real |
| **USDC** (Arbitrum y BNB Smart Chain) | Global | No | Automática, leyendo la blockchain |

Notas importantes:

- **Solo USDC.** No se acepta USDT.
- **Wise no es una pasarela de pago.** No se integra al checkout ni cobra solo. El flujo es el mismo que la transferencia bancaria: se le muestran los datos al cliente, él envía la plata desde su propio Wise o banco, y se verifica del lado nuestro. Está incluido para que el cliente del exterior tenga dónde depositar, no como integración técnica. **El desarrollador no debe buscar una API de suscripciones de Wise: no existe para este uso.**
- **Mercado Pago es el único medio con débito automático.** Todos los demás requieren que el usuario mande la plata cada mes.

### 8.2 Verificación de pagos

- **USDC:** el sistema lee la blockchain y confirma el pago automáticamente. Es el método más confiable, porque la transacción existe o no existe.
- **Transferencia y Wise:** la verificación debe hacerse **contra el movimiento real de la cuenta**, no contra una imagen de comprobante subida por el usuario. Un comprobante en imagen se falsifica en minutos.
- **Mercado Pago:** automática vía la plataforma.

Pantallas que esto implica y que no estaban en el inventario original: estado "pago pendiente de confirmación", y un panel de administración para revisar y aprobar pagos manuales.

### 8.3 Conversión a pesos

- Automática, tomando la cotización de **Dolarito**.
- **Pendiente:** definir cuál de las cotizaciones que publica Dolarito se toma (blue, MEP, cripto, oficial). La diferencia entre ellas es plata real.
- Recomendación de presentación: mostrar **"USD 30 (≈ $X al cambio de hoy)"** en vez de un precio en pesos suelto, para que el usuario no perciba que el precio sube cada semana.
- Advertencia técnica: en Mercado Pago, si el monto de una suscripción sube mucho respecto del autorizado, el sistema puede requerir que el usuario vuelva a autorizar el débito. Hay que preverlo.

### 8.4 Facturación

El autor es monotributista y **emite las facturas a mano**. El sistema **no** se conecta a ARCA. Solo guarda el registro del pago.

Se revisará cuando el volumen lo justifique.

## 9. Ciclo de suscripción

### 9.1 Vencimiento y avisos

- El usuario **conserva el acceso hasta el día de vencimiento inclusive**.
- Avisos previos, por **mail y WhatsApp**:
  - Un día antes del vencimiento.
  - El día del vencimiento: un mensaje a la mañana y otro a la tarde/noche.
- Los avisos aplican especialmente a los medios sin débito automático, que son la mayoría.

### 9.2 Pago tardío

Si vence el día 10 y el usuario paga el 13, **el nuevo período arranca el 13**, no el 10. No se le cobran días sin servicio.

### 9.3 Cancelación o baja

Al cancelar, el usuario **vuelve al nivel gratuito** con todas sus limitaciones (1 portfolio, 6 activos).

Ventana de retención de datos: **2 meses**.

- **Si vuelve a suscribirse dentro de los 2 meses:** se restaura **todo automáticamente** — todos los activos, todos los portfolios y todo el historial por lotes. El usuario no tiene que cargar nada de nuevo.
- **Si pasan los 2 meses sin resuscripción:** el sistema le pide que **elija qué 6 activos conserva**. De esos 6 se mantienen los registros por lotes y todo el historial. El resto se elimina de forma permanente.

**Pendientes de esta sección:**

- Durante esos 2 meses, ¿el usuario ve y opera sus 12 activos, o ya opera con 6 y los demás quedan guardados sin verse?
- Si tenía varios portfolios y el gratuito permite uno solo, ¿cuál queda activo durante la ventana y cómo se elige al vencerla?

### 9.4 Reembolsos

**10 días** desde el pago para pedir la devolución.

**Pendiente:** definir si aplica solo al primer pago de un usuario nuevo o a cualquier pago (incluidas las renovaciones mensuales). Con cripto la devolución es manual y tiene costo de red, así que conviene que quede escrito en los términos antes del primer reclamo.

## 10. Campañas de acceso temporal

Parte del motivo por el que el mail y el teléfono son obligatorios. En fechas comerciales (Black Friday, Cyber Monday, diciembre) se regala un mes del plan superior:

- Usuarios gratuitos → un mes del plan de $30
- Usuarios de $30 → un mes del plan de $60

Mecánica: **es sorpresa**. Se avisa por mail con cuenta regresiva ("faltan X días") sin decir de qué se trata, y el día que se activa se avisa por mail **y por WhatsApp**.

**Riesgo a tener en cuenta:** diciembre es un mes flojo para operar, y la misma condición que hace barato regalar el mes lo hace fácil de desperdiciar. Si el usuario no lo aprovecha, termina pensando "no me sirvió" en vez de "quiero más". Conviene acompañar la prueba con contenido que le diga qué mirar, o correr la campaña en enero, cuando la gente vuelve a operar.

**Idea abierta (sin definir).** Prueba gratuita de una herramienta del plan $30 para usuarios gratuitos, durante un mes. La mejor candidata es el **portfolio sin el tope de 6 activos**, porque es la única herramienta donde el usuario deja trabajo propio: carga sus datos, se acostumbra, y cuando termina la prueba tiene todo adentro. La fricción de irse pasa a ser máxima. Queda pendiente definir qué pasa al terminar la prueba (presumiblemente el mismo mecanismo de la sección 9.3).

## 11. Filosofía de producto

Decisiones que definen el carácter de la plataforma y que deberían usarse como criterio para resolver dudas de diseño que este documento no cubra.

- **Menos, pero mejor.** Se descartó sumar un resumen semanal para no caer en sobreinformación. El daily se acortó a dos temas por la misma razón. La competencia satura de contenido de baja calidad; la apuesta acá es al revés: poco volumen, alta densidad de valor.
- **Sin gamificación competitiva.** Se descartó un ranking entre usuarios. No hay medallas, niveles, rachas ni comparaciones de rendimiento entre personas. Nadie debe sentirse menos por rendir peor que otro.
- **Cercanía real, no impostada.** El autor quiere "traspasar la pantalla" y acompañar el día a día del usuario, incluso pensando en una juntada presencial anual en Argentina. La plataforma es el vehículo de una relación, no un producto anónimo.
- **Educación como valor.** El glosario financiero está disponible en el nivel gratuito, porque el autor quiere que la gente aprenda de finanzas independientemente de si le paga o no.
- **Mostrar lo bloqueado, no esconderlo.** Los usuarios ven las herramientas de niveles superiores presentes en la interfaz pero difuminadas, con el dato real borroso de fondo. El bloqueo debe **invitar, no frustrar**: "esto está acá esperándote", no "no tenés permiso".
- **Herramientas que funcionan bien dentro de su límite.** Nada se degrada a propósito para forzar la conversión. Lo que el usuario tiene, funciona impecable; lo que no tiene, es cuestión de escala (más activos, más portfolios, más herramientas). Un producto que calcula mal genera desconfianza en los números, y esa duda se contagia al resto del contenido — que es justamente el activo central de la marca.
- **Sin ansiedad.** Nada de urgencia fabricada, contadores regresivos, alarmismo ni tácticas de clickbait, ni en el contenido ni en la interfaz.

## 12. Contexto geográfico

El autor está en Argentina y el producto arranca con foco en el público hispanohablante, con proyección a expandirse a otros idiomas más adelante. El calendario económico muestra horarios en hora de Argentina. Se busca un tono neutro en modismos regionales (voseo sí, lunfardo local no) para que viaje bien a otras audiencias hispanohablantes.

---

## 13. Pendientes de este paso

**De producto:**

- **Nombre de la plataforma** y wordmark. Se definen al final, junto con la identidad visual.
- **Dónde corta exactamente la nota** para el anónimo. El criterio está definido; falta bajarlo a una marca concreta en la estructura editorial. Probablemente una marca que el autor pone al escribir, no una regla automática.
- **Mecanismo de contacto de la mentoría:** formulario, agenda u otro.
- **Precio de la mentoría.** Sin definir. No mostrar precio hasta que lo esté.
- **Sección "sobre mí":** por ahora no va. Queda para cuando el autor tenga la matrícula de idóneo. Pendiente resolver qué ve el usuario que llega a la mentoría y solo conoce la voz del autor, dado que se le está pidiendo confiar su portfolio a alguien sin bio ni credenciales visibles.

**De cobro:**

- Qué cotización de Dolarito se toma.
- Si el reembolso de 10 días aplica a todos los pagos o solo al primero.
- Qué pasarela internacional con cobro automático se suma más adelante. Requiere LLC en Estados Unidos, EIN y cuenta bancaria estadounidense en el caso de Stripe: Argentina no está entre los países que Stripe admite directamente. Alternativas a evaluar: plataformas que actúan como vendedor intermediario (tipo Paddle o dLocal Go), que habría que verificar si dan de alta a residentes argentinos.

**De ciclo de vida:**

- Comportamiento del portfolio durante los 2 meses de ventana tras la cancelación (ver 9.3).
- Qué portfolio queda activo si tenía varios y baja al gratuito.
