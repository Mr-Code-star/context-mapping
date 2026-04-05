# Context Mapping

> Guía de estudio en Español — Basada en Domain-Driven Design (DDD)  
> **Proyecto de ejemplo: ShopFacil**

---

## 1. ¿Qué es Context Mapping?

Context Maps describe el contacto entre bounded contexts y equipos de trabajo mediante una colección de patrones. Existen **nueve patrones** de context map y **tres tipos de relaciones** entre equipos.

Los patrones describen distintas perspectivas como: provisión de servicios, propagación de modelos y aspectos de gobierno. Esta diversidad de perspectivas permite obtener una visión holística de las relaciones entre equipos y bounded contexts.

### Context Maps se puede usar para:

- Analizar sistemas existentes o paisajes de aplicaciones
- Diseñar relaciones entre equipos de forma anticipada (upfront design)
- Visualizar quién influye en quién dentro de una organización
- Detectar problemas políticos o técnicos entre equipos

---

## 2. Los 3 tipos de relaciones entre equipos

### 2.1 Mutuamente Dependientes (Mutually Dependent)

Dos equipos o bounded contexts son mutuamente dependientes cuando sus artefactos de software o sistemas deben entregarse juntos para tener éxito. Se ve un vínculo recíproco estrecho entre datos, funcionalidades y capacidades de estos equipos. También necesitan mucha comunicación para coordinar sus esfuerzos.

> **Ejemplo ShopFacil:** El equipo de Pagos y el equipo de Pedidos deben lanzar sus cambios juntos en cada release. Si uno falla, el otro también falla.

### 2.2 Upstream / Downstream (Aguas arriba / Aguas abajo)

Las acciones de un equipo **upstream** (aguas arriba) tendrán efecto en el equipo **downstream** (aguas abajo), pero las acciones del downstream no tienen un impacto significativo en el upstream. El equipo upstream puede tener éxito independientemente del destino del equipo downstream.

> **Ejemplo ShopFacil:** El equipo de Catálogo (upstream) publica los productos. El equipo de Búsqueda (downstream) los consume. Si Catálogo cambia algo, Búsqueda debe adaptarse; pero si Búsqueda cambia algo, Catálogo no se entera.

### 2.3 Libre (Free)

Un Bounded Context o el equipo que trabaja en él es **libre** si los cambios en otros Bounded Contexts no influyen en su éxito o fracaso. No existe ningún vínculo organizacional ni técnico de ningún tipo entre estos equipos.

> **Ejemplo ShopFacil:** El sistema de RR.HH. gestiona vacaciones y nómina de empleados. La Tienda Online gestiona productos y pedidos. Estos dos mundos no tienen ninguna conexión.

---

## 3. Los 9 Patrones de Context Map

La comunidad de Domain-Driven Design describe nueve patrones de context mapping. Cada uno responde a un tipo específico de relación entre bounded contexts.

### 3.1 Patrones de integración de servicios

#### Open-host Service (OHS) — Servicio de anfitrión abierto

Un protocolo que da acceso a un subsistema como un conjunto de servicios. Se abre el protocolo para que todos los que necesiten integrarse puedan usarlo.

| | |
|---|---|
| **Bounded Context** | Pagos → Pedidos, Notificaciones, Reportes |
| **Ejemplo** | El equipo de Pagos expone una API REST pública con endpoints como `POST /cobrar` y `GET /estado-pago`. Cualquier otro sistema de ShopFacil puede usarla sin coordinación directa. |
| **Explicación** | El equipo de Pagos publica su API una sola vez y todos los sistemas se conectan solos. Sin OHS, cada equipo que necesite cobrar tendría que pedir al equipo de Pagos una integración personalizada. |
| **Úsalo cuando** | Cuando muchos equipos necesitan integrarse con tu sistema y no quieres coordinar con cada uno individualmente. |

---

#### Conformist (CF) — Conformista

Elimina la complejidad de la traducción entre bounded contexts adoptando fielmente el modelo del equipo upstream. Aunque esto limita el estilo del equipo downstream, elegir la conformidad simplifica enormemente la integración.

