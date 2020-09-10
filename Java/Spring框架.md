# Spring框架

## Spring

### Spring简介

​		Spring是个轻量级的IOC和AOP容器框架，松耦合，分层体系结构，允许自由选择组件，可集成其他框架，用于简化企业应用程序的开发，可使开发者只需关心业务需求。

#### 优点

- 低侵入设计，代码的污染极低。

- Spring的DI机制将对象之间的依赖关系交由框架处理，降低组件之间的耦合性。

- Spring提供AOP技术，支持将一些通用任务，如安全、事务、日志、权限等进行集中式管理，从而提供更好的复用。提升系统的可维护性。

- Spring对于主流的应用框架提供了集成支持。

#### 模块

**Spring Core**：Core封装包，核心类库，提供IOC服务。BeanFactory提供对Factory模式的经典实现来消除对程序性单例模式的需要，从程序逻辑中分离出依赖关系和配置。

**Spring Context**：构建于Core封装包基础上的 Context封装包，提供框架式的Bean访问方式，以及企业级功能（JNDI、定时任务等）。Context封装包的特性得自于Beans封装包，并添加了对国际化（I18N）的支持（例如资源绑定），事件传播，资源装载的方式和Context的透明创建，比如说通过Servlet容器。

**Spring AOP**：方法级别的AOP服务，减弱代码的功能耦合。

**Spring DAO**：对JDBC的抽象，简化了数据访问异常的处理。提供了声明式事务管理方法，对所有的POJOs（plain old Java objects）都适用。使得JDBC、Hibernate、JDO等数据访问技术更容易以一种统一的方式工作，用户可容易在持久性技术之间切换，允许在编写代码时，无需考虑捕获每种技术不同的异常。

**Spring ORM**：ORM封装包提供了常用ORM框架的支持。可以混合使用所有Spring提供的特性进行对象关系映射。

**Spring Web**：提供基本的面向Web开发的综合特性，例如多方文件上传。利用Servlet listeners进行IOC容器初始化和针对Web的ApplicationContext。当与WebWork或Struts一起使用Spring时，这个包使Spring可与其他框架结合。

**Spring MVC**：提供面向Web应用的Model-View-Controller实现，提供了一种清晰的分离模型，可以借助Spring框架的其他特性。

### 配置方式

**xml配置**：该文件主要包含类信息，包含众多<‘bean>标签装配，描述了这些类是如何配置以及相互引入的。xml配置文件冗长且更加干净，大项目中管理困难。

>*Q. Spring里面applicationContext.xml文件能不能改成其他文件名？* 

​		ContextLoaderListener是一个ServletContextListener，web应用启动时初始化。缺省情况下，会在WEB-INF/applicationContext.xml文件找Spring的配置。 可以通过定义一个<’context-param>元素，名为contextConfigLocation来改变Spring配置文件的位置。

```html
<listener> 
	<listener-class>org.springframework.web.context.ContextLoaderListener
		<context-param> 
			<param-name>contextConfigLocation</param-name> 
			<param-value>/WEB-INF/xyz.xml</param-value> 
		</context-param>  
	listener-class> 
</listener> 
```

**基于注解的配置**：通过在相关的类，方法或字段声明上使用注解将配置移动到组件类本身。需开启注解装配。

**基于Java的配置**：JavaConfig提供了配置Spring IOC容器的线程安全的纯Java方法，可以避免使用XML配置。面向对象的配置，由于配置被定义为JavaConfig中的类，用户可以充分利用Java中的面向对象功能，一个配置类可以继承另一个，重写它的@Bean方法等。

### 组件注册

#### 组件

**接口**：定义功能

**Bean类**：方法+属性+get/set

**AOP**：

**Bean配置文件**：类的信息+配置

**用户程序**：调用接口

#### 自动装配方式

**no**：默认方式，不进行自动装配，通过手工设置ref属性来进行装配bean。 

**byName**：通过bean的名称进行自动装配，匹配并装配其属性property与xml文件中有相同名称定义的bean。

**byType**：通过参数的数据类型进行自动装配。

**constructor**：调用构造函数进行装配，并且构造函数的参数通过byType进行装配。

**autodetect**：自动探测，如果有构造方法，通过construct的方式自动装配，否则使用byType的方式自动装配。

#### 注解

##### 注解原理

​		注解本质是一个继承了Annotation接口的特殊接口，其具体实现类是Java运行时生成的动态代理类。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法，该方法会从memberValues这个Map中索引出对应的值，而memberValues的来源是Java常量池。

##### 常用注解

注入类：

- **@Autowired**：将类声明为Spring Bean注册到容器中，可以更准确地控制应该在何处以及如何进行自动装配，按byType自动注入，如果该类型查询的结果不止一个，那么@Autowired会根据名称来查找，默认情况下它要求依赖对象必须存在，可以设置required属性为false，否则会抛出异常。
- **@Qualifier**：当创建了多个相同类型的bean并希望仅使用属性装配其中一个bean时，可以使用@Qualifier搭配@Autowired，通过指定bean的id 来消除歧义。

- **@Resource**：将类声明为Spring Bean注册到容器中，默认按byName自动注入，可以指定name属性。当找不到与名称匹配的bean时会按照类型来装配注入。

- **@Component**：将类声明为Spring Bean注册到容器中，Spring管理组件的通用类型。

- **@Controller**：将类声明为Spring Bean注册到容器中，用于控制层，将该类标记为Spring Web MVC控制器，主要用于接受用户请求并调用Service层返回数据给前端。

