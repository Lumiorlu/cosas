# MuleSoft - Best Practices
# Versión
	
Datum: 04.04.2023
Versión: Versión 1
Autor:
Nombre:
Dirección:
Proyecto:

|                |Función         |Teléfono        | Email
|----------------|----------------|----------------|----------------
|{nombre} |  1 |      2      |3
|       X | 1  |       2     |3
|X        |  1 |		2	  |3

Control de versiones:

|                |Fecha |Autor     
|----------------|----------------|----------------|----------------
|**Version 1**	 |  04.04.2023    |      2      
|  **Versión 2** | 1  			  |       2     
|**Versión 3**   |  1             |		2	  
|**Versión 4**   |  1             |		2	  


# ÍNDICE
- Versión
	- Autor
	- Control de versiones
- Índice
- Objeto del documento
- General
	- API
- Naming conventions en el CloudHub
- Exchange estándares
	- API
		- Obligatorio
		- Recomendaciones
	- API Fragment
		- Obligatorio
		- Recomendaciones
	- Template
		- Obligatorio
- Error Management
	- Introducción
	- Common Error Responses
	- Client Id Required
- API Naming Rules
	- Obligatorio
	- Recomendaciones
- Estructura del Proyecto
	- Estructura de archivos del proyecto
	- Naming Convention del proyecto
	- Externalización de Dataweave
	- Recomendaciones
- Oyente HTTP(s)
	- Recomendaciones
- Diseño
	- API First
	- Modelo de Madurez REST
	- Granularidad
		- Recurso
		- Subrecurso
	- Versionado
		- Estrategia general
	- Métodos
	- URIs
	- Parámetros de Query (QueryString)
	- Cabeceras
	- Códigos HTTP


# Objeto del documento

El presente documento servirá como definición de las Best Practices de MuleSoft, siendo altamente recomendables en el desarrollo del ciclo de vida de las APIs.

# General
You can rename the current file by clicking the file name in the navigation bar or by clicking the **Rename** button in the file explorer.

## API
Una API es un conjunto de funciones y procedimientos que permite integrar sistemas entre sí, permitiendo que sus funcionalidades puedan ser reutilizadas por otras aplicaciones

Una API debería contar con estas características:

-   Servicio mock expuesto
	-   El mock service es una simulación del funcionamiento de la API real con limitaciones funcionales y no-funcionales. Este funcionará bajo unos datos estáticos que habremos preparado para simular la respuesta de nuestra API.
    

-   Esta debe ser un servicio REST siempre que se pueda
    

	-   Utilizaremos servicios HTTP para cubrir las necesidades que se identifiquen.
    
	-   Debemos tener recursos identificados(entidad que representa un concepto al cual puede ser accedido).
    
	-   Estos recursos pueden tener sub-recursos(los cuales dependen de una entidad y no tienen relevancia por sí solos)
    
	-   De forma opcional se podría contemplar el uso de HATEOAS:
   
		-   Dado un punto de entrada genérico de nuestra API podremos ser capaces de descubrir sus recursos basándonos únicamente en las respuestas del servidor, esto se realizaría a través de hipervínculos asociados a otros recursos.
    

-   La API debe tener una visibilidad apropiada(API Portal)
    

	-   La API se debe referenciar en el apartado de Documentación donde:
    

		-   Realicemos una descripción de la API
    
		-   Tengamos una lista de recursos, sub.recursos y métodos
    
		-   Poseamos un listado de los servicios expuestos y las entidades que tengamos asociadas a estos
    
		-   Localizemos los endpoints que vamos a consumir
    
		-   Esté el enlace al API Portal donde estará la API implementada
    

	-   Las APIs deben aparecer siempre en el API Portal pero en caso de carecer de esta herramienta deberemos almacenar toda esta información en un espacio de Documentación, donde debemos incluir a parte de todo lo anterior debe aparecer:
    

		-   Ejemplos de casos de uso
    
		-   Instrucciones del mocking service
    
		-   Instrucciones del consumo de las misma
    

-   Todas las APIs deben estar correctamente versionadas aparte de tener una definición única, esta deberá ser versionada en caso de ser necesario.
    
-   Se debe utilizar el estándar RAML 1.0.
    
-   La documentación se debería realizar convenientemente siempre en inglés y deberá estar correctamente documentada ya que se deberá poder consumir en base a esta documentación .
    
-   El consumo de la API se deberá manejar mediante permisos, visibilidad y planes de consumo de la misma.

# API Naming Rules

Este apartado define los estándares que utilizaremos en los proyectos de MuleSoft en cuanto a la nomenclatura de todas las APIs que construiremos.

