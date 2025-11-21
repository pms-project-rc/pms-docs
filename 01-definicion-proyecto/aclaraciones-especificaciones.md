# ACLARACIONES Y RESPUESTAS A ESPECIFICACIONES DEL SISTEMA PMS

**Fecha:** 20 de Noviembre de 2025  
**Versión:** 1.0  
**Estado:** Aprobado

Este documento complementa el archivo `especificacionesGeneralesProducto.md` con aclaraciones específicas sobre reglas de negocio, cálculos, y políticas operativas que surgieron durante el análisis inicial del proyecto.

---

## 1. TARIFAS Y PRECIOS

### 1.1 Precios de servicios de lavado

Se han definido precios base para **cuatro tipos de vehículos**: carro, camión, motocicleta y **bicicleta** (añadida). Cada servicio incluye tiempo de parqueo gratuito.

**Referencia:** Ver documento `tarifas-y-precios.md` para tabla completa.

**Tipos de vehículos soportados:**
- 🚗 Carro (vehículo liviano)
- 🚛 Camión
- 🏍️ Motocicleta
- 🚲 **Bicicleta** (NUEVO)

### 1.2 Tarifas de parqueo

**Estructura de cobro:**
- **0-6 horas:** Tarifa por minuto
- **6-12 horas:** Tarifa plana fija

**Valores definidos:**
- Carro: $80/min o $20.000 fijo (6-12h)
- Camión: $120/min o $35.000 fijo (6-12h)
- Moto: $50/min o $10.000 fijo (6-12h)
- Bicicleta: $30/min o $5.000 fijo (6-12h)

**Recargo de cascos (solo motos):** $1.000 COP por casco

### 1.3 Mensualidades

**Cobertura:** Solo parqueo (lavados se cobran aparte)

**Precios mensuales:**
- Carro: $170.000
- Moto: $70.000
- Camión: $300.000
- Bicicleta: $40.000

**Vigencia:** 30 días desde activación  
**Renovación:** Alerta a los 5 días de vencer

---

## 2. CÁLCULOS Y REGLAS DE COBRO

### 2.1 Cálculo de tiempo de parqueo con lavado

**Regla actualizada:**

```
tiempo_cobrable = max(0, tiempo_total - minutos_gratuitos_lavado)
cobro_parqueo = tiempo_cobrable * tarifa_por_minuto
```

**Aclaraciones importantes:**
1. **NO hay redondeo por bloques** — se cobra por minuto exacto
2. **Tarifa mínima:** Desde el minuto 1 (si excede tiempo gratuito)
3. **Después de descontar minutos gratuitos, el reloj "reinicia" desde cero**

**Ejemplo:**
- Tiempo total: 45 minutos
- Minutos gratuitos (lavado general carro): 30 minutos
- Tiempo cobrable: 45 - 30 = 15 minutos
- Cobro: 15 × $80 = $1.200 COP

### 2.2 Transición entre tarifa por minuto y tarifa plana

**Ejemplo carro (tarifa $80/min hasta 360 min, luego $20.000 fijo):**
- 359 minutos = 359 × $80 = $28.720
- 361 minutos = $20.000 (tarifa plana)

**Lógica:**
```
if (minutos_totales <= 360):
    cobro = minutos_totales * tarifa_minuto
else:
    cobro = tarifa_plana_fija
```

---

## 3. GESTIÓN DE TURNOS

### 3.1 Modelo de turnos

**Respuesta:** Los turnos son **manuales** (login/logout)

**Características:**
- Se inician al hacer login del administrador operativo
- Se cierran al hacer logout o al finalizar jornada manualmente
- Un turno cerrado **NO acepta** nuevos gastos ni servicios

### 3.2 Manejo de sesiones múltiples

**Regla definida:**

1. **Solo UN administrador operativo puede estar logueado a la vez**
2. Si un operativo no cierra su turno, el siguiente puede:
   - Forzar logout del usuario previo
   - Iniciar sesión con su propia cuenta
3. **Solo UN administrador global puede estar logueado simultáneamente** (evita errores de configuración)

**Implementación técnica:**
- Tabla `operational_admins` tiene campo `current_shift_id`
- Al hacer login, verificar si existe otro usuario con `current_shift_id NOT NULL`
- Opción: forzar cierre de sesión anterior (con registro en audit_log)

---

## 4. BONOS Y VALES

### 4.1 Cálculo de bonos

**Regla actualizada:**

```
bono_diario = Σ (valor_neto_lavado * porcentaje_lavador)
```

**Aclaraciones:**
1. **Se calcula sobre el valor NETO** (después de descuentos de convenio)
2. Si un lavado se anula después de cerrar el día, se **resta del bono acumulado** del lavador
3. El porcentaje puede ser:
   - Global (configurado en `business_config`)
   - Individual por lavador (campo `bonus_percentage` en tabla `washers`)

**Ejemplo:**
- Lavado con cera: $28.000 (precio base)
- Descuento convenio 20%: $28.000 × 0.8 = $22.400 (valor neto)
- Porcentaje lavador: 15%
- Bono: $22.400 × 0.15 = $3.360

