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

### @Autowired

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

A partir de la versión 4.3, no es necesario anotar constructores con `@Autowired` de forma explícita a menos que se haya declarado al menos dos constructores.

```java
// Setter injection
class Car {
  Engine engine;

  @Autowired
  void setEngine(Engine engine) {
    this.engine = engine;
  }
}
``

```java`
// Field injection
class Car {
  @Autowired
  Engine engine;
}
```

`@Autowired` tiene un argumento booleano llamado `required` con un valor predeterminado de `true`. Este argumento ajusta el comportamiento de Spring cuando no encuentra un bean adecuado para conectar. Cuando es verdadero, se lanzará una excepción; de lo contrario, no se conecta nada.

Si se utiliza la inyección del constructor, **todos los argumentos del constructor son obligatorios**.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)

- [Guide to Spring @Autowired](https://www.baeldung.com/spring-autowire)

- [Constructor Dependency Injection in Spring](https://www.baeldung.com/constructor-injection-in-spring)

### @Bean

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

### @Qualifier

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

### @Value

Se puede utilizar la anotación `@Value` para inyectar valores de propiedad en beans. Es compatible con constructorres, métodos _'setter'_ y con campos.

```java
// Constructor injection
Engine(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

Por supuesto, inyectar valores estáticos no es útil. Por lo tanto, podemos usar **cadenas _'placeholder'_** en `@Value` para conectar valores definidos en fuentes externas, por ejemplo, en archivos _'.properties'_ o _'.yaml'_.

Por ejemplo, un valor en un fichero externo podría ser:

```text
engine.fuelType=petrol
```

Podemos inyectar el valor de esta forma:

```java
@Value("${engine.fuelType}")
String fuelType;
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Value.html)

- [A Quick Guide to Spring @Value](https://www.baeldung.com/spring-value-annotation)

### @DependsOn

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

### @Lazy

La anotación `@Lazy` se utiliza cuando queremos inicializar un bean de forma diferida. De forma predeterminada, Spring crea todos los beans singleton al inicio/arranque del contexto de la aplicación.

Sin embargo, hay casos en los que necesitamos crear un bean cuando se solicita solicitamos, no al iniciar la aplicación.

Esta anotación tiene un argumento con el valor predeterminado de verdadero. Es útil para anular el comportamiento predeterminado.

Por ejemplo, marcar beans para que se carguen inmediatamente cuando la configuración global es diferida, o configurar métodos específicos de `@Bean` para carga inmediata en una clase `@Configuration` marcada con @Lazy:

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

### @Lookup

Un método anotado con `@Lookup` le indica a Spring que devuelva una instancia del tipo de retorno del método cuando lo invoquemos.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Lookup.html)

- [@Lookup Annotation in Spring](https://www.baeldung.com/spring-lookup)

### @Primary

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

### @Scope

Usamos `@Scope` para definir el ámbito de una clase `@Component` o una definición de `@Bean`. Puede ser **_singleton_**, **_prototype_**, **_request_**, **_session_**, **_globalSession_** o algún ámbito personalizado.

```java
@Component
@Scope("prototype")
class Engine {}
```

**El ámbito por defecto es 'singleton'**. Esto significa que Spring crea una única instancia del bean y la reutiliza en toda la aplicación.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Scope.html)

### @Profile

Si queremos que Spring use una clase `@Component` o un método `@Bean` solo cuando un perfil específico esté activo, podemos marcarlo con `@Profile`. Podemos configurar el nombre del perfil con el argumento de la anotación:

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle {}
```

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Profile.html)

- [Spring Profiles](https://www.baeldung.com/spring-profiles)

### @Import

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

### @ImportResource

Podemos importar configuraciones XML con esta anotación. Podemos especificar las ubicaciones de los archivos XML utilizando un argumento en la anotación:

```java
@Configuration
@ImportResource("classpath:/annotations.xml")
class VehicleFactoryConfig {}
```

La anotación `@ImportResource` se utiliza exclusivamente para importar configuraciones desde archivos XML dentro del contexto de Spring.

En contraste, `@Import` se utiliza para importar configuraciones y componentes de otras clases de configuración, ya sean basadas en anotaciones o en XML, pero principalmente se usa con configuraciones basadas en anotaciones.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ImportResource.html)

### @PropertySource

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

### @PropertySources

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

### @RequestMapping

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

### @RequestBody

La anotación `@RequestBody` mapea el cuerpo de la solicitud HTTP a un objeto:

```java
@PostMapping("/save")
void saveVehicle(@RequestBody Vehicle vehicle) {
    // ...
}
```

La deserialización es automática y depende del tipo de contenido de la solicitud.

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestBody.html)

### @PathVariable

Esta anotación indica que un argumento de método está vinculado a una variable de plantilla URI. Si el nombre de la parte en la plantilla coincide con el nombre del argumento del método, no es necesario especificarlo en la anotación:

```java
import org.springframework.web.bind.annotation.*;

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

### @RequestParam

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html)

### @ResponseBody

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseBody.html)

### @ExceptionHandler

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ExceptionHandler.html)

### @ResponseStatus

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseStatus.html)

### @RestController

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)

### @ModelAttribute

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ModelAttribute.html)

### @CrossOrigin

TODO

- [Javadoc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CrossOrigin.html)

---

## Enlaces de interés

- <https://spring.io/>
- <https://docs.spring.io/spring-framework/reference/>
- <https://docs.spring.io/spring-boot/docs/current/reference/html>
- <https://www.baeldung.com/spring-core-annotations>
- <https://www.baeldung.com/spring-boot-start>

## Licencia

[![Licencia de Creative Commons](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)
Esta obra está bajo una [licencia de Creative Commons Reconocimiento-Compartir Igual 4.0 Internacional](http://creativecommons.org/licenses/by-sa/4.0/).
