# **Herramienta de escaneo de vulnerabilidades.**

# **Nessus:**

Nessus es un programa para realizar escaneos sobre los dispositivos de las redes en busca de vulnerabilidades

## **-Instalación**

Instalamos el paquete wget con el comando:

`apt update && sudo apt install wget -y`

![c1](/img/alumno1/sad_p1_c9.PNG)

Descargamos el archivo .deb de Nessus
```
wget https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/16870/download?i\_agree\_to\_tenable\_license\_agreement=true -O Nessus-10.3.0-debian9\_amd64.deb
```
![c2](/img/alumno1/sad_p1_c10.PNG)


Instalamos el paquete .deb con apt:

`apt install -f ./Nessus-10.3.0-debian9\_amd64.deb`

![c3](/img/alumno1/sad_p1_c11.PNG)

Activamos y habilitamos el servicio nessusd con los comandos:

`systemctl start nessusd`

`systemctl enable nessusd.service`

![c4](/img/alumno1/sad_p1_c12.PNG)

Comprobamos en que puerto se está ejecutando nessus con el comando:

`ss -plunt|grep 8834`

![c5](/img/alumno1/sad_p1_c13.PNG)

---

## **-Conf. Inicial**

Se puede acceder al programa escribiendo en la barra de url del navegador

https://IP-Address:8834

Lo primero que hay que hacer al entrar en la interfaz web es seleccionar un versión del programa. En mi caso selecciono Nessus essentials.

![c6](/img/alumno1/sad_p1_c14.PNG)

Ahora tenemos que poner nuestro nombre, apellido y correo para que nos genere un código de activación.y poder usar el programa.

![c7](/img/alumno1/sad_p1_c15.PNG)

Nos enviarán el código a nuestro correo. Ponemos el código en ActivationCode y pulsamos en continue.

![c8](/img/alumno1/sad_p1_c16.PNG)

Ahora tenemos que crear el usuario y contraseña para loguearnos.

![c9](/img/alumno1/sad_p1_c17.PNG)

Cuando creemos el usuario y la contraseña comenzaran a descargarse los plugins de nessus. Este es el último paso para comenzar a utilizar nessus.

![c10](/img/alumno1/sad_p1_c18.PNG)

---

## **-Prueba de uso**

Para realizar un escaneo lo primero que hay que hacer es pulsar en create a new scan o en el botón New Scan.

![c11](/img/alumno1/sad_p1_c1.PNG)

Ahora en la página de Scan Templates podemos ver que hay múltiples opciones de escaneos. Voy a seleccionar Basic Network Scan que es un escaneo sencillo de los host que especifiquemos de la red para buscar vulnerabilidades, puertos abiertos, sistema operativo,servicios, etc.

![c12](/img/alumno1/sad_p1_c2.PNG)

En esta página podemos configurar el escaneo.

El poner un nombre al escaneo nos puede ayudar en organización, si pulsamos en el botón Save podemos guardar estas opciones para ejecutarla más tarde. En caso de no querer guardarla se despliega el botón de la derecha y pulsa sobre launch.

-Folder: es la carpeta donde se guarda el escaneo

-Targets: son los objetivos. En mi caso he seleccionado una máquina virtual vulnerable para el escaneo.

\- Nombre: Metasploitable

\- Folder: My Scans

\- Targets: 192.168.50.30

![c13](/img/alumno1/sad_p1_c3.PNG)

Como podemos ver se a generado la plantilla. Ahora pulsamos sobre el botón de la derecha para iniciar el escaneo

![c14](/img/alumno1/sad_p1_c4.PNG)

Se acaba de iniciar el escaneo y se puede observar que indica todos los host objetivos, las veces anteriores que se ha ejecutado este escaneo, número de posibles vulnerabilidades, número de posibles soluciones a las vulnerabilidades, etc.

![c15](/img/alumno1/sad_p1_c5.PNG)

Ya terminado el escaneo si queremos ver mas detalles pulsamos sobre el \*host\* o el botón \*vulnerabilities\*.

![c16](/img/alumno1/sad_p1_c6.PNG)

Podemos ver que hay información sobre varias vulnerabilidades que tiene la máquina entre esta información destaca:

\- Sev: Nivel de riesgo

\- Name: El nombre de la vulnerabilidad

\- Family: La familia a la que pertenece. Esto nos puede ayudar mucho para saber de donde procede

\- Count: El número de veces que está presente

