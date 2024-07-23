# Spring Framework

... EN DESARROLLO ...

## Overview

**Spring Framework** es un poderoso y ampliamente utilizado marco de desarrollo de software para aplicaciones empresariales en Java. Diseñado para simplificar y acelerar el desarrollo de aplicaciones, Spring ofrece un enfoque integral que abarca desde la configuración hasta la implementación, abordando varios aspectos del desarrollo de software como la inversión de control, la inyección de dependencias, la gestión de transacciones, la seguridad y mucho más.

Una de las características distintivas de Spring es su **enfoque modular y extensible**, permitiendo a los desarrolladores elegir los módulos específicos que necesitan para sus proyectos. Además, fomenta las mejores prácticas de programación y sigue el principio de diseño de "Programación Orientada a Aspectos" (AOP), que facilita la separación de preocupaciones y mejora la modularidad del código.

Spring Framework se utiliza comúnmente para construir aplicaciones empresariales robustas y escalables, facilitando la creación de servicios web, aplicaciones basadas en la arquitectura Modelo-Vista-Controlador (MVC), integración con bases de datos, gestión de transacciones y mucho más. Con una comunidad activa y un ecosistema de proyectos relacionados, Spring ha evolucionado para adaptarse a las cambiantes demandas del desarrollo de software, convirtiéndose en una opción popular entre los desarrolladores Java.

**Spring Boot** es una extensión del popular Spring Framework que se centra en simplificar drásticamente el proceso de desarrollo de aplicaciones Java, especialmente aplicaciones basadas en Spring. Su objetivo principal es facilitar la creación de aplicaciones autónomas, autocontenidas y listas para la producción con la menor cantidad de configuración posible.

La relación entre Spring Boot y Spring Framework es fundamental, ya que Spring Boot se construye sobre la base sólida proporcionada por Spring. Spring Boot utiliza las características clave de Spring, como la inversión de control (IoC) y la inyección de dependencias, pero agrega una capa de convenciones y configuraciones por defecto para acelerar el desarrollo.

Lo más notable de Spring Boot es su enfoque de "opinión sobre la configuración", lo que significa que proporciona configuraciones predeterminadas sensatas para la mayoría de los casos de uso, permitiendo a los desarrolladores empezar rápidamente con sus proyectos sin tener que configurar extensamente. No obstante, sigue siendo altamente personalizable, permitiendo a los desarrolladores anular las configuraciones por defecto según sea necesario.

Con Spring Boot, el proceso de desarrollo se simplifica mediante la inclusión de un servidor embebido, como Tomcat o Jetty, lo que elimina la necesidad de desplegar la aplicación en un servidor externo. También facilita la gestión de dependencias mediante el uso de la herramienta Spring Initializr para generar proyectos con las dependencias necesarias.

En resumen, Spring Boot es una extensión de Spring Framework diseñada para hacer que el desarrollo de aplicaciones Java sea más rápido, sencillo y eficiente al proporcionar configuraciones por defecto y convenciones inteligentes sin sacrificar la flexibilidad y la potencia que ofrece Spring Framework.

> Introducción generada por ChatGPT

## [Spring Core Annotations](https://www.baeldung.com/spring-core-annotations)

Estas anotaciones forman parte del paquete [org.springframework.beans.factory.annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/package-summary.html) y [org.springframework.context.annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/package-summary.html).

### DI-Related Annotations

#### @Autowired

La anotación `@Autowired` se utiliza para marcar una dependencia que el motor DI de Spring resolverá e inyectará. Esta anotación se puede usar en un **constructor**, en un **método _'setter'_** o en un **campo**:

```java
// Constructor injection
class Car {
  Engine engine;

  @Autowired
  Car(Engine engine) {
    this.engine = engine;
  }
}
```

A partir de la versión 4.3, no es necesario anotar constructores con `@Autowired` de forma explícita. Sin embargo, si hay dos o más contructores sigue siendo necesario para que Spring sepa cuál de ellos debe utilizar.

Si se utiliza la inyección del constructor, **todos los argumentos del constructor son obligatorios**.

```java
// MyService.java
package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MyService {

    // Inyección por campo
    @Autowired
    private Dependency1 dependency1;

    private Dependency2 dependency2;

    // Inyección por constructor
    @Autowired
    public MyService(Dependency2 dependency2) {
        this.dependency2 = dependency2;
    }

    // Inyección por setter
    @Autowired
    public void setDependency1(Dependency1 dependency1) {
        this.dependency1 = dependency1;
    }

    public void printMessages() {
        System.out.println("Dependency1: " + dependency1.getMessage());
        System.out.println("Dependency2: " + dependency2.getMessage());
    }
}
```

`@Autowired` tiene un argumento booleano llamado `required` con un valor predeterminado de `true`. Este argumento ajusta el comportamiento de Spring cuando no encuentra un bean adecuado para conectar. Cuando es verdadero, se lanzará una excepción; de lo contrario, no se conecta nada.

**NOTA**: puedes utilizarse `@Inject` en lugar de `@Autowired` en Spring Framework. `@Inject` es parte de la especificación estándar de Jakarta EE/CDI, mientras que `@Autowired` es específico de Spring y ofrece funcionalidades adicionales. Para poder utilizar esta anotación, hay que añadir la dependencia al `pom.xml`:

```xml
<dependency>
    <groupId>jakarta.inject</groupId>
    <artifactId>jakarta.inject-api</artifactId>
    <version>2.0.0</version>
</dependency>
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)

- [Guide to Spring @Autowired - Baeldung](https://www.baeldung.com/spring-autowire)

- [Constructor Dependency Injection in Spring - Baeldung](https://www.baeldung.com/constructor-injection-in-spring)

- [Using @Autowired - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/autowired.html)

- [Using JSR 330 Standard Annotations - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/standard-annotations.html)

#### @Bean

La anotación `@Bean` marca un _'factory method'_ que crea una instancia de un _bean_ de Spring:

```java
@Configuration
public class AppConfiguration {
  @Bean
  public Engine engine() {
    return new Engine();
  }
}
```

**Spring llama a estos métodos** cuando se requiere una nueva instancia del tipo de retorno.

El bean resultante tiene el mismo nombre que el _'factory method'_. Si se requiere que tenga un nombre diferente, se puede hacer con el nombre o los argumentos de valor de esta anotación (el valor del argumento es un alias para el nombre del argumento):

```java
@Configuration
public class AppConfiguration {
  @Bean("engine")
  public Engine getEngine() {
    return new Engine();
  }
}
```

Hay que tener en cuenta que **todos los métodos anotados con `@Bean` deben estar en clases `@Configuration`**.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html)

#### @Qualifier

Se usa la anotación `@Qualifier` junto con `@Autowired` para proporcionar la **identificación del bean** o el **nombre del bean** que se debe usar, sobretodo en situaciones ambiguas.

```java
class Bike implements Vehicle {}

class Car implements Vehicle {}
```

En caso de ambigüedad, se utiliza `@Qualifier` para indicar a Spring **el bean a inyectar**:

```java
// Using constructor injection
@Autowired
Biker(@Qualifier("bike") Vehicle vehicle) {
    this.vehicle = vehicle;
}

// Using setter injection
@Autowired
void setVehicle(@Qualifier("bike") Vehicle vehicle) {
    this.vehicle = vehicle;
}

@Autowired
@Qualifier("bike")
void setVehicle(Vehicle vehicle) {
    this.vehicle = vehicle;
}

// Using field injection
@Autowired
@Qualifier("bike")
Vehicle vehicle;
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html)

#### @Value

Se puede utilizar la anotación `@Value` para inyectar valores de propiedad en _beans_. Es compatible con **constructores**, **métodos _'setter'_** y con **campos**.

```java
// Constructor
Engine(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

Por supuesto, inyectar valores estáticos no es útil.

Podemos usar **cadenas _'placeholder'_** en `@Value` para conectar valores definidos en fuentes externas, por ejemplo, en archivos _'.properties'_ o _'.yaml'_.

Por ejemplo, un valor en un fichero externo _"application.properties"_ podría ser:

```text
catalog.name=MovieCatalog
```

Podemos inyectar este valor de esta forma.:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name}") String catalog) {
        this.catalog = catalog;
    }
}
```

