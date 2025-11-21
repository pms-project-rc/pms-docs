## ESPECIFICACIÓN GENERAL DEL PRODUCTO

**Nombre del proyecto:** PMS

**Fecha** 19 de Noviembre de 2025

**Versión del documento** Primera

**Autor(es)** Juan Camilo D'Aleman Rodriguez

**Estado (Borrador / Aprobado)** Borrador

### 2. Control de versiones

**Tabla con:**

Fecha

Versión

Cambios relevantes

Autor

## I. VISIÓN GENERAL DEL PROYECTO
### 3. Resumen ejecutivo (para stakeholders)

**Descripción corta del proyecto:** Plataforma integral (PMS) para la gestión operativa y administrativa de un negocio de parqueadero y lavado de vehículos. Centraliza el registro de entradas/salidas, servicios de lavado, tarifas, mensualidades, convenios empresariales, control de gastos, cálculo de bonos y analítica en tiempo real, asegurando trazabilidad, reducción de errores y soporte a la toma de decisiones.

**Objetivos principales:**
- Diseñar y desarrollar un sistema unificado que reemplace procesos manuales dispersos (libretas, hojas de cálculo) por flujos digitales confiables.
- Automatizar el cálculo de tarifas (parqueo, lavado, recargos, descuentos de convenios), tiempos gratuitos y bonos diarios/mensuales de lavadores.
- Disminuir pérdidas por errores de cobro, omisiones de registro y falta de control de gastos.
- Proporcionar métricas operativas y financieras (ocupación, ingresos, gastos, rendimiento, tiempos promedio) en un dashboard de acceso inmediato.
- Asegurar una base modular escalable para futuras extensiones (facturación electrónica, nómina, pasarela de pagos en línea, analítica avanzada).
- Mantener un historial auditable de configuraciones (tarifas, porcentajes, descuentos) y operaciones clave.

**Beneficio esperado para stakeholders:** Mayor control del flujo de caja, visibilidad sobre el desempeño diario, transparencia en incentivos a empleados y capacidad de reaccionar rápidamente ante variaciones de demanda o costos.

### 4. Justificación del proyecto

**Problema actual / Situación inicial:** La operación se apoya en registros manuales (papel / planillas) y cálculos ad-hoc. Esto genera inconsistencias en tarifas, falta de trazabilidad de quién registró un evento, dificultad para consolidar ingresos y gastos, y poca visibilidad del rendimiento real.

**Impactos negativos identificados:**
- Errores recurrentes en cobros (omisión de recargos de cascos, tiempos extra no facturados, descuentos mal aplicados).
- Pérdida de tiempo operativo en búsqueda de información histórica (lavados por lavador, ocupación promedio, ingresos por tipo de servicio).
- Incentivos (bonos) percibidos como poco transparentes, afectando motivación del personal.
- Ausencia de alertas preventivas (mensualidades por vencer, ocupación crítica, tiempos fuera de rango) que deriva en decisiones reactivas.
- Dificultad para proyectar flujo de caja diario/semanal.

**Necesidades detectadas:**
- Sistematizar captura de datos operativos en tiempo real.
- Unificar reglas de negocio (tarifas, tiempos gratuitos, descuentos) en un modelo consistente.
- Mejorar trazabilidad (quién cambió un estado, quién eximió un cobro, cuándo se aplicó un ajuste de tarifas).
- Disponer de indicadores financieros y operativos consolidados sin procesamientos manuales.
- Reducir dependencia de conocimiento tácito individual.

**Oportunidad y beneficios esperados:**
- Incremento de ingresos facturados por mayor exactitud y reducción de omisiones.
- Aceleración de cierres de turno y generación automática de reportes.
- Base estructurada para expansión comercial (nuevos servicios, alianzas empresariales).
- Disminución del riesgo operativo ante rotación de personal.

**Alineación estratégica:** El proyecto se alinea con la necesidad de escalar el negocio manteniendo control, preparando una futura integración con servicios externos (pagos, facturación) y potenciando decisiones basadas en datos.

### 5. Alcance del producto

**Qué cubre (Fase inicial):**
- Gestión de parqueadero: ingreso/salida, cálculo de tarifas, ocupación en tiempo real, exenciones registradas.
- Gestión de lavados: registro, tipos de servicio, asignación de lavador, estados y tiempos.
- Tarifas y configuración: precios base, ajustes porcentuales, tiempos gratuitos, políticas de bono.
- Mensualidades: registro, renovación, alertas de vencimiento.
- Convenios y flotas: empresas, descuentos, importación masiva de vehículos, reportes por empresa.
- Control financiero operativo: gastos, vales, cálculo y acumulación de bonos.
- Dashboard y analítica: métricas de ocupación, ingresos, gastos, rendimiento operativo, tiempos de lavado e indicadores por lavador.
- Reportes y exportación: generación y descarga en CSV/PDF de datos clave.
- Seguridad y autenticación: roles definidos (Global, Operativo, Lavador) con JWT y control de permisos.
- Infraestructura básica: despliegue con Docker Compose, migraciones de base de datos, backups y monitoreo inicial.

**Qué NO cubre (Fase inicial):**
- Facturación electrónica oficial con integración a autoridades tributarias.
- Pasarela de pago en línea (solo registro de cobros internos).
- Nómina completa (solo cálculo de bonos y vales, no liquidaciones salariales completas).
- Aplicaciones móviles nativas (se entrega interfaz web responsive).
- Machine Learning / predicciones avanzadas de demanda.
- Integración contable automática con sistemas externos (exportación manual de CSV/PDF sí incluida).

**Entregables previstos:**
- Backend FastAPI con endpoints documentados (OpenAPI) y casos de uso alineados al dominio.
- Frontend SPA React + Tailwind operativa para perfiles definidos.
- Esquema de base de datos PostgreSQL con migraciones Alembic.
- Módulo de autenticación y autorización por roles.
- Implementación de reglas de negocio críticas (tarifas, tiempos gratuitos, cálculo de bonos, descuentos de convenios).
- Dashboard y panel de métricas operativas básicas.
- Módulo de importación de flotas (CSV/Excel) y exportación de reportes.
- Scripts y configuración Docker Compose para entornos local y staging.
- Estrategia de backup y guía de restauración inicial.
- Documentación técnica y de dominio (este documento + diagramas UML/PlantUML/C4).
- Suite de pruebas inicial (unitarias dominio + endpoints críticos + cobertura mínima).

## II. DESCRIPCIÓN FUNCIONAL
### 6. Descripción general del sistema
El sistema PMS es una plataforma web modular que orquesta la operación diaria de un negocio combinado de parqueadero y lavado de vehículos, integrando registro operacional, reglas de negocio y analítica en un único flujo consistente. En términos simples:

- Registra cada vehículo que ingresa (hora, tipo inferido por placa, cascos si motocicleta) y mantiene su estado hasta la salida.
- Gestiona servicios de lavado: crea órdenes, asigna lavadores disponibles, controla estados (En espera → En proceso → Terminado) y captura tiempos automáticamente para métricas de eficiencia.
- Aplica reglas de tarifas y configuración comercial: precios base, ajustes porcentuales, tiempos de parqueo gratuito según tipo de lavado, descuentos de convenios y exenciones manuales auditadas.
- Administra mensualidades y convenios empresariales, activando beneficios y alertas de vencimiento sin intervención manual repetitiva.
- Centraliza gastos, vales y cálculo de bonos para ofrecer indicadores financieros confiables (ingresos, costos, rendimiento operativo) evitando hojas dispersas y cálculos manuales.
- Genera un dashboard en tiempo real con ocupación del parqueadero, ingresos por lavado/parqueo, tiempos promedio, desempeño por lavador y alertas (mensualidades próximas a vencer, outliers de duración).
- Produce reportes y exportaciones (CSV/PDF) sin reprocesar datos externos, garantizando trazabilidad de quién hizo cada acción y cuándo.
- Asegura autenticación y autorización por roles (Global, Operativo, Lavador) separando permisos: configuración estratégica, operación diaria y ejecución de servicio.

Limitaciones operativas iniciales: no gestiona facturación electrónica ni pagos en línea, sino el registro interno y cálculo exacto del valor de servicios. Todo cambio relevante (tarifas, exenciones, ajustes) queda trazado para auditoría futura.

Flujo resumido típico: Ingreso vehículo → Clasificación y (opcional) creación de orden de lavado → Asignación de lavador → Captura de tiempos y cálculo de precio → Salida / cierre de turno → Generación automática de métricas y reportes.

El diseño (DDD + Hexagonal) separa reglas de negocio del acceso a datos, facilitando futuros refactors o integración con otros sistemas (facturación, pagos) sin romper la lógica central.

**Usuarios del sistema y su perfil**
-Administrador Global (1)
-Administrador Operacional (Varios)
-Lavadores (Varios)

### 7. Historias de usuario (alto nivel)

**Módulo: Autenticación y Gestión de Usuarios**

**HU-01 – Inicio de sesión del administrador global**

Como administrador global,

quiero iniciar sesión en el sistema usando mi nombre de usuario y contraseña,

para que pueda acceder al panel de administración y gestionar las operaciones del negocio.

**Criterios de aceptación:**

