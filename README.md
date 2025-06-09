# Spring Framework

> :warning: **DOCUMENTO EN DESARROLLO** :warning:

## Overview

**Spring Framework** es un poderoso y ampliamente utilizado marco de desarrollo de software para aplicaciones empresariales en Java. Diseñado para simplificar y acelerar el desarrollo de aplicaciones, Spring ofrece un enfoque integral que abarca desde la configuración hasta la implementación, abordando varios aspectos del desarrollo de software como la inversión de control, la inyección de dependencias, la gestión de transacciones, la seguridad y mucho más.

Una de las características distintivas de Spring es su **enfoque modular y extensible**, permitiendo a los desarrolladores elegir los módulos específicos que necesitan para sus proyectos. Además, fomenta las mejores prácticas de programación y sigue el principio de diseño de "Programación Orientada a Aspectos" (AOP), que facilita la separación de preocupaciones y mejora la modularidad del código.

Spring Framework se utiliza comúnmente para construir aplicaciones empresariales robustas y escalables, facilitando la creación de servicios web, aplicaciones basadas en la arquitectura Modelo-Vista-Controlador (MVC), integración con bases de datos, gestión de transacciones y mucho más. Con una comunidad activa y un ecosistema de proyectos relacionados, Spring ha evolucionado para adaptarse a las cambiantes demandas del desarrollo de software, convirtiéndose en una opción popular entre los desarrolladores Java.

**Spring Boot** es una extensión del popular Spring Framework que se centra en simplificar drásticamente el proceso de desarrollo de aplicaciones Java, especialmente aplicaciones basadas en Spring. Su objetivo principal es facilitar la creación de aplicaciones autónomas, autocontenidas y listas para la producción con la menor cantidad de configuración posible.

La relación entre Spring Boot y Spring Framework es fundamental, ya que Spring Boot se construye sobre la base sólida proporcionada por Spring. Spring Boot utiliza las características clave de Spring, como la inversión de control (IoC) y la inyección de dependencias, pero agrega una capa de convenciones y configuraciones por defecto para acelerar el desarrollo.

Lo más notable de Spring Boot es su enfoque de "opinión sobre la configuración", lo que significa que proporciona configuraciones predeterminadas sensatas para la mayoría de los casos de uso, permitiendo a los desarrolladores empezar rápidamente con sus proyectos sin tener que configurar extensamente. No obstante, sigue siendo altamente personalizable, permitiendo a los desarrolladores anular las configuraciones por defecto según sea necesario.

Con Spring Boot, el proceso de desarrollo se simplifica mediante la inclusión de un servidor embebido, como Tomcat o Jetty, lo que elimina la necesidad de desplegar la aplicación en un servidor externo. También facilita la gestión de dependencias mediante el uso de la herramienta Spring Initializr para generar proyectos con las dependencias necesarias.

En resumen, Spring Boot es una extensión de Spring Framework diseñada para hacer que el desarrollo de aplicaciones Java sea más rápido, sencillo y eficiente al proporcionar configuraciones por defecto y convenciones inteligentes sin sacrificar la flexibilidad y la potencia que ofrece Spring Framework.

:warning: **Sección introductoria generada por ChatGPT**

## Spring Core Annotations

Estas anotaciones forman parte del paquete [_'org.springframework.beans.factory.annotation'_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/package-summary.html) y [_'org.springframework.context.annotation'_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/package-summary.html).

- [Annotation-based Container Configuration](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config.html)