- **@Service**：将类声明为Spring Bean注册到容器中，用于服务层，主要用于操作持久层、进行逻辑。

- **@Repository**：将类声明为Spring Bean注册到容器中，用于持久层，主要用于数据库相关操作。使未经检查的异常有资格转换为Spring DataAccessException。

- **@Bean**：将方法返回的对象注册到容器中，自定义性更强。

- **@Required**：用于bean属性setter()方法，指示必须在配置时使用bean定义中的显式属性值或使用自动装配填充受影响的bean属性，如果未填充该属性，则容器抛出BeanInitializationException。

- **@Value**：值类型

- **@Conditional注解**：条件注解，只有满足一定的条件才注入组件。这个条件要实现Condition接口，这是一个功能注解接口，然后重写match方法，符合条件就注入。

生命周期类：

- **@PostConstruct**：指定该方法为init-method

- **@PreDestroy**：指定该方法为destroy-method@Configration**：在类上添加该注解，通过调用@Bean方法定义Bean之间的依赖关系。

单元测试类：

- **@Runwith(SpringJUnit4ClassRunner.class)**
- **@ContextConfiguration()**：加载配置，"classpath:配置文件"，或classes={配置类.class}。

配置类：

- **@ComponentScan**：组件扫描注解，通过扫描某些包下的组件批量的注册。关键属性有：

    1. basePackages/value：表示扫描的路径，值的类型为String[]。
    2. includeFilters：包含过滤器Filter[]，指定扫描符合条件的组件。其中Filter[]中的Filter是ComponentScan内部的注解。
    3. excludeFilters：排除过滤器Filter[]，不扫描不符合条件的组件。其中Filter[]中的Filter是ComponentScan内部的注解。
    4. useDefaultFilters：默认的扫描策略默认为true，如果想要用自定义的策略 该值要设为false。
-  **@EnableTransactionManagement**：驱动的配置类

#### Bean

##### 生命周期

1. 实例化Bean：Spring容器根据配置情况调用Bean的构造方法或工厂方法实例化Bean对象。对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。对于ApplicationContext容器，当容器启动结束后，通过获取BeanDefinition对象中的信息，实例化所有的bean。

2. 注入属性：实例化后的对象被封装在BeanWrapper对象中，紧接着Spring根据BeanDefinition中的信息，以及通过BeanWrapper提供的设置属性的接口完成Bean中所有属性值的依赖注入。

3. 如果Bean实现了BeanNameAware接口，工厂通过传递Bean的ID来调用Bean的setBeanName(String beanId)方法。

4. 如果Bean实现了BeanFactoryAware接口，工厂通过传递自身的实例的引用来调用 setBeanFactory()方法。

5. 如果Bean实现了ApplicationContextAware接口，则调用setApplicationContext(ApplicationContext)方法传入当前Spring上下文。

6. 如果存在与bean关联的BeanPostProcessors，即Bean的后处理，则调用该接口的与初始化方法postProcessBeforeInitialization() 进行加工操作。BeanPostProcessor接口提供钩子函数来动态扩展修改Bean，如果想对Bean进行一些自定义的处理，那么可以让Bean实现了BeanPostProcessor接口。

7. 如果Bean实现了InitializingBean接口，则会调用该接口的afterPropertiesSet()方法。

8. 如果为Bean指定了init方法，即配置文件中<bean>标签的init-method属性，则调用它。

9. 如果存在与bean关联的BeanPostProcessors，则将调用该接口的后置初始化方法postProcessAfterInitialization() 。

10. 程序使用该Bean。若Bean的scope为singleton，将Bean放入IOC缓存池，该Bean的生命周期交由Spring管理。若Bean的scope为prototype，每次被调用都会new一个新的对象，生命周期交由调用方管理。

11. 如果Bean实现了DisposableBean接口，则容器关闭时，会调用该接口的destroy()方法销毁Bean。

12. 如果配置文件中指定了destroy-method方法，则调用该方法销毁Bean。

##### 作用域 Scope

​		仅当web 应用时，request、session、globalSession作用域才有效。

**singleton**：单例模式，在整个Spring IOC容器中，只有一个bean实例，默认为单例。

**prototype**：原型模式，每次从容器中请求Bean，都会产生一个新的Bean实例。若没有去获取scope为prototype的实例，在容器初始化时不会初始化这个组件。

**request**：每次HTTP请求，都会创建一个新的Bean。

**session**：每次HTTP请求，都会创建一个新的Bean，同一个HTTP session共享一个Bean，不同的session使用不同的Bean。

**globalSession**：同一个全局Session共享一个Bean。在基于portlet的web应用中，Portlet规范定义了全局Session的概念，它被所有构成某个portlet web应用的各种不同的portlet所共享。在global session作用域中定义的bean被限定于全局portlet Session的生命周期范围内，web会自动把该bean当成session类型来使用。

##### 实例化

**构造方法实例化**：默认无参数，通过constructor-arg指定构造方法参数完成注入。

**静态工厂实例化**：指定factory-method为对应的实例化静态方法。

**实例工厂实例化**：通过factory-bean指定用来实例化该bean的对应对象，通过factory-method指定factory-bean中用来实例化该bean的方法。factory-bean必须也是属于ApplicationContext中的一个bean。

##### 循环依赖

###### 出现场景

- 构造器的循环依赖问题无法解决，只能抛出异常。
- 作用域为prototype的Bean的循环依赖问题，无法解决

