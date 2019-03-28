# NodeJS con Proxy Reverso

> Se necesitará una tarjeta de crédito

## Servidor en DigitalOcean

1. Obtener una cuenta en digitalocean.com. Obtener la cuenta es gratis. Crear un servidor no.

2. Crear un droplet (así llama Digital Ocean -DO- a los servidores)

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/01.png)

3. Elegir una opción preinstalada de NodeJS

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/02.png)

4. Elegir NodeJS

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/03.png)

5. Escoger el plan menos caro para iniciar.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/04.png)

6. Seleccionar cualquier región del datacenter

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/05.png)

7. Ponganle un nombre al servidor.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/06.png)

8. Ahora hay que esperar uno o dos minutos para DO cree el servidor. Una vez creado aparecerá en el listado de servidores con un ip asignado automáticamente.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/07.png)

9. DO debe haberles enviado un correo con los datos de acceso al servidor.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/08.png)

10. El acceso es por SSH. Si están en Windows, descarguen el programa "putty.exe" desde putty.org

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/09.png)

11. Descarguen la versión de 64 bits.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/24.png)

12. Ejecuten el putty.exe y pongan el ip que les envió DO. Ponganle un nombre a la sesión y guardenla.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/11.png)

13. Luego denle clic para conectarse al servidor.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/12.png)

14. El servidor les dice que se pueden conectar usando una llave (key). Por ahora denle clic a "Sí".

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/13.png)

15. Ingresen el usuario y la contraseña que les dio DO.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/14.png)

16. Ahora les pide que ingresen nuevamente la misma contraseña (LA MISMA CONTRASEÑA DEL CORREO DE DO)

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/15.png)

17. Ahora les pide que cambien la contraseña que les dio DO por una generada por ustedes.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/16.png)

## Proxy y servidores de NodeJS

1. Crear una carpeta y situarse en ésta. 

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/17.png)

2. Crear un proyecto con "npm init -y"

3. Instalar el proxy "redbird" ejecutando "npm i --save redbird"

> Crearé dos servidores de NodeJS
> Uno usará el subdominio test-node.tibajodemanda.com.
> El otro usará el subdominio node-test.tibajodemanda.com.
> Ustedes deben usar sus propios subdominios o dominios.

4. Crear el archivo proxy.js usando "nano proxy.js". Si no está instalado el programa "nano", ejecutar "apt-get install nano".

5. Copiar lo siguiente al archivo "proxy.js" (usen el botón derecho del mouse para pegar).

```javascript
const proxy = require('redbird')({port: 80})
proxy.register("test-node.tibajodemanda.com", "http://127.0.0.1:8001");
proxy.register("node-test.tibajodemanda.com", "http://127.0.0.1:8002");
```

6. ctrl+x para guardar el archivo y cerrar.

7. Crear el primer servidor llamado "server01.js" en el puerto 8001 usando "nano server01.js". Pegar el siguiente código.

```javascript
const http = require("http")

const servidor = http.createServer((req, res) => {
	res.writeHead(200, {"content-type": "text/plain; charset=utf-8"})
	res.end("Servidor en test-node.tibajodemanda.com, puerto 8001")
})

servidor.listen(8001, ()=> console.log("Servidor ejecutándose en el puerto 8001"))
```

8. ctrl+x para guardar el archivo y cerrar.

9. Crear el segundo servidor llamado "server02.js" en el puerto 8002 usando "nano server02.js". Pegar el siguiente código.

```javascript
const http = require("http")

const servidor = http.createServer((req, res) => {
	res.writeHead(200, {"content-type": "text/plain; charset=utf-8"})
	res.end("Servidor en node-test.tibajodemanda.com, puerto 8002")
})

servidor.listen(8002, ()=> console.log("Servidor ejecutándose en el puerto 8002"))
```

10. ctrl+x para guardar el archivo y cerrar.

11. Instalar pm2 con el siguiente comando: "npm i pm2 -g"

12. Ejecutar los siguientes comandos suponiendo que todos los archivos (proxy.js, server01.js y server02.js) están en el mismo directorio. Si no lo están, ejecutar el comando desde el directorio de cada uno.
	- pm2 start proxy.js -n proxy
	- pm2 start server01.js -n server01
	- pm2 start server02.js -n server02

13. Para que todo se ejecute cuando se reinicie el servidor de DO, se debe ejecutar también: 
* pm2 save
* pm2 startup

14. Ejecuten el siguiente comando para ver que todo esté correcto: "pm2 list"

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/19.png)

15. Comprobando que funciona.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/20.png)

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/21.png)

## Configuración de subdominios

Trabajo mis dominios con GoDaddy.com. Ustedes pueden tener sus dominios con GoDaddy o donde les parezca mejor.

Cualquiera de los proveedores de dominios les permite administrar los DNS del dominio.

En mi caso agregué los siguientes registros para que apunten al ip que me dio DO.

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/22.png)

![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/23.png)

## Instalación de LetsEncrypt

apt-get install certbot

## Instalación de certificados

Antes que nada deben detener el proxy:
pm2 stop proxy

certbot certonly --preferred-challenges http-01 -d test-node.tibajodemanda.com

certbot certonly --preferred-challenges http-01 -d node-test.tibajodemanda.com

En ambas instalaciones

Elegir la opción 1 (standalone)
![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/25.PNG)

Ingresen un correo para recibir notificaciones
![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/26.PNG)

Acepten los términos de LetsEncrypt y que les envíen correos (opcional)
![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/27.PNG)

Con esto ya instalaron los certificados
![](https://raw.githubusercontent.com/sergiohidalgocaceres/servidor-node-proxy/master/assets/img/28.PNG)

## Modificación de proxy.js para que acepte SSL y HTTP2

```javascript
const proxy = require('redbird')({
        port: 80,
        letsencrypt: {
                path: "/etc/letsencrypt/live",
                port: 9999
        },
        ssl: {
                http2: true,
                port: 443
        }
})
proxy.register("test-node.tibajodemanda.com", "http://127.0.0.1:8001",{
  ssl: {
    letsencrypt: {
      email: 'sergiohidalgocaceres@gmail.com',
      production: true
    }
  }
});
proxy.register("node-test.tibajodemanda.com", "http://127.0.0.1:8002",{
  ssl: {
    letsencrypt: {
      email: 'sergiohidalgocaceres@gmail.com',
      production: true
    }
  }
});
```

Finalmente inicien el proxy: 
pm2 start proxy