# code-review-checklist

TODO:
- Examples for each topic.

What should you look for:

- Magic Numbers and Hardcoded Values

  - Bad:
  ```java
  for (i = 0; i < 77; i++) {}
  ```

  - Good:
  ```java
  for (i = 0; i < 77; i++) {}
  ```

- Defensive Code
```

```
- Functions / Methods / Services / Variable Names
```

```
- Return types (look for Dependency Inversion Principle)
```

```
- Functions / Methods / Services / Variable Access
```

```
- Remove Unused/Commented Code
```

```
- Format Code

  - Bad:
  ```java
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
  public static String iAmFormatted(int a, String b) {
    return b + a;
  }
  ```

- Code coverage
```

```
- Cyclomatic Complexity
```

```
- DRY (Don't Repeat Yourself)
```
int sum(int a, int b) {
  return a + b;
}

Integer sumToo(int a, int b) {
  Integer.sum(a, b);
}
```
- Data structures and Big-O complexities
```

```
- *Adherence to our architectural pattern (Resource, Service, etc.)
```

```
- SOLID

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

  - Open-Closed Principle: You should be able to extend a classes behavior, without modifying it.

    - Bad:
    ```java
    private Object entity;
    private EventInterceptor ei;

    public void handleEvent(Event event) {
      if (event instanceof PreLoad) {
        ei.preLoad(entity);
      } else if (event instanceof PostLoad) {
        ei.preLoad(entity);
      } else if (event instanceof PrePersist) {
        ei.preLoad(entity);
      } else if (event instanceof PreSave) {
        ei.preLoad(entity);
      }
    }
    ```

    - Good:
    ```java

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

    class RectangleTest {
      void assertRectangle() {
        Rectangle rectangle = new Square(10);
        rectangle.setWidth(4);
        rectangle.setHeight(5);

        // rectangle.getArea() will return 25 because Square class is overriding setHeight to set both width and height
        assertEquals(20, rectangle.getArea());
      }
    }
    ```

    - Good:
    ```java

    ```

  - Interface Segregation Principle: Make fine grained interfaces that are client specific.

    - Bad:
    ```java

    ```

    - Good:
    ```java

    ```

  - Dependency Inversion Principle: Depend on abstractions, not on concretions.

    - Bad:
    ```java

    ```

    - Good:
    ```java

    ```

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