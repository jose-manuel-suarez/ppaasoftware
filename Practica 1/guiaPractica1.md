# Guía de desarrollo para la práctica 1

### Ejercicio 1 - Creación de un proyecto backend.

&emsp;
Entre los prerequisitos para este punto se encuentran:

1. Tener instalado Java JDK 17.
2. Crear el repositorio asociado al proyecto en GitHub.
    1. Crear una cuenta si no tenemos una o sino directamente ingresar y crear un repositorio nuevo.
3. Tener creada una cuenta de PostMan, ya que lo usaremos como aplicativo para probar nuestra API.
4.

&emsp;
Utilizaremos como base de datos, el motor H2, que se caracteriza por su velocidad, es de código abierto basado en JDBC API, administrable mediante una consola basada en browser y puede utilizarce como BBDD embebida en memoria del servidor.

Configuración a la BBDD H2:

1. Incorporaremos el motor H2 mediante la dependencia asociada (grupo: com.h2database artefacto: h2 alcance: runtime).
    1. Esta dependencia fue incorporada como una dependencia de arranque indicada mediante el aplicativo initialzr.
    2. La url asociada para la administración es (por defecto): http://localhost:8080/h2-console
        1. Dicha url puede configurarse en el archivo application.properties mediante: **spring.h2.console.path=[nombre]**
		<br>Ejemplo: si queremos consultarla con la url: http://localhost.8080/consolaBBDD configuraríamos: **spring.h2.console.path=/consolaBBDD**
2. Verifique que todos y cada uno de los parámetros configurados en el application.config coincida con los mostrados en el modal de pruebas (driver, JDBC URL, User Name, Password).
   
   &emsp;![application.properties.h2](/Imagenes/application_properties_h2.png)
3.

### Fuentes Relacionadas

&emsp;[![Java17](https://img.shields.io/badge/Descargar_JDK_17-oracle.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.oracle.com/java/technologies/downloads/#java17)</br>
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
