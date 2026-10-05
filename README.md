# Proyecto Backend III - Exp3_S8_Grupo2

## 1. Contexto y Objetivos de la Evaluación (Semana 8)
Este proyecto corresponde a la evaluación sumativa de la **Semana 8 (Exp 3)** de la asignatura **Desarrollo Backend III (PBY2203)**[cite: 2, 3]. El objetivo principal es consolidar una arquitectura de microservicios en la nube altamente resiliente, segura y orientada a eventos, integrando los siguientes componentes clave requeridos por la pauta:
* **Seguridad Avanzada:** Implementación del protocolo OAuth 2.0 mediante *Resource Servers* protegidos con JSON Web Tokens (JWT) para asegurar la infraestructura distribuida[cite: 2, 3].
* **Contenedorización y Cloud:** Creación de imágenes Docker funcionales para cada microservicio y su respectiva orquestación automatizada.
* **Tolerancia a Fallos:** Incorporación de Resilience4j para garantizar la estabilidad del sistema ante interrupciones[cite: 2].
* **Mensajería Asíncrona:** Integración con Apache Kafka para habilitar una comunicación orientada a eventos eficiente y scalable[cite: 2, 3].

## 2. Definición de la Arquitectura de Eventos
* **Patrón Seleccionado:** Se implementa una Arquitectura Orientada a Eventos (EDA) utilizando el patrón de Publicación-Suscripción (Pub/Sub) mediante coreografía, donde los microservicios reaccionan de manera desacoplada a los cambios de estado.
* **Justificación:** Este enfoque permite que las transacciones de transferencias se procesen de forma asíncrona, garantizando escalabilidad y tolerancia a fallos ante interrupciones temporales en la red o en los servicios.

## 3. Diagrama de la Solución y Flujo de Eventos
* **Flujo Asíncrono:**
    1. El cliente inicia una transacción en el Banco API (Producer).
    2. El microservicio publica el evento `TRANSFERENCIA_REALIZADA` estructurado en formato JSON.
    3. El mensaje viaja hacia el broker de Apache Kafka a través del tópico `banco-xyz.transferencias` (configurado con 3 particiones para distribución de carga).
    4. El Cliente API (Consumer) lee y procesa de forma asíncrona el evento validando el eventId, cuentas, monto y moneda.

## 4. Estructura de Seguridad (OAuth 2.0) y Resiliencia
* **OAuth 2.0 Resource Server:** Los microservicios de negocio (`Banco API` y `Cliente API`) configuran validación de seguridad por medio de tokens JWT utilizando una clave simétrica compartida en sus propiedades[cite: 2, 3].
* **Resilience4j:** Se incorporan mecanismos de resiliencia (Circuit Breakers) para proteger la disponibilidad del sistema ante fallos de conectividad en las peticiones sincrónicas y asíncronas[cite: 2].
* **Apache Kafka & Docker:** El broker corre centralizado mediante Docker Compose en el puerto 9092, facilitando la comunicación local y en contenedores[cite: 2, 3].

## 5. Estructura del Proyecto
El repositorio se encuentra organizado en microservicios independientes dentro de la misma solución:
* `Exp2_S6_Eureka_Server/`: Servidor de descubrimiento de servicios.
* `Exp2_S6_Config_Server/`: Servidor de configuración centralizada.
* `Exp2_S6_Banco_API/`: Microservicio emisor (Producer) y gestor de transacciones/seguridad.
* `Exp2_S6_Cliente_API/`: Microservicio receptor (Consumer) y procesamiento de eventos.

## 6. Instrucciones de Ejecución
1. Clona el repositorio y ubícate en la carpeta raíz del proyecto:
   ```bash
   cd Exp3_S8_Rodrigo_Olavarria
Asegúrate de que los archivos .jar compilados se encuentren dentro de las carpetas target/ de cada microservicio (o compílalos individualmente ingresando a cada carpeta con mvn clean package).

Levanta la infraestructura completa (Kafka, Eureka, Config Server y las APIs) utilizando Docker Compose:

Bash
docker-compose up --build -d