Y con la siguiente configuración:

```java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig { }
```

Si la propiedad no está definida, se podría proporcionar un valor por defecto:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name:defaultCatalog}") String catalog) {
        this.catalog = catalog;
    }
}
```

##### Lenguaje de Expresiones SpEl

Cuando `@Value("#{expression}")` contiene una expresión SpEL, el valor se calculará dinámicamente en tiempo de ejecución.

```java
// Literal expressions
"#{'Hello World'}"  //strings
"#{3.1415926}"      //numeric values (double)
"#{true}"           //boolean
"#{null}"           //null

// Inline list
"#{1,2,3,4}"                //list of number
"#{'a','b'},{'x','y'}}"    //list of list

// Inline maps
"#{name:'Nikola',dob:'10-July-1856'}"
"#{name:{first:'Nikola',last:'Tesla'},dob:{day:10,month:'July',year:1856}}" //map of maps

// Invoking Methods
"#{'abc'.length()}" //evaluates to 3
"#{say('hello')}"   //'say' is a method in the class and it has a string parameter
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Value.html)

- [A Quick Guide to Spring @Value - Baeldung](https://www.baeldung.com/spring-value-annotation)

- [Using @Value - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/value-annotations.html)

- [Spring Expression Language (SpEL) - Spring Framework](https://docs.spring.io/spring-framework/reference/core/expressions.html)

#### @DependsOn

La anotación `@DependsOn` se puede utilizar para hacer que Spring **inicialice otros beans antes del anotado**. Normalmente, este comportamiento es automático y se basa en las dependencias explícitas entre beans. Spring, de forma predeterminada, gestiona el ciclo de vida de los beans y organiza su orden de inicialización.

Solo necesitamos esta anotación cuando **las dependencias están implícitas**, por ejemplo, carga del controlador JDBC o inicialización de variables estáticas.

Podemos usar `@DependsOn` en la clase dependiente especificando los nombres de los beans de dependencia. El argumento de valor de la anotación necesita una matriz que contenga los nombres de los beans de dependencia:

```java
@DependsOn("engine")
class Car implements Vehicle {}
```

Alternativamente, si se define un bean con la anotación `@Bean`, el _'factory method'_ debería anotarse con `@DependsOn`:

```java
@Bean
@DependsOn("fuel")
Engine engine() {
    return new Engine();
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/DependsOn.html)

#### @Lazy

La anotación `@Lazy` se utiliza cuando queremos inicializar un bean de forma diferida. De forma predeterminada, Spring crea todos los beans _singleton_ al inicio/arranque del contexto de la aplicación.

Sin embargo, hay casos en los que necesitamos crear un bean cuando se solicita, no al iniciar la aplicación.

Esta anotación tiene un argumento con el valor predeterminado de 'true'. Es útil para anular el comportamiento predeterminado.

Por ejemplo, marcar beans para que se carguen inmediatamente cuando la configuración global es diferida, o configurar métodos específicos de `@Bean` para carga inmediata en una clase `@Configuration` marcada con `@Lazy`:

```java
@Configuration
@Lazy
class VehicleFactoryConfig {

    @Bean
    @Lazy(false)
    Engine engine() {
        return new Engine();
    }
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Lazy.html)

- [A Quick Guide to the Spring @Lazy Annotation](https://www.baeldung.com/spring-lazy-annotation)

#### @Lookup

Un método anotado con `@Lookup` le indica a Spring que devuelva una instancia del tipo de retorno del método cuando lo invoquemos.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Lookup.html)

- [@Lookup Annotation in Spring](https://www.baeldung.com/spring-lookup)

#### @Primary

A veces necesitamos definir **múltiples beans del mismo tipo**. En estos casos, la inyección no tendrá éxito porque Spring no sabe qué bean necesitamos.

Ya vimos una opción para manejar este escenario: marcar todos los puntos de conexión con `@Qualifier` y especificar el nombre del bean requerido.

Sin embargo, la mayoría de las veces necesitamos un bean específico y rara vez los otros. Podemos usar `@Primary` para simplificar este caso: si marcamos el bean usado más frecuentemente con `@Primary`, será elegido en los puntos de inyección no calificados:

```java
@Component
@Primary
class Car implements Vehicle {}

@Component
class Bike implements Vehicle {}

@Component
class Driver {
    @Autowired
    Vehicle vehicle;
}

@Component
class Biker {
    @Autowired
    @Qualifier("bike")
    Vehicle vehicle;
}
```

En el ejemplo anterior, _'Car'_ es el vehículo principal. Por lo tanto, en la clase _'Driver'_, Spring inyecta un bean de tipo _'Car'_. Por supuesto, en el bean _'Biker'_, el valor del campo '_vehicle'_ será un objeto de tipo _'Bike'_ porque está calificado.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Primary.html)

#### @Scope

Usamos `@Scope` para definir el ámbito de una clase `@Component` o una definición de `@Bean`. Puede ser **_singleton_**, **_prototype_**, **_request_**, **_session_**, **_globalSession_** o algún ámbito personalizado.

```java
@Component
@Scope("prototype")
class Engine {}
```

**El ámbito por defecto es 'singleton'**. Esto significa que Spring crea una única instancia del bean y la reutiliza en toda la aplicación.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Scope.html)

### Context Configuration Annotations

#### @Profile

Si queremos que Spring use una clase `@Component` o un método `@Bean` solo cuando un perfil específico esté activo, podemos marcarlo con `@Profile`. Podemos configurar el nombre del perfil con el argumento de la anotación:

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle {}
```

Para activar un perfil, se puede hacer de varias maneras. La más común sería usando propiedades de configuración (_"application.properties"_ o _"application.yml"_). Se pueden definir varios perfiles separándolos con comas:

```txt
spring.profiles.active=dev,feature-x
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Profile.html)

- [Spring Profiles - Baeldung](https://www.baeldung.com/spring-profiles)

- [Environment Abstraction - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/environment.html)

- [Profiles - Spring Boot](https://docs.spring.io/spring-boot/reference/features/profiles.html)

#### @Import

La anotación `@Import` en Spring se utiliza para importar configuraciones adicionales a una configuración principal de la aplicación.

Si tenemos una clase anotada con `@Configuration` que define beans y configuraciones específicas para la aplicación, se puede usar `@Import` para incluir esta configuración en otra clase `@Configuration` principal:

```java
@Configuration
@Import(MyAdditionalConfig.class)
public class MainConfig {
    // Configuración principal de la aplicación
}
```

Otro uso de esta antoación es que podemos utilizar **clases específicas anotadas con `@Configuration` sin escaneo de componentes** con esta anotación. Podemos proporcionar esas clases con el argumento de la anotación. Esto es útil cuando se quiere tener control explícito sobre qué configuraciones y beans están disponibles en la aplicación:

```java
@Configuration
@Import({DataSourceConfig.class, SecurityConfig.class})
public class MainConfig {
    // Configuración principal de la aplicación
}
```

Por último, esta anotación sirve para importar clases no relacionadas con `@Configuration`, es decir, además de importar configuraciones de clases `@Configuration`, también se pueden importar otras clases que no estén anotadas con `@Configuration` pero que sean necesarios en la configuración principal.

```java
@Configuration
@Import({MyUtilityClass.class, AnotherHelper.class})
public class MainConfig {
    // Configuración principal de la aplicación
}
```

Aquí, _'MyUtilityClass'_ y _'AnotherHelper'_ son clases normales que no necesariamente son configuraciones de Spring.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Import.html)

#### @ImportResource

Podemos importar configuraciones XML con esta anotación. Podemos especificar las ubicaciones de los archivos XML utilizando un argumento en la anotación:

```java
@Configuration
@ImportResource("classpath:/annotations.xml")
class VehicleFactoryConfig {}
```

La anotación `@ImportResource` se utiliza exclusivamente para importar configuraciones desde archivos XML dentro del contexto de Spring.

En contraste, `@Import` se utiliza para importar configuraciones y componentes de otras clases de configuración, ya sean basadas en anotaciones o en XML, pero principalmente se usa con configuraciones basadas en anotaciones.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ImportResource.html)

