Para obtener el WSDL de SalesForce
=============
Desde la pantalla principal ir a:

MShops Principal -> Configuracion -> Desarrollo -> API

Seleccionar Generar WSDL de empresa  y poner generate. 
Descargar el XML que genera.

Nota: Hacer esto tanto para Producción como para el Sandbox.

Los nombres que utilizo son salesForce.xml para produccion y salesForceTest.xml para el sandbox.

Comando para crear el Jar en base al WSDL
=============

Para esto es necesario bajarse el codigo y seguir el siguiente toturial: https://github.com/forcedotcom/wsc

Script ya configurados:

Prod
java -classpath target/force-wsc-28.0.0-jar-with-dependencies.jar com.sforce.ws.tools.wsdlc ruta_donde_lo_guardamos/salesForce.xml ./salesForceMshops.jar

Test
java -classpath target/force-wsc-28.0.0-jar-with-dependencies.jar com.sforce.ws.tools.wsdlc ruta_donde_lo_guardamos/salesForceTest.xml ./salesForceMshopsTest.jar


Comando para subir el jar a nexus
=============

Ejecutar desde donde se encuentran los jars creados anteriormente lo siguiente:

Prod
mvn deploy:deploy-file -DgroupId=com.mshops.api -DartifactId=mshops-salesforce -Dversion=1.0.2 -Dpackaging=jar -Dfile=/home/dballerini/forcedotcom/wsc/salesForceMshops.jar  -DrepositoryId=MLGrailsPlugins -Durl=http://git.ml.com:8081/nexus/content/repositories/MLGrailsPlugins

Test
mvn deploy:deploy-file -DgroupId=com.mshops.api -DartifactId=mshops-salesforce-test -Dversion=1.0.2 -Dpackaging=jar -Dfile=/home/dballerini/forcedotcom/wsc/salesForceMshopsTest.jar  -DrepositoryId=MLGrailsPlugins -Durl=http://git.ml.com:8081/nexus/content/repositories/MLGrailsPlugins

Cambiar la version en el pom por la nueva version que se haya subido al repositorio de maven.

Existen dos perfiles definidos en maven, estos son: prod, test (por default esta el de prod).

Para poder configurar el proyecto con el perfil de test, se debe agregar el parametro -Ptest

por ejemplo:

mvn eclipse:eclipse -Ptest


