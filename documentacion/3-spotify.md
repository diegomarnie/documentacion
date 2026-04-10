# Cómo Spotify publica actualizaciones para 675 millones de usuarios cada semana sin romper nada

**ByteByteGo Newsletter** — 8 de abril de 2026

---

Cada viernes por la mañana, un equipo de Spotify toma cientos de cambios de código escritos por decenas de equipos de ingeniería y comienza a empaquetarlos en una única actualización de la aplicación. Esa actualización acabará llegando a más de 675 millones de usuarios en Android, iOS y escritorio. Lo hacen cada semana. Y, sorprendentemente, más del 95% de esas publicaciones llegan a todos los usuarios sin ningún contratiempo.

La suposición natural es que son increíblemente cuidadosos y, por tanto, lentos; o increíblemente rápidos y, por tanto, descuidados. La verdad no es ninguna de las dos cosas.

¿Cómo publicas una actualización para 675 millones de usuarios cada semana, con cientos de cambios de decenas de equipos ejecutándose en miles de configuraciones de dispositivos, sin romper nada?

La respuesta no es hacer muchas pruebas. Spotify construyó una arquitectura de publicación donde la velocidad y la seguridad se refuerzan mutuamente. En este artículo veremos este proceso en detalle e intentaremos extraer aprendizajes.

> **Aviso:** Este artículo se basa en detalles compartidos públicamente por el equipo de ingeniería de Spotify. Por favor, comenta si detectas alguna inexactitud.

---

## El viaje de dos semanas de una versión de Spotify

Para entender cómo funciona esto, sigamos una versión concreta desde la fusión del código hasta producción.

Spotify practica el **desarrollo basado en trunk** (*trunk-based development*), lo que significa que todos los desarrolladores fusionan su código en una única rama principal tan pronto como está probado y revisado. No existen ramas de funcionalidades de larga duración donde el código permanece aislado durante semanas. Todo el mundo empuja a la misma rama continuamente, lo que mantiene pequeños los problemas de integración, pero requiere disciplina y pruebas automatizadas sólidas.

El siguiente diagrama muestra el concepto de desarrollo basado en trunk:

![Diagrama de trunk-based development](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb84eb1d0-84de-4097-b7d6-921fe9dfead8_2130x1226.png)

Cada ciclo de publicación comienza un viernes por la mañana. Se incrementa el número de versión en la rama principal. A partir de ese momento, las compilaciones nocturnas comienzan a distribuirse a los empleados de Spotify y a un grupo de probadores alfa externos. Durante esta primera semana, los equipos desarrollan y fusionan código libremente. Los informes de errores llegan de los usuarios internos y alfa. Las tasas de fallos y otras métricas de calidad se rastrean en cada compilación, tanto automáticamente como mediante revisión humana. Cuando un fallo o problema supera un umbral de gravedad predefinido, se crea automáticamente un ticket de error. Cuando algo parece sospechoso pero está por debajo de ese umbral, el Gestor de Publicaciones puede crear uno manualmente.

![Diagrama del ciclo de publicación](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1139a3dc-405e-4fb1-b141-1f33c07ead0d_3542x1226.png)