#### @PropertySource

Con esta anotación, podemos definir archivos de propiedades ('.properties' o '.yml') para la configuración de la aplicación:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@Configuration
@PropertySource("classpath:config.properties")
public class AppConfig {
    // Configuración adicional de la aplicación
}
```

Estos archivos contienen configuraciones como URLs de bases de datos, rutas de archivos, configuraciones de conexión, etc.

Las propiedades cargadas se integran con el `Environment` de Spring, lo que permite acceder a ellas desde cualquier parte de la aplicación:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Component;

@Component
public class MyComponent {

    @Autowired
    private Environment env;

    public void someMethod() {
        String dbUrl = env.getProperty("database.url");
        // Utilizar la propiedad dbUrl...
    }
}
```

`@PropertySource` aprovecha la función de anotaciones repetidas de Java 8, lo que significa que podemos marcar una clase con ella varias veces:

```java
@Configuration
@PropertySource("classpath:/annotations.properties")
@PropertySource("classpath:/vehicle-factory.properties")
class VehicleFactoryConfig {}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/PropertySource.html)

#### @PropertySources

Podemos usar esta anotación para especificar múltiples configuraciones de `@PropertySource`:

```java
@Configuration
@PropertySources({ 
    @PropertySource("classpath:/annotations.properties"),
    @PropertySource("classpath:/vehicle-factory.properties")
})
class VehicleFactoryConfig {}
```

Tenga en cuenta que desde Java 8 se puede lograr el mismo resultado con la función de anotaciones repetidas.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/PropertySources.html)

---

## [Spring Web Annotations](https://www.baeldung.com/spring-mvc-annotations)

Estas anotaciones forman parte del paquete [org.springframework.web.bind.annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/package-summary.html).

- [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html)

### @RestController

La anotación `@RestController` es una meta-anotación que combina las anotaciones `@Controller`  y `@ResponseBody`.

```java
@Controller
@ResponseBody
class VehicleRestController {
    // ...
}
```

```java
@RestController
class VehicleRestController {
    // ...
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)

### @CrossOrigin

La anotación `@CrossOrigin` en Spring Framework es utilizada para configurar las políticas de intercambio de recursos entre diferentes dominios (CORS, por sus siglas en inglés - _"Cross-Origin Resource Sharing"_). CORS es un mecanismo de seguridad implementado por los navegadores web para restringir las solicitudes HTTP que se originan desde un dominio diferente al del servidor de destino.

La anotación `@CrossOrigin` habilita la comunicación entre dominios para los métodos del controlador de solicitudes anotados:

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @CrossOrigin(origins = "http://allowed-origin.com", methods = {RequestMethod.GET, RequestMethod.POST})
    @GetMapping("/data")
    public ResponseEntity<String> getData() {
        // Lógica para obtener datos
        return ResponseEntity.ok("Data fetched successfully");
    }
}
```

Si marcamos una clase con él, se aplica a todos los métodos del controlador de solicitudes que contiene. Podemos ajustar el comportamiento de CORS con los argumentos de esta anotación.

Se puede habilitar CORS globalmente para todos los controladores usando una clase de configuración como esta:

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://domain1.com", "https://domain2.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("header1", "header2")
                .exposedHeaders("header3", "header4")
                .allowCredentials(true)
                .maxAge(3600);
    }
}
```

Si se utiliza Spring Security en la aplicación, hay que asegurarse de que la configuración de CORS no entre en conflicto con las políticas de seguridad definidas en Spring Security.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CrossOrigin.html)

- [CORS with Spring](https://www.baeldung.com/spring-cors)

- [CORS](https://docs.spring.io/spring-framework/reference/web/webmvc-cors.html)

### Request Handling Annotations

#### @RequestMapping

En pocas palabras, `@RequestMapping` marca métodos manejadores de peticiones dentro de clases anotadas con `@Controller`; puede configurarse utilizando:

- **path (o sus alias name y value)**: indica a qué URL está mapeado el método.

- **method**: define los métodos HTTP compatibles.

- **params**: filtra las peticiones basándose en la presencia, ausencia o valor de parámetros HTTP.

- **headers**: filtra las peticiones basándose en la presencia, ausencia o valor de cabeceras HTTP.

- **consumes**: especifica los tipos de medios que el método puede consumir en el cuerpo de la petición HTTP.

- **produces**: especifica los tipos de medios que el método puede producir en el cuerpo de la respuesta HTTP.

```java
@Controller
class VehicleController {

    @RequestMapping(value = "/vehicles/home", method = RequestMethod.GET)
    String home() {
        return "home";
    }
}
```

Podemos proporcionar configuraciones predeterminadas para todos los métodos manejadores en una clase `@Controller` si aplicamos esta anotación a nivel de **clase**. La única excepción es la URL, que Spring no sobrescribirá con configuraciones a nivel de método, sino que **anexará las dos partes del path**.

Por ejemplo, la siguiente configuración tiene el mismo efecto que la configuración del ejemplo anterior:

```java
@Controller
@RequestMapping(value = "/vehicles", method = RequestMethod.GET)
class VehicleController {

    @RequestMapping("/home")
    String home() {
        return "home";
    }
}
```

Además, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` y `@PatchMapping` son variantes de `@RequestMapping` donde el método HTTP ya está configurado específicamente como GET, POST, PUT, DELETE y PATCH respectivamente.

Estas anotaciones están disponibles desde la **versión 4.3 de Spring**.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html)

- [Mapping Requests](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html)

#### @RequestBody

La anotación `@RequestBody` mapea el cuerpo de la solicitud HTTP a un objeto:

```java
@PostMapping("/save")
void saveVehicle(@RequestBody Vehicle vehicle) {
    // ...
}
```

La deserialización es automática y depende del tipo de contenido de la solicitud.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestBody.html)

#### @PathVariable

Esta anotación indica que un argumento de método está vinculado a una variable de plantilla URI. Si el nombre de la parte en la plantilla coincide con el nombre del argumento del método, no es necesario especificarlo en la anotación:

```java
import org.springframework.web.bind.annotation.*;

// "http://example.com/users/{userId}"
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{userId}")
    public String getUserById(@PathVariable Long userId) {
        return "Obteniendo usuario con ID: " + userId;
    }
}
```

Sin embargo, se puede vincular un argumento (o varios argumentos) de método a una de las partes de la plantilla con `@PathVariable` si se indica como argumento de la anotación.

```java
import org.springframework.web.bind.annotation.*;

// "http://example.com/products/{category}/{productId}"
@RestController
@RequestMapping("/products")
public class ProductController {

    @GetMapping("/{category}/{productId}")
    public String getProductDetails(
            @PathVariable("category") String category,
            @PathVariable("productId") Long productId) {
        return "Detalles del producto: categoría = " + category + ", ID = " + productId;
    }
}
```

Además, se puede marcar una variable de ruta como opcional estableciendo el argumento `required = false`:

```java
@RequestMapping("/{id}")
Vehicle getVehicle(@PathVariable(required = false) long id) {
    // ...
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/PathVariable.html)

#### @RequestParam

Se utilizas `@RequestParam` para acceder a los parámetros de solicitud HTTP:

```java
// "http://example.com/users?id=123"
@GetMapping("/users")
public String getUser(@RequestParam String id) {
    // El valor de id será "123"
    return "User ID: " + id;
}
```

Tiene las mismas opciones de configuración que la anotación `@PathVariable`.

Además de esas configuraciones, con `@RequestParam` podemos especificar un valor inyectado cuando Spring no encuentra ningún valor o está vacío en la solicitud. Para lograr esto, tenemos que establecer el argumento _"defaultValue"_:

```java
@RequestMapping("/buy")
Car buyCar(@RequestParam(defaultValue = "5") int seatCount) {
    // ...
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html)

- [@RequestParam](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/requestparam.html)

#### @ModelAttribute

La anotación `@ModelAttribute` en Spring MVC es utilizada para enlazar un método o parámetro de método a un atributo de modelo. Esta anotación es útil en el contexto de controladores de Spring MVC, donde se manejan las solicitudes HTTP y se prepara la respuesta.

Cuando se usa `@ModelAttribute` en un **parámetro de método**, Spring intenta enlazar los datos de la solicitud HTTP a ese objeto. Es útil para la recepción de datos del formulario o de la solicitud HTTP.

Con esta anotación podemos acceder a elementos que ya están en el modelo de un controlador MVC:

```java
// Se indica la clave del modelo en la anotación '@ModelAttribute'
@PostMapping("/assemble")
void assembleVehicle(@ModelAttribute("vehicle") Vehicle vehicleInModel) {
    // ...
}

// El nombre del argumento del método coincide con la clave del modelo
@PostMapping("/paint")
void paintVehicle(@ModelAttribute Vehicle vehicle) {
    // ...
}
```

Cuando se anota un método de un controlador con `@ModelAttribute`, ese método se invoca antes de que cualquier método anotado con `@RequestMapping` dentro del mismo controlador se ejecute. Esto permite preparar y agregar atributos al modelo que se utilizarán en la vista.

Spring agregará automáticamente el valor de retorno del método al modelo:

```java
// Se indica la clave del modelo en la anotación '@ModelAttribute'
@ModelAttribute("vehicle")
Vehicle getVehicle() {
    // ...
}

// El nombre del método coincide con la clave del modelo
@ModelAttribute
Vehicle vehicle() {
    // ...
}
```

En Spring MVC, para procesar correctamente los datos enviados desde un formulario HTML o una solicitud HTTP POST, es **esencial enlazar** esos datos a un objeto del modelo utilizando @ModelAttribute. Si no se realiza este enlace explícitamente, Spring no podrá asignar los datos del formulario a ningún objeto y, por lo tanto, no estarán disponibles para su uso en el controlador.

Al usar `@ModelAttribute`, Spring realiza automáticamente el enlace de datos entre los parámetros de solicitud y el objeto del modelo definido. Esto incluye la conversión de tipos de datos y la validación de los campos según las anotaciones de validación de Spring, como `@NotNull`, `@Size`, etc.

En el siguiente ejemplo, se muestra un formulario en Spring:

```html
<form:form method="POST" action="/spring-mvc-basics/addEmployee" 
  modelAttribute="employee">
    <form:label path="name">Name</form:label>
    <form:input path="name" />
    
    <form:label path="id">Id</form:label>
    <form:input path="id" />
    
    <input type="submit" value="Submit" />
</form:form>
```

En el controlador, se redirige a una vista JSP pero antes, se recuperan los datos enviados desde el formulario y se añaden al modelo:

```java
@Controller
@ControllerAdvice
public class EmployeeController {

    private Map<Long, Employee> employeeMap = new HashMap<>();

    @RequestMapping(value = "/addEmployee", method = RequestMethod.POST)
    public String submit(
      @ModelAttribute("employee") Employee employee,
      BindingResult result, ModelMap model) {
        if (result.hasErrors()) {
            return "error";
        }
        model.addAttribute("name", employee.getName());
        model.addAttribute("id", employee.getId());

        employeeMap.put(employee.getId(), employee);

        return "employeeView";
    }

    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("msg", "Welcome to the Netherlands!");
    }
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ModelAttribute.html)

