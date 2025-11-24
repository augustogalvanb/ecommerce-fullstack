# Frontend - E-Commerce

Aplicación web moderna desarrollada con React y Vite para un sistema completo de comercio electrónico. Incluye catálogo de productos, carrito de compras, integración con MercadoPago y panel de administración.

## Tecnologías Principales

- **Framework**: React 19
- **Build Tool**: Vite
- **Lenguaje**: TypeScript
- **Estilos**: TailwindCSS
- **Routing**: React Router DOM v7
- **Estado Global**: Zustand
- **Formularios**: React Hook Form + Zod
- **Validación**: Zod Schemas
- **Iconos**: Lucide React
- **Pagos**: MercadoPago SDK React
- **HTTP Client**: Axios

## Estructura del Proyecto

```
src/
├── assets/            # Imágenes y recursos estáticos
├── components/        # Componentes reutilizables de la UI
├── config/            # Configuración de la aplicación
├── layouts/           # Layouts principales de la aplicación
├── pages/             # Páginas de la aplicación
│   └── admin/         # Páginas del panel de administración
├── services/          # Servicios para comunicación con la API
├── stores/            # Stores de Zustand para gestión de estado
└── types/             # Definiciones de tipos TypeScript
```

## Características Principales

### Tienda (Cliente)
- Catálogo de productos con filtros por categoría
- Vista detallada de productos
- Carrito de compras persistente
- Proceso de checkout optimizado
- Integración con MercadoPago para pagos seguros
- Confirmación de pedidos en tiempo real
- Formulario de contacto

### Panel de Administración
- Dashboard administrativo protegido
- Gestión completa de productos (CRUD)
- Subida de múltiples imágenes por producto
- Gestión de categorías
- Administración de pedidos
- Actualización de estados de pedidos
- Control de inventario

### UX/UI
- Diseño responsivo (mobile-first)
- Interfaz moderna con TailwindCSS
- Iconografía consistente con Lucide React
- Navegación intuitiva
- Feedback visual en todas las interacciones
- Loading states y error handling

## Prerequisitos

- Node.js (versión 18 o superior)
- npm o yarn
- Backend del proyecto corriendo (ver carpeta `back/`)

## Configuración del Entorno

1. **Navegar al directorio del frontend**:
```bash
cd front
```

2. **Instalar dependencias**:
```bash
npm install
```

3. **Configurar variables de entorno**:

Crear un archivo `.env` basado en `.env.example`:

```env
# URL del backend
VITE_API_URL=http://localhost:3000/api

# Nombre de la tienda
VITE_STORE_NAME=TechStore

# MercadoPago Public Key
VITE_MERCADOPAGO_PUBLIC_KEY=TEST-tu-public-key
```

