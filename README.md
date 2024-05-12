# Git web practice branch

* [Instrucciones](http://misovirtual.virtual.uniandes.edu.co/codelabs/git_web_practice_branch/index.html#0)


# Pruebas de Regresión Visual | Ghost v5 y Ghost v3

Nombre | Email Uniandes
-- | --
Héctor Oswaldo Franco   | h.franco@uniandes.edu.co 
Manuel Felipe Bejarano | mf.bejaranob1@uniandes.edu.co
Juan Sebastián Vargas     | js.vargasq1@uniandes.edu.co
María Camila Martínez  | mc.martinezm12@uniandes.edu.co

# Indice
1. [Descripción del set de pruebas VRT](#1-descripción-del-set-de-pruebas-vrt)
2. [Setup de las pruebas VRT](#2-setup-de-las-pruebas-vrt)
   - [Especificaciones técnicas del ambiente de pruebas usado](#21-especificaciones-técnicas-del-ambiente-de-pruebas-usado)
   - [Instalación Node JS o NVM](#22-instalación-node-js-o-nvm)
   - [Instalación navegador Google Chrome](#23-instalación-navegador-google-chrome)
   - [Instalación IDE](#24-instalación-ide)
   - [Instalar GIT](#25-instalar-git)
3. [Setup proyecto](#3-setup-proyecto)
   - [Descomprimir el proyecto](#31-descomprimir-el-proyecto)
   - [Estructura general del proyecto](#32-estructura-general-del-proyecto)
   - [Instalar las librerías locales del proyecto](#33-instalar-las-librerías-locales-del-proyecto)
   - [Servicio de Ghost](#34-servicio-de-ghost)
4. [Ejecución de las pruebas E2E](#4-ejecución-de-las-pruebas-e2e)
   - [Herramienta Kraken](#41-herramienta-kraken)
     - [Estructura del proyecto](#411-estructura-del-proyecto)
     - [Nomenclatura de los escenarios](#412-nomenclatura-de-los-escenarios)
     - [Ejecución de las pruebas](#413-ejecución-de-las-pruebas)
     - [Posibles situaciones que se pueden presentar](#414-posibles-situaciones-que-se-pueden-presentar)
   - [Herramienta Cypress](#42-herramienta-cypress)
     - [Instalar Cypress v13.7.3](#421-instalar-cypress-v1373)
     - [Correr instancia de Cypress](#422-correr-instancia-de-cypress)
     - [Seleccionar la carpeta del proyecto Cypress](#423-seleccionar-la-carpeta-del-proyecto-cypress)
     - [Seleccionar la prueba E2E](#424-seleccionar-la-prueba-e2e)
     - [Iniciar la prueba E2E](#425-iniciar-la-prubeba-e2e)
     - [Ejecutar la prueba](#426-ejecutar-la-prueba)
   - [Ejecución pruebas E2E en Ghost 3.42.0](#43-ejecución-pruebas-e2e-en-ghost-3420)
5. [Funcionalidades y escenarios extra](#5-funcionalidades-y-escenarios-extra)
6. [Ejecución de las pruebas VRT](#6-ejecución-de-las-pruebas-vrt)
   - [Herramienta BackstopJS](#61-herramienta-backstopjs)
     - [Estructura de carpetas Backstopjs](#611-estructura-de-carpetas-backstopjs)
     - [Ejecución Backstopjs](#612-ejecución-backstopjs)
     - [Funcionamiento de Backstopjs](#613-funcionamiento-de-backstopjs)
   - [Herramienta ResembleJS](#62-herramienta-resemblejs)
7. [Documentación extra](#7-documentación-extra)


# 1. Descripción del set de pruebas VRT
Las Pruebas de Regresión Visual o Visual Rregression Testing en Inglés, son ampliamente usadas para detectar cambios en versiones `x + 1` de las aplicaciones y comprobar que estos cambios no afectan la experiencia del cliente y no se intrudujeron bugs o fallos en el proceso de propagación de cambios.

En esta entrega se realizarán Pruebas de Regresión Visual en la ABP, Ghost. Las versiones utilizadas para este propósito se listan a continuación

Versión | URL Despliegue | ¿Es línea base?
-- | -- | --
5.14.1 | https://ghost-fcj4.onrender.com/   | Sí
3.42.0 | https://ghost-3-42-0.onrender.com/ | No


# 2. Setup de las pruebas VRT
Primero es necesario instalar un conjunto de herramientas globales que servirán para instalar las herramientas de pruebas de regresión visual VTT, **Kraken**, **Cypress**, **Resemble.js** y **Backstop**

## 2.1 Especificaciones técnicas del ambiente de pruebas usado:
* SO: Windows 11+ y MacOS Sonoma 14.1.1
* Node Versión: v20.12.0
* NPM Versión: v10.5.0
* GIT: Versión más reciente o predefinida en sistemas UNIX
* Visual Studio Code

## 2.2 Instalación Node JS o NVM
Para poder replicar bien este set de pruebas es requerido instalar en su máquina local la versión de [Node JS](https://nodejs.org/en) descrita en la sección 2.1, o mejor aún, instalar [NVM](https://github.com/nvm-sh/nvm), para poder alternar entre las diferentes versiones de Node disponibles.

Una vez instalado, se puede comprobar con el siguiente comando en la terminal o línea de comando de windows: 
Node: 
```bash
> node --version
v20.12.0
```
npm: 
```bash
> npm --version
10.5.0
```

## 2.3 Instalación navegador Google Chrome
Para la correcta ejecución de las dos herramientas es requerido instalar [Google Chrome](https://www.google.com/intl/es-419/chrome/). Por favor asegurarse que su versión de Chrome es la 124 o posterior tanto para UNIX como Windows

## 2.4 Instalación IDE 
Aunque usted no vaya a tocar una línea de código del proyecto, le recomendamos qué por favor instale el IDE [Visual Studio Code](https://code.visualstudio.com/) el cual le permitirá ver el proyecto como un todo y explorar las distintas carpetas que este posee en orden de entender mejor ambas herramientas.

## 2.5 Instalar GIT
Para clonar los repositorios en donde se encuentran las herramientas, es necesario usar la herramienta GIT, la cual puede ser instalada siguiendo los pasos de su [página oficial](https://git-scm.com/downloads) en la sección downloads.

Una vez instaladas las herramientas, se puede comprobar su correcto funcionamiento con el siguiente comando. El resultado debe ser algo parecido a esto.

```bash
> git --version
git version 2.39.3 (Apple Git-145)
```

# 3. Setup proyecto
Una vez se tiene el entorno inicial de pruebas preparado, procedemos a instalar las librerías propias del proyecto para que se ejecute de forma satisfactoria.

## 3.1 Descomprimir el proyecto
Si bien puede descargar el proyecto desde el apartado **release** de Github, también puede clonarlo, y descargarlo en su máquina local haciendo uso del siguiente comando:

```bash
> git clone https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas.git
```

## 3.2 Estructura general del proyecto

A continuación se muestra la estructura global del proyecto. Este se divide en cuatro grandes grupos. **Ghost_v3** y **Ghost_v5**, **ResembleJS** y **BackstopJS**, y las carpetas con las versiones de Ghost se dividen a su vez en 2 sub grupos, **Kraken** y **Cypress**

<img width="272" alt="Screenshot 2024-05-11 at 10 14 14 PM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/a0f9e708-c61a-4302-9a5b-9a46a35d3c79">

## 3.3 Instalar las librerías locales del proyecto
Una vez se han instalado las herramientas globales en la máquina local y se ha descomprimido el proyecto, es indispensable instalar las dependencias propias de cada una de las herramientas. Para ello solo es necesario hacer dos pasos:

1. Abrir una terminal o consola de comandos (CMD) sobre la carpeta raíz del proyecto
2. Ejecutar el siguiente comando de Node:
```bash
> npm install
```
Esto descargará e instalará las dependencias necesarias para ambas herramientas, el listado de dependencias se muestra a continuación.

```json
  "dependencies": {
    "@faker-js/faker": "^8.4.1",
    "android-platform-tools": "^3.0.2",
    "appium": "^2.5.4",
    "assert": "^2.1.0",
    "chai": "4.4.1",
    "kraken-node": "^1.0.24",
    "resemblejs": "^5.0.0",
    "backstopjs": "^6.3.23"
  }
```
Si desea consultar más sobre esto, puede ver el archivo `package.json`


## 3.4 Servicio de Ghost

<p align="center">
<img width="300" alt="Screenshot 2024-05-02 at 9 51 07 PM" src="https://ptimofeev.com/images/render.png">
<p align="center">render.com</p>


Las instancias de Ghost sobre las que se ejecutarán las pruebas VRT se encuentran en la plataforma render.com y pueden ser accedidas desde los siguientes enlaces. 

 * [Ghost 5.14.1](https://ghost-fcj4.onrender.com/ghost)
   * Usuario: `h.franco@uniandes.edu.co`
   * Contraseña: `miso20244103`

 * [Ghost 3.42.0](https://ghost-3-42-0.onrender.com/)
   * Usuario: `jsvargasq@hotmail.com`
   * Contraseña: `AdminR00t123!`


# 4. Ejecución de las pruebas E2E

Ahora se procederá a ejecutar las pruebas End-2-End modificadas de la entrega anterior. Las modificaciones hechas permiten la captura de screenshots o pantallazos durante la ejecución de las pruebas por cada escenario y funcionalidad probada.

## 4.1 Herramienta Kraken

<p align="center">
<img src="https://raw.githubusercontent.com/TheSoftwareDesignLab/KrakenMobile/master/reporter/assets/images/kraken.png" alt="kraken logo" width="140" height="193">
<p align="center">Kraken</p>
    
Ya que la estructura del proyecto ahora contempla las dos versiones de Ghost. Se debe realizar un paso extra para poder ejecutar las pruebas E2E de Kraken en la versión 5.14.1 de Ghost.

Dentro de la base de la carpeta raíz del proyecto **MISW4103-pruebas-automatizadas** realizar los siguientes comando
```bash
> cd Ghost_v5
> cd kraken
```
Por favor asegurarse de que está escribiendo Ghost con la G en mayúsculas para que el sistema de ficheros pueda dirigirse exitosamente a esa carpeta.

### 4.1.1 Estructura del proyecto:

La estructura del proyecto debe verse de la siguiente manera

<img width="317" alt="Screenshot 2024-05-05 at 9 48 05 PM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/93833525-cc94-4ccd-9d4d-74fef82dbf17">


*NOTA:* Cada *feature* está nombrado con la siguiente nomenclatura **FU00X_ESC00X**, la cual hace referencia a la funcionalidad sobre la que se está haciendo el escenario de prueba.

### 4.1.2 Nomenclatura de los escenarios

Los escenarios cumplen la siguiente nomenclatura para diferenciar los unos de los otros y correr en instancias separadas.
```
🗂️ kraken
  🗂️ features
    🗂️ web
      🗂️ steps_definitions
        📄 steps.js

    📄 FU001_ESC001.cy.js
    📄 FU001_ESC002.cy.js
    📄 FU002_ESC002.cy.js
```

### 4.1.3 Ejecución de las pruebas
Una vez adentro de esa carpeta puede ejecutar el siguiente comando que dará inicio a la ejecución de los escenarios de prueba disponibles.
```bash
> npx kraken-node run
```
Cada prueba realiza el escenario descrito y al finalizar realiza un reporte en HTML que puede ser consultado en la carpeta **reports**

De igual manera los screenshots tomados durante la ejecución de las pruebas pueden ser encontrados en carpeta **screenshots**. La estructura de esta carpeta por dentro debe lucir de la siguiente manera.

<img width="351" alt="Screenshot 2024-05-11 at 8 51 12 PM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/d5f7a422-020f-4390-ada1-87bdcf4cee9a">

Como se puede apreciar, dentro de la ruta `Ghost_5/Kraken/screenshots` se encuentran las carpetas que contienen los screenshots por cada escenario.


### 4.1.4 Posibles situaciones que se pueden presentar
Dependiendo del sistema operativo en el que se ejecuten las pruebas, estas pueden o no correr automáticamente una detrás de la otra.

Si se ejecuta la prueba en un Sistema Operativo tipo UNIX o Linux, deberían correr los escenarios uno detrás del otro automáticamente, si por el contrario se está en Windows, hay una probabilidad de que solo ejecute el primero en orden alfabético y al finalizar no siga con los demás.

Para remediar esto por favor en la carpeta de features sólo dejar un escenario y ejecutar así cada uno de ellos.

Por otro lado, ya que las pruebas toman un tiempo considerablemente mayor al hacer la toma de screenshots por cada paso ejecutado del escenario es posible que el computador se suspenda durante la prueba, lo que puede ocasionar que, las pruebas fallen automaticamente. Para no tener este inconveniente por favor asegurarse de que el computador tiene carga suficiente y en su configuraciòn no se tiene la suspensiòn automática después de unos minutos de inactividad.


## 4.2 Herramienta Cypress

<p align="center">
<img src="https://static-00.iconduck.com/assets.00/cypress-icon-2048x2045-rgul477b.png" alt="kraken logo" height="200">
<p align="center">Cypress</p>

Dentro de la base de la carpeta raíz del proyecto **MISW4103-pruebas-automatizadas** realizar los siguientes comandos

```bash
> cd Ghost_v5
> cd cypress
```
Por favor asegurarse de que está escribiendo Ghost con la G en mayúsculas para que el sistema de ficheros pueda dirigirse exitosamente a esa carpeta.

La estructura del proyecto debe verse así:

<img width="303" alt="Screenshot 2024-05-05 at 11 37 22 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/64bb1f1d-3c6f-4753-8416-226590c81311">

### 4.2.1 Instalar Cypress v13.7.3
Abrir una línea de comandos o terminal de la máquina y escribir el siguiente comando.
```bash
> npm -g install cypress@13.7.3
```
Esto instalará Cypress de forma global en el PC para ser usado desde cualquier punto.

### 4.2.2 Correr instancia de Cypress
Después en una terminal o consola de comandos abierta corremos el siguiente comando:
```bash
> cypress open
```
Una vez ejecutado el comando, debe abrirse una ventana similar a esta.

<img width="1205" alt="Screenshot 2024-05-04 at 11 52 14 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/7967dd94-6d31-4cb7-9739-8ded0aa04524">

### 4.2.3 Seleccionar la carpeta del proyecto Cypress:
Presionamos botón **Add project** de la vista principal de Cypress y seleccionamos la carpeta raíz del proyecto llamada `MISW4103-pruebas-automatizadas/Ghost_v5/cypress`.

<img width="1205" alt="Screenshot 2024-05-04 at 11 53 19 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/847aceee-48d8-464a-875d-a1ad05ec6799">

<img width="1206" alt="Screenshot 2024-05-04 at 11 55 03 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/709fbd2d-a8f3-4b24-b4a5-c72784c76b62">

*NOTA: Si a la primera vez que intentan abrir el proyecto desde Cypress este queda en una pantalla de loading que no avanza, por favor cerrar la venta del programa y volver a ejecutar y seleccionar la carpeta del proyecto de nuevo hasta que se cargue completamente.*

### 4.2.4 Seleccionar la prueba E2E
Las pruebas de reconocimiento que se harán son del tipo E2E (Extremo a Extremo), Por ende procedemos a escoger el cuadro de texto que dice **E2E Testing**

<img width="1206" alt="Screenshot 2024-05-04 at 11 56 16 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/39b0713a-6567-4378-909c-085e189ae525">

### 4.2.5 Iniciar la prubeba E2E
Cypress nos abrirá una ventana donde selecciona por defecto el Navegador Chrome o Firefox, con un botón en color verde, el cual debemos presionar y que dice **Start E2E Testing in < Navegador >**,

<img width="1207" alt="Screenshot 2024-05-04 at 11 56 53 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/e7f2f7ff-3645-4b5b-92b3-a0b9e939bb24">

### 4.2.6 Ejecutar la prueba
Una vez presionado el botón de la sección anterior, se abrirá una ventana del navegador en donde aparece el proyecto mostrando el árbol de archivos de la carpeta **e2e**, debe lucir así:
```
🗂️ cypress/e2e
    🗂️ ALL_TESTS
      📄 allTests.cy.js
    🗂️ FU001
      📄 FU001.cy.js
    🗂️ FU002
      📄 FU002.cy.js
```
<img width="1728" alt="Screenshot 2024-05-05 at 11 40 38 AM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/efa22f41-32b4-4a71-807e-76fdffc9588a">

El archivo `allTests.cy.js` contiene todos los escenarios juntos para que se ejecuten uno tras del otro. Sin embargo si le damos click al archivo de una funcionalidad en específico, esta correrá todos los escenarios que tiene adentro.

<img width="1728" alt="Screenshot 2024-05-05 at 12 28 07 PM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/3c4807c3-1590-47c5-9549-976f9f0c3af0">


Los screenshots tomados durante la ejecución de las pruebas pueden ser encontrados en carpeta **screenshots**. La estructura de esta carpeta por dentro debe lucir de la siguiente manera.

<img width="236" alt="Screenshot 2024-05-11 at 9 17 12 PM" src="https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/157188921/19bbb9f0-31b0-4723-878d-9c4ac36574f7">

Como se puede apreciar, dentro de la ruta `Ghost_5/cypress/cypress/screenshots` se encuentran las carpetas que contienen los screenshots por cada escenario.

Los screenshots de estas carpetas llevan los nombres de los pasos realizados en Cypress para ser comparados posteriormente contra los screenshots de la versión 3.42.0 de Ghost con las herramientas de ResembleJS y BackstopJS

## 4.3 Ejecución pruebas E2E en Ghost 3.42.0
Ahora para ejecutar los nuevos escenarios construidos en Cypress para Ghost 3.42.0, se deben repetir los pasos descrito durante toda la sección 4, exceptuando la parte de la instalación global de Cypress de nuevo, ya que ya se tiene instalada por defecto. 

La estructura de carpetas es completamente la misma, salvaguardando que se debe modificar los comandos de ruta de carpeta por Ghost_v3, ejemplo:

Ubicandonos sobre la carpeta raíz del proyecto, **MISW4103-pruebas-automatizadas**, realizamos los siguientes comandos

```bash
> cd Ghost_v3
> cd cypress
```

Y repetimos los pasos ya mencionados anteriormente. 


# 5. Funcionalidades y escenarios extra

Para esta entrega se agregaron dos nuevas funcionalidades extra a las 20 ya existentes de entregas pasadas. Esto se hizo con el fin de poder cumplir el criterio de acpetación de la rúbrica de esta semana, de encontrar funcionalidades que estuvieran en ambas versiones de Ghost. Las funcionalidades extra se detallan a continuación.

Código | Funcionalidad | # Escenarios
-- | -- | --
FU021 | Crear etiquetas (tags) | 4
FU022 | Borrar todo el contenido | 2

*NOTA: En total se tendrían 22 funcionalidades y 26 escenarios en total para esta entrega en Ghost v5, pero unicamente se escogieron 5 funcionalidades y 12 escenarios para ser replicados en Ghost v3*


# 6. Ejecución de las pruebas VRT
Una vez que ya se ejecutaron las pruebas End-2-End en Kraken y Cypress para Ghost v5.14.1 y Cypress para Ghost v3.42.0, se procede a realizar las pruebas VRT con las herramientas **ResembleJS** y **BackstopJS**, basandonos en los insumos (screenshots de cypress para cada escenario de las dos versiones de Ghost) proveidos de las secciones anteriores.

## 6.1 Herramienta BackstopJS

<p align="center">
<img src="https://www.drupal.org/files/project-images/backstop-lemur.png" alt="Backstop logo"  height="193">
<p align="center">BackstopJS</p>

### 6.1.1 Estructura de carpetas Backstopjs

La estructura de Backstopjs se ve de la siguiente manera
```
🗂️ BackStopJs
    📄 configBackstop.js
    🗂️ backstop_data
    🗂️ scripts
      📄 file.js
```

### 6.1.2 Ejecución Backstopjs

Para realizar las pruebas con Backstopjs es necesario ubicarnos en la carpeta BackStopJs ubicada en la raiz del proyecto

```bash
> cd BackStopJs
```

Si no tenemos instalada la herramienta Backstopjs, es necesario ejecutar el siguiente comando

```bash
> npm install -g backstopjs
```

Ya con la herramienta instalada solo tendremos que ejecutar el siguiente comando

```bash
> backstop test --config="configBackstop.js"
```

Luego de lanzar el comando apareceran los logs de la ejecución, similar a la siguiente imagen

![image](https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/42383191/d946117c-b18b-4341-a49f-3ea72289f16f)

Al finalizar la ejecución se abrira el reporte sobre el navegador teniendo una salida similar a la siguiente

![image](https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/assets/42383191/482c3c7b-3ad7-45d2-bc19-cd2feff366e6)


### 6.1.3 Funcionamiento de Backstopjs

La herramienta funciona por escenarios de prueba en donde se toma una imagen de referencia y otra a comparar, para esto se dispone el script configBackstop.js capaz de construir dinamicamente los escenarios según la estructura actual de carpetas de Ghost_v3/screenshots y Ghost_v5/screenshots.
El script revisa los screenshots almacenados por cada escenario revisando que exista el mismo en la versión 3 y 5 de Ghost, luego de esto ignora aquellas que ya se han construido previamente eliminando pantallas repetidas para comparar. 


## 6.2 Herramienta ResembleJS

<p align="center">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSUWZS1Cpv4rCriMHPEa3FeTo5KEbf7RYI6LbfC56ABJA&s" alt="Resemble logo"  height="193">
<p align="center">ResembleJS</p>




# 7. Documentación extra
Puede ver las funcionalidades escogidas, la comparativa de pros y contras de ambas herrameintas y una comparativa final de las dos en la [Wiki](https://github.com/CamilaMartinez-MISO/MISW4103-pruebas-automatizadas/wiki) del proyecto 