### 4.2 Descuento de vales

**Periodicidad:** Mensual (al final del periodo)

**Opción adicional:** Configurar pago en número específico de **quincenas**

**Campo en BD:** `vouchers.installments` (número de periodos de pago)

**Lógica:**
```
bono_final = bono_acumulado_mensual - descuento_vale
```

Si el bono no cubre la cuota completa:
- Registrar saldo pendiente en `vouchers.remaining_balance`
- Marcar como deuda del mes siguiente

---

## 5. EXENCIONES Y AUDITORÍA

### 5.1 Campos requeridos para exenciones

**Cuando se exime un cobro de parqueo:**

1. **Usuario:** Quién aprobó la exención (ID y tipo: global/operativo)
2. **Timestamp:** Fecha y hora exacta
3. **Motivo/Razón:** Campo de texto obligatorio

**Implementación en BD:**
- `parking_records.is_exempted` (boolean)
- `parking_records.exemption_reason` (text, obligatorio si is_exempted=true)
- `parking_records.exempted_by` (FK a operational_admins)
- `parking_records.exempted_at` (timestamp)

### 5.2 Límites y notificaciones

**¿Hay límite de exenciones por turno?** NO

**¿Se notifica al admin global?** SÍ

**Implementación:**
- Cada exención genera un registro en tabla `notifications`
- Tipo: `exemption`
- Destinatario: `global_admin`
- Sistema de notificaciones **no intrusivo** (badge numérico, no popups molestos)

### 5.3 Registro de auditoría

**Tabla dedicada:** `audit_logs`

**Eventos auditados:**
- Exenciones de parqueo
- Cambios de tipo de vehículo
- Correcciones de lavados finalizados
- Ajustes de tarifas
- Aprobación de servicios de convenio

**Campos registrados:**
- `entity_type` y `entity_id` (qué se modificó)
- `action` (CREATE, UPDATE, DELETE, EXEMPT, APPROVE)
- `old_value` / `new_value` (cambios realizados)
- `reason` (motivo del cambio)
- `performed_by_type` y `performed_by_id` (quién lo hizo)
- `ip_address` (trazabilidad adicional)

---

## 6. MENSUALIDADES

### 6.1 Cobertura

**Respuesta definitiva:** La mensualidad cubre **SOLO parqueo**

Los servicios de lavado se cobran normalmente (con descuento de convenio si aplica)

### 6.2 Tipos de vehículos elegibles

**Respuesta:** Cualquier tipo de vehículo puede tener mensualidad

Tipos soportados:
- Carro
- Moto
- Camión
- Bicicleta

### 6.3 Diferencia de precios

**Sí, hay diferencia:**
- Carro: $170.000/mes
- Moto: $70.000/mes
- Camión: $300.000/mes
- Bicicleta: $40.000/mes

---

## 7. CONVENIOS EMPRESARIALES

### 7.1 Modelo de facturación

**Modelo:** Postpago (factura mensual)

**Flujo:**
1. Cliente de empresa recibe servicio
2. Se registra en el sistema con descuento de convenio
3. Servicios quedan en estado "pendiente de aprobación"
4. Administrador global **aprueba** servicios antes de facturar
5. Se genera factura mensual consolidada

### 7.2 Opción de pago inmediato

**Respuesta:** Sí, el cliente puede optar por pagar en el momento del servicio si lo requiere

**Implementación:**
- Campo `washing_services.is_agreement_service` (boolean)
- Si cliente paga inmediatamente: registrar como servicio normal con descuento aplicado
- Si es postpago: marcar para facturación mensual

### 7.3 Aprobación de servicios

**¿Quién aprueba?** Administrador global

**¿Cuándo?** Antes de generar la factura mensual a la empresa

**Implementación en BD:**
- `washing_services.agreement_approved` (boolean, default: false)
- `washing_services.approved_by` (FK a global_admins)
- `washing_services.approved_at` (timestamp)

---

## 8. SEGURIDAD Y RECUPERACIÓN DE CONTRASEÑA

### 8.1 Cambio de HU-02

**Método anterior (INSEGURO):** Enviar nueva contraseña por email

**Método nuevo (SEGURO):** Enviar enlace temporal con token

**Flujo actualizado:**
1. Usuario solicita recuperación ingresando email
2. Sistema genera token único (UUID)
3. Se envía email con enlace: `https://pms.app/reset-password?token=ABC123`
4. Token válido por **1 hora**
5. Usuario hace clic, ingresa nueva contraseña
6. Token se marca como "usado" y expira

**Tabla:** `password_reset_tokens`

---

## 9. ESTADOS DEL LAVADO - REVERSIBILIDAD

### 9.1 Correcciones de lavados finalizados

**Regla:** Estado `FINISHED` bloquea cambios, salvo corrección auditada

**¿Quién puede corregir?** Solo administrador global

**¿Cómo?** Desde su dashboard personal (no desde vista operativa)

