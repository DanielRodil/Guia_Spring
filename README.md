# SpringBasics

## GUIA PARA EL DESARROLLO DE UN PROYECTO JAVA-GRADLE-SPRING

### INDICE

1. [Inicializar un proyecto Spring con initialitzer](#inicializar-un-proyecto-spring-con-initialitzer)
1. [Iniciar Proyecto en Eclipse](#iniciar-proyecto-en-eclipse)
1. [Añadir mas dependencias a Gradle](#añadir-mas-dependencias-a-gradle)
1. [Creando la estructura de carpetas de mi proyecto](#creando-la-estructura-de-carpetas-de-mi-proyecto)
1. [Inyectando Beans](#inyectando-beans)
    - [4.1 Beans por XML](#41-beans-por-xml)
    - [4.2 Beans por clase de configuracion Java](#42-beans-por-clase-de-configuracion-java)
    - [4.3 Beans por @Anotaciones](#43-beans-por-anotaciones)
    - [4.4 Prelacion de Beans](#44-prelacion-de-beans)
    - [4.5 Desambiguando Beans](#45-desambiguando-beans)
1. [Arrancando con SPRINGBoot](#arrancando-con-springboot)
    - [5.1 Añadiendo recursos externos a Spring Boot](#51-añadiendo-recursos-externos-a-spring-boot)
    - [5.2 Insertando valores externos en campos de Beans](#52-insertando-valores-externos-en-campos-de-beans)
1. [Logs](#logs)
    - [6.1 Configuración logs](#61-configuración-logs)  
1. [Autowired](#autowired)
    - [7.1 Anotaciones](#71-anotaciones)``
1. [Persistencia de datos](#persistencia-de-datos)
    - [8.1 @Entity @Repository](#81-entity-repository)  
    - [8.2 ORM por XML](#82-orm-por-xml)  
    - [8.3 ORM por XML de clases con herencia](#83-orm-por-xml-de-clases-con-herencia)  
    - [8.4 Single Table](#84-single-table)  
    - [8.5 Persistencia de clases con relación @OneToMany](#85-persistencia-de-clases-con-relación-onetomany)  
1. [REST - Añadir la capa de presentación](#rest---añadir-la-capa-de-presentación)
    - [9.0 Prerrequisitos para Spring](#90-prerrequisitos-para-spring)
    - [9.1 Limitando el acceso a repositorios](#91-limitando-el-acceso-a-repositorios)
    - [9.2 Personalizar los objetos mostrados - Serializador Jackson y mixins](#92-personalizar-los-objetos-mostrados---serializador-jackson-y-mixins)
    - [9.3 Pasar a nivel 3 HATEOAS](#93-pasar-a-nivel-3-hateoas)
    - [9.4 Personalizar Endpoints con @RestResource](#94-Personalizar-Endpoints-con-@RestResource)  
1. [Métodos Personalizados](#métodos-personalizados)  
    - [10.1 Exponer método personalizado con @RepositoryRestController](#101-exponer-método-personalizado-con-@repositoryrestcontroller)
    - [10.2 Hacer Métodos Personalizados autodescubribles](#102-hacer-métodos-personalizados-autodescubribles)
    - [10.3 Añadir link a /resources/search](#103-añadir-link-a-resources-search)
    - [10.4 Evitar los filtros del CORS del navegador](#104-evitar-los-filtros-del-cors-del-navegador)
1. [Events y Listeners en JPA](#events-y-listeners-en-jpa)
1. [Servicio Entidad](#servicio-entidad)
1. [Integrando Librerias](#integrando-librerias)
    - [13.1 Integrar Proyecto Local](#131-integrar-proyecto-local)
    - [13.2 Integrar Proyecto de GitHub](#132-integrar-proyecto-de-github)
    - [13.3 Integrar Proyecto de Repositorio en la nube](#133-integrar-proyecto-de-repositorio-en-la-nube)
    - [13.4 Precedencia y exportacion de dependencias](#134-precedencia-y-exportacion-de-dependencias)

## Inicializar un proyecto Spring con initialitzer

Ideal → iniciar el proyecto directamente con initializer

↳ Si ya hubiera trabajo hecho → iniciar con lo ya hecho

↳ Tendria que inyectar dependencias al proyecto a mano con gradle ⇒ puede generar pegas

1. ir a la pagina [Spring Initalizer](https://start.spring.io/)

Crear con Initializer

→ proyecto gradle + java

→ version spring boot → recomendada

⇒ group es.mde → nombre

⇒ packing ⇒ .jar

→ java version → 8

⇒ Añadir dependencias

↳ Se podría añadir con el build.gradle → Spring lo suple

Añadir
⇒ mejor que sobre que no falte

- Spring Boot Dev Tools
- Spring Web
- Spring Web-services
- Spring Rest Repositories
- Spring Hateoas
- JDBC API
- Spring JDBC API
- Spring Data JPA
- H2 Database
- Postgre SQL Driver
  Opcionales
- Java mail sender

enlace resumen de lo [generado en el inicializer](https://start.spring.io/#!type=gradle-project&language=java&platformVersion=2.4.5.RELEASE&packaging=jar&jvmVersion=1.8&groupId=es.mde&artifactId=SpringBasics&name=SpringBasics&description=SpringBasics&packageName=es.mde.SpringBasics&dependencies=devtools,web,hateoas,jdbc,data-jpa,h2,postgresql,mail,web-services,data-rest,data-jdbc)

[Volver a inicio](#springbasics)

## Iniciar Proyecto en Eclipse

- abrir eclipse
- En el workspace -> importar proyecto GRADLE
- Seleccionar la carpeta del proyecto
- Check en `Override workspace settings` y en `Specific gradle version 7.2`
- next...finish
- ! debe tardar porque descarga todas las librerias
- Saldra una estructura de proyecto JAVA-Gradle

Para ejecutar -> **gradle Tasks -> aplication -> boot run**

- hay que acordarse de parar la ejecucion antes de volver a ejecutar

## Añadir mas dependencias a Gradle

añadir a build gradle

```
plugins {
	id 'org.springframework.boot' version '2.4.5'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
	id 'java-library'
	id 'eclipse'
	id 'application'
}
```

```
repositories {
	mavenCentral()
	jcenter()
	maven { url 'https://jitpack.io' }
}
```

⇒ Comprobar el paquete main a iniciar
⇒ con la anotacion **@SpringBootApplication** en el **main** seria suficiente

```
application {
  mainClassName = ruta.paquete.mainMio
}
```

Ejecutar:

- **`refresh gradle proyect`**
- **`boot run`** -> debe arrancar Spring sin errores

[Volver a inicio](#springbasics)

## Creando la estructura de carpetas de mi proyecto

Tendre la siguiente estructura:

- mde.es.**PaqueteBaseAplicacion**
  - incluira el **MAIN**
  - mi clase.java de configuracion por java -> contendra los **@bean**
- **paquete.entidades** -> con las entidades propias para persistir con anotaciones **@Entity**
- **paquete.repositorios** -> contendra todo lo relativo a lo que quiera persisitir en mi BD

  - entidadesInterfazDAO
    - Con la anotacion **@RepositoryRestResource** extends **JpaRepository<class, ID>**
  - interfaces DAOCustom + clases DAOCustom implementadas a persistir (para customizar)
  - listeners

- **paquete.rest** -> con los archivos para la implementacion rest (exposicion al front)
  - Clases con anotacion **@RepositoryRestController + @RequestMapping(path = "/ruta/{id}/elemento/search")**
    - Tendran un elementoDAO con las **@GetMaping("/rutaURL/")** (rutas de los metodos a exponer) + **@ResponseBody**
- **Resources** (generado por Spring) -> Contendra:
  - los archivos **.properties**
  - paquete **config** con:
    - jpa-config.xml -> mi **entity-manager**
    - properties de jackson o de REST
  - paquete **jpa**
    - con todos los archivos.**orm.xml** para cada clase a persistir

[Volver a inicio](#springbasics)

## Inyectando Beans

`**Restaurante Casa Porras commit 7c8e058b**`

Los Beans son objetos singleton que quiero utilizar, seran las clases que luego quiera persistir o mostrar por REST.
BEAN: objeto de unas características concretas que el framework Spring va a generar y guardar en un contenedor y cuando haga falta lo recuperará y lo inyectará donde sea necesario

Voy a inyectar Beans a mano **sin usar Spring**,

> comento las lineas de mi main
>
> ```
> //@SpringBootAplication
> //SpringApplication.run(SpringBasicsApplication.class, args);
> ```

1. Creo el contenedor y le digo a Spring dónde debe buscar para crear los beans y llenarlo

    ```
    ApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"config.xml"});
    ```

- tendre que importar las recomendaciones del eclipse que sean de Springframework
- puedo poner varios archivos de configuracion por XML pasandoselos al array separados por comas.
  - -> Deben estar en la carpeta main resources para que los encuentre. Seran de:
    - de configuracion donde le paso los beans que debe hacer xml + desacoplado
    - de scaneo- para que busque las anotaciones @component... (necesito acceso al codigo)
- Tambien puedo crear **¡¡¡un solo archivo de configuracion.xml!!!** con toda la informacion que necesite.

Para recuperar un **BEAN**

1. creo un objeto del tipo que quiero emplear

    ```
    ObjetoTipo variableDeMiObjeto = new ObjetoTipo();
    ```

2. a la variable anterior le asigno el bean del contenedor mediante el metodo **`.getBean`**.  El bean se creó antes (context), ahora lo que hago es recuperarlo.

   ```
   variableDeMiObjeto = context.getBean(ObjetoTipo.class)
   ```

   > importante el **.class**

Recuperar un **BEAN** con **alias**

- El bean tendra un alias en su etiqueta `@Bean(name="alias")`

  ```
  variableDeMiObjeto = context.getBean("alias", ObjetoTipo.class)
  ```

Creo mis clases que voy a convertir en Bean

- **¡¡¡OBLIGATORIO!!!** Tienen que tener:

  - setters
  - getters
  - constructor() por defecto

[Volver a inicio](#springbasics)

### 4.1 Beans por XML

Es el modo mas desacoplado de todos.

- Obligatorio cuando no tengo acceso al codigo (compilado)
- Me permite sobreescribir codigo ajeno

Creo el archivo de configuracion xml **`config.xml`** en `main/resources/`

   ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
           xsi:schemaLocation="http://www.springframework.org/schema/beans
                               http://www.springframework.org/schema/beans/spring-beans.xsd">

        <bean class="es.mde.Test" id="test" init-method="init">
            <property name="campoNombre" value="¡He sido inyectada por XML!"></property>
        </bean>
        <!--puedo poner mas beans-->
        <bean class="ruta.clase" id="alias">
        	<property name="campoBean"></property>
    	</bean>

    </beans>
   ```
[Volver a inicio](#springbasics)

### 4.2 Beans por clase de configuracion Java

`**Sesion 5: Restaurante Casa Porras commit a5fb7639**`

Es el modo intermedio.

1. Creo una clase Java que la llamare **ConfiguracionPorJava** (podría ser otro nombre pero se usa este por convenio) en la raiz del proyecto **al lado del Main**
2. Le añado la anotacion `@Configuration`
3. Creo dentro los metodos() que me van a devolver normalmente un objeto y les pongo la anotacion `@Bean`
   - Puedo usarlos para tunear mis beans o los objetos
   - Puede tener un alias `@Bean("alias")`
     - Da igual poner el `name=` si solo hay un elemento en el parentesis
   - Puedo ponerle un metodo inicial
     `@Bean(name="alias", initMethod="init")`
1. Necesito que la clase sea escaneada al crear el contenedor configurandolo con un xml

   - Tengo que añadir el XML al contenedor

[Volver a inicio](#springbasics)

### 4.3 Beans por @Anotaciones

`**Sesion 2: Restaurante Casa Porras commit 04bc20ce**`

Es el modo mas acoplado.

> **¡Necesito acceso al código!**

1. Pongo la anotacion `@Component` en la cabecera de la **clase** que sea un Bean
   - Tambien puedo usar las anotaciones que heredan de @component
     - `@Controller` -> para la presentacion
     - `@Service` -> Para crear servicios
     - `@Repository` -> Para Persistir
   - Pueden tener un alias `@Component("alias")`
   
2. Hago que spring escanee los elementos que tengan la anotacion **@Component** o herederas creando un archivo `config-scan.xml` en `resources` donde declaro que escanear (por defecto se escanean los que estan en la misma carpeta que el main y hacia abajo).
     
```
  <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-3.0.xsd">
	<!-- es.mde es el paquete base a partir del cual Spring escaneará y component la anotación que buscará -->
    <context:component-scan base-package="es.mde"/>

</beans>
```
3. Añado el config-scan.xml al contenedor `context`
     
```
   	ApplicationContext context = new ClassPathXmlApplicationContext(new String[] { "config-scan.xml" });
```

[Volver a inicio](#springbasics)

### 4.4 Prelacion de Beans

`**Sesion 3: Restaurante Casa Porras commit 55176812**`

Si usamos conjuntamente XML y @Component:

~~~
    ApplicationContext context = new ClassPathXmlApplicationContext(new String[] { "config-scan.xml", "config.xml" });
~~~

Ocurre que tenemos 2 beans asociados el mismo tipo, dado que los bean son singleton, y por tanto se debe establecer una prioridad.

- Prelacion:

  - 1º Beans por XML
  - 2º Beans por clase de configuracion Java
  - 3º Beans por anotaciones

[Volver a inicio](#springbasics)

### 4.5 Desambiguando Beans

`**Sesion 4: Restaurante Casa Porras commit 265257f5**`

Necesitaré usar alias para referirme a un Bean concreto

- Por anotaciones: alias en su anotacion `@Component("alias")` (context -> config-scan.xml)
- Por ConfiguracionPorJava: `@Bean("alias")`
- Por xml: `<bean class="ruta.clase" id="alias">`(context -> config.xml)

Para recuperar uso el **`.getBean`** sobrecargado.

```
variableDeMiObjeto = context.getBean("alias", ObjetoTipo.class)
```

[Volver a inicio](#springbasics)

## Arrancando con SPRINGBoot

`Sesion 6: Restaurante Casa Porras commit dc0fb96`

Una vez hecho el proyecto spring con initializer, me crea una serie de anotaciones por defecto

En el **`main`**, cuya clase se denominará como el proyecto con el sufijo **Application** (RestauranteCasaPorrasApplication) y se genera **automáticamente**
-  **`@SpringBootApplication`**
  Equivale a: 
  **`@Configuration + @EnableAutoConfiguration + @ComponentScan`**

     - Automaticamente escanea buscando `@anotaciones` en todos los archivos del paquete en el que se encuentra, **y en los que cuelguen de él** .
     - Lo que esta fuerade su paquete **¡NO lo escanea!** salvo que se lo diga expresamente.
     - Tambien escaneara el `application.properties` por defecto de Spring, que también se genera **automáticamente**

- **`SpringApplication.run(SpringBasicsApplication.class, args)`**

  - Sentencia que genera automaticamente el contenedor y en el que se almacenan los Beans que encuentre.
    - Puedo asignarlo a una variable para poder acceder con `.getBean()`
    ```
    ConfigurableApplicationContext context = SpringApplication.run(*nombre*Application.class, args);
    ```

-  Importante comentar la sentencia **`context.close()`** y parar y arrancar manualmente la API

    ```
	// context.close();
    ```

[Volver a inicio](#springbasics)

### 5.1 Añadiendo recursos externos a Spring Boot

Como Spring Boot solo escanea lo que este en su mismo paquete, le debo decir donde puede buscar mas recursos que quiera inyectar.

- Insertando por **config.xml**
  1. en el `main` le digo que debe escanear el recurso (resource) de mi archivo XML. 
  2. Agrego la anotacion
     `@ImportResource({"config.xml"})` 
     - Puedo poner mas archivos separados por comas `,`
  3. Ahi tendré las configuraciones al estilo Bean (se ve mas adelante)
- **Importando clases** de configuracion por java externas 
  - Añado al `main` la anotacion 
    `@Import({ruta.ClaseConfiguracionJava.class})` con la ruta a la clase

[Volver a inicio](#springbasics)

### 5.2 Insertando valores externos en campos de Beans

`Sesion 7: Restaurante Casa Porras commit 6a5f1c7f`

  Se van a inyectar mediante la anotacion **`@Value`** unos valores que provienen del fichero **`application.properties`**, que se genera y se lee automáticamente por spring.
  
 La clase **`ConfiguracionPorJava`** contiene los métodos que devuelven los objetos que se convierten en beans. Se puede hacer de 2 formas:
 
  1. Declarando la variable fuera del `@Bean` poniendo encima la anotación `@Value` y asignandole el valor referenciado en el `application.properties`.

     ```
     @Configuration
     public class ConfiguracionPorJava {
        
        @Value("${restaurante-casa-porras.idioma}")
        String idiomaPedido;
        
        @Bean
        public Pedido getPedido() {
        Pedido pedido = new Pedido();
        pedido.setIdioma(idiomaPedido);
        
        return pedido;
        }
     }
     ```

 1. Usando la anotación `@Value` en el constructor del `@Bean` y asignandole el valor referenciado en el `application.properties`. 
  
     ```
     @Configuration
     public class ConfiguracionPorJava {
        
        @Bean
        public Pedido getPedido(@Value("${restaurante-casa-porras.pedidoMinimo}") Double pedidoMinimo) {
        Pedido pedido = new Pedido();
		pedido.setPedidoMinimo(pedidoMinimo);
        
        return pedido;
        }
     }
     ```

 1. De esta manera se referencian los valores en el `application.properties`. Y desde aquí se modifican (de forma desacoplada del código) 
  
     ```
    restaurante-casa-porras.idioma=ingles
    restaurante-casa-porras.pedidoMinimo=50.00
     ```

[Volver a inicio](#springbasics)

## Logs

`Sesion 8: Restaurante Casa Porras commit 7538f86`

Objetivo: disponer de un registro de mensajes organizados relacionados con los sucesos que ocurren con la aplicacion.
Se puede hacer un log por cada singleton en la clase que quiera emplear
Se debe importar el logger de **slf4j** - Me dará acceso a los metodos de log

Se debe crear un objeto Logger estatico final:
```
private static final Logger log = LoggerFactory.getLogger(*nombre*Application.class);
```

Y después llamarlo:

```
Pedido beanPedido = context.getBean(Pedido.class);

log.error(beanPedido.toString());
log.warn(beanPedido.toString());
log.info(beanPedido.toString());
log.debug(beanPedido.toString());
log.trace(beanPedido.toString());
```

**Orden de prioridad:**

1 ERROR - Errores  
2 WARN - Alertas  
3 INFO - Nivel Por defecto. Mostrara de info hacia arriba  
4 DEBUG - Informacion importante para depuracion (por defecto no lo mostrará, habría que configurar)  
5 TRACE - Capta todo (por defecto no lo mostrará, habría que configurar)  

[Volver a inicio](#springbasics)

### 6.1 Configuración logs

Se puede configurar el logging a traves de `application.properties`:

Fuente: https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties

- Con esto indicamos al log que solo presente los log WARN y superior:
```
logging.level.root=WARN
```
- Podemos decirle desde donde, no le decimos desde root:
```
logging.level.es.med=DEBUG
```
- Salida de mensajes a fichero:
```
logging.file.name=archivo.log
```
- Formato fecha por defecto:
```
logging.pattern.dateformat=yyyy-MM-dd HH:mm
```
- Para modificar formato fecha:
```
porras.formatofecha=%date{ddMMM HH:mm:ss, UTC}Z
logging.pattern.console=${porras.formatofecha} [%thread %clr(${PID:- })] %-5level %logger{15} => %msg %n
```
- Para cambiar también el color:
```
logging.pattern.console=${porras.formatofecha} [%thread %clr(${PID:- })] %highlight(%-5level) %cyan(%logger{15}) => %msg %n
```
Fuente http://logback.qos.ch/manual/layouts.html

[Volver a inicio](#springbasics)

## Autowired

`Sesion 9: Restaurante Casa Porras commit 17d90c9`

Cuando en un Bean necesito que se **inyecte automaticamente** otro Bean podré utilizar `@Autowired` en el Bean que vaya a ser inyectado

> La anotacion solo se debe utilizar en metodos que reciban parametros
> Normalmente si solo hay un Bean que sea del tipo necesitado en mi contenedor, no sera necesario.

En un `@Component` puedo Utilizar Autowired en tres sitios:

- En el **Constructor** -> Cuando el parametro del constructor sea un **campo obligatorio**-> automaticamente pasara al constructor al cargar el Bean inyectado (si no puede crear un null)
- En un **setter** -> Cuando el campo sea opcional
- En el **campo** directamente -> Desaconsejado.

  ```
  @Component
  @Primary
  public class Pedido {
    ...
  @Autowired
  public Pedido (Comida comida) {
        syso("Estoy inyectando el bean de tipo comida al crear el bean pedido")
        this.comida = comida;
  }
  ```

  > Ejemplo de uso de @Autowired en constructor
  > El inyectar el bean de tipo pedido buscara un bean de tipo comida y lo inyectara para su empleo

[Volver a inicio](#springbasics)

### 7.1 Anotaciones 

##### Conflictos entre constructores del Bean Creado:
Si tengo varios constructores de mi bean con sobrecarga, Spring no va a saber cual usar.
- `@Autowired` == `@Autowired(required=true)` cogera ese constructor por defecto
  - Solo puede haber un required true en mi Bean.
- `@Autowired(required=false)` de entre todos los constructores que tengan esta anotacion cogera el que tenga mayor coincidencia en el numero de parametros pasados.

##### @Qualifier
Permite seleccionar el bean deseado cuando hay varios que encajan como parámetro de una ID (constructor o setter).
```
public void setPedido(@Qualifier("comida2") Comida comida) {
    this.comida=comida;
}
```

> Inyectara el Bean con el alias indicado

**ó**

##### @Primary
Permite seleccionar el bean deseado cuando hay varios que encajan en una Inyección de Dependencias
Si quiero que en todo el codigo se escoja un Bean por defecto cuando se tenga que inyectar, puedo usar **Primary** En la declaracion del Bean.

```
@Bean
@Primary
public Comida getComida() {
		Carne carne = new Carne();
		return carne;
}
```

> Solo puedo tener un Bean con primary en todos mis candidatos, sino tendré conflictos.

[Volver a inicio](#springbasics)

# Persistencia de datos

- Java Persistance API **JPA**  es la fachada, la API que vamos a utilizar para persistir los datos. 
- Se usará con la implementacion **Hibernate**, que realiza el mapeo entre objetos JAVA y registros en un SGBD relacional. Esto se conoce como ORM . 
- El objeto JAVA creado se va a serializar y los atributos de sus datos se van a guardar en una BBDD.

**RESUMEN**: Voy a almacenar **clases java** en un **SGBD Relacional** realizando un mapeo. **ORM** (Object-Relacional Mapping).

### 8.1 @Entity @Repository

`Sesion 10: Datos deportivos commit f35f7fc`

**`Entity Manager`**: **XML responsable de hacer que todo funcione**. Fichero de configuracion que me va a decir:

- Dónde están mis **repositorios-interfaces DAO** que van proporcionar los metodos CRUD a BD
- Dónde están las **clases-entidades** a persistir
- Archivos de "mapeo" personalizado de las entidades
- Datos de configuracion del SGBD que voy a emplear

##### 1- Configuración Entity Manager

1. Copio el [archivo **`jpa-config.xml`**](https://gist.github.com/Awes0meM4n/5bef4d556f960a696823235d188d5387#file-jpa-config-xml) en la carpeta `resources/config` de mi proyecto
2. Modifico lineas 10 y 16
    ```
    10 <jpa:repositories base-package="${mde.jpa-package}" />
    16 <property name="packagesToScan" value="${mde.misEntidades}" />
    ```
    La linea 10 indica donde buscar los **repositorios**, que son los **DAOs** (1 entidad = 1 DAO)
    La linea 16 indica la ruta del paquete donde buscar las **entidades** a persistir
    EN AMBOS CASOS, SE HACE REFERENCIA A VARIABLES CUYO VALOR SE ENCUENTRA EN EL ARCHIVO **APPLICATION.PROPERTIES**

[Volver a inicio](#springbasics)

##### 2- Configuración **`application.properties`**

1. Añado el valor de las variables del punto anterior:
    ```
    mde.jpa-package=mde.repositorios
	mde.misEntidades=mde.entidades
	```

1. Añado configuración BBDD:
    ```
   hibernate.dialect=org.hibernate.dialect.H2Dialect
   
   spring.datasource.url=jdbc:h2:tcp://localhost/~/test
   spring.datasource.username=sa
   spring.datasource.password=
   spring.datasource.driver-class-name=org.h2.Driver
	```

[Volver a inicio](#springbasics)

##### 3- BBDD abierta

- Mediante los siguientes dos comandos accedo a la Base de Datos **H2**
    ```
    cd /home/alumno6/.gradle/caches/modules-2/files-2.1/com.h2database/h2/1.4.200/f7533fe7cb8e99c87a43d325a77b4b678ad9031a
    
    java -jar h2-1.4.200.jar
    ```

> Seleccionar Generic H2 (Server)
> connect - Si da pega cambiar a embebed y volver a Server - Puede dar pega el puerto
> ↳ añadir al properties `server.port = 8082` → Conflicto con Tomcat porque dice que esta ocupado

[Volver a inicio](#springbasics)

##### 4- Anotaciones **`@Entity`** y **`@Repository`**
- La anotación `@Entity` se coloca justo antes de la clase JAVA que queremos **persistir**
- La anotación `@Id` se coloca justo antes del atributo que queremos que sea nuestra **Clave Primaria**
- La anotación `@GeneratedValue` genera el atributo de forma automática
- Para personalizar el nombre de la tabla, puede tener la anotacion `@Table`

    ```
    @Entity
    @Table(name="Usuarios")
    public class Usuario {
    
        @Id
	    @GeneratedValue
	    int id;
	    ...
    }
	```
[Volver a inicio](#springbasics)

##### 5- Interfaz **`DAO`**
- Por cada clase JAVA que queramos persistir necesitamos crear una Interfaz DAO, que se nombrará con el mismo nombre que la clase, pero acabado en el sufijo DAO `(Usuario => UsuarioDAO)`
- La notación `@Repository` (hija de `@Component`) se coloca justo antes de la interfaz
     ```
    @Repository
    public interface UsuarioDAO extends JpaRepository<Usuario, String> {
    }
	```
- **Usuario** es la entidad a la que hace referencia
- **String** es el tipo de la **Clave Primaria**
- Dentro de la interfaz se pueden añadir métodos de JPA que ya están implementados por defecto en **JPA Repository**, que va a ser el encargado de hacer el **CRUD** con la BBDD (  [JpaRepository](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html) ). Por ejemplo:
    ```
    public List<Usuario> findByNombreIgnoreCase(String txt);
	```

[Volver a inicio](#springbasics)

##### 6- Clase **`main`** (**nombre**Application.java)
- Debe incorporar las anotaciones **`@SpringBootApplication`** e **`@ImportResource`** (para decirle que busque en el Entity Manager) justo antes de la clase.
     ```
    @SpringBootApplication
    @ImportResource({"classpath:config/jpa-config.xml"})
	```
- **interfazDAO** es el bean que se va a usar para hablar con la BBDD
    ```
	Participante miParticipante = new Participante("Motril FC");
	ParticipanteDAO participanteDAO = context.getBean(ParticipanteDAO.class);
	participanteDAO.save(miParticipante);
	participanteDAO.deleteById(1);
	```
    > Creamos miParticipante, que es el que queremos guardar en la BBDD persistido.
    > Para ello necesitamos invocar nuestro DAO de tipo Participante, con el que vamos a hablar con la BBDD
    > Con ese participanteDAO le decimos que guarde a miParticipante en la BBDD mediante el método save o le decimos que lo borre de la BBDD especificando su id (ambos incluidos por defecto en el repositorio JPA)

[Volver a inicio](#springbasics)

### 8.2 ORM por XML

`Sesion 11: Datos deportivos commit dc87950`

- **OBLIGATORIO** usar cuando no se tiene acceso al codigo (compilado o librerias externas)

##### 1- Definimos la entidad a persistir vía **XML** (sin las anotaciones @Entity y @Id)
- Se crea un archivo **orm.xml** por entidad a persistir
- Se guardan en la carpeta `resources/jpa`
- **MODELO**:

```
<?xml version="1.0" encoding="UTF-8"?>
<entity-mappings xmlns="http://java.sun.com/xml/ns/persistence/orm"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://java.sun.com/xml/ns/persistence/orm
                                     http://java.sun.com/xml/ns/persistence/orm_1_0.xsd"
                 version="1.0">
    <!-- ruta completa de la clase a Persistir -->
                      <!-- access:"FIELD" = cuando se va a acceder directamente a los campos  -->
                      <!-- access"POPERTY"=cuando voy a acceder a los campos a traves de setter/getters  -->
  <entity class="es.lanyu.usuarios.repositorios.ClaseAPerisitir" access="FIELD">

      <!-- nombre de la tabla en la BD -->
    <table name="USUARIOS"/>

      <!-- Campos a persistir, apareceran asi en la BD-->
    <attributes>
      <id name="nombre">
        <!-- <generated-value strategy="IDENTITY"/> -->
        <column length="16"/>
      </id>
      <basic name="correo" optional="false" />
    </attributes>
  </entity>
</entity-mappings>
```

   - Los campos:
     - En la clase tiene que haber un campo ID **Obligatorio**
       - Si quiero que se cree de manera automatica el ID
         `<generated-value strategy="IDENTITY"/>`
     - El resto de campos, si no se pone nada se almacenan automaticamente
     - Si pongo alguno uso la etiqueta `<basic>` para poder ponerle alguna restriccion de campo
       - como atributo xml:
         - `optional="false"` -> campo obligatorio (no puede ser opcional)
       - como sub-nodo xml
         - `<column` + `/>`
         - `length="16"` -> Longitud maxima
         - `unique="true"`
     - Para omitir campos en la BD uso
       `<transient name="campo"/>`
     - Para personalizar el nombre de la tabla
       `<table name="Mi_Tabla"/>`
     - Para personalizar el nombre de las columnas
       `<column name="Mi_Columna"/>`

  > ¡NOTA! : Si la tabla esta creada y cambio las caracteristicas de ésta (campos opcionales, not null...) me dará error -> tengo que hacer `DROP TABLE` de la BD

[Volver a inicio](#springbasics)

##### 2- Modificaciones en el Entity Manager

- Se añade la ruta de los archivos **ORM** creados en la propiedad *mappingResources*.

    ```
    <property name="mappingResources">
			<list>
				<value>jpa/IdentificableString.orm.xml</value>
				<value>jpa/AbstractNombrable.orm.xml</value>
			</list>
	</property>
	```
> Tendré que tener igualmente mi `interfazDAO` de la clase a persistir.!!

[Volver a inicio](#springbasics)

### 8.3 ORM por XML de clases con herencia

`Sesion 12: Datos deportivos commit f8a7b69`

- Se dara el caso de que existan clases que hereden de otras y determinados campos no estén declarados en la clase hija (lo estarán en la clase padre), pero sí sean del objeto.
- En esos casos, usaremos la etiqueta `<mapped-superclass>` en lugar de `<entity>` (o la anotación **`@MappedSuperclass`** si tengo acceso al codigo) para las superclases padres que NO son entidades (aunque se han de declarar como si fueran entidades normales) 

> Hay que ir buscando las clases padres hasta encontrar las que tienen campos que sean heredados por las hijas

1. Crear `ClasePadre.orm.xml` (El hijo debe estar tambien hecho como si fuera un POJO)

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <entity-mappings xmlns="http://java.sun.com/xml/ns/persistence/orm"
                    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                    xsi:schemaLocation="http://java.sun.com/xml/ns/persistence/orm
                                        http://java.sun.com/xml/ns/persistence/orm_1_0.xsd"
                    version="1.0">

      <mapped-superclass class="es.ruta.ClasePadre" access="FIELD">
        <attributes>
          <id name="id" />
        </attributes>
      </mapped-superclass>

    </entity-mappings>
    ```

2. Crear la **interfazDAO** de la clase que vaya a persistir (el Hijo)
3. Agregar el `ClasePadre.orm.xml` al **`Entity-Manager`**

> **El ORM de la clase hija ya no tendra su campo ID, sera heredado**
> El ORM de la clase padre no tendra nombre de tabla.

[Volver a inicio](#springbasics)

### 8.4 Single Table

`Sesion 15: Datos deportivos commit 0348727`

Puede haber casos de especializacion de entidades que tengan una superclase en comun (Con campos comunes, ID-PK) y cada subclase tenga campos especificos diferentes.
Por rendimiento en las consultas puede ser adecuado almacenar ambas especializaciones en una misma Tabla **Single Table** asumiendo que:

- Necesitaré un campo discriminatorio **TipoSubclase**
- Tendre **null´s** en los campos que no sean comunes

> Prerrequisitos: Existira una `mapped-superclass` de la cual heredaran ambas subclases. Lo normal será que sea la superclase la que contenga el ID(Clave Primaria) de la tabla (seria un campo heredado para ambas)

En una de los **`orm.xml`** de una de las subclases le indico que va a ser una `Single Table`

1. Dentro de la etiqueta `<entity>` existente introduzco como cabecera:

   - **inheritance strategy="SINGLE_TABLE"** para indicar que es una Single Table (strategy se puede omitir por ser el valor por defecto)
   - **discriminator-column name="NombreColumna"** para indicar el nombre de la columna discriminatoria
   - **`+`** **discriminator value** para cada entidad con su valor correspondiente
   - ...el resto de atributos de la entidad

     ```
     <entity class="es.ruta.Clase" access="FIELD">
          <table name="NombretablaSingle"/>

          <inheritance strategy="SINGLE_TABLE"/>
          <discriminator-column name="TIPO"/>
          <discriminator-value>S</discriminator-value>

          <attributes>
              ...
          </attributes>
     </entity>
     ```

2. Agrego al `<entity-mappings>` la otra entidad como en un orm normal, añadiendo:

   - Una etiqueta que sea el **valor discriminatorio**

     ```
     <entity class="es.ruta.OtraClase" access="FIELD">

         <discriminator-value>T</discriminator-value>

         <attributes>
            ...
         </attributes>
     </entity>
     ```

     > A veces discriminator value Eclipse lo reconoce como error, pero funciona:)

Si lo quisiera hacer por **@anotaciones**, en la subentidades hay que añadir

1. En la subclase que genera la tabla:
   ```
   @Entity
   @Table(name="NombreTablaSINGLE")`
   @Access(value=AccessType.FIELD)
   @DiscriminatorValue("Subclase 1")
   public class ClaseTipo1 extends SuperClasePadre {...}
   ```
2. En el resto de subclases

   ```
   @Entity
   @Access(value=AccessType.FIELD)
   @DiscriminatorValue("Subclase 2")
   public class ClaseTipo2 extends SuperClasePadre {...}
   ```

3. Ambas deben igualmente heradar de la misma Superclase, ya que sera esta la que las vincule por el **Mapped-Superclass** y el **ID-PK**
4. Cada subclase debe tener su **interfazDAO** para poder persistir

[Volver a inicio](#springbasics)

### 8.5 Persistencia de clases con relación **`@OneToMany`**

`Sesion 13: Datos deportivos commit 9dfaefe`

Cuando tenga una entidad que contenga un campo que sea una lista(colección) de otros elementos usaré el **One to Many**.
Para que la BD mantenga la trazabilidad de los elementos de una tabla a la otra, se hace con la propagacion de la **Clave Ajena**.
Para que la BD pueda hacer la relacion entre ambas Tablas, almacenara la PK de la tabla **@One** (se convertirá en Clave Primaria) en un campo de cada elemento de la coleccion **@Many**.
**Prerrequisito:** ambas clases deben ser **@Entity** (tener su ORM y su DAO) y tener **ID**.

> Lo que voy a guardar en el campo FK será otro objeto.
> JPA solo guarda primitivos u objetos si se estan empleando las relaciones @OneToMany.
> Si no quiero que falle al intentar guardar un tipo objeto que no conoce, antes de hacer las relaciones, debo poner el campo como **transient** para que lo evite.

##### 1. Anotar el **OneToMany** en el **campo coleccion (listado)** que contiene la clase Contenedora:

(O lo que es lo mismo, buscar la clase donde se encuentra la colección))

    public abstract class EventoImpl extends DatableInstant implements Evento {
	protected Collection<Suceso> sucesos = new ArrayList<>();

   - Por **XML**. En el `orm.xml` de la clase que tiene la coleccion (añadir un atributo mas)
     - **name**: el nombre del campo de la clase contenedora
     - **target-entity**: ruta a la clase del elemento de la lista (no puede ser una interfaz)
     - **mapped-by**: campo Objetivo de la clase en el que se va a almacenar al padre-> debe ser un objeto y deben llamarse igual
       > Si no pongo el maped by hace una join table

        ```
        <mapped-superclass class="es.lanyu.comun.evento.EventoImpl" access="FIELD">
            <attributes>
                <one-to-many name="sucesos" 
                             target-entity="es.lanyu.eventos.repositorios.SucesoConId" 
                             mapped-by="partido"/>
            </attributes>
        </mapped-superclass>
        ```

   - Por anotación **`@OneToMany`**. En la clase que tiene la coleccion, le pongo al campo Coleccion
     - **mappedBy**: campo objetivo de la clase Elemento de la lista en el que se va a lamacenar al padre-> debe ser un objeto y deben llamarse igual

        ```
        @Entity
        @Table(name = "CLIENTES")
        public class Cliente {
    
        @OneToMany(cascade = CascadeType.ALL, targetEntity = Producto.class, mappedBy = "cliente")
        private Collection<Producto> productos;
        ```

##### 2. Añado el **ManyToOne** al elemento (un atributo mas)

   - **Por XML**. En el `orm.xml` de la clase Elemento de la coleccion (`SucesoConId.orm.xml`)
     - **name**: el nombre del campo tipo claseConColeccion
       - fetch="LAZY"-> (atributo Opcional, mejora rendimiento) hace que no se carque en memoria el objeto con   coleccion cada vez que se carque un elemento de la lista
         `+` nodo interno xml
     - **join-column name**: Clave Ajena (FK) -> valor a almacenar en la BD (no se puede llamar igual que otra columna de la tabla)
     - **referencedColumnName**: nombre de la columna en la BD que contendra la FK

        ```
        <table name="SUCESOS"/>
        <attributes>
            ...
            <many-to-one name="partido" optional="false"><!-- fetch="LAZY">-->
                <join-column name="ID_PARTIDO" referencedColumnName="ID"/>
            </many-to-one>
        </attributes>
        ```

   - Por anotacion **`@ManyToOne`**. En la clase Elemento de la coleccion, le pongo al campo Contenedor

        ```
        @Entity
        @Table(name = "PRODUCTOS")
        public class Producto {
        ...
        @ManyToOne(fetch = FetchType.LAZY)
        @JoinColumn(name = "ID_CLIENTE")
        private Cliente cliente;
        ```

       > `MUY IMPORTANTE` Como se puede observar, en la clase tipo ElementoDeLaLista **Creo un campo** del tipo ClaseContenedora que tiene la coleccion ("PadreContenedor"). Por ejemplo que la clase Producto tenga un atributo. Aquí será donde se almacene la FK en los objetos Elementos de la lista
       > `MUY IMPORTANTE` Crear Getter y Setter del campo para poder sincronizar en el siguiente apartado!!
        ```
        private Cliente cliente; 
        ```

##### 3. Crear un `metodo sincronizador` en la clase con la coleccion
   
   -> Cada vez que se añada un elemento a la coleccion se tiene que **propagar el ID** (PK) de la clase que tiene la lista, al elemento de la lista (a su campo Padre Contenedor -> FK)

    public class Cliente {
    ...
    @OneToMany
    ...
    public void addProducto(Producto producto) {
		getProductos().add(producto);
		producto.setCliente(this);
	}
    
   > Tengo que tener los getters/setters
   > Cuando vaya a añadir un elemento a la lista, tendre que utilizar mi metodo personalizado para que se propage la FK
   > La tabla de los elementos de la lista tendran una nueva columna que hara referencia al elemento que los contiene.

[Volver a inicio](#springbasics)

## REST - Añadir la capa de presentación

`Sesion 14: Datos deportivos commit 0348727`

**RE**presentational **S**tate **T**ransfer: define una interfaz para el acceso a la aplicacion a traves de un protocolo **HTTP**

Mediante las operaciones de http se puede acceder a las operaciones de la BD de CRUD usando url´s (por ejemplo si un partido tiene sucesos puedo navegar entre partidos, sucesos... a través de enlaces)

- **HTTP - BD**
- POST - CREATE
- GET - READ
- PUT - UPDATE
- DELETE - DELETE

Si la API es de nivel 3 (**HATEOAS**), implica que las llamadas a la BD sean autodescubribles mediante enlaces URL

### 9.0 Prerrequisitos para Spring

El proyecto Spring debe tener las **dependencias de REST** para el correcto funcionamiento.

Al arrancar la API en local, en el puerto que inicia TOMCAT [http://localhost:8080](http://localhost:8080) (si no se ha cambiado en el properties con `server.port=`), se podra acceder a todos los elementos del proyecto que sean `@Repository`

> Se puede acceder mediante **POSTMAN**
> Ya no será necesario crear elementos en el main, se harian desde POSTMAN (o el front)

**`ESQUEMA/RESUMEN DE LOS SIGUIENTES PUNTOS:`**
+ 1 - En las entidades colocar @OneToMany / @ManyToOne donde haga falta si lo hace
+ 2 - En los DAO cambiar @Repository por @RepositoryRestResource
+ 3 - En application.properties añadir 5 lineas (2 de configuración y 3 de jackson)
+ 4 - Crear la clase Mixins con anotaciones Json y public static interface
+ 5 - En ConfiguraciónPorJava añadir esos Mixins copiando código

[Volver a inicio](#springbasics)

### 9.1 Limitando el acceso a repositorios

Añadimos 2 propiedades para que spring solo muestre lo anotado con **`@RepositoryResource`** y el prefijo de la API (añadiendo RestResource a la anotación @Repository de los DAO, con eso ya hará que los repositorios se expongan y puedan ser consultados mediante peticiones REST, llamadas a la API)

1. En el **`application.properties`** añadir:
    ```
    spring.data.rest.basePath=/api
    spring.data.rest.detection-strategy=annotated
    ```
   > La primera establece el prefijo para la API. La ruta de acceso pasara a ser `http://url`**`/api`** -> se usa para versionado de api y pruebas
   > La segunda hace que solo se expongan los repositorios anotados con **RestResource**

    Para `deserializar` añadir también:
    ```
    spring.jackson.serialization.write_dates_as_timestamps=false
    spring.jackson.serialization.FAIL_ON_EMPTY_BEANS=false
    spring.jackson.default-property-inclusion=NON_NULL
    ```

2. Cambiar la anotaciones de las **`interfazDAO`** que se quieran mostrar de `@Repository` a `@RepositoryRestResource` y personalizar las rutas URL para el acceso en las mismas:
   - por defecto Spring pone nombres a las rutas que no seran amigables -> personalizar con:

    ```
    //@Repository
    @RepositoryRestResource(path="partidos",
                            itemResourceRel="partido",
                            collectionResourceRel="partidos")
    
    public interface PartidoDAO extends JpaRepository<PartidoConId, Long> {
    }
      ```

     - **path**: sera la ruta URL personalizada por la que accedo al recurso (SIEMPRE el nombre de la clase en plural)
     - **exported=false**: hace que el recurso no se muestre aunque tenga la anotacion
       Las siguientes hacen referencia a como se llamara el recurso dentro del Json devuelto
     - **itemResourceRel**="elemento" (SIEMPRE SINGULAR nombre cuando se hace referencia a uno)
     - **collectionResourceRel**="elementos" (SIEMPRE PLURAL nombre cuando se hace referencia a varios partidos)

    > **Muy Importante**: Necesitare serializar los objetos Elementos de una coleccion que se exponen con REST.
    > Si no lo hago, al exponerlo, expondria tambien al objeto claseColeccion (Padre), que volveria a exponer a los Elementos de la Lista -> **Entra en Bucle recursivo**
    > Hay que **ignorar el campo claseColeccion** (Padre) del elemento de la lista (el campo de la FK).


[Volver a inicio](#springbasics)

### 9.2 Personalizar los objetos mostrados - Serializador Jackson y mixins

Mediante **`Mixins`** se personaliza el objeto mostrado en las peticiones, dado que habrá campos:

- Que no se quieran mostrar
- Que se les quiera cambiar el nombre
- Que se quieran mostrar en un orden determinado.


1. En la clase **`ConfiguracionPorJava`** se añade un Bean de tipo **ObjectMapper** a la que se le añaden los mixins.

   - Para añadir un mixin, se usa el metodo `.addMixin(Clase, Mixin)` 
    -> cuando se crea un objeto del tipo Clase -> se personalizan sus campos con las anotaciones que haya en el Mixin.

    ```
    @Bean
    public ObjectMapper getObjectMapper() {
    
    ObjectMapper mapper = new ObjectMapper();
    
    mapper.addMixIn(Participante.class, MixIns.Participantes.class);
    mapper.addMixIn(Partido.class, MixIns.Partidos.class);
    // Incluido sobre las interfaces
    mapper.addMixIn(Suceso.class, MixIns.Sucesos.class);
    mapper.addMixIn(Gol.class, MixIns.Goles.class);
    
    return mapper;
    }
   ```

2. Los **`Mixins`** son clases java que se guardan en el paquete `rest` para personalizar nuestra información a la hora de presentarselo al usuario. Se debe haceruna interfaz estática por cada entidad que queramos personalizar mediante anotaciones de **@Jackson** para serializar y personalizar los campos. Existen anotaciones de tipo:

    ```
    public class Mixins {
    
	    @JsonPropertyOrder({ "fechaNacimiento", "nombreCompleto" })
	    @JsonIgnoreProperties(value = { "raza" })
	    public static interface Cliente {
    
	    	@JsonProperty("nombreCompleto")
		    abstract String getNombre();
	    }
    }
    ```
    > **JsonPropertyOrder**: Personaliza orden presentación datos a usuario
    > **JsonIgnoreProperty**: Ignora esa propiedad
    > **JsonProperty**: Cambia el nombre de la variable

La serializacion es bidireccional, se aplica en los GET (salida) y en los POST (entrada) desde el front, es decir al mandar objetos tienen que tener la misma estructura con los mismos nombres en los campos.

[Volver a inicio](#springbasics)

### 9.3 Pasar a nivel 3 HATEOAS

Hará que los enlaces sean autodescubribles, es decir, donde antes me ponia un objetoConColeccion en un campo de un elemento (me ponia un JSON), y tenia el problema de la recursividad en los objetos
-> me saldra un hipervinculo:

- un enlace al padre en el campo del objetoConColeccion del elemento
- y enlaces a cada uno de los elementos de la coleccion en campo lista del objetoConColeccion
  -> me permite navegar de un objeto a otro

> **ya no tendre que ignorar** el campo del elemento que me generaba la recusividad al elementoConColeccion (el campo de la FK)

Necesito acceder a los campo para hacer anotaciones (**solo se puede hacer con anotaciones**)

  > Si no tengo acceso
  >
  > 1. hago una claseImpl hija que extienda a la que estoy haciendo la persistencia-REST. En la carpeta repositorios (al lado de su DAO).
  > 2. Sobreescribo el metodo -> `@Override` ... getColeccion()

Pasos:

1. Pongo la anotacion `@OneToMany` y le pongo la **targetEntity** al elemento de la lista (tiene que ser un objeto implementado, no puede ser una interfaz)

    ```
    //@Entity(name="PARTIDOS")
    public class PartidoConId extends Partido {
    ...
	@Override
	@OneToMany(targetEntity=SucesoConId.class)
	public Collection<Suceso> getSucesos() {
		return super.getSucesos();
	...
	}
	```

1. Pongo la anotacion `@ManyToOne` en el campo **FK** del Elemento que hace referencia al elementoConColeccion

    ```
    //@Entity(name="SUCESOS")
    public class SucesoConId extends SucesoImp {
    ...
	@ManyToOne
	PartidoConId partido;
	...
	}
	```
1. Creo el ORM (por anotaciones o xml) si no estuviera hecho ya. (si he hecho la clase heredera -> lo tendre que hacer)


[Volver a inicio](#springbasics)

### 9.4 Personalizar Endpoints con **`@RestResource`**

Con esto vamos a poder personalizar o sobreescribir el comportamiento de los metodos que ofrece **JPA Repository**.
Al crear repositorios (**ClienteDAO**) extendiendo JPA Repository, nos proporciona directamente los métodos CRUD. 
```
public interface ClienteDAO extends JpaRepository<Cliente, Long> {...}
```
Mediante dicho repositorio se tiene acceso a otros que ahora podemos personalizar [metodos **"Query"**](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repository-query-keywords)
A traves del hiperenlace **Profile** de cada entidad puedo acceder a todos los metodos que se pueden realizar sobre esa entidad desde una llamada HTTP.

`url/api/profile/pathElemento`

Si voy a la InterfazDAO puedo hacer @Override de los metodos heredados de JpaRepository (ClickSecundario - source - Override)

Al sobreescribirlos puedo anotarlos Con **@RestResource** y personalizar:

- `@RestResource(exported = false)` -> para que no se muestren
- `@RestResource(path="name")` -> para añadir una ruta personalizada al metodo

Puedo hacer metodos personalizados utilizando la sintaxis del enlace en la firma del metodo para que me realice una consulta personalizada.

JPA -> trocea el String del nombre del metodo y lo transforma en una consulta SQL.

> las consultas solo funcionan sobre campos de existentes de las tablas -> Si se necesita hay que exponer con rest controler

Las Consultas generales son:

- findBy
- getBy
- deleteBy
  `+` 
  - NombreDeLaColumnaDeLaTabla 
    - `AND`, `OR` 
      - OtraColumna
        `+` modificadores: 
        - IgnoreCase 
        - Containing 
        - Between
  
  Quedando asi:

  ```
  List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
  ```
  > Equivaldria +/- a:
  SELECT person
  FROM tablaEntidadInterfazDao
  WHERE emailAdress="parametro1" AND lastName="Parametro2"

    > **Los delete en principio solo funcionan por ID

#### **`@Param`**

Se pasarán parametros por la URL, que se han de asignar al metodo personalizado con **@Param**

- Automaticamente coge el valor String de la URL y lo inserta como parametro al metodo donde este la anotacion.

```
ObjetoRetorno metodoFindBy(@Param("ParametroEnlaURL") String parametroLocal);
```

Los parametros por la URL se pasan con el simbolo `?`
`url/search/metodo?nombreParametroEnlaURL=valor&otroParametro=valor`:

#### Añadir los Query personalizados:

1. Ir a la interfazDAO de la entidad y en el cuerpo de la interfaz añadir el metodo Query con los parametros necesarios
2. Ponerle la anotacion **@RestResource**
3. Añadir Path personalizado y parametros de la URL si hubiera
   ```
   @RestResource(path="personaemail")
   List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
   ```

> Puedo encontrar los metodos personalizados por la URL: en el **`http://localhost:8080/api/.../search`** de cada recurso (JPA o personalizados)

> ¿Que quiere decir **`http://localhost:8080/api/.../search/por-nombre{?nombre}`** ?
```
public interface ClienteDAO extends JpaRepository<Cliente, Long> {
	
@RestResource(path="por-nombre")
List<Cliente> findByNombreIgnoreCase(@Param("nombre")String txt);
```
[Volver a inicio](#springbasics)

## Métodos Personalizados

`Sesion 18: Datos deportivos commit c2d5fd5`

JPA está limitado a la hora de hacer consultas a la BD, dado que solo ofrece los querys con una pequeña personalización a través de la sintaxis de la firma de los métodos. Tienen una relación directa con las columnas de la BD (por ejemplo se busca Participantes usando su campo nombre). 
Pero habrá casos que no sea así: nos gustaría por ejemplo recuperar los Partidos de un Participante con un texto en su nombre pero en dicha tabla Partidos no están los nombres de los participantes, solo sus ids, y ya no es suficiente con usar los query methods de JPA.
Para solucionarlo, en caso de querer hacer consultas mas elaboradas que requieran el **Join** de varias tablas, y que las consultas no esten relacionadas por FK, se ha de hacer un metodo personalizado (**Custom**).

**PASOS** (Siempre lo mismo!!)

1.  Crear en la carpeta `repositorios` una interfaz **`nombreClaseDAOCustom`** donde declaramos los métodos personalizados sin implementar.
    ```
    public interface ClienteDaoCustom {

	List<Cliente> getClientesConFechaPosterior(Instant fecha);
	List<Cliente> getClientesConFechaAnterior(Instant fecha);
	List<Producto> getProductosDelClienteActivos(Long id);
    }
    ```

1.  Creo en el paquete `repositorios` una clase **`nombreClaseDAOImpl`**.
    - Justo antes de la clase tendrá la anotación **`@Transactional(readOnly = true)`** -> Permite que se compartan las consultas de la clase en el acceso a la BD
    - Implementará la interfaz `nombreClaseDAOCustom` ⇒ Sera en esta clase donde se desarrollen  sus metodos
    - Tiene que tener la anotación `@Autowired` para que se autoinicialize con el bean de la interfaz **nombreClaseDAOCustom**.
    - Tiene que tener la anotación `@PersistenceContext`, que es similar al @Autowired pero para el Entity Manager.
    - Finalmente, se `implementan los métodos personalizados` con la anotación `@Override`.

    ```
    @Transactional(readOnly = true)
    public class ClienteDAOImpl implements ClienteDaoCustom {

	    @Autowired
	    ClienteDAO clienteDAO;

	    @PersistenceContext
	    EntityManager entityManager;

	    @Override
	    public List<Cliente> getClientesConFechaPosterior(Instant fecha) {
		    List<Cliente> clientes = clienteDAO.findAll().stream().filter(j -> j.getFechaNacimiento().isAfter(fecha))
				.collect(Collectors.toList());

	    	return clientes;
	    }

	    @Override
	    public List<Cliente> getClientesConFechaAnterior(Instant fecha) {
		    List<Cliente> clientes = clienteDAO.findAll().stream().filter(j -> j.getFechaNacimiento().isBefore(fecha))
				.collect(Collectors.toList());

		    return clientes;
	    }
        
        @Override
        public List<Producto> getProductosDelClienteActivos(Long id) {
		    List<Producto> productos = clienteDAO.findById(id).get().getProductos().stream()
				.filter(j -> j.getActivo() == true).collect(Collectors.toList());

		    return productos;
	    }
        
    }
    ```

1.  El DAO original (`InterfazDAO`) debe implementar tambien la `InterfazDAOCustom`
    - Spring buscará la implementacion a traves del **Impl** y el metodo personalizado se podra emplear para atacar la BD desde la IntfzDAO original.
    - El metodo no estará expuesto (link Hateoas) -> Necesito un controlador
    
    ```
    public interface ClienteDAO extends JpaRepository<Cliente, Long>, ClienteDaoCustom {
    ```
[Volver a inicio](#springbasics)

### 10.1 Exponer método personalizado con @RepositoryRestController

`Sesion 19: Datos deportivos commit c2d5fd5`

Con los pasos realizados en el punto anterior tenemos el método personalizado implementado pero **NO** lo estamos exponiendo (es decir, no se ve con un `GET a .../search`)

**PASOS**

1.  Añado el **Controlador** creando dentro del paquete `rest` una clase **`ClienteController`**

    - Sera la responsable de exponer el metodo en la API
      → lo expongo con la anotacion **@RepositoryRestControler**
      → Le añado el path a la url con **@RequestMapping(path = /clientes/search)** para toda la clase

    - **SIEMPRE** Tendra un atributo del tipo **ClienteDAO**, que se insertara por el **constructor** y me permitira acceder al metodo personalizado

    ```
    @RepositoryRestController
    @RequestMapping(path = "/clientes/search")
    public class ClienteController {

      private ClienteDAO clienteDAO;

      public ClienteController(ClienteDAO clienteDAO) {
        this.clienteDAO = clienteDAO;
      }
      ... Metodos personalizados
    }
    ```

1.  En la misma clase controlador **ClienteController**, se implementan los **métodos personalizados**
    
    ⇒ Tendra las anotaciones en el método:

    - **`@GetMapping("/search/ruta-final-metodo")`** - define la ruta al metodo despues del `/search`
      - Puede ser Get, Post...
      - Es equivalenete a @RequestMapping(method=GET,path=..).
    - **`@ResponseBody`** -> Spring hace que lo que se devuelva se trate como una respuesta web
      
      ↳ Devuelve una colección del tipo `CollectionModel<PersistentEntityResource>` o un objeto el tipo `EntityModel<PersistentEntityResource>` 
      
      ↳ recibe como parametro un objeto del tipo **assembler**
      
      ↳ Si recibe un parametro por la URL se anota en los parametros con **`@RequestParam`** equivale a **@Param**
      `PersistentEntityResourceAssembler assembler` - sera el encargado de conformar el objeto de salida para que la capte una llamada Http

      ````
      ...
      @GetMapping("/search/con-fecha-nacimiento-posterior-a")
      @ResponseBody
      public CollectionModel<PersistentEntityResource> getClientesConFechaPosterior(@RequestParam("fecha") Instant fecha, PersistentEntityResourceAssembler assembler) {
            List<Cliente> clientes = clienteDAO.getClientesConFechaPosterior(fecha);
        
            return assembler.toCollectionModel(clientes);
      }
      
      @GetMapping("/search/con-fecha-nacimiento-anterior-a")
      @ResponseBody
      public CollectionModel<PersistentEntityResource> getClientesConFechaAnterior(@RequestParam("fecha") Instant fecha, PersistentEntityResourceAssembler assembler) {
            List<Cliente> clientes = clienteDAO.getClientesConFechaAnterior(fecha);
            
            return assembler.toCollectionModel(clientes);
      }
      
      @GetMapping("/{id}/search/activos")
      @ResponseBody
      public CollectionModel<PersistentEntityResource> getProductosDelClienteActivos(@PathVariable Long id, PersistentEntityResourceAssembler assembler) {
            List<Producto> productos = clienteDAO.getProductosDelClienteActivos(id);
            
            return assembler.toCollectionModel(productos);
      }
      ````

[Volver a inicio](#springbasics)

### 10.2 Hacer Métodos Personalizados autodescubribles

Para que el link sea autodescubierto hay que añador un Bean (en la clase de configuracion por java por ejemplo)

Se se sobreescribe el objeto de Spring y que gestiona los hiperenlaces y se le añade el enlace a nuestro metodo personalizado.

```
@Bean
RepresentationModelProcessor<RepositorySearchesResource> searchLinks(RepositoryRestConfiguration config) {
    return new RepresentationModelProcessor<RepositorySearchesResource>() {

        @Override
        public RepositorySearchesResource process(RepositorySearchesResource searchResource) {

            //le indico la entidad donde tiene que insertar el enlace
            if (searchResource.getDomainType().equals(MiEntidad.class)) {

              //si el enlace coincide lo insertara con el siguiente bloque

                try {

                    //Le digo el nombre de mi metodo personalizado
                    String nombreMetodo = "getMiMetodoPersonalizado";

                    //busco en la entidad el metodo que coincide con el mio personalizado(impoorto de java reflex)
                    //(le tengo que decir tambien los parametros que recibe + Assembler)
                    Method method = PartidoController.class.getMethod(nombreMetodo, String.class,
                            PersistentEntityResourceAssembler.class);

                    //capto la URI de la entidad con linkTo
                    URI uri = org.springframework.hateoas.server.mvc.WebMvcLinkBuilder
                            .linkTo(method).toUri();

                    //me construyo la url al metodo
                    String url = new URI(uri.getScheme()
                                        , uri.getUserInfo()
                                        , uri.getHost()
                                        , uri.getPort()
                                        , config.getBasePath() + uri.getPath()
                                        , uri.getQuery()
                                        , uri.getFragment()).toString();
                          //el ?txt sera el parametro recibido
                    searchResource.add(new Link(url + "{?txt}", nombreMetodo));
                } catch (NoSuchMethodException | URISyntaxException e) {
                    e.printStackTrace();
                }
            }

            return searchResource;
        }

    };
}
```

### 10.3 Añadir link a /resources/search

`Sesion 20: Datos deportivos commit c2d5fd5`

Ya tenemos el método personalizado accesible dcon esa llamada GET a la URL configurada. Ahora se va a conseguir que ese método sea autodescubrible, como debe ser una API nivel 3 HATEOAS `inyectando el siguiente bean` adaptado a nuestras necesidades en la clase **`ConfiguracionPorJava`**:

```
@Bean
	RepresentationModelProcessor<RepositorySearchesResource> addSearchLinks(RepositoryRestConfiguration config) {
		Map<Class<?>, Class<?>> controllersRegistrados = new HashMap<>();

		controllersRegistrados.put(Cliente.class, ClienteController.class);

		return new RepresentationModelProcessor<RepositorySearchesResource>() {

			@SuppressWarnings("deprecation")
			@Override
			public RepositorySearchesResource process(RepositorySearchesResource searchResource) {
				if (controllersRegistrados.containsKey(searchResource.getDomainType())) {
					Method[] metodos = controllersRegistrados.get(searchResource.getDomainType()).getDeclaredMethods();
					for (Method m : metodos) {
						if (!m.isAnnotationPresent(ResponseBody.class))
							continue;
						try {
							Object[] pathVars = Stream.of(m.getParameters())
									.filter(p -> p.isAnnotationPresent(PathVariable.class))
									.map(p -> "(" + p.getName() + ")").collect(Collectors.toList()).toArray();
							URI uri = linkTo(m, pathVars).toUri();
							String path = new URI(uri.getScheme(), uri.getUserInfo(), uri.getHost(), uri.getPort(),
									config.getBasePath() + uri.getPath(), uri.getQuery(), uri.getFragment()).toString()
											.replace("(", "{").replace(")", "}");
							String requestParams = Stream.of(m.getParameters())
									.filter(p -> p.isAnnotationPresent(RequestParam.class)).map(Parameter::getName)
									.collect(Collectors.joining(","));
							searchResource.add(new Link(path + "{?" + requestParams + "}", m.getName()));
						} catch (URISyntaxException e) {
							e.printStackTrace();
						}
					}
				}
				return searchResource;
			}
		};
	}
```
> **IMPORTANTE** -> Únicamente se modifica la linea 3:
```
controllersRegistrados.put(Cliente.class, ClienteController.class);
```

**IMPORTANTE** la interfazDAO de la entidad que tiene el metodo personalizado -> tiene que tener un Qeerymetod de JPA con la anotacion **`@RestResources(path= "ruta")`** para que se autodescubra
```
@RestResource(path = "nombre")
List<Elemento> findByNombre(@Param("nombre") String nombre);
```

[Volver a inicio](#springbasics)

### 10.4 Evitar los filtros del CORS del navegador

`Sesion 22: Datos deportivos commit c2d5fd5`

Para que la API pueda ser atacada por un navegador se debe añadir el **`CORS Filter`** como otro Bean (al archivo de configuracion por java)


```
@Bean
	CorsFilter corsFilter() {
		final UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
		final CorsConfiguration config = new CorsConfiguration();
		config.setAllowCredentials(true);
		config.setAllowedOrigins(Collections.singletonList("*"));
		config.setAllowedHeaders(Arrays.asList("Origin", "Content-Type", "Accept"));
		config.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "OPTIONS", "DELETE", "PATCH"));
		source.registerCorsConfiguration("/**", config);

		return new CorsFilter(source);
	}
```

## Events y Listeners en JPA

`Sesion 16: Datos deportivos commit 1b0f09b`

Las entidades pasan por un **ciclo de vida** en función de las operaciones que se ejecutan en la BD. A través de los **`listeners`** vamos a ser capaces de que, de forma automática, en un momento exacto del ciclo de vida de una entidad se ejecute otra opción.
Un listener sera una llamada desde una clase(cuando se instancia) a otra clase para que se ejecute codigo de allí aprovechando las operaciones del ciclo de vida.
Básicamente son eventos anteriores y posteriores a las operaciones **CRUD**.


- PostLoad
- PrePersist
- PostPersist
- PreUpdate
- PostUpdate
- PreRemove
- PostRemove
  > Se pueden poner en una entidad estas anotaciones (**@PostLoad**, ...) sobre alguno de sus metodos para que se inicien a lo largo de su ciclo de Vida
  > Ejemplo **seguridad**: cortando el ciclo de vida entre API y BBDD usando **sauron** y preguntando al usuario si está habilitado para esa operación

**PASOS**

1. **Crear** la **`ClaseListener`** de la entidad necesitada en el paquete`repositorios`. Será un bean `@Component` y se le inyectará la dependencia del bean DAO de la misma entidad (`@Autowired`)

    ```
    @Component
    public class ClienteListener {
	
	private Logger log = LoggerFactory.getLogger(ClienteListener.class);
	private ClienteDAO clienteDAO;
	...
	```

2. **Implementar los métodos** requeridos en la `ClaseListener` creada con las anotaciones deseadas del ciclo de vida vistas anteriormente

    ```
   @Autowired
	public void init(ClienteDAO clienteDAO) {
		this.clienteDAO = clienteDAO;
	}

	@PrePersist
	public void preGuardarCliente(Cliente cliente) {
		log.warn("Se va a guardar un cliente: " + cliente.getNombre());
	}

	@PreRemove
	public void preBorrar(Cliente cliente) {
		System.err.println("Se va a borrar un cliente: " + cliente.getNombre());
	...
	```

3. Decirle a la entidad que tiene listener **`@EntityListener`**

    ```
    @EntityListeners(ClienteListener.class)
    public class Cliente {
	...
	}
	```
[Volver a inicio](#springbasics)

## Servicio Entidad

Por rendimiento puede hacer falta cargar al iniciar la API determinados elementos de la BD en memoria, que se van a usar con frecuencia y que pueden ser usados por otras entidades. Al cargar en memoria se consigue:

- menos accesos a la BD
- Mayor rapidez al leer los datos (Ya estan en memoria).

Generar un servicio Entidad:

1. Crear un `@Bean` con un **metodo** clase java

   - llamado `getServicioEntidadACargar()`
   - que recibe por parametro las entidades de la BD que equeremos almacenar en memoria.
   - que devuelva el propio objeto **Map<Clase, ID>** servicioEntidad (que almacena pares campo-valor ).
   - Al cargarse en el contenedor, recupera de la BD los elementos que nos interesan y los almacena en una variable.

     - (Se puede crear en el archivo de configuracionPorJava)

     ```
     @Bean
     public Map<Clase, ID> getServicioEntidad(IntfDAO intfDAO){
         Map<Clase, ID> servicioEntidad = new HashMap<>();
         intfDAO.findAll().forEach(p -> {
             servicioEntidad.add(Participante.class, p)
         })
         return servicioEntidad;
     }
     ```

1. Las entidades que necesiten recuperar algun dato de los que se almacenen en el Servicio Entidad

   - Tendran una variable de tipo Servicio Entidad

1. Agrego el **parametro ServicioEntidad** al constructor/setter del resto de entidades que van a a necesitar algun elemento de lo almacenado en él, para que cuando se cree una entidad de ese tipo, recuperen los datos de lo almacenado en él sin acceder a la BD.

   - Al insertar por el constructor se asignara el parametro(que sera un bean a la variable local)

> Esto esta sin probar.

[Volver a inicio](#springbasics)
	
## Integrando Librerias

Voy a utilizar código de otro proyecto en el mio. Podré desarrollar por un lado el proyecto-**API** para realizar la persistencia y la capa REST y en otro proyecto-**LIB** (mi libreria) desarrollaré la lógica de mi negocio (java "puro")

El código de la libreria se puede obtener de un repositorio en la nube (gradle lo autocompila) o aprovechar el código de un proyecto en local. 

Ambos proyectos deberían ser Proyectos Gradle: 
- El de la API con Spring con todas las dependencias para REST y pesrsistencia
- El de la LIB sera un proyecto Gradle Spring, sin dependencias (aunque se le quitarán las anotaciones Spring).

### 13.1 Integrar Proyecto Local

Prerrequisitos: 
- Está generado el proyecto API y la libreria con Spring. 
- Están en local (clonados o generados).
- Ambos Proyectos deberian tener su propio **GIT**

1. El `proyecto-LIBreria` esta en la misma carpeta donde está mi `proyecto-API` (en carpetas hermanas).
2. Al proyecto **LIB**reria le quito todas las anotaciones e importaciones de Spring
   - En el main
     -  @SpringApplication
     -  SpringContext = `SpringApplication.run`
   - dependencias y plugins Spring del  build.gradle
   - La carpeta de Tests
3. Importar ambos proyectos Gradle en eclipse.
     - Usar valores por defecto
4. En el `build.gradle` del proyecto **LIB**
   - **NO** puede haber plugins de `Springframework`
   - Eclipse Necesita el plugin
     - `id 'java'`
     - `id 'java-library'`
     - `id 'eclipse'`
5. En el **`settings.gradle`** del proyecto **API**
   - Debe coincidir el nombre del directorio con el del preyecto 
     `rootProject.name = 'nombreProyectoAPI'` (si esta hecho con Spring lo genera automáticamente)
   - Introduzco una linea nueva con: 
      `includeFlat 'proyecto-LIBreria'`
      > Debe ser el mismo nombre que tiene el proyectoLIB en su settings.gradle
6. En el **`build.gradle`** del proyecto **API**
   - Introduzco en el apartado **`dependencias`**
      ```
      dependencies {
        //
        implementation project(':proyecto-LIBreria')
      }
      ```
7. Ejecutar **Refresh gradle project**
   - en propiedades de mi proyecto en java build path → saldrá la librería como una dependencia
8.  Ejecutar en eclipse gradle
        **gradle task ide ⇒ generate all eclipse files**

  > En propiedades de mi proyecto en java build path (o en la carpeta Project and External Dependencies) → saldrá la librería como una dependencia

  > Comprobar llamando desde la API a una clase de la libreria

### 13.2 Integrar Proyecto de GitHub

Se podrá integrar una libreria de un repositorio de codigo abierto como **MAVEN Central** (Compilado), o de un repositorio publico de **GitHub** (Sin compilar) para lo que se necesita compilar con **jit-pack**.

Eclipse Necesita el plugin
`id 'application'`

1. Añado el repositorio y JitPack
    ```
    repositories {
          mavenCentral()
          maven { url 'https://jitpack.io' }
    }
    ```

2. Ir a dependencias y añadir linea 
  `implementation:'Grupo : artefacto : Versión'`
      - grupo: es la ruta al usuario de GitHub
      - artefacto: es el proyecto del usuario
      - versión: es el tag-release
        - en version se puede poner
          - 1 version concreta
          - `sanapshot` (ultima versión)
          - 1 commit concreto
        > puedo poner varias versiones que se crearan en sus carpetas correspondientes

1. ejecutar gradle → refresh project

    - Saldrán en `project and external dependencies` las que haya añadido, pudiendo emplearlas en mi codigo

### 13.3 Integrar Proyecto de Repositorio en la nube

Podre integrar una API o una dependencia.

Api vs Dependencia
   - una api alguien puede utilizar los métodos de la api desde mi librería
   - una dependencia No se puede → solo los puede usar mi librería en local

1.  Ir a dependencias y añadir linea 
    `implementation/api:'Grupo : artefacto : Versión'`

### 13.4 Precedencia y exportacion de dependencias 

Existe precedencia cuando hay conflicto entre clases iguales
* 1° la mas arriba del build path → order export
         
* **export** ⇒ cualquiera que use mi liberia tendrá acceso a las librerias externas que esa integre.

[Volver a inicio](#springbasics)