* Dado que el administrador global se encuentra en la pantalla de inicio de sesión,  
  cuando ingrese su nombre de usuario y contraseña válidos,  
  entonces el sistema debe verificar las credenciales en la base de datos y permitir el acceso al panel principal.  
* Dado que las credenciales sean incorrectas,  
  cuando el administrador intente iniciar sesión,  
  entonces el sistema debe mostrar un mensaje de error indicando “Usuario o contraseña inválidos”.  
* Dado que el administrador global haya iniciado sesión,  
  cuando intente acceder a recursos restringidos,  
  entonces el sistema debe permitir el acceso a todas las funcionalidades correspondientes a su rol.

**HU-02 – Recuperación de contraseña del administrador global**

Como administrador global,

quiero recuperar mi contraseña a través del correo electrónico registrado,

para que pueda restablecer mi acceso al sistema si la olvido.

**Criterios de aceptación:**

Dado que el usuario está en la pantalla de inicio de sesión,

cuando haga clic en “¿Olvidó su contraseña?” e ingrese su correo electrónico,

entonces el sistema debe enviar una nueva contraseña temporal al correo asociado a su cuenta.

Dado que el correo ingresado no esté asociado a ninguna cuenta,

cuando el usuario intente recuperar la contraseña,

entonces el sistema debe mostrar un mensaje de error indicando “Correo no registrado en el sistema”.

Dado que la nueva contraseña haya sido enviada,

cuando el administrador global vuelva a iniciar sesión,

entonces el sistema debe permitir el acceso únicamente si utiliza la contraseña temporal válida.

**HU-03 – Inicio de sesión del administrador operativo**

Como administrador operativo,

quiero iniciar sesión con mi usuario y contraseña,

para que pueda registrar vehículos, asignar lavadores y gestionar los servicios de mi turno.

**Criterios de aceptación:**

Dado que el administrador operativo se encuentra en la pantalla de inicio de sesión,

cuando ingrese sus credenciales válidas,

entonces el sistema debe autenticarlo y mostrarle únicamente las opciones de su rol (gestión de lavados, parqueadero, gastos y reportes de turno).

Dado que las credenciales ingresadas sean incorrectas,

cuando el usuario intente iniciar sesión,

entonces el sistema debe denegar el acceso y mostrar el mensaje “Usuario o contraseña incorrectos”.

Dado que el administrador operativo cierre sesión,

cuando se confirme la acción,

entonces el sistema debe finalizar la sesión y redirigir al formulario de login.

**HU-04 – Creación de perfiles de lavadores**

Como administrador global o administrador operativo,

quiero crear nuevos perfiles de lavadores,

para que pueda registrar empleados y vincularlos a los servicios de lavado y cálculo de bonos.

**Criterios de aceptación:**

Dado que el administrador se encuentra en la sección “Gestión de Lavadores”,

cuando haga clic en “Nuevo lavador” e ingrese nombre, identificación, porcentaje de bono y estado,

entonces el sistema debe registrar el nuevo perfil en la base de datos.

Dado que falte información obligatoria,

cuando el usuario intente guardar el perfil,

entonces el sistema debe mostrar un mensaje de validación indicando los campos requeridos.

Dado que el perfil sea creado correctamente,

cuando el usuario vuelva a la lista de lavadores,

entonces el nuevo lavador debe aparecer activo por defecto.

**HU-05 – Edición de perfiles de lavadores**

Como administrador global o administrador operativo,

quiero editar la información de un lavador existente,

para que pueda actualizar sus datos personales, porcentaje de bono o estado laboral.

**Criterios de aceptación:**

Dado que el usuario seleccione un lavador registrado,

cuando modifique su información y confirme los cambios,

entonces el sistema debe actualizar el registro sin eliminar su historial de servicios previos.

Dado que el usuario intente guardar sin realizar cambios,

cuando presione el botón “Guardar”,

entonces el sistema debe notificar que no se detectaron modificaciones.

Dado que el porcentaje de bono haya cambiado,

cuando se confirme la edición,

entonces el sistema debe aplicar el nuevo porcentaje en los cálculos de bonos futuros, sin alterar los ya cerrados.

**HU-06 – Eliminación de perfiles de lavadores**

Como administrador global o administrador operativo,

quiero eliminar un perfil de lavador,

para que la base de datos solo conserve empleados activos o válidos.

**Criterios de aceptación:**

Dado que el lavador no tenga servicios activos,

cuando el administrador seleccione la opción “Eliminar” y confirme,

entonces el sistema debe borrar el registro definitivamente.

Dado que el lavador tenga lavados pendientes o activos,

cuando el administrador intente eliminarlo,

entonces el sistema debe impedir la acción y mostrar un mensaje indicando “El lavador tiene servicios en curso”.

Dado que el registro haya sido eliminado correctamente,

cuando el usuario actualice la lista de lavadores,

entonces el perfil eliminado ya no debe aparecer.

**Módulo de Gestión Operativa**  
**Gestión de Parqueadero**

**HU-07 – Registro de ingreso de vehículos al parqueadero**

Como administrador operativo,  
quiero registrar la entrada de un vehículo al parqueadero,  
para que el sistema controle el tiempo de permanencia y posteriormente calcule el valor del servicio.

**Criterios de aceptación:**

Dado que el administrador operativo se encuentra en el módulo “Parqueadero”,  
cuando ingrese la placa de un vehículo,  
entonces el sistema debe registrar automáticamente la hora de ingreso.

Dado que el vehículo es una motocicleta (placa termina en letra),  
cuando se registre su ingreso,  
entonces el sistema debe permitir ingresar el número de cascos y aplicar un recargo fijo de $1.000 COP por cada casco.

Dado que la información fue ingresada correctamente,  
cuando el usuario confirme el registro,  
entonces el sistema debe mostrar un mensaje de confirmación y agregar el vehículo a la lista de parqueados activos.

**HU-08 – Clasificación automática del tipo de vehículo**

Como sistema,  
quiero clasificar automáticamente el tipo de vehículo al registrar su placa,  
para que se apliquen las tarifas y reglas de cobro correspondientes sin intervención manual.

**Criterios de aceptación:**

Dado que el usuario ingresa la placa de un vehículo,  
cuando el sistema la procese,  
entonces debe determinar si pertenece a un vehículo particular, motocicleta o camión, con base en reglas predefinidas.

Dado que la placa ingresada termina en número,  
cuando el sistema la clasifique,  
entonces debe asignar automáticamente el tipo “Automóvil particular”.

Dado que la placa termina en letra,  
cuando se realice la clasificación,  
entonces el sistema debe asignar el tipo “Motocicleta” y habilitar el campo para registrar cascos.

**HU-09 – Modificación manual del tipo de vehículo**

Como administrador operativo,  
quiero modificar manualmente el tipo de vehículo asignado por el sistema,  
para que pueda corregir clasificaciones automáticas erróneas o especificar tipos especiales como camión o bicicleta.

**Criterios de aceptación:**

Dado que el sistema haya clasificado automáticamente el vehículo,  
cuando el administrador operativo cambie el tipo en el campo correspondiente,  
entonces el sistema debe actualizar el registro sin necesidad de repetir el ingreso.

Dado que se modifique el tipo de vehículo,  
cuando el sistema realice el cambio,  
entonces debe actualizar las tarifas asociadas al nuevo tipo.

Dado que el tipo de vehículo haya sido corregido manualmente,  
cuando se genere el reporte de ingresos,  
entonces el sistema debe reflejar correctamente la categoría final utilizada para el cobro.

**HU-10 – Registro de salida de vehículos del parqueadero**

Como administrador operativo,  
quiero registrar la salida de los vehículos del parqueadero,  
para que el sistema calcule automáticamente el valor a pagar según el tiempo de permanencia y el tipo de vehículo.

**Criterios de aceptación:**

Dado que un vehículo se encuentra registrado como activo en el parqueadero,  
cuando el administrador operativo indique su salida,  
entonces el sistema debe calcular el tiempo total de estadía.

Dado que el sistema conoce el tipo de vehículo y la hora de ingreso,  
cuando se calcule el cobro,  
entonces debe aplicar la tarifa correspondiente y sumar el valor de los cascos si corresponde.

Dado que el cobro haya sido realizado,  
cuando se confirme la salida,  
entonces el sistema debe liberar el espacio ocupado y registrar la transacción en el historial de movimientos.

Dado que el vehículo tiene una mensualidad activa,  
cuando se registre su salida,  
entonces el sistema no debe aplicar cobro adicional por parqueo.

**Gestión de Lavado**  
**HU-11 – Registro general de ingreso para lavado**

Como administrador operativo,  
quiero registrar la entrada de un vehículo que solicita el servicio de lavado,  
para que el sistema genere un registro con los datos del vehículo y la hora exacta de ingreso.

**Criterios de aceptación:**

Dado que el administrador operativo se encuentra en el módulo de lavados,  
cuando ingrese la placa del vehículo,  
entonces el sistema debe registrar la hora de ingreso y generar una orden de servicio pendiente.

Dado que el vehículo ya esté registrado en el parqueadero,  
cuando se registre para lavado,  
entonces el sistema debe vincular ambos registros bajo la misma placa.