- setter注入构成的循环依赖，Spring的设计机制可以解决。

###### **解决方案**

- 不使用基于构造函数的依赖注入
- 字段上使用@Autowired注解，让 Spring 决定在合适的时机注入。
- 用基于setter方法的依赖注入

###### 解决机制

​		Spring实例化一个bean分两步进行，首先调用构造函数实例化目标bean，然后为其注入属性。实例化一个bean时，首先递归的实例化其所依赖的所有bean，直到某个bean没有依赖其他bean，此时就会将该实例返回，然后反递归的将获取到的bean设置为各个上层bean的属性。

> 例如：
>
> 1. beanA进行初始化，并且将自己进行初始化的状态记录下来，并提前向外暴露一个单例工厂方法，从而使其他bean能引用到该bean。
>
> 2. beanA中有beanB的依赖，于是开始初始化beanB。
>
> 3. 初始化beanB的过程中又发现beanB依赖了beanA，于是又进行beanA的初始化，这时发现beanA已经在进行初始化了，程序发现了存在的循环依赖，然后通过步骤一中暴露的单例工厂方法拿到beanA的引用从而beanB拿到beanA的引用，完成注入，完成了初始化，此时的beanA只是完成了构造函数的注入但未完成其他步骤，如此beanB的引用也就可以被beanA拿到，从而beanA也就完成了初始化。
>
> ![image-20200413211924458](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200413211924458.png)

### 源码

### IOC/DI

#### IOC简介

​		Inverse of Control，控制反转，将创建对象的控制权转移给Spring容器，并由容器使用反射机制，根据配置的元数据在运行时动态的创建实例和管理各个实例之间的依赖关系，实现对象与对象之间的松散耦合，利于功能的复用。

​		IOC容器是Spring用来实现IOC的载体，实质是一个Map，存放各种对象。IOC 容器就像是一个工厂，当需要创建一个对象，只需要配置好配置文件/注解即可，不用考虑对象是如何被创建出来的，大大增加了项目的可维护性且降低了开发难度。

#### DI简介

​		Dependency Injection，依赖注入，和控制反转是同一个概念的不同角度的描述，即Spring框架负责创建Bean对象时，依赖IOC容器来动态注入对象所需要的外部资源。

#### IOC实现原理

​		工厂模式+反射机制

![image-20200413213707459](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200413213707459.png)

#### IOC注入方式

- 构造器注入

- setter()方法注入

- 接口注入（注解注入）


#### IOC容器种类

**BeanFactory**：

​		Spring里最底层的接口，包含了各种Bean的定义，读取Bean的配置文件，管理Bean的加载、实例化，控制Bean的生命周期，维护Bean之间的依赖关系。

​		采用延迟加载形式来注入Bean，第一次调用getBean()时，才对该Bean进行加载实例化。不能发现一些存在的Spring的配置问题，如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean()方法才会抛出异常。

​		使用语法显示提供资源对象，不支持基于依赖的注解。

**ApplicationContext**：

​		继承BeanFactory接口，提供了更完整的框架功能：继承MessageSource，支持国际化。 统一的资源文件访问方式。 提供在监听器中注册Bean的事件。同时加载多个配置文件。载入多个上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

​		在启动时创建所有的Bean。在容器启动时，我们就可以发现Spring中存在的配置错误，有利于检查所依赖属性是否注入。相对于BeanFactory，ApplicationContext唯一的不足是占用内存空间，当应用程序配置Bean较多时，程序启动较慢。

​		自己创建并管理资源对象，支持基于依赖的注解。

##### 事件类型

​		ApplicationContext提供了支持事件和代码中监听器的功能。通过创建bean，实现ApplicationListener接口，可监听在ApplicationContext中发布的ApplicationEvent类事件。

**上下文更新事件 ContextRefreshedEvent**：调用ConfigurableApplicationContext 接口中的refresh()方法时被触发。

**上下文开始事件 ContextStartedEvent**：当容器调用ConfigurableApplicationContext的Start()方法开始/重新开始容器时触发该事件。

**上下文停止事件 ContextStoppedEvent**：当容器调用ConfigurableApplicationContext的Stop()方法停止容器时触发该事件。

**上下文关闭事件 ContextClosedEvent**：当ApplicationContext被关闭时触发该事件。容器被关闭时，其管理的所有单例Bean都被销毁。

**请求处理事件 RequestHandledEvent**：在Web应用中，当一个http的request请求结束时触发该事件。

### AOP

#### AOP简介

​		OOP，面向对象编程，允许开发者定义纵向的关系，不适用于定义横向的关系，导致了大量代码的重复，不利于各个模块的复用。

​		AOP，Aspect-Oriented Programming，面向切面编程，作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装成一个可重用的模块，这个模块被命名为切面，以减少重复的代码，降低模块间的耦合，利于后期的扩展和维护。可用于事务处理、日志管理、权限控制等。

#### 实现

​		AOP实现的关键在于代理模式，AOP代理主要分为静态代理和动态代理。

##### 静态代理

​		代表为AspectJ。AOP框架在编译阶段生成AOP代理类，称为编译时增强。在编译阶段将AspectJ（切面）织入到Java字节码中，运行的时候就是增强之后的AOP对象。

##### 动态代理

​		代表为Spring AOP。Spring AOP框架为负责实施切面的框架，将切面所定义的横切逻辑编织到切面所指定的连接点中。基于动态代理，不修改字节码，每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并在特定的切点做增强处理，并回调原对象的方法。

