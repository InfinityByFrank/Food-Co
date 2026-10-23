# 🍽️ Food&Co — Una red social para quienes viven para comer

🌐 [English](README.md) · **Español**

Un producto completo: una **app móvil** (iOS y Android) donde la gente publica platillos, reseña restaurantes y comparte recetas; un **dashboard web de administración** para moderar contenido, gestionar restaurantes y monitorear el almacenamiento; y una **landing bilingüe** que además sirve las páginas públicas de contenido compartido de la app. Diseñado y desarrollado por Infinity de principio a fin.

<p align="center">
  <img src="assets/movil-feed.png" width="200" alt="Feed con una publicación y tarjeta del restaurante" />
  <img src="assets/movil-perfil.png" width="200" alt="Perfil" />
  <img src="assets/movil-receta.png" width="200" alt="Receta con ingredientes y porciones" />
</p>

---

## 🎯 Sobre el proyecto

Las apps de reseñas son frías: calificaciones sueltas y poca comunidad. El objetivo era que descubrir dónde comer se sintiera **social, cercano y visual**, y que los restaurantes se dieran a conocer a través de la gente que los visita. Además, una app social crece rápido — miles de fotos, usuarios, reportes y costos de almacenamiento — así que el equipo necesitaba **ver, moderar y decidir** desde un solo lugar.

> **Rol:** diseñé y desarrollé la app, el dashboard y la landing de punta a punta.

| Parte | Qué es |
|---|---|
| 📱 **App móvil** | iOS + Android desde una sola base de código |
| 🖥️ **Dashboard** | Panel web para moderación, restaurantes, soporte y analítica de almacenamiento |
| 🌐 **Landing** | Sitio bilingüe, páginas legales, enlaces universales y páginas públicas de contenido |

## 📱 App móvil

**Stack:** Expo SDK 54 (nueva arquitectura) · React Native 0.81 · React 19 · Expo Router · NativeWind v4 · Zustand · TanStack Query · Reanimated 4 · Skia · FlashList · Gorhom Bottom Sheet · react-native-maps · socket.io-client · i18next · Sentry · EAS

- **Descubre:** feed con carruseles de fotos, descripción y una **tarjeta del restaurante** (nombre, tipo de lugar, zona) que lleva directo al local; restaurantes cercanos y mapa interactivo.
- **Comparte:** elige varias fotos, recorta, aplica filtros, gira o voltea y publica sin salir de la app.
- **Conecta:** perfil con galería, seguidores y pestañas de publicaciones, me gusta, guardados y lugares; likes, comentarios, guardados y listas; notificaciones push.
- **Cocina:** recetas de la comunidad con ingredientes tipo checklist, tiempos de preparación/cocción/reposo, porciones ajustables y cambio de unidades métrico/US.
- **Accesible:** tamaño de texto ajustable (S/M/L/XL), alto contraste y movimiento reducido, persistidos por usuario. Modo oscuro.
- **Autenticación:** correo, Google y Apple, con tokens en almacenamiento seguro.

<p align="center">
  <img src="assets/movil-feed-carrusel.png" width="200" alt="Feed con carrusel" />
  <img src="assets/movil-nueva-publicacion.png" width="200" alt="Nueva publicación: recorte, filtros, giro, volteo" />
  <img src="assets/movil-accesibilidad.png" width="200" alt="Ajustes de accesibilidad" />
</p>

> Las capturas usan mi cuenta personal.

### Decisiones
- **Rutas por carpetas de acceso** (`(auth)`, `(root)`, `(info)`): las guardas viven en un único `_layout` por grupo, sin repetirlas en cada pantalla.
- **Zustand para estado de auth/UI + TanStack Query para datos del servidor.**
- **Sistema de diseño propio** con NativeWind y modo oscuro; FlashList y `expo-image` para listas largas con imágenes; Reanimated para animaciones.
- **Fotos HEIC de iPhone:** se procesan en el cliente con `expo-image-manipulator` y se convierten a WebP en el servidor (`sharp` + `heic-convert`).
- **Fallback elegante** cuando falta la API key de Google Maps en Android, en vez de fallar.
- Configuración de mapas, push y login social documentada con guías escritas para el cliente.