*Fuente: [Spotify Engineering Blog](https://engineering.atspotify.com/2025/04/how-we-release-the-spotify-app-part-1)*

El viernes de la segunda semana se **corta la rama de publicación**, es decir, se crea una copia separada del código base específicamente para esta versión. Este es el momento clave del ciclo. A partir de aquí, solo se permiten correcciones de errores críticos en la rama de publicación. Mientras tanto, la rama principal sigue avanzando: las nuevas funcionalidades y las correcciones no críticas continúan fusionándose allí, destinadas a la versión de la semana siguiente. Esta separación es el mecanismo que permite a Spotify desarrollar a plena velocidad mientras simultáneamente estabiliza lo que está a punto de publicarse.

Los equipos realizan entonces pruebas de regresión, comprobando que las funcionalidades existentes siguen funcionando correctamente tras los nuevos cambios, y reportan sus resultados. Los equipos con alta confianza en sus pruebas automatizadas y sus rutinas previas a la fusión pueden optar por no realizar pruebas manuales. Los probadores beta reciben compilaciones de la rama de publicación, más estable, proporcionando tiempo de ejecución real adicional durante el fin de semana.

Para el lunes, el objetivo es enviar la aplicación a las tiendas. Para el martes, si la revisión de la tienda de aplicaciones pasa y las métricas de calidad son buenas, Spotify la despliega al 1% de los usuarios. Para el miércoles, si no surge nada alarmante, se despliega al 100%.

El siguiente diagrama muestra todos los pasos de un proceso típico de publicación:

![Diagrama del proceso de publicación completo](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F21c1a584-c6a7-4f62-99fe-0cb22399c122_2736x446.png)

*Fuente: [Spotify Engineering Blog](https://engineering.atspotify.com/2025/04/how-we-release-the-spotify-app-part-1)*

A modo de ejemplo, para la versión 8.9.2, que incluyó el lanzamiento de la funcionalidad de Audiolibros en nuevos mercados, esta línea temporal se desarrolló casi exactamente como se había planificado. Lo que lo hizo posible fue todo lo que ocurrió detrás de ese cronograma.

---

## Anillos de exposición: detectar errores donde son más baratos de corregir

El código no pasa del portátil de un desarrollador a 675 millones de usuarios de un salto. Pasa por anillos concéntricos de usuarios, y cada anillo existe para detectar una categoría específica de fallos.

- **El primer anillo** son los propios empleados de Spotify. Utilizan compilaciones nocturnas de la rama principal, usando la aplicación como lo haría un usuario real. Esto detecta los errores funcionales obvios de forma temprana. Incluso un fallo que solo afecta a un pequeño número de empleados se investiga, porque un error que parece menor internamente podría indicar un problema mucho mayor cuando llegue a millones de dispositivos.

- **El segundo anillo** son los probadores alfa externos. Estos usuarios introducen más diversidad de dispositivos y patrones de uso del mundo real que el equipo interno podría no haber anticipado. Están usando compilaciones que aún están en desarrollo activo, por lo que se esperan bordes rugosos, pero los datos que generan son invaluables.

- **El tercer anillo** son los probadores beta, que reciben compilaciones de la rama de publicación en lugar de la rama principal. Se espera que estas compilaciones sean más estables. Los usuarios beta proporcionan tiempo de ejecución adicional durante los fines de semana y las noches, y sus comentarios o bien generan confianza en que la versión es sólida, o bien sacan a la luz problemas que se colaron por los dos primeros anillos.

- **El cuarto anillo** es el despliegue de producción al 1%. Usuarios reales, dispositivos reales, condiciones reales. La base de usuarios de Spotify es lo suficientemente grande como para que incluso el 1% proporcione datos estadísticamente significativos. Si aparece un problema grave durante esta fase, el despliegue se pausa de inmediato y el equipo responsable comienza a trabajar en una corrección.

- **El quinto y último anillo** es el despliegue al 100%. Solo después de que el despliegue al 1% parezca limpio, la versión se distribuye a todos.

Como referencia, el lanzamiento de Audiolibros en la versión 8.9.2 muestra cómo funciona este sistema a un nivel aún más granular.

La funcionalidad de Audiolibros no solo pasó por estos cinco anillos del proceso de publicación de la aplicación. Tuvo su propio despliegue por capas encima de eso. El código de la funcionalidad había estado integrado en la aplicación durante varias versiones ya, oculto detrás de un **indicador de funcionalidad** (*feature flag*) en el backend. Se activó primero para la mayoría de los empleados. El equipo vigiló cualquier fallo, por pequeño que fuera, que pudiera indicar problemas. Solo después de que la propia publicación de la aplicación alcanzara una base de usuarios suficiente, el equipo de Audiolibros comenzó a habilitar gradualmente la funcionalidad para usuarios reales en mercados específicos, usando el mismo indicador de backend para controlar el porcentaje.

El siguiente diagrama muestra el concepto de un indicador de funcionalidad:

![Diagrama de feature flags](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff39e3234-cd4d-42b7-9b50-d0dc82c9e21a_2130x1226.png)

Esta separación entre **desplegar código** y **activar una funcionalidad** es un patrón muy potente en el proceso de publicación de Spotify. Permite que el código esté en la aplicación, madurando en condiciones de producción de forma invisible, y se active más tarde. Si algo sale mal después de la activación, la funcionalidad puede desactivarse sin publicar una nueva versión. A la escala de Spotify, los indicadores de funcionalidad son un mecanismo de seguridad central, aunque gestionar cientos de ellos en una organización grande, cada uno con controles por mercado y por porcentaje de usuarios, es en sí mismo un reto de ingeniería.

El Gestor de Publicaciones también tomó una decisión de coordinación deliberada para la versión 8.9.2. Dado que la funcionalidad de Audiolibros era un lanzamiento de alto impacto con eventos de marketing ya programados, otra funcionalidad importante que se había planificado para la misma versión se reprogramó para la semana siguiente. Menos variables en una sola publicación significa un diagnóstico más fácil si algo sale mal. Ese tipo de decisión es una de las cosas que diferencia la gestión de publicaciones de la simple automatización.

---

## De Jira a un centro de control de publicaciones

El sistema de múltiples anillos genera una gran cantidad de datos: tasas de fallos, tickets de errores, estados de aprobación, resultados de verificación de compilaciones y progreso de la revisión en la tienda de aplicaciones. Alguien tiene que dar sentido a todo eso, y no era una tarea sencilla.

Antes de que existiera el Panel del Gestor de Publicaciones, todo vivía en Jira. El Gestor de Publicaciones tenía que saltar entre tickets, verificar estados en múltiples vistas y comprobar condiciones manualmente, todo mientras respondía preguntas de los equipos en Slack. Era fácil pasar por alto un pequeño detalle, y un detalle perdido podía significar trabajo extra o un error que se colaba.

Así que el equipo de Publicaciones construyó un centro de mando dedicado con objetivos claros:

- Optimizar el flujo de trabajo del Gestor de Publicaciones
- Minimizar el cambio de contexto
- Reducir la carga cognitiva
- Permitir decisiones rápidas y precisas

El resultado fue el **Panel del Gestor de Publicaciones** (*Release Manager Dashboard*), construido como un plugin sobre Backstage, el portal interno de desarrolladores de Spotify.

![Panel del Gestor de Publicaciones](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7849884c-3db2-4347-85b0-6d5477da49de_1600x1150.png)

*Fuente: [Spotify Engineering Blog](https://engineering.atspotify.com/2025/04/how-we-release-the-spotify-app-part-1)*

Extrae y agrega datos de alrededor de 10 sistemas de backend diferentes en una única vista. Para cada plataforma (Android, iOS, escritorio), el panel muestra errores bloqueantes, el estado de la última compilación, resultados de pruebas automatizadas, tasas de fallos normalizadas respecto al uso real (de modo que una tasa de fallos sea significativa tanto si la usan 1.000 como 1.000.000 de personas), el progreso de aprobación de los equipos y el estado del despliegue. Todo está codificado por colores:

- **Verde** significa listo para avanzar
- **Amarillo** significa que algo necesita atención
- **Rojo** significa que hay un problema que requiere acción

A continuación se muestra un ejemplo de cómo aparece el panel:

![Ejemplo del panel de publicaciones](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9debd507-7496-461a-b6b5-81ff68498a4f_1600x1464.png)

*Fuente: [Spotify Engineering Blog](https://engineering.atspotify.com/2025/04/how-we-release-the-spotify-app-part-1)*

El panel también muestra los criterios de publicación como una lista de verificación visible:

- Todos los commits en la rama de publicación están incluidos en la última compilación y superan las pruebas
- No hay tickets de errores bloqueantes abiertos
- Todos los equipos han dado su aprobación
- Las tasas de fallos están por debajo de los umbrales definidos
- Uso real suficiente de la compilación

Cuando todo se vuelve verde, la versión está lista para avanzar.

Sin embargo, el panel tuvo un comienzo difícil. La primera versión era lenta y costosa. Cada recarga de página disparaba consultas a los 10 sistemas fuente de los que dependía, causando tiempos de carga prolongados y costes elevados. El equipo de ingeniería de Spotify señaló que cada recarga costaba aproximadamente lo mismo que un buen almuerzo en Estocolmo. Después de cambiar a caché y preagregar datos cada cinco minutos, el tiempo de carga bajó a ocho segundos y el coste se volvió insignificante.

---

## El Robot: automatizar lo predecible, reservar a los humanos para lo ambiguo

El panel le dio al Gestor de Publicaciones la información necesaria para tomar decisiones rápidas.

Sin embargo, al analizar los datos de series temporales que generaba el panel, el equipo se dio cuenta de que gran parte del tiempo del ciclo de publicación no se empleaba en decisiones difíciles, sino en **esperar**.

Los mayores sumideros de tiempo eran las pruebas y la corrección de errores (inevitables), la espera de la aprobación de la tienda de aplicaciones (fuera del control de Spotify) y los retrasos por avanzar manualmente una publicación cuando un paso se completaba fuera del horario laboral. Solo ese último punto podía costar hasta 12 horas. Si la tienda de aplicaciones aprobaba una compilación a las 11 de la noche, la publicación simplemente esperaba hasta que alguien se despertara y pulsara "siguiente".

Por eso, el equipo construyó lo que llamaron **"el Robot"**.

Es un servicio de backend que modela el proceso de publicación como una **máquina de estados**: un conjunto de etapas definidas con condiciones específicas que deben cumplirse antes de pasar a la siguiente. El Robot rastrea siete estados. Los cinco estados del camino normal hacia adelante son: rama de publicación creada, candidato a versión final (la compilación que se publicará realmente), enviado para revisión en la tienda de aplicaciones, desplegado al 1% y desplegado al 100%. Dos estados adicionales gestionan los problemas: el despliegue se pausa o la publicación se cancela por completo.

El siguiente diagrama lo ilustra:

![Diagrama de la máquina de estados del Robot](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F555355ab-2ff0-42d9-a7c6-65c3f199dcb5_2396x2572.png)

*Fuente: [Spotify Engineering Blog](https://engineering.atspotify.com/2025/04/how-we-release-the-spotify-app-part-1)*

El Robot comprueba continuamente si se cumplen las condiciones para avanzar al siguiente estado. Si las pruebas manuales están aprobadas, no hay tickets de errores bloqueantes abiertos y las pruebas automatizadas pasan en el último commit de la rama de publicación, el Robot envía automáticamente la compilación para la revisión de la tienda de aplicaciones sin intervención humana. Si la tienda aprueba la compilación a las 3 de la madrugada, el Robot inicia el despliegue al 1% de inmediato en lugar de esperar a que alguien llegue a la oficina.

El resultado fue una reducción media de unas **ocho horas** por ciclo de publicación.

Sin embargo, el Robot no toma las decisiones difíciles. No decide si un fallo que afecta a usuarios de una región específica es lo suficientemente grave como para bloquear una publicación. No decide si un error en una nueva funcionalidad como Audiolibros, con eventos de marketing ya programados, debería retrasar toda la publicación o solo el despliegue de esa funcionalidad. No negocia con los equipos de funcionalidades sobre el timing. Esas decisiones requieren juicio, contexto y, en ocasiones, conversaciones difíciles. El Gestor de Publicaciones se encarga de todas ellas.

Esta división es deliberada. Las transiciones predecibles que dependen de verificaciones de reglas se automatizan. Las decisiones ambiguas que requieren coordinación y criterio se quedan con los humanos.

---

## Conclusión

Spotify publica semanalmente para 675 millones de usuarios gracias a una sólida arquitectura de publicación. La exposición por capas detecta los errores donde son más baratos de corregir, y las herramientas centralizadas convierten los datos dispersos en decisiones rápidas. La automatización se encarga de lo predecible para que los humanos puedan centrarse en lo ambiguo.

La lección clave aquí es que **la velocidad y la seguridad no son opuestos**. En Spotify, cada una habilita a la otra. Un ciclo semanal significa que cada publicación lleva menos cambios. Menos cambios significa menos riesgo por publicación. Menos riesgo significa publicar con confianza.

Dado que una publicación cancelada solo cuesta una semana, no un mes o un trimestre, los equipos están más dispuestos a matar una mala publicación en lugar de empujarla hacia adelante y esperar lo mejor.

---

## Referencias

- [A behind-the-scenes look at how we release the Spotify App - Part 1](https://engineering.atspotify.com/2025/04/how-we-release-the-spotify-app-part-1)
- [How we release the Spotify app: Under the Hood (Part 2)](https://engineering.atspotify.com/2026/2/how-we-release-the-spotify-app-part-2)
- [Branching (version control) — Wikipedia](https://en.wikipedia.org/wiki/Branching_(version_control))
- [Feature Toggles (aka Feature Flags) — Martin Fowler](https://martinfowler.com/articles/feature-toggles.html)