​		若要代理的对象实现了某个接口，默认情况下会使用JDK动态代理去创建代理对象；对于没有实现接口的对象，则使用CGlib 动态代理生成一个被代理对象的子类来作为代理。Spring会自动在JDK动态代理和CGLIB之间转换。可以强制使用CGLIB实现AOP，需添加CGLIB依赖jar包，在Spring配置文件中加入`<aop:aspectj-autoproxy proxy-target-class="true"/>`。

###### JDK动态代理

​		只提供接口的处理，不支持类的处理。核心为InvocationHandler接口和Proxy类。InvocationHandler通过invoke()方法反射来调用目标类中的代码，其中动态的将横切逻辑和业务编织在一起。接着Proxy利用InvocationHandler动态创建一个符合某一接口的实例，生成目标类的代理对象。

`invoke(Object proxy,Method method,Object[] args)`方法参数解释：

- proxy：最终生成的代理实例
- method：被代理目标实例的某个具体方法
- args：被代理目标实例某个方法的具体入参, 在方法反射调用时使用。

###### CGLib动态代理

​		若代理类没有实现InvocationHandler接口，Spring AOP则会选择使用Cglib来动态代理目标类。CGLib（Code Generation Library）是一个代码生成的类库，底层采用ASM字节码生成框架，CGLib使用字节码技术生成代理类，在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。通过继承的方式做动态代理，用final修饰的类或方法无法使用Cglib做动态代理。

**JDK和CGLib的区别和选择**

​		在jdk6之前CGLib动态代理比使用Java反射效率要高。在jdk6、jdk7、jdk8逐步对JDK动态代理优化之后，在调用次数较少的情况下，JDK代理效率高于CGLIB代理效率，只有当进行大量调用的时候，jdk6和jdk7比CGLIB代理效率低一点，但是到jdk8的时候，jdk代理效率高于CGLIB代理。

##### Spring AOP和AspectJ AOP的区别和选择

​		AOP的实现方式有静态代理和动态代理，区别在于生成AOP代理对象的时机不同。

​		AspectJ是静态代理，编译时增强，基于字节码操作，额外支持属性级别的PointCut，功能更强大，性能更好，但需要特定的编译器进行处理。

​		Spring AOP是动态代理，运行时增强，基于代理，只支持方法级别的PointCut，使用简单，无需特定的编译器处理。

#### 相关概念

**通知 Advice**：功能，所采取的操作，如安全、事物、日志等。

**连接点 JoinPoint**：Spring允许使用通知的地方，Spring 只支持方法连接点。

**切入点 Pointcut**：在连接点中选取几个点作为切入点，真正的放入通知。切入点使得通知可独立于OO层次。 例如一个提供声明式事务管理的around通知可以被应用到一组横跨多个对象中的方法上。

**切面 Aspect**：切面是通知和切入点的结合。包含连接点的定义+横切逻辑的定义。通知说明了干什么和什么时候干，什么时候通过方法名中的before、after、around等；切入点说明了在哪干，指定到底是哪个方法。

**引入 introduction**：也被称为内部类型声明（inter-type declaration）。允许引入新的接口（以及一个对应的实现）到任何被代理的对象，允许向现有的类添加新方法属性，把切面引用到新方法。例如可以使用一个引入来使bean实现IsModified接口，以简化缓存机制。

**目标 target**：要被通知的对象，即真正的业务逻辑。

**代理 proxy**：整套AOP机制的实现。

**织入 weaving**：把切面应用到目标对象来创建新的代理对象的过程，Spring AOP的weaving在运行时执行。

#### 通知类型

**Before 前置通知**：joinpoint方法前执行，抛出异常会影响连接点的执行。

**After Returning 返回后通知**：joinpoint正常执行后执行，若连接点抛出异常则不执行。

**After Throwing 抛出异常后通知**：joinpoint抛出异常退出后执行

**After(finally) 后通知**：在连接点后执行，无论连接点正常执行完毕或抛出异常退出均执行。

**Around 环绕通知**：包围在joinpoint前后，环绕通知可以在方法调用前后完成自定义的行为，可选择是否继续执行连接点，即调用ProceedingJoinPoint的proceed方法，或者直接返回他们自己的返回值，或者抛出异常来结束执行。

#### 底层原理

### 事务

​		Spring本身并不直接管理事务，而是提供了事务管理器接口，对于不同的框架或者数据源则用不同的事务管理器。本质是数据库对事务的支持，即提交和回滚通过binlog和redo log实现。

对于事务，它把相关的属性都封装到一个实体里，属性包括：

```java
int propagationBehavior;	//事务的传播行为
int isolationLevel;		//事务隔离级别
int timeout;	//事务完成的最短时间
boolean readOnly;	//是否只读
```

#### 事务管理方式

​		Spring提供了对编程式事务和声明式事务的支持。

##### 编程式事务

​		嵌在业务代码中，灵活，维护困难。

一般事务的定义步骤：

```java
TransactionDefinition td = new TransactionDefinition();		//事务属性定义
try{ 
	//do sth
	transactionManager.commit(ts);
}catch(Exception e){
	transactionManager.rollback(ts);
}
```

Spring事务主要使用transactionTemplate，省略了部分的提交，回滚，一系列的事务对象定义，需注入事务管理对象。

```java
void add(){
	transactionTemplate.execute( new TransactionCallback(){ 
		public Object doInTransaction(TransactionStatus ts){
			//do sth
		}
	}
}
```

