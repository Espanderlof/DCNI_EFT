# DCNI_EFT (Desarrollo Cloud Native I - EFT)

## 📋 Descripción del Proyecto
Sistema distribuido de alertas médicas en tiempo real para monitoreo de pacientes críticos, desarrollado con arquitectura de microservicios y desplegado en la nube. El proyecto implementa patrones cloud-native, mensajería asíncrona y autenticación en la nube para demostrar las mejores prácticas en el desarrollo de aplicaciones modernas.

## 🏗️ Arquitectura del Sistema

### Frontend (DCNI_FRONTEND)
- Desarrollado en Angular
- Características:
  - Autenticación mediante Azure IdaaS
  - Interfaz de usuario moderna y responsive
  - Integración con servicios REST del backend
- Ubicación: `/DCNI_FRONTEND`

### Backend (DCNI_BACKEND)
- Desarrollado en Spring Boot (Java)
- Características:
  - APIs RESTful securitizadas con Spring Security
  - Patrón BFF (Backend for Frontend)
  - Conexión con Oracle Cloud Database
  - Orquestación de llamadas a sistemas de mensajería
  - Desplegado en contenedores Docker en Azure
- Ubicación: `/DCNI_BACKEND`

### Sistemas de Mensajería

#### RabbitMQ (DCNI_RABBITMQ)
- Implementación en Java Spring Boot
- Componentes desplegados en Azure:
  - Productores: `/DCNI_RABBITMQ/` `productor_1` y `/productor_2`
  - Consumidores: `/DCNI_RABBITMQ/` `consumidor_1` y `/consumidor_2`
- Sistema de monitoreo en Angular para pruebas de generación de signos vitales
- Configuración mediante Docker Compose
- URL pública RabbitMQ: http://172.210.177.28:15672/

#### Apache Kafka (DCNI_KAFKA)
- Implementación en Java Spring Boot
- Microservicios desplegados en Azure:
  - Productor y Consumidor de alertas
  - Productor y Consumidor de signos vitales
  - Generador de signos vitales
- URL Kafka: http://172.200.161.74:8090/

### Base de Datos
- Oracle Cloud Database
- Scripts de creación disponibles en:
  - `ORACLE_CLOUD.sql`

## 🚀 Infraestructura Cloud

### Azure Cloud
- Todos los microservicios (excepto frontend) desplegados en Azure
- Contenedores Docker gestionados en Azure
- Integración con Azure IdaaS para autenticación
- API Manager AWS para gestión de endpoints

### Docker
- Imágenes disponibles en DockerHub:
  - Backend: `espanderlof/alertas_medicas`
  - Kafka: 
    - `espanderlof/kafka_productor_alertas`
    - `espanderlof/kafka_consumidor_alertas`
    - `espanderlof/kafka_productor_svitales`
    - `espanderlof/kafka_consumidor_svitales`
    - `espanderlof/kafka_generador_svitales`
  - RabbitMQ:
    - `espanderlof/dcni_sum_rabbitmq_productor1`
    - `espanderlof/dcni_sum_rabbitmq_productor2`
    - `espanderlof/dcni_sum_rabbitmq_consumidor1`
    - `espanderlof/dcni_sum_rabbitmq_consumidor2`

## ⚙️ Configuración y Despliegue

### Frontend (Local)
```bash
cd DCNI_FRONTEND
npm install
ng serve
```

### Backend (Azure)
```bash
# Despliegue en Azure usando Docker
docker pull espanderlof/alertas_medicas:latest
sudo docker run -d -p 8081:8081 --name alertas_medicas_backend espanderlof/alertas_medicas:latest
```

### RabbitMQ (Azure)
```bash
# Despliegue de RabbitMQ
sudo docker compose -f docker-compose-rabbitmq.yml up

# Despliegue de microservicios RabbitMQ
sudo docker compose -f docker-compose-hub-microservices.yml up

# O despliegue individual
sudo docker run -d -p 8082:8082 --name amed_productor1 espanderlof/dcni_sum_rabbitmq_productor1:latest
sudo docker run -d -p 8084:8084 --name amed_productor2 espanderlof/dcni_sum_rabbitmq_productor2:latest
sudo docker run -d -p 8083:8083 --name amed_consumidor1 espanderlof/dcni_sum_rabbitmq_consumidor1:latest
sudo docker run -d -p 8085:8085 --name amed_consumidor2 espanderlof/dcni_sum_rabbitmq_consumidor2:latest
```

### Kafka (Azure)
```bash
# Despliegue de Kafka
sudo docker compose -f docker-compose.yml up

# Despliegue de microservicios Kafka
sudo docker compose -f docker-compose-kafka-pro-cons.yml up

# O despliegue individual
sudo docker run -d -p 8092:8092 --name kafka_productor_alertas espanderlof/kafka_productor_alertas:latest
sudo docker run -d -p 8094:8094 --name kafka_consumidor_alertas espanderlof/kafka_consumidor_alertas:latest
sudo docker run -d -p 8091:8091 --name kafka_productor_svitales espanderlof/kafka_productor_svitales:latest
sudo docker run -d -p 8093:8093 --name kafka_consumidor_svitales espanderlof/kafka_consumidor_svitales:latest
sudo docker run -d -p 8095:8095 --name kafka_generador_svitales espanderlof/kafka_generador_svitales:latest
```

## 🔒 Seguridad
- Autenticación mediante Azure IdaaS
- Spring Security en el backend
- API Manager para gestión y seguridad de endpoints
- Comunicación segura entre microservicios

## 🛠️ Herramientas de Desarrollo
- Java 11+
- Spring Boot
- Angular
- Docker y Docker Compose
- Azure Cloud Services
- AWS API Manager
- Oracle Cloud Database
- RabbitMQ
- Apache Kafka

## 👥 Equipo de Desarrollo Grupo 6
- Gonzalo Durán
- Jaime Zapata