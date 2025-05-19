# Notas de demostración

Notas, consejos y sugerencias para utilizar los distintos tipos de escaneo Veracode con esta aplicación.

Consulte también la carpeta `docs/flaws` para obtener explicaciones detalladas de los diversos exploits expuestos en esta aplicación.

## Escaneo estático

Construye la aplicación:

aplicación cd
paquete limpio mvn

El archivo `app/target/verademo.war` es la aplicación creada.  Este es el archivo que se utilizará para el escaneo.  Cargue este archivo en la plataforma Veracode para un escaneo de Política/Sandbox o úselo con el escaneo de Veracode Pipeline.

## Escaneo SCA

### Cargar/Escanear

Esto simplemente sucederá como parte del escaneo de Política o Sandbox.

### Escaneo basado en agentes

Utilice la versión de línea de comandos del agente SCA (siga las instrucciones de instalación y configuración en Veracode [docs center] (https://docs.veracode.com/r/c_sc_what_is) ) o el complemento IDE para iniciar un escaneo SCA.

### Métodos vulnerables

El escaneo SCA basado en agentes Veracode también puede encontrar [métodos vulnerables] (https://docs.veracode.com/r/Finding_and_Fixing_Vulnerabilities#fixing-vulnerable-methods). En esta aplicación, hay una versión vulnerable de la biblioteca bcrypt llamada desde las clases `User` y `UserController”.

### Generación SBOM

La generación de SBOM para la aplicación es compatible después de un escaneo SCA ([enlace](https://docs.veracode.com/r/Generating_a_Software_Bill_of_Materials_SBOM_for_Upload_Scans)) 

La generación de SBOM para el contenedor Docker es compatible con el escáner Container/CLI ([enlace](https://docs.veracode.com/r/veracode_sbom)).

## Corrección de Veracode

Esta aplicación tiene fallas que se pueden solucionar con [Veracode Fix](https://docs.veracode.com/r/veracode_fix). Para ver un ejemplo de una:

### Construye la aplicación

aplicación cd
paquete limpio mvn

### Ejecute el escáner Veracode Pipeline

java -jar ${ruta-a-pipeline-scanner}/pipeline-scan.jar -f target/verademo.war -esd true 

### Ejecute Veracode Fix

veracode fix src/main/java/com/veracode/verademo/controller/UserController.java

El primer defecto es una inyección SQL alrededor de la línea 170 que se puede solucionar.

Para verificar la solución, reconstruya la aplicación y vuelva a ejecutar el escáner Pipeline. 

## Escaneo de contenedores

Desde la raíz del proyecto ejecute el escáner de contenedores Veracode:

veracode escaneo --tipo directorio --fuente. --salida contenedor_resultados.json