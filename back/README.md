# 🔧 E-commerce Backend - API RESTful

API REST desarrollada con NestJS para gestionar un sistema completo de comercio electrónico, incluyendo autenticación, gestión de productos, procesamiento de pedidos e integración con servicios externos.

## Tecnologías Principales

- **Framework**: NestJS 11
- **Lenguaje**: TypeScript
- **Base de datos**: PostgreSQL
- **ORM**: Prisma
- **Autenticación**: JWT + Passport
- **Almacenamiento de imágenes**: Cloudinary
- **Pasarela de pagos**: MercadoPago
- **Envío de emails**: Nodemailer
- **Generación de PDFs**: PDFKit
- **Validación**: Class Validator & Class Transformer

## Estructura del Proyecto

```
src/
├── auth/               # Autenticación y autorización (JWT, Guards, Strategies)
├── categories/         # Gestión de categorías de productos
├── products/           # CRUD de productos con imágenes
├── orders/             # Gestión de pedidos y estados
├── cloudinary/         # Integración con Cloudinary para subida de imágenes
├── mercado pago/       # Integración con MercadoPago para procesamiento de pagos
├── email/              # Servicio de envío de correos electrónicos
├── contact/            # Formulario de contacto
└── prisma/             # Módulo de Prisma para conexión a BD
```

## Características Principales

### Gestión de Productos
- CRUD completo de productos
- Subida múltiple de imágenes a Cloudinary
- Categorización de productos
- Control de stock
- Generación automática de slugs

### Sistema de Pedidos
- Creación y seguimiento de pedidos
- Estados de pedidos (Pending, Confirmed, Shipped, Delivered, Cancelled)
- Estados de pagos (Pending, Approved, Rejected, etc.)
- Generación de número de orden único
- Historial de items por pedido

### Autenticación y Seguridad
- Autenticación JWT para administradores
- Guards para protección de rutas
- Encriptación de contraseñas con bcrypt
- Estrategias de Passport para validación de tokens

### Integraciones Externas
- **Cloudinary**: Almacenamiento y optimización de imágenes
- **MercadoPago**: Procesamiento de pagos con webhooks
- **Nodemailer**: Envío de emails transaccionales

## Modelos de Base de Datos

### Admin
- Sistema de autenticación para administradores
- Gestión de credenciales

### Category
- Categorías para organizar productos
- Slugs únicos para SEO

### Product
- Información completa del producto
- Relación con categorías
- Múltiples imágenes
- Control de stock e inventario
- Estado activo/inactivo

### Order
- Información del cliente
- Total del pedido
- Estados de orden y pago
- Integración con MercadoPago

### OrderItem
- Detalle de productos en cada pedido
- Snapshot de precio al momento de la compra

## Prerequisitos

- Node.js (versión 18 o superior)
- PostgreSQL
- Cuenta de Cloudinary
- Cuenta de MercadoPago (modo test o producción)
- Cuenta de email SMTP (Gmail recomendado)

## Configuración del Entorno

1. **Clonar el repositorio y navegar al directorio del backend**:
```bash
cd back
```

2. **Instalar dependencias**:
```bash
npm install
```

3. **Configurar variables de entorno**:

Crear un archivo `.env` basado en `.env.example`:

```env
# Database
DATABASE_URL="postgresql://usuario:contraseña@localhost:5432/ecommerce_db?schema=public"

# JWT
JWT_SECRET="your-super-secret-jwt-key-change-this-in-production"
JWT_EXPIRATION="7d"

# Cloudinary
CLOUDINARY_CLOUD_NAME="tu-cloud-name"
CLOUDINARY_API_KEY="tu-api-key"
CLOUDINARY_API_SECRET="tu-api-secret"

# App
PORT=3000
NODE_ENV="development"

# CORS - Frontend URL
FRONTEND_URL="http://localhost:5173"

# Mercado Pago
MERCADOPAGO_ACCESS_TOKEN=TEST-tu-access-token

# Email
EMAIL_HOST="smtp.gmail.com"
EMAIL_PORT=587
EMAIL_USER="tu-cuenta@gmail.com"
EMAIL_PASSWORD="tu-app-password"
EMAIL_FROM="TechStore <tu-cuenta@gmail.com>"
```

