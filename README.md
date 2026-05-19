# docker-lab
En este repositorio guardaré la información del Laboratorio sobre Docker, del curso DEVOPS<br/>
Voy a documentar:<br/>
+ Los pasos que has realizado<br/>
+ Los comandos utilizados<br/>
+ Una breve explicación de cada paso<br/>
+ Las respuestas a las preguntas del laboratorio<br/>
# 1.- Creando imágenes
## Paso 1
Lo primero, voy a clonar mi repositorio en el local, por si hay que utilizarlo posteriormente<br/>
<img width="817" height="425" alt="image" src="https://github.com/user-attachments/assets/4f3d4597-540f-4bdf-85cd-bbcb2b257077" /><br/>
Ejecutamos el contenedor basado en **ubuntu** con el comando ```docker run -it --name mi-ubuntu ubuntu```<br/>
<img width="927" height="466" alt="image" src="https://github.com/user-attachments/assets/2701aa72-ade8-4acf-8e03-95c9293ed86d" /><br/>
Instalamos curl (actualizando antes los repositorios de ubuntu) y comprobamos que funciona, viendo su versión <br />
<img width="927" height="466" alt="image" src="https://github.com/user-attachments/assets/cbdf0c07-5b07-4200-9f34-f3c2e71d47e1" />
<img width="1002" height="269" alt="image" src="https://github.com/user-attachments/assets/c2dce0b8-e367-4319-9b90-f9d4ca21afa4" />
### Pregunta: ¿Con qué comando podríamos guardar los comabios del contenedor como una nueva imagen?
Con ```docker commit``` podría guardar los cambios en una nueva imagen, por ejemplo ```docker commit mi-ubuntu mi-ubuntu-personalizado``` me generaría una nueva imagen, con la instalación de curl, que se llamaría mi-ubuntu-personalizado, como podemos comprobar en la siguiente imagen:<br/>
<img width="1272" height="458" alt="image" src="https://github.com/user-attachments/assets/6b9a5784-6662-45b3-b3dc-dbc2f4804531" />
Y si entramos en dicha imagen, podemos comprobar que está instalado ```curl```
<img width="1261" height="441" alt="image" src="https://github.com/user-attachments/assets/4a334c7e-4165-4929-91f0-31cb6bb18bf1" />
## Paso 2 -- Dockerfile
Creamos un Dockerfile, que contiene el siguiente código:<br />
```
FROM ubuntu
RUN apt-get update && apt-get install curl -y
CMD ["curl", "--version"]
```
<img width="1261" height="441" alt="image" src="https://github.com/user-attachments/assets/456c9090-2175-45a8-80ea-bf50a320a07c" />

