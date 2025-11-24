# 🛒 E-commerce Full Stack - TechStore

![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-3982CE?style=for-the-badge&logo=Prisma&logoColor=white)

## Descripción General

Este proyecto es una solución full stack para e-commerce que permite a los usuarios navegar productos, realizar compras de forma segura, y a los administradores gestionar el inventario y pedidos de manera eficiente. La aplicación está dividida en dos partes principales: un backend basado en NestJS y un frontend construido con React.

## Características Principales

### Para Clientes
- Catálogo de productos navegable con filtros por categoría
- Vista detallada de productos con múltiples imágenes
- Carrito de compras persistente
- Proceso de checkout intuitivo
- Pagos seguros con MercadoPago
- Confirmación de pedidos en tiempo real por email
- Formulario de contacto

### Para Administradores
- Panel de administración protegido con JWT
- Gestión completa de productos (crear, editar, eliminar)
- Subida de imágenes a Cloudinary
- Gestión de categorías
- Administración de pedidos con actualización de estados
- Control de inventario y stock
- Dashboard con métricas

## Stack Tecnológico

### Backend (`/back`)
- **Framework**: NestJS 11 (Node.js con TypeScript)
- **Base de datos**: PostgreSQL
- **ORM**: Prisma
- **Autenticación**: JWT + Passport
- **Almacenamiento**: Cloudinary (imágenes)
- **Pagos**: MercadoPago API
- **Emails**: Nodemailer
- **Otros**: bcrypt, class-validator, PDFKit

### Frontend (`/front`)
- **Framework**: React 19 con TypeScript
- **Build Tool**: Vite
- **Estilos**: TailwindCSS
- **Routing**: React Router DOM v7
- **Estado**: Zustand
- **Formularios**: React Hook Form + Zod
- **Pagos**: MercadoPago SDK React
- **HTTP**: Axios
- **Iconos**: Lucide React

## Arquitectura del Proyecto

```
ecommerce-fullstack/
├── back/                    # Backend (NestJS)
│   ├── src/
│   │   ├── auth/           # Autenticación JWT
│   │   ├── categories/     # Gestión de categorías
│   │   ├── products/       # CRUD de productos
│   │   ├── orders/         # Gestión de pedidos
│   │   ├── cloudinary/     # Subida de imágenes
│   │   ├── mercado pago/   # Integración de pagos
│   │   ├── email/          # Servicio de emails
│   │   └── contact/        # Formulario de contacto
│   ├── prisma/             # Schemas y migraciones
│   └── README.md           # Documentación del backend
│
├── front/                   # Frontend (React + Vite)
│   ├── src/
│   │   ├── components/     # Componentes reutilizables
│   │   ├── pages/          # Páginas de la aplicación
│   │   ├── services/       # Servicios API
│   │   ├── stores/         # Estado global (Zustand)
│   │   └── layouts/        # Layouts principales
│   └── README.md           # Documentación del frontend
│
└── README.md               # Este archivo
```

## Prerequisitos

Antes de comenzar, asegúrate de tener instalado:

- **Node.js** v18 o superior
- **PostgreSQL** v14 o superior
- **npm** o **yarn**
- **Git**