Dado que el registro se complete,  
cuando el usuario confirme la operación,  
entonces el sistema debe mostrar el estado inicial del servicio como “En espera”.

**HU-12 – Selección del tipo de lavado**

Como administrador operativo,  
quiero seleccionar el tipo de lavado que se realizará al vehículo,  
para que el sistema calcule el valor del servicio con base en las opciones configuradas por el administrador global.

**Criterios de aceptación:**

Dado que el administrador haya registrado el vehículo para lavado,  
cuando seleccione el tipo de servicio (sencillo, general, especial, etc.),  
entonces el sistema debe asignar automáticamente el precio correspondiente según la tabla de tarifas vigente.

Dado que las tarifas hayan cambiado,  
cuando el usuario seleccione un tipo de lavado,  
entonces el sistema debe aplicar las tarifas actuales configuradas por el administrador global.

Dado que se elija un tipo de lavado no disponible,  
cuando se intente confirmar el registro,  
entonces el sistema debe mostrar un mensaje de error indicando “Tipo de lavado no disponible”.

**HU-13 – Asignación de lavador al servicio**

Como administrador operativo,  
quiero asignar un lavador disponible a cada servicio de lavado,  
para que el sistema registre quién realiza el trabajo y permita calcular posteriormente su bono.

**Criterios de aceptación:**

Dado que un servicio de lavado se encuentra en estado “En espera”,  
cuando el administrador seleccione un lavador de la lista de empleados activos,  
entonces el sistema debe vincular la identificación del lavador al servicio.

Dado que un lavador esté ocupado en otro servicio,  
cuando el administrador intente asignarlo nuevamente,  
entonces el sistema debe impedir la acción y mostrar un mensaje “Lavador actualmente en otro servicio”.

Dado que el lavador haya sido asignado correctamente,  
cuando se consulte la orden de servicio,  
entonces debe aparecer su nombre o identificación en el detalle del lavado.

**HU-14 – Registro del servicio con o sin parqueo**

Como administrador operativo,  
quiero registrar si el servicio de lavado incluye parqueo o no,  
para que el sistema calcule correctamente el valor total del servicio y los tiempos asociados.

**Criterios de aceptación:**

Dado que el administrador registre un nuevo servicio de lavado,  
cuando seleccione la opción “Con parqueo” o “Solo lavado”,  
entonces el sistema debe aplicar el cobro y tiempo de servicio correspondiente.

Dado que se elija la opción “Con parqueo”,  
cuando el servicio sea confirmado,  
entonces el sistema debe crear automáticamente un registro de parqueo asociado al mismo vehículo.

Dado que se seleccione “Solo lavado”,  
cuando se confirme la operación,  
entonces el sistema debe omitir la creación del registro de parqueo.

**HU-15 – Gestión del estado del lavado**

Como administrador operativo,  
quiero cambiar el estado de los lavados (En espera, En proceso, Terminado),  
para que el sistema refleje el progreso del servicio en tiempo real.

**Criterios de aceptación:**

Dado que un servicio se encuentra activo,  
cuando el administrador cambie su estado,  
entonces el sistema debe actualizarlo en la interfaz principal y registrar el cambio en la base de datos.

Dado que el servicio cambie a “En proceso”,  
cuando se realice la actualización,  
entonces el sistema debe registrar la hora de inicio automáticamente.

Dado que el servicio cambie a “Terminado”,  
cuando el administrador confirme el estado,  
entonces el sistema debe registrar la hora de finalización y calcular la duración total.

Dado que un estado haya sido modificado por error,  
cuando el administrador lo revierta,  
entonces el sistema debe conservar el historial de cambios y tiempos previos.

**HU-16 – Registro automático de tiempos por cambio de estado**

Como sistema,  
quiero registrar automáticamente las marcas de tiempo (timestamp) en cada cambio de estado,  
para que se pueda medir con precisión la duración total de los lavados.

**Criterios de aceptación:**

Dado que un servicio cambie de estado,  
cuando el sistema registre la acción,  
entonces debe generar una marca de tiempo asociada al nuevo estado.

Dado que el estado pase de “En proceso” a “Terminado”,  
cuando el sistema registre el evento,  
entonces debe calcular automáticamente la diferencia entre ambas marcas de tiempo.

Dado que se consulten los detalles del servicio,  
cuando el usuario visualice la información,  
entonces el sistema debe mostrar las horas exactas de inicio y fin.

**HU-17 – Visualización y análisis de tiempos de lavado**

Como administrador global,  
quiero visualizar y analizar los tiempos promedio de los lavados,  
para que pueda evaluar la eficiencia de los empleados y el rendimiento del negocio.

**Criterios de aceptación:**

Dado que el sistema almacena los registros de tiempo,  
cuando el administrador acceda al módulo de métricas,  
entonces el sistema debe mostrar el tiempo promedio de lavado por tipo de servicio y por empleado.

Dado que el usuario seleccione un rango de fechas,  
cuando se filtren los resultados,  
entonces el sistema debe recalcular las métricas correspondientes al periodo seleccionado.

Dado que se identifiquen lavados con tiempos atípicos (fuera del promedio),  
cuando el usuario revise el reporte,  
entonces el sistema debe resaltarlos visualmente (por ejemplo, con color o ícono de alerta).

**Módulo: Tarifas, Mensualidades y Convenios**

**HU-18 – Registro inicial de mensualidad de vehículo**

Como administrador operativo,  
quiero registrar vehículos bajo la modalidad de mensualidad,  
para que el sistema calcule y asigne automáticamente la tarifa fija mensual correspondiente según el tipo de vehículo.

**Criterios de aceptación:**

Dado que el administrador operativo se encuentra en el módulo de mensualidades,  
cuando ingrese la placa y seleccione el tipo de vehículo,  
entonces el sistema debe mostrar la tarifa mensual vigente según la política configurada.

Dado que el usuario confirme el registro,  
cuando se complete el proceso,  
entonces el sistema debe crear una mensualidad activa con fecha de inicio y fin automática (30 días por defecto).

**HU-19 – Renovación de mensualidad**

Como administrador operativo,  
quiero renovar mensualidades vencidas o próximas a vencer,  
para que el sistema actualice el periodo de vigencia y aplique el valor actualizado según la tarifa vigente.

**Criterios de aceptación:**

Dado que una mensualidad se encuentre vencida o próxima a vencer,  
cuando el usuario seleccione la opción “Renovar”,  
entonces el sistema debe extender la fecha de vigencia por un nuevo periodo (30 días).

Dado que haya una nueva estructura tarifaria,  
cuando se ejecute la renovación,  
entonces el sistema debe aplicar el precio vigente al momento de la renovación.

**HU-20 – Alerta de vencimiento de mensualidades**

Como sistema,  
quiero generar alertas automáticas para mensualidades próximas a vencer,  
para que los administradores puedan anticiparse y gestionar su renovación o cancelación.

**Criterios de aceptación:**

Dado que una mensualidad tenga menos de 5 días de vigencia,  
cuando el sistema procese la información diaria,  
entonces debe generar una alerta visible en el panel de mensualidades.

Dado que la mensualidad ya haya vencido,  
cuando el usuario consulte la lista,  
entonces el sistema debe marcarla como “Expirada” y ofrecer la opción “Renovar”.

**HU-21 – Definición inicial de precios base**

Como administrador global,  
quiero definir los precios base de cada tipo de vehículo y tipo de lavado,  
para que el sistema establezca la estructura tarifaria inicial del negocio.

**Criterios de aceptación:**

Dado que el administrador global acceda al módulo de tarifas,  
cuando ingrese los precios base,  
entonces el sistema debe guardarlos como referencia inicial.

Dado que los precios sean modificados en el futuro,  
cuando se consulten registros históricos,  
entonces el sistema debe mantener los valores originales de las transacciones pasadas.

**HU-22 – Modificación manual de precios**

Como administrador global,  
quiero modificar manualmente los precios de los servicios,  
para que pueda actualizar las tarifas sin alterar los registros históricos.

**Criterios de aceptación:**

Dado que un precio base necesite ser ajustado,  
cuando el administrador ingrese un nuevo valor,  
entonces el sistema debe aplicarlo a todas las nuevas operaciones a partir de esa fecha.

Dado que se consulten reportes antiguos,  
cuando se revisen los datos,  
entonces el sistema debe mantener los precios previos en esas transacciones.

**HU-23 – Ajuste dinámico de precios por inflación o reajuste**

Como administrador global,  
quiero aplicar ajustes porcentuales globales a los precios de los servicios,  
para que el sistema actualice automáticamente los valores según la inflación o políticas del negocio.

**Criterios de aceptación:**

Dado que el administrador acceda al módulo de ajustes,  
cuando ingrese un porcentaje de aumento o reducción,  
entonces el sistema debe recalcular automáticamente todos los precios vigentes.

Dado que el ajuste sea confirmado,  
cuando el sistema guarde el cambio,  
entonces debe registrar la fecha, el porcentaje aplicado y el usuario que realizó la modificación.

**HU-24 – Registro de empresas con convenio**

Como administrador global,  
quiero registrar empresas con convenio,  
para que el sistema las identifique y gestione sus beneficios especiales.

**Criterios de aceptación:**

