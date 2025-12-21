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
A continuacion un dibujo del esquema de red que crearemos como ejemplo practico:

![Imagen 1](https://github.com/kokobonger/nginx/blob/main/Esquema%20de%20red.png)

### Instalacion de NGINX

Para instalarlo debemos realizar el siguiente procedimiento paso a paso:

- **sudo apt update**
- **sudo apt install nginx -y**
Una vez realizado estos dos comandos comprobaremos el estado y la version del servicio ejecutando los siguientes comandos:

- **sduo systemctl status nginx**
- **nginx -v**
![Imagen2](https://github.com/kokobonger/nginx/blob/main/Estado%20de%20servicio.png)

Comprobamos que existe la pagina por defecto de nginx

- **ls /etc/nginx/sites-available**
  
![Imagen16](https://github.com/kokobonger/nginx/blob/main/comprobacion%20de%20instalacion%20correcta.png)
Ahora procederemos a lacreacion de los directorios de ejemplo:

- **sudo mkdir -p /var/www/web1**
- **sudo mkdir -p /var/www/web2**
- **sudo chown -R www-data:www-data /var/www**
Y comprobaremos que se han creado con exito

- **ls /var/www/**
![imagen3](https://github.com/kokobonger/nginx/blob/main/confirmacion%20directorios%20creados.png)

Una vez creados con exito los dos directorios procederemos a crear el index de las web de ejemplo:

- **sudo nano /var/www/web1/index.html**
  Dento ponemos lo que queramos que contenga nuestro index, en mi caso esto:
![Imagen4](https://github.com/kokobonger/nginx/blob/main/contenido%20web1.png)
- **sudo nano /var/www/web2/index.html**
  Dentro pondremos otra vez lo que queramos:
![Imagen5](https://github.com/kokobonger/nginx/blob/main/Contenido%20web2.png)

***Debemos guardar haciendo "CTRL + O" y salir haciendo "CTRL + X" en todos los ficheros que editaremos o crearemos***

Ahora comprobaremos que esta bien escrito poniendo el siguiente comando:
- **sudo nginx -t**
Si todo esta correcto nos devolvera un **"syntax is OK"**, si no debemos buscar que error hemos cometido.

### Creacion de virtualhost
Ahora debemos crear y editar dos ficheros para permitir o denegar el acceso a las paginas, para ello realizaremos lo siguiente:

- **sudo nano /etc/nginx/sites-available/web1**
  Dentro de este fichero debemos poner lo siguiente para permitir el acceso a todos los clientes.
  
  ![Imagen12](https://github.com/kokobonger/nginx/blob/main/Contenido%201%20de%20web1.png)
  
- **sudo nano /etc/nginx/sites-available/web2**
  Dentro de este fichero debemos poner lo siguiente para permitir el acceso solo a los clientes de la
  red interna y denegar el acceso desde la red externa.
  
  ![Imagen13](https://github.com/kokobonger/nginx/blob/main/Contenido%201%20de%20web2.png)

Una vez hecho todo esto, debemos activar los sitios asi:
- **sudo ln -s /etc/nginx/sites-available/web1 /etc/nginx/sites-enabled/**
  
- **sudo ln -s /etc/nginx/sites-available/web2 /etc/nginx/sites-enabled/**

  Y ahora realizamos una comprobacion de sintaxis:
  
- **sudo nginx -t**
  
Una vez confirmado su syntaxis correcta recargamos el servicio.

- **sudo systemctl reload nginx**
![Imagen6](https://github.com/kokobonger/nginx/blob/main/comprobacion%20y%20reinicio%20del%20servicio.png)

### Configuracion archivo HOSTS en clientes

Una vez realizado todo lo anterior, nos dirigimos a los clientes y dentro ponemos la direccion IP externa del equipo que aloja el servicio (nota: previamente deben hacer ping al servidor los equipos).

Para configurar el archivo, ejecutamos el comando:
- **sudo nano /etc/hosts**
  Dentro ponemos la ip y el nombre distinguido, en mi caso:

      192.168.0.51  www.web1.org
      192.168.0.51  www.web2.org

  Debes sustituir esa ip por la del equipo servidor que tu utilizas, luego debes **TABULAR (TAB)** y      poner el nombre distinguido que tendra tu pagina web, esto en ambos equipos cliente debe ser igual.

![Imagen7](https://github.com/kokobonger/nginx/blob/main/contenido%20fichero%20hosts%20en%20clientes.png)

### Comprobacion en clientes

Una vez tengamos todo lo anterior, nos vamos a el navegador que prefiramos y, en una pestaña privada, buscamos el nombre distinguido de nuestra pagina asi:

- **http://192.168.0.51/www.web1.org**
- **http://192.168.0.51/www.web2.org**
Sustituyendo la ip y el nombre por los que tengamos

Aqui adjunto la comprobacion realizada de las dos paginas desde los dos clientes:

### Cliente Interno

**Web1**

![Imagen8](https://github.com/kokobonger/nginx/blob/main/comprobacion%20configuracion%20correcta%20cliente%20interno.png)
**Web2**

![Imagen9](https://github.com/kokobonger/nginx/blob/main/comprobacion%20configuracion%20correcta%20cliente%20interno%202.png)

### Cliente Externo

**Web1**

![Imagen10](https://github.com/kokobonger/nginx/blob/main/comprobacion%20configuracion%20correcta%20cliente%20externo.png)

**Web2**

![Imagen11](https://github.com/kokobonger/nginx/blob/main/comprobacion%20configuracion%20correcta%20cliente%20externo%202.png)

Como podemos observar el cliente externo no tiene permitido acceder a la web2 ya que lo hemos configurado asi.

### Solicitud de autenticacion
Ahora configuraremos un directorio en la **Web1** que solicite una autenticacion al cliente para que solo los que conozcan el usuario y contraseña puedan acceder a ese directorio, para ello crearemos el directorio y un usuario el cual tendra acceso al directorio dentro de la web1 y con el intentaremos acceder, hay que tener en cuenta que solo le pedira autenticarse a los clientes que se conecten desde el exterior de la red, a los clientes dentro de la red interna les dejara acceder sin autenticacion necesaria.
Aqui muestro los comandos necesarios en orden:

Creamos el sitio:
- **sudo mkdir /var/www/web1/privado**
Instalamos las utilidades de apache2:
- **sudo apt install apache2-utils -y**

Creacion de usuario: Para crear un usuario directamente para que tenga acceso debemos ejecutar este comando:
- **sudo htpasswd -c /etc/nginx/.htpasswd fernando**
(Debemos poner una contraseña para el usuario)

![Imagen14](https://github.com/kokobonger/nginx/blob/main/creacion%20usuario%20autenticacion.png)

### Configurar Nginx
Ahora debemos añadir lineas a la configuracion del virtualhost de la **web1**: 

- **sudo nano /etc/nginx/sites-available/web1**

Debe quedarnos asi:
![Imagen15](https://github.com/kokobonger/nginx/blob/main/contenido%20fichero%20configuracion%20de%20web1.png)

Guardamos, cerramos y reiniciamos el servicio

- **sudo systemctl reload nginx**

Ahora, realizamos las comprobaciones:

### Cliente externo

Podemos observar que me pide credenciales
![Imagen17](https://github.com/kokobonger/nginx/blob/main/comprobacion%20de%20requisito%20de%20autenticacion.png)

Una vez puestas me deja acceder 

![Imagen18](https://github.com/kokobonger/nginx/blob/main/comprobacion%20configuracion%20correcta%20cliente%20externo%203.png)

### Cliente interno

Nos la da directamente sin necesidad de autenticarse
![Imagen19](https://github.com/kokobonger/nginx/blob/main/cliente%20interno%20zona%20privada.png)



# FIN
