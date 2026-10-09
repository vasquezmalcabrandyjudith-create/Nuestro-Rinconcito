# Nuestro Rinconcito💖 — instalación y publicación

Este proyecto es una aplicación web progresiva (PWA) con una interfaz romántica, autenticación por correo, código de pareja, chat en tiempo real, recuerdos con fotos privadas, cartas, juegos y puntos. **Necesitas crear y configurar los servicios antes de que pueda compartirse con tu pareja.** No incluyas nunca la `service_role key` en el navegador.

## 1. Crear el backend gratuito en Supabase

1. Entra a https://supabase.com/ y crea una cuenta/proyecto.
2. Guarda la contraseña de la base de datos en un lugar seguro.
3. En el panel del proyecto, abre **SQL Editor → New query**.
4. Abre `supabase.sql` de esta carpeta, copia todo su contenido, pégalo en SQL Editor y pulsa **Run**.
5. En **Project Settings → API** (o Connect), copia el **Project URL** y la clave pública **publishable/anon**. No uses la clave `service_role`.
6. Abre `app.js` y reemplaza las dos primeras constantes:
   - `PEGA_AQUI_TU_PROJECT_URL` por tu Project URL.
   - `PEGA_AQUI_TU_PUBLISHABLE_OR_ANON_KEY` por la clave pública/publishable/anon.
7. Guarda el archivo. En **Authentication → URL Configuration**, después de publicar, configura el Site URL y las Redirect URLs con el dominio donde alojarás la app.
8. Para pruebas, en **Authentication → Providers → Email**, puedes decidir si exigir confirmación de correo. Para compartir con otra persona, se recomienda dejarla activada y configurar el correo de autenticación.

## 2. Publicar la app para abrirla desde ambos celulares

La opción más sencilla es GitHub + Netlify:

1. Crea una cuenta en https://github.com/ y un repositorio privado o público para el código (no publiques información privada de la pareja; la clave publishable sí está diseñada para el navegador, protegida por RLS).
2. Sube los archivos de esta carpeta a la raíz del repositorio: `index.html`, `app.js`, `manifest.json`, `sw.js`, `supabase.sql`, `LEEME.md` y la carpeta `assets`.
3. En https://www.netlify.com/ importa el repositorio y publica el sitio. Para este proyecto estático no hace falta comando de build ni directorio de salida especial; deja el directorio raíz como publicación.
4. Copia el enlace HTTPS generado. Ábrelo en tu celular para probar. Comparte ese mismo enlace con tu pareja.
5. Para instalar: en Android, abre el enlace en Chrome y elige **Instalar aplicación** o **Añadir a pantalla de inicio**. En iPhone, abre el enlace en Safari, toca **Compartir → Añadir a pantalla de inicio**.

## 3. Vincular a la pareja

1. Cada persona abre el enlace y crea su propia cuenta con un correo distinto.
2. La primera persona inicia sesión y toca **Crear espacio y obtener código**.
3. Comparte el código de seis caracteres en privado con su pareja.
4. La segunda persona crea su cuenta, pega el código en **Ya tengo un código** y pulsa **Unirme al espacio**.
5. Ambos deberían ver el mismo chat, cartas y recuerdos. Prueba primero enviando mensajes desde un celular al otro.

## 4. Pruebas y límites

- Comprueba alta e inicio de sesión, confirmación de correo si está activa, creación de pareja, código inválido, segundo integrante, mensajes en ambos dispositivos, subida de imagen y acceso de un usuario no miembro (que debe ser denegado).
- No compartas la contraseña de tu cuenta. Comparte solamente el código de invitación con tu pareja.
- El bucket de fotos es privado y genera URL firmadas temporales al mostrar imágenes.
- El prototipo incluye juegos con turnos y actividades para realizar por llamada/chat; no todos son partidas sincronizadas interactivas. La puntuación y los contenidos guardados sí se comparten cuando la configuración está bien aplicada.
- Las cartas guardadas son visibles para ambos; el selector “cuándo abrirla” es una etiqueta, no un bloqueo por fecha.
- No hay cifrado de extremo a extremo personalizado. Supabase ofrece transporte seguro y control de acceso, pero no prometas que ni el proveedor pueda acceder a los datos. No subas contenido que no quieras almacenar en el servicio.
- Los puntos se guardan por eventos del cliente; para competición resistente a manipulación, mueve la validación de puntuación al servidor.
- El registro de usuarios está limitado a un espacio de pareja por cuenta. Para cambiar de pareja o eliminar datos, hay que implementar un flujo de salida/eliminación antes de usarlo en producción.

## 5. Archivos

- `index.html`: interfaz y navegación móvil.
- `app.js`: autenticación, chat, juegos, fotos, cartas y personalización.
- `supabase.sql`: tablas, políticas RLS, código de unión y bucket privado.
- `manifest.json`, `sw.js`, `assets/icon.svg`: instalación PWA e icono.

Si cambias el SQL después de crear tablas, revisa las políticas existentes y prueba con dos cuentas antes de compartir contenido real.
