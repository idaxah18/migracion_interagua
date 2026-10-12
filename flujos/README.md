# Catálogo Maestro de Flujos de Negocio — Migración INTERAGUA

Este directorio contiene la documentación técnica y funcional en **100% lenguaje natural** de los **103 flujos de negocio activos** del canal oficial de WhatsApp de Interagua (excluyendo entornos obsoletos o de pruebas de control de calidad QA).

Cada documento ha sido elaborado bajo el estándar riguroso de calidad ("Gold Standard"): describe el objetivo del flujo en palabras simples y sin tecnicismos innecesarios, los datos de entrada requeridos, el diálogo interactivo textual y botones (`>`), el desglose técnico de cada API/servicio consumido (herramienta, ID, método, endpoint exacto, payload JSON y códigos de error), y sus condiciones de salida o derivación.

---

## 🗺️ Índice por Módulos Funcionales

### 1. Inicio, Menú Principal y Privacidad (LOPD)
- [0 - Inicio](./0-inicio.md) (Skill `51065`): Entrada principal y saludo al ciudadano.
- [0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill `51066`): Consentimiento de protección de datos personales.
- [0.2 - LOPD - Deriva Contexto](./0-2-lopd-deriva-contexto.md) (Skill `51067`): Enrutador post-autorización LOPD.
- [0.3 - Menú Principal - Bienvenida](./0-3-menu-principal-bienvenida.md) (Skill `51161`): Bienvenida con botones rápidos.
- [0.3 - Menú Principal](./0-3-menu-principal.md) (Skill `51068`): Menú interactivo estructurado.
- [0.4 - Menú Principal - Opciones](./0-4-menu-principal-opciones.md) (Skill `51039`): Lista desplegable de 10 trámites principales.

### 2. Consulta y Derivación de Contratos
- [Consulta Contratos - Inicio](./consulta-contratos-inicio.md) (Skill `51072`): Identificación por contrato o cédula y paginación.
- [Consulta Contratos - Derivación](./consulta-contratos-derivacion.md) (Skill `51073`): Centralita de enrutamiento tras consulta de contrato.

### 3. Saldos, Pagos y Facturación
- [1 - Saldos - Inicio](./1-saldos-inicio.md) (Skill `51051`): Inicializador de saldos.
- [1 - Saldos - Menú](./1-saldos-menu.md) (Skill `51069`): Selección entre consumo de agua o tasa de recolección de basura (TRB).
- [1.1 - Saldos Consumo Agua - Inicio](./1-1-saldos-consumo-agua-inicio.md) (Skill `51071`): Consulta de valores de agua potable.
- [1.1 - Saldos Consumo Agua - Confirmación](./1-1-saldos-consumo-agua-confirmacion.md) (Skill `51070`): Desglose de deuda, lecturas y fecha de vencimiento.
- [1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md) (Skill `51075`): Consulta de tasa de recolección de basura.
- [1.2 - Saldos TRB - Confirmación](./1-2-saldos-trb-confirmacion.md) (Skill `51076`): Detalle de valores emitidos de TRB.
- [3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md) (Skill `51053`): Enlace seguro al portal de pagos en línea.
- [8.0 - Descargar Facturas - Inicio](./8-0-descargar-facturas-inicio.md) (Skill `51058`): Solicitud de descarga de facturas electrónicas.
- [8.1 - Descargar Facturas - Enlace](./8-1-descargar-facturas-enlace.md) (Skill `51193`): Entrega de enlaces web directos al PDF de la planilla.

### 4. Convenios de Pago y Financiación
- [2.0 - Convenio de Pago - Inicio](./2-0-convenio-de-pago-inicio.md) (Skill `51052`): Apertura de solicitud de convenio.
- [2.1 - Convenio de Pago - Contratos](./2-1-convenio-de-pago-contratos.md) (Skill `51099`): Validación de deuda y elegibilidad comercial.
- [2.2 - Convenio de Pago - Porcentaje y Plazo](./2-2-convenio-de-pago-porcentaje-y-plazo.md) (Skill `51094`): Selección de cuota inicial y meses de plazo.
- [2.3 - Convenio de Pago - Simulación](./2-3-convenio-de-pago-simulacion.md) (Skill `51095`): Proyección financiera de cuotas mensuales.
- [2.4 - Convenio de Pago - Genera Convenio](./2-4-convenio-de-pago-genera-convenio.md) (Skill `51096`): Emisión formal y generación del cupón de pago.