Dado que el administrador global se encuentre en el módulo de convenios,  
cuando registre una nueva empresa con nombre, NIT y contacto,  
entonces el sistema debe crear un perfil de convenio activo.

Dado que ya exista una empresa registrada,  
cuando se intente crear otra con el mismo NIT,  
entonces el sistema debe rechazar el registro y notificar al usuario.

**HU-25 – Configuración de condiciones y descuentos del convenio**

Como administrador global,  
quiero definir los porcentajes de descuento y los tipos de servicio a los cuales aplica,  
para que las empresas con convenio reciban los beneficios establecidos.

**Criterios de aceptación:**

Dado que una empresa tenga un convenio activo,  
cuando el administrador configure los descuentos,  
entonces el sistema debe aplicarlos automáticamente al registrar servicios asociados.

Dado que un servicio no esté incluido en el convenio,  
cuando se registre un lavado o parqueo,  
entonces el sistema debe aplicar el precio estándar.

**HU-26 – Importación masiva de flotas asociadas al convenio**

Como administrador global,  
quiero importar una lista de vehículos asociados a una empresa mediante archivo Excel o CSV,  
para que el sistema vincule automáticamente las placas a la empresa correspondiente.

**Criterios de aceptación:**

Dado que el usuario seleccione un archivo compatible,  
cuando cargue la información,  
entonces el sistema debe validar el formato y registrar las placas en la base de datos.

Dado que existan errores en el archivo,  
cuando se procese la importación,  
entonces el sistema debe generar un reporte de errores indicando las filas con formato incorrecto o duplicados.

Dado que el usuario prefiera ingresar los vehículos manualmente,  
cuando elija la opción “Agregar individualmente”,  
entonces el sistema debe permitir el registro de cada placa con los mismos campos requeridos.

**HU-27 – Registro de lavados asociados a convenios**

Como administrador operativo,  
quiero registrar lavados realizados a vehículos de empresas con convenio,  
para que el sistema aplique automáticamente los descuentos establecidos.

**Criterios de aceptación:**

Dado que un vehículo pertenezca a una empresa con convenio,  
cuando se registre un lavado,  
entonces el sistema debe aplicar la tarifa reducida configurada.

Dado que el vehículo no esté asociado a ningún convenio,  
cuando se registre su lavado,  
entonces el sistema debe aplicar la tarifa normal del servicio.

**HU-28 – Generación automática de reportes por empresa con convenio**

Como administrador global,  
quiero generar reportes automáticos de lavados por empresa,  
para que pueda visualizar el total de servicios y montos facturados en un periodo determinado.

**Criterios de aceptación:**

Dado que existan lavados asociados a una empresa,  
cuando se genere el reporte mensual,  
entonces el sistema debe mostrar la cantidad de lavados, total facturado y fecha del servicio.

Dado que el usuario seleccione un rango de fechas,  
cuando ejecute el filtro,  
entonces el sistema debe actualizar el reporte con la información correspondiente.

**HU-29 – Exportación de reportes de convenios**

Como administrador global,  
quiero exportar los reportes de convenios a formatos CSV o PDF,  
para que pueda compartirlos o integrarlos con herramientas contables externas.

**Criterios de aceptación:**

Dado que un reporte haya sido generado,  
cuando el usuario seleccione la opción de exportar,  
entonces el sistema debe permitir elegir entre formato CSV o PDF.

Dado que se exporte un reporte,  
cuando el proceso finalice,  
entonces el sistema debe mostrar una notificación de éxito y guardar una copia del archivo generado.

**Módulo: Control Financiero y Administrativo**  
**HU-30 – Asignación de lavadores a servicios**

Como administrador operativo,  
quiero asignar un lavador disponible a cada servicio de lavado,  
para que pueda controlar qué empleado realiza cada tarea y calcular sus bonos correctamente.

**Criterios de aceptación:**

Dado que un servicio de lavado esté registrado,  
cuando el administrador seleccione un lavador,  
entonces el sistema debe vincularlo al servicio y registrar la asignación.

Dado que un lavador esté ocupado,  
cuando el sistema detecte que ya tiene un servicio en proceso,  
entonces debe impedir su asignación a otro vehículo.

**HU-31 – Actualización del estado de los lavados**

Como administrador operativo,  
quiero actualizar el estado de los lavados entre En espera, En proceso y Terminado,  
para que el sistema registre el progreso del servicio y calcule los tiempos automáticamente.

**Criterios de aceptación:**

Dado que un vehículo esté en la lista de servicios,  
cuando el administrador cambie su estado,  
entonces el sistema debe guardar el nuevo estado con su respectiva marca de tiempo.

Dado que el servicio llegue a “Terminado”,  
cuando el cambio de estado se registre,  
entonces el sistema debe calcular la duración total del lavado.

**HU-32 – Registro de gastos del turno**

Como administrador operativo,  
quiero registrar los gastos asociados a mi turno,  
para que se reflejen correctamente en el balance financiero del día.

**Criterios de aceptación:**

Dado que el usuario esté en el módulo de gastos,  
cuando registre un gasto con su tipo, descripción y valor,  
entonces el sistema debe guardarlo con la fecha y turno correspondiente.

**HU-33 – Consolidación de datos entre turnos**

Como sistema,  
quiero mantener la continuidad de los registros de vehículos entre turnos,  
para que los vehículos que permanezcan en lavado o parqueo sigan visibles al iniciar un nuevo turno.

**Criterios de aceptación:**

Dado que un vehículo no haya sido dado de salida,  
cuando el nuevo turno inicie,  
entonces el sistema debe mantenerlo en el listado activo del nuevo administrador.

**HU-34 – Generación automática de reporte de turno**

Como administrador operativo,  
quiero que el sistema genere un reporte automático al finalizar mi turno,  
para que se consolide la información de ingresos, gastos y lavados realizados.

**Criterios de aceptación:**

Dado que el turno finalice,  
cuando el sistema detecte el cierre de sesión o fin de jornada,  
entonces debe generar un reporte con los indicadores del turno.

Dado que se consulte el historial de turnos,  
cuando se seleccione una fecha,  
entonces el sistema debe mostrar el reporte asociado a esa jornada.

**HU-35 – Registro de gastos diarios**

Como administrador global o operativo,  
quiero registrar gastos realizados durante el día,  
para que se mantenga un control financiero actualizado y categorizado.

**Criterios de aceptación:**

Dado que el usuario registre un gasto,  
cuando complete los campos de categoría, descripción, valor y fecha,  
entonces el sistema debe almacenarlo correctamente.

**HU-36 – Visualización consolidada de gastos**

Como administrador global,  
quiero visualizar un resumen consolidado de los gastos,  
para que pueda analizar los costos por turno, fecha o categoría.

**Criterios de aceptación:**

Dado que existan registros de gastos,  
cuando el usuario aplique un filtro por fecha o categoría,  
entonces el sistema debe mostrar los totales y su comportamiento en el tiempo.

**HU-37 – Eliminación de gastos**

Como administrador global,  
quiero eliminar gastos registrados,  
para que pueda corregir errores o eliminar registros no válidos.

**Criterios de aceptación:**

Dado que un gasto esté en la lista,  
cuando el administrador seleccione la opción “Eliminar”,  
entonces el sistema debe solicitar confirmación y eliminarlo permanentemente.

**HU-38 – Registro de vales de empleados**

Como administrador global u operativo,  
quiero registrar vales otorgados a los empleados,  
para que el sistema lleve el control de los montos prestados y sus fechas de descuento.

**Criterios de aceptación:**

Dado que el usuario registre un vale,  
cuando ingrese el monto, la fecha y el número de cuotas,  
entonces el sistema debe asociarlo al empleado y calcular el plan de descuento.

**HU-39 – Aplicación automática de descuentos por vales**

Como sistema,  
quiero descontar automáticamente del bono de cada empleado el valor de las cuotas de los vales,  
para que los cálculos de pago sean precisos y automáticos.

**Criterios de aceptación:**

Dado que un vale esté activo,  
cuando se procese el cálculo de bonos,  
entonces el sistema debe restar el valor de la cuota correspondiente.

**HU-40 – Configuración de tiempos de parqueo gratuito**

Como administrador global,  
quiero configurar los tiempos de parqueo gratuito por tipo de lavado,  
para que el sistema pueda calcular los cobros de forma automática y justa.

**Criterios de aceptación:**

Dado que el administrador configure la tabla de tiempos,  
cuando asocie minutos gratuitos a cada tipo de lavado,  
entonces el sistema debe guardar y aplicar esa configuración a nuevos registros.

**HU-41 – Cálculo dinámico del tiempo de parqueo gratuito**

Como sistema,  
quiero calcular dinámicamente el tiempo de parqueo gratuito según el tipo de lavado y la cola de espera,  
para que los cobros sean coherentes con las condiciones operativas.

**Criterios de aceptación:**

Dado que un vehículo esté en lavado,  
cuando el sistema identifique su tipo de servicio y posición en cola,  
entonces debe calcular el tiempo total de parqueo gratuito aplicable.

**HU-42 – Exención manual del cobro de parqueo**

Como administrador global u operativo,  
quiero poder eximir manualmente el cobro de parqueo de un vehículo,  
para que pueda gestionar casos especiales o cortesías.

**Criterios de aceptación:**

