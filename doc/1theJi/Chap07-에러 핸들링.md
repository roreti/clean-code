# 오류 처

> 오류 처리는 중요하다. 하지만 로직을 헷갈리게 만드는 오류처리는 나쁘다.  
(Error handling is important, but if it obscures logic, it's wrong.)

## 오류코드 대신 예외를 사용하라 ##
- 예전 프로그래밍 언어들은 exceptions를 제공하지 않았다.
- 그 경우, 개발자들은 에러 flag를 set하거나 에러 코드를 리턴, 호출하는 측에서 예외처리를 해줘야 했다.
- 하지만 이러한 방식은 예외처리를 잊어버리기 쉽고 로직을 헷갈리게 하기 쉽다.
- 그러므로 exceptions를 사용하자. 겉보기에만 아름다운 코드가 되는게 아니라 "실제 로직"과 "예외처리" 부분이 나뉘어져 필요한 부분에 집중할 수 있게 된다.
```java
// Bad
public class DeviceController {
  ...
  public void sendShutDown() {
    DeviceHandle handle = getHandle(DEV1);
    // Check the state of the device
    if (handle != DeviceHandle.INVALID) {
      // Save the device status to the record field
      retrieveDeviceRecord(handle);
      // If not suspended, shut down
      if (record.getStatus() != DEVICE_SUSPENDED) {
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
      } else {
        logger.log("Device suspended. Unable to shut down");
      }
    } else {
      logger.log("Invalid handle for: " + DEV1.toString());
    }
  }
  ...
}
```
```java
// Good
public class DeviceController {
  ...
  public void sendShutDown() {
    try {
      tryToShutDown();
    } catch (DeviceShutDownError e) {
      logger.log(e);
    }
  }
    
  private void tryToShutDown() throws DeviceShutDownError {
    DeviceHandle handle = getHandle(DEV1);
    DeviceRecord record = retrieveDeviceRecord(handle);
    pauseDevice(handle); 
    clearDeviceWorkQueue(handle); 
    closeDevice(handle);
  }
  
  private DeviceHandle getHandle(DeviceID id) {
    ...
    throw new DeviceShutDownError("Invalid handle for: " + id.toString());
    ...
  }
  ...
}
```

## Try-Catch-Finally문을 먼저 써라 ##
- try문은 transaction처럼 동작하는 실행코드로, catch문은 try문에 관계없이 프로그램을 일관적인 상태로 유지하도록 한다.
- 이렇게 함으로써 코드의 "Scope 정의"가 가능해진다.
- 예시: 잘못된 input을 넣을 경우 StorageException을 제대로 던지는지 확인하는 테스트 코드를 작성해보자
```java
  // Step 1: StorageException을 던지지 않으므로 이 테스트는 실패한다.
  
  @Test(expected = StorageException.class)
  public void retrieveSectionShouldThrowOnInvalidFileName() {
    sectionStore.retrieveSection("invalid - file");
  }
  
  public List<RecordedGrip> retrieveSection(String sectionName) {
    // dummy return until we have a real implementation
    return new ArrayList<RecordedGrip>();
  }
```
```java
  // Step 2: 이제 테스트는 통과한다.
  public List<RecordedGrip> retrieveSection(String sectionName) {
    try {
      FileInputStream stream = new FileInputStream(sectionName)
    } catch (Exception e) {
      throw new StorageException("retrieval error", e);
    }
  return new ArrayList<RecordedGrip>();
}
```
```java
  // Step 3: Exception의 범위를 FileNotFoundException으로 줄여 정확히 어떤 Exception이 발생한지 체크하자.
  public List<RecordedGrip> retrieveSection(String sectionName) {
    try {
      FileInputStream stream = new FileInputStream(sectionName);
      stream.close();
    } catch (FileNotFoundException e) {
      throw new StorageException("retrieval error", e);
    }
    return new ArrayList<RecordedGrip>();
  }
```

## Unchecked Exceptions를 사용하라 ##
- Checked exception VS Unchecked Exception(참조 1)
- 예외처리에 드는 비용 대비 이득을 생각해봐야 한다.
- 예시
 - 1. 특정 메소드에서 checked exception을 throw하고
 - 2. 3단계(메소드 콜) 위의 메소드에서 그 exception을 catch한다면
 - 3. 모든 중간단계 메소드에 exception을 정의해야 한다.(자바의 경우 메소드 선언에 throws 구문을 붙이는 등)
 - Open Closed Principle (참조 2)
- 상위 레벨 메소드에서 하위 레벨 메소드의 디테일에 대해 알아야 하기 때문에 캡슐화 또한 깨진다.
- 필요한 경우 checked exceptions를 사용해야 되지만 일반적인 경우 득보다 실이 많다.

## 예외에 의미를 제공하라 ##
- 예외가 발생한 이유와 좀 더 구체적인 Exception 타입을 제공하라.

## 호출자를 고려해 예외 클래스를 정의하 ##
- Exception class를 만드는 데에서 가장 중요한 것은 "오류를 잡아내는 방법"이다.
- 써드파티 라이브러리를 사용하는 경우 그것들을 wrapping함으로써
 - 1. 라이브러리 교체 등의 변경이 있는 경우 대응하기 쉬워진다.
 - 2. 라이브러리를 쓰는 곳을 테스트할 경우 해당 라이브러리를 만들거나 함으로써 테스트하기 쉬워진다.
 - 3. 라이브러리에 대한 API설계한 방식에 발목 잡히지 않는다(내 입맛에 만듬!!)
- 보통 특정 부분의 코드에는 exception 하나로 충분히 예외처리 할 수 있다.
```java
  // Bad
  // catch문의 내용이 거의 같다.
  
  ACMEPort port = new ACMEPort(12);
  try {
    port.open();
  } catch (DeviceResponseException e) {
    reportPortError(e);
    logger.log("Device response exception", e);
  } catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("Unlock exception", e);
  } catch (GMXError e) {
    reportPortError(e);
    logger.log("Device response exception");
  } finally {
    ...
  }
```
```java
  // Good
  // ACME 클래스를 LocalPort 클래스로 래핑해 new ACMEPort().open() 메소드에서 던질 수 있는 exception들을 간략화
  
  LocalPort port = new LocalPort(12);
  try {
    port.open();
  } catch (PortDeviceFailure e) {
    reportError(e);
    logger.log(e.getMessage(), e);
  } finally {
    ...
  }
  
  public class LocalPort {
    private ACMEPort innerPort;
    public LocalPort(int portNumber) {
      innerPort = new ACMEPort(portNumber);
    }
    
    public void open() {
      try {
        innerPort.open();
      } catch (DeviceResponseException e) {
        throw new PortDeviceFailure(e);
      } catch (ATM1212UnlockedException e) {
        throw new PortDeviceFailure(e);
      } catch (GMXError e) {
        throw new PortDeviceFailure(e);
      }
    }
    ...
  }
```

## 정상 흐름을 정의하라 ##
- 일반적으로는 위에서 봤던 방식들이 유용하지만, 아닐때도 있다.
- 이런 경우, Martin Fowler의 Special Case Pattern을 사용하자.(참조 3)
 - 1. 코드를 부르는 입장에서 예외적인 상황을 신경쓰지 않아도 된다.
 - 2. 예외상황은 special case object 내에 캡슐화된다.
```java
  // Bad

  try {
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
  } catch(MealExpensesNotFound e) {
    m_total += getMealPerDiem();
  }
```
```java
  // Good

  // caller logic.
  ...
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotal();
  ...
  
  public class PerDiemMealExpenses implements MealExpenses {
    public int getTotal() {
      // return the per diem default
    }
  }
  
  // 이해를 돕기 위해 직접 추가한 클래스
  public class ExpenseReportDAO {
    ...
    public MealExpenses getMeals(int employeeId) {
      MealExpenses expenses;
      try {
        expenses = expenseReportDAO.getMeals(employee.getID());
      } catch(MealExpensesNotFound e) {
        expenses = new PerDiemMealExpenses();
      }
      
      return expenses;
    }
    ...
  }
```

## Null을 반하지 마라 ##
- null을 리턴하고 싶은 생각이 들면 위의 Special Case object를 리턴하라.
- 써드파티 라이브러리에서 null을 리턴할 가능성이 있는 메서드가 있다면 Exception을 던지거나 Special Case object를 리턴하는 매서드로 래핑하라.
```java
  // BAD!!!!

  public void registerItem(Item item) {
    if (item != null) {
      ItemRegistry registry = peristentStore.getItemRegistry();
      if (registry != null) {
        Item existing = registry.getItem(item.getID());
        if (existing.getBillingPeriod().hasRetailOwner()) {
          existing.register(item);
        }
      }
    }
  }
  
  
  // 위 peristentStore가 null인 경우에 대한 예외처리가 안된 것을 눈치챘는가?
  // 만약 여기서 NullPointerException이 발생했다면 수십단계 위의 메소드에서 처리해줘야 하나?
  // 이 메소드의 문제점은 null 체크가 부족한게 아니라 null체크가 너무 많다는 것이다.
```
```java
  // Bad
  List<Employee> employees = getEmployees();
  if (employees != null) {
    for(Employee e : employees) {
      totalPay += e.getPay();
    }
  }
```

```java
  // Good
  List<Employee> employees = getEmployees();
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
  
  public List<Employee> getEmployees() {
    if( .. 직원이 없다 .. )
      return Collections.emptyList();
    }
}
```

## Null을 전달하지 마라 ##
- null을 리턴하는  것도 나쁘지만 null을 메서드로 넘기는 것은 더 나쁘다.
- null을 메서드의 파라미터로 넣어야 하는 API를 사용하는 경우가 아니면 null을 메서드로 넘기지 마라.
- 일반적으로 대다수의 프로그래밍 언어들은 파라미터로 들어온 null에 대해 적절한 방법을 제공하지 못한다.
- 가장 이성적인 해법은 null을 파라미터로 받지 못하게 하는 것이다.
```java
// Bad
// calculator.xProjection(null, new Point(12, 13));
// 위와 같이 부를 경우 NullPointerException 발생
public class MetricsCalculator {
  public double xProjection(Point p1, Point p2) {
    return (p2.x – p1.x) * 1.5;
  }
  ...
}

// Bad
// NullPointerException은 안나지만 윗단계에서 InvalidArgumentException이 발생할 경우 처리해줘야 함.
public class MetricsCalculator {
  public double xProjection(Point p1, Point p2) {
    if(p1 == null || p2 == null){
      throw InvalidArgumentException("Invalid argument for MetricsCalculator.xProjection");
    }
    return (p2.x – p1.x) * 1.5;
  }
}

// Bad
// 좋은 명세이지만 첫번째 예시와 같이 NullPointerException 문제를 해결하지 못한다.
public class MetricsCalculator {
  public double xProjection(Point p1, Point p2) {
    assert p1 != null : "p1 should not be null";
    assert p2 != null : "p2 should not be null";
    
    return (p2.x – p1.x) * 1.5;
  }
}
```

## 결론 ##
깨끗한 코드와 견고한 코드는 대립되는 목표가 아니다. 예외처리를 로직에서 제거하면 각각에 대해 독립적인 사고가 가능해진다.

---

#### 참조 ####
##### 1. Checked exception VS Unchecked Exception #####
출처: https://madplay.github.io/post/java-checked-unchecked-exceptions
##### 2. Open/Closed Principle #####  
출처: https://m.blog.naver.com/jwyoon25/221615569649
##### 3. Special Case Pattern(by Martin Fowler) #####
  - http://martinfowler.com/eaaCatalog/specialCase.html