| | |
|---|---|
| **Bounded Context** | Búsqueda adopta el modelo de Catálogo |
| **Ejemplo** | El equipo de Búsqueda necesita saber qué productos existen. En lugar de crear su propio modelo de producto, replica la estructura exacta de Catálogo: mismo nombre de campos, misma jerarquía de categorías. |
| **Explicación** | Búsqueda es un equipo pequeño y Catálogo tiene miles de productos bien estructurados. Crear su propio modelo costaría demasiado. Conformarse es más barato aunque se pierda flexibilidad. |
| **Úsalo cuando** | Cuando el equipo upstream es muy poderoso o no negocia cambios, y la integración simple vale más que el modelo ideal. |

---

#### Anticorruption Layer (ACL) — Capa anticorrupción

Como cliente downstream, se crea una capa de aislamiento para proveer al sistema con funcionalidad del sistema upstream en términos del propio modelo de dominio. Esta capa traduce entre los dos modelos en una o ambas direcciones.

| | |
|---|---|
| **Bounded Context** | Facturación se protege del sistema de SUNAT |
| **Ejemplo** | SUNAT tiene su propio formato XML para facturas electrónicas (UBL 2.1) con campos como `cbc:InvoiceTypeCode`. El equipo de Facturación crea un Adaptador que traduce ese XML al lenguaje interno de ShopFacil: `{ tipo: 'factura', monto: 150.00 }`. |
| **Explicación** | Sin el ACL, todos los modelos internos de ShopFacil tendrían que usar la terminología de SUNAT. Cuando SUNAT cambie su formato (como suele hacer), solo el Adaptador necesita actualizarse, no todo el sistema. |
| **Úsalo cuando** | Cuando el modelo externo es malo o muy diferente al tuyo, y quieres proteger la integridad de tu sistema. |

---

#### Published Language (PL) — Lenguaje publicado

La traducción entre los modelos de dos bounded contexts requiere un lenguaje común. Se usa un lenguaje compartido y bien documentado que pueda expresar la información de dominio necesaria.

| | |
|---|---|
| **Bounded Context** | Envíos y Pedidos hablan un idioma común |
| **Ejemplo** | Pedidos y Envíos acordaron un evento estándar llamado `ShipmentEvent` con campos exactos: `orderId`, `status` (valores: `PENDIENTE`, `EN_CAMINO`, `ENTREGADO`), `timestamp` y `carrier`. |
| **Explicación** | Sin PL, Pedidos podría decir `'en_camino'` y Envíos decir `'shipped'`. Nadie se entendería. El lenguaje publicado es el contrato oficial que todos respetan. Ejemplos famosos: iCalendar (`.ics`) y vCard. |
| **Úsalo cuando** | Cuando múltiples sistemas deben intercambiar información y quieres evitar traducciones personalizadas para cada par. |

---

### 3.2 Patrones de colaboración entre equipos

#### Shared Kernel (SK) — Núcleo compartido

Se designa un subconjunto del modelo de dominio que los equipos acuerdan compartir. Se mantiene pequeño. Este código compartido tiene un estado especial y no debe cambiarse sin consulta al otro equipo.

| | |
|---|---|
| **Bounded Context** | Pedidos y Carrito comparten el modelo de Usuario |
| **Ejemplo** | Tanto Pedidos como Carrito necesitan saber quién es el usuario: su `userId`, `email` y `rol`. En vez de duplicar ese modelo en dos lugares, ambos equipos comparten la misma clase `Usuario` en un paquete compartido. Nadie la modifica sin reunión previa. |
| **Explicación** | Si cada equipo tuviera su propia versión de Usuario, con el tiempo diferirían. El Shared Kernel garantiza que 'Usuario' signifique lo mismo en todo el sistema. |
| **Úsalo cuando** | Cuando dos sistemas necesitan los mismos datos o lógica base y duplicarlos sería peor que compartirlos. Requiere mucha comunicación entre equipos. |

---

#### Partnership (P) — Asociación

Cuando el fracaso del desarrollo en cualquiera de los dos contextos resultaría en fallo de entrega para ambos, se forja una asociación entre los equipos. Se establece un proceso de planificación coordinada del desarrollo y gestión conjunta de la integración.

