# 🧪 Test-QA-RWA - Cypress Real World App Test Suite

[![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=for-the-badge&logo=playwright&logoColor=white)](https://playwright.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)

## 📋 Descripción del Proyecto

Este proyecto es una suite de pruebas automatizadas de QA desarrollada para evaluar la **Cypress Real World App (RWA)**, diseñada específicamente como un test técnico para entrevistas de trabajo en posiciones de QA Engineer (Junior y Semi-Senior).

La suite incluye pruebas tanto de **API** como de **UI** utilizando **Playwright** con **TypeScript**, implementando patrones de diseño modernos como Page Object Model y helpers de base de datos.

## 🎯 Objetivos del Proyecto

- ✅ Demostrar habilidades en automatización de pruebas E2E
- ✅ Implementar pruebas de API robustas y completas
- ✅ Aplicar mejores prácticas de QA automation
- ✅ Mostrar conocimiento en herramientas modernas de testing
- ✅ Validar funcionalidades críticas de una aplicación financiera

## 🏗️ Arquitectura del Proyecto

```
Test-QA-RWA/
├── config/                    # Configuraciones y helpers
│   ├── database-helper.ts     # Helper para manejo de datos de prueba
│   ├── env.ts                 # Configuración de variables de entorno
│   └── test-data.ts          # Datos de prueba estáticos y dinámicos
├── tests/
│   ├── api/                   # Pruebas de API
│   │   ├── auth.spec.ts      # Tests de autenticación
│   │   └── transactions.spec.ts # Tests de transacciones
│   └── ui/                    # Pruebas de interfaz de usuario
│       ├── pages/             # Page Object Models
│       │   ├── login.page.ts
│       │   ├── contacts.page.ts
│       │   ├── feed.page.ts
│       │   └── payment.page.ts
│       ├── e2e.payment.spec.ts # Tests E2E de flujo de pagos
│       └── smoke.spec.ts      # Tests de smoke/funcionalidad básica
├── .env.sample               # Plantilla de variables de entorno
├── playwright.config.ts      # Configuración de Playwright
├── package.json
└── README.md
```

## 🚀 Setup Local

### Prerrequisitos

- **Node.js** v18.0.0 o superior
- **npm** o **yarn**
- **Git**

### 1. Configurar la Cypress Real World App

Clona la app RWA en una carpeta hermana o dentro de `./.rwa/`:

```bash
git clone https://github.com/cypress-io/cypress-realworld-app ./.rwa
cd ./.rwa
corepack enable || true
yarn install --frozen-lockfile
```

Levanta la app (cliente + API):
```bash
# Si el proyecto cambia scripts, ajusta este comando
yarn dev &

# Esperar a que levante en http://localhost:3000
npx wait-on http-get://localhost:3000
```

### 2. Configurar el Proyecto de Tests

Vuelve a este repo e instala dependencias de tests:
```bash
cd ..
npm ci
```

### 3. Configurar Variables de Entorno

Copia `.env.sample` → `.env` y revisa `BASE_URL`:
```bash
cp .env.sample .env
```

Contenido de `.env.sample`:
```env
# URL base de la app (local por defecto)
BASE_URL=http://localhost:3000

# Timeouts en ms
PW_TIMEOUT=30000
PW_EXPECT_TIMEOUT=8000

# Contraseña de prueba (requerida)
TEST_PASSWORD=s3cret
```

### 4. Instalar Navegadores y Ejecutar Tests

```bash
# Instalar navegadores de Playwright
npx playwright install --with-deps

# Ejecutar diferentes tipos de tests
npm run test:smoke    # Tests básicos de funcionalidad
npm run test:api      # Tests de API
npm run test:ui       # Tests de interfaz de usuario

# Ver reportes
npm run report        # Abre el reporte HTML
```

> **Nota:** Si prefieres usar una instancia remota del RWA (cuando esté disponible), setea `BASE_URL` al host remoto y omite el paso de levantar la app.

## 🎮 Comandos Disponibles

| Comando | Descripción |
|---------|-------------|
| `npm run test:ui` | Ejecuta todas las pruebas de UI con reporte HTML |
| `npm run test:api` | Ejecuta todas las pruebas de API con reporte HTML |
| `npm run test:smoke` | Ejecuta solo las pruebas de smoke testing |
| `npm run ci` | Ejecuta todas las pruebas (ideal para CI/CD) |
| `npm run report` | Abre el reporte HTML de la última ejecución |

## ⚙️ Configuración

### Variables de Entorno (`config/env.ts`)

```typescript
import 'dotenv/config';

export const config = {
  baseURL: process.env.BASE_URL || 'http://localhost:3000',
  timeout: Number(process.env.PW_TIMEOUT || 30000),
  expectTimeout: Number(process.env.PW_EXPECT_TIMEOUT || 8000),
};
```

### Configuración de Playwright (`playwright.config.ts`)

```typescript
import { defineConfig, devices } from '@playwright/test';
import { config as app } from './config/env';

export default defineConfig({
  testDir: './tests',
  timeout: app.timeout,
  expect: { timeout: app.expectTimeout },
  use: {
    baseURL: app.baseURL,
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
  reporter: [['html', { open: 'never' }]],
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
  ],
});
```

## 📊 Tipos de Pruebas Implementadas

### 🔐 Pruebas de API (Authentication)
- **Login exitoso** con usuarios reales de la base de datos
- **Validación de credenciales** inválidas
- **Validación de estructura** de requests
- **Pruebas de performance** y tiempo de respuesta
- **Pruebas de concurrencia** y rate limiting
- **Validación de headers** y content-type

### 💰 Pruebas de API (Transactions)
- **Autenticación requerida** para endpoints protegidos
- **Búsqueda y paginación** de usuarios
- **Actualización parcial** de datos de usuario (PATCH)
- **Validación de estructura** de respuestas

### 🖥️ Pruebas de UI (Smoke Tests)
- **Carga correcta** de la aplicación
- **Login/Logout** funcional
- **Navegación principal** entre secciones
- **Feed de transacciones** y estados vacíos
- **Validación de formularios**
- **Responsive design** (móvil/escritorio)
- **Performance** de carga de páginas

### 🔄 Pruebas E2E (Payment Flow)
- **Flujo completo de pago** con múltiples usuarios
- **Navegación entre feeds** (personal, público, contactos)
- **Validación de transacciones** en tiempo real
- **Manejo de datos dinámicos** y únicos

## 🏛️ Patrones de Diseño Utilizados

### Page Object Model (POM)
Cada página de la aplicación tiene su propia clase con métodos específicos:
- `LoginPage` - Manejo de autenticación
- `ContactsPage` - Gestión de contactos
- `PaymentPage` - Procesamiento de pagos
- `FeedPage` - Visualización de transacciones

### Database Helper Pattern
Clase utilitaria para interactuar con datos de prueba:
- Carga dinámica de usuarios válidos
- Filtrado de datos por criterios específicos
- Generación de datos de prueba únicos

### Configuration Management
Centralización de configuraciones y variables de entorno:
- Separación de ambientes (dev, staging, prod)
- Manejo seguro de credenciales
- Timeouts y configuraciones de Playwright

## 📈 Métricas y Reportes

El proyecto genera reportes detallados en múltiples formatos:
- **HTML Report** - Visualización interactiva con screenshots
- **JSON Report** - Para integración con herramientas de CI/CD
- **JUnit XML** - Compatible con sistemas de integración continua

### Características de Reporting:
- 📸 Screenshots automáticos en fallos
- 🎥 Videos de pruebas fallidas
- 📊 Métricas de tiempo de ejecución
- 🔍 Trazas detalladas para debugging

## 🔧 Configuración Avanzada

### Ejecución en Múltiples Navegadores
El proyecto está configurado para ejecutarse en:
- **Chromium** (Chrome/Edge)
- **Firefox**
- **WebKit** (Safari)

### Configuración de CI/CD
```yaml
# Ejemplo para GitHub Actions
- name: Run Playwright Tests
  run: npm run ci
- name: Upload test results
  uses: actions/upload-artifact@v3
  with:
    name: playwright-report
    path: playwright-report/
```

## 🎯 Casos de Uso Cubiertos

### Funcionalidades Críticas Validadas:
1. **Autenticación de usuarios** - Login/Logout seguro
2. **Gestión de contactos** - Búsqueda y selección
3. **Procesamiento de pagos** - Transferencias entre usuarios
4. **Visualización de transacciones** - Feeds y historial
5. **Validaciones de formularios** - Campos requeridos y formatos
6. **Responsive design** - Compatibilidad móvil/escritorio

## 🚨 Manejo de Errores y Debugging

### Estrategias Implementadas:
- **Retry automático** en pruebas fallidas (CI: 2 reintentos)
- **Screenshots** en fallos para análisis visual
- **Videos** de sesiones completas para debugging
- **Logs detallados** con timestamps
- **Timeouts configurables** por tipo de acción

## 📚 Documentación Adicional

Para información más detallada, consulta la [Wiki del proyecto](./wiki/) que incluye:
- Guías de instalación paso a paso
- Ejemplos de uso avanzado
- Troubleshooting común
- Mejores prácticas de QA
- Patrones de diseño explicados

## 🤝 Contribución y Mejoras

Este proyecto fue desarrollado como demostración técnica, pero está abierto a mejoras:

### Posibles Extensiones:
- [ ] Integración con herramientas de CI/CD
- [ ] Pruebas de accesibilidad (a11y)
- [ ] Pruebas de performance con Lighthouse
- [ ] Integración con herramientas de monitoring
- [ ] Pruebas de seguridad automatizadas

## 📞 Contacto

Desarrollado como parte de un proceso de entrevista técnica para posición de QA Engineer.

---

**Nota**: Este proyecto demuestra competencias en automatización de pruebas, patrones de diseño, y mejores prácticas de QA para aplicaciones web modernas.