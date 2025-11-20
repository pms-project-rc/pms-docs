# Reglas de Negocio

Lista de invariantes y políticas que el sistema debe garantizar.

## 1. Clasificación y Tarifas
- **Clasificación Automática**: Si la placa termina en letra → tipo "Motocicleta". Si termina en número → tipo "Automóvil". Puede ajustarse manualmente.
- **Vigencia de Tarifas**: La tarifa final cobrada siempre corresponde a la vigente en el momento del cálculo (cierre del servicio), no al inicio.

## 2. Parqueadero
- **Cálculo de Cobro**:
  1. `(Tiempo Total - Minutos Gratuitos por Lavado) * Tarifa por Minuto`
  2. Si el resultado es negativo, el cobro es 0.
- **Cascos**: Cada casco genera un recargo fijo configurado. Solo aplica si `Tipo = Motocicleta` y `Cascos > 0`.
- **Mensualidad Activa**: Un vehículo con mensualidad vigente no paga parqueo estándar.

## 3. Lavados y Servicios
- **Estados de Lavado**: El flujo estricto es `EN ESPERA` → `EN PROCESO` → `TERMINADO`.
- **Timestamps**: Se debe registrar la fecha y hora exacta de cada cambio de estado para métricas de eficiencia.
- **Bloqueo**: Un servicio en estado `TERMINADO` no puede cambiar de estado salvo corrección auditada por un Admin Global.

## 4. Incentivos (Bonos y Vales)
- **Cálculo de Bono**: `Bono Diario = Σ (Valor Lavado * Porcentaje Lavador)`.
- **Descuento de Vales**: Los vales se descuentan del bono acumulado en el periodo correspondiente. Si el bono &lt; cuota, queda saldo pendiente.

## 5. Financiero
- **Rendimiento Operativo**: Se calcula como `R = Ingresos – (Gastos + Bonos)`. Se recalcula ante cualquier modificación histórica.
- **Integridad de Turnos**: Un turno "Cerrado" no acepta nuevos registros de gastos ni servicios.
- **Seguridad**: Cambios manuales sensibles (exención de cobro, cambio de tipo de vehículo) deben registrar obligatoriamente: Usuario + Timestamp.
