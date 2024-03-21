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
**Spring Initializr**<br>
&emsp;&emsp;
Crearemos nuestro proyecto utilizando el framewor SpringBoot, un subconjunto del framework Spring que permite una creación y configuración acelearada de proyectos web con Java. Para ello utilizaremos la herramienta [**Spring initializr**](https://start.spring.io/ "Herramienta de inicialización automática para proyectos SpringBoot"), que permite crear un proyecto Spring con una serie de dependencias básicas iniciales.

&emsp;&emsp;
Utilizaremos las opciones:

-   Project -> **Gradle - Groovy**
-   Language -> **Java**
-   Spring Boot -> **3.2.3** (Versiones 3.x.x requieren Java JDK 17)
-   Project Metadata
    -   Group -> **ar.edu.unlp.ppaas**
    -   Artifact -> **ejercicio1**
    -   Name -> **ejercicio1** (autogenerado a partir de Artifact)
    -   Description -> **patrones de arquitecturas de software - ejercicio 1**
    -   Package Name -> **ar.edu.unlp.ppaas.ejercicio1** (autogenerado dinámicamente a partir de los campos previos _[Group + Artifact]_)
-   Packaging -> **Jar**
-   Java -> **17**

&emsp;&emsp;
En la sección de la derecha (Dependencies) agregaremos:

-   **Spring Web**
    -   Que nos permitirá crear aplicaciones Spring respetando el patrón MVC e incorpora el servidor de aplicaciones Apache Tomcat.
-   **Spring Data JPA**
    -   Que nos permitirá utilizar Java Persistence API, además de Hibernate.
-   **H2 Database**
    -   Que nos permitirá mantener y gestinoar una BBDD en memoria, configurable mediante nuestro browser.
-   **Spring Boot DevTools** (Opcional)
    -   Que nos permitirá el reinicio rápido de nuestra aplicación, recarga en vivo luego de realizar cambios y una más amena experiencie de desarrollo.
-   **Lombok** (Opcional)
    -   Que nos permitirá reducir código repetitivo en nuestro proyecto, manteniendo código mucho más liviano.

Finalmente generamos el proyecto Spring, seleccionando: &emsp;
<button style="background-color: rgb(109, 179, 10); color: black; letter-spacing: 1px;">**GENERATE**</button><br>
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

-   ### Capa de datos (BBDD)

    Esta capa contiene todas las bases de datos de la aplicación, en nuestro caso sólo la BBDD H2 persistida en la memoria del servidor (un Apache Tomcat corriendo en el puerto 8080).<br>
    No tiene específicamente un subdirectorio asociado en el proyecto ni una anotación que la represente porque está manejada por el DBMS, Spring Data Jpa y el ORM Hibernate.

-   ### Capa de Modelo de Datos

    Esta capa de la aplicación es la que se encarga de los artefactos, conocidos aquí como **Entidades**, más cercanos a la BBDD. Aquí se define la estructura de cada entidad del modelo de datos, representada por sus atributos y las anotaciones que los etiquetan.<br>
    &emsp;Representada por el subdirectorio: `./model`<br>
    &emsp;Anotación asociada: `@Entity`

-   ### Capa de Repositorios

    Esta capa de la aplicación es la que se encarga de reutilizar la funcionalidad ya provista por el framework Spring, en cuanto a las operaciones básicas sobre nuestra BBDD, es decir, las relativas a las operaciones CRUD (Creacion, Lectura, Actualización y Eliminación). Esto es posible mediante herencia, ya que cada repositorio de nuestra aplicación heredará de la clase **JpaRepository**.<br>
    &emsp;Representada por el subdirectorio: `./repository`<br>
    &emsp;Anotación asociada: `@Repository`

-   ### Capa de Servicios

    Esta capa de la aplicación contiene todo lo relativo a la lógica del negocio. Cada clase de servicio suele inyectar la dependencia al repositorio asociado que le permitirá la invocación de toda la lógica relativa a los CRUDs. Las operaciones que cada servicio añade suelen estar delcaradas en una interface asociada, la cuál el servicio implementa.<br>
    &emsp;Representada por el subdirectorio: `./service`<br>
    &emsp;Anotación asociada: `@Service`

-   ### Capa de Controladores

    Esta capa de la aplicación contiene todo lo relativo a los puntos o métodos de invocación (endpoints) que serán ejecutables desde un browser o cualquier cliente web o servicio externo.<br>
    &emsp;Representada por el subdirectorio: `./controller`<br>
    &emsp;Anotación asociada: `@RestController`

<br>

### Agregando la entidad 'Persona' a nuestro modelo de datos

-   Cree un subdirectorio **_/model_**, éste contedrá todas las clases asociadas a nuestro modelo.
    -   Notación recomendada para cada clase definida en este paquete: _nombreEntidad**Model**.java_, siendo "nombre" el nombre de la clase. Por ejemplo, para la entidad **Persona** definiríamos: . _/model/**PersonaModel.java**_
-   Crear la entidad del modelo de datos **Persona**, definiendo una clase que contenga, al menos, los siguientes atributos:
    -   Id
    -   Apellidos y nombres
    -   Fecha de nacimiento
    -   Email
-   Crear para cada atributo definido, los getters y setters.
-   Crear al menos 2 constructores asociadosm (sin atributos parametrizados y con todos los atributos parametrizados).
-   Es posible utilizar **_Lombok_**, mediante anotaciones para tal fin (investigar: _@NoArgsConstructor_ _@AllArgsConstructor_
    _@Getter_ _@Setter_).
-   Crear una clase <a id="dto_persona">**DTO** para la entidad **Persona**</a> que contenga los siguientes atributos:

    -   Id
    -   Nombre completo -> que surge de la concatenación de apellidos y nombres, con formato: [apellidos, nombres].
    -   Edad -> calculada a partir de los años transcurridos desde la fecha de nacimiento.
    -   Email

    &emsp;&emsp; **_Tip_**: tenga en cuenta que este objeto, por definición, no debería contener nada relativo a la lógica del negocio sino simplemente funcionalidad asociada al almacenamiento y recuperación de datos.

<br>

### Agregando el repositorio 'Persona'

-   Defina un <a id="repositorio">repositorio JPA para las personas</a>, mediante la creación de una interface que extienda de alguna de las provistas por el framework. De este modo nuestras clases utilizarán el rpositorio mediante la implementación de esta interface que contendrá implementaciones por defecto para los CRUDs.
    -   En nuestro caso extenderemos desde la clase **JpaRepository<T, id>**
    -   Existen varios tipos de repositorios más en la jerarquía, por ejemplo **PagingAndSortingRepository**, que cómo su nombre indica permite implementar queries a la BD considerando paginación y ordenamiento.

<br>

### Agregando el los servicios asociados a la 'Persona'

-   Defina un servicio encargado de transformar (mapping) una entidad de la BBDD (DAO) en el [DTO que definimos previamente](#dto_persona).

    -   Es posible hacerlo manualmente o recurrir a una librería para tal fin, como por ejemplo: **MapStruct**.<br>

-   Defina otro <a id="servicio">servicio</a> que contendrá las operaciones exportadas por el servicio web.
    -   Dar de alta una nueva persona en el sistema (**CREATE**)
    -   Obtener el listado de todas las personas del sistema (**READ** (N))
    -   Obtener una persona determinada dentro del sistema (**READ** (1))
    -   Actualizar una persona del sistema (**UPDATE**)
    -   Eliminar una persona del sistema (**DELETE**)<br>

&emsp;&emsp; **_Tips_**: <br>&emsp;&emsp;&emsp;&emsp; - Recuerde etiquetar ambas clases con la anotación **@Service** para registrarlas en Spring.<br>&emsp;&emsp;&emsp;&emsp; - Las operaciones que cada servicio debe implementar deberían estar definidas en una interface asociada.<br>&emsp;&emsp;&emsp;&emsp; - Recordar usar la etiqueta **@Autowired** en estos servicios, para inyectar las dependencias necesarias. Por ejemplo, el servicio que define los CRUDs, requerirá la inyección del [repositorio creado](#repositorio).<br>

<br>

### Creando el controlador asociado a la 'Persona'

-   Crear un controlador donde se definan los puntos de acceso (_endpoints_) para las operaciones que exportará el srvicio.

&emsp;&emsp; **_Tips_**: <br>&emsp;&emsp;&emsp;&emsp; - Recuerde etiquetar ambas clases con la anotación **@RestController** para registrarlas en Spring.<br>&emsp;&emsp;&emsp;&emsp; - Recordar usar la etiqueta **@Autowired** cuando lo requiera, para inyectar las dependencias necesarias. Por ejemplo, el controlador requerirá la inyección del [servicio creado](#servicio) para implementar cada funcionalidad expuesta por un punto de acceso.<br>&emsp;&emsp;&emsp;&emsp; - Considerar criterios REST (rutas, verbos HTTP, etc.).<br>

<br>

### Inicializando los datos asociados a las entidades 'Persona'

Dado que nuestra BBDD gestionada por H2 es mantenida en memoria del servidor, la misma es recreada cada vez que compilamos y ejecutamos nuestro servicio web. Por consiguiente, si deseamos visualizar y trabajar con datos para pruebas necesitaríamos incorporárselos previamente.

-   Crear un inicializador que cargue algunos datos de prueba (con 3 o 4 personas será suficiente). Esto se logra definiendo un servicio que implemente la interfaz **ApplicationRunner**. Allí, se inyecta el repositorio y se lo usa para dar de alta Personas en el sistema.

<br>

### Dockerización del proyecto

&emsp;
Docker es una herramienta ... su proposito es ...

En el directorio raíz de nuestro proyecto debemos crear un archivo de configuración para Docker. Aquí puedes visalizar un ejemplo de dicho archivo para nuestro proyecto, asegúrate que su nombre sea [**dockerfile**](/Practica%201/dockerfile) y que el directorio sea el correcto.<br>

&emsp;Tips del archivo **dockerfile**:

-   nombre: **dockerfile** o **Dockerfile** sin extensión alguna.
-   ubicación: al mismo nivel que la carpeta **/src** y **/build**

&emsp;Comandos:

-   **Construcción** de la imagen
    -   Formato genérico: `docker build -t org/aplicacion:version .`<br>
    -   En nuestra aplicación: `docker build -t pas/ejercicio1:1.0.0 .`<br>
    -   Resultado: una nueva imagen del proyecto generada con Docker<br>
        -   Visualizable mediante el comando: `docker list`<br><br>
-   **Ejecución** del contenedor: `docker run -p 8080:8080 pas/ejercicio1:1.0.0`
    -   Luego de su ejecución, si todo sale bien, la aplicación correrá en el puerto 8080, y será accesible mediante la url: [**_https://localhost:8080_**](https://localhost:8080/)

<br>

### Repositorios colaborativos

Para la administración de nuestro código, que nos permita trabajar de forma distribuída y colaborativa, utilizaremos **GitHub** o **GitLab**, dos ultra conocidos clientes de **Git**.

-   Registrarse como usuario (si no lo estás aún).<br>
-   Crear el repositorio asociado al proyecto. <br>
-   Añadir colaboradores al proyecto (miembros de tu grupo). <br>
-   Docker
    -   (GitLab) En la sección **Packages and registries > Container registry**. Puede accederse al registro de imágenes docker del proyecto. El mimso contiene las instrucciones para loguearse y subir una imagen Docker al mismo.<br>
    -   (GitHub) ...<br>

<br>

### Fuentes Relacionadas

&emsp;[![Java17](https://img.shields.io/badge/Descargar_JDK_17-oracle.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.oracle.com/java/technologies/downloads/#java17)</br>
&emsp;[![SpringInitializr](https://img.shields.io/badge/Spring_Initializr-start.spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://start.spring.io/)</br>
&emsp;[![SpringBoot](https://img.shields.io/badge/Documentacion_Spring_Boot-spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://spring.io/projects/spring-boot)
&emsp;[![ArqSpring](https://img.shields.io/badge/SpringBoot_Architecture-geeksforgeeks.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.geeksforgeeks.org/spring-boot-architecture/)
&emsp;[![SpringMVC](https://img.shields.io/badge/Spring_Web_MVC-docs.spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://docs.spring.io/spring-framework/docs/5.3.25/reference/html/web.html#mvc)</br>
&emsp;[![H2DB](https://img.shields.io/badge/H2_Database_Engine-h2database.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.h2database.com/html/main.html)
&emsp;[![H2DBSpring](https://img.shields.io/badge/Spring_Boot_with_H2_Data_Base-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-boot-h2-database)</br>
&emsp;[![Lombok](https://img.shields.io/badge/Lombok-projectlombok.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://projectlombok.org/)</br>
&emsp;[![DTO](https://img.shields.io/badge/DTO-wikipedia.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://en.wikipedia.org/wiki/Data_transfer_object)</br>
&emsp;[![ServiciosREST](https://img.shields.io/badge/Construyendo_servicios_REST_con_Spring-spring.io-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://spring.io/guides/tutorials/rest)</br>
&emsp;[![JPAEntity](https://img.shields.io/badge/Entidades_JPA-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/jpa-entities)
&emsp;[![PersistenceJPA](https://img.shields.io/badge/Capa_de_persistencia_JPA-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/the-persistence-layer-with-spring-data-jpa)</br>
&emsp;[![Reposiory](https://img.shields.io/badge/Repositorios_Spring-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-data-repositories)</br>
&emsp;[![SpringAnnotations](https://img.shields.io/badge/Spring_@Annotacions-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-component-repository-service)
&emsp;[![Autowired](https://img.shields.io/badge/Spring_anotacion_@Autowired-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-autowire)</br>
&emsp;[![MapStruct](https://img.shields.io/badge/Libreria_Map_Struct-mapstruct.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://mapstruct.org/documentation/stable/reference/html/#basic-mappings)</br>
&emsp;[![AppRunner](https://img.shields.io/badge/Application_Runner-baeldung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/running-setup-logic-on-startup-in-spring#7-spring-boot-applicationrunner)</br>
&emsp;[![GitHubDocs](https://img.shields.io/badge/GitHub_Docs-docs.github.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://docs.github.com/es)
&emsp;[![GitLabDocs](https://img.shields.io/badge/GitLab_Docs-docs.gitlab.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://docs.gitlab.com/)</br>
&emsp;[![CI/CD](https://img.shields.io/badge/CI/CD_con_GitLab-medium.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://alexmarket.medium.com/introducci%C3%B3n-a-ci-cd-con-gitlab-23d4e9cc6482)</br>
<br>

### Contacto

&emsp;[![LMB-email](https://img.shields.io/badge/Luis_Mariano_Bibbo-0A84FF?logo=gmail)](mailto:lmbibbo@lifia.info.unlp.edu.ar)
&emsp;[![JMS-email](https://img.shields.io/badge/Jose_Manuel_Suarez-0A84FF?logo=Gmail)](mailto:jsuarez@lifia.info.unlp.edu.ar)
&emsp;[![Canal-Discord](https://img.shields.io/badge/PPAA_de_Software-%235865F2.svg?&logo=discord&logoColor=white)](https://discord.gg/XAdRPKVZCT)
