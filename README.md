# HealthSync

**Prototipo web de monitoreo de signos vitales: un backend en Node.js expone las lecturas guardadas en MongoDB y un frontend en Next.js las muestra y actualiza periódicamente.**

![Next.js](https://img.shields.io/badge/Next.js-frontend-000000?logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-UI-61DAFB?logo=react&logoColor=black)
![Express](https://img.shields.io/badge/Express-API%20REST-000000?logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)
![Estado](https://img.shields.io/badge/estado-prototipo-orange)

> **English summary:** Full-stack prototype for remote vital-sign monitoring. A Node.js/Express REST API serves readings stored in MongoDB (temperature, mean arterial pressure, heart rate, glucose), a simulator script writes new readings every 10 seconds, and a Next.js/React dashboard polls the API and refreshes automatically. Includes landing, login and sign-up pages.

---

## ¿Qué hace?

HealthSync busca que un paciente o su médico consulte sus signos vitales en tiempo casi real desde el navegador. El prototipo cubre el flujo completo de los datos:

```
Simulador de sensores ──(cada 10 s)──▶ MongoDB ◀── API Express (GET /api/signos) ◀──(cada 13 s)── Dashboard Next.js
```

| Signo vital | Rango simulado |
|---|---|
| Temperatura | 35–37 °C |
| Presión arterial media | 90–120 mmHg |
| Frecuencia cardíaca | 60–100 lpm |
| Glucosa | 70–110 mg/dL |

## Funcionalidades

- **API REST** (`backend/backend/server.js`): Express + Mongoose con el endpoint `GET /api/signos`, CORS habilitado y manejo de errores.
- **Simulador de datos** (`backend/backend/actualizarDatos.js`): genera lecturas aleatorias dentro de rangos fisiológicos y las guarda en MongoDB cada 10 segundos (`upsert`, con *timestamps*), sustituyendo a un dispositivo físico.
- **Dashboard** (`src/app/frontend`): consulta la API con Axios cada 13 segundos y renderiza los signos sin recargar la página (`useEffect` + `setInterval`, con limpieza del intervalo).
- **Páginas de inicio, inicio de sesión y registro** con el App Router de Next.js y CSS Modules.
- **Componente de gráfica** (`src/app/Grafica`) con Chart.js para visualizar la frecuencia cardíaca en el tiempo (en desarrollo).

## Stack

| Capa | Tecnología |
|---|---|
| Frontend | Next.js (App Router), React, CSS Modules, Axios |
| Backend | Node.js, Express, CORS |
| Base de datos | MongoDB con Mongoose |
| Visualización | Chart.js / react-chartjs-2 |

## Estructura

```
.
├── backend/backend/
│   ├── server.js            # API REST (puerto 5000)
│   ├── actualizarDatos.js   # Simulador de lecturas de signos vitales
│   └── package.json
├── src/app/
│   ├── page.js              # Página de bienvenida
│   ├── iniciodesesion/      # Inicio de sesión
│   ├── registro/            # Registro de usuario
│   ├── frontend/            # Dashboard de signos vitales
│   └── Grafica              # Componente de gráfica (en desarrollo)
└── public/                  # Imágenes
```

## Cómo ejecutarlo

Requisitos: Node.js 18+ y MongoDB corriendo en `localhost:27017`.

```bash
# 1. Backend
cd backend/backend
npm install
node actualizarDatos.js     # terminal 1: genera datos simulados
node server.js              # terminal 2: API en http://localhost:5000/api/signos

# 2. Frontend (desde la raíz del proyecto)
npm install next react react-dom axios
npx next dev                # http://localhost:3000
```

El dashboard está en `http://localhost:3000/frontend`.

## Próximos pasos

Este es un prototipo académico. Lo que falta para que sea una aplicación completa:

- [ ] Autenticación real (hoy el inicio de sesión es una validación local de demostración) y conectar el formulario de registro a la base de datos.
- [ ] Integrar la gráfica de frecuencia cardíaca en el dashboard.
- [ ] Guardar un historial de lecturas por paciente en lugar de un único registro.
- [ ] Mover la cadena de conexión de MongoDB a variables de entorno.
- [ ] Sustituir el simulador por lecturas de un dispositivo real (p. ej. un microcontrolador con sensores).

## Lo que aprendí

- A diseñar una arquitectura cliente-servidor completa: base de datos, API REST y frontend que la consume.
- A modelar datos con Mongoose y a simular una fuente de datos para desarrollar sin depender del hardware.
- A manejar estado y efectos en React (`useState`, `useEffect`) para actualizar la interfaz periódicamente.

---

 **Contacto:** [GitHub @Turris20](https://github.com/Turris20)
