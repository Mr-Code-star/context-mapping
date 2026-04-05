# Mapeo de Contextos

Los Mapas de Contexto describen el contacto entre contextos delimitados y equipos mediante una colección de patrones. Existen nueve patrones de mapas de contexto y tres tipos de relaciones entre equipos. Los patrones describen diversas perspectivas como la provisión de servicios, la propagación de modelos o aspectos de gobernanza. Esta diversidad de perspectivas permite obtener una visión holística de las relaciones entre equipos y contextos delimitados.

Los Mapas de Contexto pueden usarse para analizar sistemas o paisajes de aplicaciones existentes, pero también son útiles para el diseño anticipado. Sin embargo, hemos notado que muchas personas tienen dificultades para comenzar con los patrones de mapeo de contextos a partir de las definiciones en los libros de DDD existentes. Este repositorio de GitHub busca ayudarte con los mapas de contexto mediante una hoja de referencia rápida y un kit de inicio para Miro.

## Descripción general de las relaciones entre equipos y los patrones

### Relaciones entre Equipos

#### Mutuamente Dependientes

<img src="resources/mutual-dependent.jpg" alt="Mutuamente Dependientes" width=300/>

Dos equipos o contextos delimitados son mutuamente dependientes cuando sus artefactos de software o sistemas necesitan entregarse juntos para tener éxito y funcionar. Con frecuencia se observa un vínculo estrecho y recíproco entre los datos, la funcionalidad y las capacidades de estos equipos. Dichos equipos también requieren mucha comunicación entre sí para coordinar sus esfuerzos (ver el patrón Asociación).

#### Upstream / Downstream

<img src="resources/upstream-downstream.jpg" alt="Upstream / Downstream" width=300/>

Las acciones de un equipo upstream afectarán al equipo downstream, pero las acciones del downstream no tienen un impacto significativo en el upstream. "El equipo upstream puede tener éxito independientemente del destino del equipo downstream" (Cita de la [Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/)).

#### Libre

<img src="resources/free.jpg" alt="Libre" width=300/>

Un Contexto Delimitado o un equipo que trabaja en él es libre si los cambios en otros Contextos Delimitados no influyen en su éxito o fracaso. Por lo tanto, no existe ningún vínculo organizativo ni técnico de ningún tipo entre estos equipos.

### Patrones de Mapas de Contexto

La mayoría de las publicaciones en la comunidad de Domain-Driven Design describen actualmente nueve patrones de mapeo de contextos.

#### Servicio de Anfitrión Abierto (Open-host Service)

<img src="resources/ohs.jpg" alt="Servicio de Anfitrión Abierto" height=150/>

"Un protocolo que da acceso a tu subsistema como un conjunto de servicios. Abre el protocolo para que todos los que necesiten integrarse contigo puedan usarlo. Mejora y amplía el protocolo para manejar nuevos requisitos de integración, excepto cuando un solo equipo tiene necesidades idiosincrásicas. En ese caso, usa un traductor puntual para complementar el protocolo para ese caso especial, de modo que el protocolo compartido pueda seguir siendo simple y coherente." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

El equipo que proporciona un Servicio de Anfitrión Abierto generalmente se encuentra en una posición upstream, mientras que los clientes que lo usan son equipos downstream. Los equipos downstream pueden elegir ser conformistas o construir capas anticorrupción.

#### Conformista (Conformist)

<img src="resources/cf.jpg" alt="Conformista" height=150/>

"Elimina la complejidad de la traducción entre contextos delimitados adhiriéndote servilmente al modelo del equipo upstream. Aunque esto limita el estilo de los diseñadores downstream y probablemente no produce el modelo ideal para la aplicación, elegir la conformidad simplifica enormemente la integración. Además, compartirás un lenguaje ubicuo con tu equipo upstream. El upstream lleva las riendas, por lo que conviene facilitar la comunicación para ellos. El altruismo puede ser suficiente para que compartan información contigo." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

#### Capa Anticorrupción (Anticorruption Layer)

<img src="resources/acl.jpg" alt="Capa Anticorrupción" height=150/>

"Como cliente downstream, crea una capa de aislamiento que proporcione a tu sistema la funcionalidad del sistema upstream en términos de tu propio modelo de dominio. Esta capa se comunica con el otro sistema a través de su interfaz existente, requiriendo poca o ninguna modificación en el otro sistema. Internamente, la capa traduce en una o ambas direcciones según sea necesario entre los dos modelos." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

