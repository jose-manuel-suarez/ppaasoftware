# Práctica 1 - Guía de desarrollo

### Creación del proyecto

&emsp;&emsp;
Entre los prerequisitos para este punto se encuentran:

1. Tener instalado Java JDK 17.
    1. Luego de completar la instalación ejecutamos: `java --version` para confirmar.
2. Tener instalado un entorno de desarrollo (IDE) de su preferencia (VsCode, Eclipse, IntellIJ, NetBeans, etc.).
3. Crear el repositorio asociado al proyecto en GitHub.
    1. Crear una cuenta si no tenemos una o sino directamente ingresar y crear un repositorio nuevo.
4. Tener creada una cuenta de PostMan, ya que lo usaremos como aplicativo para probar nuestra API.
5.

&emsp;
Crearemos nuestro proyecto utilizando el framewor SpringBoot, un subconjunto del framework Spring que permite una creación y configuración acelearada de proyectos web con Java. Para ello utilizaremos la herramienta [**Spring initializr**](https://start.spring.io/ "Herramienta de inicialización automática para proyectos SpringBoot"), que permite crear un proyecto Spring con una serie de dependencias básicas iniciales.
<br><br>&emsp;&emsp;
Utilizaremos las opciones:

1. Project -> **Gradle - Groovy**
2. Language -> **Java**
3. Spring Boot -> **3.2.3** (Versiones 3.x.x requieren Java JDK 17)
4. Project Metadata ->
    1. Group -> **ar.edu.unlp.ppaas**
    2. Artifact -> **ejercicio1**
    3. Name -> **ejercicio1** (autogenerado a partir de Artifact)
    4. Description -> **patrones de arquitecturas de software - ejercicio 1**
    5. Package Name -> **ar.edu.unlp.ppaas.ejercicio1** (autogenerado dinámicamente a partir de los campos previos _[Group + Artifact]_)
5. Packaging -> **Jar**
6. Java -> **17**

En la sección de la derecha (Dependencies) agregaremos:

1. **Spring Web**
    - Que nos permitirá crear aplicaciones Spring respetando el patrón MVC e incorpora el servidor de aplicaciones Apache Tomcat.
2. **Spring Data JPA**
    - Que nos permitirá utilizar Java Persistence API, además de Hibernate.
3. **H2 Database**
    - Que nos permitirá mantener y gestinoar una BBDD en memoria, configurable mediante nuestro browser.
4. **Spring Boot DevTools** (Opcional)
    - Que nos permitirá el reinicio rápido de nuestra aplicación, recarga en vivo luego de realizar cambios y una más amena experiencie de desarrollo.
5. **Lombok** (Opcional)
    - Que nos permitirá reducir código repetitivo en nuestro proyecto, manteniendo código mucho más liviano.

Finalmente seleccionamos **GENERATE**<br>
Esto nos descargará un archivo comprimido (\*.zip) con nuestro proyecto.
Procedemos a descomprimirlo, copiarlo en el directorio de nuestra preferencia y abrir el proyecto, utilizando nuestro IDE.
<br><br><br>

### Instalación de dependencias, compilación y ejecución del proyecto

&emsp;&emsp;
Ya que disponemos de la herramienta Gradle podemos ejecutar los comandos asociados.<br>&emsp;&emsp;
Asumimos que para la ejecución de todos los comandos, el directorio actual es la raíz del proyecto.

-   `./gradlew clean build` para limpiar y construir nuestro proyecto.<br>Como resultado se crea el subdirectorio /build en nuestro proyecto.<br><br>
-   `./gradlew bootRun` para ejecutar nuestro proyecto.<br>Como resultado la consola muestra las etapas de la ejecución y nuestro servidor es accesible en el puerto 8080 de nuestro servidor local. Mediante la url: _http://localhost:8080_
    <br><br><br>

### Configuración del proyecto

&emsp;
Utilizaremos como base de datos, el motor H2, que se caracteriza por su velocidad, es de código abierto basado en JDBC API, administrable mediante una consola basada en browser y puede utilizarce como BBDD embebida en memoria del servidor.

Configuración a la BBDD H2:

1. Incorporaremos el motor H2 mediante la dependencia asociada (grupo: com.h2database artefacto: h2 alcance: runtime).

    1. Esta dependencia fue incorporada como una dependencia de arranque indicada mediante el aplicativo initialzr.
    2. La url asociada para la administración es (por defecto): http://localhost:8080/h2-console
        1. Dicha url puede configurarse en el archivo application.properties mediante: **spring.h2.console.path=[nombre]**
           <br>Ejemplo: si queremos consultarla con la url: http://localhost.8080/consolaBBDD configuraríamos: `spring.h2.console.path=/consolaBBDD`

2. Todo proyecto Spring tiene un archivo de configuración **application.properties**, que contiene todas las configuraciones necesarias para su correcto funcionamiento. Éste archivo puede verse en el directorio **./src/main/resources/** del proyecto.
    - En nuestro caso para la configuración inicial de la BBDD H2, proporcionamos un [**application.properties**](./application.properties "Archivo de configuración del proyecto") como ejemplo.
3. Verifique que todos y cada uno de los parámetros configurados en el archivo, es decir, que cada parámetro coincida con los mostrados en la consola de configuración de H2 (específicamente: Driver Class, JDBC URL, User Name y Password).
   <figure>
    <img src="../Imagenes/conexion_h2.png" alt="Conexion a H2" style="width: 80%;">
    <figcaption>Coincidencia entre parametrización de configuración H2 y consola de pruebas.</figcaption>
</figure>
1. Verifique la conexión mediante **Test Connection**
2. Verifique el ingreso mediante **Connect**

<br><br><br>

### Arquitectura del proyecto

&emsp;
El proyecto se arquitectura en capas, bajo un esquema propuesto por Spring.

-   ### Capa de Modelo de Datos

-   ### Capa de Repositorios

-   ### Capa de Servicios

-   ### Capa de Controladores

<br><br><br>

### Dockerización del proyecto

<br><br><br>

### Fuentes Relacionadas

&emsp;[![Java17](https://img.shields.io/badge/Descargar_JDK_17-oracle.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.oracle.com/java/technologies/downloads/#java17)</br>
&emsp;[![SpringInitializr](https://img.shields.io/badge/Spring_Initializr-start.spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://start.spring.io/)</br>
&emsp;[![SpringBoot](https://img.shields.io/badge/Documentacion_Spring_Boot-spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://spring.io/projects/spring-boot)</br>
&emsp;[![H2DB](https://img.shields.io/badge/H2_Database_Engine-h2database.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.h2database.com/html/main.html)</br>
&emsp;[![H2DBSpring](https://img.shields.io/badge/Spring_Boot_with_H2_Data_Base-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-boot-h2-database)</br>
&emsp;[![SpringMVC](https://img.shields.io/badge/Spring_Web_MVC-docs.spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://docs.spring.io/spring-framework/docs/5.3.25/reference/html/web.html#mvc)</br>
&emsp;[![ServiciosREST](https://img.shields.io/badge/Construyendo_servicios_REST_con_Spring-spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://spring.io/guides/tutorials/rest)</br>
&emsp;[![JPAEntity](https://img.shields.io/badge/Entidades_JPA-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/jpa-entities)</br>
&emsp;[![PersistenceJPA](https://img.shields.io/badge/Capa_de_persistencia_JPA-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/the-persistence-layer-with-spring-data-jpa)</br>
&emsp;[![Reposiory](https://img.shields.io/badge/Repositorios_Spring-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-data-repositories)</br>
&emsp;[![Autowired](https://img.shields.io/badge/Spring_anotacion_@Autowired-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-autowire)</br>
&emsp;[![MapStruct](https://img.shields.io/badge/Libreria_Map_Struct-mapstruct.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://mapstruct.org/documentation/stable/reference/html/#basic-mappings)</br>
&emsp;[![AppRunner](https://img.shields.io/badge/Application_Runner-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/running-setup-logic-on-startup-in-spring#7-spring-boot-applicationrunner)</br>

<br><br><br>

### Contacto

Ante cualquier consulta, problema o sugerencia relativa a ésta práctica, no dudes en comunicarte con nosotros mediante los canales disponibles:

&emsp;[![Luisma-email](https://img.shields.io/badge/Luis_Mariano_Bibbo-0A84FF?logo=gmail)](mailto:lmbibbo@lifia.info.unlp.edu.ar.com)
&emsp;[![Jose-email](https://img.shields.io/badge/Jose_Manuel_Suarez-0A84FF?logo=Gmail)](mailto:jsuarez@lifia.info.unlp.edu.ar)
&emsp;[![Canal-Discord](https://img.shields.io/badge/Discord_PPAA_de_Software-%235865F2.svg?&logo=discord&logoColor=white)](https://www.discord.com/josemanuel.suarez.87)