- [Spring Core Annotations - Baeldung](https://www.baeldung.com/spring-core-annotations)

### DI-Related Annotations

#### @Autowired

La anotación `@Autowired` se utiliza para marcar una dependencia que **el motor DI de Spring resolverá e inyectará**. Esta anotación se puede usar en un **constructor**, en un **método _'setter'_** o en un **campo**:

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

`@Autowired` tiene un argumento booleano llamado `required` con un valor predeterminado de `true`. Este argumento ajusta el comportamiento de Spring cuando no encuentra un _bean_ adecuado para conectar. Cuando es verdadero, se lanzará una excepción si no encuentra el _bean_; de lo contrario, no se conecta nada. Sin embargo, se recomienda dejar el valor a `true` (predeterminado) para evitar excepciones de _'null pointer'_ al no estar inicializado el _bean_ y ser utilizado en el código.

```java
@Autowired(required=false)
public MyService(Dependency2 dependency2) {
    this.dependency2 = dependency2;
}
```

Puede utilizarse `@Inject` en lugar de `@Autowired` en Spring Framework. `@Inject` (y `@Named`) es parte de la especificación estándar [JSR-330](https://docs.spring.io/spring-framework/reference/core/beans/standard-annotations.html) de Jakarta EE/CDI, mientras que `@Autowired` es específico de Spring y ofrece funcionalidades adicionales. Para poder utilizar esta anotación, hay que añadir la dependencia **_Jakarta Dependency Injection_** al fichero `pom.xml` o al fichero `build.gradle`:

```xml
<dependency>
    <groupId>jakarta.inject</groupId>
    <artifactId>jakarta.inject-api</artifactId>
    <version>2.0.0</version>
</dependency>
```

La anotación `@Autowired` por sí sola no es suficiente para que Spring gestione la inyección de dependencias. Es crucial que Spring esté configurado para encontrar y manejar los _beans_ en el contexto de la aplicación.

Los _beans_ son aquellas clases anotadas con [`@Component`](#component), [`@Service`](#service), [`@Repository`](#repository) o [`@Controller`](#controller).

Si Spring está configurado mediante clases Java, se utiliza la anotación [`@ComponentScan`](#componentscan) para indicar a Spring dónde buscar los beans. En una aplicación Spring Boot, la anotación [`@SpringBootApplication`](#springbootapplication) ya incluye `@ComponentScan` por defecto, escaneando el paquete donde se encuentra la clase de arranque y sus subpaquetes.

Una vez que los _beans_ están registrados en el contexto de la aplicación, ya se puede utilizar `@Autowired` para inyectar estos _beans_ en otras partes de la aplicación. La inyección se realiza en el momento en que Spring crea e inicializa los objetos de la clase que necesita las dependencias.

- [Javadoc - @Autowired](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)

- [Guide to Spring @Autowired - Baeldung](https://www.baeldung.com/spring-autowire)

- [Constructor Dependency Injection in Spring - Baeldung](https://www.baeldung.com/constructor-injection-in-spring)

- [Using @Autowired - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/autowired.html)

- [Using JSR 330 Standard Annotations - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/standard-annotations.html)

#### @Bean

La anotación `@Bean` se usa en métodos dentro de una clase anotada con [`@Configuration`](#configuration) para registrar y configurar _beans_ en el contexto de la aplicación. **Spring llamará a estos métodos** cuando se requiera de una instancia del tipo de retorno del método:

```java
@Configuration
public class AppConfiguration {
  @Bean
  public Engine engine() {
    return new Engine();
  }
}
```

Esta anotación se utiliza básicamente cuando se definen _beans_ que no pueden ser anotados directamente, como por ejemplo, cuando se trabaja con **clases de librerías de terceros**.

El _bean_ resultante por defecto tiene el mismo nombre que el _'factory method'_. Si se requiere que tenga un nombre diferente, se puede proporcionar un nombre explícito como argumento `name` de la anotación o mediante un alias. Además, se pueden indicar varios nombres que pueden ser utilizados de forma indistinta:

```java
@Configuration
public class AppConfiguration {
  @Bean("engine")
  public Engine getEngine() {
    return new Engine();
  }

  @Bean(name = "vehicle")
  public Vehicle getVehicle() {
    return new Vehicle();
  }

  @Bean(name = {"car", "truck"})
  public Vehicle getVehicle() {
    return new Vehicle();
  }
}
```

> **Todos los métodos anotados con `@Bean` deben estar en clases `@Configuration`**.

- [Javadoc - @Bean](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html)

- [Bean Overview - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/definition.html)

- [Basic Concepts: @Bean and @Configuration - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/java/basic-concepts.html)

- [Using the @Configuration annotation - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/java/configuration-annotation.html)

#### @Qualifier

Se usa la anotación `@Qualifier` junto con `@Autowired` para proporcionar la **identificación** o el **nombre** del _bean_ que Spring debe usar en situaciones ambiguas.

```java
@Component
class Bike implements Vehicle {}

@Component
class Car implements Vehicle {}
```

En caso de ambigüedad, Spring lanzará la excepción **_NoUniqueBeanDefinitionException_**. En el ejemplo anterior, Spring no sabrá que _bean_ debe inyectar. En estos casos, se utiliza la anotación `@Qualifier` para indicar a Spring **el _bean_ a inyectar**:

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

Otra forma de gestionar las ambigüedades es utilizando la anotación [`@Primary`](#primary).

- [Javadoc - @Qualifier](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html)

- [Fine-tuning Annotation-based Autowiring with Qualifiers - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/autowired-qualifiers.html#page-title)

#### @Value

Se puede utilizar la anotación `@Value` para inyectar valores de propiedad en _beans_. Es compatible con **constructores**, **métodos _'setter'_** y con **campos**.

```java
// Constructor
Engine(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

Por supuesto, inyectar valores estáticos como en el ejemplo no tiene demasiada utilidad.

Podemos usar **cadenas _'placeholder'_** en `@Value` para conectar valores definidos en fuentes externas, por ejemplo, en archivos _".properties"_ o _".yaml"_.

Por ejemplo, un valor en un fichero externo _"application.properties"_ podría ser:

```text
catalog.name=MovieCatalog
```

Podemos inyectar este valor de la siguiente forma:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name}") String catalog) {
        this.catalog = catalog;
    }
}
```

Hay que tener en cuenta que además, se necesitaría la siguiente configuración:

```java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig { }
```

Si la propiedad no está definida, se puede proporcionar un **valor por defecto**:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name:defaultCatalog}") String catalog) {
        this.catalog = catalog;
    }
}
```

- [Javadoc - @Value](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Value.html)

- [A Quick Guide to Spring @Value - Baeldung](https://www.baeldung.com/spring-value-annotation)

- [Using @Value - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/value-annotations.html)

##### Lenguaje de Expresiones SpEL

El **Lenguaje de Expresiones de Spring (SpEL)** permite evaluar expresiones en tiempo de ejecución dentro del contexto de Spring. Cuando `@Value("#{expression}")` contiene una expresión SpEL, el valor se calculará dinámicamente en tiempo de ejecución.

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

// Invoking Methods from another bean
"#{otherBean}"          // Bind a bean
"#{otherBean.property}" // Property from bean
"#{otherBean.method()}" // Invoke method from bean

// Seguridad de tipos con el operador ?.
"#{otherBean?.property}" // Antes de acceder a property se valida que otherBean != null
```

**SpEL** es útil para evaluar expresiones dinámicas en configuraciones de Spring, como valores de propiedades, condiciones, y más.

Se puede usar en diversas anotaciones y configuraciones de Spring, como [`@Value`](#value), [`@ConditionalOnExpression`](#conditionalonexpression), entre otras.

- [Spring Expression Language (SpEL) - Spring Framework](https://docs.spring.io/spring-framework/reference/core/expressions.html)

#### @DependsOn

La anotación `@DependsOn` se puede utilizar para hacer que Spring **inicialice otros _beans_ antes del _bean_ anotado**. Normalmente, este comportamiento de inicialización y carga es automático y se basa en las dependencias explícitas entre _beans_. Spring, de forma predeterminada, gestiona el ciclo de vida de los _beans_ y organiza su orden de inicialización según las necesidades.

Solo necesitamos esta anotación cuando **las dependencias están implícitas**, como por ejemplo, la carga de un controlador JDBC o la inicialización de variables estáticas.

Las **dependencias implícitas** son aquellas que no están declaradas directamente entre beans (por ejemplo, usando `@Autowired`) pero que son necesarias para el correcto funcionamiento de la aplicación.

Podemos usar `@DependsOn` en la clase dependiente especificando los nombres de los _beans_ de dependencia. El argumento de valor de la anotación necesita una matriz que contenga los nombres de los _beans_ de dependencia:

```java
@DependsOn("engine")
class Car implements Vehicle {}
```

Alternativamente, si se define un bean con la anotación [`@Bean`](#bean), el _'factory method'_ debería anotarse con `@DependsOn`:

```java
@Bean
@DependsOn("fuel")
Engine engine() {
    return new Engine();
}
```

- [Javadoc - @DependsOn](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/DependsOn.html)

#### @Lazy

La anotación `@Lazy` se utiliza a nivel de **clase** o **método** cuando queremos inicializar un _bean_ de forma diferida o perezosa.

De forma predeterminada, Spring crea todos los _beans_ con el alcance _"singleton"_ por defecto al inicio del contexto de la aplicación de manera ansiosa o _'eager'_.

Sin embargo, hay casos en los que necesitamos crear un _bean_ cuando se solicita, no al iniciar la aplicación. En estos casos se utilizará la anotación `@Lazy`.

Esta anotación tiene un argumento con el valor predeterminado de 'true'. Es útil para anular el comportamiento predeterminado.

Por ejemplo, marcar _beans_ para que se carguen inmediatamente cuando la configuración global es diferida, o configurar métodos específicos de `@Bean` para carga inmediata en una clase `@Configuration` marcada con `@Lazy`:

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

- [Javadoc - @Lazy](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Lazy.html)

- [A Quick Guide to the Spring @Lazy Annotation](https://www.baeldung.com/spring-lazy-annotation)

#### @Lookup

Un método anotado con `@Lookup` le indica a Spring que devuelva una instancia del tipo de retorno del método cuando lo invoquemos.

La anotación `@Lookup` en Spring permite la **obtención dinámica de _beans_** desde el contenedor de Spring. Se utiliza para resolver y obtener instancias de _beans_ de manera perezosa, especialmente útil en combinación con _beans 'prototype'_ o cuando se requiere obtener una nueva instancia de un bean en cada solicitud. Spring reemplaza el método anotado con `@Lookup` para devolver una nueva instancia del bean solicitado cada vez que el método es invocado.

Esta anotación es útil en casos donde la inyección directa no es posible o no es adecuada, como por ejemplo cuando se inyecta un _bean 'prototype'_ en un _bean 'singleton'_.

La explicación es que cuando se inyecta un _bean 'singleton'_, Spring crea una sola instancia de ese _bean_ y la reutiliza en toda la aplicación. Si se intenta inyectar un _bean 'prototype'_ en un _bean 'singleton'_ mediante inyección directa, Spring solo inyectará una instancia única del _bean 'prototype'_, es decir, la misma instancia será utilizada en cada solicitud, lo cual no es el comportamiento deseado.

La anotación `@Lookup` permitirá obtener una nueva instancia del _bean 'prototype'_ en cada invocación del método anotado.

```java
// bean 'singleton'
@Component
public class MiBeanSingleton {

    @Lookup
    public MiBeanPrototype obtenerBeanPrototype() {
        // Spring reemplaza esta implementación para devolver una nueva instancia de 'MiBeanPrototype'
        return null;
    }

    public void usarBeanPrototype() {
        MiBeanPrototype beanPrototype = obtenerBeanPrototype();
        // Trabajar con la nueva instancia del bean prototype
    }
}

// bean 'prototype'
@Component
@Scope("prototype")
public class MiBeanPrototype {
    // Bean creado con alcance prototype
}
```

- [Javadoc - @Lookup](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Lookup.html)

- [@Lookup Annotation in Spring](https://www.baeldung.com/spring-lookup)

#### @Primary

A veces se necesita definir **múltiples _beans_ del mismo tipo**. En estos casos, la inyección no tendrá éxito porque Spring no sabe qué _bean_ necesitamos.

Una opción para manejar este escenario es marcar todos los puntos de conexión con [`@Qualifier`](#qualifier) y especificar el nombre del _bean_ requerido.

Sin embargo, la mayoría de las veces se necesita un _bean_ específico y rara vez los otros. Se puede emplear `@Primary` para simplificar este caso: si se marca el _bean_ usado más **frecuentemente** con `@Primary`, será elegido en los puntos de inyección no calificados:

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

En el ejemplo anterior, _'Car'_ es el vehículo principal. Por lo tanto, en la clase _'Driver'_, Spring inyecta un _bean_ de tipo _'Car'_. Por supuesto, en el _bean_ _'Biker'_, el valor del campo '_vehicle'_ será un objeto de tipo _'Bike'_ porque está calificado.

- [Javadoc - @Primary](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Primary.html)

- [Fine-tuning Annotation-based Autowiring with Qualifiers - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/autowired-qualifiers.html#page-title)

#### @Scope

Usamos `@Scope` para definir el **ámbito o alcance** de una clase `@Component` o una definición de `@Bean`. Los ámbitos pueden ser:

- **_singleton_** - [ConfigurableBeanFactory.SCOPE_SINGLETON](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/ConfigurableBeanFactory.html#SCOPE_SINGLETON)

- **_prototype_** - [ConfigurableBeanFactory.SCOPE_PROTOTYPE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/ConfigurableBeanFactory.html#SCOPE_PROTOTYPE)

- **_request_** - [WebApplicationContext.SCOPE_REQUEST](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/WebApplicationContext.html#SCOPE_REQUEST)

- **_session_** - [WebApplicationContext.SCOPE_SESSION](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/WebApplicationContext.html#SCOPE_SESSION)

- **_application_** - [WebApplicationContext.SCOPE_APPLICATION](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/WebApplicationContext.html#SCOPE_APPLICATION)

- **_websocket_**

- **ámbito personalizado**

```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
class Engine {}
```

> El ámbito o alcance por **defecto** es **_'singleton'_**.

El ámbito **_'singleton'_** significa que Spring crea **una única instancia** del _bean_ en el contexto de la aplicación y la reutiliza en toda la aplicación. Por tanto, independientemente de cuántas veces se inyecte un _bean_, Spring inyectará la misma instancia.

- [Javadoc - @Scope](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Scope.html)

- [Bean Scopes - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html)

### Context Configuration Annotations

#### @Profile

La anotación `@Profile` es útil para cargar beans solo en entornos específicos (_'dev'_, _'test'_, _'prod'_) o bajo condiciones especiales. Podemos configurar el nombre del perfil como argumento de la anotación:

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle {}
```

Se puede aplicar a clases o métodos para controlar la configuración en función de los perfiles activos. Si se utiliza a nivel de clase en una clase de configuración por ejemplo, se cargarán todos los _beans_ del perfil activo. Cuando un perfil no está activo, los _beans_ anotados con `@Profile` correspondiente no se crean, lo que evita la carga innecesaria de componentes en entornos donde no son necesarios.

Además, esta anotación puede combinarse a nivel de clase y a nivel de método en una misma clase:

```java
@Configuration
@Profile("dev")
public class MixedConfig {

    // Se carga con el perfil "dev"
    @Bean
    public MyBean myDevBean() {
        return new MyBean("Dev Bean");
    }

    // Se carga con el perfil "test"
    @Bean
    @Profile("test")
    public AnotherBean anotherTestBean() {
        return new AnotherBean("Another Test Bean");
    }
}
```

Por último, se pueden combinar perfiles mediante operadores lógicos como `AND`, `OR` y `NOT`:

```java
// Indica que el bean se carga cuando dev está activo y prod no lo está.
@Configuration
@Profile({"dev", "!prod"})
public class MixedConfig {
    // ...   
}
```

Para activar un perfil, se puede hacer de varias maneras. La más común sería usando las propiedades de configuración `spring.profiles.active` y/o `spring.profiles.default` en el fichero _"application.properties"_ o _"application.yml"_. Otras formas sería con parámetros de línea de comandos, o mediante variables de entorno. Se pueden definir varios perfiles separándolos con comas:

```txt
// Determina los perfiles activos
spring.profiles.active=dev,feature-x

// Si no hay perfiles activos, Spring se fija en el perfil por defecto
spring.profiles.default=prod
```

Si no se establece ninguna de las dos propiedades anteriores, no habrá perfiles activos y **Spring sólo creará los _beans_ que no tengan perfil**.

- [Javadoc - @Profile](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Profile.html)

- [Spring Profiles - Baeldung](https://www.baeldung.com/spring-profiles)

- [Environment Abstraction - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/environment.html)

- [Profiles - Spring Boot](https://docs.spring.io/spring-boot/reference/features/profiles.html)

#### @ActiveProfiles

Spring dispone de la anotación `@ActiveProfiles`, que es una anotación compatible con JUnit 5, para declarar los perfiles activos a usar cuando se carga un contexto de aplicación en las clases de prueba.

Por lo tanto, si anotamos una clase de prueba con la anotación `@ActiveProfiles` y establecemos el atributo `value` a `test`, todas las pruebas en la clase usarán el perfil `test`:

```java
@SpringBootTest(classes = ActiveProfileApplication.class)
@ActiveProfiles(value = "test")
public class TestActiveProfileUnitTest {

    @Value("${profile.property.value}")
    private String propertyString;

    @Test
    void whenTestIsActive_thenValueShouldBeKeptFromApplicationTestYaml() {
        Assertions.assertEquals("This the the application-test.yaml file", propertyString);
    }
}
```

La anotación `@ActiveProfiles` permite definir perfiles activos específicos para pruebas. Se puede usar con un solo perfil o múltiples perfiles si se necesita combinar configuraciones, por ejemplo: `@ActiveProfiles({"test", "dev"})`.

Si se activan múltiples perfiles, el orden en que se especifiquen puede ser relevante en situaciones donde las propiedades se superpongan.

- [Javadoc - @ActiveProfiles](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/context/ActiveProfiles.html)

- [@ActiveProfiles - Spring Framework](https://docs.spring.io/spring-framework/reference/testing/annotations/integration-spring/annotation-activeprofiles.html#page-title)

- [Execute Tests Based on Active Profile With JUnit 5 - Baeldung](https://www.baeldung.com/spring-boot-junit-5-testing-active-profile)

#### @Import

La anotación `@Import` en Spring se utiliza para importar configuraciones adicionales a la configuración principal de la aplicación.

Es decir, si tenemos una clase anotada con `@Configuration` que define _beans_ y/o configuraciones específicas para la aplicación, se puede usar `@Import` para incluir esta configuración en otra clase anotada con `@Configuration` y que corresponde con la configuración principal:

```java
@Configuration
@Import(MyAdditionalConfig.class)
public class MainConfig {
    // Configuración principal de la aplicación
}
```

Además, `@Import` permite utilizar **clases específicas anotadas con `@Configuration` sin escaneo de componentes**, lo cual es útil para tener control explícito sobre qué configuraciones y _beans_ están disponibles en la aplicación. Esas clases específicas se pueden proporcionar como argumento de la anotación:

```java
@Configuration
@Import({DataSourceConfig.class, SecurityConfig.class})
public class MainConfig {
    // Configuración principal de la aplicación
}
```

Por último, `@Import` también permite importar clases que no están anotadas con `@Configuration`, asegurando que estas clases estén disponibles en el contexto de la aplicación. Importar clases no anotadas con `@Configuration` puede ser útil para asegurar que ciertas utilidades o servicios auxiliares estén disponibles en el contexto de la aplicación, incluso si no forman parte directa de la configuración de Spring.

```java
@Configuration
@Import({MyUtilityClass.class, AnotherHelper.class})
public class MainConfig {
    // Configuración principal de la aplicación
}
```

Aquí, _'MyUtilityClass'_ y _'AnotherHelper'_ son clases normales que no necesariamente son configuraciones de Spring, pero que pueden ser útiles en el contexto de la aplicación.

- [Javadoc - @Import](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Import.html)

#### @ImportResource

Para importar **configuraciones XML** podemos utilizar la anotación `@ImportResource`. Se puede especificar la ubicación del archivo XML mediante el argumento de la anotación:

```java
@Configuration
@ImportResource("classpath:/annotations.xml")
class VehicleFactoryConfig {}
```

Se pueden especificar múltiples ubicaciones de archivos XML usando `@ImportResource`, separando las ubicaciones con comas:

```java
@Configuration
@ImportResource({"classpath:/annotations.xml", "classpath:/more-config.xml"})
class VehicleFactoryConfig { }
```

La anotación `@ImportResource` se utiliza **exclusivamente** para importar configuraciones desde archivos XML dentro del contexto de Spring. Esto puede ser útil en proyectos que aún dependen de configuraciones XML o cuando se integran con sistemas existentes.

En contraste, `@Import` se utiliza para importar configuraciones y componentes de **otras clases de configuración**, ya sean basadas en anotaciones o en XML, pero principalmente se usa con configuraciones basadas en anotaciones.

- [Javadoc - @ImportResource](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ImportResource.html)

#### @PropertySource

La anotación `@PropertySource` se utiliza para definir archivos de propiedades ('.properties' o '.yml') para la configuración de la aplicación. Se debe indicar la ubicación del fichero de propiedades a cargar mediante un argumento de la anotación:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@Configuration
@PropertySource("classpath:config.properties")
public class AppConfig {
    // Configuración adicional de la aplicación
}
```

Estos archivos pueden contener configuraciones como URLs de bases de datos, rutas de archivos, configuraciones de conexión, etc. Los archivos en `src/main/resources` se incluyen en el **classpath** durante la construcción del proyecto, además de todas las dependencias incluidas con Maven o Gradle.

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
        // Default value
        String user = env.getProperty("database.user", "userByDefault");
        // Convertir un valor a int
        int connectionCount = env.getProperty("database.count", Integer.class, 30);
        // ...
    }
}
```

El objeto `Environment` está **disponible en el contexto de la aplicación** y contiene propiedades predeterminadas proporcionadas por Spring, como **valores de configuración** de la JVM (por ejemplo, `java.home`) o **propiedades del sistema**.

Mediante este objeto se pueden consultar los perfiles activos y el perfil actual del entorno utilizando este objeto. Esto es útil para ajustar el comportamiento de la aplicación según el perfil activo.

Otra forma de acceder a una propiedad podría ser utilizar directamente la anotación [`@Value`](#value). Esta anotación `@Value` es útil para inyectar valores directamente en campos de clase, especialmente para casos simples.

```java
@Component
public class MyComponent {
    @Value("${database.url}")
    private String dbUrl;
}
```

La anotación `@PropertySource` aprovecha la función de anotaciones repetidas de Java 8, lo que significa que podemos marcar una clase con ella varias veces:

```java
@Configuration
@PropertySource("classpath:/annotations.properties")
@PropertySource("classpath:/vehicle-factory.properties")
class VehicleFactoryConfig {}
```

En caso de **conflicto de claves**, prevalecerá el **último archivo cargado**; es decir, el último archivo sobreescribe el valor de la clave si hay una clave repetida en ambos archivos.

- [Javadoc - @PropertySource](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/PropertySource.html)

- [Javadoc - Environment](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/env/Environment.html)

- [Resources](https://docs.spring.io/spring-framework/reference/core/resources.html#resources-introduction)

#### @PropertySources

La anotación `@PropertySources` es un contenedor que se utiliza para especificar múltiples configuraciones de `@PropertySource`:

```java
@Configuration
@PropertySources({ 
    @PropertySource("classpath:/annotations.properties"),
    @PropertySource("classpath:/vehicle-factory.properties")
})
class VehicleFactoryConfig {}
```

Tenga en cuenta que desde Java 8 se puede lograr el mismo resultado con la función de anotaciones repetidas.

- [Javadoc - @PropertySources](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/PropertySources.html)

#### @ConfigurationProperties

La anotación `@ConfigurationProperties` en Spring Boot permite mapear propiedades externas (como propiedades en archivos `.properties` o `.yaml`) a una clase Java, facilitando el acceso a configuraciones de manera **estructurada y tipada** dentro de una aplicación Spring.

Esta anotación acepta un prefijo que indica qué propiedades del archivo se deben mapear a la clase. Por ejemplo, al utilizar el prefijo "app", todas las propiedades que comiencen con "app." se mapearán automáticamente.

Para que Spring reconozca y cargue las propiedades en la clase, se debe **habilitar el soporte** para `@ConfigurationProperties`. Esto se suele hacer con la anotación `@EnableConfigurationProperties` en una clase de configuración:

```java
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableConfigurationProperties(AppProperties.class)
public class AppConfig {
    // Otras configuraciones
}
```

Por ejemplo, un archivo de propiedades llamado `application.properties`:

```txt
app.name=MyApp
app.version=1.0.0
app.features.enabled=true
```

Con el prefijo "app", las propiedades anteriores se mapean a la siguiente clase:

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "app")
public class AppProperties {

    private String name;
    private String version;
    private boolean featuresEnabled;

    // Getters y setters
}
```

El objeto `AppProperties` puede luego inyectarse y utilizarse en cualquier componente de Spring:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class AppService {

    private final AppProperties appProperties;

    @Autowired
    public AppService(AppProperties appProperties) {
        this.appProperties = appProperties;
    }

    public void printProperties() {
        System.out.println("App Name: " + appProperties.getName());
        System.out.println("App Version: " + appProperties.getVersion());
        System.out.println("Features Enabled: " + appProperties.isFeaturesEnabled());
    }
}
```

- [Javadoc - @ConfigurationProperties](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/context/properties/ConfigurationProperties.html)

- [Using Environment Variables in Spring Boot’s Properties Files - Baeldung](https://www.baeldung.com/spring-boot-properties-env-variables)

- [Configuring System Environment Properties - Spring Boot](https://docs.spring.io/spring-boot/reference/features/external-config.html#features.external-config.system-environment)

---

## Spring Web Annotations

Estas anotaciones forman parte del paquete [_'org.springframework.web.bind.annotation'_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/package-summary.html).

- [Spring Web MVC - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc.html)

- [Spring Web Annotations - Baeldung](https://www.baeldung.com/spring-mvc-annotations)

### @RestController

La anotación `@RestController` es una meta-anotación que combina las anotaciones [`@Controller`](#controller) y [`@ResponseBody`](#responsebody) para todos los métodos de la clase, lo que significa que los valores retornados por estos métodos se serializan directamente en la respuesta HTTP (generalmente en formato JSON o XML).

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

La principal diferencia entre `@Controller` y `@RestController` es que:

- `@Controller`: Se usa para los controladores tradicionales en aplicaciones web, donde los métodos de los controladores retornan vistas o datos que deben ser procesados por una vista (como un archivo HTML).

- `@RestController`: Se usa para servicios RESTful y APIs, donde los métodos retornan directamente datos que se serializan en el formato adecuado (como JSON) sin necesidad de una vista intermedia.

- [Javadoc - @RestController](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)

- [Annotated Controllers - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller.html)

### @CrossOrigin

La anotación `@CrossOrigin` en Spring Framework es utilizada para configurar las políticas de intercambio de recursos entre diferentes dominios (**CORS**, por sus siglas en inglés - _"Cross-Origin Resource Sharing"_). CORS es un mecanismo de seguridad implementado por los navegadores web para restringir las solicitudes HTTP que se originan desde un dominio diferente al del servidor de destino.

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

En este ejemplo, se permite el acceso a <http://allowed-origin.com> para los métodos GET y POST del endpoint `/data`. Si la anotación se coloca en una clase, se aplica a todos los métodos del controlador que contiene.

También se puede habilitar CORS globalmente para todos los controladores mediante una configuración global en una clase que implemente `WebMvcConfigurer`:

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

Aquí, se permiten solicitudes desde <http://domain1.com> y <https://domain2.com> con los métodos HTTP especificados. Los encabezados permitidos y expuestos se pueden definir, y se puede configurar el uso de credenciales.

Si se está utilizando Spring Security en la aplicación, es fundamental asegurarse de que la configuración de **CORS** no entre en conflicto con las políticas de seguridad definidas en Spring Security. La configuración de **CORS** en Spring Security se realiza a través de la clase `WebSecurityConfigurerAdapter`, y se puede integrar con la configuración global de **CORS** para asegurar una política coherente.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.cors()
            .and()
            .authorizeRequests()
            .anyRequest().authenticated();
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(Arrays.asList("http://domain1.com", "https://domain2.com"));
        configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE"));
        configuration.setAllowedHeaders(Arrays.asList("header1", "header2"));
        configuration.setExposedHeaders(Arrays.asList("header3", "header4"));
        configuration.setAllowCredentials(true);
        configuration.setMaxAge(3600L);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```

Este ejemplo configura **CORS** en la seguridad de Spring para que coincida con la configuración global y permita solicitudes desde los dominios especificados con los métodos y encabezados adecuados.

- [Javadoc - @CrossOrigin](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CrossOrigin.html)

- [CORS with Spring - Baeldung](https://www.baeldung.com/spring-cors)

- [CORS - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc-cors.html)

### Request Handling Annotations

#### @RequestMapping

En Spring Framework, la anotación `@RequestMapping` se utiliza para marcar **métodos manejadores de peticiones** dentro de clases anotadas con `@Controller` o `@RestController`.

Permite mapear solicitudes HTTP a métodos de controlador específicos en función de varios criterios. Los elementos clave de `@RequestMapping` son:

- `path` (o sus alias `name` y `value`): indica a qué URL está mapeado el método.

- `method`: define los métodos HTTP compatibles.

- `params`: filtra las peticiones basándose en la presencia, ausencia o valor de parámetros HTTP.

- `headers`: filtra las peticiones basándose en la presencia, ausencia o valor de cabeceras HTTP.

- `consumes`: especifica los tipos de medios que el método puede consumir en el cuerpo de la petición HTTP.

- `produces`: especifica los tipos de medios que el método puede producir en el cuerpo de la respuesta HTTP.

```java
@Controller
class VehicleController {

    @RequestMapping(value = "/vehicles/home", method = RequestMethod.GET)
    String home() {
        return "home";
    }
}
```

Podemos proporcionar configuraciones predeterminadas para todos los métodos manejadores en una clase `@Controller` aplicando `@RequestMapping` a nivel de clase. La URL base especificada a nivel de clase se combinará con los _paths_ definidos en los métodos individuales:

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

Desde la **versión 4.3** de Spring, se introdujeron las anotaciones específicas para métodos HTTP como:

- `@GetMapping`

- `@PostMapping`

- `@PutMapping`

- `@DeleteMapping`

- `@PatchMapping`

Estas meta-anotaciones son variantes de `@RequestMapping` y permiten una configuración más concisa y específica para cada tipo de solicitud HTTP:

```java
@GetMapping("/vehicles/home")
public String home() {
    return "home";
}
```

- [Javadoc - @RequestMapping](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html)

- [Mapping Requests - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html)

#### @HttpExchange

Es una anotación introducida en Spring Framework 6 que nos permite definir interfaces HTTP de **forma declarativa**. Si bien el objetivo principal de `@HttpExchange` es abstraer el código del cliente HTTP con un proxy generado, la interfaz HTTP sobre la que se colocan estas anotaciones es un contrato neutral para el uso del cliente frente al servidor

```java
@HttpExchange("/persons")
interface PersonService {

  @GetExchange("/{id}")
  Person getPerson(@PathVariable Long id);

  @PostExchange
  void add(@RequestBody Person person);
}
```

El controlador implementa la interfaz declarada:

```java
@RestController
class PersonController implements PersonService {

  public Person getPerson(@PathVariable Long id) {
    // ...
  }

  @ResponseStatus(HttpStatus.CREATED)
  public void add(@RequestBody Person person) {
    // ...
  }
}
```

- [Javadoc - @HttpExchange](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/service/annotation/HttpExchange.html)

- [@HttpExchange - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html#mvc-ann-httpexchange-annotation)

#### @RequestBody

La anotación `@RequestBody` en Spring Framework se utiliza para **mapear el cuerpo de una solicitud HTTP a un objeto Java**. Esta anotación es particularmente útil en los métodos de controladores que manejan solicitudes POST, PUT, o PATCH, donde los datos se envían en el cuerpo de la solicitud y no en la URL.

```java
@PostMapping("/save")
void saveVehicle(@RequestBody Vehicle vehicle) {
    // ...
}
```

En el ejemplo anterior, el cuerpo de la solicitud HTTP se deserializa automáticamente en una instancia de la clase `Vehicle`. El tipo de deserialización se basa en el tipo de contenido de la solicitud, como `application/json`, `application/xml`, etc.

Spring utiliza un convertidor adecuado, como [Jackson para JSON](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/jackson.html), para realizar esta conversión.

- [Javadoc - @RequestBody](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestBody.html)

- [@RequestBody - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/requestbody.html)

#### @PathVariable

La anotación `@PathVariable` se utiliza para **vincular un argumento de método a una variable de ruta** en una URI. Si el nombre de la variable en la ruta coincide con el nombre del argumento del método, no es necesario especificarlo en la anotación:

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

Si los nombres no coinciden o si se requiere especificarlo explícitamente, se puede hacer usando `@PathVariable` con el nombre de la variable de ruta:

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

Si una variable de ruta es opcional, se puede indicar estableciendo el argumento de la anotación como `'required = false'`.

Cuando se establece `'required = false'`, la variable de ruta puede ser omitida en la URI. Si se omite, el valor del argumento será `null` (en el caso de objetos) o el valor por defecto para tipos primitivos, como 0 para `long`:

```java
@RequestMapping("/{id}")
Vehicle getVehicle(@PathVariable(required = false) long id) {
    // ...
}
```

- [Javadoc - @PathVariable](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/PathVariable.html)

#### @RequestParam

La anotación `@RequestParam` se utiliza para **acceder a los parámetros de solicitud HTTP**:

```java
// "http://example.com/users?id=123"
@GetMapping("/users")
public String getUser(@RequestParam String id) {
    // El valor de id será "123"
    return "User ID: " + id;
}
```

La anotación `@RequestParam` ofrece opciones de configuración similares a las de `@PathVariable`.

Además, permite especificar un valor predeterminado para el parámetro en caso de que no se encuentre en la solicitud o esté vacío. Esto se logra mediante el atributo `defaultValue`:

```java
@RequestMapping("/buy")
Car buyCar(@RequestParam(defaultValue = "5") int seatCount) {
    // ...
}
```

- [Javadoc - @RequestParam](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html)

- [@RequestParam - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/requestparam.html)

#### @ModelAttribute

La anotación `@ModelAttribute` en Spring MVC se utiliza para **vincular un método o parámetro de método a un atributo del modelo**. Esto es especialmente útil en controladores de Spring MVC, donde se manejan solicitudes HTTP y se preparan datos que serán utilizados en la vista.

Cuando se usa `@ModelAttribute` en un **parámetro de método**, Spring enlaza automáticamente los datos de la solicitud HTTP a ese objeto, facilitando la recepción de datos de formularios o solicitudes HTTP. Con esta anotación, también podemos acceder a elementos que ya están presentes en el modelo del controlador MVC:

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

Si un método de un controlador se anota con `@ModelAttribute`, este método se ejecuta **antes** que cualquier método anotado con `@RequestMapping` en el mismo controlador. Esto permite preparar y agregar atributos al modelo que serán utilizados en la vista. Spring agrega automáticamente el valor de retorno de este método al modelo:

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

En Spring MVC, para procesar correctamente los datos enviados desde un formulario HTML o una solicitud HTTP POST, es **esencial enlazar** esos datos a un objeto del modelo utilizando `@ModelAttribute`. Si no se realiza este enlace explícitamente, Spring no podrá asignar los datos del formulario a ningún objeto y, por lo tanto, no estarán disponibles para su uso en el controlador.

El uso de `@ModelAttribute` también permite la [validación](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html) automática de los datos de entrada mediante anotaciones como `@Valid` o `@Validated`, lo que asegura que los datos recibidos cumplen con las reglas de validación antes de procesarse.

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

- [Javadoc - @ModelAttribute](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ModelAttribute.html)

- [Spring MVC and the @ModelAttribute Annotation - Baeldung](https://www.baeldung.com/spring-mvc-and-the-modelattribute-annotation)

- [@ModelAttribute - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/modelattrib-method-args.html)

- [Model - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-modelattrib-methods.html)

- [Validation - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html)

#### @CookieValue

La anotación `@CookieValue` en Spring Framework se utiliza para **enlazar el valor de una _cookie_ HTTP específica a un parámetro de método** en un controlador de Spring MVC. Esto es útil cuando se necesita acceder al valor de una _cookie_ enviada por el cliente en una solicitud HTTP.

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

Por defecto, la presencia de la _cookie_ es **requerida**. Si la _cookie_ no está presente, Spring lanzará una excepción `MissingCookieValueException`. Para indicar que la _cookie_ no es obligatoria, puedes usar el argumento `'required = false'` en la anotación:

```java
@GetMapping("/optionalCookie")
public ResponseEntity<String> getOptionalCookie(@CookieValue(name = "optionalCookie", required = false) String optionalCookie) {
    // Si la cookie "optionalCookie" no está presente, optionalCookie será null
    return ResponseEntity.ok("Value of 'optionalCookie': " + optionalCookie);
}
```

También es posible recuperar múltiples _cookies_ en un solo método del controlador utilizando `@CookieValue` varias veces:

```java
@GetMapping("/getCookies")
public ResponseEntity<String> getCookies(@CookieValue(name = "cookie1", defaultValue = "default1") String cookie1,
                                        @CookieValue(name = "cookie2", defaultValue = "default2") String cookie2) {
    // Aquí puedes usar cookie1 y cookie2, que contendrán los valores de las cookies "cookie1" y "cookie2" respectivamente
    return ResponseEntity.ok("Value of 'cookie1': " + cookie1 + ", Value of 'cookie2': " + cookie2);
}
```

- [Javadoc - @CookieValue](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CookieValue.html)

- [@CookieValue - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/cookievalue.html)

#### @RequestHeader

La anotación `@RequestHeader` en Spring Framework se utiliza para **enlazar el valor de una cabecera HTTP específica a un parámetro de método** en un controlador de Spring MVC. Esto es útil cuando necesitas acceder a valores específicos de las cabeceras HTTP enviadas por el cliente en una solicitud.

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

Por defecto, Spring espera que **la cabecera esté presente** en la solicitud. Si la cabecera no se encuentra, se generará una excepción `MissingRequestHeaderException`. Para manejar este caso y proporcionar un valor predeterminado si la cabecera está ausente, puedes usar el atributo `defaultValue` de la anotación:

```java
@GetMapping("/getCustomHeader")
public ResponseEntity<String> getCustomHeader(@RequestHeader(value = "X-Custom-Header", defaultValue = "defaultValue") String customHeader) {
    // Aquí puedes usar customHeader, que contendrá el valor de la cabecera "X-Custom-Header" o "defaultValue" si no está presente
    return ResponseEntity.ok("X-Custom-Header value: " + customHeader);
}
```

En este ejemplo, si la cabecera `X-Custom-Header` no está presente en la solicitud, la variable `customHeader` tomará el valor "defaultValue" en lugar de generar una excepción.

- [Javadoc - @RequestHeader](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestHeader.html)

- [@RequestHeader - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/requestheader.html)

### Response Handling Annotations

#### @ResponseBody

La anotación `@ResponseBody` en Spring Framework se utiliza para indicar que **el valor retornado por un método de controlador debe ser vinculado directamente a la respuesta HTTP**. En otras palabras, Spring convierte automáticamente el objeto retornado en un formato específico (como JSON o XML) usando convertidores de medios configurados.

Es decir, cuando un método de controlador anotado con `@ResponseBody` se invoca y retorna un objeto, Spring convierte automáticamente ese objeto en un formato específico para la respuesta HTTP. La conversión del objeto a formato se realiza a través de un convertidor de medios (_'media converter'_), que depende de la configuración de Spring y de las bibliotecas disponibles en el classpath.

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

Si se anota una clase con `@ResponseBody`, todos los métodos de la clase también se verán afectados. Sin embargo, `@RestController` combina `@Controller` y `@ResponseBody`, aplicando `@ResponseBody` a todos los métodos de la clase automáticamente.

Para que la conversión funcione correctamente, asegúrate de tener las bibliotecas adecuadas en el _classpath_, como Jackson para JSON o JAXB para XML. En caso de errores, se pueden usar mecanismos como `@ExceptionHandler` para manejar excepciones y devolver respuestas adecuadas.

Además, si necesitas una conversión personalizada, puedes configurar [convertidores de medios](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-config/message-converters.html) en la configuración de Spring.

- [Javadoc - @ResponseBody](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseBody.html)

- [@ResponseBody - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/responsebody.html)

#### @ExceptionHandler

La anotación `@ExceptionHandler` en Spring Framework se utiliza para **manejar excepciones generadas por métodos de controladores** de solicitudes. Puedes declarar un método de manejo de errores personalizado utilizando esta anotación, y Spring invocará este método cuando se detecte una excepción especificada.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ExampleController {

    @GetMapping("/example")
    public String exampleMethod() {
        // Simulamos una excepción
        throw new IllegalArgumentException("Invalid argument");
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> onIllegalArgumentException(IllegalArgumentException exception) {
        // Personaliza la respuesta de error
        return new ResponseEntity<>("Error: " + exception.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

Puedes manejar múltiples excepciones en un solo método usando una lista de excepciones:

```java
@ExceptionHandler({IllegalArgumentException.class, NullPointerException.class})
public ResponseEntity<String> handleMultipleExceptions(Exception exception) {
    return new ResponseEntity<>("Error: " + exception.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
}
```

El método anotado con `@ExceptionHandler` puede devolver diferentes tipos de respuestas, como `ResponseEntity`, para proporcionar una respuesta más completa y personalizada:

```java
@ExceptionHandler(IllegalArgumentException.class)
public ResponseEntity<ErrorResponse> handleIllegalArgumentException(IllegalArgumentException exception) {
    ErrorResponse errorResponse = new ErrorResponse("Invalid argument", exception.getMessage());
    return new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST);
}
```

Donde `ErrorResponse` es una clase que se podría definir para encapsular detalles del error:

```java
public class ErrorResponse {
    private String error;
    private String message;

    // Constructor, getters y setters
}
```

- [Javadoc - @ExceptionHandler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ExceptionHandler.html)

- [Exceptions - Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-exceptionhandler.html)

#### @ResponseStatus

La anotación `@ResponseStatus` en Spring Framework se utiliza para **especificar el código de estado HTTP** de la respuesta generada por un método manejador de solicitudes. Esta anotación puede declararse en métodos de controladores de solicitudes y también en métodos de manejo de excepciones para establecer el código de estado HTTP adecuado.

Puedes usar `@ResponseStatus` para asignar un código de estado HTTP a una respuesta de un método:

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

En este ejemplo, el método `helloWorld` devuelve un código de estado '200 OK' con la respuesta.

Además, podemos proporcionar un motivo para el código de estado utilizando el argumento `reason`:

```java
@GetMapping("/notfound")
@ResponseStatus(value = HttpStatus.NOT_FOUND, reason = "Resource not found")
public void resourceNotFound() {
    // Método vacío con código de estado 404 y motivo "Resource not found"
}
```

El argumento `reason` es opcional y se utiliza para proporcionar una descripción adicional que puede aparecer en los registros del servidor o en los detalles de la respuesta, dependiendo del cliente y del servidor.

La anotación `@ResponseStatus` también puede usarse en métodos de manejo de excepciones para devolver un código de estado HTTP específico cuando se produce una excepción:

```java
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ExampleController {

    @ExceptionHandler(IllegalArgumentException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    void onIllegalArgumentException(IllegalArgumentException exception) {
        // Manejo de IllegalArgumentException con código de estado 400 Bad Request
    }
}
```

En este caso, cuando se lanza una `IllegalArgumentException`, el código de estado de la respuesta será '400 Bad Request'.

- [Javadoc - @ResponseStatus](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseStatus.html)

- [Returning Custom Status Codes from Spring Controllers - Baeldung](https://www.baeldung.com/spring-mvc-controller-custom-http-status-code)

## Spring Boot Annotations

Spring Boot facilita la configuración de Spring con su función de configuración automática.

Estas anotaciones forman parte del paquete [_'org.springframework.boot.autoconfigure'_](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/package-summary.html) y [_'org.springframework.boot.autoconfigure.condition'_](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/package-summary.html).

- [Spring Boot Annotations - Baeldung](https://www.baeldung.com/spring-boot-annotations)

### @SpringBootApplication

La anotación `@SpringBootApplication` se utiliza para marcar la **clase principal de una aplicación Spring Boot**. Esta anotación es esencial en las aplicaciones Spring Boot, ya que combina varias configuraciones clave en una sola anotación, simplificando y automatizadno el proceso de configuración.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class VehicleFactoryApplication {
    public static void main(String[] args) {
        SpringApplication.run(VehicleFactoryApplication.class, args);
    }
}
```

Esta anotación es una **meta-anotación** de las anotaciones `@SpringBootConfiguration` (**meta-anotación** de `@Configuration`), `@EnableAutoConfiguration` y `@ComponentScan` con sus atributos predeterminados.

- La anotación [`@Configuration`](#configuration) marca la clase como una **clase de configuración** de Spring. Esto indica que la clase puede contener métodos anotados con `@Bean`, los cuales definen los beans del contexto de aplicación de Spring.

- La anotación [`@EnableAutoConfiguration`](#enableautoconfiguration) habilita la **configuración automática** de Spring Boot. Spring Boot intenta configurar automáticamente la aplicación basándose en las dependencias presentes en el _classpath_. Esto simplifica la configuración inicial y reduce la necesidad de especificar manualmente los beans de configuración.

- La anotación [`@ComponentScan`](#componentscan) activa el **escaneo de componentes** en el paquete en el que se encuentra la clase principal y en los sub-paquetes. Esto permite que Spring detecte y registre componentes, servicios, controladores y otros beans definidos en el proyecto.

> El método `SpringApplication.run` arranca la aplicación Spring Boot, configurando el contexto de aplicación y lanzando el servidor embebido (como Tomcat, si se está utilizando).

- [Javadoc - @SpringBootApplication](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/SpringBootApplication.html)

- [Using the @SpringBootApplication Annotation - Spring Boot](https://docs.spring.io/spring-boot/reference/using/using-the-springbootapplication-annotation.html)

- [SpringApplication - Spring Boot](https://docs.spring.io/spring-boot/reference/features/spring-application.html)

### @EnableAutoConfiguration

La anotación `@EnableAutoConfiguration` en Spring Boot habilita la **configuración automática** de la aplicación. Spring Boot busca _beans_ de configuración automática en el _classpath_ y los aplica según las dependencias presentes.

Para utilizar `@EnableAutoConfiguration` directamente, debe ser combinada con la anotación `@Configuration`:

```java
@Configuration
@EnableAutoConfiguration
public class VehicleFactoryConfig {
    // Configuración de la aplicación
}
```

Por otro lado, la anotación `@SpringBootApplication` es una meta-anotación de las anotaciones `@Configuration`, `@EnableAutoConfiguration` y `@ComponentScan` con sus atributos predeterminados.

Eso significa que si se utiliza `@SpringBootApplication`, no es necesario utilizar la anotación `@EnableAutoConfiguration` por separado.

- [Javadoc - @EnableAutoConfiguration](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/EnableAutoConfiguration.html)

- [Auto-configuration - Spring Boot](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)

### Auto-Configuration Conditions

En el contexto de Spring Framework, las condiciones de auto-configuración (`@Conditional`) permiten aplicar configuraciones basadas en condiciones específicas durante la ejecución.

Estas condiciones se utilizan ampliamente en la auto-configuración de Spring Boot y en la configuración personalizada de aplicaciones Spring, permitiendo una configuración modular y flexible que se adapta dinámicamente según el entorno y las condiciones del sistema.

Spring proporciona **diversas condiciones predefinidas** listas para ser utilizadas, como `@ConditionalOnProperty` y `@ConditionalOnClass`.

Estas anotaciones de configuración se pueden colocar en clases `@Configuration` o métodos `@Bean`.

```java
@Configuration
@ConditionalOnProperty(name = "feature.enabled", havingValue = "true")
public class FeatureConfig {
    @Bean
    public MyFeature myFeature() {
        return new MyFeature();
    }
}
```

- [Creating Your Own Auto-configuration - Spring Boot](https://docs.spring.io/spring-boot/reference/features/developing-auto-configuration.html)

- [Create a Custom Auto-Configuration with Spring Boot - Baeldung](https://www.baeldung.com/spring-boot-custom-auto-configuration)

#### @Conditional

La anotación `@Conditional` se utiliza para aplicar una condición a un componente de Spring, como un _bean_ o una configuración, para que se active únicamente si la condición especificada se cumple en tiempo de ejecución:

```java
@Configuration
@Conditional(OnProductionEnvironmentCondition.class)
public class ProductionConfiguration {
    // Configuración específica para el entorno de producción
}
```

La clase `OnProductionEnvironmentCondition` podría verificar si la aplicación se está ejecutando en un entorno de producción. Esto permite que la configuración específica solo se aplique en ese entorno, asegurando que las configuraciones adecuadas se carguen según el contexto:

```java
public class OnProductionEnvironmentCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        String env = context.getEnvironment().getProperty("env");
        return "production".equalsIgnoreCase(env);
    }
}
```

- [Javadoc - @Conditional](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Conditional.html)

#### @ConditionalOnClass and @ConditionalOnMissingClass

Usando estas condiciones, Spring solo aplicará el _bean_ de configuración automática marcado si la clase especificada en el argumento de la anotación está presente (`@ConditionalOnClass`) o ausente (`@ConditionalOnMissingClass`) en el classpath.

```java
@Configuration
@ConditionalOnClass(DataSource.class)
class MySQLAutoconfiguration {
    // Configuración específica para MySQL si DataSource está presente
}
```

```java
@Configuration
@ConditionalOnMissingClass("com.example.SomeClass")
class FallbackConfiguration {
    // Configuración alternativa si SomeClass no está presente
}
```

- [Javadoc - @ConditionalOnClass](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnClass.html)

- [Javadoc - @ConditionalOnMissingClass](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnMissingClass.html)

#### @ConditionalOnBean and @ConditionalOnMissingBean

Podemos usar estas anotaciones cuando queramos definir condiciones basadas en la presencia (`@ConditionalOnBean`) o ausencia (`@ConditionalOnMissingBean`) de un _bean_ específico en el contexto de Spring.

```java
@Bean
@ConditionalOnBean(name = "dataSource")
LocalContainerEntityManagerFactoryBean entityManagerFactory() {
    // Configuración del EntityManagerFactory si dataSource está presente
}
```

```java
@Bean
@ConditionalOnMissingBean(name = "dataSource")
DataSource fallbackDataSource() {
    // Configuración alternativa si dataSource no está presente
}
```

- [Javadoc - @ConditionalOnBean](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnBean.html)

- [Javadoc - @ConditionalOnMissingBean](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnMissingBean.html)

#### @ConditionalOnProperty

Con esta anotación, podemos poner condiciones sobre los valores de las propiedades en el archivo de configuración de la aplicación, como `application.properties` o `application.yml`.

```java
@Bean
@ConditionalOnProperty(name = "usemysql", havingValue = "local")
DataSource dataSource() {
    // ...
}
```

También podemos usar `@ConditionalOnProperty` para condicionar la configuración basada en la presencia o ausencia de una propiedad utilizando el atributo `matchIfMissing`:

```java
@Bean
@ConditionalOnProperty(name = "usemysql", matchIfMissing = true)
DataSource defaultDataSource() {
    // Configuración del DataSource por defecto si 'usemysql' no está presente
}
```

- [Javadoc - @ConditionalOnProperty](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnProperty.html)

#### @ConditionalOnResource

Podemos hacer que Spring use una definición solo cuando un recurso específico esté presente en el classpath:

```java
@ConditionalOnResource(resources = "classpath:mysql.properties")
Properties additionalProperties() {
    // Configuración adicional si 'mysql.properties' está presente en el classpath
}
```

- [Javadoc - @ConditionalOnResource](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnResource.html)

#### @ConditionalOnWebApplication and @ConditionalOnNotWebApplication

Con estas anotaciones podemos crear condiciones en función de si la aplicación actual es una aplicación web (`@ConditionalOnWebApplication`) o no es una aplicación web (`@ConditionalOnNotWebApplication`):

```java
@Bean
@ConditionalOnWebApplication
HealthCheckController healthCheckController() {
    // Configuración del controlador de verificación de salud si es una aplicación web
}
```

- [Javadoc - @ConditionalOnWebApplication](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnWebApplication.html)

- [Javadoc - @ConditionalOnNotWebApplication](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnNotWebApplication.html)

#### @ConditionalOnExpression

Podemos utilizar esta anotación en situaciones más complejas. Spring usará la definición marcada cuando la [expresión SpEL](#lenguaje-de-expresiones-spel) se evalúe como verdadera:

```java
@Bean
@ConditionalOnExpression("${usemysql} && ${mysqlserver == 'local'}")
DataSource dataSource() {
    // Configuración del DataSource si usemysql es true y mysqlserver es 'local'
}
```

- [Javadoc - @ConditionalOnExpression](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/condition/ConditionalOnExpression.html)

## Spring Scheduling Annotations

Cuando la ejecución de un solo hilo no es suficiente, podemos usar anotaciones del paquete [_'org.springframework.scheduling.annotation'_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html).

- [Task Execution and Scheduling - Spring Framework](https://docs.spring.io/spring-framework/reference/integration/scheduling.html#scheduling-annotation-support)

- [Task Execution and Scheduling - Spring Boot](https://docs.spring.io/spring-boot/reference/features/task-execution-and-scheduling.html)

- [Spring Scheduling Annotations - Baeldung](https://www.baeldung.com/spring-scheduling-annotations)

### @EnableAsync

Esta anotación habilita la funcionalidad asíncrona en Spring. Para habilitarla, debemos usar `@EnableAsync` junto con `@Configuration`:

```java
@Configuration
@EnableAsync
class VehicleFactoryConfig {}
```

Ahora que hemos habilitado las llamadas asíncronas, podemos usar `@Async` para definir los métodos que las admiten:

```java
@Async
public void processOrder() {
    // Código para procesar la orden de manera asíncrona
}
```

El método `processOrder` se ejecutará en un hilo separado, permitiendo que el hilo principal continúe su ejecución sin esperar a que este método termine.

Además, es posible configurar un `Executor` personalizado si se necesita un control más fino sobre los hilos utilizados para las tareas asíncronas:

```java
@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean(name = "taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(25);
        executor.initialize();
        return executor;
    }

}
```

Configurar un `Executor` personalizado permite ajustar el número de hilos y la capacidad de la cola para las tareas asíncronas. Con esta configuración, cualquier método anotado con `@Async` utilizará este `taskExecutor` definido.

- [Javadoc - @EnableAsync](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/EnableAsync.html)

### @EnableScheduling

Esta anotación habilita la programación en la aplicación. Para habilitarla, debemos usar `@EnableScheduling` junto con `@Configuration`:

```java
@Configuration
@EnableScheduling
class VehicleFactoryConfig {}
```

Como resultado, ahora podemos ejecutar métodos periódicamente con `@Scheduled`:

```java
@Scheduled(fixedRate = 5000)
public void reportCurrentTime() {
    System.out.println("The time is now " + new Date());
}
```

El método `reportCurrentTime` se ejecutará cada 5 segundos, imprimiendo la hora actual. [`@Scheduled`](#scheduled) admite varios parámetros como `fixedRate`, `fixedDelay` y `cron` para definir la frecuencia de ejecución de los métodos.

- [Javadoc - @EnableScheduling](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/EnableScheduling.html)

### @Async

La anotación `@Async` en Spring se utiliza para marcar un método como **asíncrono**, permitiendo que el método se ejecute en un hilo separado o en un pool de hilos administrado por Spring. Esto es útil para operaciones que no necesitan bloquear el hilo principal de ejecución, como tareas de larga duración, operaciones de E/S intensivas, o llamadas a servicios externos.

Para marcar un método como asíncrono, simplemente lo anotamos con `@Async`:

```java
@Async
void repairCar() {
    // ...
}
```

Si aplicamos esta anotación a una clase, todos los métodos se llamarán de forma asincrónica.

Para que `@Async` funcione correctamente, la clase que contiene el método anotado debe ser administrada por Spring (normalmente usando `@Service`, `@Component`, o `@Repository`)

Es importante habilitar las llamadas asíncronas con `@EnableAsync` o mediante configuración XML.

Un método anotado con `@Async` puede devolver un valor envuelto en un `Future` si se necesita obtener el resultado de la ejecución asíncrona:

```java
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;
import java.util.concurrent.Future;
import java.util.concurrent.CompletableFuture;

@Service
public class MyService {

    @Async
    public Future<String> asyncMethodWithReturn() {
        // Método que retorna un resultado de manera asíncrona
        return new AsyncResult<>("Resultado asíncrono");
    }

    @Async
    public CompletableFuture<String> asyncMethodWithCompletableFuture() {
        // Método que retorna un resultado de manera asíncrona usando CompletableFuture
        return CompletableFuture.completedFuture("Resultado asíncrono con CompletableFuture");
    }
}
```

El uso de `CompletableFuture` permite manejar operaciones más complejas y no bloqueantes.

- [Javadoc - @Async](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Async.html)

- [Javadoc - CompletableFuture](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/CompletableFuture.html)

- [How To Do @Async in Spring - Baeldung](https://www.baeldung.com/spring-async)

### @Scheduled

La anotación `@Scheduled` en Spring se utiliza para programar la ejecución de métodos a intervalos específicos, en momentos específicos o según expresiones cron. Esta anotación es útil para tareas recurrentes y automatización de procesos dentro de una aplicación Spring.

Para ejecutar un método periódicamente, podemos usar esta anotación:

```java
@Scheduled(fixedRate = 10000)
void checkVehicle() {
    // ...
}
```

Podemos usarla para ejecutar un método a **intervalos fijos** o con **expresiones cron**:

- **`fixedRate`**: ejecuta el método con una tasa fija en milisegundos, independientemente de cuánto tiempo haya tomado la ejecución anterior.

- **`fixedDelay`**: ejecuta el método con un retraso fijo en milisegundos después de que se completa la ejecución anterior.

- **`initialDelay`**: especifica un retraso inicial en milisegundos antes de la primera ejecución del método.

- **`cron`**: permite una expresión cron para definir horarios más complejos y específicos.

La anotación `@Scheduled` aprovecha la función de anotaciones repetidas de Java 8, lo que significa que podemos marcar un método con ella varias veces:

```java
// Se ejecutará cada 10 segundos y también a la hora en punto de lunes a viernes.
@Scheduled(fixedRate = 10000)
@Scheduled(cron = "0 * * * * MON-FRI")
void checkVehicle() {
    // ...
}
```

Tenga en cuenta que el método anotado con `@Scheduled` debe tener un tipo de retorno nulo.

Además, debemos habilitar la programación para que esta anotación funcione, por ejemplo, con `@EnableScheduling` o la configuración XML.

Para que `@Scheduled` funcione correctamente, la clase que contiene el método anotado debe ser administrada por Spring (normalmente utilizando `@Component`, `@Service`, o similar).

- [Javadoc - @Scheduled](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html)

- [The @Scheduled Annotation in Spring - Baeldung](https://www.baeldung.com/spring-scheduled-tasks)

### @Schedules

La anotación `@Schedules` permite especificar múltiples reglas `@Scheduled`:

```java
@Schedules({ 
  @Scheduled(fixedRate = 10000), 
  @Scheduled(cron = "0 * * * * MON-FRI")
})
void checkVehicle() {
    // ...
}
```

Desde Java 8, se puede lograr lo mismo utilizando anotaciones repetidas:

```java
@Scheduled(fixedRate = 10000)
@Scheduled(cron = "0 * * * * MON-FRI")
void checkVehicle() {
    // ...
}
```

Las anotaciones repetidas simplifican el uso de múltiples reglas `@Scheduled`, haciendo que el código sea más limpio y fácil de mantener.

- [Javadoc - @Schedules](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Schedules.html)

## Spring Data Annotations

**Spring Data** proporciona una abstracción sobre las tecnologías de almacenamiento de datos. Por lo tanto, nuestro código de lógica de negocios puede ser mucho más independiente de la implementación de persistencia subyacente. Además, Spring simplifica el manejo de los detalles del almacenamiento de datos que dependen de la implementación.

- [Javadoc - Spring Data Core](https://docs.spring.io/spring-data/commons/docs/current/api/)

- [Data Access - Spring Framework](https://docs.spring.io/spring-framework/reference/data-access.html)

- [Spring Data Annotations - Baeldung](https://www.baeldung.com/spring-data-annotations)

### Common Spring Data Annotations

#### @Transactional

La anotación `@Transactional` en Spring administra transacciones de manera declarativa en métodos o clases. Esta anotación permite que los métodos anotados se ejecuten dentro de una transacción gestionada por Spring, asegurando la atomicidad, consistencia, aislamiento y durabilidad (ACID) de las operaciones realizadas en la base de datos u otros recursos transaccionales.

Para configurar el comportamiento transaccional de un método, podemos usar:

```java
@Transactional
void pay() {
    // ...
}
```

La anotación `@Transactional` puede aplicarse a interfaces y métodos de interfaces, no solo a clases y métodos de clases.

Si aplicamos esta anotación a nivel de clase, afecta a todos los métodos dentro de la clase. Podemos anular su comportamiento aplicando una configuración diferente a un método específico:

```java
@Transactional
public class PaymentService {

    @Transactional
    public void pay() {
        // ...
    }

    @Transactional(readOnly = true)
    public void checkPaymentStatus() {
        // ...
    }
}
```

La anotación `@Transactional` admite varios atributos para personalizar el comportamiento transaccional:

- `propagation`: define cómo debe comportarse la transacción actual en relación con las transacciones existentes.

- `isolation`: especifica el nivel de aislamiento de la transacción.

- `timeout`: define el tiempo máximo que puede durar la transacción antes de ser abortada.

- `readOnly`: indica si la transacción es solo de lectura.

- `rollbackFor`: especifica las excepciones que deben provocar un rollback de la transacción.

Además, `@Transactional` es compatible con varios gestores de transacciones, como JPA, JDBC, y JMS.

Para que `@Transactional` funcione correctamente, la clase que contiene el método anotado debe ser administrada por Spring (normalmente utilizando `@Service`, `@Component`, o similar).

- [Javadoc - @Transactional](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)

- [Transactions with Spring and JPA - Baeldung](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)

- [Using @Transactional - Spring Framework](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html)

#### @NoRepositoryBean

La anotación `@NoRepositoryBean` en Spring se utiliza para indicar que una interfaz de repositorio específica no debe ser considerada como un repositorio gestionado por Spring Data. Esto significa que Spring no creará una instancia de esa interfaz de repositorio, y no se aplicarán las características típicas de repositorio, como la generación automática de consultas y métodos CRUD.

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

La anotación `@NoRepositoryBean` es útil cuando se requiere definir una interfaz base para repositorios que contengan métodos comunes o personalizados, pero que no represente un repositorio que deba ser instanciado directamente por Spring Data. Esto es especialmente útil en proyectos grandes donde se necesita una estructura de repositorios más compleja y reutilizable.

- [Javadoc - @NoRepositoryBean](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/NoRepositoryBean.html)

#### @Param

La anotación `@Param` permite pasar parámetros con nombre a nuestras consultas:

```java
@Query("FROM Person p WHERE p.name = :name")
Person findByName(@Param("name") String name);
```

Es importante referirse al parámetro con la sintaxis `:name`. Usar parámetros con nombre mejora la legibilidad y evita errores en las consultas.

`@Param` es útil tanto en consultas JPQL como en consultas SQL nativas.

También se puede usar `@Param` en consultas con múltiples parámetros:

```java
@Query("FROM Person p WHERE p.name = :name AND p.age = :age")
Person findByNameAndAge(@Param("name") String name, @Param("age") int age);
```

- [Javadoc - @Param](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/query/Param.html)

- [Spring Data JPA @Query - Baeldung](https://www.baeldung.com/spring-data-jpa-query)

#### @Id

La anotación `@Id` marca un campo en una clase de modelo como la **clave principal**:

```java
class Person {

    @Id
    Long id;

    // ...
    
}
```

Al ser independiente de la implementación, permite que una clase de modelo sea compatible con múltiples motores de almacenamiento de datos.

La clave principal es fundamental en el contexto de las bases de datos, ya que **identifica de manera única cada entidad**.

- [Javadoc - @Id](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/Id.html)

#### @Transient

Podemos usar esta anotación para marcar un campo en una clase de modelo como **transitorio**. Esto significa que el motor de almacenamiento de datos no leerá ni escribirá el valor de este campo:

```java
class Person {

    // ...

    @Transient
    int age;

    // ...

}
```

Al igual que `@Id`, la anotación `@Transient` también es independiente de la implementación, lo que facilita su uso con múltiples implementaciones de almacenamiento de datos.

Esta anotación es útil para campos **calculados o temporales** que no necesitan ser persistidos en la base de datos.

- [Javadoc - @Transient](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/Transient.html)

#### Campos de auditoría

En Spring Data, las anotaciones de campos de auditoría se utilizan para mantener un rastro automático de ciertas acciones comunes, como la creación y modificación de entidades. Estas anotaciones permiten que Spring Data gestione automáticamente campos como el usuario que creó o modificó la entidad, y las fechas de creación y modificación:

- **`@CreatedBy`**: marca un campo que debe contener el usuario que creó la entidad. Este campo se llena automáticamente cuando la entidad se persiste por primera vez.

- **`@LastModifiedBy`**: indica el usuario que realizó la última modificación a la entidad. Este campo se actualiza automáticamente cada vez que la entidad se modifica.

- **`@CreatedDate`**: marca un campo para almacenar la fecha y hora en que la entidad fue creada. Este campo se llena automáticamente cuando la entidad se persiste por primera vez.

- **`@LastModifiedDate`**: anota un campo que debe contener la fecha y hora de la última modificación de la entidad. Este campo se actualiza automáticamente cada vez que la entidad se modifica.

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

Para utilizar estas anotaciones, es necesario seguir algunos pasos adicionales para habilitar la auditoría en una aplicación Spring:

- Añadir la anotación `@EnableJpaAuditing` en una clase de configuración de Spring para habilitar la auditoría:

```java
@Configuration
@EnableJpaAuditing(auditorAwareRef = "auditorProvider")
public class AuditConfig {
}
```

- Implementar la interfaz `AuditorAware` para especificar cómo se obtiene el usuario actual:

```java
import org.springframework.data.domain.AuditorAware;
import org.springframework.stereotype.Component;

@Component("auditorProvider")
public class AuditorAwareImpl implements AuditorAware<User> {

    @Override
    public Optional<User> getCurrentAuditor() {
        // Lógica para obtener el usuario actual
        return Optional.of(new User("currentUser"));
    }
}

```

- [Auditing with JPA, Hibernate, and Spring Data JPA - Baeldung](https://www.baeldung.com/database-auditing-jpa)

- [Auditing - Spring Data](https://docs.spring.io/spring-data/jpa/reference/auditing.html)

### Spring Data JPA Annotations

- [Javadoc - Spring Data JPA](https://docs.spring.io/spring-data/jpa/docs/current/api/)

#### @Query

Con la anotación `@Query`, se proporciona una **implementación JPQL** para un método de repositorio:

```java
@Query("SELECT COUNT(*) FROM Person p")
long getPersonCount();
```

Además, se puede utilizar parámetros con nombre con la anotación `@Param`:

```java
@Query("FROM Person p WHERE p.name = :name")
Person findByName(@Param("name") String name);
```

También se pueden utilizar **consultas SQL nativas** configurando el argumento `nativeQuery = true`:

```java
@Query(value = "SELECT AVG(p.age) FROM person p", nativeQuery = true)
int getAverageAge();
```

La anotación `@Query` es útil para consultas complejas que no se pueden expresar fácilmente con los métodos de consulta derivados. Además, permite el uso de expresiones SpEL (_Spring Expression Language_) para consultas dinámicas.

- [Javadoc - @Query](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Query.html)

- [JPA Query Methods - Spring Data](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html)

- [Spring Data JPA @Query - Baeldung](https://www.baeldung.com/spring-data-jpa-query)

#### @Procedure

Con Spring Data JPA, podemos llamar fácilmente a procedimientos almacenados desde repositorios.

Primero, es necesario declarar el procedimiento almacenado en la clase de entidad usando anotaciones JPA estándar. La anotación `@NamedStoredProcedureQuery` se utiliza para definir procedimientos almacenados en la entidad:

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

Después de esto, se puede hacer referencia a él en el repositorio usando el nombre declarado en el argumento `name`:

```java
@Procedure(name = "count_by_name")
long getCountByName(@Param("name") String name);
```

La anotación `@Procedure` se utiliza para llamar a procedimientos almacenados definidos en la entidad, así como a otros procedimientos almacenados que estén definidos en otras entidades o directamente en la base de datos:

```java
@Procedure(procedureName = "person.count_by_name")
long getCountByNameDirect(@Param("name") String name);
```

Los procedimientos almacenados son útiles para encapsular lógica compleja en la base de datos y mejorar el rendimiento.

- [Javadoc - @Procedure](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/query/Procedure.html)

#### @Lock

La anotación `@Lock` en Spring Data JPA se utiliza para especificar el nivel de bloqueo que debe aplicarse a una consulta JPA en las operaciones de acceso a la base de datos. Esta anotación es fundamental para controlar la concurrencia y evitar problemas como las condiciones de carrera, especialmente en entornos donde múltiples transacciones podrían intentar acceder o modificar los mismos datos simultáneamente.

La anotación `@Lock` admite varios tipos principales de bloqueo:

- `LockModeType.NONE`: no se aplica ningún bloqueo. Este es el **modo predeterminado** si no se especifica un tipo de bloqueo.

- `LockModeType.PESSIMISTIC_READ`: este bloqueo permite que otras transacciones lean los datos, pero no permite actualizaciones. Es útil para evitar que los datos cambien mientras se está leyendo.

- `LockModeType.PESSIMISTIC_WRITE`: este bloqueo evita que otras transacciones lean o actualicen los datos mientras se mantiene el bloqueo. Es útil cuando se necesita asegurarse de que nadie más pueda leer o modificar los datos hasta que la transacción actual termine.

- `LockModeType.PESSIMISTIC_FORCE_INCREMENT`: adquiere un bloqueo de escritura pesimista e incrementa la versión de la entidad asegurando que las transacciones concurrentes vean la nueva versión de la entidad.

- `LockModeType.OPTIMISTIC`: utiliza el bloqueo optimista, que se basa en versiones. Permite que múltiples transacciones lean y actualicen los datos, pero verifica al final de la transacción si los datos han cambiado desde la última lectura. Si es así, lanza una excepción `OptimisticLockException`.

- `LockModeType.OPTIMISTIC_FORCE_INCREMENT`: similar al bloqueo optimista, pero además incrementa la versión de la entidad, asegurando que cualquier otra transacción que quiera leer los datos tendrá que esperar hasta que la transacción actual complete.

- `LockModeType.READ`: sinónimo de `OPTIMISTIC`, aunque se utiliza raramente en la práctica.

- `LockModeType.WRITE`: sinónimo de `OPTIMISTIC_FORCE_INCREMENT` también poco utilizado en la práctica.

Es importante considerar que los **bloqueos pesimistas** pueden afectar el rendimiento de la base de datos, ya que impiden que otras transacciones accedan a los datos bloqueados.

En cambio, los **bloqueos optimistas** son más adecuados para sistemas con alta concurrencia, ya que permiten una mayor escalabilidad al no bloquear los datos de manera exclusiva.

- [Javadoc - @Lock](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Lock.html)

- [Javadoc - LockModeType](https://jakarta.ee/specifications/persistence/2.2/apidocs/javax/persistence/lockmodetype)

- [Enabling Transaction Locks in Spring Data JPA - Baeldung](https://www.baeldung.com/java-jpa-transaction-locks)

- [Locking - Spring Data](https://docs.spring.io/spring-data/jpa/reference/jpa/locking.htm)

#### @Modifying

La anotación `@Modifying` se utiliza en Spring Data JPA para indicar que un método de repositorio modifica datos en la base de datos. Específicamente, se emplea en combinación con la anotación @Query cuando la consulta realiza operaciones de modificación como UPDATE, DELETE o INSERT.

Por ejemplo, se puede modificar el nombre de una persona en la base de datos con un método de repositorio anotado con `@Modifying`:

```java
@Modifying
@Query("UPDATE Person p SET p.name = :name WHERE p.id = :id")
void changeName(@Param("id") long id, @Param("name") String name);
```

Es importante destacar que, al usar `@Modifying`, es recomendable también gestionar las transacciones para asegurarse de que los cambios se persistan correctamente. Si el método no está en una transacción activa, Spring iniciará una automáticamente.

- [Javadoc - @Modifying](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Modifying.html)

- [Spring Data JPA @Query - Baeldung](https://www.baeldung.com/spring-data-jpa-query)

#### @EnableJpaRepositories

Para utilizar repositorios JPA, se le tiene que indicar a Spring mediante la anotación `@EnableJpaRepositories`.

Hay que tener en cuenta que esta anotación se debe usar con la anotación `@Configuration`:

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

En aplicaciones Spring Boot no es necesario usar esta anotación explícitamente, ya que Spring Boot lo configura **automáticamente** si detecta la dependencia de Spring Data JPA en el classpath.

Sin embargo, en configuraciones avanzadas, como cuando se trabaja con múltiples fuentes de datos o configuraciones de repositorios personalizados, puede ser necesario.

- [Javadoc - @EnableJpaRepositories](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/config/EnableJpaRepositories.html)

### Spring Data Mongo Annotations

- [Javadoc - Spring Data MongoDB](https://docs.spring.io/spring-data/mongodb/docs/current/api/)

- [Introduction to Spring Data MongoDB - Baeldung](https://www.baeldung.com/spring-data-mongodb-tutorial)

#### @Document

La anotación `@Document` marca una clase como un objeto de dominio que queremos conservar en una base de datos MongoDB:

```java
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;

@Document(collection = "user")
class User {

    @Id
    private String id;

    @Field("username")
    private String name;

    // Getters and setters...
}
```

Esta anotación también nos permite elegir el nombre de la colección que queremos utilizar a través del atributo `collection`.

Esta anotación `@Document` es el equivalente en MongoDB de `@Entity` en JPA. Al igual que `@Entity`, se utiliza junto con otras anotaciones como `@Id` para definir el campo que actúa como clave primaria, y `@Field` para mapear campos de la clase a nombres específicos en la colección.

- [Javadoc - @Document](https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/core/mapping/Document.html)

#### @Field

Con la anotación `@Field`, podemos configurar el nombre de un campo que queremos usar cuando MongoDB persiste el documento:

La anotación `@Field` en Spring Data MongoDB se utiliza para especificar el nombre de un campo en la base de datos que es diferente del nombre del campo en la clase Java. Esto es útil cuando se necesita que los nombres de los campos en MongoDB sigan una convención diferente a la de las propiedades en el código Java:

```java
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;

@Document(collection = "user")
class User {

    @Field("email")
    private String emailAddress;

    @Field("first_name")
    private String firstName;

    @Field("last_name")
    private String lastName;

    // Getters and setters...
}
```

Este ejemplo muestra cómo el campo `emailAddress` en Java se mapea al campo `email` en la colección MongoDB.

La anotación `@Field` es el equivalente en MongoDB de `@Column` en JPA. Al igual que `@Column`, se utiliza para mapear un nombre de campo en el código a un nombre específico en la base de datos.

Destacar que `@Field` es opcional si el nombre del campo en Java es el mismo que en MongoDB. Se usa principalmente cuando se necesita una diferenciación en los nombres.

- [Javadoc - @Field](https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/core/mapping/Field.html)

#### @Query

La anotación `@Query` en Spring Data MongoDB se utiliza para definir consultas personalizadas directamente en los métodos del repositorio. Esto es útil cuando necesitas realizar consultas que no pueden expresarse fácilmente con los métodos de consulta derivada proporcionados por Spring Data.

```java
import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.data.mongodb.repository.Query;
import java.util.List;

public interface UserRepository extends MongoRepository<User, String> {

    @Query("{ 'name' : ?0 }")
    List<User> findUsersByName(String name);

    @Query("{ 'age' : { $gt: ?0, $lt: ?1 } }")
    List<User> findUsersByAgeRange(int ageStart, int ageEnd);
}
```

En este ejemplo, la consulta `@Query("{ 'name' : ?0 }")` busca documentos en la colección donde el campo `name` coincide con el valor proporcionado por el parámetro `name` del método.

La consulta se define en formato JSON de MongoDB. El placeholder `?0` se refiere al primer argumento del método (en este caso, `String name`).

Puedes crear consultas más complejas utilizando operadores MongoDB. Por ejemplo, la siguiente consulta busca usuarios cuya edad está entre dos valores:

```java
@Query("{ 'age' : { $gt: ?0, $lt: ?1 } }")
List<User> findUsersByAgeRange(int ageStart, int ageEnd);
```

También es posible definir proyecciones para seleccionar solo ciertos campos de los documentos, optimizando así las operaciones de lectura.

- [Javadoc - @Query](https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/repository/Query.html)

#### @EnableMongoRepositories

La anotación `@EnableMongoRepositories` en Spring Data MongoDB se utiliza para habilitar la creación de repositorios basados en MongoDB. Esta anotación indica a Spring que busque interfaces de repositorio y las implemente automáticamente.

Para usar esta anotación, debes aplicarla a una clase de configuración marcada con `@Configuration`. Spring buscará las interfaces de repositorio en los subpaquetes de la clase configurada:

```java
@Configuration
@EnableMongoRepositories
class MongoConfig {}
```

Por defecto, Spring escaneará todos los subpaquetes del paquete en el que se encuentra esta clase `@Configuration`. Si deseas especificar un paquete concreto donde Spring debería buscar los repositorios, puedes utilizar el argumento `basePackages`:

```java
@Configuration
@EnableMongoRepositories(basePackages = "com.baeldung.repository")
class MongoConfig {}
```

Alternativamente, puedes usar `basePackageClasses` para especificar clases de un paquete, lo que también guiará a Spring a escanear los repositorios en esos paquetes:

```java
@Configuration
@EnableMongoRepositories(basePackageClasses = MyRepository.class)
class MongoConfig {}
```

En aplicaciones Spring Boot, esta anotación no es necesaria explícitamente. Spring Boot configurará automáticamente los repositorios si detecta que Spring Data MongoDB está presente en el classpath, es decir, si se ha incluido como dependencia en el proyecto.

- [Javadoc - @EnableMongoRepositories](https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/repository/config/EnableMongoRepositories.html)

## Spring Bean Annotations

El contenedor de Spring se encarga de crear los beans en la aplicación y de coordinar las relaciones entre estos objetos a través de la inyección de dependencias o DI.

A la hora de expresar una especificación de conexión de bean, Spring ofrece tres mecanismos principales:

- **Configuración explícita en XML**: los _beans_ se declaran en un fichero de configuración `.xml`.

- **Configuración explícita en Java (JavaConfig)**: los _beans_ se declaran usando la anotación `@Bean` en una clase de configuración anotada con `@Configuration`.

- **Análisis y detección implícita de beans y conexión automática de beans**: finalmente, podemos marcar la clase como componentes gestionados por el contenedor de Spring con una de las anotaciones del paquete [_'org.springframework.stereotype'_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/package-summary.html) y dejar el trabajo para el escaneo de componentes mediante `@ComponentScan`.

Todos los mecanismos son compatibles entre sí y no excluyentes, es decir, dos mecanismos pueden ser utilizados en la misma aplicación.

La recomendación es usar la **conexión automática** ya que requiere menos configuración explícita y además usar la **configuración explícita** en el caso de usar librerías y frameworks de terceros para conectar sus componentes porque en estos casos **no se dispone de acceso al código fuente** para anotar las clases con `@Component` o `@Service` o cualquier otra.

- [Spring Bean Annotations - Baeldung](https://www.baeldung.com/spring-bean-annotations)

### @ComponentScan

Spring puede **escanear automáticamente** un paquete en busca de _beans_ si el [escaneo de componentes está habilitado](#enableautoconfiguration).

La anotación `@ComponentScan` se utiliza junto a la anotación [`@Configuration`](#configuration) para especificarle a Spring qué paquetes debe escanear en busca de componentes anotados con `@Component`, `@Service`, `@Repository`, entre otros. Esta anotación configura los paquetes base que deben ser escaneados.

Podemos especificar los nombres de los paquetes base directamente con los argumentos `basePackages` (o su alias `value`):

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
class VehicleFactoryConfig {}
```

Además, podemos indicar **clases** en los paquetes base con el argumento `basePackageClasses`, que proporciona seguridad de tipos. Esta forma es útil para evitar inconsistencias durante la refactorización de nombres de paquetes o clases:

```java
@Configuration
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

Ambos argumentos son matrices, por lo que podemos proporcionar varios paquetes o clases para escanear:

```java
@Configuration
@ComponentScan(basePackageClasses = {VehicleFactoryConfig.class, OtherFactoryConfig.class})
class VehicleFactoryConfig {}
```

Si no se especifica ningún argumento, Spring escaneará automáticamente el mismo paquete donde está presente la clase anotada con `@ComponentScan`. Se recomienda que la clase de configuración o la clase principal de la aplicación esté en un paquete raíz para que todos los subpaquetes se incluyan en el escaneo.

Esto es común en aplicaciones Spring Boot, donde la clase principal anotada con `@SpringBootApplication` actúa como punto de partida para el escaneo. La anotación [`@SpringBootApplication`](#springbootapplication) es una meta-anotación, que incluye, entre otras, la anotación `@ComponentScan` por lo que el escaneo se hace automáticamente:

```java
package com.example.myapplication;

@SpringBootApplication
public class MyApplication { ... }
```

En este caso, Spring escaneará todos los subpaquetes, como _'com.example.myapplication.customer'_ o _'com.example.myapplication.order'_.

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

En Java 8, gracias a las anotaciones repetidas, se pueden utilizar varias anotaciones `@ComponentScan` en una clase de configuración, lo que permite especificar varias configuraciones de escaneo:

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

Alternativamente, se puede usar la anotación `@ComponentScans` para especificar múltiples configuraciones de `@ComponentScan`:

```java
@Configuration
@ComponentScans({ 
  @ComponentScan(basePackages = "com.baeldung.annotations"), 
  @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
})
class VehicleFactoryConfig {}
```

Spring también permite aplicar filtros para incluir o excluir determinados beans durante el escaneo. Esto se puede lograr con los atributos `includeFilters` y `excludeFilters`:

```java
@ComponentScan(
  basePackages = "com.baeldung.annotations",
  includeFilters = @ComponentScan.Filter(MyCustomAnnotation.class),
  excludeFilters = @ComponentScan.Filter(AnotherCustomAnnotation.class)
)
class VehicleFactoryConfig {}
```

- [Javadoc - @ComponentScan](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html)

- [Javadoc - @ComponentScans](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScans.html)

- [Spring Component Scanning - Baeldung](https://www.baeldung.com/spring-component-scanning)

- [Classpath Scanning and Managed Components - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/classpath-scanning.html)

- [Structuring Your Code - Spring Boot](https://docs.spring.io/spring-boot/reference/using/structuring-your-code.html)

### @Component

La anotación `@Component` es una anotación a **nivel de clase** en Spring que marca una clase como un _bean_ gestionado por el contenedor de Spring. Durante el [análisis de componentes con `@ComponentScan`](#componentscan), Spring Framework detecta automáticamente las clases anotadas con `@Component` y las registra en el contexto de aplicación como _beans_:

```java
import org.springframework.stereotype.Component;

@Component
class CarUtility {
    // ...
}
```

De forma predeterminada, el nombre del _bean_ registrado será el mismo que el nombre de la clase, pero con la inicial en minúscula (`carUtility` en este caso). Sin embargo, podemos especificar un nombre personalizado para el _bean_ utilizando el argumento `value` opcional:

```java
import org.springframework.stereotype.Component;

@Component("customServiceName")
public class MyService {
    public void performService() {
        System.out.println("Service is being performed");
    }
}
```

Otra forma de asignar nombres a un _bean_ es utilizando la anotación `@Named` de la **especificación de inyección de dependencias de Java JSR-330**. Esta anotación es equivalente a `@Component`, pero es parte de un estándar de inyección de dependencias más amplio:

```java
import javax.inject.Named;

@Named("customServiceName")
public class MyService {
    public void performService() {
        System.out.println("Service is being performed");
    }
}
```

Anotaciones como `@Repository`, `@Service`, `@Controller` y `@Configuration` son **meta-anotaciones** de `@Component`, lo que significa que heredan el comportamiento de `@Component`. Además de marcar clases como _beans_, estas anotaciones proporcionan una semántica más específica sobre el propósito del _bean_:

- [`@Repository`](#repository) ➔ Usada para clases que interactúan directamente con la base de datos. Ofrece funcionalidades adicionales, como la conversión automática de excepciones de persistencia.

- [`@Service`](#service) ➔ Indica que una clase contiene lógica de negocio. Aunque funcionalmente igual a `@Component`, su uso mejora la claridad semántica en el código.

- [`@Controller`](#controller) ➔ Se utiliza para clases que gestionan solicitudes HTTP en aplicaciones web. Además de registrarlas como _beans_, Spring las trata como controladores de peticiones.

- [`@Configuration`](#configuration) ➔ Indica que la clase proporciona métodos de configuración que producen _beans_.

Estas anotaciones también permiten a Spring aplicar comportamientos específicos. Por ejemplo, `@Repository` está asociada con la traducción de excepciones de bases de datos en Spring, mientras que `@Service` no tiene ese comportamiento por defecto.

- [Javadoc - @Component](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html)

- [Using JSR 330 Standard Annotations - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/standard-annotations.html)

### @Repository

Las clases DAO o _Repository_ representan la capa de acceso a la base de datos en una aplicación y deben anotarse con `@Repository`:

```java
@Repository
class VehicleRepository {
    // ...
}
```

Una de las ventajas clave de utilizar la anotación `@Repository` es que habilita la **traducción automática de excepciones de persistencia**. Esto significa que cualquier excepción lanzada por el proveedor de persistencia, como Hibernate o JPA, será capturada y traducida automáticamente por Spring en una subclase de `DataAccessException`, que es una excepción más general y manejable.

Por ejemplo, si Hibernate lanza una `ConstraintViolationException`, Spring la traducirá en una `DataIntegrityViolationException`, lo que facilita el manejo coherente de errores en la capa de acceso a datos.

Para asegurarse de que Spring traduzca automáticamente estas excepciones, es posible declarar explícitamente un bean de tipo `PersistenceExceptionTranslationPostProcessor`, aunque en la mayoría de los casos, Spring lo configura automáticamente.

```java
@Bean
public PersistenceExceptionTranslationPostProcessor exceptionTranslation() {
    return new PersistenceExceptionTranslationPostProcessor();
}
```

Este _bean_ permite que las excepciones nativas lanzadas dentro de las clases anotadas con `@Repository` se traduzcan correctamente a subclases de `DataAccessException`.

En aplicaciones basadas en Spring Boot, esta funcionalidad suele estar habilitada automáticamente si se utiliza un marco de persistencia como JPA o Hibernate, sin necesidad de declarar explícitamente el bean `PersistenceExceptionTranslationPostProcessor`.

- [Javadoc - @Repository](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Repository.html)

### @Service

La **lógica de negocio** de una aplicación normalmente reside dentro de la **capa de servicio**, por lo que usamos la anotación `@Service` para indicar que una clase pertenece a esa capa:

```java
@Service
public class VehicleService {
    // ...    
}
```

La anotación `@Service` es una especialización de `@Component`, por lo que las clases anotadas con `@Service` son detectadas automáticamente por el escaneo de componentes de Spring. Esto permite que Spring las registre como _beans_ y las gestione dentro del contenedor de aplicación.

A diferencia de `@Component`, que es una anotación genérica para cualquier componente de Spring, `@Service` se utiliza para marcar **servicios que contienen la lógica de negocio**.

Aunque `@Service` no tiene un efecto funcional adicional en comparación con `@Component`, su uso mejora la claridad y semántica del código al indicar que la clase realiza tareas relacionadas con la lógica de negocio.

- [Javadoc - @Service](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Service.html)

### @Controller

La anotación `@Controller` es una anotación a **nivel de clase** que le indica a Spring Framework que esta clase sirve como un controlador en el contexto de Spring MVC:

```java
@Controller
public class VehicleController {

    @GetMapping("/vehicles")
    public String listVehicles(Model model) {
        List<Vehicle> vehicles = vehicleService.findAll();
        model.addAttribute("vehicles", vehicles);
        return "vehicleList"; // nombre de la vista (ej. un archivo Thymeleaf o JSP)
    }
}
```

En este ejemplo, el método `listVehicles` maneja las solicitudes GET a la ruta </vehicles>, obtiene una lista de vehículos del servicio y los añade al modelo. Luego, retorna el nombre de la vista que se debe renderizar.

La anotación `@Controller` es parte del framework Spring MVC y se utiliza para manejar solicitudes web. Las clases anotadas con `@Controller` contienen métodos que gestionan las solicitudes HTTP y retornan vistas o datos.

Los métodos en una clase anotada con `@Controller` están destinados a manejar **solicitudes web**. Estos métodos suelen estar anotados con `@RequestMapping`, `@GetMapping`, `@PostMapping`, etc., para mapear las solicitudes a métodos específicos.

Las clases `@Controller` a menudo trabajan con modelos (`@ModelAttribute`) y vistas (_"ModelAndView"_). En una arquitectura MVC (Modelo-Vista-Controlador), `@Controller` se encarga de preparar datos del modelo y decidir qué vista debe ser renderizada.

Aunque `@Controller` se usa en aplicaciones Spring MVC, en aplicaciones Spring Boot, que suelen retornar JSON o XML, es común usar [`@RestController`](#restcontroller), que combina `@Controller` y `@ResponseBody`.

- [Javadoc - @Controller](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Controller.html)

### @Configuration

La anotación `@Configuration` se utiliza para definir clases que contienen métodos de configuración de _beans_ en Spring. Estas clases son utilizadas para crear y configurar _beans_ dentro del contexto de la aplicación.

Las clases anotadas con `@Configuration` pueden contener métodos anotados con `@Bean`. Estos métodos se encargan de instanciar, configurar y devolver los objetos que se registrarán como _beans_ en el contexto de la aplicación.

```java
@Configuration
public class AppConfig {

    @Bean
    public Engine engine() {
        return new Engine();
    }

    @Bean
    public Vehicle vehicle() {
        return new Vehicle(engine());
    }
}
```

En este ejemplo, la clase `AppConfig` define dos _beans_: `engine` y `vehicle`. El _bean_ `vehicle` depende del _bean_ `engine`, y Spring se encargará de la inyección de dependencias cuando se cree una instancia de `Vehicle`.

Esta anotación `@Configuration` es una **meta-anotación** o especialización de `@Component`, lo que significa que también se considera un componente de Spring y, por lo tanto, puede ser detectada automáticamente durante el escaneo de componentes si se usa `@ComponentScan` o `@SpringBootApplication`.

Las clases `@Configuration` pueden ser utilizadas para definir varias configuraciones dentro de una aplicación. Es común usar esta anotación para definir configuraciones específicas de diferentes módulos o áreas de la aplicación.

- [Javadoc - @Configuration](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)

- [Basic Concepts: @Bean and @Configuration - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/java/basic-concepts.html)

- [Using the @Configuration annotation - Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/java/configuration-annotation.html)

## Spring AOP

La **Programación Orientada a Aspectos (AOP)** es un paradigma de programación que se enfoca en separar las preocupaciones transversales del núcleo de la lógica del negocio.

Una preocupación transversal pueden definirse como cualquier **funcionalidad que afecta a varios puntos de una aplicación**, como los inicios de sesión, la gestión de transacciones, el registro (logging), la seguridad, y el manejo de excepciones.

La AOP es muy útil para modularizar estas funcionalidades que se repiten en diferentes partes de la aplicación y desacoplar estas preocupaciones de la lógica de negocio.

- [Aspect Oriented Programming with Spring - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop.html)

- [Spring AOP - Baeldung](https://docs.spring.io/spring-framework/reference/core/aop.html)

- [Introduction to Spring AOP - Baeldung](https://www.baeldung.com/spring-aop)

### Conceptos de AOP

- **Aspecto (_Aspect_)**

  Un aspecto es un módulo que encapsula una **preocupación transversal**. En términos simples, es **una clase que contiene la lógica** para una funcionalidad transversal, como el registro de auditoría, la seguridad o el manejo de transacciones.

- **Consejo (_Advice_)**

  Un _advice_ es **la acción que un aspecto realiza en un punto específico** en el programa. Es el código que se ejecuta cuando se encuentra un punto de unión. Define lo que se debe hacer y cuándo debe hacerse. Los tipos comunes de _advice_ son:

  - **`Before`**: Se ejecuta antes de que el método objetivo sea llamado.

  - **`After`**: Se ejecuta después de que el método objetivo ha sido llamado, independientemente de su resultado.

  - **`After Returning`**: Se ejecuta después de que el método objetivo retorna un resultado.

  - **`After Throwing`**: Se ejecuta si el método objetivo lanza una excepción.

  - **`Around`**: Envuelve la ejecución del método objetivo, permitiendo realizar acciones antes y después de que se llame al método, y también puede controlar si se llama o no al método objetivo.

- **Punto de Unión (_Join Point_)**

  Un punto de unión es **un punto en la ejecución del programa donde se podría aplicar el aspecto**, como la invocación de un método o la ejecución de un bloque de código. En Spring AOP, los puntos de unión son principalmente las invocaciones de métodos.

- **Corte Transversal (Pointcut)**

  Un pointcut es una **expresión que selecciona uno o más puntos de unión** donde un _advice_ debería ejecutarse. Define el **lugar exacto  donde se aplica un aspecto** en el código. Por ejemplo, se puede definir un _pointcut_ para todos los métodos en una clase específica o para métodos con un nombre particular.

- **Objeto de Intercepción (Proxy)**

  En AOP, un proxy es **un objeto creado en tiempo de ejecución que intercepta las llamadas** a los métodos de un objeto objetivo para aplicar el _advice_. Los proxies permiten que los aspectos se apliquen sin modificar el código original del objeto.

- **Objetivo (_Target Object_)**

  El objetivo es el objeto cuyo método está siendo interceptado. Es el **objeto en el que se aplican los aspectos**.

Estos términos no son específicos de Spring.  

- [AOP Concepts - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop/introduction-defn.html)  

### Implementación de AOP en Spring

**Spring AOP** es el framework de Spring para la programación orientada a aspectos. Utiliza **proxies dinámicos** para aplicar aspectos en tiempo de ejecución y se integra de manera efectiva con otros componentes de Spring. Spring AOP está implementado en Java puro.

El enfoque de Spring AOP hacia AOP difiere del enfoque de la mayoría de otros frameworks de AOP. El objetivo no es proporcionar la implementación de AOP más completa si no más bien proporcionar una implementación de AOP más **simple y centrada** en las necesidades más comunes de los desarrolladores de aplicaciones empresariales.

Algunas características y elementos clave de Spring AOP son:

- Spring AOP se puede usar con **AspectJ**, un framework completo de AOP para Java que ofrece más funcionalidades avanzadas. AspectJ permite una programación orientada a aspectos más extensa, pero Spring AOP es suficiente para muchas necesidades de desarrollo.

- Spring AOP utiliza **proxies dinámicos** para aplicar aspectos en tiempo de ejecución. Esto significa que puede aplicar aspectos solo a los _beans_ de Spring y únicamente a métodos públicos.

- Spring AOP se centra en aspectos que se aplican **antes, después o alrededor de la ejecución de métodos**. Esto cubre los casos de uso más comunes en aplicaciones empresariales.

- Spring AOP utiliza varias **anotaciones** para definir aspectos y _advice_, como `@Aspect`, `@Before`, `@After`, `@Around`, etc. Estas anotaciones son parte de AspectJ, pero Spring interpreta estas anotaciones utilizando su propia implementación de AOP.

- Los aspectos en Spring AOP pueden configurarse utilizando XML o la configuración basada en anotaciones.

Definición de un aspecto de ejemplo en **Spring AOP**:

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBeforeMethod() {
        System.out.println("Logging before method execution");
    }
}
```

En este ejemplo, `LoggingAspect` es un aspecto que se aplica antes de la ejecución de cualquier método en la capa de servicio (`com.example.service`). El método `logBeforeMethod()` es el _advice_. La anotación `@Before` indica que el método `logBeforeMethod` debe ejecutarse antes de la ejecución de los métodos especificados por el _pointcut_.

### @AspectJ support

**@AspectJ** se refiere a un estilo de declaración de aspectos utilizando clases Java normales anotadas con anotaciones específicas. Este estilo fue introducido por el proyecto AspectJ en la versión AspectJ 5.

Spring interpreta las anotaciones de AspectJ, utilizando una biblioteca proporcionada por AspectJ para el análisis y coincidencia de _pointcuts_. Sin embargo, el tiempo de ejecución de AOP sigue siendo puramente Spring AOP, y no hay dependencia del compilador o tejedor de AspectJ.

Esto significa que mientras que el estilo de definición de aspectos y los _pointcuts_ se basan en las anotaciones de AspectJ, la implementación en tiempo de ejecución y la aplicación de los aspectos se realizan a través de Spring AOP.

Este enfoque permite que Spring use un estilo de declaración de aspectos más moderno y expresivo sin necesidad de integrar completamente el compilador AspectJ en el proceso de construcción de la aplicación.

- [@AspectJ support - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop/ataspectj.html)

#### Enabling @AspectJ Support

Para activar el soporte de AspectJ con Java, agregue la anotación `@EnableAspectJAutoProxy`, como muestra el siguiente ejemplo:

Para activar el soporte de AspectJ en una aplicación Spring, se debe agregar la anotación `@EnableAspectJAutoProxy` en una clase de configuración. Esta anotación habilita el uso de _proxies_ basados en AspectJ, permitiendo que los aspectos definidos se apliquen a los beans gestionados por Spring:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@EnableAspectJAutoProxy
public class AppConfig {
    // Puedes definir otros beans aquí si es necesario
}
```

La anotación `@Configuration` indica que la clase contiene definiciones de _beans_ y configuraciones de Spring, mientras que `@EnableAspectJAutoProxy` activa la capacidad de AOP utilizando _proxies_, facilitando la integración de aspectos en la aplicación.

- [Enabling @AspectJ Support - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop/ataspectj/aspectj-support.html)

#### Declaring an Aspect

Con el soporte de @AspectJ habilitado, cualquier _bean_ definido en el contexto de aplicación que sea una clase anotada con `@Aspect` es automáticamente detectado por Spring y utilizado para configurar Spring AOP. A continuación se muestra un ejemplo de cómo declarar un aspecto:

```java
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class NotVeryUsefulAspect {
  // Métodos del aspecto se pueden definir aquí
}
```

Los aspectos, que son clases anotadas con `@Aspect`, pueden tener **métodos y campos**, al igual que cualquier otra clase en Java. Estos métodos pueden ser utilizados para definir el comportamiento del _advice_ en puntos de unión específicos.

Las clases de aspecto se pueden registrar como _beans_ normales mediante configuración XML de Spring, a través de métodos `@Bean` en clases anotadas con `@Configuration`, o hacer que Spring las detecte automáticamente mediante el escaneo del _classpath_, de la misma manera que cualquier otro _bean_ administrado por Spring, siempre y cuando esté anotada con `@Component`, o cualquier otra anotación de estereotipo como `@Service`, etcétera..

- [Declaring an Aspect - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop/ataspectj/at-aspectj.html)

#### Declaring a Pointcut

Los _pointcuts_ determinan los puntos de unión de interés y, por lo tanto, nos permiten controlar **cuándo se ejecutan los _advice_**. Spring AOP sólo admite puntos de unión de ejecución de métodos para _beans_ de Spring.

Una declaración de _pointcut_ tiene dos partes: una firma que comprende un nombre y cualquier parámetro, y una expresión de _pointcut_ que determina exactamente qué ejecuciones de métodos nos interesan.

En el estilo de anotación `@AspectJ` de AOP, una firma de _pointcut_ se proporciona mediante una definición de un método (este método debe tener un tipo de retorno `void`), y la expresión de _pointcut_ se indica mediante el uso de la anotación `@Pointcut`:

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.After;

@Aspect
public class MyAspect {

    // Definir el pointcut con una firma de método
    @Pointcut("execution(* com.example.service.*.*(..))")
    private void myPointcut() {
        // Método de marcador, el cuerpo está vacío
    }

    // Usar el pointcut en un consejo @Before
    @Before("myPointcut()")
    public void beforeAdvice() {
        System.out.println("Advice ejecutado antes del método objetivo");
    }

    // Otro consejo que usa el mismo pointcut
    @After("myPointcut()")
    public void afterAdvice() {
        System.out.println("Advice ejecutado después del método objetivo");
    }
}
```

Spring AOP soporta los siguientes **designadores de _pointcut_** de AspectJ (PCD) para su uso en expresiones:

- **execution**: Coincide con puntos de unión de ejecución de métodos. Este es el **designador principal** para trabajar con Spring AOP.

- **within**: Limita la coincidencia a los puntos de unión dentro de ciertos tipos.

- **this**: Limita la coincidencia a los puntos de unión donde la referencia del _bean_ es una instancia del tipo dado.

- **target**: Limita la coincidencia a los puntos de unión donde el objeto objetivo es una instancia del tipo dado.

- **args**: Limita la coincidencia a los puntos de unión donde los argumentos son instancias de los tipos dados.

- **@target**: Limita la coincidencia a los puntos de unión donde la clase del objeto que ejecuta tiene una anotación del tipo dado.

- **@args**: Limita la coincidencia a los puntos de unión donde el tipo en tiempo de ejecución de los argumentos reales pasados tiene anotaciones de los tipos dados.

- **@within**: Limita la coincidencia a los puntos de unión dentro de tipos que tienen la anotación dada.

- **@annotation**: Limita la coincidencia a los puntos de unión donde el método que se ejecuta tiene la anotación dada.

Spring AOP añade un designador de _pointcut_ adicional llamado **bean** y que no forma parte del estándar PCD de AspectJ. Este PCD te permite limitar la coincidencia de puntos de unión a un _bean_ de Spring con un nombre específico o a un conjunto de _beans_ de Spring con nombres específicos (cuando usas comodines). El PCD **bean** tiene la siguiente forma:

```java
@Aspect
public class MyAspect {

    // Definir el pointcut usando el designador de bean
    @Pointcut("bean(myBean)")
    public void myBeanPointcut() {
        // Método de marcador, el cuerpo está vacío
    }

    // Definir el pointcut usando el designador de bean combinado con una expresión normal
    @Pointcut("bean(myServiceBean) && execution(* com.example.service.MyService.*(..))")
    public void myCombinedPointcut() {
        // Método de marcador, el cuerpo está vacío
    }

    // Definir el pointcut usando el designador de bean con comodines
    // limita la coincidencia a los beans cuyos nombres terminan en Service.
    @Pointcut("bean(*Service)")
    public void serviceBeansPointcut() {
        // Método de marcador, el cuerpo está vacío
    }

    @Before("myCombinedPointcut()")
    public void beforeAdvice() {
        System.out.println("Advice ejecutado");
    }
}
```

Puedes combinar expresiones de _pointcut_ usando `&&`, `||` y `!`. También puedes referenciar expresiones de _pointcut_ por nombre:

```java
package com.xyz;

public class Pointcuts {

  @Pointcut("execution(public * *(..))")
  public void publicMethod() {}

  @Pointcut("within(com.xyz.trading..*)")
  public void inTrading() {}

  @Pointcut("publicMethod() && inTrading()")
  public void tradingOperation() {}

}
```

En aplicaciones empresariales, a menudo necesitas referirte a módulos y conjuntos particulares de operaciones desde varios aspectos. La recomendación es definir una **clase dedicada** para capturar **expresiones de _pointcut_ con nombre comúnmente usadas**, como en el siguiente ejemplo de `CommonPointcuts`:

```java
import org.aspectj.lang.annotation.Pointcut;

public class CommonPointcuts {

  /**
   * A join point is in the web layer if the method is defined
   * in a type in the com.xyz.web package or any sub-package
   * under that.
   */
  @Pointcut("within(com.xyz.web..*)")
  public void inWebLayer() {}

  /**
   * A join point is in the service layer if the method is defined
   * in a type in the com.xyz.service package or any sub-package
   * under that.
   */
  @Pointcut("within(com.xyz.service..*)")
  public void inServiceLayer() {}

  /**
   * A join point is in the data access layer if the method is defined
   * in a type in the com.xyz.dao package or any sub-package
   * under that.
   */
  @Pointcut("within(com.xyz.dao..*)")
  public void inDataAccessLayer() {}

  /**
   * A business service is the execution of any method defined on a service
   * interface. This definition assumes that interfaces are placed in the
   * "service" package, and that implementation types are in sub-packages.
   *
   * If you group service interfaces by functional area (for example,
   * in packages com.xyz.abc.service and com.xyz.def.service) then
   * the pointcut expression "execution(* com.xyz..service.*.*(..))"
   * could be used instead.
   *
   * Alternatively, you can write the expression using the 'bean'
   * PCD, like so "bean(*Service)". (This assumes that you have
   * named your Spring service beans in a consistent fashion.)
   */
  @Pointcut("execution(* com.xyz..service.*.*(..))")
  public void businessService() {}

  /**
   * A data access operation is the execution of any method defined on a
   * DAO interface. This definition assumes that interfaces are placed in the
   * "dao" package, and that implementation types are in sub-packages.
   */
  @Pointcut("execution(* com.xyz.dao.*.*(..))")
  public void dataAccessOperation() {}

}
```

- [Declaring a Pointcut - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop/ataspectj/pointcuts.html)

- [The AspectJ Programming Guide - Eclipse](https://eclipse.dev/aspectj/doc/released/progguide/index.html)

- [The AspectJ 5 Development Kit Developer's Notebook - Eclipse](https://eclipse.dev/aspectj/doc/released/adk15notebook/index.html)

#### Declaring Advice

El _advice_ está asociado con una expresión de _pointcut_ y se ejecuta antes, después o alrededor de las ejecuciones de métodos que coinciden con el _pointcut_.

La expresión puede ser una expresión de _pointcut_ en línea o una referencia a un _pointcut_ con nombre.

- [Declaring Advice - Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop/ataspectj/advice.html)

##### Before Advice

Puedes declarar un _advice_ que se ejecute antes en un aspecto utilizando la anotación `@Before`.

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class BeforeExample {

  // Expresión de pointcut en línea 
  @Before("execution(* com.xyz.dao.*.*(..))")
  public void doAccessCheck() {
    // ...
  }

  // Pointcut con nombre
  @Before("com.xyz.CommonPointcuts.dataAccessOperation()")
  public void doAccessCheck() {
    // ...
  }

}
```

Este _advice_ no tiene parámetros específicos, pero puede recibir un parámetro [`JointPoint`](https://eclipse.dev/aspectj/doc/released/runtime-api/org/aspectj/lang/JoinPoint.html)

```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

@Aspect
public class MyAspect {

    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {
        // Método de marcador, el cuerpo está vacío
    }

    @Before("serviceMethods()")
    public void beforeAdvice(JoinPoint joinPoint) {
        System.out.println("Antes de la ejecución del método: " + joinPoint.getSignature());
    }
}
```

La interfaz `JoinPoint` en Spring AOP proporciona una serie de métodos útiles que permiten acceder a información detallada sobre el método que está siendo interceptado.

- **`getArgs()`**: accede a los argumentos del método interceptado.

- **`getThis()`**: obtiene el objeto proxy de Spring AOP.

- **`getTarget()`**: obtiene el objeto objetivo que está siendo llamado.

- **`getSignature()`**: proporciona una descripción del método siendo aconsejado

- **`toString()`**: imprime una descripción legible del join point`

##### After Returning Advice

El _advice_ se ejecuta cuando la ejecución del método coincidente finaliza con normalidad. Se puede declarar utilizando la anotación `@AfterReturning`.

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.AfterReturning;

@Aspect
public class AfterReturningExample {

  @AfterReturning("execution(* com.xyz.dao.*.*(..))")
  public void doAccessCheck() {
    // ...
  }

}
```

A veces, se necesita acceso en el cuerpo del _advice_ al valor real que se ha devuelto. Se puede usar la forma de `@AfterReturning` que vincula el valor de retorno para obtener ese acceso con el parámetro `returning`, como muestra el siguiente ejemplo:

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.AfterReturning;

@Aspect
public class AfterReturningExample {

  @AfterReturning(
  pointcut="execution(* com.xyz.dao.*.*(..))",
    returning="retVal")
  public void doAccessCheck(Object retVal) {
    // ...
  }

}
```

##### After Throwing Advice

El _advice_ se ejecuta cuando la ejecución del método coincidente finaliza lanzando una excepción. Se puede declarar utilizando la anotación `@AfterThrowing`, como muestra el siguiente ejemplo:

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.AfterThrowing;

@Aspect
public class AfterThrowingExample {

  @AfterThrowing("execution(* com.xyz.dao.*.*(..))")
  public void doRecoveryActions() {
    // ...
  }

}
```

##### After (Finally) Advice

El _advice_ se ejecuta cuando una ejecución de método coincidente termina, ya sea que haya terminado normalmente o haya lanzado una excepción. Se declara utilizando la anotación `@After`. Se usa para realizar tareas de limpieza o liberación de recursos que deben ejecutarse sin importar si el método termina correctamente o con error.

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.After;

@Aspect
public class AfterFinallyExample {

  @After("execution(* com.xyz.dao.*.*(..))")
  public void doReleaseLock() {
    // ...
  }
}

```

##### Around Advice

Este _advice_ se ejecuta "alrededor" de la ejecución de un método coincidente. Tiene la oportunidad de realizar trabajo tanto antes como después de que el método se ejecute y puede determinar cuándo, cómo, e incluso si el método debe ejecutarse en absoluto.

Siempre se deb utilizarla forma de _advice_ menos más específica y menos invasiva que satisfaga los requisitos. Por ejemplo, no uses _"around advice"_ si _"before advice"_ es suficiente.

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.ProceedingJoinPoint;

@Aspect
public class AroundExample {

  @Around("execution(* com.xyz..service.*.*(..))")
  public Object doBasicProfiling(ProceedingJoinPoint pjp) throws Throwable {
    // start stopwatch
    Object retVal = pjp.proceed();
    // stop stopwatch
    return retVal;
  }

}
```

En Spring AOP, el _around advice_ requiere que se declare un primer parámetro de tipo [`ProceedingJoinPoint`](https://eclipse.dev/aspectj/doc/released/runtime-api/org/aspectj/lang/ProceedingJoinPoint.html), que es una subclase de `JoinPoint`.

## Spring Test

- [Testing in Spring Boot](https://docs.spring.io/spring-boot/reference/testing/index.html)

- [Testing in Spring Framework](https://docs.spring.io/spring-framework/reference/testing.html)

- [Testing in Spring Boot - Baeldung](https://www.baeldung.com/spring-boot-testing)

El proyecto `spring-boot-starter-test` es el _starter_ recomendado por **Spring Boot** para realizar pruebas en aplicaciones Spring. Este _starter_ agrupa y configura automáticamente un conjunto de librerías populares para facilitar la escritura de tests unitarios, de integración y de componentes en entornos Spring. Al incluir esta dependencia en el proyecto, se dispone de herramientas modernas para pruebas, aserciones, mocks, validación de JSON y utilidades para pruebas asíncronas, todo preconfigurado para funcionar de manera óptima con Spring Boot.

Las principales librerías incluidas son:

- **JUnit 5**: Framework estándar para la ejecución y organización de pruebas en Java.
- **Spring Test**: Utilidades y anotaciones para pruebas de integración con el contexto de Spring.
- **AssertJ**: Librería de aserciones fluida y expresiva.
- **Hamcrest**: Matchers para aserciones más legibles y flexibles.
- **Mockito**: Framework para la creación de objetos simulados (_mocks_) y pruebas de comportamiento.
- **JSONassert**: Herramienta para comparar y validar estructuras JSON en pruebas.
- **JsonPath**: Permite consultar y validar fragmentos de JSON.
- **Awaitility**: Facilita la escritura de tests para código asíncrono o basado en concurrencia.

Esto permite escribir pruebas robustas y expresivas, cubriendo desde la lógica de negocio hasta la integración de componentes y APIs REST, con una configuración mínima.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
```

Ejemplo de `@Service`:

```java
@Service
public class UserService {
    
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public Optional<User> findUserById(Long id) {
        return userRepository.findById(id);
    }
}
```

Test unitario del servicio:

```java
@ExtendWith(MockitoExtension.class) // Habilita Mockito en JUnit 5. Habilita el uso de @Mock y @InjectMocks.
class UserServiceTest {

    @Mock // Crea un objeto simulado de una dependencia (sin lógica real).
    private UserRepository userRepository;

    @InjectMocks // Crea una instancia real del objeto a probar (UserService) e inyecta los mocks automáticamente.
    private UserService userService;

    @Test
    void testFindUserById_UserExists() {
        // Arrange (preparación del escenario)
        User user = new User(1L, "Alice");
        when(userRepository.findById(1L)).thenReturn(Optional.of(user));

        // Act (ejecución del método a testear)
        Optional<User> result = userService.findUserById(1L);

        // Assert (verificación del resultado)
        assertTrue(result.isPresent());
        assertEquals("Alice", result.get().getName());
    }
}
```

Controlador REST de ejemplo:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return userService.findUserById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
}
```

Test usando `@WebMvcTest`:

```java
// Carga solo el controlador especificado y los beans necesarios para la capa web (como MockMvc). Ideal para pruebas de controladores REST.
@WebMvcTest(UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc; // Herramienta de Spring que simula llamadas HTTP sin levantar un servidor. Permite hacer get, post, etc., y validar respuestas.

    @MockBean // Mockea el UserService y lo inyecta en el controlador
    private UserService userService;

    @Test
    void testGetUser_ReturnsUser() throws Exception {
        // Arrange
        User user = new User(1L, "Alice");
        when(userService.findUserById(1L)).thenReturn(Optional.of(user));

        // Act & Assert
        mockMvc.perform(get("/users/1")) // Simula GET /users/1
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Alice"));
    }

    @Test
    void testGetUser_NotFound() throws Exception {
        // Arrange
        when(userService.findUserById(2L)).thenReturn(Optional.empty());

        // Act & Assert
        mockMvc.perform(get("/users/2"))
                .andExpect(status().isNotFound());
    }
}
```

Entidad y repositorio:

```java
// Entidad
@Entity
public class User {

    @Id
    private Long id;

    private String name;

    // Constructores, getters, setters
}

// Repository
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByNameContainingIgnoreCase(String name);
}
```

Test con `@DataJpaTest`:

```java
@DataJpaTest // Carga solo los componentes de JPA (repos, entidades, etc.)
class UserRepositoryTest {

    @Autowired
    private TestEntityManager entityManager; // Permite persistir datos para los tests

    @Autowired
    private UserRepository userRepository;

    @Test
    void testFindByNameContainingIgnoreCase() {
        // Arrange
        User user1 = new User(1L, "Alice");
        User user2 = new User(2L, "Alicia");
        User user3 = new User(3L, "Bob");

        entityManager.persist(user1);
        entityManager.persist(user2);
        entityManager.persist(user3);
        entityManager.flush();

        // Act
        List<User> results = userRepository.findByNameContainingIgnoreCase("ali");

        // Assert
        assertEquals(2, results.size());
        assertTrue(results.stream().anyMatch(u -> u.getName().equals("Alice")));
        assertTrue(results.stream().anyMatch(u -> u.getName().equals("Alicia")));
    }
}
```

---

## Enlaces

### Spring

- 🔸 [Spring](https://spring.io/)
- ⭐ [Spring Framework](https://spring.io/projects/spring-framework)
- ⭐ [Spring Boot](https://spring.io/projects/spring-boot)
- [Spring Framework Documentation](https://docs.spring.io/spring-framework/reference/index.html)
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/index.html)
- 📕 <https://goalkicker.com/SpringFrameworkBook/>
- <https://roadmap.sh/spring-boot>
- <https://eclipse.dev/aspectj/>

### Spring - Learning

- [Spring Framework Introduction](https://www.baeldung.com/spring-intro)
- [Learn Spring Boot](https://www.baeldung.com/spring-boot)
- [Spring Dependency Injection](https://www.baeldung.com/spring-dependency-injection)
- [Spring MVC Guides](https://www.baeldung.com/spring-mvc)
- [REST with Spring Tutorial](https://www.baeldung.com/rest-with-spring-series)
- [Security with Spring](https://www.baeldung.com/security-spring)
- [All Spring Data Guides](https://www.baeldung.com/spring-data)
- <https://spring.academy/>
- <https://spring.io/blog>
- <https://dev.to/burakboduroglu/spring-boot-cheat-sheet-460c>
- <https://www.geeksforgeeks.org/spring-boot/>
- <https://www.geeksforgeeks.org/spring/>
- <https://www.javatpoint.com/spring-boot-annotations>
- <https://www.adevguide.com/all-spring-annotations-cheat-sheet>
- <https://www.techferry.com/articles/spring-annotations.html>

## Licencia

[![Licencia de Creative Commons](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)
Esta obra está bajo una [licencia de Creative Commons Reconocimiento-Compartir Igual 4.0 Internacional](http://creativecommons.org/licenses/by-sa/4.0/).