![c17](/img/alumno1/sad_p1_c7.PNG)


Si pulsamos sobre una de las vulnerabilidades podemos ver información más detallada de ella. El puerto, protocolos que está usando la vulnerabilidad, Posibles soluciones, formas de explotarla, etc.

![c18](/img/alumno1/sad_p1_c8.PNG)

---

## **-Uso en Red**

Ahora vamos a probar a escanear toda una red, para variar un poco en vez de volver a hacer el Basic Network Scan, haremos el Advanced Scan.

Ponemos un nombre, descripción (opcional) y la parte importante es poner la dirección de red junto a la máscara de red.

Cuando terminemos de configurar el escaneo lo ejecutamos.

![c19](/img/alumno1/sad_p1_c19.PNG)


Esta parte del proceso es igual al escaneo anterior pero con la diferencia de que ahora en vez de escanear solo un host ahora son todos los host de la red.

![c20](/img/alumno1/sad_p1_c20.PNG)

---

## **-Monitorización continua**

Ahora vamos a configurar un escaneo para que se ejecute de manera automática.

Podemos crear o editar una plantilla existente. En mi caso editaré el escaneo anterior.

Estando dentro del escaneo pulsamos en el botón configure

![c21](/img/alumno1/sad_p1_c25.PNG)

Nos aparecerá esta ventana. Nos dirigimos a schedule y activamos el único botón que hay.

En mi caso lo configuraré para que se ejecute todas las semanas de lunes a viernes a las 23:10.

![c22](/img/alumno1/sad_p1_c26.PNG)

Se pueden configurar por: día,semana,mes,año.

![c23](/img/alumno1/sad_p1_c27.PNG)

Podemos configurar la cantidad de días,semanas,meses… que se va a ejecutar.

![c24](/img/alumno1/sad_p1_c28.PNG)

Ya está configurado el escaneo. Lo puse para las 23:10 ya son las 23:11 y como podemos ver se está ejecutando como cualquier otro escaneo.

![c25](/img/alumno1/sad_p1_c29.PNG)

![c26](/img/alumno1/sad_p1_c30.PNG)

---

## **-Alertas Correo**

### **Instalación postfix**

Para recibir alertas por correo primero tenemos que configurar el servidor de correo postfix:

Primero comprobamos el hostname de la máquina con el comando hostname; Tras eso instalamos lo siguiente:

sudo apt install mailutils

sudo apt install postfix

En el menú que aparece seleccionamos **internet site**, y en el siguiente diálogo introducimos (si no está ya) el hostname que consultamos antes.

### **Configuración postfix**

Para configurar el servidor editamos el fichero /etc/postfix/main.cf, tenemos que modificar las siguientes líneas:

inet\_interfaces = loopback-only

mydestination = $myhostname, localhost.$your\_domain, $your\_domain

Tras esto reiniciamos el servicio:

sudo systemctl restart postfix

podemos enviar un correo de pruebas:

echo "Cuerpo del correo" | mail -s "Asunto del correo" email\_address

---

## **-Análisis Informes**

Para la creación de un informe tenemos que pulsar al botón report que aparece cuando estamos viendo los resultados de un escaneo. Cuando pulsemos el botón podremos ver que lo podemos generar en html y csv, también se puede elegir la plantilla y la información a mostrar. En mi caso lo generare en html porque para mi se lee mejor.


![c27](/img/alumno1/sad_p1_c21.PNG)


Cuando pulsemos sobre el fichero html podremos ver que se muestra en el navegador la información del escaneo.

![c28](/img/alumno1/sad_p1_c22.PNG)

![c29](/img/alumno1/sad_p1_c23.PNG)

---

## **-Demo Parcheo**

Cuando revisamos los resultados de los escaneos podemos ver que hay una pestaña que dice remediations. Si pulsamos en ella podremos ver algunas vulnerabilidades y lo que tenemos que hacer para corregirlas.

![c30](/img/alumno1/sad_p1_c31.PNG)

Según la información si se actualiza bind9 deberían solucionarse 3 vulnerabilidades relacionadas con ese paquete.

![c31](/img/alumno1/sad_p1_c32.PNG)

Cuando terminó de actualizarse realice otro escaneo para ver los resultados.

![c32](/img/alumno1/sad_p1_c33.PNG)

Como podemos ver la vulnerabilidad relacionada con bind9 ya no está. Ha sido corregida por la actualización del paquete.

![c33](/img/alumno1/sad_p1_c34.PNG)