### 5. Reclamos Comerciales (Alta Facturación)
- [6.0 - Alta Facturación - Inicio](./6-0-alta-facturacion-inicio.md) (Skill `51056`): Apertura de reclamo por consumos elevados.
- [6.1 - Alta Facturación - Contratos](./6-1-alta-facturacion-contratos.md) (Skill `51090`): Recuperación de cuenta para reclamo.
- [6.2 - Alta Facturación - Facturas](./6-2-alta-facturacion-facturas.md) (Skill `51098`): Selección de la planilla impugnada.
- [6.3 - Alta Facturación - Registrar](./6-3-alta-facturacion-registrar.md) (Skill `51092`): Registro formal del reclamo comercial.
- [6.4 - Alta Facturación - Mensaje Personalizado](./6-4-alta-facturacion-mensaje-personalizado.md) (Skill `51093`): Notificación de confirmación o restricción comercial.

### 6. Averías, Daños en Red e Incidencias Técnicas
- [4.0 - Alcantarillado Sanitario - Inicio](./4-0-alcantarillado-sanitario-inicio.md) (Skill `51054`): Reporte de rebose o taponamiento.
- [4.1 - Alcantarillado Sanitario - Configuración](./4-1-alcantarillado-sanitario-configuracion.md) (Skill `51101`): Detalle técnico del daño sanitario.
- [5.0 - Fugas de Agua - Inicio](./5-0-fugas-de-agua-inicio.md) (Skill `51055`): Reporte de fugas en tuberías matrices o aceras.
- [5.1 - Fugas de Agua - Configuración](./5-1-fugas-de-agua-configuracion.md) (Skill `51102`): Localización y tipo de fuga de agua.
- [10.0 - Aguas Lluvias - Inicio](./10-0-aguas-lluvias-inicio.md) (Skill `51062`): Reporte de inundaciones o sumideros obstruidos.
- [10.1 - Aguas Lluvias - Configuración](./10-1-aguas-lluvias-configuracion.md) (Skill `51109`): Detalle del evento pluvial.
- [11.0 - Faltante de Tapa - Inicio](./11-0-faltante-de-tapa-inicio.md) (Skill `51063`): Alerta de peligro por tapa faltante.
- [11.1 - Faltante de Tapa - Configuración](./11-1-faltante-de-tapa-configuracion.md) (Skill `51110`): Ubicación exacta del pozo sin tapa.
- [7.0 - Afectación Servicio - Inicio](./7-0-afectacion-servicio-inicio.md) (Skill `51057`): Consulta de cortes o baja presión.
- [7.1 - Afectación Servicio - Contratos](./7-1-afectacion-servicio-contratos.md) (Skill `51106`): Cuenta afectada por interrupción.
- [7.2 - Afectación Servicio - Interrupciones](./7-2-afectacion-servicio-interrupciones.md) (Skill `51107`): Consulta de trabajos programados y horas de restablecimiento.
- [Reclamos - Inicio](./reclamos-inicio.md) (Skill `51184`): Menú raíz de reclamos.
- [Reclamos - Menú](./reclamos-menu.md) (Skill `51185`): Clasificación de quejas o incidencias.
- [Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) (Skill `51103`): Entrada general a reclamos técnicos.
- [Reclamo Técnico - Contratos](./reclamo-tecnico-contratos.md) (Skill `51104`): Selección del predio afectado.
- [Reclamo Técnico - Información](./reclamo-tecnico-informacion.md) (Skill `51105`): Dirección, referencias y fotos del daño.
- [Reclamo Técnico - Incidentes](./reclamo-tecnico-incidentes.md) (Skill `51186`): Tipificación de emergencias técnicas.
- [Reclamo Técnico - Quejas](./reclamo-tecnico-quejas.md) (Skill `51188`): Registro de quejas operativas.
- [Reclamo Técnico - Registrar](./reclamo-tecnico-registrar.md) (Skill `51097`): Asignación de orden técnica a cuadrilla.
- [Reclamo Técnico - Mensaje Personalizado](./reclamo-tecnico-mensaje-personalizado.md) (Skill `51187`): Estado de órdenes preexistentes.

### 7. Venta de Servicios Comerciales y Certificados
- [12 - Venta Servicios - Inicio](./12-venta-servicios-inicio.md) (Skill `51077`): Catálogo comercial de servicios especiales.
- [12.1 - Venta Servicios - Servicios](./12-1-venta-servicios-servicios.md) (Skill `51078`): Menú de opciones (pozos sépticos, etc.).
- [12.2 - Venta Servicios - Departamentos](./12-2-venta-servicios-departamentos.md) (Skill `51080`): Cotización y aprobación de tarifas.
- [12.3 - Venta Servicios - Registrar](./12-3-venta-servicios-registrar.md) (Skill `51081`): Generación de orden de trabajo de ingeniería.
- [12.4 - Venta Servicios - Trampa Grasa](./12-4-venta-servicios-trampa-grasa.md) (Skill `51082`): Inspección técnica de trampas de grasa.
- [12.5 - Venta Servicios - Trampa Grasa Registrar](./12-5-venta-servicios-trampa-grasa-registrar.md) (Skill `51083`): Emisión del certificado comercial.
- [13 - Certificados - Inicio](./13-certificados-inicio.md) (Skill `51084`): Emisión de certificados administrativos.
- [13.1 - Certificados - Precio](./13-1-certificados-precio.md) (Skill `51086`): Tarifario de certificados oficiales.
- [13.2 - Certificados - Confirmación](./13-2-certificados-confirmacion.md) (Skill `51088`): Cargo en planilla y emisión.
- [9.0 - Consulta Solicitudes - Inicio](./9-0-consulta-solicitudes-inicio.md) (Skill `51059`): Rastreo de trámites previos.
- [9.1 - Consulta Solicitudes - Solicitudes](./9-1-consulta-solicitudes-solicitudes.md) (Skill `51089`): Estado de trámites en curso.

