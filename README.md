# TutorialRPA
Un sencillo tutorial de como usar UIPath para extraer información de un CSV y llenar automaticamente un formulario de Google.

Usando una de las herramientas más populares en Robotic Process Automation.

##Introducción
En este tutorial voy a explicar qué es Robotic Proccess Automation (RPA) y luego describiré el paso a paso para automatizar el llenado de un formulario ficticio con información extraída de un base de datos en formato CSV usando UiPath.

###0- ¿Qué es RPA?
De acuerdo al “Institute for Robotic Process Automation & Artificial Intelligence”, RPA se entiende como:

“Automatización robótica de procesos a la aplicación de tecnología que facilita a los empleados de una compañía a configurar un softbot capaz de capturar e interpretar existentes procesos de negocios, transacciones, manipulación de información y comunicación automática con otros sistemas.”
Diferencias con sistemas tradicionales de automatización:
Un sistema de RPA se diferencia de un sistema informático de automatización tradicional debido a las siguientes características:

Sus usuarios finales normalmente son gente sin necesidad de poseer un background técnico. (Ejem: Financistas)
Poseen una interfaz gráfica de usuario configurable a través de la grabación de pasos repetitivos.
No es necesario saber programar para poder utilizar dichas herramientas.
Proveedores de sistemas de RPA:
De acuerdo al reporte “The Forrester Wave™: Robotic Process Automation, Q1 2017” (2017), los proveedores líderes de RPA hoy en el mercado son Automation Anywhere, Blueprism y UIPath.


Dado a qué UIPath posee una versión gratuita, este tutorial esta basado en dicha herramienta.

Vamos a necesitar:

*UIPath Studio Community (En el tutorial usamos la versión 2017.1.6309)
*La librería de UiPath Studio Communiy “Excel Activities”.
*Repositorio con CSV de prueba: https://github.com/IAARhub/TutorialRPA
*Formulario de GoogleForms de prueba: https://goo.gl/forms/HV5QjRDt0wmP6p9z2

###1- UIPath, secuencias de procesos.
Bajando la versión community de UIPath.
Nos dirigimos a https://www.uipath.com/community y luego hacemos clic en “get community version”.


Una vez bajado, se precisa instalarlo. Terminada la instalación abrimos el UIPath y hacemos click en “Simple Proccess”. Luego asignamos un nombre al proyecto y descripción (Opcional).


Y borramos la secuencia por default:


Debería quedar así:


####Instalando Excel activities en UIPath Studio…
Vamos al icono de “paquetes” en la barra del costado izquierdo. Luego vamos a Available -> All y seleccionamos “UI.Path.Excel.Activities”, le damos al botón de “Install”. (Luego aparecerá una tilde indicando que ya esta instalado el paquete).


#####Iniciando secuencia…
Ahora necesitamos agregar la secuencia de procesos para el llenado del formulario. En Favorites -> Sequence, arrastramos el bloque de “Sequence” al diagrama de flujo de trabajo.


Hacemos click en nuestro nuevo bloque creado. (Donde dice “Sequence”).

Ahora necesitamos agregar el primer bloque de proceso de nuestra secuencia. Para ello hacemos clic en “Available” -> “UI Automation” (Ubicado en la parte izquierda de la pantalla), vamos a “Browser” y tomamos “Open Browser” y lo soltamos dentro del contenedor de nuestra secuencia.

En “URL” escribimos el link a nuestro formulario de prueba: “https://goo.gl/forms/HV5QjRDt0wmP6p9z2” , recordá que debe estar escrito entre comillas para que funcione.


NOTA: Por default el browser a usar es el IE, el cuál usaremos en este tutorial dado que no requiere ninguna instalación adicional. En caso de querer usar Chrome es necesario bajarse la extensión de UIPath para Chrome: https://studio.uipath.com/v2016.2/docs/installing-the-chrome-extension-for-uipath-studio

Creando toda la secuencia.
En este caso el formulario de prueba tiene 3 inputs diferentes: (A) Nombre, (B) Apellido y © E-mail. Nuestro objetivo será crear una secuencia completa de llenado automatico que tipee de manera programa en cada elemento html del navegador la información provista por una base de datos CSV. Antes de extraer la información del CSV crearemos el paso a paso.

Para crear la secuencia, es necesario pensar cómo haría el proceso un humano. En este caso los pasos serian:

1- Tipear en “Nombre” mi nombre.

2- Tipear en “Apellido” mi apellido.

3- Tipear en “E-mail” mi mail.

4- Darle clic a “Enviar”.

Para empezar, vamos a crear un nuevo nodo de proceso seleccionando UI Automation -> Element -> Keyboard -> Type into

Al bloque de “Type into” lo tomamos y soltamos en el contenedor “Do” de nuestra secuencia.


Abrimos una pestaña/ventana de chrome a nuestro formulario de prueba: https://goo.gl/forms/HV5QjRDt0wmP6p9z2

