# Tutorial: Proyecto MVC con Express + EJS (SSR)
Universidad del Valle de Guatemala | Campus Altiplano
Programación Web 1
Lic. Marvin Quiñónez
mjquinonez@uvg.edu.gt
Fecha: 6 de febrero de 2026


Clase #4
Este tutorial guía paso a paso la construcción de un proyecto MVC sencillo con **Express** y **EJS**, renderizado en servidor (SSR). El objetivo es que todo el grupo trabaje en la misma sintonía y con el mismo flujo.

---

## 1) Requisitos previos

- Node.js 18+ (recomendado)
- npm 9+ (incluido con Node)
- Terminal y editor (VS Code recomendado)

Para verificar versiones:

```bash
node -v
npm -v
```

---

## 2) Estructura del proyecto

Usaremos esta estructura (MVC):

```
mvcapp/
├── app.js
├── package.json
└── src/
    ├── controllers/
    │   └── userController.js
    ├── models/
    │   └── userModel.js
    ├── routes/
    │   └── userRoutes.js
    └── views/
        └── vista1.ejs
```

- **Modelos**: manejan datos y lógica básica.
- **Controladores**: reciben la solicitud y envían respuesta.
- **Rutas**: conectan endpoints con controladores.
- **Vistas**: HTML renderizado en servidor con EJS.

---

## 3) Inicializar el proyecto

1. Crea la carpeta del proyecto y entra:

```bash
mkdir mvcapp
cd mvcapp
```

2. Inicializa npm:

```bash
npm init -y
```

3. Instala dependencias:

```bash
npm install express ejs
```

4. En el `package.json`, activa ES Modules con:

```json
{
  "type": "module"
}
```

---

## 4) Código completo por archivo

> Copia y pega exactamente lo siguiente en cada archivo.

### app.js

```js
import express from "express"; // Importa Express usando ES modules
import path from "path"; // Importa utilidades de rutas (path) con ES modules
import { fileURLToPath } from "url"; // Convierte import.meta.url a ruta de archivo
import userRoutes from "./src/routes/userRoutes.js"; // Importa las rutas con extensión .js

const __filename = fileURLToPath(import.meta.url); // Emula __filename en ES modules
const __dirname = path.dirname(__filename); // Emula __dirname en ES modules

const app = express();

app.use(express.urlencoded({ extended: true })); // Habilita lectura de datos de formularios

app.set("view engine", "ejs"); // Define el motor de plantillas
app.set("views", path.join(__dirname, "src/views")); // Define la carpeta de vistas

app.use("/", userRoutes); // Monta las rutas principales

app.listen(3000, () => { // Inicia el servidor
  console.log("Servidor en http://localhost:3000");
});
```

### src/routes/userRoutes.js

```js
import express from "express"; // Importa Express con ES modules
import { index, add } from "../controllers/userController.js"; // Importa controladores

const router = express.Router(); // Crea el enrutador

router.get("/", index); // Ruta principal
router.post("/add", add); // Ruta para agregar usuario

export default router; // Exporta el enrutador (ES modules)
```

### src/controllers/userController.js

```js
import userModel from "../models/userModel.js"; // Importa el modelo con ES modules

function index(req, res) {
  res.render("index", { users: userModel.getAll() }); // Renderiza la vista con la lista de usuarios
}

function add(req, res) {
  userModel.add(req.body.name); // Agrega un usuario desde el formulario
  res.redirect("/"); // Redirige a la página principal
}

export { index, add }; // Exporta funciones para ser usadas en las rutas
```

### src/models/userModel.js

```js
const users = []; // Almacén en memoria de usuarios

function getAll() {
  return users; // Devuelve todos los usuarios
}

function add(name) {
  users.push({ id: users.length + 1, name }); // Agrega un usuario con id incremental
}

export default {
  getAll,
  add,
}; // Exporta el modelo como default (ES modules)
```

### src/views/vista1.ejs

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <title>MVC SSR</title>
</head>
<body>
  <h1>Usuarios</h1>

  <form method="POST" action="/add">
    <input type="text" name="name" required />
    <button type="submit">Agregar</button>
  </form>

  <ul>
    <% users.forEach(u => { %>
      <li><%= u.name %></li>
    <% }) %>
  </ul>
</body>
</html>
```

---

## 5) Ejecutar el proyecto

Desde la raíz del proyecto:

```bash
node app.js
```

Abre tu navegador en:

```
http://localhost:3000
```

---

## 6) Qué aprenderás con este proyecto

- Flujo MVC básico en Node.js
- Enrutamiento con Express
- Renderizado en servidor con EJS
- Manejo de formularios y redirecciones

---

## 7) Sugerencias para el curso

- Todos deben mantener la misma estructura de carpetas.
- Usar exactamente los mismos nombres de archivos.
- Si algo falla, revisar primero:
  - Que `type` sea `module` en `package.json`.
  - Que las rutas de importación tengan extensión `.js`.
  - Que la carpeta de vistas sea `src/views`.

---

## 8) Qué más se puede hacer?

Este proyecto es básico. El objetivo es segmentar las responsabilidades. Obviamente, hay que agregar todas las validaciones necesarias y almacenar la información en base de datos. Por lo que también se puede:

- Persistir usuarios en un archivo JSON o base de datos.
- Agregar validaciones más estrictas.
- Crear páginas adicionales (editar/eliminar usuarios).


