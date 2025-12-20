# Tarea 4 HTTP
## Fernando Nunez Liger

### Introduccion
En este fichero readme veremos como instalar y configurar una pagina web con nginx en linux

### Descripcion de NGINX
NGINX es un servidor web de código abierto muy rápido y eficiente, puede ser utilizado para un monton de servicios entre los que destacan:
- Proxy inverso
- Balanceador de carga
- Servidor de correo
- Cache HTTP
Entre otros, nosotros lo utilizaremos en sustitucion del servicio **Apache2**

### Comparacion APACHE2 VS NGINX
Apache es un servidor web veterano, muy flexible y con una enorme cantidad de módulos. Su arquitectura basada en procesos o hilos lo hace fácil de extender y muy compatible con aplicaciones tradicionales, pero también implica que consume más recursos cuando hay muchas conexiones simultáneas. Por eso suele funcionar muy bien en entornos donde se necesita personalización detallada, como configuraciones por directorio con .htaccess, o en hosting compartido donde cada sitio requiere ajustes propios.

NGINX, en cambio, utiliza una arquitectura basada en eventos que le permite manejar miles de conexiones con muy poca memoria. Esto lo convierte en una opción excelente para sitios de alto tráfico, contenido estático y como proxy inverso o balanceador de carga. No ofrece la misma flexibilidad granular que Apache, pero a cambio es más rápido, más eficiente y más estable bajo carga pesada.

En conclusion,  Apache destaca por su flexibilidad y compatibilidad, mientras que NGINX sobresale por su rendimiento y eficiencia.

### Esquema de Red
A continuacion un dibujo del esquema de red que crearemos como ejemplo practico

![Imagen 1](https://github.com/kokobonger/nginx/blob/main/Esquema%20de%20red.png)