- [Spring MVC and the @ModelAttribute Annotation](https://www.baeldung.com/spring-mvc-and-the-modelattribute-annotation)

- [@ModelAttribute](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/modelattrib-method-args.html)

- [Model](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-modelattrib-methods.html)

#### @CookieValue

La anotación `@CookieValue` en Spring Framework se utiliza para enlazar el valor de una cookie HTTP específica a un parámetro de método en un controlador de Spring MVC. Esto es útil cuando se necesita acceder al valor de una cookie específica enviada por el cliente en una solicitud HTTP.

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/getCookie")
    public ResponseEntity<String> getCookieValue(@CookieValue(name = "myCookie", defaultValue = "default") String cookieValue) {
        // Aquí puedes usar cookieValue, que contendrá el valor de la cookie "myCookie"
        return ResponseEntity.ok("Value of 'myCookie': " + cookieValue);
    }
}
```

Por defecto, se requiere la presencia de la cookie. En caso contrario se lanza la excepción `MissingCookieValueException`. Se puede indicar que no es requerida con el argumento `required=false` en la anotación.

Se puedes usar `@CookieValue` varias veces en un método de controlador para recuperar múltiples valores de cookies:

```java
@GetMapping("/getCookies")
public ResponseEntity<String> getCookies(@CookieValue(name = "cookie1", defaultValue = "default1") String cookie1,
                                        @CookieValue(name = "cookie2", defaultValue = "default2") String cookie2) {
    // Aquí puedes usar cookie1 y cookie2, que contendrán los valores de las cookies "cookie1" y "cookie2" respectivamente
    return ResponseEntity.ok("Value of 'cookie1': " + cookie1 + ", Value of 'cookie2': " + cookie2);
}

```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CookieValue.html)

- [@CookieValue](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/cookievalue.html)

#### @RequestHeader

La anotación `@RequestHeader` en Spring Framework se utiliza para enlazar el valor de una cabecera HTTP específica a un parámetro de método en un controlador de Spring MVC. Esto es útil cuando necesitas acceder a valores específicos de las cabeceras HTTP enviadas por el cliente en una solicitud.

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/getUserAgent")
    public ResponseEntity<String> getUserAgent(@RequestHeader("User-Agent") String userAgent) {
        // Aquí puedes usar userAgent, que contendrá el valor de la cabecera "User-Agent"
        return ResponseEntity.ok("User-Agent header value: " + userAgent);
    }
}
```

Por defecto, se requiere la presencia de la cabecera, lo que significa que Spring espera encontrar la cabecera especificada. Si no se encuentra, se generará una excepción `MissingRequestHeaderException` a menos que se especifique un valor predeterminado a falso.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestHeader.html)

- [@RequestHeader](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/requestheader.html)

### Response Handling Annotations

#### @ResponseBody

Si marcamos un método de controlador de solicitudes con `@ResponseBody`, Spring trata el resultado del método como la respuesta misma.

Es decir, cuando un método de controlador anotado con `@ResponseBody` se invoca y retorna un objeto, Spring convierte automáticamente ese objeto en un formato específico para la respuesta HTTP. La conversión del objeto a formato se realiza a través de un convertidor de medios (media converter), que depende de la configuración de Spring y de las bibliotecas disponibles en el classpath.

Es útil cuando se está construyendo una API RESTful y se desea retornar objetos como JSON o XML en lugar de vistas HTML.

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ExampleController {

    @GetMapping("/hello")
    @ResponseBody
    public String helloWorld() {
        return "Hello, World!";
    }

    @GetMapping("/user")
    @ResponseBody
    public User getUser(@RequestParam String username) {
        // Supongamos que aquí se obtiene un usuario de una base de datos
        User user = userRepository.findByUsername(username);
        return user;
    }
}
```

Si anotamos una clase @Controller con esta anotación, todos los métodos del controlador de solicitudes la usarán. Este es el efecto de `@RestController`, que no es más que una metaanotación marcada con `@Controller` y `@ResponseBody`

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseBody.html)

- [@ResponseBody](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/responsebody.html)

#### @ExceptionHandler

Con esta anotación, podemos declarar un método de manejo de errores personalizado. Spring llama a este método cuando un método de controlador de solicitudes genera cualquiera de las excepciones especificadas.

La excepción detectada se puede pasar al método como argumento:

```java
@ExceptionHandler(IllegalArgumentException.class)
void onIllegalArgumentException(IllegalArgumentException exception) {
    // ...
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ExceptionHandler.html)

- [Exceptions](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-exceptionhandler.html)

#### @ResponseStatus

Podemos especificar el código de estado HTTP deseado de la respuesta si anotamos un método manejador de solicitud con esta anotación. Podemos declarar el código de estado con el argumento `code`, o su alias, el argumento `value`.

```java
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ExampleController {

    @GetMapping("/hello")
    @ResponseStatus(HttpStatus.OK)
    public String helloWorld() {
        return "Hello, World!";
    }
}
```

Además, podemos proporcionar un motivo utilizando el argumento `reason`:

```java
@GetMapping("/notfound")
@ResponseStatus(value = HttpStatus.NOT_FOUND, reason = "Resource not found")
public void resourceNotFound() {
    // Method body
}
```