## Obligatorio

  

-   Utilizar kebab-case para los PATH segments. Ejemplo: example-<CLIENTE>
    
-   Usar snake-case (nunca camel-case) para los query parameters. Ejemplo: ?page_size=30
    
-   Usar encabezados HTTP con guión. Ejemplo: X-Request-Id
    
-   Los sustantivos singulares se utilizarán para nombrar los recursos atómicos. Ejemplo: system-health
    
-   Pluralizar los sustantivos para los nombres de las colecciones. Ejemplo: /beers
    
-   Evitar trailing slashes. Ejemplo: <CLIENTE>/
    

## Recomendaciones

  

Todos los nombres y descripciones deben estar inglés.

Preferiblemente usar upper-camel-case para los campos de cabecera HTTP. Ejemplo: Content Type.

Usar encabezados estandarizados.

# Estructura del Proyecto


## Estructura de archivos del proyecto  

-   Nombre de proyecto en kebab-case. Ejemplo: employee-data-api.
    
-   Crear un xml separado para los elementos globales. Ejemplo: global.xml,
    
-   Crear un xml separado para cada caso de uso o implementación de recursos.
    
-   Crear xml separados para estructuras/lógicas comunes, si las hay.
    
-   Crear xml separado para el tratamiento de errores generales: error-handling.xml
    
-   Crear diferentes paquetes para los recursos: Ejemplos: dataweave, examples, keystores, properties.
    
-   Todas las configuraciones deben estar situadas en el archivo global.xml
    

## Naming Convention del proyecto

**Proyecto**: layer-apiname-subdomain ( solo en APIs de Sistema ) domain-category-instance ( optional )

**Capa**: indica el nivel de capa de una API en el modelo API-Led. Los diferentes tipos de capa son:

		eapi: Api de la capa de Experiencia

		papi: API de la capa de Proceso

		sapi: API de la capa de Sistema

		bapi: API para batch

**Nombre de API**: Nombre de la API o aplicación. Debe ser descriptivo, relacionado con la funcionalidad o el objeto implementado y estar en plural. Ejemplo: productos, usuarios…

**Subdominio**: campo opcional indicado únicamente en las APIS de la capa de Sistema. Añadido para indicar el dominio del sistema de donde se obtiene la información. Ej: Salesforce, sap.

**Dominio**: Nombre del dominio del negocio o rama de negocio responsable de la API. Ejemplo: datos, marketing…

**Categoría**: La categoría de la API. Esta puede ser core, oem, template o generic. En el caso de las EAPI este campo no será necesario.

**Instancia**: El nombre de la instancia de la API en el caso de estar desplegada en múltiples Business Groups.

  ---
**Mule Configuration File**:

- Configuraciones: global.xml

- Entorno: Incluir propiedad global con el entorno, mule.env = {​​​​​​​default/dev/int/qa…}.

- Fichero de propiedades: {​​​​​​​default/dev/int/qa…}​​​​​​​-properties.yaml. Ejemplo: dev-properties.yaml

- Propiedades por entorno: A la hora de hacer referencia al fichero de propiedades utilice un marcador de posición en el nombre de la ubicación del archivo para identificar el entorno. Ejemplo: ${mule.env}-properties.yaml

- Property Key: (kebab-case) {​​​​​​​significant-name}​​​​​​​. Ejemplo: salesforce-user

- Nombre de flujo: {nombre-descriptivo}​​​​​​​-{acción}​​​​​​​-flow.  Ejemplo: order-get-flow, common-logging-flow.

- Nombre subflujo: {​​​​​​​nombre-descriptivo}​​​​​​​-subflow. Ejemplo: process-users-subflow

- Conector: {acción}​​​​​​​ {​​​​​​​nombre-descriptivo}​​​​​​​. Ejemplo: set-response-OK

- Nombre de variable de flujo: (camelCase) {nombre-descriptivo}​​​​​​​

  

## Externalización de Dataweave

Toda implementación de dataweave debe estar en ficheros separados .dwl, los cuales deben estar ubicados en la carpeta src/main/resources/dataweave.

Esta funcionalidad debe ser usada para acelerar la implementación del Dataweave.

Toda lógica que pueda ser reutilizable debe implementarse como una función y exponerse en un archivo separado de Dataweave, por ejemplo, utils.dwl.

Cuando se trabaje con cargas útiles grandes, se debe incluir la directiva indent=false para mejorar el rendimiento de análisis del cliente y reducir el tamaño de la carga útil de respuesta.

## Recomendaciones

Si el implementation.xml se vuelve demasiado grande debe ser dividido en diferentes archivos.

