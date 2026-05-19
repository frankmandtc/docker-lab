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
- <img width="1349" height="76" alt="image" src="https://github.com/user-attachments/assets/76b6d1a0-0420-4ea9-8d86-279cc0fb2ff6" />

- Creamos un nuevo contenedor usando el mismo volumen<br/>
- <img width="1349" height="151" alt="image" src="https://github.com/user-attachments/assets/b8f2d0fe-5fb9-4475-b090-7718d19294cc" />

- Comprobamos que los datos siguen existiendo<br/>
<img width="1349" height="259" alt="image" src="https://github.com/user-attachments/assets/98b03357-0136-4dfe-bf25-f22c6ed6138a" />
**Efectivamente!!!** comprobamos que los datos persisten en esta nueva imagen, ya que hemos vinculado nuestro volumen en el PC, con la carpeta data del postgresql<br/>
# 4.- Bind mounts
# 5.- Auditando volúmenes -- opcional
Con ```docker volume inspect <nombre_volumen>``` <br/>
En mi caso sería: <br/>
```docker volume inspect mi-data-vol```
<br/>
<img width="1349" height="259" alt="image" src="https://github.com/user-attachments/assets/ad8bda76-ffb7-47bd-9b81-00fbec3c84e1" />