También podemos usarlo junto con `@ExceptionHandler`:

```java
@ExceptionHandler(IllegalArgumentException.class)
@ResponseStatus(HttpStatus.BAD_REQUEST)
void onIllegalArgumentException(IllegalArgumentException exception) {
    // ...
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseStatus.html)

- [Returning Custom Status Codes from Spring Controllers](https://www.baeldung.com/spring-mvc-controller-custom-http-status-code)

## [Spring Boot Annotations](https://www.baeldung.com/spring-boot-annotations)

Spring Boot facilita la configuración de Spring con su función de configuración automática.

Estas anotaciones forman parte del paquete [org.springframework.boot.autoconfigure](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/package-summary.html) y [org.springframework.boot.autoconfigure.condition](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/package-summary.html).

### @SpringBootApplication

Esta anotación se utiliza para marcar la clase principal de una aplicación Spring Boot:

```java
@SpringBootApplication
class VehicleFactoryApplication {

    public static void main(String[] args) {
        SpringApplication.run(VehicleFactoryApplication.class, args);
    }
}
```

Esta anotación es una meta-anotación de las anotaciones `@Configuration`, `@EnableAutoConfiguration` y `@ComponentScan` con sus atributos predeterminados.

- [Javadoc](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/SpringBootApplication.html)

- [SpringApplication - Spring Boot](https://docs.spring.io/spring-boot/reference/features/spring-application.html)

### @EnableAutoConfiguration

Esta anotación `@EnableAutoConfiguration`, como su nombre indica, permite habilitar la configuración automática. Significa que Spring Boot busca _beans_ de configuración automática en su classpath y los aplica automáticamente.

Hay que tener en cuenta que hay que usar esta anotación con `@Configuration`:

```java
@Configuration
@EnableAutoConfiguration
class VehicleFactoryConfig {}
```

Por otro lado, la anotación `@SpringBootApplication` es una meta-anotación de las anotaciones `@Configuration`, `@EnableAutoConfiguration` y `@ComponentScan` con sus atributos predeterminados.

Por lo tanto, si se utiliza `@SpringBootApplication` no es necesario utilizar la anotación `@EnableAutoConfiguration`.

- [Javadoc](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/EnableAutoConfiguration.html)

- [Auto-configuration - Spring Boot](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)

### Auto-Configuration Conditions

En el contexto de Spring Framework, las condiciones de auto-configuración (`@Conditional`) permiten condicionar la aplicación de configuraciones basadas en ciertas condiciones en tiempo de ejecución.

Estas condiciones se utilizan ampliamente en la auto-configuración automática de Spring Boot y en la configuración personalizada de aplicaciones Spring ya que permiten una configuración modular y flexible de aplicaciones Spring, adaptándose dinámicamente según el entorno y las condiciones del sistema.

Spring proporciona diversas condiciones predefinidas que listas para utilizar.

Estas anotaciones de configuración se pueden colocar en clases `@Configuration` o métodos `@Bean`.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Conditional.html)

- [Create a Custom Auto-Configuration with Spring Boot](https://www.baeldung.com/spring-boot-custom-auto-configuration)

- [Creating Your Own Auto-configuration](https://docs.spring.io/spring-boot/reference/features/developing-auto-configuration.html)

#### @Conditional

La anotación `@Conditional` se utiliza para aplicar una condición a un componente de Spring, como un bean o una configuración, para que solo se active si la condición especificada se cumple en tiempo de ejecución:

```java
@Configuration
@Conditional(OnProductionEnvironmentCondition.class)
public class ProductionConfiguration {
    // Configuración específica para el entorno de producción
}
```

#### @ConditionalOnClass and @ConditionalOnMissingClass

Usando estas condiciones, Spring solo usará el bean de configuración automática marcado si la clase en el argumento de la anotación está presente/ausente:

```java
@Configuration
@ConditionalOnClass(DataSource.class)
class MySQLAutoconfiguration {
    //...
}
```

#### @ConditionalOnBean and @ConditionalOnMissingBean

Podemos usar estas anotaciones cuando queramos definir condiciones basadas en la presencia o ausencia de un bean específico:

```java
@Bean
@ConditionalOnBean(name = "dataSource")
LocalContainerEntityManagerFactoryBean entityManagerFactory() {
    // ...
}
```

#### @ConditionalOnProperty

Con esta anotación, podemos poner condiciones sobre los valores de las propiedades:

```java
@Bean
@ConditionalOnProperty(
    name = "usemysql", 
    havingValue = "local"
)
DataSource dataSource() {
    // ...
}
```

#### @ConditionalOnResource

Podemos hacer que Spring use una definición solo cuando un recurso específico esté presente:

```java
@ConditionalOnResource(resources = "classpath:mysql.properties")
Properties additionalProperties() {
    // ...
}
```

#### @ConditionalOnWebApplication and @ConditionalOnNotWebApplication

Con estas anotaciones podemos crear condiciones en función de si la aplicación actual es o no una aplicación web:

```java
@ConditionalOnWebApplication
HealthCheckController healthCheckController() {
    // ...
}
```

#### @ConditionalExpression

Podemos utilizar esta anotación en situaciones más complejas. Spring usará la definición marcada cuando la expresión SpEL se evalúe como verdadera:

```java
@Bean
@ConditionalOnExpression("${usemysql} && ${mysqlserver == 'local'}")
DataSource dataSource() {
    // ...
}
```

## [Spring Scheduling Annotations](https://www.baeldung.com/spring-scheduling-annotations)

Cuando la ejecución de un solo hilo no es suficiente, podemos usar anotaciones del paquete [org.springframework.scheduling.annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html).

- [Task Execution and Scheduling](https://docs.spring.io/spring-framework/reference/integration/scheduling.html#scheduling-annotation-support)

### @EnableAsync

Con esta anotación, podemos habilitar la funcionalidad asíncrona en Spring.

Debemos usarla junto con `@Configuration`:

```java
@Configuration
@EnableAsync
class VehicleFactoryConfig {}
```

Ahora que habilitamos las llamadas asincrónicas, podemos usar `@Async` para definir los métodos que las admiten.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/EnableAsync.html)

### @EnableScheduling

Con esta anotación, podemos habilitar la programación en la aplicación.

Debemos usarla junto con `@Configuration`:

```java
@Configuration
@EnableScheduling
class VehicleFactoryConfig {}
```

Como resultado, ahora podemos ejecutar métodos periódicamente con `@Scheduled`.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/EnableScheduling.html)

### @Async

La anotación `@Async` en Spring se utiliza para marcar un método como **asíncrono**, lo que permite que dicho método sea ejecutado en un hilo separado o en un pool de hilos dedicado administrado por Spring. Esto es útil para operaciones que no necesitan bloquear el hilo principal de ejecución, como tareas de larga duración, operaciones de E/S intensivas, o llamadas a servicios externos.

Para lograr esto, podemos anotar el método con `@Async`:

```java
@Async
void repairCar() {
    // ...
}
```

Si aplicamos esta anotación a una clase, todos los métodos se llamarán de forma asincrónica.

Para que `@Async` funcione correctamente, la clase que contiene el método anotado debe ser administrada por Spring (normalmente usando `@Service`, `@Component`, o `@Repository`)

Tenga en cuenta que debemos habilitar las llamadas asincrónicas para que funcione esta anotación, con `@EnableAsync` o configuración XML.

Un método anotado con `@Async` puede devolver un valor envuelto en un _"Future"_ si se necesita obtener el resultado de la ejecución asíncrona:

```java
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;
import java.util.concurrent.Future;

@Service
public class MyService {

