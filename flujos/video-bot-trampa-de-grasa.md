# Video Bot - Trampa de Grasa

Este flujo está especialmente concebido para propietarios de restaurantes, cafeterías, locales de expendio de alimentos y actividades comerciales afines en Guayaquil. Mediante cápsulas audiovisuales y explicaciones paso a paso, los guía sobre la correcta instalación y limpieza periódica de su trampa de grasa, las normativas ambientales de descarga de efluentes, los costos del proceso de inspección y cómo obtener el Certificado de Trampa de Grasa exigido por Interagua y el Municipio.

## Datos que debe traer al entrar

- `contexto`: Identificador del trámite (`video_asistencia` o `trampa_grasa`).
- Consentimiento de tratamiento de datos personales aceptado.

## Contenido audiovisual y formativo

El recorrido formativo acompaña al comerciante a través de las siguientes etapas clave:

---

### Etapa 1. Mantenimiento y prevención en trampas de grasa

Entrega videos tutoriales sobre:
- Funcionamiento hidromecánico de la trampa de grasa.
- Frecuencia recomendada de limpieza y retiro de sedimentos y grasas acumuladas.
- Disposición adecuada de los residuos para evitar el taponamiento del alcantarillado sanitario de la ciudad.

---

### Etapa 2. Proceso para obtener el Certificado de Trampa de Grasa

Una vez revisadas las recomendaciones de mantenimiento, el bot presenta la guía para la obtención formal del certificado:

> Ahora que conoces cómo mantener y cuidar tu trampa de grasa, es hora de saber cómo obtener el certificado. Te explicaremos los pasos a seguir para obtener el certificado, los costos involucrados y los controles que realiza la empresa en la descarga de efluentes para asegurar el cumplimiento de las normativas. ¡Vamos a detallarlo para que puedas completar todo el proceso correctamente!

A continuación, remite las cápsulas en video detallando los costos de inspección técnica de ingeniería y los análisis de efluentes.

---

### Etapa 3. Finalización de la formación y solicitud

Al aproximarse a la conclusión de los módulos multimedia, el bot consulta al usuario mediante botones interactivos:

> ¡Estás a un paso de completar el recorrido! Ahora que has avanzado hasta aquí, es momento de finalizar con la última sección, donde conoceremos cómo obetener tu certificado de forma sencilla. ¿Listo para completar la formación?

El cliente puede decidir continuar para iniciar el trámite de inspección, revisar nuevamente los videos o agendar una cita.

---

### Canales complementarios de atención

- **Atención por videollamada personalizada:**
  El bot brinda el enlace directo al portal de citas previas virtuales de Interagua:
  > Puedes ingresar al siguiente enlace para ser atendido por video llamada.  
  > https://citasprevias.interagua.com.ec/

- **Conexión con un asesor especializado:**
  Si el comerciante requiere asistencia directa, el bot activa el intento de conexión humana (`JelouConnect`):
  > Permíteme comunicarte con un asesor técnico para resolver tus inquietudes sobre tu trampa de grasa.

## Salidas del flujo

- **Retorno al menú principal:** En cualquier etapa, el cliente puede volver a [0.3 - Menú Principal - Bienvenida](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-3-menu-principal-bienvenida.md) (Skill `51161`).
- **Inicio de solicitud comercial:** Puede derivar al flujo de registro en [12.4 - Venta Servicios - Trampa Grasa](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/12-4-venta-servicios-trampa-grasa.md) (Skill `51082`).