| | |
|---|---|
| **Bounded Context** | Promociones y Precios se necesitan mutuamente |
| **Ejemplo** | Para el Buen Fin, Promociones necesita saber los precios base para calcular descuentos, y Precios necesita saber qué promociones están activas para mostrar el precio correcto. Ambos equipos se reúnen cada sprint para coordinar interfaces y lanzan juntos. |
| **Explicación** | Si uno lanza sin el otro, los precios mostrados serían incorrectos. El Partnership formaliza que se comprometen a coordinarse o ambos fallan. |
| **Úsalo cuando** | Cuando el fracaso de un sistema inevitablemente causa el fracaso del otro, y ambos equipos pueden coordinarse activamente. |

---

#### Customer / Supplier (CUS/SUP) — Cliente / Proveedor

Cuando dos equipos están en una relación upstream-downstream, se establece una clara relación cliente/proveedor entre los dos equipos, lo que significa que las prioridades del downstream se consideran en la planificación del upstream.

| | |
|---|---|
| **Bounded Context** | Reportes (cliente) pide datos a Analytics (proveedor) |
| **Ejemplo** | El equipo de Reportes necesita la métrica 'tasa de conversión por categoría'. Se lo piden formalmente al equipo de Analytics. Analytics los trata como cliente: agenda el feature, estima el tiempo y se compromete a entregarlo en el sprint 12. |
| **Explicación** | A diferencia del Conformist, aquí Reportes tiene poder de pedir. Analytics no puede ignorarlos porque tienen un acuerdo formal de customer/supplier con compromisos reales. |
| **Úsalo cuando** | Cuando el equipo upstream puede atender las necesidades del downstream y quieres formalizar esa relación con compromisos reales. |

---

### 3.3 Patrones de aislamiento y separación

#### Separate Ways (SW) — Caminos separados

Cuando dos bounded contexts no tienen una relación significativa pueden separarse. Se declara que un bounded context no tiene ninguna conexión con los demás, permitiendo a los desarrolladores encontrar soluciones simples y especializadas dentro de este pequeño alcance.

| | |
|---|---|
| **Bounded Context** | RR.HH. y la Tienda Online no se integran |
| **Ejemplo** | El sistema de RR.HH. maneja vacaciones, nómina y asistencia de empleados. La Tienda Online maneja productos, pedidos y clientes. Estos dos mundos no tienen nada que compartir. Cada uno resuelve sus propios problemas de forma completamente independiente. |
| **Explicación** | Forzar una integración sería complejo y no aportaría valor. Separate Ways es la decisión consciente de mantenerlos separados para que cada uno evolucione libremente. |
| **Úsalo cuando** | Cuando la integración es muy costosa o compleja y ambos equipos pueden funcionar bien de forma autónoma. |

---

#### Big Ball of Mud (BBoM) — Gran bola de barro

Una parte del sistema que es un desastre por tener modelos mezclados y límites inconsistentes. No se debe dejar que este mal modelo se propague a otros Bounded Contexts. Big Ball of Mud es una demarcación de un mal modelo o mala calidad del sistema.

| | |
|---|---|
| **Bounded Context** | El sistema legacy de inventario (año 2005) |
| **Ejemplo** | El sistema de inventario tiene 20 años. Mezcla tablas de clientes con productos, la lógica de precios está dispersa en 40 stored procedures, y nadie entiende bien qué hace cada parte. La solución: ponerle un Adaptador (ACL) alrededor para que ese caos no se propague a Catálogo ni a ningún otro sistema moderno. |
| **Explicación** | No siempre se puede reescribir todo. El BBoM se identifica, se delimita con una línea punteada en el mapa, y se le pone una capa protectora (ACL). El resto del sistema vive feliz sin saber qué hay adentro. |
| **Úsalo cuando** | Para identificar y contener zonas problemáticas del sistema, asegurándose de que el caos no se propague a otros bounded contexts. |

---

## 4. Buenas prácticas para Context Maps

No es necesario poner todos los patrones y relaciones de equipo en un solo context map grande. Dicho mapa crecerá con el tiempo y será difícil de entender.

