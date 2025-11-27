# Especificación Explícita de Vistas Frontend
> Documento de referencia para implementación de UI/UX (Dev C y Dev D)

Este documento detalla las pantallas y componentes específicos requeridos para cubrir las Historias de Usuario del sistema PMS.

## 1. Módulo de Autenticación y Acceso
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Login Administrativo** | `/auth/login` | • Input Email<br>• Input Password<br>• Link "Olvidé mi contraseña"<br>• Botón "Ingresar" |
| **Login Operativo** | `/auth/operational` | • Input PIN o Usuario simple<br>• Teclado numérico en pantalla (opcional para táctil)<br>• Botón "Iniciar Turno" |
| **Recuperación Contraseña** | `/auth/recovery` | • Input Email<br>• Botón "Enviar Link" |
| **Reset Contraseña** | `/auth/reset/:token` | • Input Nueva Contraseña<br>• Input Confirmar Contraseña |

## 2. Módulo de Gestión de Personal (Lavadores)
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Directorio de Lavadores** | `/staff/washers` | • **Tabla de Datos**:<br>  - Columnas: Nombre, Documento, Teléfono, Estado (Activo/Inactivo), % Comisión<br>  - Acciones: Editar (Icono Lápiz), Eliminar/Desactivar (Icono Basura)<br>• **Botón Flotante/Principal**: "Nuevo Lavador" |
| **Formulario Lavador** | Modal o `/staff/washers/form` | • Inputs: Nombre Completo, Cédula, Teléfono, Correo (Opcional)<br>• Selector: Porcentaje de Comisión Base<br>• Toggle: Activo/Inactivo |

## 3. Módulo Operativo (Core)
### 3.1 Recepción e Ingreso
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Recepción Unificada** | `/operations/reception` | • **Pestañas/Toggle**: "Parqueadero" vs "Lavado"<br>• **Buscador**: Buscar vehículo activo por placa |
| **Formulario Ingreso Parqueadero** | (En Recepción) | • Input Placa (Grande, validación formato)<br>• Selector Tipo Vehículo (Moto/Carro/Camioneta - Usar Iconos)<br>• Checkbox "¿Deja Cascos?" + Input Cantidad<br>• Botón "Registrar Ingreso" |
| **Formulario Ingreso Lavado** | (En Recepción) | • Input Placa<br>• Selector Tipo Vehículo<br>• Selector Tipo de Lavado (Básico, General, Motor, etc.)<br>• **Selector Lavador Asignado** (Dropdown con lavadores activos)<br>• Checkbox "¿Incluye Parqueo?"<br>• Botón "Crear Orden" |

### 3.2 Gestión de Servicios (Kanban/Tablero)
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Tablero de Control** | `/operations/dashboard` | • **Vista de Columnas (Kanban)**:<br>  1. **En Espera**: Vehículos registrados sin iniciar.<br>  2. **En Proceso**: Lavados en curso (Mostrar cronómetro/hora inicio).<br>  3. **Terminado/Por Cobrar**: Listos para entrega.<br>• **Tarjetas de Servicio**:<br>  - Placa, Tipo Servicio, Lavador, Hora Ingreso.<br>  - Botones de Acción rápida: "Iniciar", "Terminar", "Cobrar". |

### 3.3 Salida y Facturación
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Checkout / Cobro** | `/operations/checkout/:id` | • **Resumen de Costos**:<br>  - Valor Servicio Lavado<br>  - Valor Tiempo Parqueadero (Cálculo automático)<br>  - Recargos (Cascos, etc.)<br>  - Descuentos (Input código o selector)<br>• **Total a Pagar** (Grande y visible)<br>• Selector Método Pago (Efectivo, Nequi, Daviplata)<br>• Botón "Confirmar Pago y Salida" |

## 4. Módulo Financiero y Administrativo
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Gestión de Gastos** | `/finance/expenses` | • **Tabla Histórica**: Fecha, Concepto, Categoría, Monto, Responsable.<br>• **Botón "Registrar Gasto"** (Abre Modal):<br>  - Input Monto, Input Descripción, Selector Categoría (Insumos, Servicios, Nómina, Varios). |
| **Nómina y Vales** | `/finance/payroll` | • **Tabla Resumen por Lavador**:<br>  - Nombre, Total Lavados Mes, Total Comisiones, Total Vales, **Neto a Pagar**.<br>• **Acción "Registrar Vale/Adelanto"**: Seleccionar lavador y monto. |
| **Configuración Tarifas** | `/admin/pricing` | • Tabla editable de precios por Tipo de Vehículo y Tipo de Servicio.<br>• Configuración de valor minuto/hora parqueadero. |

## 5. Analítica y Reportes (Dashboard Gerencial)
| Vista | Ruta Sugerida | Elementos Clave |
|-------|---------------|-----------------|
| **Dashboard Principal** | `/dashboard` | • **Filtro Global de Fechas** (Hoy, Esta Semana, Este Mes, Rango).<br>• **Tarjetas KPI (Top Row)**:<br>  - Ingresos Totales<br>  - Gastos Totales<br>  - Vehículos Atendidos<br>  - Ticket Promedio<br>• **Gráficas Principales**:<br>  - Ingresos vs Gastos (Barras/Líneas)<br>  - Ocupación Promedio por Hora (Heatmap/Barras)<br>  - Distribución Tipos de Lavado (Pie Chart)<br>  - Top Lavadores (Ranking) |
| **Reportes Exportables** | `/reports/export` | • Selectores de rango de fecha y tipo de reporte (Excel/PDF).<br>• Botón "Descargar". |

## Notas Técnicas para Frontend
1. **Componentes Reutilizables**: Crear componentes base para `Card`, `Table`, `Modal`, `Button` y `Input` para mantener consistencia.
2. **Manejo de Estado**: Usar React Context o Zustand para manejar el estado global de la sesión (Usuario logueado) y el carrito/orden actual.
3. **Responsive**: Las vistas del **Módulo Operativo** deben ser 100% funcionales en móvil (diseño vertical). El Dashboard Gerencial puede priorizar Desktop.
4. **Feedback**: Implementar "Toasts" o notificaciones flotantes para confirmar acciones (ej: "Lavado registrado exitosamente").
