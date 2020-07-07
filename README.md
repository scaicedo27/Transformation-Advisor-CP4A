# Transformation-Advisor-CP4A
En este Hands On se va a realizar una muestra dentro de un ambiente de pruebas donde se utilizará las tecnológias de IBM como lo es Data Collector Utility y Transformation  Advisor para evaluar si una aplicación java cumple con los requisitos para pasar a un ambiente Cloud.

## Requisitos
1. Tener una cuenta de IBM Cloud, de no tenerla crearla en el siguiente link:


## Ambiente

[https://bluedemos.com/show/2459](https://bluedemos.com/show/2459)

## Habilitar ambiente

En este paso se dejara listo el ambiente para trabajar sobre las maquinas virtuales brindadas por Skytap al momento de solicitar el ambiente de pruebas en bluedemos.

1. Ingresar al link enviado a su correo del acceso a las maquinas virtuales con skytap.
2. Encender las seis maquinas virtuales que se le fueron asignadas, dando click en el botón de play.
![Captura de Pantalla 2020-07-07 a la(s) 1 12 24 a  m](https://user-images.githubusercontent.com/45157348/86725280-21993880-bfef-11ea-97e0-5b8bfb525e20.png)
3. Cuando las maquinas esten encendidas dar click sobre la maquina virtual llamada Workstation, para acceder a ella es necesario iniciar sesión a la maquina con los siguientes datos:
![Captura de Pantalla 2020-07-07 a la(s) 1 12 35 a  m](https://user-images.githubusercontent.com/45157348/86725305-25c55600-bfef-11ea-82d0-ffbe341ced76.png)

```
	usuario: ibmdemo
	contraseña: passw0rd
```

## Revisar aplicaciones on-premise a analizar

En este paso podrán observar las aplicaciones ejemplo desplegadas de manera local en el ambiente del servidor WebSphere Application.

1. Después de haber ingresado a la maquina virtual Workstation, ahora es necesario encender el servidor local que posee las aplicaciones a manejar en el ejemplo, para esto es necesario acceder a la terminal de la maquina dando click en la barra de acceso izquierda.
2. Una vez que la terminal está abierta, se ingresa el siguiente el comando para encender el servidor WAS:
![Captura de Pantalla 2020-07-07 a la(s) 1 12 48 a  m](https://user-images.githubusercontent.com/45157348/86725319-2958dd00-bfef-11ea-827c-8ff70cde3665.png)
```
	/home/ibmdemo/cp4a-labs/shared/startWAS.sh
```

Una vez ingresado es posible que se le pida la constraseña del usuario sudo, por lo tanto debe ingresar la contraseña: passw0rd. El proceso puede llevar un par de minutos.
![Captura de Pantalla 2020-07-07 a la(s) 1 13 00 a  m](https://user-images.githubusercontent.com/45157348/86725352-2f4ebe00-bfef-11ea-8a41-ee60348c7455.png)
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

En la página de Recomendaciones, el entorno de origen de migración identificado se muestra en la sección Profile y el entorno de destino en la sección Preferred Migration. La herramienta de recopilación de datos detecta que el entorno de origen es su perfil de aplicación WebSphere Application Server ND. El entorno de destino es Liberty en OpenShift, que es el entorno de destino predeterminado para las aplicaciones Java On-Premise. 

La página de Recomendaciones también muestra los resultados del análisis resumido de todas las aplicaciones en el entorno AppSrv01 que se moverán a un entorno Liberty en OpenShift. Para cada aplicación, puede ver estos resultados:

 - Nombre
 - Nivel de complejidad
 - Coincidencia de tecnología
 - Dependencias 
 - Problemas identificados
 - Costo de desarrollo estimado en días

Por ejemplo, si desea mover la aplicación modresorts-1_0_war.ear a Liberty en OpenShift, el nivel de complejidad es Simple y la tecnología concuerda en un 100%, lo que indica que no es necesario cambiar el código de la aplicación para poder moverlo a la nube. La aplicación tiene dependencia de nodo, tiene 1 problema de menor nivel y el esfuerzo de desarrollo estimado es de 0 días porque no hay cambio de código.

Por defecto el movimiento predeterminado al entorno de Cloud es Liberty en OpenShift, sin embargo, el Transformation Advisor también puede proporcionar opciones de migración si desea migrar su aplicación a diferentes entornos de destino.

## Observar la aplicación Mod Resorts

1. Desde la ventana del navegador web, haga clic en la nueva pestaña para abrir una nueva ventana del navegador. Escriba la aplicación ModResorts URL: http: // localhost: 9080 / resorts / y presione Entrar. Se muestra la página de inicio de la aplicación Mod Resorts.

## Análisis de resultados Mod Resorts
Si se observa las complejidades de estas aplicaciones, puede ver que moderesorts-1.0_war.ear  y pbw-ear.ear tienen la complejidad simple, lo que significa que estas dos aplicaciones se pueden migrar al Cloud sin ninguna alteración al código. Pero dado que moderesorts-1.0_war.earapp tiene un problema menor (1) que pbw-ear.earapp (4), se analizaran los resultados de la aplicación moderesorts-1.0_war.ear en detalle.

1.  Seleccione el link moderesorts-1_0_war.ear para ver el resultado del analisis. 

La primera sección en la página de resumen del análisis detallado es la sección de Complejidad. La complejidad general de la aplicación es simple, lo que indica que la aplicación se puede mover directamente a la nube sin ningún cambio de código.

2. Despláce hacia abajo hasta la sección Application Details. Puede ver que aunque no se requiere un cambio de código ni un costo de desarrollo, la migración estimada de todos los costos de desarrollo es de 5 días. Esta estimación se basa en datos de compromisos de servicios de IBM, que incluyen la gestión de migración, la configuración del servidor y las pruebas.

3. Continúe desplazándose hacia abajo hasta la sección Issues o Problemas donde puede observar que el único problema potencial es menor y tiene que ver con  la configuración de la aplicación en el contenedor Docker.

4. A continuación, desplácese hacia abajo hasta la parte inferior de la página y haga clic en el enlace Informe de tecnología o Technology Report; se abrirá una nueva ventana del navegador para mostrar el Informe de evaluación de la aplicación.

El reporte lista todas las tecnologías Java que la aplicación utilizó y si estas tecnologías son compatibles con una plataforma WebSphere específica desde Liberty para Java en IBM Cloud hasta WebSphere tradicional para z/OS. Se utiliza para determinar si un producto WebSphere en particular es adecuado para una aplicación.

Como puede ver en el informe, la aplicación Mod Resorts solo utiliza Java Servlet, que es compatible con todas las plataformas de WebSphere.

5. Volver a la pagina de Transformation Advisor y dar clic en el link de Analysis Report o Reporte de análisis, despues dar clic en continuar y podra ver ahora en detalle el reporte de migración.

En este reporte se muestran todos los problemas encontrados a nivel de código.

6. Deslice hasta la sección Detailed Results by Rule, donde puede observar todos los problemas identificados en base a las reglas de migración. Para la aplicación que estamos analizando aparece solo un error en la configuración para contenedores Docker. 

Dando clic en el link Show results, se puede observar en detalle los problemas a nivel de código de manera especifica mostrando la clase y la linea de error. Esto es un beneficio para ayudar a los desarrolladores a encontrar los problemas especificos.

7. Regrese a la pagina de Transformation Advisor desde el navegador y de clic en el link Inventory report donde se mostrará un informe detallado que ayuda a examinar qué hay en su aplicación, incluida la cantidad de módulos, sus relaciones y las tecnologías en esos módulos. También le ofrece una vista de todos los archivos JAR de utilidad en la aplicación que tienden a acumularse con el tiempo. También se incluyen posibles problemas de implementación y consideraciones de rendimiento.

Desplácese hacia abajo para ver este informe que sirve como una buena herramienta de toma de decisiones para informarle lo que hay dentro de su tiempo de ejecución y para ayudarlo a comprender mejor el tiempo de ejecución, los componentes que tiene y las relaciones entre ellos.

De los informes de análisis que examinó anteriormente, sabe que la aplicación Mod Resort es compatible con Liberty en OpenShif, que es el entorno de destino, y el problema que la herramienta identificada no afectaría a la migración de la aplicación. Puede seleccionar con confianza la aplicación como un buen candidato para pasar a la libertad en la nube en el proceso de reempaquetado con el mínimo esfuerzo.