Dado que un vehículo esté registrado,  
cuando el usuario seleccione la opción de exención,  
entonces el sistema debe registrar el motivo y marcar el cobro como “Exento”.

**HU-43 – Cálculo automático de bonos diarios**

Como sistema,  
quiero calcular automáticamente los bonos diarios de los lavadores,  
para que los incentivos se generen con base en el valor de los lavados realizados.

**Criterios de aceptación:**

Dado que existan registros de lavados,  
cuando el sistema procese el día,  
entonces debe calcular el porcentaje de bono diario para cada lavador.

**HU-44 – Acumulación y visualización mensual de bonos**

Como administrador global,  
quiero visualizar el total mensual de bonos por lavador,  
para que pueda evaluar el rendimiento y los incentivos acumulados.

**Criterios de aceptación:**

Dado que existan bonos diarios,  
cuando se consulte el panel de métricas,  
entonces el sistema debe mostrar la suma total mensual por empleado.

**HU-45 – Configuración y ajuste de porcentajes de bono**

Como administrador global,  
quiero configurar los porcentajes de bono generales o individuales,  
para que los cálculos se adapten a las políticas de la empresa.

**Criterios de aceptación:**

Dado que el administrador acceda al módulo de configuración,  
cuando modifique el porcentaje,  
entonces el sistema debe aplicarlo a todos los lavadores o solo al seleccionado según el caso.  
**HU-46 – Consolidado de ingresos**

Como administrador global,  
quiero visualizar un consolidado de ingresos agrupado por día, semana o mes,  
para que pueda analizar el flujo financiero del negocio.

**Criterios de aceptación:**

Dado que existan registros de cobros,  
cuando se seleccione un rango de fechas,  
entonces el sistema debe mostrar los ingresos totales agrupados según la selección.

**HU-47 – Visualización gráfica de ingresos**

Como administrador global,  
quiero visualizar los ingresos en gráficos de líneas o barras,  
para que pueda interpretar fácilmente las tendencias del negocio.

**Criterios de aceptación:**

Dado que existan datos de ingresos,  
cuando se genere la vista gráfica,  
entonces el sistema debe representar los valores diarios en el eje Y y las fechas en el eje X.

**HU-48 – Cálculo de rendimiento operativo**

Como sistema,  
quiero calcular el rendimiento del negocio mediante la fórmula  
R \= Ingresos – (Gastos \+ Bonos),  
para que el administrador conozca la rentabilidad del periodo.

**Criterios de aceptación:**

Dado que existan registros de ingresos, gastos y bonos,  
cuando se seleccione un rango de fechas,  
entonces el sistema debe mostrar el rendimiento calculado.

**HU-49 – Visualización del rendimiento en tiempo real**

Como administrador global,  
quiero visualizar el rendimiento en tiempo real dentro del dashboard,  
para que pueda monitorear el desempeño financiero del negocio.

**Criterios de aceptación:**

Dado que el sistema actualice los ingresos o gastos,  
cuando se modifique cualquier registro financiero,  
entonces el indicador de rendimiento debe actualizarse automáticamente con la nueva información.

**Módulo : Analítica y Dashboard**  
**HU-50 – Generación automática del reporte gráfico de actividad**

Como administrador global,  
quiero que el sistema genere automáticamente un reporte gráfico con los días de mayor actividad de lavado,  
para que pueda analizar la demanda de servicios y planificar recursos de forma eficiente.

**Criterios de aceptación:**

Dado que existan registros de lavados,  
cuando el sistema procese los datos del periodo,  
entonces debe generar un gráfico que muestre la cantidad y valor total de servicios por día.

Dado que el gráfico se genere automáticamente,  
cuando el usuario acceda al módulo de análisis,  
entonces el reporte debe estar disponible sin necesidad de acciones manuales.

**HU-51 – Configuración del eje temporal y tipo de visualización**

Como administrador global o operativo,  
quiero configurar el eje temporal (por días, semanas o meses) y el tipo de gráfico (líneas o barras),  
para que la visualización del reporte se adapte a mis necesidades analíticas.

**Criterios de aceptación:**

Dado que el usuario esté en el panel de análisis,  
cuando seleccione un tipo de eje temporal o gráfico,  
entonces el sistema debe actualizar la visualización con esa configuración.

**HU-52 – Filtrado y comparación de actividad por empresa**

Como administrador global,  
quiero filtrar y comparar la actividad de lavado por empresa con convenio,  
para que pueda evaluar el comportamiento y nivel de consumo de cada cliente empresarial.

**Criterios de aceptación:**

Dado que existan datos de lavados asociados a empresas,  
cuando el usuario seleccione una o más empresas,  
entonces el sistema debe mostrar un gráfico comparativo de actividad entre ellas.

**HU-53 – Registro de datos de ocupación del parqueadero**

Como sistema,  
quiero registrar automáticamente el número de vehículos en el parqueadero,  
para que se puedan generar métricas de ocupación y reportes históricos.

**Criterios de aceptación:**

Dado que vehículos ingresan o salen del parqueadero,  
cuando el evento ocurra,  
entonces el sistema debe actualizar el contador de ocupación en tiempo real.

**HU-54 – Generación del gráfico comparativo de ocupación**

Como administrador global,  
quiero visualizar un gráfico que muestre la evolución del nivel de ocupación del parqueadero,  
para que pueda analizar tendencias de uso por día o semana.

**Criterios de aceptación:**

Dado que existan registros de ocupación,  
cuando el sistema genere el reporte,  
entonces debe mostrar un gráfico comparando los niveles de ocupación a lo largo del tiempo.

**HU-55 – Cálculo de métricas promedio operativas**

Como sistema,  
quiero calcular automáticamente los indicadores clave del negocio,  
para que se puedan evaluar el rendimiento y la eficiencia operativa del lavadero y parqueadero.

**Criterios de aceptación:**

Dado que existan registros históricos,  
cuando se ejecute el cálculo,  
entonces el sistema debe generar los siguientes indicadores:

Nivel promedio de ocupación del parqueadero.

Ingresos totales y promedio diario.

Gastos totales y promedio diario.

Comparativo de ingresos vs. gastos.

Rendimiento neto (Ingresos – Gastos – Bonos).

Promedio de tiempo de lavado por tipo de servicio.

Porcentaje de utilización del personal (lavadores activos/total).

**HU-56 – Panel de análisis de métricas del negocio**

Como administrador global,  
quiero acceder a un panel de análisis con las métricas del negocio organizadas por categorías,  
para que pueda consultar rápidamente los indicadores clave de desempeño.

**Criterios de aceptación:**

Dado que existan métricas operativas,  
cuando el usuario abra el dashboard,  
entonces el sistema debe mostrar secciones de: Generales, Lavados, Gastos y Lavadores.

Dado que el usuario seleccione un periodo o tipo de servicio,  
cuando aplique filtros,  
entonces el panel debe actualizar las métricas correspondientes.

**HU-57 – Visualización de métricas operativas generales**

Como administrador global,  
quiero ver una tabla resumen con los indicadores generales del negocio,  
para que pueda obtener una visión rápida del desempeño global.

**Criterios de aceptación:**

Dado que existan registros financieros,  
cuando se cargue la vista “Métricas generales”,  
entonces el sistema debe mostrar los valores de ingresos por lavado, ingresos por parqueo, gastos y bonos totales.

Dado que los datos estén cargados,  
cuando se visualice el gráfico inferior,  
entonces debe mostrar las curvas de ingresos y gastos a lo largo del periodo.

**HU-58 – Visualización de métricas por día (resumen general diario)**

Como administrador global,  
quiero ver una tabla con los ingresos, gastos y bonos desglosados por día,  
para que pueda analizar el comportamiento financiero detallado.

**Criterios de aceptación:**

Dado que se seleccione un periodo,  
cuando se genere el reporte diario,  
entonces el sistema debe mostrar una tabla con columnas por día y una fila de totales acumulados.

**HU-59 – Métricas específicas de lavado**

Como administrador global,  
quiero ver métricas detalladas de los servicios de lavado,  
para que pueda conocer el rendimiento diario de este servicio.

**Criterios de aceptación:**

Dado que se acceda a la sección “Lavados”,  
cuando se muestre la información,  
entonces el sistema debe desplegar una tabla con ingresos por día y un gráfico con el comportamiento diario del valor recaudado.

**HU-60 – Métricas de gastos**

Como administrador global,  
quiero visualizar métricas de gastos en tablas y gráficos,  
para que pueda identificar tendencias de consumo o costos anómalos.

**Criterios de aceptación:**

Dado que existan registros de gastos,  
cuando se genere la vista de métricas,  
entonces el sistema debe mostrar una tabla con totales diarios y un gráfico comparativo de evolución.

**HU-61 – Visualización general de métricas por lavador**

Como administrador global,  
quiero ver una tabla con todos los lavadores registrados y sus métricas acumuladas,  
para que pueda comparar el rendimiento entre empleados.

**Criterios de aceptación:**

Dado que existan datos de lavadores,  
cuando se consulte el módulo “Lavadores”,  
entonces el sistema debe mostrar una tabla con bonos, vales, lavados realizados y totales del periodo.

**HU-62 – Visualización detallada por lavador**

