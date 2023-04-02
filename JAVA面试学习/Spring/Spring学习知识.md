#### 1、Spring的事务管理

1. 编程式事务管理：需要手动编写代码来控制事务的启动、提交和回滚。在使用编程式事务管理时，需要通过`TransactionTemplate`或`PlatformTransactionManager`接口进行事务管理。这种方式可以实现对事务隔离级别、超时时间等细节的控制，但需要开发者手动编写大量的重复性代码，并且不易于维护。
2. 声明式事务管理：通过配置`XML`文件或注解来实现事务管理。声明式事务管理通常使用`AOP`技术，在方法执行前后自动添加事务管理的切面，以实现事务的自动管理。在`XML`文件中，可以使用`tx:advice`元素和`tx:annotation-driven`元素来声明事务管理。在注解驱动的情况下，可以使用`@Transactional`注解来标记需要进行事务管理的方法。声明式事务管理具有可读性强、可维护性好等优点，是`Spring`框架中最为常用的事务管理方式之一。
3. 注解驱动的事务管理：使用`@Transactional`注解来标记需要进行事务管理的方法。通过该注解，可以指定事务的传播行为、隔离级别、超时时间、只读属性等。这种方式非常方便，但不够灵活，不能对多个方法进行统一控制。
4. 基于`AspectJ`的事务管理：使用`AspectJ`切面来管理事务，可以实现更细粒度的事务管理。`AspectJ`是一个独立的`AOP`框架，可以作为`Spring AOP`的替代品，提供更强大和灵活的`AOP`支持。基于`AspectJ`的事务管理可以使用编译时、织入时或运行时三种方式，其中运行时方式最为常用。

#### 2、Spring的异常管理

1. 统一异常处理：可以使用`@ControllerAdvice`和`@ExceptionHandler`注解来定义全局的异常处理器，捕获并处理应用程序中的所有异常。统一异常处理可以将不同的异常转换为统一的响应格式，以提高用户体验，避免暴露敏感信息。通过这种方式，可以更好地管理异常，并为客户端提供更加友好的错误提示。
2. 数据访问异常处理：`Spring`框架提供了`DataAccessException`及其子类，用于处理数据访问过程中可能抛出的各种异常，例如数据库连接异常、`SQL`语句异常、事务回滚异常等。在使用Spring进行数据访问时，通常需要对这些异常进行特殊处理，例如记录日志、发出警报等操作。
3. 事务异常处理：`Spring`框架提供了`TransactionException`及其子类，用于处理在事务管理过程中可能出现的各种异常，例如事务超时异常、事务回滚异常等。当使用`Spring`进行声明式或编程式事务管理时，通常需要对这些异常进行特殊处理，以保证事务的正确执行。
4. 校验异常处理：`Spring`框架提供了`ValidationException`及其子类，用于处理数据校验过程中可能抛出的各种异常，例如表单验证错误、输入参数非法等。在开发`Web`应用程序时，通常需要对这些异常进行特殊处理，并返回相应的错误信息给用户。

#### 3、AOP代理模式

1. `JDK`动态代理：如果目标对象的实现类实现了接口。`Spring AOP`将会采用`JDK`动态代理来生成`AOP`代理类
2. `CGLIB`代理：如果日标对象的实现类没有实现接口，`Spring AOP`将会采用`CGLIB`来生成`AOP`代理类

##### 3.1 、什么是JDK动态代理？

通过在运行时动态地创建接口的代理对象来实现对目标对象的的增强和拦截，`JDK`动态代理主要基于反射机制实现，与传动的静态代理具有更高的灵活性和扩展性

1. 目标类必须==实现至少一个接口==；
2. 需要提供一个`InvocationHandler`接口的实现类，用于处理代理对象的方法调用；当代理对象方法被调用时，`JDK`动态代理会将此方法的调用转发给`InvocationHandler`对象的`invoke()`方法。

```java
public interface UserService {
    void addUser(String name, String password);
}

/* 代理对象 - 实现接口 */
public class UserServiceImpl implements UserService {
    @Override
    public void addUser(String name, String password) {
        System.out.println("Add user: " + name);
    }
}
```

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/* InvocationHandler实现类，负责拦截与增强 */
public class UserServiceProxy implements InvocationHandler {

    private Object target;

    public UserServiceProxy(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before add user.");
        Object result = method.invoke(target, args);
        System.out.println("After add user.");
        return result;
    }

    public static void main(String[] args) {
        UserService userService = new UserServiceImpl();
        UserService proxy = (UserService) Proxy.newProxyInstance(
                userService.getClass().getClassLoader(),
                userService.getClass().getInterfaces(),
                new UserServiceProxy(userService));
        proxy.addUser("Alice", "123456");
    }
}
```

##### 3.2、什么是CGLIB动态代理

是一种利用`CGLIB`框架动态生成字节码来实现对象代理的技术，`CGLIB`代理可以直接对目标类进行代理，无需对目标类实现接口，`CGLIB`代理通过==生成目标对象的子类==来实现对目标类的扩展和增强。当调用目标类的方法时，`CGLIB`会拦截此方法，并回调实现`MethodInterceptor`接口的拦截器，实现对该方法的处理。在拦截器中可以使用`super`关键字执行原始方法的逻辑，也可以根据需求进行其他操作。`CGLIB`代理是基于字节码增强技术，能代理任何没有被`final`修饰的类和方法，==无需实现接口==，执行效率比`JDK`动态代理更高，同时也更消耗资源。

```java
import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class MyClass {

    public static void main(String[] args) {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(MyClass.class);
        enhancer.setCallback(new MyInterceptor());

        MyClass myObject = (MyClass) enhancer.create();
        myObject.myMethod();
    }

    public void myMethod() {
        System.out.println("Hello World!");
    }

    static class MyInterceptor implements MethodInterceptor {
        public Object intercept(Object obj, java.lang.reflect.Method method, Object[] args,
                                MethodProxy proxy) throws Throwable {
            System.out.println("Before method " + method.getName());
            Object result = proxy.invokeSuper(obj, args);
            System.out.println("After method " + method.getName());
            return result;
        }
    }
}
```

#### 4、事务传播行为

- **支持当前事务的情况：**

TransactionDefinition.PROPAGATION_REQUIRED：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务；

TransactionDefinition.PROPAGATION_SUPPORTS：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行；

TransactionDefinition.PROPAGATION_MANDATORY：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。

- **不支持当前事务的情况：**

TransactionDefinition.PROPAGATION_REQUIRES_NEW：创建一个新的事务，如果当前存在事务，则把当前事务挂起；

TransactionDefinition.PROPAGATION_NOT_SUPPORTED：以非事务方式运行，如果当前存在事务，则把当前事务挂起。

TransactionDefinition.PROPAGATION_NEVER：以非事务方式运行，如果当前存在事务，则抛出异常。

- **其他情况：**

TransactionDefinition.PROPAGATION_NESTED：如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于 TransactionDefinition.PROPAGATION_REQUIRED。