##### 声明式事务

​		使用注解或基于xml文件配置来管理事务，事务管理与业务代码分离，只需在配置文件中做相关的事务规则声明或通过@Transactional注解的方式，便可以将事务规则应用到业务逻辑中。声明式事务管理建立在AOP上，本质是通过AOP对方法前后进行拦截，将事务处理的功能编织到方法中，即目标方法开始前加入一个事务，执行完毕后根据执行情况提交或回滚事务。声明式事务的缺点是最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。

​		@Transactional 注解可以被用在接口定义和接口方法、类定义和类的 public 方法上。对于其它非public的方法，如果标记了@Transactional也不会报错，但方法没有事务功能。

​		用Spring事务管理器时，由spring来负责数据库的打开、提交、回滚。默认遇到运行期异常，即不受检查的异常（throw new RuntimeException("注释");）时会回滚。而遇到需要捕获的异常，即非运行时抛出的受检查的异常（throw new Exception("注释");）时不会回滚，需我们指定方式来让事务回滚。要想所有异常都回滚，要加上 `@Transactional(rollbackFor={Exception.class,其它异常})` 。如果让unchecked例外不回滚，需加上`@Transactional(notRollbackFor=RunTimeException.class)`。

​		仅仅 @Transactional 注解的出现不足于开启事务行为，它仅仅是一种元数据，能够被可以识别 @Transactional 注解和上述的配置适当的具有事务行为的beans所使用。

​		Spring团队的建议是你在具体的类（或类的方法）上使用 @Transactional 注解，而不要使用在类所要实现的任何接口上。可以在接口上使用 @Transactional 注解，但这将只能当你设置了基于接口的代理时它才生效。因为注解是不能继承的，这就意味着如果你正在使用基于类的代理时，那么事务的设置将不能被基于类的代理所识别，而且对象也将不会被事务代理所包装（将被确认为严重的）。

> 《阿里巴巴开发手册》规定：
>
> - 【参考】@Transactional事务不要滥用。事务会影响数据库的QPS，另外使用事务的地方需要考虑各方面的回滚方案，包括缓存回滚、搜索引擎回滚、消息补偿、统计修正等。
>

###### 参数

- readOnly

    该属性用于设置当前事务是否为只读事务，设置为true表示只读，false则表示可读写，默认值为false。

    > 例如：`@Transactional(readOnly=true)`

- rollbackFor

    该属性用于设置需要进行回滚的异常类数组，当方法中抛出指定异常数组中的异常时，则进行事务回滚。

    > 例如：指定单一异常类：`@Transactional(rollbackFor=RuntimeException.class)`。指定多个异常类：`@Transactional(rollbackFor={RuntimeException.class, Exception.class})`。

- rollbackForClassName

    该属性用于设置需要进行回滚的异常类名称数组，当方法中抛出指定异常名称数组中的异常时，则进行事务回滚。

    > 例如：指定单一异常类名称：`@Transactional(rollbackForClassName="RuntimeException")`。指定多个异常类名称：`@Transactional(rollbackForClassName={"RuntimeException","Exception"})`。

- noRollbackFor

    该属性用于设置不需要进行回滚的异常类数组，当方法中抛出指定异常数组中的异常时，不进行事务回滚。

    > 例如：指定单一异常类：`@Transactional(noRollbackFor=RuntimeException.class)`。指定多个异常类：`@Transactional(noRollbackFor={RuntimeException.class, Exception.class})`。

- noRollbackForClassName

    该属性用于设置不需要进行回滚的异常类名称数组，当方法中抛出指定异常名称数组中的异常时，不进行事务回滚。

    > 例如：指定单一异常类名称：`@Transactional(noRollbackForClassName="RuntimeException")`。指定多个异常类名称：`@Transactional(noRollbackForClassName={"RuntimeException","Exception"})`。

- propagation

    该属性用于设置事务的传播行为。

    ```java
    //如果有事务, 那么加入事务, 没有的话新建一个(默认情况下)
    @Transactional(propagation=Propagation.REQUIRED) 
    　
    //容器不为这个方法开启事务
    @Transactional(propagation=Propagation.NOT_SUPPORTED) 
    　　
    //不管是否存在事务,都创建一个新的事务,原来的挂起,新的执行完毕,继续执行老的事务
    @Transactional(propagation=Propagation.REQUIRES_NEW) 
    　　
    //必须在一个已有的事务中执行,否则抛出异常
    @Transactional(propagation=Propagation.MANDATORY) 
    
    //必须在一个没有的事务中执行,否则抛出异常(与Propagation.MANDATORY相反)
    @Transactional(propagation=Propagation.NEVER) 
    
    //如果其他bean调用这个方法,在其他bean中声明事务,那就用事务.如果其他bean没有声明事务,那就不用事务。
    @Transactional(propagation=Propagation.SUPPORTS) 
    ```

    > 例如：@Transactional(propagation=Propagation.NOT_SUPPORTED,readOnly=true)

- isolation

    该属性用于设置底层数据库的事务隔离级别，事务隔离级别用于处理多事务并发的情况，通常使用数据库的默认隔离级别即可，基本不需要进行设置。MYSQL 默认为 REPEATABLE_READ 级别，SQLSERVER 默认为 READ_COMMITTED 级别。

    ```java
    //读取未提交数据(会出现脏读, 不可重复读) 基本不使用
    @Transactional(isolation = Isolation.READ_UNCOMMITTED)
    
    //读取已提交数据(会出现不可重复读和幻读)
    @Transactional(isolation = Isolation.READ_COMMITTED)
    　　
    //可重复读(会出现幻读)
    @Transactional(isolation = Isolation.REPEATABLE_READ)
    
    //串行化
    @Transactional(isolation = Isolation.SERIALIZABLE)
    ```