Como administrador global,  
quiero abrir la vista individual de un lavador específico,  
para que pueda ver su rendimiento diario, vales y comisiones.

**Criterios de aceptación:**

Dado que se seleccione un lavador,  
cuando se abra su perfil,  
entonces el sistema debe mostrar una tabla con lavados por día, bono aplicado y vales registrados.

Dado que sea necesario un ajuste,  
cuando el administrador edite los porcentajes o registros,  
entonces el sistema debe guardar las modificaciones.

**HU-63 – Vista detallada por día (verificación de lavados por vehículo)**

Como administrador global o supervisor,  
quiero revisar la lista de vehículos lavados por un empleado en un día específico,  
para que pueda verificar la correcta asignación de servicios y corregir errores.

### 8. Módulos del sistema

A continuación se definen los módulos funcionales del sistema. Cada módulo indica su objetivo y las funcionalidades principales. Esta organización servirá tanto para estructurar el código (carpetas / bounded contexts) como para planificar el trabajo entre los 4 desarrolladores.

1. Autenticación y Gestión de Usuarios
  - Objetivo: Control seguro de acceso y administración de perfiles (global admin, operativo, lavador).
  - Funcionalidades: Login (JWT + refresh), recuperación de contraseña, creación/edición/eliminación de lavadores, gestión de roles y permisos.
2. Parqueadero (Parking)
  - Objetivo: Registrar entradas/salidas y calcular permanencia/cobros.
  - Funcionalidades: Registro de ingreso, salida, cálculo de tarifa, recargo de cascos, exención manual de cobro, ocupación en tiempo real.
3. Lavados (Car Wash)
  - Objetivo: Gestionar el ciclo completo del servicio de lavado.
  - Funcionalidades: Registro de orden, selección de tipo de lavado, asignación de lavador, estados (En espera, En proceso, Terminado), timestamps automáticos, cálculo de precio final.
4. Tarifas, Ajustes y Configuración Comercial
  - Objetivo: Mantener la estructura tarifaria y reglas globales (tiempos de parqueo gratuito, ajustes inflacionarios, porcentajes de bono base).
  - Funcionalidades: CRUD de precios base, ajustes porcentuales masivos, configuración de tiempos gratuitos, actualización de políticas.
5. Mensualidades (Subscriptions)
  - Objetivo: Gestionar planes de pago mensuales vinculados a vehículos.
  - Funcionalidades: Registro inicial, renovación, alertas de vencimiento, cálculo automático de vigencia.
6. Convenios y Flotas
  - Objetivo: Administrar empresas con acuerdos especiales y sus vehículos.
  - Funcionalidades: Registro de empresa, configuración de descuentos, importación masiva (CSV/Excel), reporte por empresa.
7. Control Financiero y Administrativo
  - Objetivo: Centralizar gastos, vales, bonos y consolidar ingresos por turno.
  - Funcionalidades: Registro de gastos, clasificación por tipo, eliminación/edición, reporte de turno automático, registro y aplicación de vales, cálculo de bonos diarios y acumulados.
8. Bonos y Vales (Incentivos)
  - Objetivo: Calcular incentivos económicos y gestionar descuentos por vales sobre bonos.
  - Funcionalidades: Cálculo de bono diario/mensual (estrategia de porcentaje), aplicación automática de descuentos de vales, ajustes manuales.
9. Reportes y Analítica
  - Objetivo: Proveer indicadores clave (ingresos, gastos, rendimiento, tiempos de lavado, ocupación) y visualizaciones.
  - Funcionalidades: Generación de métricas agregadas, gráficos comparativos, exportación CSV/PDF, detección de valores atípicos.
10. Dashboard y Métricas Operativas
   - Objetivo: Visualización consolidada en tiempo real para admins globales.
   - Funcionalidades: Panel general, filtros por periodo, actualización en vivo de rendimiento, comparativos ingresos vs. gastos.
11. Integración y Exportación
   - Objetivo: Facilitar interoperabilidad y salida de datos.
   - Funcionalidades: Exportación CSV/PDF, importación de flotas, generación de archivos históricos.
12. Infraestructura y Servicios Técnicos
   - Objetivo: Capas técnicas transversales (persistencia, email, logging, backups).
   - Funcionalidades: Repositorios, servicios externos (Email), tareas programadas (alertas, backups), proxy y certificados.

Asignación sugerida inicial (para independencia entre 4 desarrolladores):
- Dev A: Autenticación/Usuarios + Seguridad.
- Dev B: Parqueadero + Lavados + Bonos/Vales.
- Dev C: Tarifas/Ajustes + Mensualidades + Convenios/Flotas.
- Dev D: Financiero/Admin (Gastos, Turnos) + Reportes/Analítica + Dashboard.

Infraestructura y refactors cruzados: rotación quincenal o apoyo puntual; code owners por carpeta.

### 9. Reglas de negocio

Reglas clave (invariantes y políticas) que se deben reflejar en el dominio:
1. Clasificación de vehículo: Si la placa termina en letra → tipo motocicleta; puede ajustarse manualmente. La tarifa final siempre corresponde al tipo vigente en el momento del cálculo.
2. Permanencia de parqueadero: El cobro se calcula como a. Tiempo total por la tarifa de parqueo por minutos según aplique (Parking only service) o b. (tiempo total - minutos gratuitos por tipo de lavado) sin redondeo, desde el minuto exacto. La tarifa mínima es de 1 minuto. Después de descontar los minutos gratuitos, el cálculo comienza desde cero (como si fuera el minuto 0). Estructura: 0-6h tarifa/minuto, 6-12h tarifa plana fija. 
3. Cascos en motocicletas: Cada casco genera un recargo fijo configurado. Solo se aplica si el tipo de vehículo es motocicleta y cascos > 0.
4. Mensualidad activa: Vehículo con mensualidad vigente no paga parqueo estándar; sí paga servicios de lavado (la mensualidad cubre SOLO parqueo). Aplica para cualquier tipo de vehículo: carro ($170.000), moto ($70.000), camión ($300.000), bicicleta ($40.000).
5. Vencimiento de mensualidades: Al quedar ≤ 5 días se genera alerta; al vencerse se marca “Expirada” sin permitir uso de beneficios.
6. Convenios (empresas): Descuento se aplica solo a servicios marcados y vehículos asociados; si no coincide, se usa tarifa estándar. Modelo de cobro: postpago (factura mensual), con opción de pago inmediato por el cliente si lo requiere. El administrador global debe aprobar los lavados antes de facturarlos a la empresa.
7. Ajustes globales de tarifas: Un ajuste porcentual afecta únicamente precios futuros; nunca modifica históricos ya registrados.
8. Estados de lavado: Transiciones válidas: WAITING → IN_PROCESS → FINISHED; registrar timestamps en cada transición; FINISHED bloquea cambios posteriores salvo corrección auditada.
9. Cálculo de bono: Bono diario = Σ (valor neto lavado * porcentaje lavador), donde valor neto es después de descuentos de convenio. Si un lavado se anula después de cerrar el día, se resta del bono acumulado del lavador. Descuentos de vales se aplican mensualmente al final del periodo, con opción de configurar pago en número específico de quincenas.
10. Vales: Cada cuota se descuenta del bono en el periodo correspondiente; si el bono no cubre la cuota completa se registra saldo pendiente.
11. Rendimiento operativo: R = Ingresos – (Gastos + Bonos). Se recalcula ante cualquier modificación de gasto, bono o servicio.
12. Seguridad de cambios y auditoría: Cambios manuales de tipo de vehículo, descuento aplicado o exención de parqueo deben registrar: usuario que realiza el cambio, timestamp, y motivo/razón obligatorio. No hay límite de exenciones por turno/día. Las exenciones generan notificación automática al administrador global (sistema de notificaciones no intrusivo).
13. Tiempo de parqueo gratuito: Solo se asigna si el lavado está activo o en espera; al terminar el lavado no se añaden nuevos minutos gratuitos.
14. Integridad de turnos: Un turno cerrado no acepta nuevos gastos ni servicios; correcciones requieren permiso de administrador global. Los turnos se gestionan por login/logout manual. Solo puede haber un administrador operativo logueado a la vez; si un operativo no cierra turno, el siguiente puede desloguearlo y loguearse. Solo un administrador global puede estar logueado simultáneamente para evitar errores.
15. Persistencia de histórico: Tarifas, porcentajes de bono y descuentos se almacenan con vigencia efectiva para auditoría.
16. Tipos de lavados: Los tipos de lavados son los siguientes: 
      Carro (vehículo liviano):
      -Lavado general ($18.000 - 30 min gratis)
      -Lavado con cera ($28.000 - 45 min gratis)
      -Lavado interior / aspirado + superficies ($22.000 - 45 min gratis)
      -Lavado de motor ($20.000 - 30 min gratis)
      -Polishado / pulido de carrocería ($80.000 - 60 min gratis)
      Camión:
      -Lavado general exterior ($40.000 - 45 min gratis)
      -Lavado de cabina interior ($30.000 - 45 min gratis)
      -Lavado de chasis ($35.000 - 30 min gratis)
      -Lavado de motor ($30.000 - 30 min gratis)
      -Polishado de cabina ($120.000 - 60 min gratis)
      Motocicleta:
      -Lavado general ($10.000 - 20 min gratis)
      -Lavado y desengrasado de cadena ($18.000 - 30 min gratis)
      -Lavado de motor ($14.000 - 20 min gratis)
      -Polishado / pintura y partes metálicas ($40.000 - 45 min gratis)
      Bicicleta:
      -Lavado general ($8.000 - 15 min gratis)
      -Lavado completo + cadena ($12.000 - 20 min gratis)
      -Polishado ($25.000 - 30 min gratis)