Y con el comando: <br/>
```docker build -t mi-ubuntu-curl-dockerfile .```
<br/>
creamos nuestra imagen de ubuntu, con curl instalado, mediante dockerfile<br/>
<img width="1360" height="455" alt="image" src="https://github.com/user-attachments/assets/dde84461-a606-449a-87f4-fd03b7ab595d" />
Y comprobamos que efectivamente tenemos la imagen en nuestro equipo <br />
<img width="1335" height="455" alt="image" src="https://github.com/user-attachments/assets/9d4177c6-aefe-4142-8291-ad8c8e6a9a43" />
### Pregunta: ¿Qué comando permite ver las capas de una imagen Docker?
Con el comando: <br/>
```docker history la_imagen ```, en mi caso:<br />
<img width="1335" height="325" alt="image" src="https://github.com/user-attachments/assets/416af343-59b4-402c-b32e-553e880a5fb8" />
# 2.- Limpiando imágenes --opcional
Lo primero que voy a hacer es un nuevo Dockerfile que coja únicamente una imagen de ubuntu. Para no "machacar" mi Dockerfile (el generado en el punto anterior), le voy a poner una extensión (en mi caso .op1). Después iré creando las nuevas imágenes con este Dockerfile (tendré que especificarle al build el nombre exacto de este dockerfile)<br/>
<img width="1335" height="325" alt="image" src="https://github.com/user-attachments/assets/b33b0bc6-142b-4075-aebf-f6a86e2f159b" />
Construyo la imagen con este dockerfile:<br/>
```docker build -f Dockerfile.op1 -t mi-ubuntu-opcional-1 .```<br/>
<img width="1346" height="754" alt="image" src="https://github.com/user-attachments/assets/e4086463-3a31-455d-92ed-33fa746d75d2" />
A continuación, modifico el Dockerfile.op1 y le añado que instale curl
<img width="1306" height="172" alt="image" src="https://github.com/user-attachments/assets/504b59ce-b5d4-4420-9ef4-9bf61d5894fb" />
Y genero la nueva imagen y listo las imágenes:
<img width="1472" height="791" alt="image" src="https://github.com/user-attachments/assets/59bb8e14-2aed-4033-9d52-4e47e121af33" />
Puedo observar que **aparece la última imagen creada**. Por lo visto, en versiones anteriores, aparecían las anteriores, que quedaban como "huérfanas". Pero en mi caso, sólo veo la última generada (al menos con mi versión de Docker)<br/><br/>
Vuelvo a modificar el **Dockerfile.op1**, y a generar la nueva imagen, comprobando cómo quedan las imágenes de mi sistema:<br/>
<img width="1562" height="941" alt="image" src="https://github.com/user-attachments/assets/922fbb49-3ad3-49ed-8694-83ce6025dda2" />
# 3.- Volúmenes persistentes
Primero, creamos nuestra imagen, con un volumen en mi sistema que "apunta" a la carpeta en la que postgresql guarda los datos <br/>
<img width="1314" height="557" alt="image" src="https://github.com/user-attachments/assets/621bf5b7-f6df-474b-bc80-61b25ea8e0f7" />
A continuación, creamos tabla y datos dentro de la BD. Lo primero será entrar en nuestro docker y abrir el postgres:<br/>
<img width="1322" height="210" alt="image" src="https://github.com/user-attachments/assets/40c51ff9-76be-4f3c-9031-1cf4f05c6db9" />
Y una vez aquí, cremos la tabla y datos: <br/>
<img width="1349" height="284" alt="image" src="https://github.com/user-attachments/assets/e3230144-19a6-4114-9929-b1b2e82c8bca" />
### Comprobación de la persistencia
- Paramos el contenedor y Eliminamos el contenedor<br/>
 <img width="1349" height="76" alt="image" src="https://github.com/user-attachments/assets/76b6d1a0-0420-4ea9-8d86-279cc0fb2ff6" /><br/>
- Creamos un nuevo contenedor usando el mismo volumen<br/>
- <img width="1349" height="151" alt="image" src="https://github.com/user-attachments/assets/b8f2d0fe-5fb9-4475-b090-7718d19294cc" /><br/>

- Comprobamos que los datos siguen existiendo<br/>
<img width="1349" height="259" alt="image" src="https://github.com/user-attachments/assets/98b03357-0136-4dfe-bf25-f22c6ed6138a" /><br>

**Efectivamente** comprobamos que los datos persisten en esta nueva imagen, ya que hemos vinculado nuestro volumen en el PC, con la carpeta data del postgresql <br/>

# 4.- Bind mounts
Creamos el fichero index.html, en Visual Code <br/>
<img width="1349" height="259" alt="image" src="https://github.com/user-attachments/assets/03d1b4e5-7143-4719-9b7c-2de607b8b647" />
Ejecutamos el contenedor de nginx, mapeando el puerto y montando el fichero **index.html de mi PC** en el index.html del contenedor (en **/usr/share/nginx/html/index.html**)<br/>
<img width="1349" height="259" alt="image" src="https://github.com/user-attachments/assets/2e4d6dd6-b9c5-42a6-89ba-46495a57696e" />
Abrimos nuestro navegador, y comprobamos que nginx carga "nuestro" index.html (el que está en mi PC).<br/>
Vamos a hacer una modificación en "nuestro" index.html (el local)  y vamos a comprobar si el cambio se ve directamente en el navegador<br/>
<img width="1839" height="324" alt="image" src="https://github.com/user-attachments/assets/e643727d-77e3-494b-b1de-66e000b3335f" />
<img width="1839" height="324" alt="image" src="https://github.com/user-attachments/assets/ab6911b9-806f-4e4d-95bd-f11f7b2ab531" />
Efectivamente, el cambio en el fichero local index.html (tras actualizar la página con F5) se ve directamente en el navegador<br/>

# 5.- Auditando volúmenes -- opcional
Con ```docker volume inspect <nombre_volumen>``` <br/>
En mi caso sería: <br/>
```docker volume inspect mi-data-vol```
<br/>
<img width="1349" height="259" alt="image" src="https://github.com/user-attachments/assets/ad8bda76-ffb7-47bd-9b81-00fbec3c84e1" />