**Nota importante**: Para obtener las credenciales de MercadoPago:
- Crear cuenta en [MercadoPago Developers](https://www.mercadopago.com/developers)
- Ir a "Tus integraciones" → "Credenciales"
- Usar las credenciales de TEST para desarrollo

## Scripts Disponibles

### Desarrollo
```bash
# Iniciar servidor de desarrollo (puerto 5173 por defecto)
npm run dev
```
La aplicación estará disponible en `http://localhost:5173`

### Producción
```bash
# Compilar TypeScript y construir para producción
npm run build

# Previsualizar build de producción
npm run preview
```

### Linting
```bash
# Ejecutar linter
npm run lint
```

## Estructura de Rutas

### Rutas Públicas
- `/` - Página principal con catálogo de productos
- `/products/:slug` - Detalle de producto
- `/cart` - Carrito de compras
- `/checkout` - Proceso de pago
- `/success` - Confirmación de pedido exitoso
- `/contact` - Formulario de contacto

### Rutas de Administración (Protegidas)
- `/admin/login` - Login de administrador
- `/admin/dashboard` - Panel principal
- `/admin/products` - Gestión de productos
- `/admin/products/new` - Crear nuevo producto
- `/admin/products/:id/edit` - Editar producto
- `/admin/categories` - Gestión de categorías
- `/admin/orders` - Gestión de pedidos

## Gestión de Estado

El proyecto utiliza **Zustand** para la gestión de estado global:

### Auth Store
- Manejo de autenticación de administradores
- Persistencia de token JWT
- Estado de sesión

### Cart Store
- Gestión del carrito de compras
- Persistencia en localStorage
- Cálculo automático de totales
- Actualización de cantidades

### Products Store (opcional)
- Caché de productos
- Filtros y búsquedas

## Servicios API

Los servicios están organizados por recursos en la carpeta `services/`:

- **authService**: Login y autenticación
- **productsService**: CRUD de productos
- **categoriesService**: Gestión de categorías
- **ordersService**: Creación y consulta de pedidos
- **paymentsService**: Integración con MercadoPago
- **contactService**: Envío de mensajes de contacto

Ejemplo de uso:
```typescript
import { productsService } from '@/services/productsService';

// Obtener todos los productos
const products = await productsService.getAll();

// Obtener producto por slug
const product = await productsService.getBySlug('laptop-dell-xps');
```

## Formularios y Validación

Se utiliza **React Hook Form** con **Zod** para validación robusta:

```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const schema = z.object({
  name: z.string().min(3, 'Mínimo 3 caracteres'),
  email: z.string().email('Email inválido'),
  // ... más validaciones
});

const { register, handleSubmit, formState: { errors } } = useForm({
  resolver: zodResolver(schema)
});
```

## Integración con MercadoPago

El checkout utiliza el SDK oficial de MercadoPago:

```typescript
import { initMercadoPago, Wallet } from '@mercadopago/sdk-react';

// Inicializar en el componente
initMercadoPago(import.meta.env.VITE_MERCADOPAGO_PUBLIC_KEY);

// Usar el componente Wallet
<Wallet initialization={{ preferenceId: 'preference-id' }} />
```

## Estilos con TailwindCSS

El proyecto usa TailwindCSS para un diseño moderno y responsivo:

- Configuración personalizada en `tailwind.config.js`
- Clases utilitarias para desarrollo rápido
- Diseño mobile-first
- Sistema de colores y tipografía consistente

## Flujo de Trabajo del Usuario

### Comprador
1. Navegar catálogo de productos
2. Filtrar por categorías
3. Ver detalles del producto
4. Agregar productos al carrito
5. Revisar carrito
6. Proceder al checkout
7. Completar información de envío
8. Pagar con MercadoPago
9. Recibir confirmación del pedido

### Administrador
1. Login en `/admin/login`
2. Acceder al dashboard
3. Gestionar productos (crear, editar, eliminar)
4. Subir imágenes de productos
5. Crear y organizar categorías
6. Ver y procesar pedidos
7. Actualizar estados de pedidos

## Consideraciones de Desarrollo

### Autenticación
- Token JWT almacenado en localStorage
- Interceptor de Axios para incluir token en requests
- Redirección automática si el token expira
- Guards de rutas para proteger páginas administrativas

### Optimización
- Code splitting por rutas
- Lazy loading de componentes pesados
- Imágenes optimizadas con Cloudinary
- Caché de datos con Zustand

### Seguridad
- Validación en cliente y servidor
- Sanitización de inputs
- HTTPS en producción
- Variables de entorno para datos sensibles

## Solución de Problemas

### Error de conexión con el backend
Verifica que:
- El backend está corriendo en `http://localhost:3000`
- La variable `VITE_API_URL` está configurada correctamente
- No hay problemas de CORS

### Error con MercadoPago
Asegúrate de:
- Usar la public key correcta (TEST o PROD)
- El backend tiene configurado el access token correspondiente
- La cuenta de MercadoPago está activa

### Problemas con las imágenes
Las imágenes se cargan desde Cloudinary a través del backend. Verifica que el backend tenga las credenciales correctas.

### Build falla
```bash
# Limpiar node_modules y reinstalar
rm -rf node_modules package-lock.json
npm install

# Verificar versión de Node
node --version  # Debe ser >= 18
```

## Recursos Adicionales

- [Documentación de React](https://react.dev/)
- [Documentación de Vite](https://vitejs.dev/)
- [Documentación de TailwindCSS](https://tailwindcss.com/docs)
- [Documentación de React Router](https://reactrouter.com/)
- [Documentación de Zustand](https://docs.pmnd.rs/zustand/)
- [Documentación de React Hook Form](https://react-hook-form.com/)
- [Documentación de MercadoPago](https://www.mercadopago.com/developers)

## Próximas Mejoras

- [ ] Sistema de búsqueda avanzada
- [ ] Wishlist de productos favoritos
- [ ] Historial de pedidos para clientes
- [ ] Sistema de reviews y ratings
- [ ] Notificaciones push
- [ ] Chat de soporte en vivo
- [ ] Multi-idioma (i18n)
- [ ] Modo oscuro

## Licencia

Private
