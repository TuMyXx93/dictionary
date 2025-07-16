# Diccionario Virtual Misak

El objetivo de este proyecto es crear un diccionario virtual para la comunidad Misak. La plataforma permitirá a los usuarios acceder a palabras en la lengua Namtrik, junto con su significado en español, una imagen alusiva y un archivo de audio con la pronunciación.

## Stack de Tecnologías

Este proyecto utiliza una arquitectura cliente-servidor con las siguientes tecnologías:

*   **Frontend:** [React](https://react.dev/) con [Vite](https://vitejs.dev/), [TypeScript](https://www.typescriptlang.org/) y [React-Bootstrap](https://react-bootstrap.github.io/)
*   **Backend:** [Python](https://www.python.org/) con [FastAPI](https://fastapi.tiangolo.com/)
*   **Base de Datos:** [MongoDB](https://www.mongodb.com/) (ejecutándose en Docker)

## Funcionalidades Implementadas

### 🔍 **Búsqueda de Palabras**
- Búsqueda en tiempo real por término, significado o ejemplo
- Búsqueda case-insensitive con soporte para caracteres especiales
- Contador de resultados dinámico
- **Búsqueda en Panel Admin**: Funcionalidad específica para administradores con filtrado local

### 📝 **Gestión de Palabras (CRUD Completo)**
- **Crear**: Agregar nuevas palabras con validación
- **Leer**: Visualizar todas las palabras del diccionario
- **Actualizar**: Editar palabras existentes con modal intuitivo
- **Eliminar**: Eliminar palabras con confirmación de seguridad
- **Buscar en Admin**: Filtro de búsqueda local en el panel de administración

### 🎨 **Interfaz de Usuario**
- Diseño responsivo con Bootstrap
- Navegación intuitiva entre páginas
- Modales para confirmaciones y edición
- Mensajes de feedback para todas las operaciones
- Badges informativos y contadores (total y filtrados)
- **Buscador específico en Admin**: Campo de búsqueda dedicado para administradores

### 📊 **Estadísticas**
- Endpoint para obtener métricas del diccionario
- Contador de palabras total
- Contador de palabras con/sin ejemplos

### 🔧 **Panel de Administración**
- **CRUD completo**: Crear, editar y eliminar palabras
- **Búsqueda local**: Filtro de búsqueda en tiempo real para administradores
- **Contador dinámico**: Muestra palabras filtradas vs total (ej: "5 / 25")
- **Botones de acción**: Editar (✏️) y eliminar (🗑️) para cada palabra
- **Validación**: Prevención de errores con feedback visual
- **Modales**: Confirmaciones para edición y eliminación

### 🎯 **Funcionalidades Específicas del Admin**
- **Búsqueda Administrativa**: Campo específico para buscar palabras y facilitar la edición
- **Filtrado Local**: Búsqueda instantánea sin necesidad de consultar la API
- **Contador de Resultados**: Badge que muestra "X / Y" palabras (filtradas/total)
- **Botón de Limpiar**: Opción para limpiar el filtro de búsqueda
- **Mensajes de Estado**: Feedback visual para operaciones exitosas y errores

## Estructura del Proyecto

El repositorio está organizado en dos directorios principales:

*   `frontend/`: Contiene la aplicación de una sola página (SPA) de React.
*   `backend/`: Contiene el servidor de la API REST de FastAPI.

## Cómo Empezar

Sigue estas instrucciones para configurar y ejecutar el proyecto en tu máquina local.

### Prerrequisitos

*   [Node.js](https://nodejs.org/) (versión 18 o superior)
*   [Python](https://www.python.org/downloads/) (versión 3.9 o superior)
*   [Docker](https://www.docker.com/) (para MongoDB)

### 1. Configuración de la Base de Datos MongoDB

```bash
# Crear y ejecutar contenedor MongoDB
docker run -d \
  --name misak-mongo \
  -p 27017:27017 \
  -v misak-mongo-data:/data/db \
  mongo:latest

# Verificar que el contenedor esté corriendo
docker ps
```

### 2. Configuración del Backend

```bash
# 1. Navega al directorio del backend
cd backend

# 2. Crea un entorno virtual
python3 -m venv venv

# 3. Activa el entorno virtual
# En macOS y Linux:
source venv/bin/activate
# En Windows:
# .\venv\Scripts\activate

# 4. Instala las dependencias de Python
pip install -r requirements.txt

# 5. Inicia el servidor de desarrollo
# El servidor se ejecutará en http://localhost:8000
uvicorn main:app --reload
```

### 3. Configuración del Frontend

```bash
# 1. En una nueva terminal, navega al directorio del frontend
cd frontend

# 2. Instala las dependencias de Node.js
npm install

# 3. Inicia el servidor de desarrollo de Vite
# La aplicación se ejecutará en http://localhost:5173
npm run dev
```

## API Endpoints

### Gestión de Palabras

- `GET /api/dictionary` - Obtener todas las palabras
- `POST /api/dictionary` - Crear una nueva palabra
- `PUT /api/dictionary/{word_id}` - Actualizar una palabra existente
- `DELETE /api/dictionary/{word_id}` - Eliminar una palabra
- `GET /api/dictionary/search?q={query}` - Buscar palabras

### Utilidades

- `GET /api/health` - Verificar estado del servicio
- `GET /api/dictionary/stats` - Obtener estadísticas del diccionario

### Ejemplo de Uso de la API

```bash
# Crear una nueva palabra
curl -X POST "http://localhost:8000/api/dictionary" \
  -H "Content-Type: application/json" \
  -d '{
    "term": "pishau",
    "meaning": "agua",
    "example": "Pishau purap - El agua está fría"
  }'

# Buscar palabras
curl -X GET "http://localhost:8000/api/dictionary/search?q=agua"

# Actualizar una palabra
curl -X PUT "http://localhost:8000/api/dictionary/{word_id}" \
  -H "Content-Type: application/json" \
  -d '{
    "term": "pishau",
    "meaning": "agua sagrada",
    "example": "Pishau purap - El agua sagrada está fría"
  }'

# Eliminar una palabra
curl -X DELETE "http://localhost:8000/api/dictionary/{word_id}"
```

## Comandos de Desarrollo

### Backend
```bash
# Activar entorno virtual
source venv/bin/activate

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar servidor en modo desarrollo
uvicorn main:app --reload

# Ejecutar servidor en modo producción
uvicorn main:app --host 0.0.0.0 --port 8000
```

### Frontend
```bash
# Instalar dependencias
npm install

# Ejecutar en modo desarrollo
npm run dev

# Construir para producción
npm run build

# Previsualizar build de producción
npm run preview

# Ejecutar linter
npm run lint
```

### Base de Datos
```bash
# Iniciar MongoDB
docker start misak-mongo

# Detener MongoDB
docker stop misak-mongo

# Ver logs de MongoDB
docker logs misak-mongo

# Acceder a MongoDB shell
docker exec -it misak-mongo mongosh

# Backup de la base de datos
docker exec misak-mongo mongodump --db misak_dictionary --out /data/backup

# Restaurar backup
docker exec misak-mongo mongorestore --db misak_dictionary /data/backup/misak_dictionary
```

## Comandos de Producción

### Usando Docker Compose (Recomendado)

```bash
# Construir y ejecutar todos los servicios
docker-compose up --build

# Ejecutar en segundo plano
docker-compose up -d

# Detener todos los servicios
docker-compose down

# Ver logs
docker-compose logs -f
```

### Despliegue Manual

```bash
# Backend
cd backend
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000

# Frontend
cd frontend
npm install
npm run build
# Servir archivos estáticos con nginx o servidor web
```

## Estructura de la Base de Datos

### Colección: `dictionary`
```javascript
{
  "_id": ObjectId("..."),
  "term": "String",      // Palabra en Namtrik
  "meaning": "String",   // Significado en español
  "example": "String"    // Ejemplo de uso (opcional)
}
```

## Próximas Características

- [ ] **Autenticación**: Sistema de login para administradores
- [ ] **Categorías**: Clasificación de palabras (sustantivos, verbos, etc.)
- [ ] **Multimedia**: Soporte para imágenes y archivos de audio
- [ ] **Exportar/Importar**: Funcionalidad para carga masiva de palabras
- [ ] **Paginación**: Para mejorar rendimiento con grandes cantidades de datos
- [ ] **Validación**: Prevención de palabras duplicadas
- [ ] **Historial**: Registro de cambios y versiones

## Contribución

1. Fork el proyecto
2. Crea tu rama de características (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## Contacto

Para más información sobre el proyecto o la comunidad Misak, contacta a:
- **Desarrollador**: [Tu nombre]
- **Email**: [tu-email@ejemplo.com]
- **Proyecto**: [https://github.com/tumidev/dictionary](https://github.com/tumidev/dictionary)

---

*Este proyecto está dedicado a la preservación y difusión de la lengua Namtrik de la comunidad Misak.*
