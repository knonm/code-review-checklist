# Code review checklist / guidelines

## Magic numbers and hardcoded values

- Bad:
```java
// 26 doesn't mean anything
for (i = 0; i < 26; i++) {}
```

- Good:
```java
// Variable gives the number 26 context
int alphabetLength = 26;

for (i = 0; i < alphabetLength; i++) {}
```

## Lack of defensive code

- Bad:
```java
// "emptyList" could be null or empty
void notDefensive(List<String> emptyList) {
  System.out.println("Index 1: " + emptyList.get(1));
}
```

- Good:
```java
// Avoids undesired exceptions.
void defensive(List<String> emptyList) {
  if (emptyList != null && emptyList.size() > 1) {
    System.out.println("Index 1: " + emptyList.get(1));
  } else {
    System.err.println("List has no elements.");
  }
}
```

## Too defensive code

- Bad:
```java
private static boolean isNumber(String str) {
  // The "try-catch" block is too generic, it will catch exceptions besides the number parse one (NumberFormatException)
  try {
      Double.valueOf(str);
      return true;
  } catch (Exception e) {
      return false;
  }
}
```

- Good:
```java
private static boolean isNumber(String str) {
  // The "try-catch" block only catches NumberFormatException.
  try {
      Double.valueOf(str);
      return true;
  } catch (NumberFormatException e) {
      return false;
  }
}
```

## Functions / Classes / Methods / Variable Names

- Bad:
```java
public class Calendar {

  public void print(boolean datBoi) {
    int mSize = 12;

    for (int z = 0; z < mSize; z++) {
      Month m = Month.of(z);
      if (datBoi) {
        System.out.println(m.toString());
      } else {
        System.out.println(m.getValue());
      }
    }
  }
}
```

- Good:
```java
public class MonthService {

  public void printAllMonths(boolean isDisplayName) {
    int numberOfMonths = 12;

    for (int monthNumber = 1; monthNumber <= numberOfMonths; monthNumber++) {
      Month month = Month.of(monthNumber);
      if (isDisplayName) {
        System.out.println(month.toString());
      } else {
        System.out.println(month.getValue());
      }
    }
  }
}
```

## Return types (see Dependency Inversion Principle)

- Bad:
```java
// Returns implementations
void HashMap<Long, LinkedList<String>> namesById() {...}
```

- Good:
```java
// Returns abstractions
void Map<Long, List<String>> namesById() {...}
```

## Functions / Classes / Methods / Variable Access

- Bad:
```java
package org.checklist.codereview;

// Only classes from "org.checklist.codereview" package uses "Service" class
public class Service {

  final Dao dao;

  Service(Dao dao) {
    this.dao = dao;
  }

  public void insert() {
    Long id = getId();
    dao.insert(id);
  }

  public void update() {
    Long id = getId();
    dao.update(id);
  }

  // Only methods from "Service" class uses "getId"
  public Long getId() {
    return 1L;
  }
}

public class Resource {

  private Service service;

  Resource(Service service) {
    this.service = service;
  }

  public void insert() {
    service.insert();
  }

  public void update() {
    service.update();
  }
}
```

- Good:
```java
package org.checklist.codereview;

// Only classes from "org.checklist.codereview" package uses "Service" class
class Service {

  private final Dao dao;

  Service(Dao dao) {
    this.dao = dao;
  }

  public void insert() {
    Long id = getId();
    dao.insert(id);
  }

  public void update() {
    Long id = getId();
    dao.update(id);
  }

  // Only methods from "Service" class uses "getId"
  private Long getId() {
    return 1L;
  }
}

public class Resource {

  private Service service;

  Resource(Service service) {
    this.service = service;
  }

  public void insert() {
    service.insert();
  }

  public void update() {
    service.update();
  }
}
```

## Remove unused and commented code

