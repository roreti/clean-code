# 1. 깨끗한 코드
## 코드가 존재하리라
* 코드가 사라질 가망성은 없으며 코드는 요구사항을 상세히 표현하는 수단
* 기계가 실할 정도로 상세하게 요구사항을 명시하는 작업이 프로그래밍이고 이러렇게 명시한 결과가 코드!
* 궁극적으로 코드는 요구사항을 표현하는 언어라는 것을 명심

## 나쁜코드 
* Killer app 하나로 대박난 회사가 머지 않아 망한 일이 있었다. 그 원인은 나쁜 코드
* 일정에 맞추기 위해 나쁜 코드들을 방치하고는 '나중에 고쳐야지'라고 생각한 경험
    * Later equals never * LeBlanc's law (나중은 절대 오지 않는다 * 르블랑의 법칙)
    
## 나쁜코드로 치르는 대가
* 초기에는 매우 빠른 속도로 진행되던 프로젝트가 1~2년 만에 달팽이처럼 느린 페이스로 진행되게 되는 것을 볼 수 있다.
    * 나쁜 코드로 짠 프로그램에 가해지는 변경사항은 어느것 하나 사소하지 않다.
* 나쁜 코드가 쌓일 수록 그 팀의 생산성은 떨어지고 이윽고 0에 수렴한다.
    * 관리팀은 인력을 추가하려 한다.
    * 새 팀원은 구조를 이해하지 못한다.
    * 그 팀은 '새 인력을 투입했으므로 생산성이 늘겠지'라는 압박을 받는다.
    * <b>나쁜 코드는 더 쌓인다.</b>
