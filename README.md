🚀 API REST Clientes y Pedidos — Backend PHP + MySQL

Sistema backend desarrollado con PHP y MySQL basado en arquitectura RESTful, orientado a la gestión de clientes y pedidos mediante operaciones CRUD seguras, utilizando PDO, validaciones y respuestas JSON estructuradas.

📌 Descripción General

Este proyecto implementa una API REST profesional desarrollada en PHP que permite administrar clientes y pedidos utilizando una base de datos relacional MySQL.

La solución fue diseñada siguiendo buenas prácticas backend modernas, aplicando:

Arquitectura Cliente → API → Base de Datos
Seguridad mediante PDO y consultas preparadas
Validación de datos
Respuestas HTTP correctas
Integración mediante JSON
Testing funcional con Postman
Separación de responsabilidades

El objetivo principal es evitar que el cliente interactúe directamente con la base de datos, utilizando la API como capa intermedia de lógica, validación y seguridad.

🏗 Arquitectura del Proyecto
Cliente HTTP
     ↓
API REST PHP
     ↓
MySQL Database
Flujo de funcionamiento
El cliente realiza una solicitud HTTP.
La API recibe la petición.
PHP procesa la lógica de negocio.
Se validan los datos recibidos.
PDO ejecuta consultas seguras.
MySQL almacena o devuelve información.
La API responde en formato JSON.
✅ Ventajas de la Arquitectura
Seguridad
Escalabilidad
Modularidad
Separación de responsabilidades
Fácil integración con frontend
Preparado para React, Vue o Angular
🛠 Tecnologías Utilizadas
Backend
PHP 8
Apache2
MySQL 8
JSON
PDO Seguro
Infraestructura
Ubuntu
WSL2
XAMPP
phpMyAdmin
Testing y Control
Postman
Git
GitHub
📂 Estructura del Proyecto
api_clientes/
│
├── clientes.php
├── pedidos.php
├── conexion.php
├── config.php
├── database.sql
├── README.md
├── postman_collection.json
│
└── assets/
⚙ Configuración del Entorno
1️⃣ Clonar repositorio
git clone https://github.com/usuario/api_clientes.git
2️⃣ Mover proyecto al servidor Apache
Ubuntu / WSL
sudo mv api_clientes /var/www/html/

Ruta final:

/var/www/html/api_clientes
3️⃣ Configurar Apache

Iniciar servicio:

sudo service apache2 start

Verificar:

http://localhost/api_clientes
🗄 Configuración Base de Datos
Crear base de datos
CREATE DATABASE api_clientes_db;
Importar SQL
mysql -u root -p api_clientes_db < database.sql
🧩 Modelo Relacional
Tabla: clientes
Campo	Tipo
id	INT PK
nombre	VARCHAR
correo	VARCHAR UNIQUE
telefono	VARCHAR
creado_en	TIMESTAMP
Tabla: pedidos
Campo	Tipo
id	INT PK
cliente_id	INT FK
producto	VARCHAR
cantidad	INT
estado	VARCHAR
creado_en	TIMESTAMP
🔗 Relación
clientes (1) ────────< pedidos (N)

Un cliente puede tener múltiples pedidos.

🔐 Seguridad Implementada
Medidas aplicadas
Prepared Statements
PDO seguro
Validación de correos
Validación numérica de IDs
Sanitización de entradas
Restricción UNIQUE
Manejo de errores
Validación JSON
CORS controlado
Prevención SQL Injection
🔑 Configuración conexión segura
config.php
<?php

define("DB_HOST", "localhost");
define("DB_NAME", "api_clientes_db");
define("DB_USER", "root");
define("DB_PASS", "tu_password");

?>
conexion.php
<?php

require_once "config.php";

try {

    $pdo = new PDO(
        "mysql:host=" . DB_HOST . ";dbname=" . DB_NAME . ";charset=utf8",
        DB_USER,
        DB_PASS
    );

    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

} catch(PDOException $e) {

    die(json_encode([
        "ok" => false,
        "mensaje" => "Error conexión DB",
        "error" => $e->getMessage()
    ]));
}

?>
🚀 Endpoints Implementados
👤 Clientes
📥 Obtener todos los clientes
GET
/clientes.php
Respuesta
[
  {
    "id": 1,
    "nombre": "Freddy",
    "correo": "freddy@email.com",
    "telefono": "987654321"
  }
]
🔍 Obtener cliente por ID
GET
/clientes.php?id=1
➕ Crear cliente
POST
/clientes.php
Body JSON
{
  "nombre": "Freddy",
  "correo": "freddy@email.com",
  "telefono": "987654321"
}
✏ Actualizar cliente
PUT
/clientes.php?id=1
Body JSON
{
  "nombre": "Freddy Actualizado",
  "correo": "nuevo@email.com",
  "telefono": "123456789"
}
❌ Eliminar cliente
DELETE
/clientes.php?id=1
📦 Pedidos
📥 Obtener pedidos
GET
/pedidos.php
➕ Crear pedido
POST
/pedidos.php
Body JSON
{
  "cliente_id": 1,
  "producto": "Notebook",
  "cantidad": 2,
  "estado": "pendiente"
}
🧠 Lógica CRUD Implementada
$method = $_SERVER['REQUEST_METHOD'];

