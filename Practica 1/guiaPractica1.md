# Práctica 1 - Guía de desarrollo

[**_Ver enunciado_**](/Practica%201/Practica%201.pdf "Practica1.pdf")


### Objetivo del proyecto

&emsp;&emsp;
Crear un servicio web muy sencillo que nos permita realizar algunas operaciones básicas (CRUD) sobre una entidad **Persona**.

### Creación del proyecto

&emsp;&emsp;
Entre los prerequisitos para este punto se encuentran:

1. Tener instalado Java JDK 17.
    - Luego de completar la instalación ejecutamos: `java --version` para confirmar.
2. Tener instalado un entorno de desarrollo (IDE) de su preferencia (VsCode, Eclipse, IntellIJ, NetBeans, etc.).
3. Crear el repositorio asociado al proyecto en GitHub.
    - Crear una cuenta si no tenemos una o sino directamente ingresar y crear un repositorio nuevo.
4. Tener creada una cuenta de PostMan, ya que lo usaremos como aplicativo para probar nuestra API.

<br>&emsp;&emsp;
Crearemos nuestro proyecto utilizando el framewor SpringBoot, un subconjunto del framework Spring que permite una creación y configuración acelearada de proyectos web con Java. Para ello utilizaremos la herramienta [**Spring initializr**](https://start.spring.io/ "Herramienta de inicialización automática para proyectos SpringBoot"), que permite crear un proyecto Spring con una serie de dependencias básicas iniciales.

Utilizaremos las opciones:

1. Project -> **Gradle - Groovy**
2. Language -> **Java**
3. Spring Boot -> **3.2.3** (Versiones 3.x.x requieren Java JDK 17)
4. Project Metadata
    - Group -> **ar.edu.unlp.ppaas**
    - Artifact -> **ejercicio1**
    - Name -> **ejercicio1** (autogenerado a partir de Artifact)
    - Description -> **patrones de arquitecturas de software - ejercicio 1**
    - Package Name -> **ar.edu.unlp.ppaas.ejercicio1** (autogenerado dinámicamente a partir de los campos previos _[Group + Artifact]_)
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

<br>

### Instalación de dependencias, compilación y ejecución del proyecto

&emsp;&emsp;
Ya que disponemos de la herramienta Gradle podemos ejecutar los comandos asociados.<br>&emsp;&emsp;
Asumimos que para la ejecución de todos los comandos, el directorio actual es la raíz del proyecto.

-   `./gradlew clean build` para limpiar y construir nuestro proyecto.<br>Como resultado se crea el subdirectorio /build en nuestro proyecto.<br><br>
-   `./gradlew bootRun` para ejecutar nuestro proyecto.<br>Como resultado la consola muestra las etapas de la ejecución y nuestro servidor es accesible en el puerto 8080 de nuestro servidor local. Mediante la url: [**_https://localhost:8080_**](https://localhost:8080/)

<br>

### Configuración del proyecto

&emsp;
Utilizaremos como base de datos, el motor H2, que se caracteriza por su velocidad, es de código abierto basado en JDBC API, administrable mediante una consola basada en browser y puede utilizarce como BBDD embebida en memoria del servidor.

Configuración a la BBDD H2:

1. Incorporaremos el motor H2 mediante la dependencia asociada (grupo: com.h2database artefacto: h2 alcance: runtime).

    1. Esta dependencia fue incorporada como una dependencia de arranque indicada mediante el aplicativo initialzr.
    2. La url asociada para la administración es (por defecto): http://localhost:8080/h2-console
        1. Dicha url puede configurarse en el archivo application.properties mediante: **spring.h2.console.path=[/ruta]**
           <br>Ejemplo: si queremos consultarla con la url: **_http://localhost.8080/consolaBBDD_** configuraríamos: `spring.h2.console.path=/consolaBBDD`

2. Todo proyecto Spring tiene un archivo de configuración **application.properties**, que contiene todas las configuraciones necesarias para su correcto funcionamiento. Éste archivo puede verse en el directorio **./src/main/resources/** del proyecto.
    - En nuestro caso para la configuración inicial de la BBDD H2, proporcionamos un [**application.properties**](./application.properties "Archivo de configuración del proyecto") como ejemplo.
3. Verifique que todos y cada uno de los parámetros configurados en el archivo, es decir, que cada parámetro coincida con los mostrados en la consola de configuración de H2 (específicamente: Driver Class, JDBC URL, User Name y Password).
 <figure>
 <img src="../Imagenes/conexion_h2.png" alt="Conexion a H2" style="width: 70%;">
 <figcaption>Coincidencia entre parametrización de configuración H2 y consola de pruebas.</figcaption>
 </figure>

-   Verifique la conexión mediante el botón **Test Connection**
-   Verifique el ingreso mediante el botón **Connect**

<br>

### Arquitectura del proyecto

&emsp;
El proyecto se estructura siguiendo la arquitectura de capas, bajo un patrón Modelo Vista Controlador (MVC) propuesto por Spring.

 <figure>
 <img src="../Imagenes/springboot_arquitectura.png" alt="Arquitectura del proyecto" style="width: 50%;">
 <figcaption>Lineamiento general de la arquitectura propuesta por Spring.</figcaption>
 </figure>

-   ### Capa de BBDD

    Esta capa contiene todas las bases de datos de la aplicación, en nuestro caso sólo la BBDD H2 persistida en la memoria del servidor (un Apache Tomcat corriendo en el puerto 8080).<br>
    No tiene específicamente un subdirectorio asociado en el proyecto ni una anotación que la represente porque está manejada por el DBMS, Spring Data Jpa y el ORM Hibernate.

-   ### Capa de Modelo de Datos

    Esta capa de la aplicación es la que se encarga de los artefactos, conocidos aquí como **Entidades**, más cercanos a la BBDD. Aquí se define la estructura de cada entidad del modelo de datos, representada por sus atributos y las anotaciones que los etiquetan.<br>
    Representada por el subdirectorio: `./model`<br>
    Anotación asociada: `@Entity`

-   ### Capa de Repositorios

    Esta capa de la aplicación es la que se encarga de reutilizar la funcionalidad ya provista por el framework Spring, en cuanto a las operaciones básicas sobre nuestra BBDD, es decir, las relativas a las operaciones CRUD (Creacion, Lectura, Actualización y Eliminación). Esto es posible mediante herencia, ya que cada repositorio de nuestra aplicación heredará de la clase **JpaRepository**.<br>
    Representada por el subdirectorio: `./repository`<br>
    Anotación asociada: `@Repository`

-   ### Capa de Servicios

    Esta capa de la aplicación contiene todo lo relativo a la lógica del negocio. Cada clase de servicio suele inyectar la dependencia al repositorio asociado que le permitirá la invocación de toda la lógica relativa a los CRUDs. Las operaciones que cada servicio añade suelen estar delcaradas en una interface asociada, la cuál el servicio implementa.<br>
    Representada por el subdirectorio: `./service`<br>
    Anotación asociada: `@Service`

-   ### Capa de Controladores

    Esta capa de la aplicación contiene todo lo relativo a los puntos o métodos de invocación (endpoints) que serán ejecutables desde un browser o cualquier cliente web o servicio externo.<br>
    Representada por el subdirectorio: `./controller`<br>
    Anotación asociada: `@RestController`

<br>

### Dockerización del proyecto

&emsp;
Docker es una herramienta ... su proposito es ...

En el directorio raíz de nuestro proyecto debemos crear un archivo de configuración para Docker. Aquí puedes visalizar un ejemplo de dicho archivo para nuestro proyecto, asegúrate que su nombre sea [**dockerfile**](/Practica%201/dockerfile) y que el directorio sea el correcto.<br>

&emsp;Tips del archivo **dockerfile**:
- nombre: **dockerfile** o **Dockerfile** sin extensión alguna.
- ubicación: al mismo nivel que la carpeta **/src** y **/build**

&emsp;Comandos:

-   **Construcción** de la imagen
    -   Formato genérico: `docker build -t org/aplicacion:version .`<br>
    -   En nuestra aplicación: `docker build -t pas/ejercicio1:1.0.0 .`<br>
    -   Resultado: una nueva imagen del proyecto generada con Docker<br>
        -   Visualizable mediante el comando: `docker list`<br><br>
-   **Ejecución** del contenedor: `docker run -p 8080:8080 pas/ejercicio1:1.0.0`
    -   Luego de su ejecución, si todo sale bien, la aplicación correrá en el puerto 8080, y será accesible mediante la url: [**_https://localhost:8080_**](https://localhost:8080/)

<br>

### Fuentes Relacionadas

&emsp;[![Java17](https://img.shields.io/badge/Descargar_JDK_17-oracle.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.oracle.com/java/technologies/downloads/#java17)</br>
&emsp;[![SpringInitializr](https://img.shields.io/badge/Spring_Initializr-start.spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://start.spring.io/)</br>
&emsp;[![SpringBoot](https://img.shields.io/badge/Documentacion_Spring_Boot-spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://spring.io/projects/spring-boot)</br>
&emsp;[![H2DB](https://img.shields.io/badge/H2_Database_Engine-h2database.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.h2database.com/html/main.html)</br>
&emsp;[![H2DBSpring](https://img.shields.io/badge/Spring_Boot_with_H2_Data_Base-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-boot-h2-database)</br>
&emsp;[![ArqSpring](https://img.shields.io/badge/SpringBoot_Architecture-geeksforgeeks.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.geeksforgeeks.org/spring-boot-architecture/)</br>
&emsp;[![SpringMVC](https://img.shields.io/badge/Spring_Web_MVC-docs.spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://docs.spring.io/spring-framework/docs/5.3.25/reference/html/web.html#mvc)</br>
&emsp;[![ServiciosREST](https://img.shields.io/badge/Construyendo_servicios_REST_con_Spring-spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://spring.io/guides/tutorials/rest)</br>
&emsp;[![JPAEntity](https://img.shields.io/badge/Entidades_JPA-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/jpa-entities)</br>
&emsp;[![PersistenceJPA](https://img.shields.io/badge/Capa_de_persistencia_JPA-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/the-persistence-layer-with-spring-data-jpa)</br>
&emsp;[![Reposiory](https://img.shields.io/badge/Repositorios_Spring-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-data-repositories)</br>
&emsp;[![Autowired](https://img.shields.io/badge/Spring_anotacion_@Autowired-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-autowire)</br>
&emsp;[![MapStruct](https://img.shields.io/badge/Libreria_Map_Struct-mapstruct.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://mapstruct.org/documentation/stable/reference/html/#basic-mappings)</br>
&emsp;[![AppRunner](https://img.shields.io/badge/Application_Runner-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/running-setup-logic-on-startup-in-spring#7-spring-boot-applicationrunner)</br>

<br>

### Contacto

Ante cualquier consulta, problema o sugerencia relativa a ésta práctica, no dudes en comunicarte con nosotros mediante los canales disponibles:

&emsp;[![Luisma-email](https://img.shields.io/badge/Luis_Mariano_Bibbo-0A84FF?logo=gmail)](mailto:lmbibbo@lifia.info.unlp.edu.ar.com)
&emsp;[![Jose-email](https://img.shields.io/badge/Jose_Manuel_Suarez-0A84FF?logo=Gmail)](mailto:jsuarez@lifia.info.unlp.edu.ar)
&emsp;[![Canal-Discord](https://img.shields.io/badge/Discord_PPAA_de_Software-%235865F2.svg?&logo=discord&logoColor=white)](https://www.discord.com/josemanuel.suarez.87)