**¿Se crea nuevo registro o se modifica?** Se **modifica** el existente

**Trazabilidad:**
- Cambio se registra en `audit_logs`
- Razón obligatoria
- Cambios se ven reflejados en tiempo real en el shift del admin operativo

**Implementación:**
- Endpoint especial: `PATCH /api/admin/washing-services/{id}/correct`
- Requiere rol `GLOBAL_ADMIN`
- Parámetros: `field`, `new_value`, `reason`

---

## 10. ASIGNACIÓN DE LAVADORES

### 10.1 Sistema de asignación

**Modelo mixto:** Manual + sugerencia automática

**Características:**
1. **Orden automático sugerido:** Sistema muestra lavadores disponibles en orden de antigüedad o rotación
2. **Asignación manual:** Administrador puede elegir cualquier lavador disponible
3. **Validación:** Sistema impide asignar lavador que ya tiene servicio en estado `IN_PROCESS`

### 10.2 Cola de espera

**¿Asignación automática al terminar?** NO

**Flujo:**
1. Lavador termina servicio (estado → `FINISHED`)
2. Sistema marca lavador como "disponible"
3. Siguiente servicio en cola se queda en `WAITING`
4. Administrador operativo asigna manualmente el siguiente lavador

**Razón:** Flexibilidad operativa (ej: enviar lavador a descanso, rotar servicios complejos)

### 10.3 Capacidad máxima

**¿Existe límite de lavadores simultáneos?** NO

Todos los lavadores activos pueden trabajar simultáneamente (cada uno en un vehículo)

---

## 11. CONFIGURACIÓN GENERAL DEL SISTEMA

### 11.1 Moneda

**Respuesta:** Solo COP (Peso Colombiano)

**NO** se soporta multimoneda en fase inicial

### 11.2 Zona horaria

**Respuesta:** Solo Colombia (America/Bogota, UTC-5)

**NO** se requiere soporte para múltiples sedes internacionales

### 11.3 Retención de datos

**Periodo:** 1 año

**Razón:** Exportación a CSV/PDF permite archivar datos históricos externamente

**Política de limpieza:**
- Datos mayores a 1 año pueden archivarse (migrar a tabla `*_archived`)
- Reportes y métricas consolidadas se mantienen indefinidamente

### 11.4 Concurrencia de operativos

**Respuesta:** NO pueden estar dos operativos logueados al mismo tiempo

**Implementación:**
- Validar `current_shift_id IS NULL` en otros usuarios antes de permitir login
- Opción de forzar cierre de sesión previa (con confirmación)

### 11.5 Validación de placas

**Regla simple:** Si placa termina en **letra** → Motocicleta

**NO** hay validación de formato estricto (ej: ABC-123) en fase inicial

**Flexibilidad:** Administrador puede cambiar tipo manualmente si clasificación automática falla

### 11.6 Generación de reportes

**Modalidad:** Bajo demanda (on-demand)

**Tipos:**
- Reportes de turno (al cerrar shift)
- Reportes generales (filtros por fecha, tipo, etc.)
- Exportación CSV/PDF

**NO** hay generación programada automática (ej: diaria a las 6 AM) en fase inicial

---

## 12. CAMBIOS PENDIENTES EN DOCUMENTACIÓN

### 12.1 Historia de usuario HU-02

**Estado:** Pendiente actualización por encoding UTF-8

**Cambio requerido:** Reemplazar envío de contraseña temporal por token de recuperación

**Archivo afectado:** `especificacionesGeneralesProducto.md` líneas 159-171

---

## 13. RESUMEN DE IMPACTO EN BASE DE DATOS

**Tablas nuevas creadas:**
1. `password_reset_tokens` - Tokens de recuperación de contraseña
2. `fleet_vehicles` - Relación vehículos-empresas
3. `audit_logs` - Registro de auditoría completo
4. `notifications` - Sistema de notificaciones
5. `business_config` - Configuración global del negocio

**Campos importantes añadidos:**
- `vehicles.type` ahora soporta `'bicicleta'`
- `washing_services.agreement_approved`, `approved_by`, `approved_at`
- `parking_records.is_exempted`, `exemption_reason`, `exempted_by`, `exempted_at`
- `vouchers.installments` (soporta quincenas)
- `rates` rediseñado para soportar tarifas por minuto y planas

**Índices críticos:**
- `(vehicle_id, shift_id)` en `washing_services`
- `(washer_id, status)` en `washing_services`
- `(recipient_type, recipient_id, is_read)` en `notifications`
- `(agreement_id, vehicle_id)` único en `fleet_vehicles`

---

**Fin del documento**

**Próximos pasos:**
1. ✅ Crear tabla de precios (`tarifas-y-precios.md`)
2. ✅ Actualizar reglas de negocio en especificación principal
3. ✅ Rediseñar modelo de base de datos (`dbdiagram.dbml`)
4. ⏳ Actualizar diagrama UML de clases
5. ⏳ Actualizar HU-02 (pendiente por UTF-8)
6. ⏳ Revisar y actualizar diagramas C4