### 8. Requisitos y Guías de Trámites
- [16 - Consulta Requisitos - Inicio](./16-consulta-requisitos-inicio.md) (Skill `51060`): Menú general de requisitos.
- [16.1 - Requisitos Generales](./16-1-consulta-requisitos-requisitos.md) (Skill `51164`): Documentación básica.
- [16.2 - Servicio de Revisión](./16-2-consulta-requisitos-servicio-revision.md) (Skill `51175`): Inspección de instalaciones.
- [16.3 - Control de Efluentes](./16-3-consulta-requisitos-control-efluentes.md) (Skill `51174`): Descargas no domésticas.
- [16.4 - Certificados Administrativos](./16-4-consulta-requisitos-cert-admin.md) (Skill `51173`): No adeudar e historial.
- [16.5 - Nueva Instalación](./16-5-consulta-requisitos-nueva-instalacion.md) (Skill `51172`): Acometidas de agua y alcantarillado.
- [16.6 - Individualización](./16-6-consulta-requisitos-individualizacion.md) (Skill `51171`): Medidores independientes.
- [16.7 - Certificado de Aprobación](./16-7-consulta-requisitos-cert-aprobacion.md) (Skill `51170`): Planos hidrosanitarios.
- [16.8 - Certificado de Mejora](./16-8-consulta-requisitos-cert-mejora.md) (Skill `51169`): Obras e infraestructura.
- [16.9 - Cambio de Dueño](./16-9-consulta-requisitos-cambio-de-dueno.md) (Skill `51168`): Actualización de titularidad.
- [16.10 - Exoneraciones](./16-10-consulta-requisitos-exoneraciones.md) (Skill `51165`): Tercera edad y discapacidad.
- [16.11 - Cierre de Servicio](./16-11-consulta-requisitos-cierre-servicio.md) (Skill `51166`): Cancelación de acometida.
- [16.12 - Carta Modelo](./16-12-consulta-requisitos-carta-modelo.md) (Skill `51167`): Formato institucional de cartas.
- [15 - Reporte Robo](./15-reporte-robo.md) (Skill `51194`): Denuncia de medidores o accesorios sustraídos.
- [Reconexión](./reconexion.md): Plazos y normativas de restablecimiento.

### 9. Seguridad, Validación y Gestión de Sesión
- [Guardia de Teléfono](./guardia-telefono.md) (Skill `51038`): Verificación y captura interactiva del número de WhatsApp.
- [OTP - Inicio](./otp-inicio.md) (Skill `51079`): Despacho de código de seguridad al correo enmascarado.
- [OTP - Deriva Contexto](./otp-deriva-contexto.md) (Skill `51087`): Derivación post-verificación de código.
- [Por Defecto](./por-defecto.md) (Skill `51033`): Router global para mensajes no entendidos y ayuda humana.
- [Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill `51045`): Caídas técnicas de servicios externos.
- [Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill `51091`): Notificación de reglas de negocio insatisfechas.
- [Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill `51131`): Ventanas de mantenimiento planificado.
- [Operator Not Found](./operator-not-found.md) (Skill `51040`): Cola de asesores llena o indisponible.
- [Operator Out Schedule](./operator-out-schedule.md) (Skill `51041`): Asistencia fuera de horario de oficina.
- [Utility - Opción no disponible](./utility-opcion-no-disponible.md) (Skill `51189`): Manejo de opciones deshabilitadas.
- [Utility - Encuesta Trampa Grasa](./utility-encuesta-trampa-grasa.md) (Skill `51190`): Captura de satisfacción en base de datos.
- [Atención Adicional](./atencion-adicional.md) (Skill `51074`): Pregunta de cierre antes de finalizar.
- [Atención Telefónica](./atencion-telefonica.md) (Skill `51191`): Contactos del Call Center oficial.
- [Ayuda Personalizada](./ayuda-personalizada.md) (Skill `51108`): Asistencia asistida con operador.
- [Adiós](./adios.md) (Skill `51139`): Mensajes aleatorios de despedida.