- timeout

    该属性用于设置事务的超时秒数，默认值为-1，表示永不超时。

    > 例如：`@Transactional(timeout=30)`    //30秒

#### 事务隔离级别

**ISOLATION_DEFAULT**：默认隔离级别，使用数据库默认的事务隔离级别。

**ISOLATION_READ_UNCOMMITTED**：读未提交，允许另外一个事务可以读到这个事务未提交的数据。可能导致脏读、幻读、不可重复读。

**ISOLATION_READ_COMMITTED**：读已提交，保证一个事务修改的数据提交后才能被另一事务读取，且能看到其他事务已经提交的对已有记录的更新。可能导致幻读、不可重复读。

**ISOLATION_REPEATABLE_READ**：可重复读，保证一个事务修改的数据提交后才能被另一事务读取，但是不能看到该事务对已有记录的更新，即对同一查询读取结果一致。可能导致幻读。可重复读是指对于其他事务操作的数据，多次读取的结果都是一样的，如果第一次读取能够读取到其他事务的数据，后面不管其他事务有任何其他任何操作，都不会影响已读取到的其他事务产生的数据。但是自己所在的事务中产生的数据，自己是都可以读取到的。

> 例如：可重复读模式下幻读现象：
>
> 事务A操作：
>
> 1. 打开事务
> 2. 查询号码为X的记录，不存在
> 3. 插入号码为X的数据，插入报错
> 4. 查询号码为X的记录，发现还是不存在（由于是可重复读，所以读取记录X还是不存在的）
>
> 事物B操作：在事务A第②步操作时插入了一条X的记录，违反了唯一约束，导致A中第3步插入报错。
>
> 上面操作对A来说就像发生了幻觉一样，明明查询X（A中第二步、第四步）不存在，但却无法插入成功。幻读可以这么理解：事务中后面的操作（插入号码X）需要上面的读取操作（查询号码X的记录）提供支持，但读取操作却不能支持下面的操作时产生的错误，就像发生了幻觉一样。

**ISOLATION_SERIALIZABLE**：串行化，事务依次执行，不并发，影响性能。一个事务在执行的过程中完全看不到其他事务对数据库所做的更新。

#### 事务传播行为

​		当一个事务方法被另一个事务方法调用时，即多个事务同时存在时，Spring如何处理这些事务的行为。

**PROPAGATION_REQUIRED**：若当前存在事务则加入事务；若当前没有事务则新建一个事务。

**PROPAGATION_SUPPORTS**：若当前存在事务则加入事务；若当前没有事务则以非事务方式执行。

**PROPAGATION_MANDATORY**：若当前存在事务则加入事务；若当前没有事务则抛出异常。

**PROPAGATION_REQUIRES_NEW**：新建事务，若当前存在事务则把当前事务挂起。

**PROPAGATION_NOT_SUPPORTED**：以非事务方式执行操作，若当前存在事务则把当前事务挂起。

**PROPAGATION_NEVER**：以非事务方式执行，若当前存在事务则抛出异常。

**PROPAGATION_NESTED**：若当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；若当前没有事务，则新建一个事务，类似PROPAGATION_REQUIRED。

### 线程安全

单例Bean的线程安全问题：

​		Spring框架并没有对单例bean进行任何多线程的封装处理。关于单例bean的线程安全和并发问题需要开发者自行去搞定。但实际上，大部分的Spring bean并没有可变的状态，如Serview类和DAO类，所以在某种程度上说Spring的单例bean是线程安全的。如果bean有多种状态，如View和Model对象，就需要自行保证线程安全。最直接的解决办法就是将多态bean的作用域由singleton变更为prototype。

Spring处理线程并发问题：

​		通常只有无状态的Bean才可以在多线程环境下共享。Spring使用ThreadLocal解决线程安全问题。Spring中绝大部分Bean都可以声明为singleton作用域，Spring对RequestContextHolder、TransactionSynchronizationManager、LocaleContextHolder等Bean中非线程安全状态采用ThreadLocal进行处理，让它们也成为线程安全的状态，使得有状态的Bean可以在多线程中共享。

## Spring MVC

### Spring MVC简介

​		Model1时代结构为jsp+javaBean，jsp既是控制层又是表现层，前后端依赖严重，控制逻辑和表现逻辑混杂，代码重用率第，开发效率低。Model2时代结构为JavaBean+JSP+Servlet，是早期的JavaWeb MVC开发模式，但抽象和封装程度还远远不够，程度可维护性和复用性低。Spring MVC天然与Spring集成，可以帮助我们进行更简洁的Web层的开发。可以支持各种视图技术，不仅仅局限于JSP，支持各种请求资源的映射策略。Spring MVC一般把后端项目分为Controller层（控制层，返回数据给前台页面）、Service层（处理业务）、Dao层（数据库操作）、Entity层（实体类）。

### 组件

**前端控制器 DispatcherServlet**：相当于转发器，接收请求、响应结果，减少了其它组件之间的耦合度。不需要程序员开发。

**处理器映射器 HandlerMapping**：根据请求的URL来查找Handler。不需要程序员开发。