Cada flujo de integración implementado no debe de tener más de diez procesadores de mensajes.

Cada flujo debe tener una descripción proporcionada en la pestaña de notas del menú de configuración del flujo. De esta manera será fácil de entender las funcionalidades implementadas por el flujo.

# **Naming conventions en el CloudHub**

-   Interfaz de la API (RAML)
    -   El archivo se deberá nombrar en base al nombre de la API.
    -   Nunca se nombrará con el nombre del dominio de la API.
-   Despliegue en CloudHub   
    -   Se debe incluir la categoría a la que pertenece la API .
    -   Debemos especificar la capa a la que pertenece la API que queremos desplegar.  
    -   Absolutamente todos los despliegues deben incluir el nombre del dominio de la API desplegada.
-   URL de la API
    -   El número de versión principal de la API se incluirá en la URL.  
    -   El nombre de dominio se incluirá también en la URL.
    
# **Exchange estándares**

## API

**Obligatorio**

-   En este apartado será necesario considerar los siguientes puntos:
    -   Especificar las políticas de seguridad que han sido aplicadas en la API.   
    -   Realizar una descripción de las funciones que va a tener nuestra API.
    
**Recomendaciones**
-   Sería interesante realizar una descripción de los recursos y métodos soportados en nuestra API para de esta manera completar su documentación.

## API Fragment

**Obligatorio**

-   Deberemos añadir una pequeña descripción del fragmento.
-   Indicar el tipo de fragmento, ya sea trait, dataType,example, uses…   
    
**Recomendaciones**
-   Sería recomendable si el fragmento a trabajar es pequeño, añadir el contenido del API Fragment.
## Template

**Obligatorio**
-   Es imprescindible una guía de usuario la cual indique cómo trabajar con el template y los cambios que son necesarios realizar.
-   Describir la funcionalidad que va a tener nuestro template   
## Example

**Obligatorio**
-   Es imprescindible una guía de usuario la cual indique cómo realizar correctamente el set-up de nuestro example y también de cómo trabajar correctamente con él.    
-   Describir la funcionalidad que va a tener nuestro example.

# Error Management

## Introducción
-   Es necesario gestionar los errores en absolutamente todas las integraciones y tareas especificadas.

## Common Error Responses
-   Para utilizar este fragmento deberemos añadir trait en la cabecera de las especificación de la API
<div>
<img src=https://raw.githubusercontent.com/Lumiorlu/cosas/main/Imagen1.png format=jpg&name=small" width="800px">
</div>

- El formato que utilizaremos para dar forma a la respuesta del error en el RAML será el siguiente

<div>
<img src=https://raw.githubusercontent.com/Lumiorlu/cosas/main/Imagen2.png format=jpg&name=small" width="300px">
</div>

-   En la imagen anterior podemos observar el esquema del JSON de error que vamos a utilizar, este modelo de respuesta es muy eficaz ya que nos da de forma muy detallada la información del error.
    -   También podemos observar 3 campos principales, estos son:
        -   Message: Muestra el tipo de error y su código de estado.
        -   Description: Descripción del error.
        -   URL: Enlace a la documentación con información sobre el error y posibles soluciones hacia el mismo.
## Client Id Required

- En este trait se va a especificar la respuesta del sistema relacionada con la información de la gestión de la identificación del cliente.
<div>
<img src=https://raw.githubusercontent.com/Lumiorlu/cosas/main/Imagen1.png format=jpg&name=small" width="800px">
</div>

- Client-id-required.raml
<div>
<img src=https://raw.githubusercontent.com/Lumiorlu/cosas/main/Imagen3.png format=jpg&name=small" width="250px">
</div>


# Diseño
## API First

Es un enfoque tanto a nivel empresarial como a nivel de desarrollo de software. el cual se caracteriza por dar máxima prioridad e importancia al diseño y desarrollo de la API.

Una vez la empresa ya ha creado esta API, se empiezan a construir los diferentes elementos que la rodearán, como pueden ser la lógica de negocio o la interfaz del usuario (UI).
Este enfoque se basa en varios principios, entre los que destacan:

 1. La API es el núcleo del software: La API es la base fundamental del software y todo lo demás se construye encima de ella.
 2.  La API es independiente de la UI: La API se desarrolla de forma independiente de la interfaz de usuario, lo que permite una mayor flexibilidad en la construcción de la UI.
