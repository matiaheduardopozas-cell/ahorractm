# 💸 Ahorra CTM — Wallet Personal de Ingresos y Gastos

> **Control financiero personal inteligente, privado y en tiempo real.**  
> Aplicación web progresiva (PWA) de alto rendimiento, diseñada con arquitectura *Zero-Build* (Vanilla HTML5 / CSS3 / ES6+) y respaldada en la nube por **Google Firebase**.

---

## 🌐 Enlaces de Producción

- **Dominio Principal**: [https://ahorractm.web.app](https://ahorractm.web.app)
- **Dominio Alternativo**: [https://ahorractm.firebaseapp.com](https://ahorractm.firebaseapp.com)
- **Dominio de Proyecto**: [https://wallet-personal-73c03.web.app](https://wallet-personal-73c03.web.app)

---

## 📋 Resumen de la Sesión y Cambios Implementados

### 1. 🔐 Autenticación de Usuarios (Google Sign-In)
- **Proveedor OAuth**: Se habilitó y desplegó Google Sign-In mediante Firebase Authentication.
- **SDK Integrado**: `firebase-auth-compat.js` (v10.13.1).
- **Flujo de Acceso**: Ventana emergente (*Popup*) con mecanismo de contingencia automática (*Redirect*) para navegadores móviles con bloqueo de popups.
- **Manejo de Sesión**: Cierre de sesión seguro con liberación de escuchadores de Firestore en memoria para evitar consumo innecesario de recursos.

### 2. 🗄️ Aislamiento y Privacidad Multi-Usuario en Firestore
- Se eliminó la colección compartida global (`shared_wallet`).
- Cada usuario que inicia sesión cuenta con un espacio de datos **100% aislado** bajo su propio UID de Google:
  ```text
  users/{userId}                         <- Perfil, temas y categorías personalizadas
  users/{userId}/transactions/{txId}     <- Movimientos exclusivos del usuario
  ```
- **Migración de Datos**: Se migraron exitosamente las 30 transacciones históricas, categorías personalizadas (*Arriendo, Suscripciones, Vicios, Compras Pendejas, etc.*) y tema visual a la cuenta de Google del propietario (`matiaheduardopozas@gmail.com`).

### 3. 🛡️ Reglas de Seguridad en Cloud Firestore (`firestore.rules`)
- Se reemplazó el acceso abierto (`allow read, write: if true;`) por reglas de seguridad de nivel de producción:
  - **Propiedad Estricta**: Solo el usuario autenticado puede leer, crear, modificar o eliminar sus propios documentos (`request.auth.uid == userId`).
  - **Validación de Esquema**: Se verifican tipos de datos, montos numéricos positivos, tipos de movimiento válidos (`income`, `expense`, `savings`), método de pago (`cash`, `card`) y límites de longitud de caracteres.
  - **Denegación por Defecto**: Cualquier intento de acceso anónimo o cruzado entre usuarios queda denegado automáticamente a nivel de infraestructura.

### 4. 🎨 Identidad Visual y Marca: "Ahorra CTM"
- **Logotipo Oficial**: Procesamiento e integración del logo oficial (bolsa rota goteando monedas de oro con contorno verde esmeralda) en alta resolución:
  - `logo.png`: Uso en cabeceras y tarjeta de bienvenida.
  - `icon.png`: Ícono para PWA y dispositivos móviles (pantalla de inicio).
- **Tarjeta de Inicio / Login**:
  - Título: **Ahorra CTM**
  - Subtítulo: *Wallet personal de ingresos y gastos*
  - Viñetas de valor: Datos 100% aislados y privados, Guardado automático en la nube, Gráficos y análisis de gastos.
  - Botón: *Continuar con Google*.
  - Crédito de autor: *Aplicación diseñada por **Zeykox*** con degradado estilizado.

### 5. ✨ Mejoras de Experiencia de Usuario (UI/UX)
- **Topbar Móvil**: Barra superior fija con logotipo, título, estado de sincronización (`🟢 Sincronizado`) y avatar con acceso directo a Configuración.
- **Sidebar Desktop**: Navegación lateral con cabecera de marca y tarjeta de perfil de usuario al pie con foto, nombre y correo.
- **Saludo Personalizado**: Saludo contextual según la hora del día y el primer nombre del usuario (*"Buenas noches, Matías 👋"*).
- **Selector de Fecha**: Posibilidad de registrar movimientos con fechas pasadas directamente desde el modal.
- **Atajos de Teclado (PC)**: En escritorio, el modal responde a teclas numéricas físicas (`0-9`, `.`), borrado (`Backspace`), guardado (`Enter`) y cierre (`Esc`).
- **Sistema de Notificaciones Toast**: Feedback flotante animado al guardar, editar, eliminar o cerrar sesión.
- **Exportación de Datos**: Botón en Configuración para descargar una copia de seguridad local de todas las transacciones en formato JSON.
- **Sanitización contra XSS**: Funciones de escape HTML para notas y nombres de categorías para prevenir inyección de código.

---

## 📁 Estructura del Proyecto

```text
wallet-web/
├── index.html        # Aplicación SPA completa (HTML5, estilos CSS3 y lógica JS cliente)
├── manifest.json     # Manifiesto PWA para instalación móvil y escritorio (AhorraCTM)
├── icon.png          # Ícono oficial de la aplicación (512x512)
├── logo.png          # Logotipo recortado y centrado
├── firebase.json     # Configuración de despliegue multi-sitio (ahorractm) y OAuth
├── firestore.rules   # Reglas de seguridad auditadas de Cloud Firestore
└── README.md         # Documentación técnica del proyecto
```

---

## 🚀 Comandos de Despliegue y Mantenimiento

Para realizar cambios futuros sin afectar los datos de los usuarios:

```bash
# Desplegar solo cambios de diseño/interfaz (Recomendado):
npx firebase-tools deploy --only hosting

# Desplegar actualizaciones en las reglas de Firestore:
npx firebase-tools deploy --only firestore:rules

# Desplegar cambios en la configuración de autenticación OAuth:
npx firebase-tools deploy --only auth
```

---

*Diseñado y desarrollado por **Zeykox**.*
