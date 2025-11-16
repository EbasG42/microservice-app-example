# Microservice App - PRFT Devops Training

This is the application you are going to use through the whole traninig. This, hopefully, will teach you the fundamentals you need in a real project. You will find a basic TODO application designed with a [microservice architecture](https://microservices.io). Although is a TODO application, it is interesting because the microservices that compose it are written in different programming language or frameworks (Go, Python, Vue, Java, and NodeJS). With this design you will experiment with multiple build tools and environments. 

## Components
In each folder you can find a more in-depth explanation of each component:

1. [Users API](/users-api) is a Spring Boot application. Provides user profiles. At the moment, does not provide full CRUD, just getting a single user and all users.
2. [Auth API](/auth-api) is a Go application, and provides authorization functionality. Generates [JWT](https://jwt.io/) tokens to be used with other APIs.
3. [TODOs API](/todos-api) is a NodeJS application, provides CRUD functionality over user's TODO records. Also, it logs "create" and "delete" operations to [Redis](https://redis.io/) queue.
4. [Log Message Processor](/log-message-processor) is a queue processor written in Python. Its purpose is to read messages from a Redis queue and print them to standard output.
5. [Frontend](/frontend) Vue application, provides UI.

## Architecture

Take a look at the components diagram that describes them and their interactions.
![microservice-app-example](/arch-img/Microservices.png)


Taller Microservicios Plataformas 2
Nombre: Juan Sebastian Guerrero Lasso
Codigo: A00395907

El objetivo de este taller es desplegar una arquitectura de microservicios robusta, escalable y observable en Kubernetes (Minikube), aplicando principios de DevOps como Contenerización, Orquestación, Despliegue Continuo (CD), Autoescalado (HPA), Micro-segmentación (Network Policies) y Monitoreo (Prometheus/Grafana). Para ello vamos a implementar esto bajo nuestra maquina ubuntu dentro de wsl.


Inicialmente Se verificó la instalación de las herramientas necesarias (Docker, kubectl, Minikube, Helm) en el entorno WSL Ubuntu, asegurando la capacidad de construir imágenes y gestionar el clúster.

### Parte 1: despliegue en Docker

Para el despliegue en docker se crearon dockerfiles para cada uno de los servicios con los que contamos en el repositorio
<img width="921" height="716" alt="image" src="https://github.com/user-attachments/assets/1a88e94f-9b71-4572-9973-c62dcf89a640" />
<img width="921" height="636" alt="image" src="https://github.com/user-attachments/assets/0d5e12ac-b96d-491b-ba9f-87f60c74c833" />
<img width="921" height="394" alt="image" src="https://github.com/user-attachments/assets/13b79acb-8fb8-4d6e-a143-2725fca1232e" />
<img width="921" height="550" alt="image" src="https://github.com/user-attachments/assets/7a195be0-bf19-4033-a9ae-c33954b67919" />

Despues de esto hicimos un despliegue inicial de manera local, con el cual se pudo acceder a la aplicacion.


### Parte 2: Despliegue con cluster en Kubernetes e Implementacion de Monitoreo, Secrets, CI/CD, HPA

Esta segunda parte fue mucho mas complicada, presento varios retos y problemas que explicaré mas adelante.

Para el despliegue en Kubernetes empezamos inicializando minikube, previamente instaladoo en nuestro entorno
<img width="921" height="285" alt="image" src="https://github.com/user-attachments/assets/4938e26d-142f-4795-8654-b04ea43e7961" />


Para esta segunda parte se realizaron las archivos correspondientes para cada uno de los microservicios, estos archivos yaml se encuentran en la carpeta k8s.

despues e ejecutarlos se crearon los pods correspondientes a cada servicio

<img width="1314" height="332" alt="image" src="https://github.com/user-attachments/assets/d3276496-63e6-4c03-ae61-23118910dc73" />

Aqui es donde encontramos uno de los problemas que se tuvieron en el taller, 2 de los servicios no levantaron y se quedaron en CrashLoopBackOff.
lo cual causa que el pod se destruya al no poderse levantar correctamente, creando un ciclo de reinicios (lo cual se evidencia en la imagen anterior)

Lamentablemente este error no pudo ser solucinado y esos servicios quedaron sin su respectivo despliegue.

Con esto, se implementaron  los Secrets, El monitoreo con grafana y prometheus, CI/CD y el autoescalado.

Depues de instalar grafana y prometheus, se pudo acceder a estos servicios por medio de los puertos 3000 y 9090 respectivamente

<img width="921" height="572" alt="image" src="https://github.com/user-attachments/assets/f9a970dd-7c7a-487d-b5d4-5dd2d92961c8" />

<img width="921" height="533" alt="image" src="https://github.com/user-attachments/assets/bcc392c3-3e5b-45f0-ba3b-b936e05830d8" />

<img width="921" height="304" alt="image" src="https://github.com/user-attachments/assets/5ae0b2d5-79d7-4e8e-85ca-8bd7c963e03c" />

<img width="921" height="308" alt="image" src="https://github.com/user-attachments/assets/5ba22f6c-754b-4c1b-bc0e-cd00821f9619" />

CI/CD se implemento por medio de un script, que, al realizar un commit a este repositorio, se ejecuta y realiza una serie de pasos para un despliegue automatico en DockerHUB,
<img width="1916" height="822" alt="image" src="https://github.com/user-attachments/assets/890a9986-74e7-4b76-a763-f8dff60036dc" />

Para poder hacer este despliegue con CI/CD tambien fue necesario el uso de los secrets dentro del repositorio, para el usuario y un token de acceso en docker.
<img width="998" height="327" alt="image" src="https://github.com/user-attachments/assets/40b98163-ce4e-4a58-bc07-b67ed7fa6714" />


Este taller fue bastante enriquecedor dado que se aplican gran parte de los conocimientos adquiridos a lo largo del curso, represento un reto al tener varias complicaciones entre ella el tema de compatibilidad de versiones en algunos de los recursos base del repositorio, lo cual causaba bastantes irregularidades y errores a la hora de desplegar inicialmente con docker, y posteriormente algunos problemas con kubernetes.
el uso de este tipo de herramientas da un valor muy significativo dentro del contexto de trabajo de la ingenieria telematica por lo cual fue bastante grato el desarrollo de este.

Para terminar, adjunto el video de showcase de este taller https://drive.google.com/file/d/1R-u98nTGH2r1u7V81zNROB4uwV4dXX5-/view?usp=sharing