### 10. Agendamiento, Multimedia y Campañas
- [14 - Agendamiento Citas](./14-agendamiento-citas.md) (Skill `51100`): Citas previas en agencia.
- [Citas](./citas.md): Enlace directo al portal institucional de citas.
- [Video Bot - Inicio](./video-bot-inicio.md) (Skill `51122`): Acceso a formación multimedia.
- [Video Bot](./video-bot.md) (Skill `51061`): Tutoriales de autolectura y fugas internas.
- [Video Bot - Trampa de Grasa](./video-bot-trampa-de-grasa.md) (Skill `51162`): Cápsulas especializadas para comercios.
- [HSM - Guarda mi contacto](./hsm-guarda-mi-contacto.md) (Skill `51192`): Envío de tarjeta de contacto vCard oficial.
- [Pauta Solicitar Limpieza](./pauta-solicitar-limpieza.md) (Skill `51196`): Entrada desde anuncios de redes sociales.

### 11. Entorno de Desarrollo y Pruebas
- [Demo](./demo.md): Demostración de interfaz WebView para IRIS.
- [Dev Gea](./dev-gea.md) (Skill `51387`): Pruebas con agente de Inteligencia Artificial (Azure GPT-4o).
- [Dev Consulta Contratos - Inicio](./dev-consulta-contratos-inicio.md) (Skill `51897`): Consulta de contratos en servidor de desarrollo.
- [Dev Consulta Contratos - Derivación](./dev-consulta-contratos-derivacion.md) (Skill `51898`): Derivación de contratos en desarrollo.
- [Dev 12 - Venta Servicios - Inicio](./dev-12-venta-servicios-inicio.md) (Skill `51899`): Venta de servicios en desarrollo.
- [Dev 12.1 - Venta Servicios - Servicios](./dev-12-1-venta-servicios-servicios.md) (Skill `51900`): Catálogo de servicios en desarrollo.
- [Dev 12.2 - Venta Servicios - Departamentos](./dev-12-2-venta-servicios-departamentos.md) (Skill `51901`): Cotización y tarifas en desarrollo.
- [Dev 12.3 - Venta Servicios - Registrar](./dev-12-3-venta-servicios-registrar.md) (Skill `51902`): Registro de órdenes en desarrollo.

---

## 📋 Tabla Completa de los 103 Flujos

