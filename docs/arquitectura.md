# Arquitectura del sistema

## Propósito

Brudi Notas es una automatización diseñada para convertir audios enviados por Telegram en documentos útiles, estructurados y presentables. El objetivo es transformar notas de voz, clases, juntas o grabaciones largas en salidas como transcripciones limpias, resúmenes, minutas o reportes en formato PDF o Word.

El sistema busca ofrecer una experiencia sencilla para el usuario final: enviar un audio, conocer el costo según su duración, completar el pago y recibir un documento final bien organizado.

## Flujo general del usuario

1. El usuario envía un archivo de audio al bot de Telegram.
2. El sistema detecta la duración del archivo y estima el costo del servicio.
3. El usuario realiza la compra de créditos o utiliza los disponibles.
4. Una vez confirmado el pago, se inicia el procesamiento del audio.
5. El audio se convierte a texto usando la API de ChatGPT.
6. El texto obtenido se reorganiza según el formato de salida deseado por el usuario.
7. Se genera un documento final en PDF o Word.
8. El archivo final se entrega al usuario a través de Telegram.

## Arquitectura funcional

La automatización se organiza en los siguientes módulos:

### 1. Módulo de entrada
Recibe el archivo enviado por el usuario y obtiene metadatos relevantes como nombre, formato, peso y duración.

### 2. Módulo de cotización
Calcula el precio estimado con base en la duración del audio y la lógica de créditos definida en el sistema.

### 3. Módulo de validación de pago
Verifica si el usuario cuenta con créditos suficientes o si debe completar un pago mediante Mercado Pago, integrando la API dentro de la automatización.

### 4. Módulo de transcripción
Envía el audio a la API de ChatGPT para convertirlo en texto.

### 5. Módulo de estructuración
Se usa la APÏ de ChatGPT para reorganizar la transcripción en el formato solicitado por el usuario, como resumen, minuta, notas o reporte.

### 6. Módulo de generación de documento
Inserta el contenido procesado dentro de una plantilla predefinida y genera el archivo final en PDF o Word.

### 7. Módulo de entrega
Devuelve el archivo final directamente al usuario mediante Telegram.

## Lógica de negocio

El modelo de cobro se basa en créditos medidos en minutos de audio procesable.

### Paquetes propuestos
- 60 minutos por $29 MXN
- 120 minutos por $59 MXN
- 180 minutos por $79 MXN
- 300 minutos por $129 MXN

Antes de iniciar el procesamiento, el sistema valida que el usuario tenga saldo suficiente o que el pago del paquete de créditos haya sido confirmado correctamente.

## Estrategia de procesamiento de audio

Para manejar distintos tipos de carga de trabajo, se proponen varias rutas de procesamiento:

- Duración menor a 20 minutos: procesamiento directo.
- Duración entre 20 y 90 minutos: procesamiento por fragmentos.
- Duración mayor a 90 minutos: procesamiento especial.

Esta estrategia busca optimizar costos, estabilidad y experiencia de uso, evitando imponer límites estrictos como diferenciador del servicio.

## Consideraciones de experiencia de usuario

El sistema busca mantener mensajes positivos y claros durante todo el proceso, evitando respuestas innecesariamente restrictivas o ambiguas.

También se propone ofrecer opciones de estilo tanto para el contenido del audio como para el diseño del documento final. Para reducir consumo de tokens y mantener consistencia visual, se contempla el uso de formatos predefinidos mediante plantillas HTML o CSS.
