# Plan de Distribución de Trabajo

## Asignación de Historias de Usuario por Desarrollador

### Dev A (Juan Camilo) - Líder Técnico, Financiero, Analítica
**Responsabilidades**: Arquitectura base, módulo financiero, reportes y analítica.

**Historias Asignadas**:
- **HU-01**: Login admin global
- **HU-02**: Recuperación de contraseña
- **HU-32/35/37**: Gestión de Gastos (Turno y Diarios)
- **HU-34**: Reporte automático de turno
- **HU-38/39**: Vales y descuentos en nómina
- **HU-43/44/45**: Gestión de Bonos (Cálculo diario, Acumulado mensual, Porcentajes)
- **HU-48/49**: Rendimiento Operativo (Cálculo y Real-time)
- **HU-17**: Análisis tiempos de lavado
- **HU-28/29**: Reportes por empresa convenio
- **HU-46/47**: Consolidado ingresos (Tabla y Gráfica)
- **HU-50/51**: Gráfica actividad de lavado
- **HU-53/54**: Ocupación parqueadero (Registro y Gráfica)
- **HU-55/56/57**: Dashboard de métricas (Generales, KPIs)

**Entregables Adicionales**:
- Configuración inicial de arquitectura (DDD + Hexagonal)
- Setup de base de datos PostgreSQL
- Configuración de Docker Compose
- Definición de esquemas de Alembic

---

### Dev B (Juan Esteban) - Operativo, Tarifas, Mensualidades
**Responsabilidades**: Flujos operativos principales, gestión de tarifas y mensualidades.

**Historias Asignadas**:
- **HU-03**: Login operativo
- **HU-07**: Registro ingreso parqueadero (con recargo cascos)
- **HU-08**: Clasificación automática vehículo (placa letra vs número)
- **HU-09**: Modificación manual tipo vehículo
- **HU-10**: Salida de parqueadero y cobro
- **HU-11**: Registro ingreso lavado
- **HU-12**: Selección tipo de lavado
- **HU-13**: Asignación de lavador
- **HU-14**: Servicio con o sin parqueo
- **HU-15**: Gestión estados (En espera -> Proceso -> Terminado)
- **HU-16**: Timestamps automáticos en cambios de estado
- **HU-18/19/20**: Gestión de Mensualidades (Registro, Renovación, Alertas)
- **HU-21/22/23**: Gestión de Tarifas (Base, Modificación, Ajuste Global)
- **HU-24/25/26**: Gestión Convenios (Empresas, Descuentos, Importación Flotas)
- **HU-27**: Aplicación de descuentos convenio en lavado
- **HU-40/41**: Tiempos de parqueo gratuito (Config y Cálculo)

**Entregables Adicionales**:
- Implementación de reglas de negocio core
- Lógica de cálculo de tarifas
- Gestión de estados de servicios

---

### Dev C (Jaime Darley) - Frontend, UI/UX
**Responsabilidades**: Todas las interfaces de usuario y experiencia.

**Historias Asignadas**:
- Implementación de todas las vistas frontend para las HU asignadas a Dev A y Dev B
- Diseño de componentes reutilizables con React y Tailwind CSS
- Implementación de formularios de gestión (lavadores, vehículos, etc.)
- Dashboards y visualizaciones gráficas
- Responsive design para acceso móvil
- Sistema de navegación y menús

**Entregables Adicionales**:
- Setup de Vite + React 18
- Configuración de Tailwind CSS
- Biblioteca de componentes base
- Sistema de routing
- Integración con APIs del backend

---

### Dev D (Jeremy Lee) - Soporte, Módulos Auxiliares
**Responsabilidades**: CRUD de lavadores, soporte a otros módulos, testing.

**Historias Asignadas**:
- **HU-04/05/06**: CRUD de perfiles de lavadores
- **HU-30**: Asignación de lavadores (Control)
- Soporte en testing e integración
- Documentación técnica
- Scripts de utilidad y mantenimiento

**Entregables Adicionales**:
- Tests unitarios e integración
- Documentación de APIs
- Scripts de seed de datos iniciales
- Validaciones y manejo de errores

---

## Fases de Desarrollo

### Fase 1 - Fundamentos (Semanas 1-2)
- **Dev A**: Arquitectura base, autenticación, setup BD
- **Dev B**: Modelos de dominio core
- **Dev C**: Setup frontend, componentes base
- **Dev D**: CRUD lavadores, testing framework

### Fase 2 - Operativo (Semanas 3-5)
- **Dev A**: Módulo financiero básico
- **Dev B**: Parqueadero y lavado completo
- **Dev C**: Interfaces operativas
- **Dev D**: Soporte y testing

### Fase 3 - Comercial (Semanas 6-7)
- **Dev B**: Tarifas, mensualidades, convenios
- **Dev C**: Interfaces comerciales
- **Dev A y Dev D**: Soporte

### Fase 4 - Analítica (Semanas 8-9)
- **Dev A**: Reportes y dashboards backend
- **Dev C**: Visualizaciones y gráficas
- **Dev B y Dev D**: Soporte y testing

### Fase 5 - Integración y Testing (Semana 10)
- Todo el equipo: Testing integral, correcciones, optimizaciones