| # | Nombre del Flujo | Identificador Técnico (Slug) | Skill ID | Archivo de Documentación |
|---|------------------|------------------------------|----------|--------------------------|
| 1 | _ 0.1 - LOPD - Inicio | `0-1-lopd-inicio` | 51066 | [0-1-lopd-inicio.md](./0-1-lopd-inicio.md) |
| 2 | _ 0.2 - LOPD - Deriva Contexto | `0-2-lopd-deriva-contexto` | 51067 | [0-2-lopd-deriva-contexto.md](./0-2-lopd-deriva-contexto.md) |
| 3 | _ 0.3 - Menú Principal | `0-3-menu-principal` | 51068 | [0-3-menu-principal.md](./0-3-menu-principal.md) |
| 4 | _ 0.3 - Menú Principal - Bienvenida | `0-3-menu-principal-bienvenida` | 51161 | [0-3-menu-principal-bienvenida.md](./0-3-menu-principal-bienvenida.md) |
| 5 | _ 0.4 - Menú Principal - Opciones | `0-4-menu-principal-opciones` | 51039 | [0-4-menu-principal-opciones.md](./0-4-menu-principal-opciones.md) |
| 6 | _ 0 - Inicio | `0-inicio` | 51065 | [0-inicio.md](./0-inicio.md) |
| 7 | _ 1.1 - Saldos Consumo Agua - Confirmación | `1-1-saldos-consumo-agua-confirmacion` | 51070 | [1-1-saldos-consumo-agua-confirmacion.md](./1-1-saldos-consumo-agua-confirmacion.md) |
| 8 | _ 1.1 - Saldos Consumo Agua - Inicio | `1-1-saldos-consumo-agua-inicio` | 51071 | [1-1-saldos-consumo-agua-inicio.md](./1-1-saldos-consumo-agua-inicio.md) |
| 9 | _ 1.2 - Saldos TRB - Confirmación | `1-2-saldos-trb-confirmacion` | 51076 | [1-2-saldos-trb-confirmacion.md](./1-2-saldos-trb-confirmacion.md) |
| 10 | _ 1.2 - Saldos TRB - Inicio | `1-2-saldos-trb-inicio` | 51075 | [1-2-saldos-trb-inicio.md](./1-2-saldos-trb-inicio.md) |
| 11 | _ 1 - Saldos - Inicio | `1-saldos-inicio` | 51051 | [1-saldos-inicio.md](./1-saldos-inicio.md) |
| 12 | _ 1 - Saldos - Menu | `1-saldos-menu` | 51069 | [1-saldos-menu.md](./1-saldos-menu.md) |
| 13 | _ 10.0 - Aguas lluvias - Inicio | `10-0-aguas-lluvias-inicio` | 51062 | [10-0-aguas-lluvias-inicio.md](./10-0-aguas-lluvias-inicio.md) |
| 14 | _ 10.1 - Aguas lluvias - Configuración | `10-1-aguas-lluvias-configuracion` | 51109 | [10-1-aguas-lluvias-configuracion.md](./10-1-aguas-lluvias-configuracion.md) |
| 15 | _ 11.0 - Faltante de Tapa - Inicio | `11-0-faltante-de-tapa-inicio` | 51063 | [11-0-faltante-de-tapa-inicio.md](./11-0-faltante-de-tapa-inicio.md) |
| 16 | _ 11.1 - Faltante de Tapa - Configuración | `11-1-faltante-de-tapa-configuracion` | 51110 | [11-1-faltante-de-tapa-configuracion.md](./11-1-faltante-de-tapa-configuracion.md) |
| 17 | _ 12.1 - Venta Servicios - Servicios | `12-1-venta-servicios-servicios` | 51078 | [12-1-venta-servicios-servicios.md](./12-1-venta-servicios-servicios.md) |
| 18 | _ 12.2 - Venta Servicios - Departamentos | `12-2-venta-servicios-departamentos` | 51080 | [12-2-venta-servicios-departamentos.md](./12-2-venta-servicios-departamentos.md) |
| 19 | _ 12.3 - Venta Servicios - Registrar | `12-3-venta-servicios-registrar` | 51081 | [12-3-venta-servicios-registrar.md](./12-3-venta-servicios-registrar.md) |
| 20 | _ 12.4 - Venta Servicios - Trampa Grasa | `12-4-venta-servicios-trampa-grasa` | 51082 | [12-4-venta-servicios-trampa-grasa.md](./12-4-venta-servicios-trampa-grasa.md) |
| 21 | _ 12.5 - Venta Servicios - Trampa Grasa Registrar | `12-5-venta-servicios-trampa-grasa-registrar` | 51083 | [12-5-venta-servicios-trampa-grasa-registrar.md](./12-5-venta-servicios-trampa-grasa-registrar.md) |
| 22 | _ 12 - Venta Servicios - Inicio | `12-venta-servicios-inicio` | 51077 | [12-venta-servicios-inicio.md](./12-venta-servicios-inicio.md) |
| 23 | _ 13.1 - Certificados - Precio | `13-1-certificados-precio` | 51086 | [13-1-certificados-precio.md](./13-1-certificados-precio.md) |
| 24 | _ 13.2 - Certificados - Confirmación | `13-2-certificados-confirmacion` | 51088 | [13-2-certificados-confirmacion.md](./13-2-certificados-confirmacion.md) |
| 25 | _ 13 - Certificados - Inicio | `13-certificados-inicio` | 51084 | [13-certificados-inicio.md](./13-certificados-inicio.md) |
| 26 | _ 14 Agendamiento Citas | `14-agendamiento-citas` | 51100 | [14-agendamiento-citas.md](./14-agendamiento-citas.md) |
| 27 | 15 - Reporte Robo | `15-reporte-robo` | 51194 | [15-reporte-robo.md](./15-reporte-robo.md) |
| 28 | _ 16.1 - Consulta Requisitos - Requisitos | `16-1-consulta-requisitos-requisitos` | 51164 | [16-1-consulta-requisitos-requisitos.md](./16-1-consulta-requisitos-requisitos.md) |
| 29 | _ 16.10 - Consulta Requisitos - Exoneraciones | `16-10-consulta-requisitos-exoneraciones` | 51165 | [16-10-consulta-requisitos-exoneraciones.md](./16-10-consulta-requisitos-exoneraciones.md) |
| 30 | _ 16.11 - Consulta Requisitos - Cierre Servicio | `16-11-consulta-requisitos-cierre-servicio` | 51166 | [16-11-consulta-requisitos-cierre-servicio.md](./16-11-consulta-requisitos-cierre-servicio.md) |
| 31 | _ 16.12 - Consulta Requisitos - Carta Modelo | `16-12-consulta-requisitos-carta-modelo` | 51167 | [16-12-consulta-requisitos-carta-modelo.md](./16-12-consulta-requisitos-carta-modelo.md) |
| 32 | _ 16.2 - Consulta Requisitos - Servicio Revision | `16-2-consulta-requisitos-servicio-revision` | 51175 | [16-2-consulta-requisitos-servicio-revision.md](./16-2-consulta-requisitos-servicio-revision.md) |
| 33 | _ 16.3 - Consulta Requisitos - Control Efluentes | `16-3-consulta-requisitos-control-efluentes` | 51174 | [16-3-consulta-requisitos-control-efluentes.md](./16-3-consulta-requisitos-control-efluentes.md) |
| 34 | _ 16.4 - Consulta Requisitos - Cert. Admin. | `16-4-consulta-requisitos-cert-admin` | 51173 | [16-4-consulta-requisitos-cert-admin.md](./16-4-consulta-requisitos-cert-admin.md) |
| 35 | _ 16.5 - Consulta Requisitos - Nueva Instalacion | `16-5-consulta-requisitos-nueva-instalacion` | 51172 | [16-5-consulta-requisitos-nueva-instalacion.md](./16-5-consulta-requisitos-nueva-instalacion.md) |
| 36 | _ 16.6 - Consulta Requisitos - Individualizacion | `16-6-consulta-requisitos-individualizacion` | 51171 | [16-6-consulta-requisitos-individualizacion.md](./16-6-consulta-requisitos-individualizacion.md) |
| 37 | _ 16.7 - Consulta Requisitos - Cert. Aprobacion | `16-7-consulta-requisitos-cert-aprobacion` | 51170 | [16-7-consulta-requisitos-cert-aprobacion.md](./16-7-consulta-requisitos-cert-aprobacion.md) |
| 38 | _ 16.8 - Consulta Requisitos - Cert. Mejora | `16-8-consulta-requisitos-cert-mejora` | 51169 | [16-8-consulta-requisitos-cert-mejora.md](./16-8-consulta-requisitos-cert-mejora.md) |
| 39 | _ 16.9 - Consulta Requisitos - Cambio de Dueno | `16-9-consulta-requisitos-cambio-de-dueno` | 51168 | [16-9-consulta-requisitos-cambio-de-dueno.md](./16-9-consulta-requisitos-cambio-de-dueno.md) |
| 40 | _ 16 - Consulta Requisitos - Inicio | `16-consulta-requisitos-inicio` | 51060 | [16-consulta-requisitos-inicio.md](./16-consulta-requisitos-inicio.md) |
| 41 | _ 2.0 - Convenio de Pago - Inicio | `2-0-convenio-de-pago-inicio` | 51052 | [2-0-convenio-de-pago-inicio.md](./2-0-convenio-de-pago-inicio.md) |
| 42 | _ 2.1 - Convenio de Pago - Contratos | `2-1-convenio-de-pago-contratos` | 51099 | [2-1-convenio-de-pago-contratos.md](./2-1-convenio-de-pago-contratos.md) |
| 43 | _ 2.2 - Convenio de Pago - Porcentaje y Plazo | `2-2-convenio-de-pago-porcentaje-y-plazo` | 51094 | [2-2-convenio-de-pago-porcentaje-y-plazo.md](./2-2-convenio-de-pago-porcentaje-y-plazo.md) |
| 44 | _ 2.3 - Convenio de Pago - Simulación | `2-3-convenio-de-pago-simulacion` | 51095 | [2-3-convenio-de-pago-simulacion.md](./2-3-convenio-de-pago-simulacion.md) |
| 45 | _ 2.4 - Convenio de Pago - Genera Convenio | `2-4-convenio-de-pago-genera-convenio` | 51096 | [2-4-convenio-de-pago-genera-convenio.md](./2-4-convenio-de-pago-genera-convenio.md) |
| 46 | _ 3 - Pago de Planilla - Inicio | `3-pago-de-planilla-inicio` | 51053 | [3-pago-de-planilla-inicio.md](./3-pago-de-planilla-inicio.md) |
| 47 | _ 4.0 - Alcantarillado Sanitario - Inicio | `4-0-alcantarillado-sanitario-inicio` | 51054 | [4-0-alcantarillado-sanitario-inicio.md](./4-0-alcantarillado-sanitario-inicio.md) |
| 48 | _ 4.1 - Alcantarillado Sanitario - Configuración | `4-1-alcantarillado-sanitario-configuracion` | 51101 | [4-1-alcantarillado-sanitario-configuracion.md](./4-1-alcantarillado-sanitario-configuracion.md) |
| 49 | _ 5.0 - Fugas de Agua - Inicio | `5-0-fugas-de-agua-inicio` | 51055 | [5-0-fugas-de-agua-inicio.md](./5-0-fugas-de-agua-inicio.md) |
| 50 | _ 5.1 - Fugas de Agua - Configuración | `5-1-fugas-de-agua-configuracion` | 51102 | [5-1-fugas-de-agua-configuracion.md](./5-1-fugas-de-agua-configuracion.md) |
| 51 | _ 6.0 - Alta Facturación - Inicio | `6-0-alta-facturacion-inicio` | 51056 | [6-0-alta-facturacion-inicio.md](./6-0-alta-facturacion-inicio.md) |
| 52 | _ 6.1 - Alta Facturación - Contratos | `6-1-alta-facturacion-contratos` | 51090 | [6-1-alta-facturacion-contratos.md](./6-1-alta-facturacion-contratos.md) |
| 53 | _ 6.2 - Alta Facturación - Facturas | `6-2-alta-facturacion-facturas` | 51098 | [6-2-alta-facturacion-facturas.md](./6-2-alta-facturacion-facturas.md) |
| 54 | _ 6.3 - Alta Facturación - Registrar | `6-3-alta-facturacion-registrar` | 51092 | [6-3-alta-facturacion-registrar.md](./6-3-alta-facturacion-registrar.md) |
| 55 | _ 6.4 - Alta Facturación - Mensaje Personalizado | `6-4-alta-facturacion-mensaje-personalizado` | 51093 | [6-4-alta-facturacion-mensaje-personalizado.md](./6-4-alta-facturacion-mensaje-personalizado.md) |
| 56 | _ 7.0 - Afectación Servicio - Inicio | `7-0-afectacion-servicio-inicio` | 51057 | [7-0-afectacion-servicio-inicio.md](./7-0-afectacion-servicio-inicio.md) |
| 57 | _ 7.1 - Afectación Servicio - Contratos | `7-1-afectacion-servicio-contratos` | 51106 | [7-1-afectacion-servicio-contratos.md](./7-1-afectacion-servicio-contratos.md) |
| 58 | _ 7.2 - Afectación Servicio - Interrupciones | `7-2-afectacion-servicio-interrupciones` | 51107 | [7-2-afectacion-servicio-interrupciones.md](./7-2-afectacion-servicio-interrupciones.md) |
| 59 | _ 8.0 - Descargar Facturas - Inicio | `8-0-descargar-facturas-inicio` | 51058 | [8-0-descargar-facturas-inicio.md](./8-0-descargar-facturas-inicio.md) |
| 60 | 8.1 - Descargar Facturas - Enlace | `8-1-descargar-facturas-enlace` | 51193 | [8-1-descargar-facturas-enlace.md](./8-1-descargar-facturas-enlace.md) |
| 61 | _ 9.0 - Consulta Solicitudes - Inicio | `9-0-consulta-solicitudes-inicio` | 51059 | [9-0-consulta-solicitudes-inicio.md](./9-0-consulta-solicitudes-inicio.md) |
| 62 | _ 9.1 - Consulta Solicitudes - Solicitudes | `9-1-consulta-solicitudes-solicitudes` | 51089 | [9-1-consulta-solicitudes-solicitudes.md](./9-1-consulta-solicitudes-solicitudes.md) |
| 63 | _ Adiós | `adios` | 51139 | [adios.md](./adios.md) |
| 64 | _ Atención Adicional | `atencion-adicional` | 51074 | [atencion-adicional.md](./atencion-adicional.md) |
| 65 | Atención telefónica | `atencion-telefonica` | 51191 | [atencion-telefonica.md](./atencion-telefonica.md) |
| 66 | _ Ayuda Personalizada | `ayuda-personalizada` | 51108 | [ayuda-personalizada.md](./ayuda-personalizada.md) |
| 67 | Citas | `citas` | N/A | [citas.md](./citas.md) |
| 68 | _ Consulta Contratos - Derivación | `consulta-contratos-derivacion` | 51073 | [consulta-contratos-derivacion.md](./consulta-contratos-derivacion.md) |
| 69 | _ Consulta Contratos - Inicio | `consulta-contratos-inicio` | 51072 | [consulta-contratos-inicio.md](./consulta-contratos-inicio.md) |
| 70 | Demo | `demo` | N/A | [demo.md](./demo.md) |
| 71 | _ Dev 12.1 - Venta Servicios - Servicios | `dev-12-1-venta-servicios-servicios` | 51900 | [dev-12-1-venta-servicios-servicios.md](./dev-12-1-venta-servicios-servicios.md) |
| 72 | _ Dev 12.2 - Venta Servicios - Departamentos | `dev-12-2-venta-servicios-departamentos` | 51901 | [dev-12-2-venta-servicios-departamentos.md](./dev-12-2-venta-servicios-departamentos.md) |
| 73 | _ Dev 12.3 - Venta Servicios - Registrar | `dev-12-3-venta-servicios-registrar` | 51902 | [dev-12-3-venta-servicios-registrar.md](./dev-12-3-venta-servicios-registrar.md) |
| 74 | _ Dev 12 - Venta Servicios - Inicio | `dev-12-venta-servicios-inicio` | 51899 | [dev-12-venta-servicios-inicio.md](./dev-12-venta-servicios-inicio.md) |
| 75 | _ Dev Consulta Contratos - Derivación | `dev-consulta-contratos-derivacion` | 51898 | [dev-consulta-contratos-derivacion.md](./dev-consulta-contratos-derivacion.md) |
| 76 | _ Dev Consulta Contratos - Inicio | `dev-consulta-contratos-inicio` | 51897 | [dev-consulta-contratos-inicio.md](./dev-consulta-contratos-inicio.md) |
| 77 | _ Dev Gea | `dev-gea` | 51387 | [dev-gea.md](./dev-gea.md) |
| 78 | _ Guardia Teléfono | `guardia-telefono` | 51038 | [guardia-telefono.md](./guardia-telefono.md) |
| 79 | HSM - Guarda mi contacto | `hsm-guarda-mi-contacto` | 51192 | [hsm-guarda-mi-contacto.md](./hsm-guarda-mi-contacto.md) |
| 80 | _ Mensaje Error Personalizado | `mensaje-error-personalizado` | 51091 | [mensaje-error-personalizado.md](./mensaje-error-personalizado.md) |
| 81 | _ Mensaje Error Servicio | `mensaje-error-servicio` | 51045 | [mensaje-error-servicio.md](./mensaje-error-servicio.md) |
| 82 | _ Mensaje Mantenimiento | `mensaje-mantenimiento` | 51131 | [mensaje-mantenimiento.md](./mensaje-mantenimiento.md) |
| 83 | _ Operator Not Found | `operator-not-found` | 51040 | [operator-not-found.md](./operator-not-found.md) |
| 84 | _ Operator Out Schedule | `operator-out-schedule` | 51041 | [operator-out-schedule.md](./operator-out-schedule.md) |
| 85 | _ OTP - Deriva Contexto | `otp-deriva-contexto` | 51087 | [otp-deriva-contexto.md](./otp-deriva-contexto.md) |
| 86 | _ OTP - Inicio | `otp-inicio` | 51079 | [otp-inicio.md](./otp-inicio.md) |
| 87 | Pauta solicitar limpieza | `pauta-solicitar-limpieza` | 51196 | [pauta-solicitar-limpieza.md](./pauta-solicitar-limpieza.md) |
| 88 | Por defecto | `por-defecto` | 51033 | [por-defecto.md](./por-defecto.md) |
| 89 | _ Reclamo Técnico - Contratos | `reclamo-tecnico-contratos` | 51104 | [reclamo-tecnico-contratos.md](./reclamo-tecnico-contratos.md) |
| 90 | Reclamo Técnico - Incidentes | `reclamo-tecnico-incidentes` | 51186 | [reclamo-tecnico-incidentes.md](./reclamo-tecnico-incidentes.md) |
| 91 | _ Reclamo Técnico - Información | `reclamo-tecnico-informacion` | 51105 | [reclamo-tecnico-informacion.md](./reclamo-tecnico-informacion.md) |
| 92 | _ Reclamo Técnico - Inicio | `reclamo-tecnico-inicio` | 51103 | [reclamo-tecnico-inicio.md](./reclamo-tecnico-inicio.md) |
| 93 | Reclamo Técnico - Mensaje Personalizado | `reclamo-tecnico-mensaje-personalizado` | 51187 | [reclamo-tecnico-mensaje-personalizado.md](./reclamo-tecnico-mensaje-personalizado.md) |
| 94 | Reclamo Técnico - Quejas | `reclamo-tecnico-quejas` | 51188 | [reclamo-tecnico-quejas.md](./reclamo-tecnico-quejas.md) |
| 95 | _ Reclamo Técnico - Registrar | `reclamo-tecnico-registrar` | 51097 | [reclamo-tecnico-registrar.md](./reclamo-tecnico-registrar.md) |
| 96 | Reclamos - Inicio | `reclamos-inicio` | 51184 | [reclamos-inicio.md](./reclamos-inicio.md) |
| 97 | Reclamos - Menu | `reclamos-menu` | 51185 | [reclamos-menu.md](./reclamos-menu.md) |
| 98 | Reconexión | `reconexion` | N/A | [reconexion.md](./reconexion.md) |
| 99 | Utility - Encuesta Trampa Grasa | `utility-encuesta-trampa-grasa` | 51190 | [utility-encuesta-trampa-grasa.md](./utility-encuesta-trampa-grasa.md) |
| 100 | Utility - Opción no disponible | `utility-opcion-no-disponible` | 51189 | [utility-opcion-no-disponible.md](./utility-opcion-no-disponible.md) |
| 101 | _ Video bot | `video-bot` | 51061 | [video-bot.md](./video-bot.md) |
| 102 | _ Video Bot - Inicio | `video-bot-inicio` | 51122 | [video-bot-inicio.md](./video-bot-inicio.md) |
| 103 | _ Video Bot - Trampa de Grasa | `video-bot-trampa-de-grasa` | 51162 | [video-bot-trampa-de-grasa.md](./video-bot-trampa-de-grasa.md) |
