# code-review-checklist

TODO:
- Examples for each topic.

What should you look for:
- Magic Numbers and Hardcoded Values
```
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
```
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
  - Single Responsibility Principle
  ```

  ```
  - Open-Closed Principle
  ```

  ```
  - Liskov Substitution Principle
  ```

  ```
  - Interface Segregation Principle
  ```

  ```
  - Dependency Inversion Principle
  ```

  ```

General:
- http://bigocheatsheet.com/
- https://github.com/joho/awesome-code-review

Java:
- https://stlackoverflow.com/questions/559839/big-o-summary-for-java-collections-framework-implementations

Tools:
- https://pmd.github.io/
- https://getcodeflow.com/
- https://eslint.org/
- https://prettier.io/
- https://www.sonarqube.org/
- https://www.sonarlint.org/
- https://www.reviewboard.org/

Intellij plugins:
- Cyclomatic Complexity: https://plugins.jetbrains.com/plugin/93-metricsreloaded

VSCode plugins:
- Cyclomatic Complexity: https://marketplace.visualstudio.com/items?itemName=kisstkondoros.vscode-codemetrics

References:
1. Publication about code review made by a Microsoft researcher: https://www.sback.it/publications/icse2013.pdf
2. "What to Look for in a Code Review", short book written by a JetBrains employee: https://leanpub.com/s/MMUrwWmotCIr412BkGxt9w.pdf
3. Code review checklist example: https://dev.to/bosepchuk/a-code-review-checklist-prevents-stupid-mistakes-o6
4. Code review checklist example: https://dev.to/designpuddle/code-review-checklist-14ke
5. Atalassian blog post: https://www.atlassian.com/agile/software-development/code-reviews
6. Jeff Atwood blog post: https://blog.codinghorror.com/code-reviews-just-do-it
7. Cyclomatic Complexity: https://dev.to/designpuddle/coding-concepts---cyclomatic-complexity-3blk
