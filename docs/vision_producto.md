# Visión del producto y modelo de negocio

## Idea base y problemática a resolver

Se propone el desarrollo de un bot automatizado capaz de transcribir audios y convertirlos en notas, informes o minutas de forma rápida y accesible.

Aunque actualmente existen herramientas gratuitas que realizan la transcripción de audio a texto, el diferenciador principal de este proyecto es que no se entrega una transcripción cruda, sino un contenido estructurado y adaptado al tipo de audio enviado.

El objetivo es ofrecer formatos de salida personalizados según el contexto del audio, por ejemplo:

- **Audio de una clase:**  
  Se genera un resumen estructurado que incluye temas centrales, puntos clave, fechas importantes, tareas mencionadas y posibles referencias para profundizar en el tema. El formato está diseñado para ser óptimo para el estudio, priorizando claridad, organización y retención de información.

- **Audio de una conferencia:**  
  Se genera un informe con los puntos más relevantes, ideas principales y conceptos clave. Este formato está orientado a facilitar la comunicación de lo aprendido a terceros, por ejemplo, dentro de un entorno laboral.

- **Audio de una sesión informativa:**  
  Se entrega un resumen enfocado en datos importantes como fechas, enlaces, requisitos o documentos solicitados. El objetivo es que pueda utilizarse como un aviso claro y práctico.

- **Audio de reuniones laborales:**  
  Se genera una minuta estructurada que incluye problemáticas discutidas, soluciones propuestas, acuerdos alcanzados y decisiones tomadas (ya sea de forma unánime o mayoritaria). Este formato busca facilitar la documentación de reuniones y reducir la carga de trabajo manual.

En todos los casos, el sistema busca ofrecer una salida clara, profesional y útil, eliminando la necesidad de que el usuario edite o interprete la transcripción por su cuenta.

---

## Modelo de negocio

A pesar de la existencia de herramientas que permiten la transcripción de audio a texto, muchas operan bajo modelos de suscripción mensual o tienen costos elevados. Por otro lado, las opciones gratuitas suelen tener limitaciones significativas, como restricciones de duración o número de archivos.

Este proyecto busca ofrecer una alternativa:

- accesible en precio  
- fácil de usar  
- sin fricción técnica para el usuario  

La propuesta es implementar un bot en Telegram (con posible expansión a WhatsApp) que permita al usuario simplemente enviar un audio y recibir, en cuestión de minutos, un documento en formato PDF o Word con la información estructurada.

El valor principal del producto radica en:
- la simplicidad de uso (interfaz tipo chat)
- la automatización completa del proceso
- la calidad del resultado final

---

## Créditos y pagos

Debido a los costos asociados al uso de APIs (como OpenAI), la infraestructura de automatización (n8n) y las comisiones de pago (Mercado Pago), se implementa un modelo basado en créditos.

El usuario adquiere paquetes de minutos disponibles que puede consumir conforme utiliza el servicio.

### Paquetes propuestos

- 60 minutos por $29 MXN  
- 120 minutos por $59 MXN  
- 180 minutos por $79 MXN  
- 300 minutos por $129 MXN  

Como punto de partida, se plantea inicialmente un formato general que pueda cubrir los distintos casos de uso mencionados. Sin embargo, conforme evolucione el producto y para mejorar la calidad del servicio, se contempla diferenciar los formatos de salida y ajustar los costos de acuerdo con su complejidad de procesamiento.

### Funcionamiento

1. El usuario envía un audio y define el tipo de formato deseado.
2. El sistema calcula la duración del audio y estima el costo en minutos.
3. Se muestra al usuario:
   - minutos disponibles  
   - minutos que se descontarán  
4. Si no cuenta con saldo suficiente, se le ofrece adquirir un paquete.
5. Una vez confirmado el pago o validados los créditos, se procesa el audio.

Para evitar el uso ineficiente del sistema, se establece un consumo mínimo de 5 minutos por audio, independientemente de su duración real.

Este modelo permite:
- mantener costos accesibles  
- simplificar la experiencia del usuario  
- garantizar la sostenibilidad del servicio  