3. La API es una interfaz bien definida: La API debe estar bien documentada y ser fácil de entender y utilizar por otros desarrolladores.
4. La API es la única forma de acceder a los datos y servicios del software: Todo el acceso a los datos y servicios se realiza a través de la API, lo que permite un mejor control de la seguridad y la privacidad de los datos.
5. La API es escalable: La API se diseña desde el principio para ser escalable, lo que permite una fácil integración con otros sistemas y una mayor adaptabilidad a los cambios en el entorno del software.

Al seguir el enfoque API First, los equipos de desarrollo pueden construir software más modular, escalable y adaptable. Además, se puede mejorar la eficiencia en el desarrollo y reducir los tiempos de comercialización, ya que se enfoca en construir primero lo que es fundamental para el negocio y se reduce la dependencia de una sola tecnología o plataforma.


## Modelo de Madurez REST

REST es un enfoque arquitectónico de diseño de sistemas que se utiliza en la construcción de sisitos web. Este enfoque define unos principios de cómo deben ser diseñados y estructurados los servicios web. REST se basa en la utilización de protocolos HTTP y HTTPS, utilizando el formato JSON y XML para el intercambio de datos.

A la hora de utilizar este enfoque, nos apoyamos en el modelo propuesto por Leonard Richardson, el cual nos ayuda a entender mejor el concepto de REST y a tratar de implementarlo y afrontarlo de la mejor forma posible.