4. **Configurar la base de datos**:

```bash
# Generar el cliente de Prisma
npm run prisma:generate

# Ejecutar las migraciones
npm run prisma:migrate

# (Opcional) Poblar la base de datos con datos de prueba
npm run prisma:seed
```

## Scripts Disponibles

### Desarrollo
```bash
# Modo desarrollo con hot-reload
npm run start:dev

# Modo debug
npm run start:debug
```

### Producción
```bash
# Build del proyecto
npm run build

# Ejecutar en producción
npm run start:prod
```

### Base de Datos
```bash
# Generar cliente de Prisma
npm run prisma:generate

# Crear y ejecutar migraciones
npm run prisma:migrate

# Abrir Prisma Studio (GUI para la BD)
npm run prisma:studio

# Poblar la base de datos
npm run prisma:seed

# Resetear base de datos (¡cuidado!)
npm run prisma:reset

# Resetear sin seed
npm run prisma:clean

# Resetear y arrancar servidor
npm run db:fresh
```

### Testing
```bash
# Ejecutar tests
npm run test

# Tests en modo watch
npm run test:watch

# Tests con cobertura
npm run test:cov

# Tests e2e
npm run test:e2e
```

### Linting y Formato
```bash
# Ejecutar linter
npm run lint

# Formatear código
npm run format
```

## Endpoints Principales

### Autenticación
- `POST /api/auth/login` - Login de administrador
- `POST /api/auth/register` - Registro de administrador (protegido)

### Categorías
- `GET /api/categories` - Listar todas las categorías
- `POST /api/categories` - Crear categoría (protegido)
- `PATCH /api/categories/:id` - Actualizar categoría (protegido)
- `DELETE /api/categories/:id` - Eliminar categoría (protegido)

### Productos
- `GET /api/products` - Listar productos
- `GET /api/products/:slug` - Obtener producto por slug
- `POST /api/products` - Crear producto (protegido)
- `PATCH /api/products/:id` - Actualizar producto (protegido)
- `DELETE /api/products/:id` - Eliminar producto (protegido)
- `POST /api/products/:id/images` - Subir imágenes (protegido)

### Pedidos
- `GET /api/orders` - Listar pedidos (protegido)
- `GET /api/orders/:id` - Obtener pedido por ID
- `POST /api/orders` - Crear pedido
- `PATCH /api/orders/:id/status` - Actualizar estado (protegido)

### Pagos
- `POST /api/mercadopago/create-preference` - Crear preferencia de pago
- `POST /api/mercadopago/webhook` - Webhook de notificaciones

### Contacto
- `POST /api/contact` - Enviar mensaje de contacto

## Flujo de Trabajo Típico

1. **Administrador**:
   - Registrar/login de administrador
   - Crear categorías de productos
   - Crear productos con imágenes
   - Gestionar inventario
   - Procesar pedidos

2. **Cliente**:
   - Navegar catálogo de productos
   - Crear pedido
   - Realizar pago con MercadoPago
   - Recibir confirmación por email

## Consideraciones de Seguridad

- Todas las rutas administrativas están protegidas con JWT
- Las contraseñas se encriptan con bcrypt antes de almacenarse
- Validación de datos en todas las peticiones
- CORS configurado para permitir solo el frontend autorizado
- Variables sensibles en variables de entorno

## Solución de Problemas

### Error de conexión a la base de datos
Verifica que PostgreSQL esté corriendo y que la `DATABASE_URL` sea correcta.

### Error con Cloudinary
Asegúrate de tener credenciales válidas y que tu cuenta esté activa.

### Error con MercadoPago
Verifica que estés usando el token correcto (TEST para desarrollo, PROD para producción).

### Error con emails
Si usas Gmail, necesitas generar una "App Password" en tu cuenta de Google.

## Recursos Adicionales

- [Documentación de NestJS](https://docs.nestjs.com/)
- [Documentación de Prisma](https://www.prisma.io/docs/)
- [Documentación de MercadoPago](https://www.mercadopago.com.ar/developers/es)
- [Documentación de Cloudinary](https://cloudinary.com/documentation)