Estas reglas se implementarán mediante entidades, value objects (Money, Percentage, TimeWindow), servicios de dominio (PricingService, BonusCalculator) y políticas configurables persistidas (BusinessConfig).

### 10. Glosario del dominio (muy importante para DDD)

Término | Definición
------- | ----------
Vehículo | Unidad física identificada por placa (puede tener mensualidad o convenio).
Servicio de Lavado (Service) | Proceso de limpieza asociado a un vehículo, con tipo de lavado y estados.
Tipo de Lavado (WashType) | Categoría con precio base y minutos de parqueo gratuito incluidos.
Registro de Parqueo (ParkingRecord) | Entrada/salida de vehículo, con cálculo de tarifa y posibles exenciones.
Mensualidad (MonthlySubscription) | Plan fijo de parqueo por periodo (30 días) vinculado a un vehículo.
Convenio (CompanyAgreement) | Acuerdo empresarial que otorga descuentos a vehículos afiliados.
Flota (Fleet) | Conjunto de vehículos asociados a un convenio.
Turno (Shift) | Intervalo operativo gestionado por un administrador (ingresos/gastos/servicios).
Gasto (Expense) | Salida de dinero categorizada que afecta rendimiento.
Vale (Voucher) | Anticipo o préstamo al empleado que se descuenta de bonos.
Bono (Bonus) | Incentivo calculado como porcentaje del valor de lavados realizados por un lavador.
Rendimiento Operativo | Métrica financiera: Ingresos – (Gastos + Bonos).
Ocupación | Número de vehículos presentes simultáneamente en parqueadero.
Porcentaje de bono | Valor configurable aplicado al total de servicios lavados por empleado.
Configuración de Negocio (BusinessConfig) | Conjunto de parámetros globales (bono base, tiempos gratuitos, políticas).
Descuento de Convenio | Reducción porcentual aplicada a servicios de vehículos asociados.
Tiempo Gratuito | Minutos de parqueo sin costo otorgados por tipo de lavado.
Exención | Acción manual que marca un cobro como no aplicable.

## III. MODELO DE DOMINIO (DDD)
### 11. Contextos delimitados (Bounded Contexts)

Definimos bounded contexts para aislar modelos y evitar ambigüedades:

1. Parking Context
  - Objetivo: Gestionar entradas, salidas, ocupación y cobros del parqueadero.
  - Límites: No calcula lavados; consume minutos gratuitos definidos por WashType.
  - Responsabilidades: Registro de parking, cálculo de tarifa, exenciones.
  - Eventos: VehicleEntered, VehicleExited, ParkingFeeCalculated.
2. Car Wash Context
  - Objetivo: Gestionar ciclo de lavado y asignación de lavadores.
  - Límites: No maneja tarifas base globales; usa datos del Pricing Context.
  - Responsabilidades: Estados, timestamps, cálculo del valor base de lavado.
  - Eventos: WashRegistered, WashStarted, WashFinished.
3. Pricing & Configuration Context
  - Objetivo: Centralizar tarifas, ajustes, tiempos gratuitos y porcentajes de bono base.
  - Límites: No registra operaciones; sólo provee valores y políticas.
  - Eventos: RatesUpdated, GlobalAdjustmentApplied, BonusPolicyChanged.
4. Subscription & Agreements Context
  - Objetivo: Mensualidades y convenios empresariales.
  - Límites: No procesa cobros; expone beneficios/estado aplicables.
  - Eventos: SubscriptionCreated, SubscriptionExpired, AgreementDiscountApplied.
5. Financial & Incentives Context
  - Objetivo: Gastos, bonos, vales y rendimiento operativo.
  - Límites: No define precios; consume datos de Pricing y operaciones de Parking/Lavados.
  - Eventos: ExpenseRecorded, BonusCalculated, VoucherInstallmentApplied.
6. Reporting & Analytics Context
  - Objetivo: Agregaciones, métricas y exportaciones.
  - Límites: No modifica estado de negocio; lee proyecciones.
  - Eventos (consumidos): WashFinished, VehicleExited, BonusCalculated, ExpenseRecorded.

Interacción: Contextos emiten eventos de dominio que otros consumen para cálculos diferidos (ej. BonusCalculated alimenta Reporting).

### 12. Modelo conceptual

El diagrama conceptual (ya generado externamente) se respalda con esta descripción textual:
- Entidades principales: Vehicle, Service (Wash), ParkingRecord, MonthlySubscription, CompanyAgreement, Fleet, Shift, Expense, Voucher, Bonus, Rate, BusinessConfig, Report.
- Value Objects sugeridos: Money (monto + moneda), Percentage (0–100), TimeRange (start/end), Plate (validación de formato), HelmetCount, Duration.
- Agregados:
  - Vehicle (con MonthlySubscription opcional).
  - Service (referencias a Vehicle y Washer; controla transición de estados).
  - ParkingRecord (ligado a Vehicle; maneja cálculo de tarifa).
  - Shift (agrupa Expense, Service y ParkingRecord para reportes).
  - CompanyAgreement (agrupa Fleet y reglas de descuento).
- Servicios de dominio: PricingService (cálculo consolidado lavado + parqueo), BonusCalculator, DiscountPolicyService.
- Eventos de dominio: WashFinished, ParkingFeeCalculated, BonusCalculated, SubscriptionExpired, RatesUpdated.
- Invariantes clave: Un Service no puede estar simultáneamente IN_PROCESS y FINISHED. Un ParkingRecord sólo se cierra una vez. Una MonthlySubscription activa cubre parqueo sin cobro adicional.
- Relaciones: Vehicle → Service/ ParkingRecord; Service → Washer/OperationalAdmin; Shift agrega operaciones del periodo; Fleet agrupa Vehicles.

### 13. Aplicación y CQRS (si aplica)

Se adopta inicialmente un modelo **simplificado** (CRUD + casos de uso) sin CQRS estricto para acelerar entrega.
- Casos de uso (Application Services): RegisterVehicleEntry, RegisterWash, AssignWasher, FinishWash, CalculateParkingFee, GenerateShiftReport, ApplyRateAdjustment, RenewSubscription, RecordExpense, CalculateDailyBonuses.
- Comandos: Representados implícitamente por DTOs de entrada (ej. FinishWashCommand).
- Queries: Servicios de lectura especializados (DashboardQueries, ReportingQueries) optimizados con índices y vistas.
- Evolución: CQRS parcial podrá introducirse si la carga de lectura/reportes exige proyecciones desvinculadas (read models). Eventos de dominio ya previstos facilitan esta transición.

## IV. ARQUITECTURA DEL SISTEMA
### 14. Visión general de arquitectura

Arquitectura elegida: **DDD + Hexagonal (Ports & Adapters) sobre capas limpias monolíticas**.

Capas:
1. Dominio: Entidades, Value Objects, Servicios de dominio, Eventos.
2. Aplicación: Casos de uso, orquestación, transacciones, validaciones simples.
3. Infraestructura: Repositorios concretos (SQLAlchemy), proveedores externos (Email, Export), adaptadores (importación CSV/Excel), persistencia PostgreSQL.
4. Presentación / API: FastAPI (REST + JSON), autenticación JWT.
5. Frontend: React + Vite + Tailwind (SPA) consumiendo API.

Justificación:
- Monolito modular inicial reduce complejidad operacional comparado con microservicios (menor coste de coordinación, despliegue sencillo en Docker Compose).
- Hexagonal permite aislar dominio de detalles técnicos y facilita futura extracción de microservicios.
- DDD asegura claridad en reglas de negocio y escalabilidad conceptual.
- FastAPI + React aceleran el time-to-market.

No se eligen microservicios aún: volumen de transacciones moderado, equipo pequeño (4 devs), necesidad de velocidad y simplicidad de despliegue.

### 15. Diagrama de arquitectura

Ya realizado en archivos de diseño (PlantUML / C4). Se referenciarán en anexos sin replicar aquí.

### 16. Frameworks, herramientas y stack tecnológico

Backend:
- Lenguaje: Python 3.12.
- Framework: FastAPI (ASGI, performance, tipado, OpenAPI automático).
- ORM: SQLAlchemy (modo async con asyncpg) / alternativa futura Tortoise si simplifica migraciones.
- Autenticación: OAuth2 Password Flow + JWT (access + refresh) usando PyJWT.
- Validación / Esquemas: Pydantic v2.
- Tareas programadas: APScheduler / Celery (evaluar si se requiere background jobs para alertas y agregaciones).

Frontend:
- Framework: React 18 + Vite (build rápido).
- Estilos: Tailwind CSS.
- Estado: React Query (fetch/cache), Context API para auth.
- Gráficas: Recharts / Chart.js.