    @Async
    public Future<String> asyncMethodWithReturn() {
        // Método que retorna un resultado de manera asíncrona
        return new AsyncResult<>("Resultado asíncrono");
    }
}
```java
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;
import java.util.concurrent.Future;

@Service
public class MyService {

    @Async
    public Future<String> asyncMethodWithReturn() {
        // Método que retorna un resultado de manera asíncrona
        return new AsyncResult<>("Resultado asíncrono");
    }
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Async.html)

- [How To Do @Async in Spring](https://www.baeldung.com/spring-async)

### @Scheduled

La anotación `@Scheduled` en Spring se utiliza para programar la ejecución de métodos a intervalos específicos, en momentos particulares o según expresiones cron. Esta anotación es útil para tareas recurrentes y automatización de procesos dentro de una aplicación Spring.

Si necesitamos que un método se ejecute periódicamente, podemos usar esta anotación:

```java
@Scheduled(fixedRate = 10000)
void checkVehicle() {
    // ...
}
```

Podemos usarlo para ejecutar un método a **intervalos fijos**, o podemos ajustarlo con **expresiones similares a cron**:

- **'fixedRate'**: ejecuta el método con una tasa fija en milisegundos, independientemente de cuánto tiempo haya tomado la ejecución anterior.

- **'fixedDelay'**: ejecuta el método con un retraso fijo en milisegundos después de que se completa la ejecución anterior.

- **'initialDelay'**: especifica un retraso inicial en milisegundos antes de la primera ejecución del método.

- **'cron'**: permite una expresión cron para definir horarios más complejos y específicos.

`@Scheduled` aprovecha la función de anotaciones repetidas de Java 8, lo que significa que podemos marcar un método con ella varias veces:

```java
@Scheduled(fixedRate = 10000)
@Scheduled(cron = "0 * * * * MON-FRI")
void checkVehicle() {
    // ...
}
```

Tenga en cuenta que el método anotado con `@Scheduled` debe tener un tipo de retorno nulo.

Además, debemos habilitar la programación para que esta anotación funcione, por ejemplo, con `@EnableScheduling` o la configuración XML.

Para que `@Scheduled` funcione correctamente, la clase que contiene el método anotado debe ser administrada por Spring (normalmente utilizando `@Component`, `@Service`, o similar).

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html)

- [The @Scheduled Annotation in Spring](https://www.baeldung.com/spring-scheduled-tasks)

### @Schedules

Podemos usar esta anotación para especificar múltiples reglas `@Scheduled`:

```java
@Schedules({ 
  @Scheduled(fixedRate = 10000), 
  @Scheduled(cron = "0 * * * * MON-FRI")
})
void checkVehicle() {
    // ...
}
```

Hay que tener en cuenta que desde Java 8 se puede lograr lo mismo con la función de anotaciones repetidas.

## [Spring Data Annotations](https://www.baeldung.com/spring-data-annotations)

**Spring Data** proporciona una abstracción sobre las tecnologías de almacenamiento de datos. Por lo tanto, nuestro código de lógica de negocios puede ser mucho más independiente de la implementación de persistencia subyacente. Además, Spring simplifica el manejo de los detalles del almacenamiento de datos que dependen de la implementación.

- [Javadoc](https://docs.spring.io/spring-data/commons/docs/current/api/)

- [Data Access](https://docs.spring.io/spring-framework/reference/data-access.html)

### Common Spring Data Annotations

#### @Transactional

La anotación `@Transactional` en Spring se utiliza para administrar transacciones en métodos o clases de manera declarativa. Esta anotación permite que los métodos anotados se ejecuten dentro de una transacción gestionada por Spring, asegurando la atomicidad, consistencia, aislamiento y durabilidad (ACID) de las operaciones realizadas en la base de datos u otros recursos transaccionales.

Cuando queramos configurar el comportamiento transaccional de un método, podemos hacerlo con:

```java
@Transactional
void pay() {}
```

Si aplicamos esta anotación a nivel de clase, funciona en todos los métodos dentro de la clase. Sin embargo, podemos anular sus efectos aplicándolo a un método específico.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)

- [Transactions with Spring and JPA](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)

#### @NoRepositoryBean

La anotación `@NoRepositoryBean` en Spring se utiliza para indicar a Spring que una interfaz de repositorio específica no debe ser considerada como un repositorio gestionado por **Spring Data**. Esto significa que no se creará una instancia de Spring para esa interfaz de repositorio en particular, y por lo tanto, no se aplicarán las características de repositorio típicas como la generación automática de consultas y métodos CRUD.

```java
import org.springframework.data.repository.NoRepositoryBean;
import org.springframework.data.repository.Repository;

@NoRepositoryBean
public interface MyBaseRepository<T, ID> extends Repository<T, ID> {
    // Métodos personalizados que no son generados automáticamente por Spring Data
}

@Repository
public interface UserRepository extends MyBaseRepository<User, Long> {
    // Spring Data generará automáticamente métodos CRUD para la entidad User
}
```

La anotación `@NoRepositoryBean` es útil cuando se requiere definir una interfaz base para repositorios que contengan métodos comunes o personalizados, pero que no represente un repositorio que deba ser instanciado directamente por **Spring Data**.

- [Javadoc](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/NoRepositoryBean.html)

#### @Param

Podemos pasar parámetros con nombre a nuestras consultas usando `@Param`:

```java
@Query("FROM Person p WHERE p.name = :name")
Person findByName(@Param("name") String name);
```

Tenga en cuenta que nos referimos al parámetro con la sintaxis `:name`.

- [Javadoc](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/query/Param.html)

- [Spring Data JPA @Query](https://www.baeldung.com/spring-data-jpa-query)

#### @Id

La anotación `@Id` marca un campo en una clase de modelo como **clave principal**:

```java
class Person {

    @Id
    Long id;

    // ...
    
}
```

Dado que es independiente de la implementación, hace que una clase de modelo sea fácil de usar con múltiples motores de almacenamiento de datos.

- [Javadoc](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/Id.html)

#### @Transient

Podemos usar esta anotación para marcar un campo en una clase de modelo como **transitorio**. Por lo tanto, el motor del almacén de datos no leerá ni escribirá el valor de este campo:

```java
class Person {

    // ...

    @Transient
    int age;

    // ...

}
```

Al igual que `@Id`, `@Transient` también es independiente de la implementación, lo que hace que sea conveniente utilizarlo con múltiples implementaciones de almacenes de datos.

- [Javadoc](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/Transient.html)

#### Campos de auditoría

En Spring Data, las anotaciones de campos de auditoría son utilizadas para mantener un rastro automático de ciertas acciones comunes, como la creación y modificación de entidades. Estas anotaciones permiten que Spring Data gestione automáticamente campos como el usuario que creó la entidad, el usuario que la modificó, y las fechas de creación y modificación:

- **`@CreatedBy`**: se utiliza para marcar un campo que debe contener el usuario que creó la entidad. Este campo se llena automáticamente cuando la entidad se persiste por primera vez.

- **`@LastModifiedBy`**: indica el usuario que realizó la última modificación a la entidad. Este campo se actualiza automáticamente cada vez que la entidad se modifica.

- **`@CreatedDate`**: marca un campo para almacenar la fecha y hora en que la entidad fue creada. Este campo se llena automáticamente cuando la entidad se persiste por primera vez.

- **`@LastModifiedDate`**: se utiliza para anotar un campo que debe contener la fecha y hora de la última modificación de la entidad. Este campo se actualiza automáticamente cada vez que la entidad se modifica.

```java
public class Person {

    // ...

    @CreatedBy
    User creator;
    
    @LastModifiedBy
    User modifier;
    
    @CreatedDate
    Date createdAt;
    
    @LastModifiedDate
    Date modifiedAt;

    // ...

}
```

Para utilizar estas anotaciones, se debe seguir algunos pasos adicionales para habilitar la auditoría en una aplicación Spring:

- Añadir la anotación `@EnableJpaAuditing` en una clase de configuración de Spring para habilitar la auditoría:

```java
@Configuration
@EnableJpaAuditing(auditorAwareRef = "auditorProvider")
public class AuditConfig {
}
```

- Implementar la interfaz `AuditorAware` para especificar cómo se obtiene el usuario actual:

```java
@Component
public class AuditorAwareImpl implements AuditorAware<String> {