## 🖥️ Dashboard de administración

**Stack:** Astro 6 (SSR en Cloudflare) · React 18 · Tailwind CSS 4 · ApexCharts · FullCalendar · Leaflet · jVectorMap · Flatpickr · Framer Motion · middleware de autenticación

Módulos: analítica, usuarios (+ estadísticas), administradores, publicaciones, recetas, restaurantes (+ estadísticas), **importación desde Google Places**, boletos de soporte y servicios de archivos/almacenamiento. Buscador global, menú lateral colapsable, modo claro/oscuro.

### Caso: R2 Analytics
**Problema:** no había visibilidad del crecimiento ni del costo del almacenamiento de imágenes. **Solución:** un módulo en el backend que consulta la API de analítica de Cloudflare y un panel con tendencia de GB/objetos, tasa de éxito vs. error, subidas diarias, operaciones por acción y selector de rango de 7/14/30 días.

<p align="center">
  <img src="assets/dashboard-r2-analytics.png" width="460" alt="R2 Analytics: almacenamiento, objetos, tendencia, éxito vs error" />
  <img src="assets/dashboard-r2-operaciones.png" width="460" alt="Subidas diarias y operaciones por acción" />
</p>

### Caso: importar restaurantes desde Google
Capturar cada restaurante a mano es lento, así que construí un flujo de importación con Google Places: se elige el centro de búsqueda en un mapa, densidad/radio y tipos de lugar, con una estimación de costo antes de ejecutar.

<p align="center"><img src="assets/dashboard-importar-google.png" width="600" alt="Importar restaurantes desde Google Places" /></p>

## 🌐 Landing

**Stack:** Astro 6 · React 19 · Tailwind CSS 4 (`@tailwindcss/vite`) · Cloudflare (`@astrojs/cloudflare`) · i18n propio

Bilingüe (ES/EN), responsiva, con modo claro/oscuro y un módulo por sección (Nav, Hero, Features, HowItWorks, SocialFeed, Categories, Stats, DownloadCTA, HelpCenter, Footer).

- **Enlaces compartibles:** cada publicación, restaurante, receta y perfil tiene su página pública (`/post/[id]`, `/restaurant/[id]`, `/recipe/[slug]`, `/u/[username]`) que invita a abrir la app.
- **Enlaces universales** iOS y Android (`apple-app-site-association` y `assetlinks.json` generados como rutas Astro).
- **Legal y cumplimiento de tiendas:** términos, privacidad, normas de comunidad, aviso, política de contenido, centro de ayuda y **página para eliminar la cuenta** (requisito de las tiendas), todo en dos idiomas.

<p align="center">
  <img src="assets/landing-hero.png" width="460" alt="Portada de la landing" />
  <img src="assets/landing-caracteristicas.png" width="460" alt="Seis características" />
</p>
<p align="center">
  <img src="assets/landing-como-funciona.png" width="460" alt="Cómo funciona: tres pasos" />
  <img src="assets/landing-comunidad.png" width="460" alt="Sección de comunidad en modo oscuro" />
</p>

> Las cifras de la landing (p. ej. "+128 reseñas") y los logos de "mencionado por" son maquetas, no resultados reales.

## 🌱 Notas honestas

- El proyecto móvil todavía no tiene pruebas automatizadas.
- La compilación de Android necesita una API key de Google Maps.

## 📄 Nota

Este repositorio contiene **solo documentación**; el código fuente es privado. Con gusto lo recorro en una entrevista.

## 📬 Contacto

**Infinity dev por Frank** · [GitHub](https://github.com/TheGeistGamer) · [Infinity](https://infinitydev.agency)