![enter image description here](https://waltermontes.files.wordpress.com/2014/02/overview_thumb.png?w=532&h=315&zoom=1)

Los niveles que se proponen en el diagrama son los siguientes:

-   Nivel 0 – Swamp of POX: Se refiere a usar HTTP para interacciones remotas pero sin usar otros mecanismos existentes para la web.
    
-   Nivel 1 – Recursos: La idea es identificar los recursos a través de un URI sin especificar la acción a realizar sobre el mismo. Ejemplo:
    

	La URI `http://www.misitio.com/personas/1` representa a una persona y p `http://www.misitio.com/personas/2` representa a otra persona.

	En lugar de `http://www.misitio.com/personas?id:1`

-   Nivel 2 – Verbos HTTP: Este nivel nos indica que el API debería utilizar los verbos HTTP que nos proporciona el propio protocolo (GET, POST, DELETE, PUT). Utilizando correctamente los verbos y las respuestas, tenemos los siguientes ejemplos:
    

		GET http://www.misitio.com/personas/1

		POST http://www.misitio.com/personas/1

		DELETE http://www.misitio.com/personas/1

		PUT http://www.misitio.com/personas/1

	Evitando por ejemplo que si ocurre un error lo que se retorne sea un OK (200) que representa una solicitud correcta, sin errores.

-   Nivel 3 – Controles de hypermedia: Se centra en la utilización de HATEOAS (Hipertexto como el mecanismo del estado de la aplicación). Este principio se basa en la idea de que un servicio web debe incluir en sus respuestas información adicional que permita a los clientes (aplicaciones o sistemas) conocer cómo interactuar con el servicio. En otras palabras, el servicio web debe proporcionar enlaces a los recursos relacionados y a las acciones que se pueden realizar en el contexto actual.
    

	Ejemplo: Si solicitamos datos de una persona `http://www.misitio.com/personas/1` a través de un middleware intermediario podemos determinar qué permisos tiene la persona y que acciones puede realizar y así retornarlas. Aquí es donde HATEOAS ayudaría a retornarnos el acceso a diversas funcionalidades para el elemento, ya sea a través de HTML o XML/JSON.

		GET http://www.misitio.com/personas/1

	Resultado en JSON:

		{
		“nombre”: “Walter”,
		”apellido”: “Montes”,
		”href”: “http://www.misitio.com/personas/1”
		}

	Con la referencia a la persona junto con la respuesta. Así cuando necesitemos realizar otra acción podemos utilizar los datos de URI del recurso.

En todas nuestras aplicaciones, buscaremos un **nivel de madurez 3**.

## Granularidad

La granularidad de una API hace referencia al nivel de detalle y complejidad de los recursos y operaciones que se exponen a través de la API. Dependiendo del nivel de granularidad, el nivel de detalle y complejidad en los recursos variará, además del nivel de abstracción de los mismos. Siguiendo las mejores prácticas, utilizaremos los siguientes parámetros a la hora de conseguir la granularidad deseada:

-   Una API no debe exceder de 2 niveles de jerarquía (dependencias entre recursos y subrecursos) y no debe tener más de 8 recursos (cantidad de subentidades).
    
-   Un mismo recurso puede tener distintas operaciones identificadas por sus métodos.
    
-   Los recursos identificados vía URL establecen una relación de jerarquía entre ellos:
    

		/<entity>
		<entity>/{entity_id}/
		<entity>/{entity_id}/<sub_entity>
		<entity>/{entity_id}/<sub_entity>/{sub_entity_id}


No se recomiendan patrones  ***/me*** en el diseño, ya que puede causar duplicidad de recursos. Consumir APIs que requieran información sobre el usuario autenticado deben hacer uso de tokens para - mediante introspección en caso de OAuth o validación de tokens JWT – obtener la identidad del usuario. En caso de la ausencia de información en el token, deberá hacerse uso de recursos genéricos del tipo /user/{id} para extraer dicha información,, en lugar de usar patrones /me.

### Recurso

Este concepto hace referencia a una entidad u objeto de negocio que es identificable y accesible a través de un URI único y bien definido. Estos recursos son la base de una API REST, ya que representan los datos que se están utilizando y manipulando a través de la API. Cada recurso debe tener una representación clara y consistente, sin posibilidad de ambigüedad.

### Subrecurso

Por otro lado, este concepto se entiende como un objeto de negocio que no tiene relevancia por sí mismo y necesita del recurso para tener valor y significado. Es un objeto que está relacionado jerárquicamente a un recurso.

## Versionado

En este apartado se explicará la estrategia general a seguir por parte de la organización para definir y normalizar el versionado utilizado para las APIs. Esta estrategia deberá ser visible por toda la organización y por el C4E.

### Estrategia general

-   Todo cambio realizado en la API tratará de ser compatible con la versión anterior.
    
-   Versionar la API en el caso de que se necesiten cambios incompatibles con la versión anterior.
    
-   Se seguirá el enfoque de versionado más común para las APIs, siendo el esquema Major.Minor.Patch, también conocido como semántica de versionado.
    

A definir este esquema, tenemos los siguientes niveles:

-   La versión Major se incrementa cuando se realizan cambios importantes e incompatibles en la API, como agregar o eliminar recursos o modificar la forma en que se accede a ellos. El cambio de versión Major indica que es posible que se requieran cambios significativos en las aplicaciones cliente existentes para que sean compatibles con la nueva versión de la API.
    
-   La versión Minor se incrementa cuando se agregan nuevas funcionalidades a la API sin afectar la compatibilidad con las versiones anteriores. Los cambios de versión Minor indican que se han agregado nuevas funcionalidades, pero que las aplicaciones cliente existentes pueden seguir funcionando sin cambios.
    
-   La versión Patch se incrementa cuando se realizan correcciones de errores o soluciones a problemas de seguridad, sin afectar la compatibilidad con las versiones anteriores. Los cambios de versión Patch indican que se han corregido errores o se han implementado mejoras de seguridad, pero que las aplicaciones cliente existentes pueden seguir funcionando sin cambios.

Esta estrategia permite a los desarrolladores de API comunicar de manera clara y coherente los cambios realizados en la API y su impacto en la compatibilidad con las versiones anteriores, lo que ayuda a los clientes a tomar decisiones informadas sobre la actualización de sus aplicaciones.

## Métodos

| Método HTTP  | Operación CRUD | Descripción |
| -- |--|--|
| GET | Read | Para recuperar un recurso |
| POST	 | Create | Para crear un recurso o ejecutar una operación compleja en un recurso. |
| PUT | Update | Para editar (si el recurso existe) o crear (si el recurso no existe) un recurso. |
| PATCH | Update | Para realizar una actualización parcial de un recurso. |
| DELETE | Delete | Para eliminar un recurso. |
| HEAD | Read | Para recuperar un recurso sin el contenido del body. |

La operación real invocada debe coincidir con la semántica del método HTTP como se define en la tabla anterior.
- Operaciones para el arquetipo **Colección**:
	- GET: Petición de la lista de recursos de una colección (puede contener filtros):
		 `GET /<resource>`
	- POST: Creación de un recurso: 
		`POST /<resource>`
	- La colección debe tener un nombre en plural.
- Operaciones para el arquetipo **Documento/Recurso**:
	- GET: Acceso a un documento: 
		`GET /<resource>/{resource_id}`
	- PUT: Actualización completa de un recurso: 
		`PUT /<resource>/{resource_id}`
	- PATCH: Actualización parcial de un recurso: 
		`PATCH /<resource>/{resource_id}`
	- DELETE: Eliminación de un recurso: 
		`DELETE /<resource>/{resource_id}`
- Operaciones para el arquetipo **controlador**:
	- SIEMPRE se usa un POST
	- La operación debe ser un verbo que describa la acción a aplicar al recurso.
- El método GET no debe tener efectos secundarios. Por ejemplo, no debe cambiar el estado de un recurso subyacente.
	- POST: el método debe usarse para crear un nuevo recurso en una colección.
		- Idempotencia: Si se lanza una ejecución posterior de la misma invocación y el recurso ya se creó, entonces la solicitud DEBERÍA ser idempotente.

El método POST deberá utilizarse para crear un nuevo sub-recurso y establecer su relación con el recurso principal. Este método puede usarse en operaciones complejas junto con el nombre de la operación. Esto refleja el arquetipo “controlador” y se considera una excepción al modelo RESTful. Es más aplicable en los casos en que los recursos representan un proceso de negocio y las operaciones son los pasos o acciones a realizar como parte de él.

El método PUT deberá usarse para actualizar los atributos de los recursos o para establecer una relación de un recurso a un subsistema existente. El método actualiza el recurso principal con una referencia al sub recurso.

## URIs

Las URIs de las APIs deberán seguir los siguientes principios:
-   Se definen en minúsculas, siguiendo la notación kebab-case.
-   Los cuerpos de las entradas y salidas seguirán la notación lowerCamelCase.
-   No se deben incluir verbos.
	-   Al apuntar a los diferentes recursos, estos deberán ser nombres (sustantivos).
	-   No usar URIs del estilo:

			`https://msapi.<CLIENTE>.com/user/get-users`

	Esta regla se aplica en todos los casos excepto en las operaciones relacionadas con controladores.

-   Los recursos seguirán el siguiente esquema y jerarquía:
    

		`<recurso1>/<recurso1_id>/<recurso2>/<recurso2_id>`

	La semántica del id del recurso tendrá las siguientes características:

		-   Representado entre llaves.
		-   Nombre en singular.
		-   Acabado en _id.
	    

	Ejemplo:
		`https://msapi.<CLIENTE>.com/v1/namespace/resource/{resource_id}`
		`https://msapi.<CLIENTE>.com/v1/billing/payment/{payment_id}`

## Parámetros de Query (QueryString)

Dentro del contexto de las solicitudes de HTTP, nos encontramos con las Query String, las cuales serán comprendidas por las llamadas Query Parameters. Estas se utilizan para enviar información al servidor al realizar una solicitud.

Ejemplo: `https://www.google.com/search?q=ejemplo+de+query+string&e=y+esto`

Parámetros como, por ejemplo, los de paginación deberán seguir las guías de paginación.

| QueryString| Descripción |
| -- |--|--|
| **page**  (deseable)| Entero no negativo que representa la página de los resultados esperados. El envío por parte del cliente es opcional. El valor por defecto es 1. Debe responder a un número incorrecto: 400 BadRequest. Si el número de página es demasiado grande, el recurso debe responder con el código 200 OK y una lista de resultados vacía.
| **pageSize** (deseable)| Entero no negativo que indica el número máximo de resultados que se devolverán a la vez. El envío por parte del cliente es opcional. Debe tener un valor por defecto. <br> Ejemplo : `https://msapi.<CLIENTE>.com/v1/namespace/resource/{resource_id}?page_size=50`
| **offset**| Entero no negativo que representa el ítem por el que comenzará la página devuelta. Por defecto, 0. <br>Ejemplo: <br> - Si una petición GET devuelve un listado de 100 elementos con un tamaño de página 20, una petición sin añadir offset nos devolvería los primeros 20 elementos. <br>- En una petición con un offset de 1, nos devolvería 20 elementos obviando el primer elemento. <br>- En una petición con un offset de 5, nos devolvería 20 elementos obviando los 5 primeros elementos.

Todos los demás métodos e instrucciones de las cabeceras seguirán el formato oficial de las mismas. Ejemplo:

| QueryString | Descripción |
|--|--|
| **sort** (deseable) | Permite ordenar los resultados obtenidos de manera ascendente o descendente según el campo indicado. La sintáxis general será: <br>{field_name_1}{asc/desc},{field_name_2} {asc/desc}<br> `https://msapi.<CLIENTE>.com/v1/namespace/resource/{resource_id}?sort=field1/asc,field2/desc`  

## Cabeceras

| Cabecera | Descripción |
|--|--|
| Accept-Language (deseable) | El consumidor solicita la lista de idiomas por orden de preferencia. |
| Content-Language (deseable) | El servidor responde con el idioma de la respuesta. |
| Authorization | Campo para enviar el token de acceso al API (OAuth, JWT). |
| If-None-Match (en caso de necesidad) | Obtiene los datos cacheados siempre que le ETag no haya cambiado. |
| If-Match (en caso de necesidad |Utilizado para solicitudes de modificación de servicios. Permite realizar cambios sólo si ningún otro cliente ha realizado ningún otro cambio durante la ejecución. |
| ETag (en caso de necesidad) | Campo utilizado en el proceso de acceso/modificación de recursos cacheados |
| Accept (deseable) | El consumidor solicita el formato de datos de la respuesta. | 
| Content-Type | El servidor responde con el formato de los datos de respuesta. |
| Location | El servidor devuelve tras un POST la URL del recurso recién creado. |

## Códigos HTTP

Todas las APIs deben usar solo los siguientes códigos de estado. No se deben devolver códigos que no estén en este listado y puede devolver solo algunos de ellos.

Cuando hacemos una solicitud a un recurso en una API siempre debe devolver el código HTTP relacionado con el error que reporta. Los códigos de estado HTTP tienen 5 grupos principales:

	- 1xx: Informativo, solicitud recibida, proceso en curso.

	- 2xx: Exitoso, la acción fue recibida, entendida y aceptada con éxito.

	- 3xx: Redirigida, se debe tomar una acción adicional para completar la solicitud.

	- 4xx: Error del cliente, la solicitud contiene mala sintaxis o no puede ser cumplida.

	- 5xx: Error del servidor, el servidor no pudo cumplir una solicitud aparentemente válida.

Estos grupos principales son comprendidos por otros subgrupos los cuales son:
|Código| Descripción |
|--|--|
| 200 | El código de estado 200 (OK) indica que la solicitud ha tenido éxito |
| 201 | El código de estado 201 (Create) indica que la solicitud se ha cumplido y ha dado lugar a la creación de uno o más recursos nuevos. |
| 202 | El código de estado 202 (Accepted) indica que la solicitud ha sido aceptada para su procesamiento, pero que el procesamiento no se ha completado.. |
| 203 | El código de estado 203 (Non-Authoritative Information) indica que la petición fue satisfactoria pero su contenido ha sido modificado por un transformador proxy desde los orígenes del servidor 200 (OK). |
| 204 | El código de estado 204 (No content) indica que el servidor ha cumplido satisfactoriamente la solicitud y que no hay contenido adicional que enviar en el body de respuesta. |
| 400 | El código de estado 400 (Bad Request) indica que el servidor no puede o no quiere procesar la solicitud debido a algo que se percibe como un error del cliente. Este código debe documentarse en todas las operaciones en las que sea necesario recibir datos en la solicitud. |
| 401 | El código de estado 401 (No Authorizated) indica que la solicitud no se ha aplicado porque carece de credenciales de autenticación válidas para el recurso de destino.Este código debe documentarse en todas las operaciones de las API que requieran la suscripción de un cliente. |
| 403 | El código de estado 403 (Forbidden) indica que el servidor entendió la solicitud pero se niega a autorizar.Este código suele estar documentado en las operaciones. |
| 404 | El código de estado 404 (No encontrado) indica que el servidor de origen no ha encontrado una representación actual del recurso de destino o no está dispuesto a revelar que existe. Es necesario documentar esta devolución en tales situaciones. |
| 405 | El código de estado 405 (Method Not Allowed) indica que el método recibido en la línea de solicitud es conocido por el servidor de origen pero no está respaldado por el recurso de destino. |
| 406| El código de estado 406 (Not Acceptable) indica que el recurso de destino no tiene una representación actual que sería aceptable para el agente usuario, según los campos de encabezamiento recibidos en la solicitud, y el servidor no está dispuesto a ofrecer una representación. |
| 408 | El código de estado 408 (Request Timeout) indica que el servidor no recibió un mensaje de solicitud completo en el tiempo que estaba dispuesto a esperar. |
| 415 | El código de estado 415 (Unsupported Media Type) indica que la entidad solicitante tiene un tipo de medio que el servidor o recurso no admite. Este código debe ser documentado. |
| 500| El código de estado 500 (Internal Server Error) indica que el servidor se encontró con una condición inesperada que le impidió cumplir con la solicitud. Este código siempre debe ser documentado. | 502| El código de estado 502 (Bad Gateway) indica que el servidor estaba actuando como gateway o proxy y recibió una respuesta inválida del servidor upstream.
| 503| El código de estado 503 (Service Unavailable) indica que el servidor no puede atender la solicitud (porque está sobrecargado o no disponible para mantenimiento).

Estos códigos siguen la especificación oficial HTTP/1.1.

# Oyente HTTP(s)
Usar los siguientes puertos según el protocolo de conexión:

-   **${http.port}** cuando se implementa en CloudHub/Hybrid mediante HTTP (http.port es una palabra reservada en CloudHub que se resuelve en **8081**)
    
-   **${https.port}** cuando se implementa en CloudHub/Hybrid mediante HTTPS (https.port es una palabra reservada en CloudHub que se resuelve en **8082**)
    
-   **${http.private.port}** cuando se implementa en CloudHub mediante HTTP y un equilibrador de carga dedicado (http.private.port es una palabra reservada en CloudHub que se resuelve en **8091**)
    
-   **${https.private.port}** cuando se implementa en CloudHub mediante HTTPS y un equilibrador de carga dedicado (https.private.port es una palabra reservada en CloudHub que se resuelve en **8092**)
    

A la hora de hacer la petición, los puertos de https se mapean automáticamente al 443 y los http al 80, por lo que deberemos hacer la petición a estos puertos.

## Recomendaciones
Siempre que se conecte dos APIs desde MuleSoft, por ejemplo, una API de proceso y otra de sistemas, utilizaremos el puerto privado (de http o https) para restringir la conexión desde fuera de la VPC.

Esta conexión entre las dos APIs se hará a través del worker, para lo que en la petición request tendremos que añadir delante del host de la API mule-worker-internal- y el puerto será el 8082 o 8092.

Si una API tiene dos posibles entradas, desde fuera de la VPC (internet, sería el caso de una experience) y desde dentro, habría que implementar dos listener distintos, uno para cada entrada para limitar a que se pueda acceder desde fuera de la VPC sólo los endpoint necesarios.


# Testing/QA
A tener en cuenta:

-   Las pruebas unitarias NO dependen unas de otras.
    
-   Las pruebas unitarias pueden ejecutarse en cualquier orden.
    
-   Cada prueba unitaria cubre un área específica más pequeña del código, por lo que es muy fácil identificar el problema en caso de fallo de la prueba unitaria.
    
-   La ejecución de las pruebas unitarias no lleva mucho tiempo. Idealmente, cada ejecución de la prueba unitaria no debería tomar más de unos pocos segundos.
    
-   La prueba unitaria NO se utiliza como prueba de integración = invocación de extremo a extremo de la API / flujo de integración.
    

## MUnit

La implementación de MUnit debe comenzar haciendo clic con el botón derecho en el sub/flujo probado, seleccionando MUnit -> Record test for this flow

Es necesario generar pruebas sobre el mismo flujo pero con diferentes entradas de valor. Cada prueba representará un escenario de caso diferente.

Se deben añadir casos erróneos para comprobar que el control de errores es correcto.

| Qué hacer | Por qué |
|--|--|
| Identificar las unidades de código que desea probar. Se trata más de probar escenarios particularmente importantes que de lograr cobertura. | Lograr la cobertura puede ser inútil si no se prueba la lógica de la aplicación. |
| Reconocer las condiciones previas y posteriores para su prueba. Sepa qué valores necesitará al principio para ejecutar su código y qué valores espera al final de la prueba. | Cuando reconoce la lógica de las entradas y salidas, es más fácil construir la prueba. |
| Identificar los procesadores a simular y seleccionarlos con el atributo doc:id. | Doc:id El atributo es único en todo el archivo XML, por lo que es mejor elegir los procesadores a través de este atributo para evitar confusiones con otros procesadores similares y tener un control granular. |
| Cambiar el nombre de los procesadores MUnit para representar su función. | Ayuda a identificarlos sobre los mismos componentes. |
| Utilizar nombres descriptivos para las pruebas y agregue una descripción en la sección de configuración del componente. | Explique que la intención de las pruebas las hace más legibles y comprensibles. |
| Organizar pruebas en suites y use nombres descriptivos como nombres de archivo para identificar el alcance de la suite. | Es más fácil buscar una prueba si está organizada dentro de la suite con un nombre descriptivo que represente el propósito del grupo de pruebas. |
| Simular los componentes que dependen de recursos externos, como bases de datos, servicios web, colas, etc. Y no olvide los conectores de salida simulados. | Las pruebas unitarias no son pruebas de integración o funcionales. No es necesario realizar llamadas reales ni activar otras funciones, y puede ser contraproducente, ya que las pruebas unitarias tienden a automatizarse y pueden ejecutarse en cualquier momento. |
| Utilizar archivos para especificar entradas de muestra, respuestas o valores devueltos. Se deben guardar en la carpeta src/test/resources y léalos con la función readURL Datawave. | Tener un archivo separado es más fácil de leer y actualizar, y puede promover la reutilización. |
| Una buena métrica es tener una cobertura de código entre el 80 % y el 90 %. | Una cobertura superior al 80% significa que se prueba la mayor parte de la funcionalidad. |