Base de datos:
- PostgreSQL 15 (razones: tipos avanzados, extensiones, robustez ACID, facilidad backup).
- Migraciones: Alembic.

Infra y DevOps:
- Contenedores: Docker + Docker Compose (local y entorno staging).
- Proxy / Reverse: NGINX (balanceo simple, TLS terminación).
- Certificados: Certbot + cron (producción).
- Monitorización inicial: Health endpoints + logs estructurados; evolución a Prometheus + Grafana.
- Backups: pg_dump programado + retención (7 días diarios, 4 semanas semanales, 6 meses mensuales).
- CI/CD: GitHub Actions (lint, test, build, scan de dependencias, push imagen a registry).

Testing:
- Unit Tests: pytest.
- API Tests: httpx + pytest.
- Coverage: pytest-cov (objetivo ≥ 70% core domain en primeras iteraciones).
- Lint: Ruff + Black (formato), mypy para tipado estricto domain/application.

Seguridad:
- Dependabot para alertas CVE.
- Headers seguros (CORS, HSTS en NGINX producción).
- Hash contraseñas: bcrypt.

Mensajería/Eventos (futuro opcional): Redis Streams o RabbitMQ si se requiere procesamiento asíncrono separado.

### 17. Estándares de desarrollo

Estrategia de repositorios y organización GitHub:
- Organización: `pms-org/`.
- Repos recomendados (multi-repo para independencia):
  1. `pms-backend` (FastAPI + dominio).
  2. `pms-frontend` (React SPA).
  3. `pms-infra` (Docker Compose, NGINX configs, scripts de despliegue, backups).
  4. `pms-docs` (Markdown, PlantUML, especificaciones).
  (Alternativa: monorepo con `/backend`, `/frontend`, `/infra`, `/docs` si se prioriza coordinación de versiones; para independencia entre devs se inicia multi-repo.)

Branching (GitFlow simplificado):
- `main`: código estable y liberable.
- `develop`: integración continua de nuevas features.
- `feature/<módulo>-<breve>`: desarrollo de funcionalidad (ej. `feature/parking-fee-calculation`).
- `hotfix/<issue>`: correcciones críticas en producción.
- `release/<versión>`: preparación de versiones (tag + changelog).

Convenciones de naming:
- Python: snake_case para funciones/métodos, PascalCase para clases, módulos cortos y semánticos.
- React: PascalCase para componentes, archivos `.tsx` organizados por feature.
- Repositorios: prefijo `pms-`.

Commits (Conventional Commits):
`feat:`, `fix:`, `docs:`, `refactor:`, `perf:`, `test:`, `chore:`, `build:`.
Ej: `feat(parking): calcular tarifa con minutos gratuitos`.

Pull Requests:
- Plantilla obligatoria (Descripción, Issue link, Cómo probar, Checklist). 
- Revisión mínima: 1 aprobador distinto al autor (ideal code owner del módulo).
- Checks requeridos: build + tests + lint.

Code Owners:
- Archivo `CODEOWNERS` en cada repo asignando rutas por desarrollador (ej. `/parking/* @devB`).

Calidad y Estilo:
- Lint (Ruff, ESLint) y format (Black, Prettier) en CI.
- Reglas de complejidad: función Python máx 40 líneas (excluir tests).
- Evitar lógica de negocio en controladores FastAPI — usar servicios de aplicación.

Versionado:
- SemVer: MAJOR.MINOR.PATCH.
- Tag de release automatizado desde GitHub Action al merge a `main` de una branch `release/*`.

Documentación:
- Docstrings en servicios de dominio y entidades complejas.
- README por módulo.
- PlantUML y diagramas actualizados en `pms-docs`.

Revisiones Arquitectónicas:
- Cada 2 sprints (quincenal) revisar deuda técnica y posibles refactors cruzados.

### 18. Patrones usados

Patrones seleccionados (se exige mínimo dos):
1. Repository Pattern (per context): Abstrae acceso a persistencia (SQLAlchemy) evitando filtrar lógica de dominio por detalles de infraestructura. Permite test con repos fake/in-memory.
2. Strategy Pattern: Para cálculo de precios y bonos. Ejemplos:
  - `PricingStrategy`: base vs. con convenio vs. con ajuste global.
  - `BonusStrategy`: porcentaje estándar vs. porcentaje personalizado por lavador.
  Facilita cambiar políticas sin modificar servicios de aplicación.
3. Adapter Pattern (infra): Adaptadores para EmailService, CSV/Excel Importer, Exporters PDF/CSV encapsulan proveedores externos.
4. Domain Service: Lógica que no pertenece a una sola entidad (PricingService). Mantiene invariantes cruzadas.
5. Event Publisher (Domain Events): Emisión de eventos (WashFinished, BonusCalculated) para futuras proyecciones.

Patrones descartados inicialmente:
- Unit of Work explícito: SQLAlchemy session gestionada por dependencia FastAPI + scope de request; suficiente por ahora.
- Command Bus / Event Sourcing: complejidad añadida no justificada en fase inicial.

## V. DISEÑO DETALLADO DE SOFTWARE
### 19. Diagramas de clases (o equivalente)

Para cada módulo del dominio:

Entidades

Relación entre clases

Atributos y métodos relevantes

### 20. Interfaces por capa

Separar por ejemplo:

Dominio

Interfaces de repositorios

Servicios de dominio

Aplicación

Casos de uso

Servicios de aplicación

Infraestructura

Adaptadores concretos

Esto ayuda a que los programadores arranquen sin perderse.

### 21. APIs y contratos

Si el sistema expone APIs:

Endpoints

Método

Cuerpo de solicitud

Respuesta

Códigos HTTP usados

Ejemplos JSON

### 22. Modelo de datos

Modelo lógico (MER)

Tablas

Relaciones

Tipos de datos

Reglas adicionales (triggers, constraints)

## VI. OPERACIÓN Y NO FUNCIONALES
### 23. Requisitos no funcionales

### **RNF-01 – Usabilidad del sistema**

La interfaz debe ser visual, intuitiva y fácil de usar, especialmente para usuarios con bajo nivel tecnológico.

### **RNF-02 – Rendimiento y tiempo de respuesta**

Las operaciones principales (login, registro, cálculos) no deben superar los 2 segundos de respuesta.

### **RNF-03 – Seguridad de datos**

El sistema debe encriptar contraseñas y proteger la información sensible con autenticación segura.

### **RNF-04 – Disponibilidad y recuperación**

El sistema debe mantener la integridad de los datos ante fallos y permitir recuperación de información.

### **RNF-05 – Escalabilidad y mantenimiento**

El diseño debe permitir agregar nuevos módulos (por ejemplo, nómina o facturación) sin alterar el funcionamiento base.

### 24. Políticas de seguridad

Manejo de roles

Protección de datos

Acceso a BD

Políticas de contraseñas

### 25. Despliegue

Estrategia: Docker + Docker Compose para desarrollo/local y staging; producción en VPS o PaaS (DigitalOcean / Render / Railway).

Contenedores principales:
- `backend`: Imagen basada en Python slim + dependencias; arranque con `gunicorn -k uvicorn.workers.UvicornWorker`.
- `frontend`: Build React (Vite) → artefacto estático servido por NGINX.
- `nginx`: Reverse proxy, TLS terminación, red interna a backend.
- `postgres`: Base de datos con volumen persistente y backups programados.
- (Opcional futuro) `worker`: Tareas background (bonos nocturnos, alertas mensualidades).

Compose (servicios y redes): Una red interna (`app_net`) y volúmenes: `pg_data`, `pg_backups`.

Variables de entorno principales:
- `DATABASE_URL`, `JWT_SECRET`, `REFRESH_SECRET`, `RATE_ADJUSTMENT_POLICY`, `BONUS_BASE_PERCENTAGE`.

Pipeline DevOps (GitHub Actions):
1. Build & Test (lint + pytest + coverage).
2. Build Docker images (tag `:sha` y `:latest`).
3. Security scan (Trivy) sobre imagen.
4. Deploy staging (manual approval) — `docker compose pull && up -d`.
5. Deploy producción (tag release) — mismo proceso + migraciones Alembic.

Entornos:
- Desarrollo local: Compose completo sin TLS, datos iniciales seed.
- QA/Staging: Igual a producción con datos de prueba y TLS auto (Let's Encrypt staging).
- Producción: TLS válido, backups programados, monitoreo básico.

Backups:
- Script cron contenedor: `pg_dump` diario → `pg_backups`; subida opcional a S3/Spaces.
- Restauración documentada (ver Sección 26 mantenimiento).

Primer despliegue (guía rápida):
1. Instalar Docker / Docker Compose.
2. Crear `.env` con secretos y credenciales DB.
3. Ejecutar `docker compose up --build` en staging.
4. Aplicar migraciones: `docker compose exec backend alembic upgrade head`.
5. Crear usuario administrador inicial vía endpoint `/admin/seed` o script.
6. Configurar Certbot (si VPS): emitir certificados y montar volumen en NGINX.

Escalabilidad futura:
- Separar worker asíncrono, añadir Redis para cola, horizontalizar backend detrás de NGINX/Load Balancer.


### 26. Mantenimiento esperado

Incluye:

Backup

Recuperación

Plan de continuidad

Métricas a monitorear