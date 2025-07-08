
# API de Acceso a Datos de Gaolos

## Descripción
La API de Gaolos proporciona un acceso estructurado a los datos a través de varios endpoints diseñados para facilitar la gestión y recuperación de información específica. Este documento describe el funcionamiento y la utilización de los diferentes endpoints, incluyendo la definición de los parámetros de entrada y el formato de la respuesta.

## Índice
- [Obtener información de un cliente](#Obtener-información-de-un-cliente)
- [Obtener Factura](#obtener-factura)
- [Obtener Detalles Factura](#obtener-detalles-factura)
- [Obtener Facturas Entre Fechas](#obtener-facturas-entre-fechas)
- [Obtener Presupuesto](#obtener-presupuesto)
- [Obtener Detalles Presupuesto](#obtener-detalles-presupuesto)
- [Obtener Asistencias Entre Fechas](#obtener-asistencias-entre-fechas)
- [Obtener Mantenimiento](#obtener-mantenimiento)
- [Obtener Documento](#obtener-documento)
- [Devolución de un Error](#devolución-de-un-error)
- [Restricciones](#restricciones)
- [Licencia](#licencia)
## Uso

### Obtener información de un cliente
Para solicitar las facturas, presupuestos y mantenimientos de un cliente, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetclienteinformacion?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_cli2","año"],
    "ParamsValues": [34359],[2024]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Clave de identificación del cliente y el año como límite para las facturas.
- `ParamsValues`: Identificador del cliente y el año.

**Nota:** `ParamsValues` debe contener el identificador único del cliente. La API devuelve los datos de un único cliente por solicitud.

### Respuesta de la API

#### Formato de la Respuesta
La API devuelve los siguientes campos para la información del cliente:

- **Datos del cliente:**
  - `detfacs`: Lista de identificadores de las facturas (id_fac).
  - `detpres`: Lista de identificadores de los presupuestos y sí está en borrador (id_pres2, borrador).
  - `detman`: Lista de identificadores de los mantenimientos (id_man2).

## Ejemplo de Respuesta

```json
{
  "obj": {
    "detfacs": [
      {
        "id_fac": 393153
      }
    ],
    "detpres": [
      {
        "id_pres2": 81248,
        "borrador": false
      }
    ],
    "detman": [
      {
        "id_man2": 13830
      }
    ]
  },
  "err": {
    "eserror": false,
    "salir": false
  }
}
```

---

### Obtener Factura
Para solicitar los datos de una factura específica, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetfactura?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_fac"],
    "ParamsValues": [4011118]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Clave de identificación de la factura.
- `ParamsValues`: Identificador de la factura que se desea obtener.

**Nota:** `ParamsValues` debe contener el identificador único de la factura. La API devuelve los datos de una única factura por solicitud.

### Respuesta de la API

#### Formato de la Respuesta
La API devuelve los siguientes campos para la factura y sus vencimientos:

- **Factura:**
  - `id_fac`: Identificador de la factura.
  - `fe_fa`: Fecha de la factura.
  - `usu`: Usuario que generó la factura.
  - `sicob`: Estado de cobro (totalmente cobrada o no).
  - `serie`: Serie de la factura.
  - `emp`: Cliente.
  - `id_cli2`: Identificador del cliente.
  - `admin`: Administrador relacionado (si aplica).
  - `id_adm2`: Identificador del administrador relacionado (si aplica).
  - `base`: Importe base.
  - `total`: Importe total.
  - `numenv`: Número de envíos por correo electrónico.
  - `ven`: Lista de vencimientos.
  - `obs`: Observaciones de la factura.
  - `id_soli2`: Identificador de la tarea pendiente.
  - `id_manplan`: Identificador de la planificación de mantenimiento, si existe.

- **Vencimiento:**
  - `fe_ve`: Fecha de vencimiento.
  - `imp`: Importe.
  - `forma`: Forma de pago.
  - `cuecli`: IBAN del cliente.
  - `cueneg`: IBAN de destino.
  - `fe_cob`: Fecha de cobro, si está cobrada.
  - `fe_dev`: Fecha de la última devolución, si ha sido devuelta.
  - `enespera`: Fecha de negociación del cobro, si está por negociar.

## Ejemplo de Respuesta

```json
{
  "obj": {
    "id_fac": 4011118,
    "fe_fa": "2024-03-22T00:00:00",
    "usu": "Juan Perez",
    "sicob": true,
    "serie": "R24-",
    "emp": "CONSTRUCCIONES MANOLO, S.L. ",
    "id_cli2": "12345",
    "admin": "",
    "id_adm2": "",
    "base": -24,
    "total": -29.04,
    "numenv": 1,
    "ven": [
      {
        "fe_Ve": "2024-03-22T00:00:00",
        "imp": -29.04,
        "forma": "Compensado",
        "cuecli": null,
        "cueneg": null,
        "fe_cob": "2024-03-22T00:00:00",
        "fe_dev": null,
        "enespera": null
      }
    ],
   "obs": "Factura corregida por error en importe.",
    "id_soli2": "9876"
  },
  "err": {
    "mensaje": null,
    "eserror": false
  }
}
```

---

### Obtener Detalles Factura
Para solicitar los detalles de una factura específica, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetfacturadetalles?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_fac"],
    "ParamsValues": [4011118]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Clave de identificación de la factura.
- `ParamsValues`: Identificador de la factura que se desea obtener.

**Nota:** `ParamsValues` debe contener el identificador único de la factura. La API devuelve los detalles de una única factura por solicitud.

### Respuesta de la API

#### Formato de la Respuesta
La API devuelve los siguientes campos para la factura:

- **Factura:**
  - `det`: detalles de la factura.

- **Factura Detalles:**
  - `can`: Cantidad.
  - `expo`: Exposición del servicio editado.
  - `id_serv2`: Identificador del servicio.
  - `serv`: Descripción del servicio.
  - `preciouf`: Precio unitario final.

## Ejemplo de Respuesta

```json
{
  "obj": {
    "det": [
      {
        "can": "10",
        "expo": "Descripción del servicio 1",
        "id_serv2": "001",
        "serv": "Servicio 1",
        "preciouf": "100.00"
      },
      {
        "can": "20",
        "expo": "Descripción del servicio 2",
        "id_serv2": "002",
        "serv": "Servicio 2",
        "preciouf": "200.00"
      }
    ]
  },
  "err": {
    "eserror": false,
    "salir": false
  }
}
```

---


### Obtener Facturas Entre Fechas
Para obtener la lista de identificadores de facturas dentro de un intervalo de fechas, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetfacturasentrefechas?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["fe_in","fe_fi"],
    "ParamsValues": ["01/02/2025"],["06/02/2025"]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Lista de claves de los parámetros **`fe_in`** (fecha de inicio) y **`fe_fi`** (fecha de fin).
- `ParamsValues`: Lista de valores de las fechas correspondientes en formato **`dd/MM/yyyy`**, donde:
  - **`fe_in`**: Fecha de inicio del período de consulta.
  - **`fe_fi`**: Fecha de fin del período de consulta.

**Nota:** `ParamsValues` debe contener las fechas de inicio (**`fe_in`**) y fin (**`fe_fi`**) en formato `dd/MM/yyyy`. La API devuelve una lista de identificadores de facturas dentro del intervalo de fechas especificado.

**Importante:** El intervalo de fechas entre `fe_in` y `fe_fi` **no puede ser superior a 7 días**.


### Respuesta de la API

#### Formato de la Respuesta
La API devuelve una lista de identificadores de facturas dentro del intervalo de fechas especificado.

- **Facturas:**
  - `obj`: Lista de identificadores únicos de facturas.

## Ejemplo de Respuesta

```json
{
  "obj": [392781, 392782, 392783, 392784],
  "err": {
    "eserror": false,
    "salir": false
  }
}
```

---


### Obtener Presupuesto
Para solicitar los datos de un presupuesto específico, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetpresupuesto?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_pres2"],
    "ParamsValues": [4011118]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Clave de identificación del presupuesto.
- `ParamsValues`: Identificador del presupuesto que se desea obtener.

**Nota:** `ParamsValues` debe contener el identificador único del presupuesto. La API devuelve los datos de un único presupuesto por solicitud.

### Respuesta de la API

#### Formato de la Respuesta
La API devuelve los siguientes campos para el presupuesto:

- **Presupuesto:**
  - `id_pres2`: Identificador del presupuesto.
  - `fe_al`: Fecha de creación.
  - `tipopres`: Tipo presupuesto, si procede.
  - `breve`: Breve comentario.
  - `cli`: Cliente.
  - `id_cli2`: Identificador del cliente.
  - `admin`: Administrador relacionado (si aplica).
  - `id_adm2`: Identificador del administrador relacionado (si aplica).
  - `base`: Importe base.
  - `total`: Importe total.
  - `numenv`: Número de envíos por correo electrónico.
  - `obs_priv`: Observaciones privadas.

## Ejemplo de Respuesta

```json
{
  "obj": {
    "fe_al": "2024-03-18T10:19:36.217",
    "id_pres2": 992446,
    "tipopres": null,
    "breve": "",
    "cli": "C.P. LEPANTO 888",
    "id_cli2": 888,
    "admin": null,
    "id_adm2": null,
    "base": 58,
    "total": 70.18,
    "numenv": 0
  },
  "err": {
    "eserror": false,
    "salir": false
  }
}
```

---

### Obtener Detalles Presupuesto
Para solicitar los detalles de un presupuesto específico, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetpresupuestodetalles?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_pres2"],
    "ParamsValues": [4011118]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Clave de identificación del presupuesto.
- `ParamsValues`: Identificador del presupuesto que se desea obtener.

**Nota:** `ParamsValues` debe contener el identificador único del presupuesto. La API devuelve los detalles de un único presupuesto por solicitud.

### Respuesta de la API

#### Formato de la Respuesta
La API devuelve los siguientes campos para el presupuesto:

- **Presupuesto:**
  - `det`: detalles del presupuesto.

- **Presupuesto Detalles:**
  - `can`: Cantidad.
  - `expo`: Exposición del servicio editado.
  - `id_serv2`: Identificador del servicio.
  - `serv`: Descripción del servicio.
  - `preciouf`: Precio unitario final.

## Ejemplo de Respuesta

```json
{
  "obj": {
    "det": [
      {
        "can": "10",
        "expo": "Descripción del servicio 1",
        "id_serv2": "001",
        "serv": "Servicio 1",
        "preciouf": "100.00"
      },
      {
        "can": "20",
        "expo": "Descripción del servicio 2",
        "id_serv2": "002",
        "serv": "Servicio 2",
        "preciouf": "200.00"
      }
    ]
  },
  "err": {
    "eserror": false,
    "salir": false
  }
}
```

---


### Obtener Asistencias Entre Fechas
Para obtener la lista de identificadores de asistencias dentro de un intervalo de fechas, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetasistenciasentrefechas?paramsin=
```

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["fe_in","fe_fi"],
    "ParamsValues": ["01/02/2025"],["06/02/2025"]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Lista de claves de los parámetros **`fe_in`** (fecha de inicio) y **`fe_fi`** (fecha de fin).
- `ParamsValues`: Lista de valores de las fechas correspondientes en formato **`dd/MM/yyyy`**, donde:
  - **`fe_in`**: Fecha de inicio del período de consulta.
  - **`fe_fi`**: Fecha de fin del período de consulta.

**Nota:** `ParamsValues` debe contener las fechas de inicio (**`fe_in`**) y fin (**`fe_fi`**) en formato `dd/MM/yyyy`. La API devuelve una lista de identificadores de asistencia dentro del intervalo de fechas especificado.

**Importante:** El intervalo de fechas entre `fe_in` y `fe_fi` **no puede ser superior a 7 días**.


### Respuesta de la API

#### Formato de la Respuesta
La API devuelve una lista de identificadores de asistencia dentro del intervalo de fechas especificado.

- **Asistencias:**
  - `obj`: Lista de asistencias con los siguientes datos:
    - `id_asis2`: Identificador único de la asistencia.
    - `id_cli2`: Identificador del cliente asociado a la asistencia.
    - `id_man2`: Identificador del mantenimiento asociado a la asistencia (si aplica).

## Ejemplo de Respuesta

```json
{
  "obj": [
    {
      "id_asis2": 226964,
      "id_cli2": 26766,
      "id_man2": 14856
    },
    {
      "id_asis2": 226968,
      "id_cli2": 11443,
      "id_man2": 0
    },
    {
      "id_asis2": 226969,
      "id_cli2": 27624,
      "id_man2": 0
    },
    {
      "id_asis2": 226974,
      "id_cli2": 13112,
      "id_man2": 0
    }
  ],
  "err": {
    "eserror": false,
    "salir": false
  }
}
```

---


## Obtener Mantenimiento
Para solicitar los datos de mantenimiento específico, se debe realizar una petición al endpoint siguiente:

#### Parámetros de Entrada
La solicitud debe incluir los siguientes parámetros en formato JSON:

```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_man2"],
    "ParamsValues": [4011118]
  }
}
```

- `RefNeg`: Referencia del negocio.
- `ClaveSesion`: Token de autenticación.
- `ParamsKeys`: Clave de identificación del mantenimiento.
- `ParamsValues`: Identificador del mantenimiento que se desea obtener.

**Nota:** `ParamsValues` debe contener el identificador único del mantenimiento. La API devuelve los datos de un único mantenimiento por solicitud.

### Respuesta de la API

#### Formato de la Respuesta
La API devuelve los siguientes campos para el mantenimiento:

- **Mantenimiento:**
  - `id_man2`: Identificador del mantenimiento.
  - `fe_in`: Fecha de inicio.
  - `fe_exp`: Fecha de expiración, sí la hay.
  - `perivis`: El nº de días entre revisiones.
  - `mes`: Mes de la revisión anual.
  - `fe_prox`: Fecha de la próxima revisión.
  - `obspub`: Observaciones públicas.
  - `obspriv`: Observaciones privadas.
  - `id_asis2`: Nº de asistencia.
  - `fe_fin`: Fecha de finalización de la asistencia.
  - `usu_fin`: Usuario que finaliza la asistencia.
  - `manok`: Indica si el resultado del mantenimiento ha sido correcto.

## Ejemplo de Respuesta

```json
{
  "obj": {
    "fe_in": "2023-05-01T00:00:00",
    "fe_exp": null,
    "id_man2": 48,
    "perivis": 90,
    "mes": 5,
    "fe_prox": "2024-05-01T00:00:00",
    "obspub": "",
    "obspriv": "",
    "id_asis2": 112233,
    "fe_fin": null,
    "usu_fin": null,
    "manok": false
  },
  "err": {
    "eserror": false,
    "salir": false
  }
}
```
---

## Obtener Documento
Para solicitar un documento específico, se debe realizar una petición al endpoint siguiente:

```
https://api.gaolos.com/xxx/apirestgetdocumento?paramsin=
```


### Tipos de Documento

#### Documento Factura
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_fac"],
    "ParamsValues": [402298]
  }
}
```
*Devuelve la factura en formato PDF.*

#### Documento Presupuesto
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_pres2"],
    "ParamsValues": [402298]
  }
}
```
*Devuelve el presupuesto en formato PDF.*

#### Documento Mantenimiento
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_man2"],
    "ParamsValues": [402298]
  }
}
```
*Devuelve el certificado del último mantenimiento finalizado en formato PDF.*

#### Documento Mantenimiento Certificado - Método 1
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_manplan2c"],
    "ParamsValues": [402298]
  }
}
```
*Devuelve el certificado indicado en formato PDF.*

#### Documento Mantenimiento Certificado - Método 2
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_manplanc"],
    "ParamsValues": [23491001]
  }
}
```
*Devuelve el certificado indicado en formato PDF.*

#### Documento Mantenimiento Informe - Método 1
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_manplan2i"],
    "ParamsValues": [402298]
  }
}
```
*Devuelve el informe indicado en formato PDF.*

#### Documento Mantenimiento Informe - Método 2
##### Parámetros de Entrada
```json
{
  "parameters": {
    "RefNeg": "Referencia negocio",
    "ClaveSesion": "Token",
    "ParamsKeys": ["id_manplani"],
    "ParamsValues": [23491001]
  }
}
```
*Devuelve el informe indicado en formato PDF.*

## Ejemplo de Respuesta

```json
{
  "obj": {
    "tipo": "pdf",
    "nombre": "Factura R24-140.pdf",
    "fichero": "JVBERi0xLj......CAwIFINCj4+DQoNCnN0YXJ0eHJlZg0KNzIxMTMNCiUlRU9GDQo=",
    "tipoMime": "application/pdf"
  },
  "err": {
    "mensaje": null,
    "eserror": false
  }
}
```


---

## Devolución de un Error

En caso de producirse un error, devolverá el siguiente JSON, donde `eserror` indica si ha habido un error o no, y `mensaje` indicará el tipo de error.

```json
{
  "obj": null,
  "err": {
    "mensaje": "Factura no localizada",
    "eserror": true
  }
}
```

---

## Restricciones

Las restricciones que puede definir dentro de Gaolos para limitar el acceso a la API son:

- Por número de peticiones por tiempo.
- Por IP de origen.

El cliente es el encargado de dar de alta el acceso, bloqueo temporal o dar de baja el acceso a la API desde el Módulo de Dispositivos.

---

## Licencia

El uso de esta API es exclusivo para clientes de Gaolos.