# 6.- Creando redes privadas
Lo primero, es crear una red "docker",a través del cual, los contenedores que indiquemos que pueden usar dicha red, **se puedan comunicar entre sí**:<br/>
<img width="1347" height="128" alt="image" src="https://github.com/user-attachments/assets/28a0cc52-139a-4907-8bc7-4b8ffae2ebdc" />
Una vez creada la red, vamos a arrancar 2 contenedores, indicándoles que deben usar dicha red: <br/>
<img width="1347" height="133" alt="image" src="https://github.com/user-attachments/assets/f5e4e4fb-8f87-4d81-b408-83071495940b" />
Y ahora probaremos si podemos hacer ping (es posible que no esté instalado. Si fuese así, lo instalaríamos en ambos contenedores):<br/>
**ERROR**
<img width="1347" height="133" alt="image" src="https://github.com/user-attachments/assets/64a3068f-5e21-41e0-b015-bbd03c75b2dd" />
En este punto, me da un error, debido a que no puedo entrar porque al no tener ningún servicio el contenedor, ha finalizado su ejecución. Tendré que arrancarlos de nuevo, con la opción *sleep infinity*: <b
<img width="1347" height="170" alt="image" src="https://github.com/user-attachments/assets/e010851d-1bb9-431c-9808-15345613f02b" />
Y volvemos a intentar hacer ping y compruebo que no funciona, por lo que lo instalo:<br/>
```
apt-get update && apt-get install iputils-ping -y
ping ubuntu2
```
<br/>
<img width="1347" height="170" alt="image" src="https://github.com/user-attachments/assets/b5257354-dbbf-40cd-b4ee-18a722684feb" />
Finalmente, pueden hacer ping. Lo interesante, es que pueden hacer ping por nombre, lo que impolica que docker usa un DNS interno para que se reconozcan los 2 contenedores, a través del nombre
# 7. Red none --opcional
Sirve para "aislar" completamente nuestro docker de cualquier red. Se usa, sobre todo, para procesos en los que se necesita mucha seguridad <br/>
# 8. Multi-network --opcional
Creamos las 2 redes <br/>

<img width="1347" height="170" alt="image" src="https://github.com/user-attachments/assets/38b449f2-ed6f-4cab-8b16-de2685879340" />
Arrancamos un contenedor en public-zone<br/>
<img width="1347" height="170" alt="image" src="https://github.com/user-attachments/assets/eaf2a423-2dda-420e-be36-9310fdd7665e" />
### Pregunta - Puede conectarlo también a la secure-zone?
Sí, sería como añadir una segunda tarjeta de red, que estaría conectada a otra red. Para ello usaríamos: <br/>
<img width="1347" height="170" alt="image" src="https://github.com/user-attachments/assets/ffa50de2-9345-4872-9248-5d6efd53781a" />
Y para comprobar que están las 2 redes, tendríamos que "auditar" mi contenedor, con la opción inspect <br/>

```
docker inspect mi-nginx
```

<br/>
que nos devuelve un fichero en .JSON, en el que podemos ver muchas configuraciones de mi contenedor, entre otras, "Networks", y como vemos en la imagen, se observan las 2 "redes", cada una haría referencia a una "tarjeta de red", que podemos ver que estarían en 2 subredes distintas (una en la 172.19 y la otra en 172.20) <br/>
<img width="1347" height="759" alt="image" src="https://github.com/user-attachments/assets/b3677ee6-d746-4de6-afd0-7a5f333c9ae1" />

# 9.- Docker Compose --- Compartiendo volúmenes
Para este ejercicio, vamos a crear 2 servicios. Uno escribe en un fichero cada 30 segundos (la fecha en la que se ejecuta el comando date) y el otro lee (lo ponemos de solo lectura con **ro**). Los 2 "apuntan" a la misma carpeta (/app/logs), por lo que la comparten. Dentro de esa carpeta está el fichero proceso.log, que será donde escriba uno (con echo $(date)) y otro lea las últimas líneas (con tail). Al poner en el reades el "depens_on" de writer, le indicamos que no empiece hasta que lo haga writer (así no intentamos leer de un fichero que no existe).
El YAML correspondiente sería: <br/>

<img width="1838" height="559" alt="image" src="https://github.com/user-attachments/assets/c647e7aa-fee5-4719-8bda-e33d9e5a24dd" />

Levantamos los 2 contenedores y comprobamos que cada 30 segundos, aparece un mensaje (lo escribe el writer y lo lee el reader) <br/>
<img width="1352" height="482" alt="image" src="https://github.com/user-attachments/assets/5652a466-8ef0-4573-9a37-069f59bb96ef" />