### 4.1 Prefiere mapas pequeños para preguntas específicas

Los Context Maps pueden responder preguntas muy específicas como:

- ¿Cómo se propaga el modelo de un sistema dado a través del paisaje de aplicaciones?
- ¿Qué tipo de influencia tiene un determinado equipo sobre los demás?
- ¿Quién tiene influencia sobre un equipo dado?
- ¿Cómo se están manejando las políticas entre equipos?

Trabaja con context maps más pequeños, orientados a responder preguntas específicas. Solo incluye aquellos patrones que te ayuden a responder esas preguntas.

### 4.2 Documenta y explica los patrones que vayas a usar

Context Mapping es una técnica poderosa, pero puede volverse difícil de entender para personas que no tienen experiencia con Domain-Driven Design. Incluso algunos nombres de patrones no son autoexplicativos para ciertos grupos de stakeholders.

Antes de empezar a dibujar context maps, decide cuáles patrones vas a usar y proporciona una explicación de esos patrones a todos los que van a trabajar con tu mapa. Los ejemplos siempre son una buena idea.

### 4.3 Trabaja con diferentes perspectivas

Usa múltiples context maps para diferentes perspectivas. Lo que le importa al equipo de arquitectura es diferente a lo que le importa al equipo de negocio. No intentes satisfacer a todos con un solo diagrama.

---

## 5. Tabla resumen de los 9 patrones

| Sigla | Patrón | En una línea |
|---|---|---|
| **OHS** | Open-host Service | Expones tu sistema como una API pública que todos pueden usar |
| **CF** | Conformist | Adoptas el modelo del upstream sin cambiarlo |
| **ACL** | Anticorruption Layer | Creas una capa traductora para proteger tu modelo |
| **PL** | Published Language | Todos usan un lenguaje común y bien documentado |
| **SK** | Shared Kernel | Dos equipos comparten un subconjunto del código o modelo |
| **P** | Partnership | Dos equipos se asocian formalmente y fallan o tienen éxito juntos |
| **CUS/SUP** | Customer / Supplier | El cliente influye en las prioridades del proveedor |
| **SW** | Separate Ways | Los equipos deciden no conectarse y resuelven sus problemas solos |
| **BBoM** | Big Ball of Mud | Una zona caótica que se identifica, contiene y aísla |

---

## 6. Proyecto ShopFacil — Mapa completo

Todos los patrones aplicados en un único proyecto ficticio de e-commerce peruano, para que puedas ver cómo conviven en la realidad.

| Patrón | Bounded Contexts involucrados | Situación en ShopFacil |
|---|---|---|
| **OHS** | Pagos → Pedidos, Notificaciones, Reportes | API REST pública de cobros que cualquier sistema consume |
| **CF** | Búsqueda adopta modelo de Catálogo | Búsqueda replica la estructura de productos sin traducción |
| **ACL** | Facturación ↔ SUNAT | Adaptador que traduce el XML de SUNAT al modelo interno |
| **PL** | Pedidos ↔ Envíos | Evento `ShipmentEvent` con campos acordados entre ambos |
| **SK** | Pedidos y Carrito comparten Usuario | Clase `Usuario` compartida en paquete común |
| **Partnership** | Promociones + Precios | Ambos coordinan y lanzan juntos en cada release |
| **CUS/SUP** | Reportes (cliente) ↔ Analytics (proveedor) | Reportes pide métricas y Analytics las agenda formalmente |
| **SW** | RR.HH. y Tienda Online | Ninguna conexión: cada uno resuelve sus problemas solo |
| **BBoM** | Sistema legacy de inventario (2005) | Zona caótica contenida con un ACL protector alrededor |

---

## 7. Para saber más

- [DDD Reference — Eric Evans](https://domainlanguage.com/ddd/reference)
- [Team Topologies](https://teamtopologies.com)
- [Context Mapper (herramienta)](https://contextmapper.org)
- [Miro Starter Kit — DDD Crew](https://github.com/ddd-crew/context-mapping)
- *Dynamic Reteaming* — Heidi Helfand
- *Pioneers, Settlers & Town Planners* — Simon Wardley