    @Override
    public Optional<String> getCurrentAuditor() {
        // Lógica para obtener el usuario actual, por ejemplo, del contexto de seguridad de Spring
        return Optional.of(SecurityContextHolder.getContext().getAuthentication().getName());
    }
}
```

- [Javadoc](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/package-summary.html)

- [Auditing with JPA, Hibernate, and Spring Data JPA](https://www.baeldung.com/database-auditing-jpa)

- [Auditing](https://docs.spring.io/spring-data/jpa/reference/auditing.html)

### Spring Data JPA Annotations

#### @Query

Con la anotación `@Query`, se proporciona proporcionar una **implementación JPQL** para un método de repositorio:

```java
@Query("SELECT COUNT(*) FROM Person p")
long getPersonCount();
```

Además, se puede utilizar parámetros con nombre con la anotación `@Param`:

```java
@Query("FROM Person p WHERE p.name = :name")
Person findByName(@Param("name") String name);
```

Spring se pueden utilizar consultas SQL nativas, si se configura el argumento `nativeQuery = true`:

```java
@Query(value = "SELECT AVG(p.age) FROM person p", nativeQuery = true)
int getAverageAge();
```

- [Spring Data JPA @Query](https://www.baeldung.com/spring-data-jpa-query)

#### @Procedure

Con Spring Data JPA podemos llamar fácilmente a procedimientos almacenados desde repositorios.

Primero, se necesita declarar el repositorio en la clase de entidad usando anotaciones JPA estándar:

```java
@NamedStoredProcedureQueries({ 
    @NamedStoredProcedureQuery(
        name = "count_by_name", 
        procedureName = "person.count_by_name", 
        parameters = { 
            @StoredProcedureParameter(
                mode = ParameterMode.IN, 
                name = "name", 
                type = String.class),
            @StoredProcedureParameter(
                mode = ParameterMode.OUT, 
                name = "count", 
                type = Long.class) 
            }
    ) 
})

class Person {}
```

Después de esto, se puedes hacer referencia a él en el repositorio con el nombre que se declaramos en el argumento `name`:

```java
@Procedure(name = "count_by_name")
long getCountByName(@Param("name") String name);
```

#### @Lock

La anotación `@Lock` en Spring es parte del módulo **Spring Data JPA** y se utiliza para especificar el nivel de bloqueo que se debe aplicar a una consulta JPA en las operaciones de acceso a la base de datos. Esta anotación es útil para controlar la concurrencia y evitar problemas como condiciones de carrera, especialmente en entornos donde múltiples transacciones pueden intentar acceder o modificar los mismos datos simultáneamente.

La anotación `@Lock` admite varios tipos principales de bloqueo:

- **`LockModeType.NONE`**: no se aplica ningún bloqueo. Este es el **modo predeterminado** si no se especifica un tipo de bloqueo.

- **`LockModeType.PESSIMISTIC_READ`: este bloqueo permite que otras transacciones lean los datos, pero no permite actualizaciones. Es útil para evitar que los datos cambien mientras se está leyendo.

- **`LockModeType.PESSIMISTIC_WRITE`**: este bloqueo evita que otras transacciones lean o actualicen los datos mientras se mantiene el bloqueo. Es útil cuando se necesita asegurarse de que nadie más pueda leer o modificar los datos hasta que la transacción actual termine.

- **`LockModeType.PESSIMISTIC_FORCE_INCREMENT`**: adquiere un bloqueo de escritura pesimista y además incrementa la versión de la entidad. Asegura que las transacciones concurrentes vean la nueva versión de la entidad.

- **`LockModeType.OPTIMISTIC`**: utiliza el bloqueo optimista, que se basa en versiones. Permite que múltiples transacciones lean y actualicen los datos, pero verifica al final de la transacción si los datos han cambiado desde la última lectura. Si es así, lanza una excepción `OptimisticLockException`.

- **`LockModeType.OPTIMISTIC_FORCE_INCREMENT`**: similar al bloqueo optimista, pero además incrementa la versión de la entidad. Es útil para asegurar que cualquier otra transacción que quiera leer los datos tendrá que esperar hasta que la transacción actual complete.

- **`LockModeType.READ`: sinónimo de `OPTIMISTIC`. Poco usado en la práctica.

- **`LockModeType.WRITE`: sinónimo de `OPTIMISTIC_FORCE_INCREMENT`. Poco usado en la práctica.

Los **bloqueos pesimistas** pueden afectar el rendimiento de la base de datos, ya que impiden que otras transacciones accedan a los datos bloqueados.

Los **bloqueos optimistas** son más apropiados para sistemas con alta concurrencia, ya que permiten una mayor escalabilidad al no bloquear los datos de manera exclusiva.

- [Javadoc](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Lock.html)

- [Javadoc](https://jakarta.ee/specifications/persistence/2.2/apidocs/javax/persistence/lockmodetype)

- [Enabling Transaction Locks in Spring Data JPA](https://www.baeldung.com/java-jpa-transaction-locks)

- [Locking](https://docs.spring.io/spring-data/jpa/reference/jpa/locking.htm)

#### @Modifying

Se pueden modificar datos con un método de repositorio si se anota con `@Modifying`:

```java
@Modifying
@Query("UPDATE Person p SET p.name = :name WHERE p.id = :id")
void changeName(@Param("id") long id, @Param("name") String name);
```

- [Spring Data JPA @Query](https://www.baeldung.com/spring-data-jpa-query)

#### @EnableJpaRepositories

Para utilizar repositorios JPA, se le tiene que indicar a Spring mediante la anotación `@EnableJpaRepositories`.

Hay que tener en cuenta que se debe usar esta anotación con la anotación `@Configuration`:

```java
@Configuration
@EnableJpaRepositories
class PersistenceJPAConfig {}
```

Spring buscará repositorios en los subpaquetes de esta clase `@Configuration`. Se puede alterar este comportamiento con el argumento `basePackages`:

```java
@Configuration
@EnableJpaRepositories(basePackages = "com.baeldung.persistence.dao")
class PersistenceJPAConfig {}
```

También hay que tener en cuenta que Spring Boot hace esto automáticamente si encuentra **Spring Data JPA en el classpath**.

### Spring Data Mongo Annotations

Spring Data hace que trabajar con MongoDB sea mucho más fácil.

- [Introduction to Spring Data MongoDB](https://www.baeldung.com/spring-data-mongodb-tutorial)

#### @Document

Esta anotación marca una clase como un objeto de dominio que queremos conservar en la base de datos:

```java
@Document
class User {}
```

También nos permite elegir el nombre de la colección que queremos utilizar:

```java
@Document(collection = "user")
class User {}
```

Esta anotación `@Document` es el equivalente en Mongo de `@Entity` en JPA.

#### @Field

Con la anotación `@Field`, podemos configurar el nombre de un campo que queremos usar cuando MongoDB persiste el documento:

```java
@Document
class User {

    // ...

    @Field("email")
    String emailAddress;

    // ...

}
```

Esta anotación `@Field` es el equivalente en Mongo de `@Column` en JPA.

#### @Query

Con la anotación `@Query`, podemos proporcionar una consulta de buscador en un método de repositorio de MongoDB:

```java
@Query("{ 'name' : ?0 }")
List<User> findUsersByName(String name);
```

#### @EnableMongoRepositories

Para utilizar repositorios de MongoDB, tenemos que indicárselo a Spring. Podemos hacer esto con `@EnableMongoRepositories`. Esta anotación se tiene que utilizar con la anotación `@Configuration`:

```java
@Configuration
@EnableMongoRepositories
class MongoConfig {}
```

Spring buscará repositorios en los subpaquetes de esta clase `@Configuration`. Podemos alterar este comportamiento con el argumento `basePackages`:

```java
@Configuration
@EnableMongoRepositories(basePackages = "com.baeldung.repository")
class MongoConfig {}
```

Spring Boot hace esto automáticamente si encuentra **Spring Data MongoDB en el classpath**.

## [Spring Bean Annotations](https://www.baeldung.com/spring-bean-annotations)

El contenedor de Spring se encarga de crear los beans en la aplicación y de coordinar las relaciones entre estos objetos a través de la DI.

A la hora de expresar una especificación de conexión de bean, Spring ofrece tres mecanismos principales:

- **Configuración explícita en XML**: los _beans_ se declaran en un fichero de configuración `.xml`.

- **Configuración explícita en Java (JavaConfig)**: los _beans_ se declaran usando la anotación `@Bean` en una clase de configuración anotada con `@Configuration`.

- **Análisis y detección implícita de beans y conexión automática de beans**: finalmente, podemos marcar la clase como componentes gestionados por el contenedor de Spring con una de las anotaciones del paquete [org.springframework.stereotype](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/package-summary.html) y dejar el trabajo para el escaneo de componentes mediante `@ComponentScan`.

Todos los mecanismos son compatibles entre sí y no excluyentes, es decir, dos mecanismos pueden ser utilizados en la misma aplicación.

La recomendación es usar la **conexión automática** ya que requiere menos configuración explícita y además usar la **configuración explícita** en el caso de usar librerías y frameworks de terceros para conectar sus componentes porque en estos casos **no se dispone de acceso al código fuente** para anotar las clases con `@Component` o `@Service` o cualquier otra.

### @ComponentScan

Spring puede **escanear automáticamente** un paquete en busca de _beans_ si el [escaneo de componentes está habilitado](#enableautoconfiguration).

Esta anotación `@ComponentScan` se utiliza junto a la anotación [`@Configuration`](#configuration).

La anotación `@ComponentScan` configura qué paquetes escanear en busca de _beans_. Podemos especificar los nombres de los **paquetes** base directamente con uno de los argumentos "_basePackages_" o _"value"_ (alias para _"basePackages"_):

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
class VehicleFactoryConfig {}
```

