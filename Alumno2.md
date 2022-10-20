# **Sistema de detección de intrusos**

# **Snort:**

## **Instalación**

El paquete de Snort se encuentra en los repositorios de debian, por lo que podemos instalarlo directamente con apt

`sudo apt update`

`sudo apt install snort`

con `snort -V` podemos comprobar que se ha instalado correctamente:

![c1](img/alumno2/snort_c1)

---

## **Configuración de la red**

Tras instalarlo, iniciará la configuración de la red. En la configuración que se abre, podemos seleccionar la ip/rango de ips de la máquina, aunque más adelante modificando el fichero de configuración, podemos elegir que utilice solo una interfaz de red o todas (la opción por defecto).

![c2](img/alumno2/snort_c2)

Para modificar las opciones tras la instalación tenemos que modificar el fichero /etc/snort/snort.debian.conf (al tratarse de una instalación en debian, se carga el contenido de este fichero antes que la propia configuración de snort). Concretamente nos interesan las siguientes líneas:

DEBIAN\_SNORT\_HOME\_NET="10.0.0.0/8"

DEBIAN\_SNORT\_INTERFACE="enp7s0"

Como el propio fichero indica, después de modificarlo tenemos que ejecutar el siguiente comando para que la configuración se actualice:

`sudo dpkg-reconfigure snort`

Tras la ejecución del comando, reconfiguramos Snort, decidiendo cuando se ejecuta, las interfaces e ips, si activamos el modo promiscuo, y por último si queremos que se cree una tarea de cron para mandar correos diariamente con el resultado del log.

---

## **Reglas**:

Para configurar los grupos de reglas que queremos activar, tenemos que editar el fichero de configuración /etc/snort/snort.conf:

![c3](img/alumno2/snort_c3)

En la instalación que se realiza de los repositorios de debian, están incluidas las reglas de la comunidad. En el caso de que queramos utilizar una set de reglas concreta, habría que descomentar el set específico. También se puede observar en la imagen como el fichero de reglas personalizadas está activo (local.rules).

---

## **Reglas propias**

De momento el fichero de reglas propias está vacío. Las reglas se construyen de la siguiente manera:

![c4](img/alumno2/snort_c4)

Como podemos ver, La regla consta de dos partes principales, la **cabecera**, que contiene información relacionada con la red, y las **opciones**, que contienen detalles de investigación de paquetes. La regla que se muestra a continuación, sirve para detectar que se está realizando un ping a la máquina:

`alert icmp any any -> $HOME\_NET any (msg:"Ping detectado";sid:1000001;rev:1)`

vamos a analizar la regla:



|**Estructura**|**Valor**|**Descripción**|
| :-: | :-: | :-: |
|Action|alert|le dice a snort que hacer cuando la regla salta|
|Protocol|icmp|Protocolo a ser analizado (TCP, UDP, ICMP, IP)|
|Source IP|any|Direcciones IP de origen|
|Source Port|any|Puertos de origen|
|Direction|->|Operador de dirección. Determina la dirección del tráfico|
|Destination IP|$HOME\_NET|Direcciones IP de destino|
|Destination Port|any|Puertos de destino|
|Message|msg:“Ping detectado”|Mensaje a mostrar cuando aplique la regla|
|Rule ID|sid:1000001|ID único de la regla|
|Revision info|rev:1|Información de revisión|

---

**Demostración de la regla**

Una vez hemos añadido al regla al fichero de local.rules, ya podemos probar la regla. Vamos a ejecutar snort en modo consola para que muestre el resultado en pantalla (-A console):

`sudo snort -A console -q -c /etc/snort/snort.conf -i enp1s0`

![c5](img/alumno2/snort_c5)

En el caso de esta regla, hemos utilizado la Action alert, que genera una alerta cuando los requisitos se cumplen. Sin embargo, hay más opciones, que son:

alert - generate an alert using the selected alert method, and then log the packet

1. log - registra los paquetes
2. pass - ignora los paquetes
3. drop - bloquea y registra los paquetes
4. **reject** - bloquea, registra el paquete, y luego envia un reset TCP si el protocolo es TCP o un mensaje de puerto ICMP inalcanzable si el protocolo es UDP.
5. sdrop - bloquea el paquete sin dejar registro.