- Bad:
```java
// This method does something
public void doSomething(int a, int b) {
  // Uncomment in 3 months
  // int c = a + b;
  // int d = c * c * b - a;

  System.out.println("Hello darkness my old friend...");

  /*
        ／￣＼
        ／   ヽ |_
        ￣＼     ＼
        ／￣ /|/| /ﾚ､ ヽ＿
        ￣＼ Yヘ |/ヘ幺 ∠
        ＜_|(･> <･) 6) ／
          (ﾞ _ ﾞ／＜
          ＞､ーイ￣￣
          ／厂ヽ／￣/＼
          ( ｜ (📕)(> )
          (ﾐ)ﾆ只ニ(ﾐ_／
          /〈/L〉 ヽ
          ｜ ヽ  |
          〈＿／ ＼＿〉
          ／ )  ( ＼
              ________
        _,.-Y  |  |  Y-._
    .-~"   ||  |  |  |   "-.
    I" ""=="|" !""! "|"[]""|     _____
    L__  [] |..------|:   _[----I" .-{"-.
    I___|  ..| l______|l_ [__L]_[I_/r(=}=-P
  [L______L_[________]______j~  '-=c_]/=-^
    \_I_j.--.\==I|I==_/.--L_]
      [_((==)[`-----"](==)j
        I--I"~~"""~~"I--I
        |[]|         |[]|
        l__j         l__j
        |!!|         |!!|
        |..|         |..|
        ([])         ([])
        ]--[         ]--[
        [_L]         [_L]
        /|..|\       /|..|\
      `=}--{='     `=}--{='
      .-^--r-^-.   .-^--r-^-.
    */
}
```

- Good:
```java
public void doSomething(int a, int b) {
  System.out.println("Doing something...");
}
```

## Format code

- Bad:
```java
// Unformatted code
public static
String
iAmNotFormatted
(
int a,
String b
)
{return b + a
;
}
```

- Good:
```java
// Formatted code (stick to IntelliJ/VSCode or use a styleguide like Google's: https://google.github.io/styleguide/javaguide.html)
public static String iAmFormatted(int a, String b) {
  return b + a;
}
```

## Code coverage

- Bad:
```java
class HelloWorld {
  String hello(boolean isGoingHome) {
    if (isGoingHome) {
      return "Bye!";
    }

    return "Hello!";
  }

  // Ternary operators can result in false positives against some categories of code coveragem, like the statement coverage type
  String world(int type) {
    return type < 0 ? "Not a planet." : "Planet.";
  }
}

// Covers half of the "HelloWorld" class
class HelloWorldTest {
  private HelloWorld helloWorld;

  // Covers 50% of the "hello" method
  @Test
  void shouldSayHello() {
    assertEquals("Hello!", helloWorld.hello(false));
  }

  // False positive: covers 100% of the "world" method but isn't reaching "type < 0" condition
  @Test
  void shouldBeAPlanet() {
    assertEquals("Planet.", helloWorld.world(1));
  }
}
```

- Good:
```java
class HelloWorld {
  String hello(boolean isGoingHome) {
    if (isGoingHome) {
      return "Bye!";
    }

    return "Hello!";
  }

  String world(int type) {
    if (type < 0) {
      return "Not a planet.";
    }

    return "Planet.";
  }
}

// Covers all of the "HelloWorld" class
class HelloWorldTest {
  private HelloWorld helloWorld;

  // Covers half of the "hello" method
  @Test
  void shouldSayHello() {
    assertEquals("Hello!", helloWorld.hello(false));
  }

  // Covers the other half of "hello" method
  @Test
  void shouldSayBye() {
    assertEquals("Bye!", helloWorld.hello(true));
  }

  // Covers half of the "world" method
  @Test
  void shouldBeAPlanet() {
    assertEquals("Planet.", helloWorld.world(1));
  }

  // Covers the other half of "world" method
  @Test
  void shouldNotBeAPlanet() {
    assertEquals("Not a planet.", helloWorld.world(-1));
  }
}
```

## Cyclomatic complexity

- Bad:
```java

```

- Good:
```java

```

## DRY (Don't Repeat Yourself)

- Bad:
```java
int sum(int a, int b) {
  return a + b;
}

Integer sumToo(int a, int b) {
  Integer.sum(a, b);
}
```

- Good:
```java
int sum(int a, int b) {
  return a + b;
}
```

## Data structures and Big-O complexities
```