Además, podemos indicar **clases** en los paquetes base con el argumento _"basePackageClasses"_, lo que mejora la seguridad de tipos. Este método proporciona una ventaja ya que en el caso de refactorizar nombres de clases, no habrá incosistencias, mientras que si se usa una cadena como "com.example.myapplication", apuntará a clases que han cambiado de nobmre:

```java
@Configuration
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

Ambos argumentos son matrices, por lo que podemos proporcionar varios paquetes o clases para cada uno.

```java
@Configuration
@ComponentScan(basePackageClasses = {VehicleFactoryConfig.class, OtherFactoryConfig.class})
class VehicleFactoryConfig {}
```

Si no se especifica ningún argumento, el escaneo se realiza desde el mismo paquete donde está presente la clase anotada con `@ComponentScan`. Generalmente se recomienda que se ubique la clase de aplicación principal en un paquete raíz encima de otras clases. Normalmente, la clase anotada con `@SpringBootApplication` se considera la clase principal.

En este ejemplo, la clase principal se ubica en _"com.example.myapplication"_ por lo que ese se considera el paquete raíz. Si no se indica un paquete base en `@ComponentScan` se utilizará ese paquete raíz como base. Por tanto el escaneo se hará a partir de ese paquete e incluirá _"com.example.myapplication.customer"_, _"com.example.myapplication.order"_, etcétera...:

```txt
com
 +- example
     +- myapplication
         +- MyApplication.java
         |
         +- customer
         |   +- Customer.java
         |   +- CustomerController.java
         |   +- CustomerService.java
         |   +- CustomerRepository.java
         |
         +- order
             +- Order.java
             +- OrderController.java
             +- OrderService.java
             +- OrderRepository.java
```

La anotación `@ComponentScan` aprovecha la función de anotaciones repetidas de Java 8, lo que significa que podemos marcar una clase con ella varias veces:

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

Como alternativa, se puede usar la anotación `@ComponentScans` para especificar múltiples configuraciones de `@ComponentScan`:

```java
@Configuration
@ComponentScans({ 
  @ComponentScan(basePackages = "com.baeldung.annotations"), 
  @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
})
class VehicleFactoryConfig {}
```

- [Javadoc - `@ComponentScan`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html)

- [Javadoc - `@ComponentScans`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScans.html)

- [Spring Component Scanning - Baeldung](https://www.baeldung.com/spring-component-scanning)

- [Structuring Your Code - Spring Boot](https://docs.spring.io/spring-boot/reference/using/structuring-your-code.html)

### @Component

La anotación `@Component` es una anotación de **nivel de clase**. Durante el [análisis de componentes con `@ComponentScan`](#componentscan), Spring Framework detecta automáticamente las clases anotadas con `@Component`:

```java
@Component
class CarUtility {
    // ...
}
```

De forma predeterminada, las instancias de _bean_ de esta clase tienen **el mismo nombre que el nombre de la clase** con una inicial en minúscula. Además, podemos especificar un nombre diferente utilizando el argumento _"value"_ opcional de esta anotación.

```java
import org.springframework.stereotype.Component;

@Component("customServiceName")
public class MyService {
    public void performService() {
        System.out.println("Service is being performed");
    }
}
```

Otra forma de asignar nombres a un _bean_ implica descartar la anotación `@Component` y, en su lugar, usar la anotación `@Named` de la especificación de inyección de dependencias de Java **JSR-330** para proporcionar un nombre al _bean_:

```java
import javax.inject.Named;

@Named("customServiceName")
public class MyService {
    public void performService() {
        System.out.println("Service is being performed");
    }
}
```

Dado que `@Repository`, `@Service`, `@Configuration` y `@Controller` son **metanotaciones** de `@Component`, comparten el mismo comportamiento de denominación de _beans_. Spring también los detecta automáticamente durante el proceso de escaneo de componentes.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html)

- [Using JSR 330 Standard Annotations - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/standard-annotations.html)

### @Repository

Las clases DAO o _Repository_ generalmente representan la capa de acceso a la base de datos en una aplicación y deben anotarse con `@Repository`:

```java
@Repository
class VehicleRepository {
    // ...
}
```

Una ventaja de utilizar esta anotación es que tiene habilitada la **traducción automática de excepciones de persistencia**.

Cuando se utiliza un marco de persistencia, como Hibernate, las excepciones nativas lanzadas dentro de las clases anotadas con `@Repository` se traducirán automáticamente a subclases de `DataAccessExeption` de Spring.

Para habilitar la traducción de excepciones, necesitamos declarar nuestro propio bean _"PersistenceExceptionTranslationPostProcessor"_:

```java
@Bean
public PersistenceExceptionTranslationPostProcessor exceptionTranslation() {
    return new PersistenceExceptionTranslationPostProcessor();
}
```

Tenga en cuenta que en la mayoría de los casos, Spring realiza el paso anterior automáticamente.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Repository.html)

### @Service

La **lógica de negocio** de una aplicación normalmente reside dentro de **la capa de servicio**, por lo que usaremos la anotación `@Service` para indicar que una clase pertenece a esa capa:

```java
@Service
public class VehicleService {
    // ...    
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Service.html)

### @Controller

La anotación `@Controller` es una anotación a **nivel de clase**, que le dice a Spring Framework que esta clase sirve como controlador en Spring MVC:

```java
@Controller
public class VehicleController {
    // ...
}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Controller.html)

### @Configuration

Las clases de configuración pueden contener **métodos de definición de _beans_** anotados con `@Bean`:

```java
@Configuration
class VehicleFactoryConfig {

    @Bean
    Engine engine() {
        return new Engine();
    }

}
```

Esta anotación `@Configuration` es una **meta-anotación** o especialización de `@Component` que se utiliza para definir una clase de configuración en Spring. Las clases anotadas con `@Configuration` son utilizadas para definir _beans_ en el contexto de la aplicación y además, son consideradas en sí mismas como _beans_.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)

---

## Referencias

- <https://spring.io/>
- <https://docs.spring.io/spring-framework/reference/>
- <https://docs.spring.io/spring-boot/>

### Guías (Baeldung)

- [Spring Framework Introduction](https://www.baeldung.com/spring-intro)
- [Learn Spring Boot](https://www.baeldung.com/spring-boot)
- [Spring Dependency Injection](https://www.baeldung.com/spring-dependency-injection)
- [Spring MVC Guides](https://www.baeldung.com/spring-mvc)
- [REST with Spring Tutorial](https://www.baeldung.com/rest-with-spring-series)
- [Security with Spring](https://www.baeldung.com/security-spring)
- [All Spring Data Guides](https://www.baeldung.com/spring-data)

## Licencia

[![Licencia de Creative Commons](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)
Esta obra está bajo una [licencia de Creative Commons Reconocimiento-Compartir Igual 4.0 Internacional](http://creativecommons.org/licenses/by-sa/4.0/).
