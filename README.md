# Sistema 18 años Evelyn — Invitación + Panel del festejado + Panel de recepción

Sistema completo con backend real (Node/Express) para que el panel del festejado
y el panel de recepción compartan la misma base de datos de invitados.

## Estructura

```
evelyn-sistema/
├── server.js              → servidor Express + API
├── package.json
├── db.json                → "base de datos" (archivo JSON, se crea solo)
└── public/
    ├── invitacion.html    → invitación para los invitados
    ├── panel-festejado.html   → para que Evelyn gestione invitados
    └── panel-recepcion.html   → para escanear QR el día del evento
```

## Cómo funciona

1. En **panel-festejado.html**, agregas cada invitado (nombre + núm. de pases).
   El sistema genera automáticamente un **link único**, ej:
   `https://tu-app.onrender.com/invitacion.html?id=b62b2d48`
2. Le mandas ese link a cada invitado (por WhatsApp, por ejemplo). Cuando lo abre,
   la invitación se personaliza sola con su nombre y su número de pases, y su
   pase QR queda ligado a su propio link.
3. Cuando el invitado confirma asistencia (RSVP), el panel del festejado se
   actualiza con su estado en tiempo real.
4. El día del evento, abres **panel-recepcion.html** en un celular o tablet en
   la puerta. Escaneas el QR de cada invitado con la cámara y el sistema
   marca su entrada automáticamente — y avisa si alguien ya había entrado.

## ⚠️ Antes de publicar — 2 cosas que debes cambiar

1. **Número de WhatsApp del RSVP**: en `public/invitacion.html` busca el
   comentario `⚠️ Diego: reemplaza el número de WhatsApp` y cambia
   `521234567890` por el número real (formato `52` + 10 dígitos, sin espacios).
2. **Hora del evento**: puse `8:00 pm` como hora de bienvenida (ajustable en el
   itinerario) y la cuenta regresiva usa `2026-10-10T20:00:00`. Si la hora real
   es otra, búscala en `invitacion.html` (sección `datebox`/`countdown`/
   `eventDate` y los horarios del itinerario) y ajústala.

## Probar en tu computadora (opcional, antes de subirlo)

```bash
npm install
npm start
```

Abre `http://localhost:3000/invitacion.html`,
`http://localhost:3000/panel-festejado.html` y
`http://localhost:3000/panel-recepcion.html`.

## Subir a GitHub

```bash
git init
git add .
git commit -m "Sistema completo 18 años Evelyn"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git push -u origin main
```

## Desplegar en Render (con base de datos real y gratis)

### 1. Crea la base de datos primero

1. En Render, click en **"New +" → "PostgreSQL"**
2. Ponle un nombre, ej. `evelyn-18-db`
3. **Instance Type:** Free
4. Click **"Create Database"**
5. Espera 1-2 minutos a que quede lista. Cuando esté lista, busca el campo
   **"Internal Database URL"** y cópialo (empieza con `postgres://...`)

### 2. Crea el servicio web

1. Click en **"New +" → "Web Service"**
2. Conecta tu repositorio de GitHub (el que subiste)
3. Configura:
   - **Name:** `evelyn-18-sistema`
   - **Runtime:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Instance Type:** Free
4. Antes de crear, baja hasta **"Environment Variables"** y agrega una:
   - **Key:** `DATABASE_URL`
   - **Value:** (pega el "Internal Database URL" que copiaste en el paso 1)
5. Click en **"Create Web Service"**

Con eso, tus invitados quedan guardados en PostgreSQL — **no se pierden** aunque
el servidor se duerma y despierte, ni aunque hagas otro `git push` después.

### Cómo saber si quedó bien conectado

En los logs de Render (pestaña "Logs" de tu Web Service), deberías ver:
```
✅ Conectado a PostgreSQL — los datos son permanentes.
```
Si en cambio ves `⚠️ Sin DATABASE_URL...`, significa que la variable de entorno
no quedó bien puesta — revisa el paso 4.

### Nota sobre el plan gratis de PostgreSQL en Render

La base de datos gratis de Render **expira a los 90 días** (te avisan por correo
antes de que pase). Como el evento es el 10 de octubre de 2026, sobra tiempo de
margen. Si más adelante quieres seguir usando el sistema para otro evento, en
ese momento se puede migrar a otra base gratuita o subir a un plan pagado.

## Notas de diseño

- Paleta de colores: negro + plata + brillos blancos (tema "Bola de disco")
- Fotos: 3 fotos de infancia de Evelyn en la sección de historia, y su foto
  actual como imagen principal del hero
- No se incluyó música de fondo (no se especificó una canción; puedes agregar
  un `<audio>` tú mismo en `invitacion.html` con una pista propia si quieres)
