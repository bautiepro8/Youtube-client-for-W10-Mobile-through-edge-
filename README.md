# Cliente de YouTube para Windows 10 Mobile (Edge)

Cliente web de YouTube hecho en un único archivo (`index.html`), pensado para abrirse en **Microsoft Edge de Windows 10 Mobile 1703** y también en cualquier navegador moderno. Usa la **API de datos de YouTube v3** y el reproductor embebido oficial.

**Sitio publicado:** https://julioxhernanz99-debug.github.io/Youtube-client-for-W10-Mobile-through-edge-/

## Características

- **Tendencias** por región y **búsqueda** de videos, con miniatura, duración, vistas y fecha.
- **Paginación** ("Más resultados") en listas, canales y comentarios.
- **Reproductor** con controles propios: barra de progreso, adelantar y atrasar 10 o 30 segundos, play/pausa.
- **Pantalla completa** que oculta los controles propios.
- **Subtítulos** activables o desactivables (el video se recarga y sigue desde el mismo segundo).
- **Selector de calidad** hasta 1080p (mejor esfuerzo, ver limitaciones).
- **Comentarios** ordenados por populares o recientes, con respuestas desplegables.
- **Páginas de canal** con foto, suscriptores y todos sus videos.
- **Idiomas de la interfaz:** español, inglés, portugués y francés.
- El botón físico "atrás" del teléfono vuelve a la pantalla anterior.

## Cómo usarlo

1. Abrí el sitio publicado en Edge.
2. Tocá el engranaje (⚙) y pegá tu **clave de API** de YouTube. Se guarda solo en el navegador del dispositivo.
3. Elegí región e idioma y tocá **Guardar**.
4. Opcional: en el menú de Edge, **Anclar a Inicio** para tenerlo como una app más.

## Conseguir una clave de API

1. Entrá a [Google Cloud Console](https://console.cloud.google.com/) y creá un proyecto.
2. Activá **YouTube Data API v3** en *APIs y servicios → Biblioteca*.
3. En *Credenciales* creá una **clave de API**.
4. Recomendado: restringila a **Sitios web (referentes HTTP)** con la dirección `https://julioxhernanz99-debug.github.io/*` y limitá la API a *YouTube Data API v3*. La clave queda visible en el navegador, así que la restricción evita que otros la usen.

La cuota gratuita es de 10.000 unidades por día. Una búsqueda cuesta 100 unidades; el resto de las consultas (tendencias, canales, comentarios) cuestan 1.

## Publicarlo (GitHub Pages)

1. Subí `index.html` a la raíz del repositorio.
2. En **Settings → Pages**, elegí *Deploy from a branch*, rama `main` y carpeta `/ (root)`.
3. Esperá un par de minutos y abrí la dirección que muestra GitHub.

Tiene que servirse por **https**. Abriendo el archivo directamente (`file://`) el reproductor de YouTube no funciona.

## Compatibilidad

El código está escrito en **ES5** (sin async/await, sin CSS grid ni variables CSS) para funcionar en EdgeHTML 15, el motor de Edge en Windows 10 Mobile 1703. Para actualizar la página en el teléfono, recargala o cerrá y reabrí la pestaña si sigue mostrando la versión anterior.

## Limitaciones

- La API de YouTube no entrega los streams de video: se usa el reproductor embebido, y algunos videos pueden tener la reproducción incrustada desactivada. En ese caso usá el enlace "Abrir en YouTube".
- **Calidad:** YouTube ignora a menudo los pedidos externos de calidad. Para forzarla, la página agranda el reproductor por dentro y lo reduce para que entre en pantalla, de modo que YouTube elija esa resolución. Que llegue a 1080p depende de la conexión y del dispositivo. El menú muestra la calidad real con la que se está reproduciendo.
- No hay reproducción en segundo plano con la pantalla apagada.
- No se puede iniciar sesión, comentar, dar "me gusta" ni suscribirse (requeriría OAuth).
- Los subtítulos solo aparecen si el video los tiene, ya sean propios o automáticos.

## Estructura

```
index.html   # toda la aplicación: HTML, CSS y JavaScript
README.md    # este archivo
```

## Aviso

Proyecto personal y no oficial. No está afiliado a YouTube ni a Google. El uso de la API está sujeto a los [Términos de servicio de los servicios de API de YouTube](https://developers.google.com/youtube/terms/api-services-terms-of-service).