#### Núcleo Compartido (Shared Kernel)

<img src="resources/shared-kernel.jpg" alt="Núcleo Compartido" width=300/>

"Designa con un límite explícito un subconjunto del modelo de dominio que los equipos acuerdan compartir. Mantén este núcleo pequeño. Dentro de este límite, incluye, junto con este subconjunto del modelo, el subconjunto del código o del diseño de base de datos asociado a esa parte del modelo. Este elemento explícitamente compartido tiene un estatus especial y no debe modificarse sin consultar al otro equipo." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

#### Asociación (Partnership)

<img src="resources/partnership.jpg" alt="Asociación" width=300/>

"Cuando el fracaso en el desarrollo de cualquiera de los dos contextos implicaría el fracaso en la entrega de ambos, forja una asociación entre los equipos a cargo de los dos contextos. Establece un proceso de planificación coordinada del desarrollo y gestión conjunta de la integración. Los equipos deben cooperar en la evolución de sus interfaces para satisfacer las necesidades de desarrollo de ambos sistemas. Las funcionalidades interdependientes deben programarse para completarse en la misma versión." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

#### Desarrollo Cliente / Proveedor (Customer / Supplier Development)

<img src="resources/customer-supplier.jpg" alt="Cliente / Proveedor" width=300/>

"Cuando dos equipos se encuentran en una relación upstream-downstream, donde el equipo upstream puede tener éxito independientemente del destino del equipo downstream, las necesidades del downstream se abordan de diversas maneras con un amplio rango de consecuencias. [...] Establece una relación clara de cliente/proveedor entre los dos equipos, lo que significa que las prioridades del downstream se incorporan a la planificación del upstream. Negocia y presupuesta tareas para los requisitos del downstream de modo que todos entiendan el compromiso y el cronograma." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

#### Lenguaje Publicado (Published Language)

<img src="resources/published-language.jpg" alt="Lenguaje Publicado" width=300/>

"La traducción entre los modelos de dos contextos delimitados requiere un lenguaje común. [...] Usa un lenguaje compartido bien documentado que pueda expresar la información de dominio necesaria como medio común de comunicación, traduciendo hacia y desde ese lenguaje según sea necesario." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

Ejemplos ampliamente conocidos de un Lenguaje Publicado son iCalendar o vCard. El lenguaje publicado se combina frecuentemente con un servicio de anfitrión abierto.

#### Caminos Separados (Separate Ways)

<img src="resources/separate-ways.jpg" alt="Caminos Separados" width=300/>

