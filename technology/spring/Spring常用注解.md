# spring常用注解

### 组件注解

#### @Component(“xxx”)

指定某个类是容器的bean，@Component(value="xx")相当于<bean id="xx" />，其中value可以不写。

用于标注类为spring容器bean的注解有四个，主要用于区别不同的组件类，提高代码的可读性。

- @Component, 用于标注一个普通的bean
- @Controller 用于标注一个控制器类（控制层 controller）
- @Service 用于标注业务逻辑类（业务逻辑层 service）
- @Repository 用于标注DAO数据访问类 （数据访问层 dao）
对于上面四种注解的解析可能是相同的，尽量使用不同的注解提高代码可读性。

注解用于修饰类，当不写value属性值时，默认值为类名首字母小写。

#### @Scope(“prototype”)

该注解和@Component这一类注解联合使用，用于标记该类的作用域，默认singleton。
也可以和@Bean一起使用，此时@Scope修饰一个方法。关于@Bean稍后有说明

#### @Lazy（true）

指定bean是否延时初始化，相当于<bean id="xx" lazy-init=""> ，默认false。@Lazy可以和@Component这一类注解联合使用修饰类，也可以和@Bean一起使用修饰方法

注：此处初始化不是指不执行init-method，而是不创建bean实例和依赖注入。只有当该bean（被@Lazy修饰的类或方法）被其他bean引用（可以是自动注入的方式）或者执行getBean方法获取，才会真正的创建该bean实例，其实这也是BeanFactory的执行方式。

#### @DepondsOn({“aa”，“bb”})

该注解也是配合@Component这类注解使用，用于强制初始化其他bean

```
@DepondsOn("other")
@Lazy(true)
@Controller
@Scope("prototype")
public class UserAction{
	...............
}
```

上面的代码指定，初始化bean “userAction"之前需要先初始化“aa”和“bb”两个bean,但是使用了@Lazy(true)所以spring容器初始化时不会初始化"userAction” bean。

#### @PostConstructor和@PreDestroy

@PostConstructor和@PreDestroy这两个注解是jee规范下的注解。这两个注解用于修饰方法，spring用这两个注解管理容器中spring生命周期行为。

- @PostConstructor 从名字可以看出构造器之后调用,相当于<bean init-method="">。就是在依赖注入之后执行
- @PreDestroy 容器销毁之前bean调用的方法，相当于<bean destroy-method="">

#### @Resource（name=“xx”）

@Resource 可以修饰成员变量也可以修饰set方法。当修饰成员变量时可以不写set方法，此时spring会直接使用jee规范的Field注入。
@Resource有两个比较重要的属性，name和type

- 如果指定了name和type，则从Spring容器中找到唯一匹配的bean进行装配，找不到则抛出异常;
- 如果指定了name，则从spring容器查找名称（id）匹配的bean进行装配，找不到则抛出异常;
- 如果指定了type，则从spring容器中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常;
- 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配

如果没有写name属性值时

- 修饰成员变量，此时name为成员变量名称
- 修饰set方法，此时name 为set方法的去掉set后首字母小写得到的字符串

#### @Autowired（required=false）

@Autowired可以修饰构造器，成员变量，set方法，普通方法。@Autowired默认使用byType方式自动装配。required标记该类型的bean是否时必须的，默认为必须存在（true）。
可以配合@Qualifier(value="xx"),实现按beanName注入。

- required=true(默认），为true时，从spring容器查找和指定类型匹配的bean，匹配不到或匹配多个则抛出异常
- 使用@Qualifier（"xx"）,则会从spring容器匹配类型和 id 一致的bean，匹配不到则抛出异常

@Autowired会根据修饰的成员选取不同的类型

- 修饰成员变量。该类型为成员变量类型
- 修饰方法，构造器。注入类型为参数的数据类型，当然可以有多个参数

### Aop相关注解

#### @Aspect

修饰Java类，指定该类为切面类。当spring容器检测到某个bean被@Aspect修饰时，spring容器不会对该bean做增强处理（bean后处理器增强，代理增强）

```
package com.example.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class UserAdvice{

}
```

#### @Before

修饰方法，before增强处理。。用于对目标方法（切入点表达式表示方法）执行前做增强处理。可以用于权限检查，登陆检查
常用属性

- value：指定切入点表达式 或者引用一个切入点

对com.example.aop 包下所有的类的所有方法做 before增强处理

```
@Before(value = "execution(* com.example.aop.*(..)))")
public void before(JoinPoint joinPoint){
        System.out.println("before  增强处理。。。。。。。。。。。。");
}
```

