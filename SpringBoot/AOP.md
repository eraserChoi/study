### AOP (Aspect-Oriented Programming) - 이해하기

AOP(Aspect-Oriented Programming, 관점 지향 프로그래밍)는 **비즈니스 로직과 부가적인 기능을 분리**하기 위해 사용되는 프로그래밍 기법입니다. 주로 스프링 프레임워크에서 사용되며, 로깅, 트랜잭션 관리, 보안 같은 횡단 관심사(Cross-Cutting Concern)를 효율적으로 관리할 수 있게 해줍니다.

#### AOP란 무엇인가?

AOP는 \*\*프로그램의 여러 부분에 공통적으로 사용하는 기능(횡단 관심사)\*\*을 핵심 로직과 분리하여 **재사용성과 유지 보수성을 높이는 방법**입니다. 전통적인 객체 지향 프로그래밍(OOP)에서는 핵심 비즈니스 로직과 로깅, 보안, 예외 처리와 같은 부가적인 기능이 뒤섞이게 되면서 코드가 복잡해지는 문제가 생깁니다. AOP는 이러한 공통 기능을 별도의 모듈로 관리함으로써 **핵심 비즈니스 로직에 집중할 수 있도록** 도와줍니다.

AOP의 핵심 개념은 **Aspect**입니다. Aspect는 횡단 관심사를 모듈화한 것으로, 이를 통해 특정 기능을 별도로 분리해 관리할 수 있습니다. 스프링에서는 AOP를 통해 메소드의 실행 전후나 예외 발생 시점에 특정 로직을 쉽게 적용할 수 있습니다.

#### AOP의 동작 원리

AOP는 **프록시(Proxy) 패턴**을 기반으로 동작합니다. 스프링 AOP는 런타임 시점에 프록시 객체를 생성하여, 대상 객체의 메소드 호출을 가로채고 필요한 부가 기능(Advice)을 적용합니다. 스프링 AOP에서는 보통 **JDK 동적 프록시** 또는 **CGLIB 프록시**를 사용하여 AOP를 구현합니다.

- **JDK 동적 프록시**: 인터페이스를 구현한 대상에 대해 프록시 객체를 생성합니다. 인터페이스 기반으로 동작하며, 인터페이스가 없는 클래스에는 사용할 수 없습니다.
- **CGLIB 프록시**: 인터페이스가 없는 클래스에도 프록시를 생성할 수 있는 방법으로, 대상 클래스를 상속받아 프록시 객체를 생성합니다.

스프링은 빈을 생성할 때 해당 빈에 AOP가 적용되어야 한다면, 프록시 객체를 대신 생성하여 컨테이너에 등록합니다. 이후 클라이언트가 빈의 메소드를 호출하면, 프록시 객체가 먼저 호출되고 프록시 객체 내부에서 **Join Point**를 통해 실제 대상 객체의 메소드를 호출하게 됩니다. 이 과정에서 **Advice**가 적용되어 부가 기능을 추가하게 됩니다.

#### AOP의 주요 용어

- **Aspect**: 횡단 관심사를 모듈화한 것, 예를 들어 로깅이나 보안 기능을 Aspect로 구현할 수 있습니다.
- **Join Point**: 프로그램 실행 시점에서 AOP의 기능이 적용될 수 있는 지점입니다. 보통 메서드 실행 지점이나 예외 발생 지점이 될 수 있습니다. 스프링 AOP에서는 주로 메서드 호출 시점이 Join Point로 사용됩니다. Join Point는 Advice가 적용될 수 있는 모든 위치를 의미하며, 메서드의 실행 전, 실행 후, 또는 예외가 발생했을 때 같은 다양한 시점을 포함합니다.
- **Advice**: 특정 Join Point에서 실행되는 작업을 의미하며, Aspect의 실제 동작 부분입니다. Advice는 언제 실행되느냐에 따라 **Before, After, AfterReturning, AfterThrowing, Around** 등으로 구분됩니다. 예를 들어, **Before Advice**는 메서드 실행 전에 동작하고, **After Advice**는 메서드 실행 후에 동작합니다. **Around Advice**는 메서드 실행 전후 전체를 감싸며, 실행 시간을 측정하거나 추가적인 로직을 실행하기에 유용합니다.
- **Pointcut**: Advice가 적용될 Join Point를 정의하는 표현식입니다. 어떤 메소드에 AOP를 적용할지 설정할 수 있습니다.

#### AOP는 왜 사용할까?