```

## Adherence to the project's architectural pattern (Resource, Service, etc.)

- Resource:
  - Each resource class is associated with a URI template (@Path).
  - Each method represents an endpoint:
    - The method should validate its input (request) using a validator.
    - The method should call the service after validating the request.

- Service:
  - Lógica de negócios da funcionalidade.
  - Normalmente é onde os DAOs e Utils/Commons/Helpers são injetados e usados.
  - Responsável por manipular models e transformar objetos de acordo com a necessidade.

- Model:
  - Podem representar:
    - Tabelas do banco de dados.
    - Requests HTTP (DTOs).
    - Responses HTTP (DTOs), normalmente correspondentes às necessidades de layout do front-end.
    - Command:
      - aaa

- DAO:
  - Objeto que abstrai o acesso ao banco de dados.
  - Cada método da classe corresponde a um SELECT/INSERT/DELETE/UPDATE.

- Mapper:
  - Fazem o "de-para" do resultado das queries do DAO para objetos.

- Validator:
  - Responsável pela validação das requests recebidas nos Resources:
    - Campos estão nulos ou vazios.
    - Tamanho de campos.
    - IDs existem no banco de dados.

- Enums / Types:
  - aaa

- Util / Commons / Helper:
  - aaa

- Excel:

  - ExcelImport:
    - aaa

  - ExcelExportTab:
    - Interface "SkeletonExportTab".

  - ExcelHeader:
    - Interface "ExcelHeader".

  - ImportTab:
    - Interface "ImportTab".

  - ExcelImportValidator:
    - aaa

## SOLID Principles

- Single Responsibility Principle: A class should have one, and only one, reason to change.

  - Bad:
  ```java
  public interface Modem {
    public void dial(string pno);
    public void hangup();
    public void send(char c);
    public char recv();
  }

  public class ConcreteModem implements Modem {...}
  ```

  - Good:
  ```java
  public interface Connection {
    public void dial(string pno);
    public void hangup();
  }

  public interface DataChannel {
    public void send(char c);
    public char recv();
  }

  public class ConcreteModem implements Connection, DataChannel {...}
  ```

- Open-Closed Principle: Software entities should be open for extension and closed for modification. You should be able to extend a classes behavior, without modifying it.

  - Bad:
  ```java
  public class EventInterceptor {
    public void preLoad(Entity entity) {...}
    public void postLoad(Entity entity) {...}
    public void prePersist(Entity entity) {...}
    public void preSave(Entity entity) {...}
  }

  public class EventHandler {
    private Entity entity;
    private EventInterceptor ei;

    // Should modify this method for every new Event implementation
    public void handleEvent(Event event) {
      if (event instanceof PreLoad) {
        ei.preLoad(entity);
      } else if (event instanceof PostLoad) {
        ei.postLoad(entity);
      } else if (event instanceof PrePersist) {
        ei.prePersist(entity);
      } else if (event instanceof PreSave) {
        ei.preSave(entity);
      }
    }
  }
  ```

  - Good:
  ```java
  public abstract class Event {
    abstract void interceptEvent(Entity entity);
  }

  public class PreLoad extends Event {
    @Override
    void interceptEvent(Entity entity) {...}
  }

  public class PostLoad extends Event {
    @Override
    void interceptEvent(Entity entity) {...}
  }

  public class PrePersist extends Event {
    @Override
    void interceptEvent(Entity entity) {...}
  }

  public class PreSave extends Event {
    @Override
    void interceptEvent(Entity entity) {...}
  }

  public class EventHandler {
    private Entity entity;
    private EventInterceptor ei;

    // Method is now closed for changes
    public void handleEvent(Event event) {
      event.interceptEvent(entity);
    }
  }
  ```

- Liskov Substitution Principle: Derived classes must be substitutable for their base classes.

  - Bad:
  ```java
  class Rectangle {
    private int width, height;

    Rectangle(int width, int height) {
      this.width = width;
      this.height = height;
    }

    void setWidth(int width) {
      this.width = width;
    }

    void setHeight(int height) {
      this.height = height;
    }

    int getArea() {
      return this.width * this.height;
    }
  }

  class Square extends Rectangle {
    Square(int widthAndHeight) {
      super(widthAndHeight, widthAndHeight);
    }

    @Override
    void setWidth(int width) {
      super.setWidth(width);
      super.setHeight(width);
    }

    @Override
    void setHeight(int height) {
      super.setWidth(height);
      super.setHeight(height);
    }
  }

  class ShapeTest {
    void assertRectangle() {
      Rectangle rectangle = new Rectangle(10);
      rectangle.setWidth(4);
      rectangle.setHeight(5);

      // OK
      assertEquals(20, rectangle.getArea());
    }

    void assertSquare() {
      Rectangle rectangle = new Square(10);
      rectangle.setWidth(4);
      rectangle.setHeight(5);

      // rectangle.getArea() will return 25 because Square class is overriding setHeight to set both width and height
      assertEquals(20, rectangle.getArea());
    }
  }
  ```

  - Good (through composition):
  ```java
  class Rectangle {
    private int width, height;

    Rectangle(int width, int height) {
      this.width = width;
      this.height = height;
    }

    void setWidth(int width) {
      this.width = width;
    }

    void setHeight(int height) {
      this.height = height;
    }

    int getArea() {
      return this.width * this.height;
    }
  }

  class Square {
    private Rectangle rectange;

    Square(Rectangle rectange) {
      this.rectange = rectange;
    }

    void setWidthAndHeight(int widthAndHeight) {
      rectange.setWidth(widthAndHeight);
      rectange.setHeight(widthAndHeight);
    }
  }

  class ShapeTest {
    void assertRectangle() {
      Rectangle rectangle = new Rectangle(10);
      rectangle.setWidth(4);
      rectangle.setHeight(5);

      // OK
      assertEquals(20, rectangle.getArea());
    }

    // You can't test squares as rectangles anymore
    void assertSquare() {
      Square square = new Rectangle(10);
      square.setWidth(4);
      square.setHeight(5);

      assertEquals(20, square.getArea());
    }
  }
  ```

  - Good (by inheritance):
  ```java
  interface Shape {

  }

  class Rectangle  {
    private int width, height;

    Rectangle(int width, int height) {
      this.width = width;
      this.height = height;
    }

    void setWidth(int width) {
      this.width = width;
    }

    void setHeight(int height) {
      this.height = height;
    }

    int getArea() {
      return this.width * this.height;
    }
  }

  class Square {
    private Rectangle rectange;

    Square(Rectangle rectange) {
      this.rectange = rectange;
    }

    void setWidthAndHeight(int widthAndHeight) {
      super.setWidth(widthAndHeight);
      super.setHeight(widthAndHeight);
    }
  }

  class RectangleTest {
    // You can't test squares as rectangles anymore
    void assertRectangle() {
      Rectangle rectangle = new Rectangle(10);
      rectangle.setWidth(4);
      rectangle.setHeight(5);

      assertEquals(20, rectangle.getArea());
    }
  }
  ```

- Interface Segregation Principle: Make fine grained interfaces that are client specific. Clients should not be forced to depend upon interfaces that they do not use.

  - Bad:
  ```java
  class TimerClient {
    abstract void timeOut();
  }

  class Timer {
    public void register(int timeout, TimerClient timerClient)
  }

  class Door {
    abstract void lock();
    abstract void unlock();
    abstract void isDoorOpen();
  }

  class TimedDoor extends Door {...}
  ```

  ```java
  interface Codec<T> {
    T decode(Reader reader);
    void encode(final T value, Writer writer);
  }

  class StringCodec implements Codec<String> {
    @Override
    public String decode(Reader reader) {...}

    @Override
    public String encode(final String value, Writer writer) {...}
  }
  ```

  - Good:
  ```java
  interface Decoder<T> {
    T decode(Reader reader);
  }

  interface Encoder<T> {
    void encode(final T value, Writer writer);
  }

  class StringCodec implements Decoder<String>, Encoder<String> {
    @Override
    public String decode(Reader reader) {...}

    @Override
    public String encode(final String value, Writer writer) {...}
  }

  class BooleanEncoder implements Encoder<Boolean> {
    @Override
    public void encode(final Boolean value, Writer writer) {...}
  }

  class DoubleFloatCodec implements Decoder<Double>, Encoder<Float> {
    @Override
    public Double decode(Reader reader) {...}

    @Override
    public void encode(final Float value, Writer writer) {...}
  }
  ```

- Dependency Inversion Principle: High level modules should not depend upon low level modules, both should depend on abstraction. Abstractions should not depend upon details, details should depend upon abstractions.

  - Bad:
  ```java
  // High level module
  class ServiceImpl implements IService {

    // Bad: using implementation over abstraction (ValidatorImpl)
    private ValidatorImpl validator;
    // Bad: using implementation over abstraction (DAOImpl)
    private DAOImpl dao;

    // Bad: using implementation over abstraction (ArrayList)
    void insert(ArrayList<Object> model) {
      validator.validate(model);
      dao.insert(model);
    }
  }

  // Low level module
  class ValidatorImpl implements IValidator {

    // Bad: using implementation over abstraction (ArrayList)
    @Override
    void validate(ArrayList<Object> model) {...}
  }

  // Low level module
  class DAOImpl implements IDAO {

    // Bad: using implementation over abstraction (LinkedList)
    @Override
    void insert(LinkedList<Object> model) {...}
  }
  ```

  - Good:
  ```java
  // High level module
  class ServiceImpl implements IService {

    // Good: using abstraction over implementation (IValidator)
    private IValidator validator;
    // Good: using abstraction over implementation (IDAO)
    private IDAO dao;

    // Good: using abstraction over implementation (Collection / List)
    void insert(Collection<Object> model) {
      validator.validate(model);
      dao.insert(model);
    }
  }

  // Low level module
  class ValidatorImpl implements IValidator {

    // Good: using abstraction over implementation (Collection / List)
    @Override
    void validate(Collection<Object> model) {...}
  }

  // Low level module
  class DAOImpl implements IDAO {

    // Good: using abstraction over implementation (Collection / List)
    @Override
    void insert(Collection<Object> model) {...}
  }
  ```

# References and useful resources

General:
- [Big O Cheatsheet](http://bigocheatsheet.com/)
- [An "Awesome" list of code review resources - articles, papers, tools, etc](https://github.com/joho/awesome-code-review)

Java:
- [Big O Summary for Java collections framework implementations](https://stlackoverflow.com/questions/559839/big-o-summary-for-java-collections-framework-implementations)

Tools:
- https://pmd.github.io/
- https://getcodeflow.com/
- https://eslint.org/
- https://prettier.io/
- https://www.sonarqube.org/
- https://www.sonarlint.org/
- https://www.reviewboard.org/

Intellij plugins:
- [Cyclomatic Complexity](https://plugins.jetbrains.com/plugin/93-metricsreloaded)

VSCode plugins:
- [Cyclomatic Complexity](https://marketplace.visualstudio.com/items?itemName=kisstkondoros.vscode-codemetrics)

References:
1. [Publication about code review made by a Microsoft researcher](https://www.sback.it/publications/icse2013.pdf)
2. ["What to Look for in a Code Review", short book written by a JetBrains employee](https://leanpub.com/s/MMUrwWmotCIr412BkGxt9w.pdf)
3. [dev.to: Code review checklist example](https://dev.to/bosepchuk/a-code-review-checklist-prevents-stupid-mistakes-o6)
4. [dev.to: Code review checklist example](https://dev.to/designpuddle/code-review-checklist-14ke)
5. [Atalassian blog post](https://www.atlassian.com/agile/software-development/code-reviews)
6. [Jeff Atwood blog post](https://blog.codinghorror.com/code-reviews-just-do-it)
7. [dev.to: Cyclomatic Complexity](https://dev.to/designpuddle/coding-concepts---cyclomatic-complexity-3blk)
8. [Uncle Bob: The Principles of OOD](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)
9. [What is the dependency inversion principle and why is it important?](https://stackoverflow.com/questions/62539/what-is-the-dependency-inversion-principle-and-why-is-it-important)