**处理器适配器 HandlerAdapter**：

**处理器 Handler**：需要程序员开发。

**视图解析器 ViewResolver**：进行视图的解析，根据视图逻辑名解析成真正的视图。不需要程序员开发。

**视图 View**：接口，实现类支持不同类型的视图，如jsp、freemarker、pdf等。需要程序员开发。

### 工作原理

#### 架构图

![image-20200414173656636](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200414173656636.png)

#### 执行流程

1. 客户端向服务器发送http请求，至前端控制器DispatcherServlet，前端控制器本质是一个servlet，是 SpringMVC提供的所有请求的的统一入口。

2. DispatcherServlet根据请求信息调用HandlerMapping处理器映射器，解析请求对应的Handler。

3. 处理器映射器解析到对应的处理器Handler后，生成处理器对象及处理器拦截器（如果有的话），以HandlerExecutionChain对象的形式一并返回给DispatcherServlet。

4. DispatcherServlet根据该Handler调用合适的HandlerAdapter处理器适配器，执行拦截器的preHandler()方法。
5. 提取Request中的模型数据，填充Handler的入参，HandlerAdapter经过适配调用具体的处理器，即后端控制器Controller。
6. Controller执行相应的业务逻辑，完成后返回ModelAndView。
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。
8. DispatcherServlet将ModelAndView传给合适的已注册的ViewReslover视图解析器。
9.  ViewResolver根据逻辑View查找并返回实际的View给DispatcherServlet。
10. DispatcherServlet根据Model模型数据填充到View进行渲染视图。
11. DispatcherServlet响应用户。

#### 执行顺序

**Interceptor拦截器和Filter的区别**：Spring的拦截器与Servlet的Filter均为AOP编程思想的体现，都能实现权限检查、日志记录等。不同的是：

1. 使用范围不同：Filter是Servlet规范规定的，只能用于Web程序中。而拦截器既可以用于Web程序，也可以用于Application、Swing程序中。
2. 规范不同：Filter是在Servlet规范中定义的，是Servlet容器支持的。而拦截器是在Spring容器内的，是Spring框架支持的。
3. 使用的资源不同：同其他的代码块一样，拦截器也是一个Spring的组件，归Spring管理，配置在Spring文件中，因此能使用Spring里的任何资源、对象，例如Service对象、数据源、事务管理等，通过IOC注入到拦截器即可；而Filter则不能。
4. 深度不同：Filter在只在Servlet前后起作用。而拦截器能够深入到方法前后、异常抛出前后等，因此拦截器的使用具有更大的弹性。

**Filter、interceptor、AOP的执行顺序**：

​		程序执行的顺序是先进过滤器，再进拦截器，最后进切面。注意：如果拦截器中preHandle方法返回的为false时，则无法进入切面。

### 使用

#### 启动类配置

​		Spring容器是SpringMVC容器的父容器，子容器可以使用父容器的组件，但父容器不得使用子容器。把跟web不太相关的组件放在SpringMVC容器中，Spring容器会在dispatherServlet请求分发前进行组件的初始化，SpringMVC容器会在第一次调用后进行组件的初始化。

#### 注解

**@RequestMapping**：处理请求URL映射的注解，可用于类或方法上。添加method属性，值为RequestMethod.GET/POST可拦截指定方式提交的方法。

**@RequestBody**：接收HTTP请求的Json数据，将Json转换为java对象。

**@ResponseBody**：将conreoller方法返回对象转化为Json对象响应给客户端。

**@RestController**：相当于@ResponseBody+@Controller。方法无法返回到jsp页面，视图解析器不起作用。

**@PathVariable**：

**@RequestHeader**：获取请求头中的参数。分隔符为逗号，可用String或String[]接收。

**@RequestParam**：

#### 参数传递

##### 返回值

**String**

**ModelAndView**：把视图和数据结合在一起

##### 从后台向前台传递数据的对象

**Model对象**：addAttribute()方法传递数据，前台用el表达式获取。

**内置对象**：HttpServletRequest、HttpSession对象的setAttribute()方法传递参数，前台用el表达式获取。可以在类上添加@SessionAttributes注解，value属性设置要放入session里面的参数名，types属性设置要放入session里面的参数类型，前提是需在处理器中将参数设置到了model中。

### 线程安全

​		SpringMVC中的Controller、Service、Dao都是默认单例，好处在与提高性能，不用每次创建Controller实例，减少了对象创建和垃圾收集的时间。单例模式若使用不当存在一定的安全隐患，当被多个线程同时调用时，其成员变量线程不安全。但因为Controller、Service、Dao等基本不定义成员变量，只关注于方法本身，属于无状态Bean，所以线程安全。如果要定义属性的话，scope最好为prototype，设置为多例模式；或者使用ThreadLocal变量。对于Spring提供的有状态的Bean，一般提供了通过ThreadLocal去解决线程安全的方法，比如RequestContextHolder，它通过为每个线程提供一个独立的变量副本解决了变量并发访问的冲突问题。

## Spring Boot

### Spring Boot简介

​		Spring Boot是Spring开源组织下的子项目，是Spring组件一站式解决方案，简化了Spring的使用，简省了配置，提供各种启动器。

#### 优点

​		简化配置，自动配置，上手容易。内置容器，可独立运行，在外部容器中部署时，可以选择排除依赖关系以避免潜在的jar冲突，部署时灵活指定配置文件的选项。

### 配置

#### 配置文件

