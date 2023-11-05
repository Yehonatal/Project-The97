# Принцип единственности ответственности
(В оригинале - The Single Responsibility Principle)

Один из основных принципов хорошего дизайна – это:

> Помещай вместе то, что изменяется по одной и той же причине, и разделяй то, что изменяется по разным причинам.

Этот принцип часто называют «принципом единственности ответственности». Говоря коротко, принцип декларирует, что подсистема, модуль, класс или даже функция не должны иметь более одной причины для изменения своего кода. Классический пример – класс для работы с бизнес-правилами, отчетами и базой данных:

```
public class Employee {
  public Money calculatePay() ...
  public String reportHours() ...
  public void save() ...
}
```

Некоторые программисты могут считать, что помещение этих трех функций в общий класс – отличная практика. В конце концов, класс и является коллекцией функций, обрабатывающих общие данные. Однако, проблема здесь в том, что изменения в эти три функции нужно будет вносить по полностью различным причинам. Функция calculatePay будет меняться, если изменится бизнес-правило начисления зарплаты. Функция reportHours будет меняться, если кому-то понадобится другой формат отчета. А функция save изменится, если изменится схема БД. И эти три причины вместе делают класс Employee сильно изменчивым. Он будет меняться по любой из них. Более того, эти изменения коснутся и всех классов, зависящих от класса Employee.

Хороший дизайн системы означает, что мы разделяем систему на компоненты, каждый из которых может быть выпущен независимо. Независимый выпуск означает, что если мы изменим только один компонент, то нам не придется перевыпускать все остальные. Однако, если в данном примере класс Employee активно используется другими классами, то его изменение повлечет за собой необходимость перевыпуска и остальных компонентов тоже. Это разрушает все преимущества компонентного дизайна (еще один используемый термин – сервис-ориентированная архитектура, SOA).

```
public class Employee {
  public Money calculatePay() ...
}

public class EmployeeReporter {
  public String reportHours(Employee e) ...
}

public class EmployeeRepository {
  public void save(Employee e) ...
}
```

Приведенное выше разбиение решает эту проблему. Каждый из классов может быть помещен в отдельный компонент. Точнее, все классы, работающие с отчетом, помещаются в компонент отчета, все классы, работающие с БД – в компонент БД, а все, что касается бизнес-правил – в компонент бизнес-правил.

Внимательный читатель может заметить, что в приведенном выше примере все еще есть зависимости. Так, класс Employee все еще зависит от остальных классов. Т.е. если изменится именно класс Employee, то скорее всего, остальные классы нужно будет перекомпилировать и перевыпустить тоже. Таким образом, класс Employee по-прежнему не может быть изменен и выпущен независимо. Однако, остальные классы могут меняться и перевыпускаться независимо. Изменение одного из них не повлечет перекомпиляции и перевыпуска остальных. И даже Employee можно сделать независимо выпускаемым, аккуратно используя принцип инверсии зависимости, однако это уже совсем другая тема.

Аккуратное применение принципа единственности ответственности, разделяя вещи, меняющиеся по разным причинам – ключевой фактор для создания дизайна, в основе которого лежит структура отдельно выпускаемых компонент.

Автор оригинала - [Uncle Bob](http://programmer.97things.oreilly.com/wiki/index.php/Uncle_Bob)