### 원대한 재설계의 꿈
* 팀은 봉기한다. 재디자인을 요구한다.
* 달갑지는 않지만 관리팀 또한 개발팀의 생산성이 바닥을 기는 것을 알고 있으므로 허가하게 된다.
* 새 tiger team이 setup되고 기존의 프로덕트의 스펙 + 새로운 기능을 맡게 된다. 기존의 팀원들은 기존의 코드를 유지보수하게 된다.
* 두 팀은 오래 동안 경쟁한다.
* tiger team이 기존의 프로젝트를 거의 따라잡을 즈음, tiger team의 초기 맴버들은 대부분 새 맴버들로 교체되어 있다.
* 다시 재디자인을 요구한다(반복)
### 태도
* 한시간이면 될 변경을 1주일이 넘도록 보고 있다던지, 한줄만 바꾸면 될 문제를 가지고 수백개의 모듈을 건드린다던지 하는 증상은 흔하다.
* 왜 좋은 코드는 그렇게도 빠르게 나쁜 코드로 바뀌는 것일까?
* 초기와 다른 스펙, 스케쥴, 멍청한 매니저, 참을성 없는 고객, 쓸데없는 마케팅 인간들을 비난할 지도 모른다. 하지만 그건 우리 잘못이다.
* 대부분은 매니저들은 우리 생각보다 더 진실을 원하고 있다.
* 그들 또한 좋은 코드를 원한다. 그와 동시에 스케쥴 또한 지키고 싶어한다.
* 그와 마찬가지로, 좋은 코드를 지키는 것 또한 우리의 몫이다.
### 원초적 난제
* 더러운 코드는 생산성을 저하시킨다. 그와 동시에 개발자들은 기한을 맞추기 위해 더러운 코드를 짠다. 하지만, 더러운 코드를 만들어서는 절대 기한을 맞추지 못한다.
* 빨리 가기 위한 단 하나의 방법은 "최대한 깨끗한 코드를 항상 유지하는 것"이다.
### 깨끗한 코드라는 예술?
* 깨끗한 코드와 나쁜 코드를 구분할 줄 안다고 깨끗한 코드를 작성할 줄 안다는 뜻은 아님
* 깨끗한 코드를 작성하려면 "청결"이라는 힘겹게 습득한 감각을 활용해 자잘한 기법들을 적용하는 절제와 규율이 필요
### 깨끗한 코드란?
> 비야네 스트롭스트롭 <b> Bjarne Stroustrup, inventor of C++ and author of The C++ Programming Language </b>
* 코드는 즐겁게 읽혀야 한다.
* 효율적인 코드라야 한다. 이는 성능적 측면 뿐만 아니라 나쁜 코드는 난장판을 더 키우기 때문이다.(깨진 유리창 이론)<sup> [1](#fn1)</sup>
* 에러 핸들링, 메모리 누수, 경쟁상태, 일관되지 않은 네이밍 등 디테일을 신경쓰라.
* 나쁜 코드는 여러가지 일을 하려고 한다. 나쁜 코드는 애매한 의도와 모호한 목적을 포함한다. 클린코드는 한 가지에 집중한다. **클린코드는 한 가지 일을 잘 한다.**
> 그래디부치 <b> Grady Booch, author of Object Oriented Analysis and Design with Applications </b>
* 클린코드는 하나의 잘 쓰여진 산문처럼 읽혀야 한다. 소설의 기승전결처럼 문제를 제시하고 명쾌한 해답을 제시해야 한다.
* 명백한 추상: 코드는 추측 대신 실제를 중시, 필요한 것만 포함하며 독자로 하여금 결단을 내렸다고 생각하게 해야 한다.
> 큰 데이브 토마스 <b> “Big” Dave Thomas, founder of OTI, godfather of the Eclipse strategy </b>
* 다른 이가 수정하기 쉬워야 한다.
* 테스트를 해야 한다.
* 코드는 간결할 수록 좋다.(Smaller is better)
* 코드는 세련되어야 한다.
> 마이클 페더스 <b> Michael Feathers, author of Working Effectively with Legacy Code </b>
* 코드를 **care**하라.(주의, 관심을 가지고 작성하라)
> 론 제프리스 <b> Ron Jeffries, author of Extreme Programming Installed and Extreme Programming Adventures in C# </b>
* 중복을 없애라
* 클래스/메서드는 한 가지 일만 하게 하라
* 메서드의 이름 등으로 코드가 하는 일을 명시하라
* (메서드 등을) 일찍 추상화해서 프로젝트를 빠르게 진행할 수 있게 하라

> 워드 커닝햄 <b> Ward Cunningham, inventor of Wiki, inventor of Fit, coinventor of eXtreme Programming. Motive force behind Design Patterns. Smalltalk and OO thought leader. The godfather of all those who care about code. </b>
* 읽고, 끄덕이고, 다음으로 넘어갈 수 있는 코드를 작성하라.
* 당신이 사용하는 언어를 탓하지 말라. 코드를 아름답게 만드는 것은 프로그래머이다.

## 보이스카우트 규칙
* 캠프장은 처음 왔을 때보다 더 깨끗하게 해놓고 떠나라.
## 프리퀄의 원칙
* SRP(The Single Responsibility Prilciple): 클래스에는 한 가지, 단 한 가지 변경 이유만 존재해야 한다.
* OCP(The Open Closed Principle): 클래스는 확장에 열려 있어야 하며 변경에 닫혀 있어야 한다.
* LSP(The Liskov Substitution Principle): 상속받은 클래스는 기초 클래스를 대체할 수 있어야 한다.
* DIP(The Dependency Inversion Principle): 추상화에 의존해야 하며, 구체화에 의존하면 안 된다.
* ISP(The Interface Segregation Principle): 클라이언트에 밀접하게 작게 쪼개진 인터페이스를 유지한다.


# 2. 의미 있는 이름

## 의도를 분명히 밝혀라
* 변수의 존재 이유, 기능, 사용법 등이 변수/함수/클래스명에 드러나야 한다. 따로 주석이 필요하지 않을 정도로.  
* 의미를 함축하거나 독자(코드를 읽는 사람)가 사전지식을 가지고 있다고 가정하지 말자.
* 예시 1
  * Bad
    * int d; // elapsed time in days
  * Good
    * int elapsedTimeInDays;
    * int daysSinceCreation;
    * int daysSinceModification;
    * int fileAgeInDays;
* 예시 2

```java
// Bad
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int[]>();
    for (int[] x : theList) {
        if (x[0] == 4) {
            list1.add(x);
        }
    }
    return list1;
}
```
 
```java
// Good
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell : gameBoard) {
        if (cell[STATUS_VALUE] == FLAGGED) {
            flaggedCells.add(cell);
        }
    }
    return flaggedCells;
}
```
## 그릇된 정보를 피하라
* 중의적으로 해석될 수 있는 이름 지양하기.  
* 개발자에게는 특수한 의미를 가지는 단어(List 등)는 실제 컨테이너가 List가 아닌 이상 accountList와 같이 변수명에 붙이지 말자. 차라리 accountGroup, bunchOfAccounts, accounts등으로 명명하자  
* 비슷해 보이는 명명에 주의하자.  

## 의미 있게 구분하라
* 말이 안되는 단어(한 글자만 바꾼다던지 한 단어), [a1, a2, …]과 같이 숫자로 구분하는 경우 주의  
* 클래스 이름에 Info, Data와 같은 불용어를 붙이지 말자. 정확한 개념 구분이 되지 않음  
* 예시  
 * `Name` VS `NameString`
 * `getActiveAccount()` VS `getActiveAccounts()` VS `getActiveAccountInfo()` (이들이 혼재할 경우 서로의 역할을 정확히 구분하기 어렵다.)
 * `money` VS `moneyAmount`
 * `message` VS `theMessage`
 
## 발음하기 쉬운 이름을 사용하라

```java
// Bad
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
    /* ... */
};
```
 
```java
// Good
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
    /* ... */
};
```

## 검색하기 쉬운 이름을 사용하라
* 상수는 static final과 같이 정의해 쓰자.  
* 변수 이름의 길이는 변수의 범위에 비례해서 길어진다.  

## 인코딩을 피하라
### 헝가리식 표기범
* 변수명에 해당 변수의 타입(String, Int 등)을 적지 말자
### 멤버 변수 접두어
* 맴버 변수 접두어를 붙이지 말자
### 인터페이스 클래스와 구현 클래스
* 인터페이스 클래스와 구현 클래스를 나눠야 한다면 구현 클래스의 이름에 정보를 인코딩하자. 
## 자신의 기억력을 자랑하지 마라
* 독자가 머리속으로 한번 더 생각해 변환해야 할만한 변수명을 쓰지 말라.(eg, URL에서 호스트와 프로토콜을 제외한 소문자 주소를 r이라는 변수로 명명하는 일 등)  
* 똑똑한 프로그래머와 전문가 프로그래머를 나누는 기준 한가지는 "Clarity(명료함)"이다.  
## 클래스 이름
* 명사 혹은 명사구를 사용하라.(Customer, WikiPage, Account, AddressParser)  
* Manager, Processor, Data, Info와 같은 단어는 피하자  
* 동사는 사용하지 않는다.  
## 메서드 이름
* 동사 혹은 동사구를 사용하라.(postPayment, deletePayment, deletePage, save 등)  
* 접근자, 변경자, 조건자는 get, set, is로 시작하자.  (추가: should, has 등도 가능)
* 생성자를 오버로드할 경우 정적 팩토리 메서드를 사용하고 해당 생성자를 private으로 선언한다.
```java 
// 첫번째 보다 두 번째 방법이 더 좋다.  
Complex fulcrumPoint = new Complex(23.0);  
Complex fulcrumPoint = Complex.FromRealNumber(23.0);  
```
## 기발한 이름은 피하라  
* 특정 문화에서만 사용되는 재미있는 이름보다 의도를 분명히 표현하는 이름을 사용하라  
  * HolyHandGrenade → DeleteItems  
  * whack() → kill()  

## 한 개념에 한 단어를 사용하라  
* 추상적인 개념 하나에 단어 하나를 사용하자.  
  * fetch, retrieve, get  
  * controller, manager, driver  

## 말장난을 하지 마라(위 내용에 이어)  
* 한 단어를 두 가지 목적으로 사용하지 말자. 아래와 같은 경우에는 ii를 append 혹은 insert로 바꾸는게 옳겠다.

```java
public static String add(String message, String messageToAppend)  
public List<Element> add(Element element)  
```

## 해법 영역(Solution Domain) 용어를 사용하자  
* 개발자라면 당연히 알고 있을 `JobQueue`, `AccountVisitor(Visitor pattern)`등을 사용하지 않을 이유는 없다. 전산용어, 알고리즘 이름, 패턴 이름, 수학 용어 등은 사용하자.  

## 문제 영역(Problem Domain) 용어를 사용하자  
* 적절한 프로그래머 용어(위 13)가 없거나 문제영역과 관련이 깊은 용어의 경우 문제 영역 용어를 사용하자.  

## 의미 있는 맥락을 추가하라  
* 클래스, 함수, namespace등으로 감싸서 맥락(Context)을 표현하라  
* 그래도 불분명하다면 접두어를 사용하자.  

```java
// Bad
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {  
        number = "no";  
        verb = "are";  
        pluralModifier = "s";  
    }  else if (count == 1) {
        number = "1";  
        verb = "is";  
        pluralModifier = "";  
    }  else {
        number = Integer.toString(count);  
        verb = "are";  
        pluralModifier = "s";  
    }
    String guessMessage = String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );

    print(guessMessage);
}
```

```java
// Good
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;

    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );
    }

    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }

    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }

    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }

    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```

## 불필요한 맥락을 없애라  
* `Gas Station Delux` 이라는 어플리케이션을 작성한다고 해서 클래스 이름의 앞에 GSD를 붙이지는 말자. G를 입력하고 자동완성을 누를 경우 모든 클래스가 나타나는 등 효율적이지 못하다.  
위 a처럼 접두어를 붙이는 것은 모듈의 재사용 관점에서도 좋지 못하다. 재사용하려면 이름을 바꿔야 한다.(eg, `GSDAccountAddress` 대신 `Address`라고만 해도 충분하다.)  


> 두려워하지 말고 서로의 명명을 지적하고 고치자. 그렇게 하면 이름을 외우는 것에 시간을 빼앗기지 않고 "자연스럽게 읽히는 코드"를 짜는 데에 더 집중할 수 있다.  
