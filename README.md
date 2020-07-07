# Transformation-Advisor-CP4A
En este Hands On se va a realizar una muestra dentro de un ambiente de pruebas donde se utilizará las tecnológias de IBM como lo es Data Collector Utility y Transformation  Advisor para evaluar si una aplicación java cumple con los requisitos para pasar a un ambiente Cloud.

## Requisitos
1. Tener una cuenta de IBM Cloud, de no tenerla crearla en el siguiente link:

2.

## Ambiente

[https://bluedemos.com/show/2459](https://bluedemos.com/show/2459)

## Habilitar ambiente

En este paso se dejara listo el ambiente para trabajar sobre las maquinas virtuales brindadas por Skytap al momento de solicitar el ambiente de pruebas en bluedemos.

 1. Ingresar al link enviado a su correo del acceso a las maquinas virtuales con skytap.
2. Encender las seis maquinas virtuales que se le fueron asignadas, dando click en el botón de play.
3. Cuando las maquinas esten encendidas dar click sobre la maquina virtual llamada Workstation, para acceder a ella es necesario iniciar sesión a la maquina con los siguientes datos:
```
	usuario: ibmdemo
	contraseña: passw0rd
```

## Revisar aplicaciones on-premise a analizar

En este paso podrán observar las aplicaciones ejemplo desplegadas de manera local en el ambiente del servidor WebSphere Application.

1. Después de haber ingresado a la maquina virtual Workstation, ahora es necesario encender el servidor local que posee las aplicaciones a manejar en el ejemplo, para esto es necesario acceder a la terminal de la maquina dando click en la barra de acceso izquierda.
2. Una vez que la terminal está abierta, se ingresa el siguiente el comando para encender el servidor WAS:
```
	/home/ibmdemo/cp4a-labs/shared/startWAS.sh
```

Una vez ingresado es posible que se le pida la constraseña del usuario sudo, por lo tanto debe ingresar la contraseña: passw0rd. El proceso puede llevar un par de minutos.

3. Ahora hay que acceder a la consola de administrador del WebSphere para observar la aplicación desplegada, para esto es necesarios ingresar al navegador web Firefox, donde encontrara una pestaña en el bookmark con el nombre de WebSphere Integrated Solution Console y debe hacer click encima de ella.

4. Cuando ya pueda observar la consola del WAS, es necesario ingresar al sistema con las siguientes credenciales y hacer click en login:

```
	usuario: wsadmin
	contraseña: passw0rd
```

5. En la consola WAS es necesario acceder a la siguiente ruta para ver las aplicaciones desplegadas:
```
	Applications -> Application Types -> WebSphere Enterprise applications
```
6. Al ingresar a la lista podrá observar todas las aplicaciones que están desplegadas localmente en el servidor WebSphere, de las cuales algunas serán utilizadas en las próximas secciones.

## Descargar el Transformation Advisor Collector

El Transformation Advisor puede evaluar cualquier aplicación basada en Java. Durante este hands on , se utilizará para evaluar si la aplicación de WebSphere, Mod Resorts, es adecuada para pasar a la nube y cuál es el esfuerzo necesario para llegar allí. El transformation advisor en este caso esta corriendo bajo OpenShift y utilizará la utilidad del recopilador de datos para obtener los datos de la aplicación.

Para evaluar las aplicaciones locales de Java, debe ejecutar la utilidad del recopilador de datos del transformation advisor en el entorno del servidor de aplicaciones para extraer toda la información de la aplicación del entorno primero, para realizar esto debe seguir lo siguientes pasos:

1. Para descargar la utilidad, es necesario acceder por medio del navegador Firefox al bookmark que lleva por nombre IBM Transformation Advisor, una vez abra la página web hay que iniciar sesión seleccionando primero el ingreso por htpasswd e ingresar los siguientes datos:
```
	usuario: ibmadmin
	constraseña: engageibm
```
2. Una vez el acceso sea correcto se ingresa a la página de bienvenida del transformation advisor, donde es necesario crear un nuevo espacio de trabajo o workspace, para esto se ingresa  el nombre Evaluation y se da click en siguiente.

3. Ahora es necesario crear una colección dentro del espacio de trabajo, el cual llevara por nombre Server1 y dar click en let’s go.

4. Una vez que creado el espacio de trabajo y la colección, tendrá dos opciones una es descargar la utilidad de recopilador de datos (Data Collector) o cargar el archivo de datos existente. En este caso se utilizará el Data Collector para esto hay que hacer clic en el Data Collector para ir a la página de descarga.

5. En la página de descarga se puede descargar diferentes versiones de la utilidad según el sistema operativo deseado. También muestra cómo utilizar la utilidad en la línea de comandos para recopilar datos de aplicaciones de los servidores de WebSphere, WebLogic y Tomcat. Dado que el ambiente es una maquina virtual con Linux, se descarga el instalador para dicho sistema operativo.

6. Cuando se abre la ventana de dialogo de Descarga, hay que seleccionar la opción de guardar archivo y aceptar. La dirección del archivo será la siguiente:

```
	/home/ibmdemo/Downloads
```


## Correr la utilidad de recolección de datos del Transformation Advisor

1. Ir a la terminal de la maquina virtual Workstation desde el escritorio, navegar hasta la ruta /home/ibmdemo/Downloads y observar el contenido de la carpeta.

```
	cd /home/ibmdemo/Downloads/
	ls -l
```
 2. Ahora que se ve el archivo descargado de la utilidad de Transformation Advisor, es necesario extraerlo desde el archivo .zip al computador para eso se ingresa el siguiente comando:
```
	tar xvfz transformationadvisor-Linux_Evaluation_Server1.tgz
```
Cuando se extraen los archivos quedan en en la ruta /home/ibmdemo/Downloads/transformationadvisor-2.0.3 de la maquina virtual.

 3. El siguiente paso es ejecutar el recolector de datos del transformation advisor con las aplicaciones del servidos WebSphere Local, es necesario ejecutar el comando proporcionado e ingresar la contraseña passw0rd cuando sea necesario. 
```
	cd /home/ibmdemo/Downloads/transformationadvisor-2.0.3

	sudo ./bin/transformationadvisor -w /opt/IBM/WebSphere/AppServer -p AppSrv01wsadmin passw0rd
```

4. Cuando la terminal solicite ingresar un numero digite 1 para aceptar la licencia y presione enter,  la utilidad comenzara a recolectar los datos de las aplicaciones. 

Este proceso tardará algunas veces en completarse según la cantidad de aplicaciones desplegadas en el servidor WAS local. En este HandsOn, podrían ser unos minutos. Cuando termine, verá el mensaje que indica que termino el proceso:  "Thank you for uploading your data. You can proceed to the application UI for doing further analysis”. 

 Los datos de su aplicación se recopilan y se guardan como un archivo zip en el directorio de herramientas como se muestra a continuación. 

En general, si su servidor de aplicaciones y el Transformation Advisor están en la misma infraestructura de red, los datos recopilados se cargarán automáticamente al Transformation Advisor para que pueda ver los resultados del análisis. De lo contrario, debe cargar manualmente los datos en el Transformation Advisor antes de poder verlos.

## Evaluación de las aplicaciones java on-premise
En esta parte de la guía se va a utilizar el Transformation Advisor para ver los resultados del analisis del recolector de datos. 

1. Regresar al navegador web Firefox, donde se encuentra la pagina de Transformation Advisor y dar clic sobre el link Server1 para ir a la sección de recomendaciones.

En la página de recomendaciones se pueden observar todas las aplicaciones desplegadas dentro del servidor WAS local.

En la página de Recomendaciones, el entorno de origen de migración identificado se muestra en la sección Perfil y el entorno de destino en la sección Migración preferida. La herramienta de recopilación de datos detecta que el entorno de origen es su perfil de aplicación WebSphere Application Server ND. El entorno de destino es Liberty en OpenShift, que es el entorno de destino predeterminado. La página de Recomendaciones también muestra los resultados del análisis resumido de todas las aplicaciones en el entorno AppSrv01 que se moverán a un entorno Liberty en OpenShiften. Para cada aplicación, puede ver estos resultados: • Nombre • Nivel de complejidad • Coincidencia de tecnología • Dependencias • Problemas • Costo de desarrollo estimado en días Por ejemplo, si desea moverlosodresorts-1_0_war.earapplication a Liberty en OpenShift, el nivel de complejidad es Simple y Tech la coincidencia es del 100%, lo que indica que no es necesario cambiar el código de la aplicación para poder moverlo a la nube. La aplicación tiene dependencia de nodo, tiene 1 problema de menor nivel y el esfuerzo de desarrollo estimado es de 0 días porque no hay cambio de código. Como puede ver, el movimiento predeterminado al entorno de la nube es Liberty en OpenShift, sin embargo, el Asesor de transformación también puede proporcionar opciones de migración si desea migrar su aplicación a diferentes entornos de destino como se muestra abajo:



