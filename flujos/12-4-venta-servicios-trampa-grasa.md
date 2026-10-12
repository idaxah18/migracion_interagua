# 12.4 - Venta Servicios - Trampa Grasa

Captura y valida la información legal y operativa del local comercial para la inspección y control de trampas de grasa. Determina la titularidad del inmueble, recopila los datos del propietario y del establecimiento (RUC, nombre comercial y número de local), validando estrictamente los formatos numéricos y alfanuméricos mediante expresiones regulares.

## Datos que debe traer al entrar

- `contexto`: `venta_servicios`.
- `is_trampa_grasa`: `true`.
- Contrato de agua potable e identificación del solicitante.

## Paso 1. Determinación de titularidad del contrato

El bot consulta la relación del usuario con el inmueble:

> ¿Usted es el Dueño del contrato de agua potable?

Opciones con botones:
1. **Sí:** Procede directamente a solicitar el RUC del establecimiento (Paso 2B).
2. **No:** Solicita los datos del titular o propietario previo a la información comercial (Paso 2A).

## Paso 2A. Captura de datos del titular (en caso de no ser el dueño)

1. **Nombre del propietario:**
   - Expresión regular: `^[a-zA-ZáéíóúÁÉÍÓÚñÑ0-9\s.,;:()\-_/°"#%&¡!¿?@]+$`
   - Si no cumple el formato:
     > El nombre no debe contener números ni caracteres especiales. Por favor, inténtalo nuevamente.
   - Variable: `nombre_propietario`.

2. **Cédula del propietario:**
   - Expresión regular: `^\d{10}$` (exactamente 10 dígitos numéricos).
   - Si no cumple:
     > La identificación debe contener únicamente 10 dígitos. Por favor, inténtalo nuevamente.
   - Variable: `identificacion_propietario`.

## Paso 2B. Captura de datos del establecimiento comercial

1. **RUC del establecimiento:**
   - Expresión regular: `^\d{13}$` (exactamente 13 dígitos numéricos).
   - Si no cumple:
     > El RUC debe contener únicamente 13 dígitos. Por favor, inténtalo nuevamente.
   - Variable: `ruc_propietario`.

2. **Nombre comercial del establecimiento:**
   - Variable: `nombre_establecimiento`.

3. **Número de establecimiento / local:**
   - Expresión regular: `^\d+$` (dígitos numéricos).
   - Si no cumple:
     > El número del establecimiento deben ser dígitos. Por favor, inténtalo nuevamente.
   - Variable: `numero_establecimiento`.

## Salida

- **Continuación normal:** Transfiere a [_ 12.2 - Venta Servicios - Departamentos](./12-2-venta-servicios-departamentos.md) (Skill ID `51080`) para calcular la tarifa correspondiente a la inspección comercial.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