Cuando dos contextos delimitados no tienen una relación significativa, pueden separarse.
"Declara que un contexto delimitado no tiene ninguna conexión con los demás, permitiendo a los desarrolladores encontrar soluciones simples y especializadas dentro de este pequeño ámbito." ([Fuente: Referencia DDD de Eric Evans](https://www.domainlanguage.com/ddd/reference/))

#### Gran Bola de Barro (Big Ball Of Mud)

<img src="resources/bbom.jpg" alt="Gran Bola de Barro" width=300/>

Un (parte de un) sistema que es un desastre por tener modelos mezclados y límites inconsistentes. No permitas que este modelo deficiente se propague a otros Contextos Delimitados. La Gran Bola de Barro es una demarcación de un modelo o calidad del sistema deficiente. Debes asegurarte de que este desorden no se propague a otros contextos delimitados.

## Buenas prácticas para los Mapas de Contexto

No es necesario incluir todos los patrones y relaciones entre equipos en un gran mapa de contexto. Dicho mapa crecerá con el tiempo y será difícil de entender. Te verás obligado a explicar un mapa de contexto tan complejo a una gran cantidad de partes interesadas que tienen perspectivas diferentes según sus roles o la naturaleza de su trabajo. Por ello, se recomienda tener en cuenta las siguientes sugerencias:

- Prefiere mapas de contexto pequeños para preguntas concretas
- Documenta y explica los patrones que vayas a utilizar
- Trabaja con diferentes perspectivas y múltiples mapas de contexto para cada perspectiva

### Mapas de contexto pequeños para preguntas concretas

Los Mapas de Contexto pueden responder a una gran variedad de preguntas, como por ejemplo:

- ¿Cómo se propaga el modelo de un sistema dado a través del paisaje de aplicaciones?
- ¿Qué tipo de influencia tiene un determinado equipo sobre los demás?
- ¿Quién influye en un equipo dado?
- ¿Cómo se juegan las dinámicas políticas?

Como puedes ver, estas preguntas son muy específicas y podemos responderlas con un mapa de contexto grande, pero ese mapa tarde o temprano se convertirá en una sobrecarga de información.

Por ello:

Trabaja con mapas de contexto más pequeños, orientados a responder preguntas concretas. Incluye solo aquellos patrones en estos mapas que te ayuden a responder dichas preguntas.

### Documenta y explica los patrones que vayas a usar

El mapeo de contextos es una técnica poderosa para visualizar las relaciones entre sistemas y equipos. Sin embargo, puede volverse difícil de entender para personas que no tienen experiencia con Domain-Driven Design o con los patrones de mapeo de contextos. Incluso algunos nombres de patrones no son autoexplicativos para ciertos grupos de partes interesadas, y lo mismo aplica a algunas de las definiciones.

Por ello:

Antes de comenzar a dibujar mapas de contexto, debes decidir qué patrones vas a usar. Te harás un gran favor a ti mismo y a las partes interesadas si proporcionas una explicación de esos patrones. Asegúrate de que todos los involucrados en tu mapa de contexto entiendan los patrones. Los ejemplos siempre son una buena idea para estas explicaciones.

## Hoja de Referencia Rápida del Mapa de Contexto

A continuación se muestra una hoja de referencia con breves descripciones de los patrones de mapeo de contextos:

![Hoja de Referencia del Mapa de Contexto](resources/context-map-cheat-sheet.png)

## Kit de Inicio Remoto para Mapeo de Contextos en Miro

Si realizas el mapeo de contextos con Miro, hay una copia de seguridad de tablero que te permite comenzar con todos los objetos para los patrones, relaciones entre equipos y límites. Además, el tablero de Miro contiene algunos ejemplos para ayudarte a empezar:

![Kit de Inicio Remoto para Mapeo de Contextos en Miro](resources/RemoteContextMappingStarterKit.jpg)

[Enlace a la Copia de Seguridad del Tablero de Miro](resources/Remote-Context-Mapping-Starter-Kit.rtb)

También puedes consultar (y comentar) [una versión de solo lectura del kit de inicio en Miro](https://miro.com/app/board/o9J_kqtuB6A=/)

## Mapeo de Contextos con Context Mapper

Si deseas mantener tus mapas de contexto en tu repositorio de código, [Context Mapper](https://contextmapper.org/) te permite describir tus contextos en un archivo de texto y generar un diagrama a partir de él. [Context Mapper](https://contextmapper.org/) está disponible [en línea (en el navegador vía Gitpod)](https://contextmapper.org/docs/online-ide/) o mediante plugins de IDE para [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=contextmapper.context-mapper-vscode-extension) y [Eclipse](https://marketplace.eclipse.org/content/context-mapper/).

## Lecturas Adicionales

- [Dynamic Reteaming](https://leanpub.com/dynamicreteaming)
- [Pioneers, Settlers & Town Planners](http://wardleypedia.org/mediawiki/index.php/Pioneers_settlers_town_planners)
- [Team Topologies](https://teamtopologies.com/)
- [Visualising Sociotechnical Architecture with Context Maps](https://speakerdeck.com/mploed/visualizing-sociotechnical-architectures-with-context-maps)

## Colaboradores

Gracias a todos los [colaboradores presentes y futuros](https://github.com/ddd-crew/context-mapping/graphs/contributors) y a las siguientes personas que contribuyeron a la Hoja de Referencia del Mapa de Contexto:

- [Kacper Gunia](https://twitter.com/cakper)
- [Nick Tune](https://github.com/ntcoding)

## Contribuciones y Comentarios

La Hoja de Referencia del Mapa de Contexto está disponible gratuitamente para su uso. Además, tus comentarios e ideas son bienvenidos para mejorar la técnica o crear versiones alternativas.

Si tienes preguntas, puedes contactarnos o abrir un [Issue](https://github.com/ddd-crew/context-map-cheat-sheet/issues/new/choose).

No dudes en enviarnos también un pull request con tus ejemplos o informes de experiencia.

También puedes comentar directamente en el [tablero de Miro de la Hoja de Referencia del Mapa de Contexto](https://miro.com/app/board/o9J_kqrI8ck=/)

[![CC BY 4.0][cc-by-shield]][cc-by]

Este trabajo está licenciado bajo una [Licencia Internacional Creative Commons Atribución 4.0][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
