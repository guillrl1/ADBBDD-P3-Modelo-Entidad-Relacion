# Modelo entidad/relación — Viveros (Tajinaste S.A.)

Administración y diseño de bases de datos · Grado en Ingeniería Informática · ULL

**Integrante:** Guillermo López Concepción

![Modelo E/R](https://github.com/guillrl1/ADBBDD-P3-Modelo-Entidad-Relacion/blob/main/ejercicio3_entidad_relacion.drawio.png)

Ficheros: [`ejercicio3_entidad_relacion.drawio`](https://github.com/guillrl1/ADBBDD-P3-Modelo-Entidad-Relacion/blob/main/ejercicio3_entidad_relacion.drawio) · [`ejercicio3_entidad_relacion_drawio.png`](https://github.com/guillrl1/ADBBDD-P3-Modelo-Entidad-Relacion/blob/main/ejercicio3_entidad_relacion.drawio.png)

**Notación**: rectángulo = entidad; rectángulo doble = entidad débil; rombo = relación (`ID` = dependencia en identificación); elipse subrayada = identificador; elipse doble = atributo compuesto; triángulo = jerarquía. La participación `(mín,máx)` se escribe junto a la entidad e indica cuántas ocurrencias de esa entidad se asocian con una ocurrencia de la entidad del otro extremo.

---

## 1. Entidades

| Entidad | Tipo | Descripción |
|---|---|---|
| **VIVERO** | Fuerte | Establecimiento de la red de Tajinaste S.A. |
| **ZONA** | Débil (ID de VIVERO) | Área de un vivero (exterior, almacén…). Se identifica con `id_vivero` + `id_zona`. |
| **PRODUCTO** | Fuerte | Planta, producto de jardinería o decoración que se vende. |
| **EMPLEADO** | Fuerte | Persona que trabaja en la empresa. |
| **PUESTO** | Fuerte | Histórico de puestos de los empleados. |
| **TAREA** | Fuerte | Tarea que se desempeña en un puesto. |
| **CLIENTE** | Fuerte | Cliente de la empresa. |
| **CLIENTE TAJINASTE PLUS** | Subentidad de CLIENTE (jerarquía parcial) | Cliente adscrito al programa de fidelización. |
| **BONIFICACIÓN** | Débil (ID de CLIENTE TAJINASTE PLUS) | Bonificación asignada a un cliente Plus según su volumen de compras. |
| **PEDIDO** | Fuerte | Pedido realizado por un cliente. |

## 2. Atributos y dominios

| Entidad / relación | Atributo | Tipo | Dominio | Ejemplo |
|---|---|---|---|---|
| VIVERO | `id_vivero` | Identificador | Entero | `3` |
| | `nombre` | Descriptor | Texto | `Vivero La Laguna` |
| | `georreferenciación` | Compuesto | — | — |
| | ↳ `latitud` | Descriptor | Real | `28.4874` |
| | ↳ `longitud` | Descriptor | Real | `-16.3159` |
| ZONA | `id_zona` | Identificador (parcial) | Entero | `2` |
| | `nombre` | Descriptor | Texto | `Zona exterior`, `Almacén` |
| | `georreferenciación` (`latitud`, `longitud`) | Compuesto | Igual que en VIVERO | `28.4871, -16.3162` |
| PRODUCTO | `código_prod` | Identificador | Texto | `PL-00231` |
| | `nombre` | Descriptor | Texto | `Drago 40 cm` |
| | `precio` | Descriptor | Real | `24.90` |
| EMPLEADO | `dni` | Identificador | Texto | `12345678Z` |
| | `nombre`, `apellidos` | Descriptor | Texto | `Ana`, `Pérez Díaz` |
| PUESTO | `fecha_inicio` | Identificador | Fecha | `2026-06-01` |
| | `tipo` | Descriptor | Texto | `Jardinero`, `Cajero` |
| | `fecha_fin` | Descriptor | Fecha (nula si el puesto está vigente) | `2026-09-30` |
| TAREA | `nombre` | Identificador | Texto | `Riego` |
| | `descripción` | Descriptor | Texto | `Riego de la zona exterior` |
| CLIENTE | `id_cliente` | Identificador | Entero | `1045` |
| | `nombre`, `apellidos` | Descriptor | Texto | `Luis`, `Gómez` |
| CLIENTE TAJINASTE PLUS | `fecha_ingreso` | Descriptor | Fecha | `2025-11-03` |
| BONIFICACIÓN | `tipo_bonificación` | Descriptor | Texto | `Descuento 5 %` |
| | `volumen_compra` | Descriptor | Real | `350.00` |
| PEDIDO | `número_pedido` | Identificador | Entero | `90017` |
| | `fecha` | Descriptor | Fecha | `2026-09-12` |
| | `importe` | Descriptor | Real | `87.40` |
| *almacena* | `cantidad` | Atributo propio | Entero | `35` |
| *contiene* | `cantidad` | Atributo propio | Entero | `3` |
| | `precio` | Atributo propio | Real | `22.90` |

## 3. Relaciones y cardinalidad

| Relación | Entidades | Cardinalidad | Participaciones |
|---|---|---|---|
| **tiene** (ID) | VIVERO – ZONA | 1:N | Una zona pertenece a exactamente 1 vivero `(1,1)`; un vivero tiene 1..N zonas `(1,N)`. |
| **almacena** | ZONA – PRODUCTO | N:M | Un producto está en 1..N zonas `(1,N)`; una zona contiene 0..N productos `(0,N)`. `cantidad` = stock del producto en la zona. |
| **ubicado en** | PUESTO – ZONA | 1:N | Un puesto se ubica en exactamente 1 zona `(1,1)`; una zona tiene 0..N puestos `(0,N)`. |
| **realiza** | TAREA – PUESTO | 1:N | Un puesto tiene exactamente 1 tarea `(1,1)`; una tarea se asocia a 1..N puestos `(1,N)`. |
| **ocupa** | EMPLEADO – PUESTO | 1:N | Un puesto es de exactamente 1 empleado `(1,1)`; un empleado ocupa 1..N puestos `(1,N)`. |
| **gestiona** | EMPLEADO – PEDIDO | 1:N | Un pedido lo gestiona exactamente 1 empleado `(1,1)`; un empleado gestiona 0..N pedidos `(0,N)`. |
| **contiene** | PEDIDO – PRODUCTO | N:M | Un pedido contiene 1..N productos `(1,N)`; un producto aparece en 0..N pedidos `(0,N)`. Atributos `cantidad` y `precio`. |
| **realiza** | CLIENTE – PEDIDO | 1:N | Un pedido lo realiza exactamente 1 cliente `(1,1)`; un cliente realiza 0..N pedidos `(0,N)`. |
| **es un tipo de** | CLIENTE – CLIENTE TAJINASTE PLUS | Jerarquía parcial | Todo cliente Plus es un cliente `(1,1)`; un cliente puede ser o no Plus `(0,1)`. |
| **recibe** (ID) | CLIENTE TAJINASTE PLUS – BONIFICACIÓN | 1:N | Una bonificación pertenece a exactamente 1 cliente Plus; un cliente Plus recibe 0..N bonificaciones. |

## 4. Restricciones semánticas

- **RS1.** Un empleado nunca tiene dos destinos: los periodos `[fecha_inicio, fecha_fin]` de los puestos de un mismo empleado no pueden solaparse.
- **RS2.** `cantidad ≥ 0` y `fecha_fin ≥ fecha_inicio`.
- **RS3.** Cada pedido tiene un único empleado responsable.
- **RS4.** Los pedidos Plus que se contabilizan cumplen `fecha ≥ fecha_ingreso`.