​		application配置文件，主要用于Spring Boot项目的自动化配置。格式有.properties 和 .yml，区别是书写格式不同，.yml格式不支持@PropertySource注解导入配置。

#### 读取配置

​		Spring Boot可以通过@PropertySource、@Value、@Environment、@ConfigurationProperties来绑定变量。读取指定文件资源目录下的配置文件config/db-config.properties：@PropertySource+@ConfigurationProperties或@PropertySource+@Value。

#### 配置加载顺序

1. properties文件

2. YAML文件

3. 系统环境变量

4. 命令行参数
5. ...

#### 定义多套不同环境的配置

1. 提供多套配置文件。如applcation.properties、application-dev.properties、application-test.properties。

2. 运行时指定具体的配置文件。在application.properties中添加spring.profiles.active=配置文件名。

### 使用

#### 开启Spring Boot特性方式

1. 继承spring-boot-starter-parent项目
2. 导入spring-boot-dependencies项目依赖

#### 运行

- 命令打包或者放到容器中运行，Spring Boot内置了Tomcat/Jetty容器，可独立运行。

- 直接执行main方法运行

- 用Maven/Gradle插件运行

#### 热部署

​		Spring Boot提供了一个名为spring-boot-devtools的模块来使应用支持热部署，提高开发者的开发效率，无需手动重启Spring Boot应用。添加依赖devtools，在配置文件中自定义：

- 热部署开关：spring.devtools.restart.enabled: true
- 指定热部署的目录：spring.devtools.restart.additional-paths: src/main/java
- 指定目录不更新：spring.devtools.restart.exclude: test/**

#### 注解

**@SpringBootApplication**：包含了三个注解：

- **@SpringBootConfiguration**：组合了@Configuration注解，定义配置类，替换xml配置文件，实现配置文件的功能。添加该注解的类内部包含有一个或多个标有@Bean注解的方法， 这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，将实例注入到Spring容器中，初始化Spring容器。
- **@ComponentScan**：Spring组件扫描，用于将一些标注了特定注解的bean定义批量采集注册到Spring的IOC容器之中，这些特定的注解大致包括：@Controller、@Entity、@Component、@Service、@Repository。
- **@EnableAutoConfiguration**：打开自动配置的功能，也可以用exclude属性关闭某个自动配置的选项。将应用所有符合条件的@Configuration配置都加载到当前IOC容器中。

**@RequestMapping**：

**@PathVariable**：用于将请求URL中的模板变量映射到功能处理方法的参数上

**@ResponseBody**： 

**@RequestHeader**： 

**@RequestParam**：

**@ImportResource**：导入老Spring项目配置文件，以兼容老Spring项目。

**@ConfigurationProperties**：类上添加该注解，prefix属性为配置文件中key的前缀，将prefix和类中成员变量名拼接，即为配置文件中的key，则不需用@Value("${配置文件key}")对成员变量进行赋值。

**@PropertiesSource**：引入properties的springboot配置文件

**@ImportResource**：引入xml的spring配置文件

**@SpringBootTest**：单元测试

**@WebServlet**：Servlet类上添加该注解，设置urlPatterns属性。

**@ServletComponentScan**：启动类上添加该注解，设置basePackages属性。

**@Listener**：

**@Filter**:

**@Mapper**：接口上添加该注解，该接口在编译时会生成相应的实现类，该接口中不可以定义同名的方法，因为会生成相同的id，即不支持重载，虽然java语法允许。

**@MapperScan**：在启动类上添加该注解，设置basePackages属性为mapper所在路径。

**@EnableTransactionManagement**：启动类上添加该注解，开启事务支持。

**@Transactional**： Service层相应方法添加该注解，使用事务。

### 原理

#### 自动配置原理

​		扫描所有具有META-INF/spring.factories的jar包，如spring-boot-autoconfigure-x.x.x.x.jar。这个spring.factories文件是一组一组的key=value形式，其中一个key是EnableAutoConfiguration类的全类名，value是一个xxxxAutoConfiguration的类名的列表，这些类名以逗号分隔。这个@EnableAutoConfiguration注解通过@SpringBootApplication被间接的标记在了Spring Boot的启动类上，在SpringApplication.run(...)的内部就会执行selectImports()方法，找到所有JavaConfig自动配置类的全限定名对应的class，然后将所有自动配置类加载到Spring 容器中。Spring Boot 启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以 AutoConfiguration结尾来命名的，实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性，如server.port ，而XxxxProperties类通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定。

#### 启动器 Starters

​		Starters包含一系列可以集成到应用里面的依赖包，可以一站式集成Spring及其他技术，而不需要到处找示例代码和依赖包。可以实现ApplicationRunner接口或者CommandLineRunner接口，重写run()方法，可在Spring Boot启动时运行一些特定的代码。

### 事务

​		整个事务建立在AOP的基础之上，其核心类是TransactionInterceptor，该类的invokeWithinTransaction方法是事务处理的核心方法，其中封装了我们创建的DataSourceTransactionManager对象，该对象是执行回滚或者提交的执行单位。TransactionInterceptor和标注@Aspect注解的类的作用相同，拦截指定方法，而在TransactionInterceptor中是通过是否标有事务注解来决定的，如果一个类中任意方法含有事务注解，那么这个方法就会被代理。而Mybatis的事务和Spring的事务协作则根据他们的SqlSession是否是同一个SqlSession来决定的，如果是同一个，则交给Spring，如果不是，Mybatis则自己处理。@Transactional