1. **중복 코드 제거**: 로깅, 트랜잭션 관리 등과 같은 공통 기능을 각 클래스에 반복해서 작성할 필요 없이, 한 곳에서 관리할 수 있습니다.
2. **유지보수성 향상**: 핵심 비즈니스 로직과 부가적인 기능을 분리하여 관리하므로, 코드의 가독성이 높아지고 수정이 쉬워집니다.
3. **비즈니스 로직 집중**: 개발자가 핵심 로직에 집중할 수 있게 하여, 부가적인 기능은 AOP를 통해 관리하도록 합니다.

#### AOP 사용 예시

예를 들어, 서비스의 모든 메소드에 대해 실행 시간을 측정하고 로그를 남기고 싶다고 가정해봅시다. AOP 없이 이 기능을 구현하려면 각 메소드마다 시작 시간과 종료 시간을 기록하는 로직을 추가해야 합니다. 이렇게 되면 코드가 중복되고, 각 클래스에 불필요한 코드가 들어가 복잡해집니다.

AOP를 사용하면 이러한 문제를 쉽게 해결할 수 있습니다. 아래는 스프링 AOP를 사용하여 모든 서비스 메소드의 실행 시간을 로깅하는 예시입니다.

```java
@Aspect
@Component
public class LoggingAspect {

    @Pointcut("execution(* com.example.service..*(..))")
    public void serviceMethods() {}

    @Around("serviceMethods()")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```

위 코드에서 `@Aspect` 어노테이션을 사용해 Aspect를 정의했습니다. `@Pointcut`은 실행될 메소드를 지정하고, `@Around` Advice를 통해 지정된 메소드가 실행될 때마다 그 실행 시간을 기록하는 로직을 추가했습니다. 이렇게 하면 모든 서비스 메소드의 실행 시간을 효율적으로 로깅할 수 있습니다.

만약 실무에서 특정 진단을 위한 서비스 기능 로그를 데이터베이스에 저장하고 싶다면, AOP를 사용하여 이를 구현할 수 있습니다. 예를 들어, 아래와 같이 AOP를 활용해 특정 메소드의 실행 로그를 데이터베이스에 저장하는 로직을 구현할 수 있습니다. 

```java
@Aspect
@Component
public class DiagnosisLoggingAspect {

    @Autowired
    private DiagnosisLogRepository diagnosisLogRepository;

    @Pointcut("execution(* com.example.service.DiagnosisService..*(..))")
    public void diagnosisMethods() {}

    @Around("diagnosisMethods()")
    public Object logDiagnosis(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        
        DiagnosisLog log = new DiagnosisLog();
        log.setMethodName(joinPoint.getSignature().toString());
        log.setExecutionTime(executionTime);
        log.setTimestamp(new Date());
        diagnosisLogRepository.save(log);
        
        return proceed;
    }
}
```

위 코드에서는 `DiagnosisService` 클래스의 메소드에 대해 실행 시간을 측정하고, 이를 `DiagnosisLogRepository`를 통해 데이터베이스에 저장하는 기능을 구현했습니다. 이를 통해 진단에 대한 실행 로그를 쉽게 관리할 수 있습니다.

#### 실무에서 AOP 사용하기

- **트랜잭션 관리**: 데이터베이스와 관련된 작업에서 트랜잭션을 관리할 때, AOP를 통해 트랜잭션 시작과 종료를 공통적으로 처리할 수 있습니다.
- **로깅**: 애플리케이션의 흐름을 추적하거나 문제를 디버그하기 위해 로깅을 AOP로 관리합니다.
- **보안**: 메소드 호출 전에 사용자 권한을 체크하는 로직을 AOP로 구현해 쉽게 적용할 수 있습니다.

#### 기술면접에서 나올 수 있는 질문과 답변 예시

1. **AOP란 무엇인가요?**

   - AOP는 관점 지향 프로그래밍으로, 핵심 로직과 부가적인 기능(횡단 관심사)을 분리하여 관리하기 위한 프로그래밍 기법입니다. 주로 로깅, 트랜잭션 관리, 보안과 같은 공통 기능을 모듈화할 때 사용됩니다.

2. **AOP를 사용하는 이유는 무엇인가요?**

   - AOP를 사용하면 중복 코드를 제거하고, 핵심 비즈니스 로직에 집중할 수 있으며, 유지보수성을 높일 수 있습니다. 공통 기능을 한 곳에서 관리함으로써 코드의 가독성과 재사용성을 높입니다.

3. **AOP의 주요 개념에는 무엇이 있나요?**

   - Aspect, Join Point, Advice, Pointcut 등이 있으며, Aspect는 횡단 관심사를 모듈화한 것이고, Advice는 실제 동작을 정의하는 부분입니다.