Ahora debemos indicar en qué elemento se cliqueará.

Para ello hacemos clic en “Indicate element inside browser”.

En la ventana de navegador de nuestro formulario hacemos click en el elemento input html debajo de “Nombre”.


Repetimos el proceso con los otros dos inputs:


Vamos a colocar alguna información de prueba en cada uno. Recorda que lo que escribas debe estar con comillas.


Ahora hace falta automatizar el clic al botón de “enviar”. Para ello creamos un nuevo nodo de proceso seleccionando “UI Automation -> Element -> Mouse -> Click


Le damos a “Indicate element inside browser” y seleccionamos el botón de “enviar”.


NOTA: Notarán que estos pasos pueden grabarse usando el “Web recorder”. En función de qué el lector pueda tener un entendimiento más amplio del paso a paso y de la importancia de captar los pasos humanos, en este tutorial lo hacemos paso por paso. Asimismo, a veces al grabar se adicionan “procesos de ruido”, es decir procesos que accidentalmente hicimos y luego debemos editar.

Ahora es momento de probar nuestro “script” de automatización. Para ello vamos a “Run” o apretamos “F5”:


Como vamos a querer volver a agregar más registros a partir de nuestra base de datos vamos a agregar un bloque al final de la cadena de “Go Back”.

El cual se encuentra en UI Automation -> Browser -> Go back.


Ahora hace falta extraer la información del CSV.

###2- Extrayendo la información del CSV.
Bajando los archivos:
Primero que nada precisamos bajarnos nuestro CSV de prueba, para ello vamos a : https://github.com/IAARhub/TutorialRPA

Y nos bajamos el repositorio.


En la carpeta /data esta nuestro archivo “data.csv”.

Leyendo el CSV en UIPath:
Ahora volvemos a nuestro diágrama de flujo de trabajo haciendo doble clic en “Main”.

Precisamos hacer nuestro nodo de procesos en torno al procesado del CSV.

Para eso vamos a Available -> App Integration -> Excel -> CSV -> Read CSV

Y arrastramos ese bloque dentro de nuestro flujo de trabajo “Main”.


Doble clic en el bloque de “Read CSV” para abrirlo y poder editarlo.

Hacemos clic en los tres puntillos para buscar nuestro archivo.


“
Ahora volvemos al flujo de trabajo “Main”, hacemos clic derecho en el bloque de “Read CSV” y le damos a “Set as start node”:


Nuestro CSV debe tener un formato de salida de DataTable para poder ser procesado, para ello vamos a “Create Variable” -> “Table”.


Le asignamos un nombre. A modo de ejemplo en este tutorial la vamos a llamar “dataTable”.

Ahora asignamos esa variable como salida de nuestro CSV:


Ahora vamos a crear nuestro “loop” de iteración de registros de nuestra base de datos:

Vamos a Available -> Programming -> DataTable -> For Each Row


Ahora doble clic en el nodo de “For each row” para editarlo.

Escribimos la expresión “dataTable” luego de “in”:


Ahora volvemos al diagrama de flujo de trabajo “Main” , hacemos clic derecho en el nodo de “For each row” y lo cortamos.


Abrimos la secuencia que hicimos haciendo doble clic en “Sequence”. Y lo pegamos luego del bloque de “Open Browser” dentro del contenedor de “Do”:


Ahora arrastramos todos los pasos de la secuencia dentro del contenedor de “Body” de nuestro loop:


En el primer “Type into” vamos a colocar la información de la fila de nombres de nuestro csv, para ello escribimos:

row(“Nombre”).ToString
En el segundo escribimos:

row(“Apellido”).ToString
y en el tercero :

row(“Mail”).ToString
Hemos terminado toda nuestra secuencia, ahora volvemos al diagrama de flujo de trabajo “Main” y nos aseguramos de conectar los nodos de la siguiente manera:


Y LISTO ! Felicitaciones, ya tenes tu proyecto de RPA terminado. Ahora solo queda darle “play” y ver los resultados.

Fuentes y documentación:
Definition & Benefits. Institute for Robotic Processs Automation & Artificial Intelligence (2017), recuperado de http://irpaai.com/definition-and-benefits/
The Forrester Wave™: Robotic Process Automation, Q1 2017 (2017) https://www.edgeverve.com/wp-content/uploads/2017/02/forrester-wave-robotic-process-automation.pdf
Repositorio de Github “TutorialRPA”, IAARhub, IAAR (2017) https://github.com/IAARhub/TutorialRPA
Web Data Entry Automation. CSV to Salesforce — UiPath Studio (2014). UIPath, https://www.youtube.com/watch?v=EhEE_DVxaS4

https://planetachatbot.com/tutorial-rpa-automatizando-el-llenado-de-un-formulario-con-uipath-244f11f90403
