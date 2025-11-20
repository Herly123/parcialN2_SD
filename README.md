📌 1. Descripción General del Proyecto

Este proyecto consiste en un Sistema Distribuido de Historia Clínica Electrónica (HCE) diseñado para funcionar sobre una arquitectura moderna basada en:

Base de datos distribuida con PostgreSQL + Citus

Middleware en Python con FastAPI

Despliegue en Kubernetes (Minikube)

Interfaces gráficas HTML/Jinja2

Seguridad OAuth2 + JWT

Exportación de historias clínicas en PDF

El sistema permite gestionar información médica de forma segura, escalable y accesible desde cualquier dispositivo dentro de la red local.
Se diseñó para simular un entorno real utilizado en instituciones de salud, cumpliendo funciones como:

Registro de pacientes

Gestión de admisiones

Registro de notas médicas

Consulta de resultados

Exportación de historias clínicas en PDF

Su objetivo principal es integrar los conceptos de sistemas distribuidos, seguridad, infraestructura y experiencia de usuario en un único proyecto funcional.

📌 2. Arquitectura del Sistema

La arquitectura se divide en cuatro capas principales:

a. Base de Datos Distribuida (PostgreSQL + Citus)

1 coordinator

2 workers

Fragmentación de tablas por documento_id

Ejecución de consultas distribuidas automáticamente

b. Middleware (FastAPI)

Gestiona peticiones REST

Conecta con el clúster Citus

Aplica autenticación y autorización

Genera PDFs

Sirve las interfaces HTML

c. Interfaces de Usuario (HTML + Jinja2)

Cuatro roles principales:

Rol	Función
Paciente	Consultar su información
Admisionista	Registrar ingresos
Médico	Registrar notas y diagnósticos
Resultados	Consultar y exportar historia clínica
d. Seguridad (OAuth2 + JWT)

Inicio de sesión con usuario y contraseña

Generación de tokens

Rutas protegidas

Autorización según rol


Estructura del proyecto

parcialN2_SD
.
├── backend
│   ├── auth.py
│   ├── crud_paciente.py
│   ├── crud.py
│   ├── database.py
│   ├── Dockerfile
│   ├── generate_hashes.py
│   ├── main.py
│   ├── models.py
│   ├── README.md
│   ├── requirements.txt
│   ├── schemas.py
│   ├── templates
│   │   └── register.html
│   ├── test_api.py
│   └── test_backend.py
├── docker-compose.yml
├── frontend
│   ├── app.py
│   ├── Dockerfile
│   ├── README.md
│   ├── requirements.txt
│   ├── static
│   │   ├── scripts.js
│   │   └── style.css
│   └── templates
│       ├── admisionista.html
│       ├── base.html
│       ├── login.html
│       ├── medico.html
│       ├── paciente.html
│       ├── register.html
│       └── secretaria.html
├── k8s
│   ├── frontend-service.yaml
│   ├── init-scripts
│   │   └── create_db.sql
│   ├── parcial-backend-nodeport.yaml
│   ├── parcial-backend.yaml
│   ├── parcial-frontend-nodeport.yaml
│   └── parcial-frontend.yaml
└── README.md

8 directories, 35 files


📌 3. Tecnologías Utilizadas
Backend / Middleware

Python 3.10+

FastAPI

Uvicorn

Psycopg2

WeasyPrint (PDF)

Base de Datos

PostgreSQL 14+

Citus

Infraestructura

Kubernetes

Minikube

kubectl

Docker

Frontend

HTML5

Jinja2

CSS básico

Seguridad

OAuth2 Password Flow

JWT (PyJWT)

📌 4. Cómo Instalar el Proyecto
# 1. Clonar el repositorio
git clone https://github.com/Herly123/parcialN2_SD
cd parcialN2_SD

# 2. Crear entorno virtual
python -m venv venv
source venv/bin/activate        # Linux / Mac
venv\Scripts\activate           # Windows

# 3. Instalar dependencias
pip install -r requirements.txt

📌 5. Cómo Ejecutar el Proyecto
Ejecutar el middleware localmente
uvicorn main:app --reload --host 0.0.0.0 --port 8000


Abrir en navegador:

http://localhost:8000

📌 6. Endpoints de la API
🔐 Autenticación
Método	Ruta	Descripción
POST	/login	Genera un JWT
POST	/register	Registrar usuario (opcional)
👤 Pacientes
Método	Ruta	Descripción
GET	/paciente/{id}	Obtiene datos del paciente (requiere JWT)
POST	/paciente	Crea un nuevo paciente
🧑‍⚕️ Médico
Método	Ruta	Descripción
POST	/medico/nota	Registra nota médica
GET	/medico/notas/{id}	Obtiene notas del paciente
📄 PDF
Método	Ruta	Descripción
GET	/exportar_pdf/{id}	Exporta historia clínica en PDF (requiere JWT)
📌 7. Cómo Desplegarlo en Kubernetes
1. Iniciar Minikube
minikube start

2. Crear recursos
kubectl apply -f kubernetes/citus-coordinator.yaml
kubectl apply -f kubernetes/citus-worker.yaml
kubectl apply -f kubernetes/middleware-deployment.yaml
kubectl apply -f kubernetes/middleware-service.yaml

3. Verificar pods
kubectl get pods

4. Exponer servicio
minikube service middleware-service

📌 8. Autores y Roles del Equipo
Integrante	Rol	Responsabilidades
Herly123	Backend & DevSecOps	Citus, FastAPI, Docker, JWT, Kubernetes
Frontend & UX	Interfaces HTML, Jinja2, flujo por roles, PDF
Jose David VIllegas Pacheco, Herly Machado Parra

Ambos participaron en:

Arquitectura

Pruebas funcionales

Documentación
