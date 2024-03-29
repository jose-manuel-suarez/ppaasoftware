# Práctica 2 - Guía de desarrollo

[**_Ver enunciado_**](/Practica%202/Practica%202.pdf "Practica2.pdf")

### Objetivo

&emsp;
Incrementar la funcionalidad de nuestro proyecto web, a partir de la incorporación de las siguientes características: <br>

-   **Validación de datos** a las entidades.
-   **Asociaciones** (relaciones) entre entidades del dominio.
-   **Seguridad** para el acceso a los endpoints.<br>

### Implementando validaciones de datos sobre el proyecto

&emsp;&emsp;
Para la implementación de nuestras validaciones de datos, recurriremos a la utilización de la dependencia **jakarta**, que permite entre otras funcionalidades, la utilización de etiquetas para indicar diferentes restricciones sobre los datos.

1. Para utilizar este paquete debemos agregar en la sección **dependencies** del archivo `build.gradle` ubicado en el directorio raíz del proyecto, las dependencias asociadas a **spring-boot-starter-validation** y **jakarta-validation-api**.
 <figure>
  <img src="../Imagenes/dependencias_validaciones.png" alt="Dependencias de validaciones" style="width: 50%;">
  </figure>

&emsp;&emsp;&emsp;
Tener en cuenta que luego de agregar cualquier dependencia al proyecto siempre debemos limpiar y reconstruir la aplicación mediante: `./gradlew clean build`<br>
&emsp;&emsp;&emsp;
La documentación asociada al [_**uso de jakarta**_](https://beanvalidation.org/2.0/spec/ "Información sobre Jakarta") y al [_**validaciones básicas**_](https://www.baeldung.com/java-validation "Validaciones básicas Jakarta") y la [_**lista completa de validaciones**_](https://beanvalidation.org/2.0/spec/#builtinconstraints "Lista de validaciones Jakarta").

2. Es posible (y recomendable) también configurar un **manejador de excepciones** para que muestre mensajes más representativos y concisos ante cada invocación.
   La documentación asociada a la [_**configuración del manejador de excepcioens**_](https://www.baeldung.com/spring-boot-bean-validation "Manejador de excepciones").

3. Utilizaremos las validaciones en los controladores, mediante la anotación **@Valid**, tal y como se indica en este [_**ejemplo**_](https://www.baeldung.com/spring-boot-bean-validation#implementing-a-rest-controller "Ejemplo uso @Valid").

<br>

### Asociaciando personas y direcciones

1. Crear la entidad **Direccion**, con los siguientes atributos:

    - **Calle** -> no puede ser vacío.
    - **Número** -> no puede ser vacío.
    - **Piso** -> puede estar vacío, es opcional.
    - **Departamento** -> puede estar vacío, es opcional.
    - **Localidad** -> no puede ser vacío.
    - **Provincia** -> no puede ser vacío.
    - **País** -> no puede ser vacío.

2. Actualizar la entidad **Persona**, con las siguientes validaciones:

    - **Apellido** -> no puede ser vacío.
    - **Nombre** -> no puede ser vacío.
    - **Fecha de Nacimiento** -> no se admiten valores inferiores a 18 ni mayores a 150.
    - **Email** -> no puede ser vacío, tener un formato inválido o repetirse en el sistema.

3. Verificar todas y cada una de las validaciones implementadas mediante **PostMan** o cualquier librería o extensión para tal fin.

    **_Recomendación_**: Analice las distintas anotaciones que el framework proporciona para cada tipo de validación requerida y utilice la más indicada para cada caso. Ej. pregúntese para cada atributo: _¿ debería usar @NotNull, @NotEmpty o @NotBlank aquí ?_

4. Actualizar el dominio de aplicación para permitir administrar la relación entre cada persona y sus direcciones. El sistema debe incorporar dos tipos de direcciones para cada persona:<br>
&emsp;- **Direcciones asociadas de envío** (debe tener al menos una)<br>
&emsp;- **Dirección de facturación** (debe tener una).<br><br>
En los siguientes enlaces encontrará más información sobre este tema:
    - [_**Relaciones en Spring**_](https://www.baeldung.com/spring-data-rest-relationships "Relaciones en Spring")
        - Céntrese en las relaciones, ignorar el uso de @RestResource
    - [_**Relación 1 a 1**_](https://www.baeldung.com/jpa-one-to-one "Relación 1 a 1")
    - [_**Relación 1 a N**_](https://www.baeldung.com/hibernate-one-to-many "Relación 1 a N")
    - [_**Relación N a N**_](https://www.baeldung.com/jpa-many-to-many "Relación N a N")

<br>

### Implementando autenticación y autorización al proyecto

Implementar mediante **JSON Web Token** (JWT), la autenticación y autorización para cada invocación de los servicios REST expuestos.
Para ello, inicialmente consideraremos únicamente dos roles posibles:

-   **USER** (usuario) -> sólo puede realizar lectura sobre los datos.
-   **ADMIN** (administrador) -> puede realizar lectura y escritura sobre los datos.<br>
**_Aclaración_**: Considere utilizar como _username_ el **email** de las personas.

<br>

### Implementación de casos de prueba mediante JUnit + Mockito

Para las validaciones de nuestras clases utilizaremos JUnit y la utilización de Mocks Objects mediante la librería Mockito.

<br>

### Documentación de la API mediante OpenAPI

Para la documentación de nuestra API Rest utilizaremos el estandar OpenAPI que nos permitirá obtener de manera directa la documentación asociada a la definición de nuestros puntos de entrada en formato JSON  o YAML.

Además mediante su integración con Swagger nos permitirá visualizar y trabajar sobre cada punto de entrada de manera ágil y simple, utilizando una interfaz gráfica atractiva y muy intuitiva.

<br>

### Documentación de la arquitectura mediante C4

Para la documentación de nuestras arquitecturas de software utilizaremos el **modelo C4**.
Esto nos permitirá comunicar a cualquier interesado en el proyecto, los diferentes niveles de detalle del mismo.

<br>

### Fuentes Relacionadas

&emsp;[![JakartaValidation](https://img.shields.io/badge/Jakarta_Validation-beanvalidation.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://beanvalidation.org/2.0/spec/)<br>
&emsp;[![JavaValidation](https://img.shields.io/badge/Java_Validation_Basics-bealdung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/java-validation)<br>
&emsp;[![ExceptionHandler](https://img.shields.io/badge/Manejador_de_Excepciones-bealdung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/java-validation)<br>
&emsp;[![ListaValidaciones](https://img.shields.io/badge/Lista_de_Validaciones_Jakarta-beanvalidation.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://beanvalidation.org/2.0/spec/#builtinconstraints)<br>
&emsp;[![@Valid](https://img.shields.io/badge/Uso_de_@Valid-bealdung.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.baeldung.com/spring-boot-bean-validation#implementing-a-rest-controller)<br>

&emsp;[![OpenAPI](https://img.shields.io/badge/OpenAPI_Initiative-openapis.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.openapis.org/)<br>

&emsp;[![STC](https://img.shields.io/badge/Self_Testing_Code-martinFouler.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://martinfowler.com/bliki/SelfTestingCode.html)<br>
&emsp;[![EjMockito](https://img.shields.io/badge/Ejemplo_con_Mockito-youtube.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.youtube.com/watch?v=uGZQdD9IpQc)<br>

&emsp;[![JSON](https://img.shields.io/badge/Sintaxis_JSON-json.org-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://www.json.org/json-en.html)<br>
&emsp;[![Yaml](https://img.shields.io/badge/Sintaxis_YAML-docs.ansible.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)<br>

&emsp;[![C4](https://img.shields.io/badge/Modelo_de_Visualizacion_de_Arquitectura_C4-c4model.com-1abc9c.svg?logo=GoogleChrome&logoColor=1abc9c)](https://c4model.com/)<br>


<br>

### Contacto

&emsp;[![LMB-email](https://img.shields.io/badge/Luis_Mariano_Bibbo-0A84FF?logo=gmail)](mailto:lmbibbo@lifia.info.unlp.edu.ar)
&emsp;[![JMS-email](https://img.shields.io/badge/Jose_Manuel_Suarez-0A84FF?logo=Gmail)](mailto:jsuarez@lifia.info.unlp.edu.ar)
&emsp;[![Canal-Discord](https://img.shields.io/badge/PPAA_de_Software-%235865F2.svg?&logo=discord&logoColor=white)](https://discord.gg/XAdRPKVZCT)