También necesitarás crear cuentas en:
- [Cloudinary](https://cloudinary.com/) - Para almacenamiento de imágenes
- [MercadoPago](https://www.mercadopago.com/developers) - Para procesamiento de pagos
- Servicio SMTP (Gmail recomendado) - Para envío de emails

## Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone <url-del-repositorio>
cd ecommerce-fullstack
```

### 2. Configurar el Backend

```bash
# Navegar al directorio del backend
cd back

# Instalar dependencias
npm install

# Copiar archivo de ejemplo de variables de entorno
cp .env.example .env

# Editar .env con tus credenciales
# Incluye: DATABASE_URL, JWT_SECRET, Cloudinary, MercadoPago, Email

# Generar cliente de Prisma
npm run prisma:generate

# Ejecutar migraciones de base de datos
npm run prisma:migrate

# (Opcional) Poblar la base de datos con datos de ejemplo
npm run prisma:seed
```

**Configuración importante del `.env` del backend:**
- `DATABASE_URL`: Cadena de conexión a PostgreSQL
- `JWT_SECRET`: Clave secreta para tokens JWT
- `CLOUDINARY_*`: Credenciales de Cloudinary
- `MERCADOPAGO_ACCESS_TOKEN`: Token de acceso (usar TEST para desarrollo)
- `EMAIL_*`: Configuración SMTP para emails

Ver documentación completa en [`back/README.md`](./back/README.md)

### 3. Configurar el Frontend

```bash
# Navegar al directorio del frontend (desde la raíz)
cd front

# Instalar dependencias
npm install

# Copiar archivo de ejemplo de variables de entorno
cp .env.example .env

# Editar .env con la URL del backend y credenciales de MercadoPago
```

**Configuración del `.env` del frontend:**
- `VITE_API_URL`: URL del backend (ej: `http://localhost:3000/api`)
- `VITE_MERCADOPAGO_PUBLIC_KEY`: Public key de MercadoPago (TEST para desarrollo)
- `VITE_STORE_NAME`: TechStore

Ver documentación completa en [`front/README.md`](./front/README.md)

## Ejecución en Desarrollo

### Iniciar el Backend

```bash
cd back
npm run start:dev
```

El backend estará disponible en: `http://localhost:3000`

### Iniciar el Frontend

En otra terminal:

```bash
cd front
npm run dev
```

El frontend estará disponible en: `http://localhost:5173`

## Uso de la Aplicación

### Acceso como Cliente

1. Abrir `http://localhost:5173`
2. Navegar por el catálogo de productos
3. Agregar productos al carrito
4. Proceder al checkout
5. Completar información y realizar pago con MercadoPago
6. Recibir confirmación por email

### Acceso como Administrador

1. Ir a `http://localhost:5173/admin/login`
2. Iniciar sesión con credenciales de administrador
3. Acceder al panel de administración
4. Gestionar productos, categorías y pedidos

**Nota**: El primer administrador debe crearse manualmente en la base de datos o mediante el seed de Prisma.

## Flujo de Datos

```
Cliente → Frontend (React)
         ↓
    API REST (NestJS)
         ↓
    Base de datos (PostgreSQL)

Integraciones externas:
- Cloudinary (almacenamiento de imágenes)
- MercadoPago (procesamiento de pagos + webhooks)
- SMTP (envío de emails transaccionales)
```

## Build para Producción

### Backend
```bash
cd back
npm run build
npm run start:prod
```

### Frontend
```bash
cd front
npm run build
# Los archivos compilados estarán en /front/dist
```

## Despliegue

### Recomendaciones de Hosting

**Backend:**
- Railway
- Render
- Heroku
- DigitalOcean
- AWS EC2

**Frontend:**
- Vercel (recomendado para Vite)
- Netlify
- Cloudflare Pages
- AWS S3 + CloudFront

**Base de datos:**
- Railway (PostgreSQL)
- Supabase
- ElephantSQL
- AWS RDS

### Configuración para Producción

1. Configurar variables de entorno de producción
2. Usar credenciales de producción de MercadoPago
3. Configurar CORS con el dominio del frontend
4. Habilitar HTTPS en ambos servicios
5. Configurar webhooks de MercadoPago con la URL pública

## Estructura de Base de Datos

El proyecto utiliza PostgreSQL con Prisma ORM. Modelos principales:

- **Admin**: Usuarios administradores con autenticación
- **Category**: Categorías de productos
- **Product**: Productos con imágenes, precio, stock
- **Order**: Pedidos con información del cliente
- **OrderItem**: Detalle de productos en cada pedido

Ver el schema completo en `back/prisma/schema.prisma`

## Seguridad

- Autenticación JWT con tokens de corta duración
- Contraseñas hasheadas con bcrypt
- Validación de datos en frontend y backend
- CORS configurado específicamente
- Variables sensibles en archivos `.env` (nunca en el código)
- Sanitización de inputs
- Rutas administrativas protegidas con guards

## Solución de Problemas Comunes

### No se conecta el frontend con el backend
- Verificar que ambos servicios estén corriendo
- Verificar la variable `VITE_API_URL` en el frontend
- Revisar configuración de CORS en el backend

### Error con las migraciones de Prisma
```bash
cd back
npm run prisma:reset  # ¡Cuidado! Esto borra todos los datos
npm run prisma:migrate
```

### Error con MercadoPago
- Verificar que las credenciales sean correctas (TEST vs PROD)
- Revisar que el webhook esté configurado correctamente
- Consultar logs del backend para ver respuestas de la API

### Problemas con emails
- Si usas Gmail, necesitas una "App Password"
- Verificar configuración SMTP en el `.env`
- Revisar que el puerto 587 no esté bloqueado

## Documentación Adicional

- [Documentación detallada del Backend](./back/README.md)
- [Documentación detallada del Frontend](./front/README.md)

---

Desarrollado con ❤️ usando NestJS y React