switch ($method) {

    case 'GET':
        // listar o buscar
        break;

    case 'POST':
        // crear
        break;

    case 'PUT':
        // actualizar
        break;

    case 'DELETE':
        // eliminar
        break;

    default:

        http_response_code(405);

        echo json_encode([
            "ok" => false,
            "mensaje" => "Método no permitido"
        ]);
}
📌 Ejemplo CRUD — Crear Cliente
$data = json_decode(file_get_contents("php://input"), true);

$nombre = $data["nombre"] ?? "";
$correo = $data["correo"] ?? "";
$telefono = $data["telefono"] ?? "";

$sql = "INSERT INTO clientes (nombre, correo, telefono)
        VALUES (?, ?, ?)";

$stmt = $pdo->prepare($sql);

$stmt->execute([
    $nombre,
    $correo,
    $telefono
]);
🔄 Flujo Interno del Backend
Postman
   ↓
Solicitud HTTP
   ↓
clientes.php
   ↓
Validaciones
   ↓
PDO Seguro
   ↓
MySQL
   ↓
Respuesta JSON
📡 Códigos HTTP Utilizados
Código	Descripción
200	OK
201	Created
400	Bad Request
404	Not Found
405	Method Not Allowed
409	Conflict
500	Internal Server Error
🧪 Testing con Postman
CRUD Validado
✔ Listado
✔ Búsqueda
✔ Creación
✔ Actualización
✔ Eliminación
📬 Colección Postman

Incluida en:

postman_collection.json

Permite probar rápidamente todos los endpoints.

⚠ Problemas Detectados y Soluciones
❌ Problema 1 — Archivos no encontrados
Solución

Uso correcto de rutas WSL:

/mnt/c/Users/
❌ Problema 2 — Base de datos incorrecta
Solución
DB_NAME = api_clientes_db
❌ Problema 3 — Acceso denegado MySQL
Solución
DB_PASS = contraseña_real
❌ Problema 4 — Error conexión API
Solución

Verificación completa de:

config.php
conexion.php
credenciales
Apache
MySQL
📘 Buenas Prácticas Aplicadas
Arquitectura REST
Código modular
Validación backend
Manejo de excepciones
Separación de lógica
Seguridad SQL
JSON estructurado
Uso correcto de HTTP Status
Integración preparada para frontend
🔮 Escalabilidad Futura

El proyecto queda preparado para integrar:

ReactJS
VueJS
Angular
JWT Authentication
Middleware
Roles y permisos
Docker
Despliegue AWS
CI/CD
Swagger Documentation
📚 Aprendizajes Obtenidos
Backend
APIs REST
CRUD avanzado
Validaciones
Seguridad SQL
Manejo JSON
Base de Datos
Relaciones
Integridad referencial
Claves foráneas
Restricciones UNIQUE
Infraestructura
Apache
Ubuntu
WSL2
XAMPP
Testing
Postman
QA funcional
Validación endpoints
Profesional
Troubleshooting
Configuración de entornos
Documentación técnica
Buenas prácticas backend
✅ Conclusión

Este proyecto permitió implementar exitosamente una solución backend profesional basada en una arquitectura moderna Cliente → API REST → Base de Datos Relacional, cumpliendo completamente los objetivos técnicos y académicos planteados.

La API desarrollada utilizando PHP, MySQL y Apache logró administrar clientes y pedidos mediante operaciones CRUD completas, aplicando principios fundamentales de:

Seguridad web
Arquitectura backend
Validaciones
Integridad relacional
Testing funcional
Documentación técnica

Uno de los puntos más importantes fue la implementación de medidas reales de seguridad utilizando:

PDO
Prepared Statements
Validaciones
Restricciones UNIQUE
Sanitización de entradas
Manejo de errores HTTP

Además, el proyecto permitió resolver problemas reales de configuración, conexión y estructura de entornos Linux/Windows, fortaleciendo competencias prácticas orientadas al desarrollo profesional.

Finalmente, la incorporación de GitHub, documentación estructurada y pruebas con Postman transforma esta solución en una base sólida, escalable y preparada para integrarse con tecnologías frontend modernas o futuras arquitecturas empresariales.

Video explicativo 
https://correoaiep-my.sharepoint.com/:v:/g/personal/johannes_rojasm_correoaiep_cl/IQAlWFtaf-S8RbD_X5sWWuCMAYjgJ2BY4YNUi3h3kMQGdLM?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=2es58h

👨‍💻 Autor

Freddy Saldivia
Johannes Rojas
Samuel Sariego

Desarrollador Full Stack | Backend | APIs REST | SQL | AWS
