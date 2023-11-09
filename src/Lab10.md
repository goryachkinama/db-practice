---
title: Лабораторная работа 10. Индексы и ограничения
---

[Таблица стран](assets/lab3/Страны.xlsx)

[Таблица учеников](assets/lab5/Students.xlsx)

[К списку лабораторных >>>](../README.md)

---

### Задания не по таблицам

---

### Ограничения

```sql
ALTER TABLE имя_таблицы ADD перечень_полей_с_характеристиками 
```
– позволяет добавить новые поля в таблицу;

```sql
ALTER TABLE имя_таблицы DROP COLUMN перечень_полей 
```
– позволяет удалить поля из таблицы;

```sql
ALTER TABLE имя_таблицы ADD CONSTRAINT имя_ограничения 
FOREIGN KEY(поля) REFERENCES таблица_справочник(поля) 
```
– позволяет определить связь между таблицей и таблицей справочником

Добавление свойства IDENTITY к полю 
– позволяет сделать это поле автоматически заполняемым (полем-счетчиком) для таблицы;

---

### Прочие ограничения (UNIQUE, DEFAULT, CHECK)

При помощи ограничения UNIQUE можно сказать, что значения для каждой строки в данном поле или в наборе полей должно быть уникальным.

```sql
CREATE TABLE Production.TransactionHistoryArchive4  
 (  
   TransactionID int NOT NULL,   
   CONSTRAINT AK_TransactionID UNIQUE(TransactionID)   
)

ALTER TABLE Employees ADD CONSTRAINT UQ_Employees_Email UNIQUE(Email)

ALTER TABLE имя_таблицы ADD CONSTRAINT имя_ограничения UNIQUE(поле1,поле2,…)
```

При помощи добавления к полю ограничения DEFAULT мы можем задать значение по умолчанию, 
которое будет подставляться в случае, если при вставке новой записи данное поле не будет перечислено в списке полей команды INSERT.

```sql
ALTER TABLE Employees ADD HireDate date NOT NULL DEFAULT SYSDATETIME()

ALTER TABLE Employees ADD DEFAULT SYSDATETIME() FOR HireDate

ALTER TABLE Employees ADD CONSTRAINT DF_Employees_HireDate DEFAULT SYSDATETIME() FOR HireDate
```

Проверочное ограничение CHECK используется в том случае, когда необходимо осуществить проверку вставляемых в поле значений. 
Например, наложим данное ограничение на поле табельный номер, которое у нас является идентификатором сотрудника (ID). 
При помощи данного ограничения скажем, что табельные номера должны иметь значение от 1000 до 1999:

```sql
ALTER TABLE Employees ADD CONSTRAINT CK_Employees_ID CHECK(ID BETWEEN 1000 AND 1999)
```

Можно так же создать ограничения UNIQUE и CHECK без указания имени:

```sql
ALTER TABLE Employees ADD UNIQUE(Email) ALTER TABLE Employees 
ADD CHECK(ID BETWEEN 1000 AND 1999)
```

Все виды ограничений, которые создаются командой вида 
```sql
ALTER TABLE имя_таблицы ADD CONSTRAINT имя_ограничения
```

PRIMARY KEY – первичный ключ;
FOREIGN KEY – настройка связей и контроль ссылочной целостности данных;
UNIQUE – позволяет создать уникальность;
CHECK – позволяет осуществлять корректность введенных данных;
DEFAULT – позволяет задать значение по умолчанию;

Все ограничения можно удалить, используя команду вида
```sql
ALTER TABLE имя_таблицы DROP CONSTRAINT имя_ограничения
```
---

### Индексы

Кластерный (CLUSTERED) и некластерный (NONCLUSTERED) индекс.

Создание самостоятельных индексов.

Под самостоятельностью здесь имеются в виду индексы, которые создаются не для ограничения PRIMARY KEY или UNIQUE.

Индексы по полю или полям можно создавать следующей командой:
```sql
CREATE INDEX IDX_Employees_Name ON Employees(Name)
```

Так же можно указать опции CLUSTERED, NONCLUSTERED, UNIQUE, 
а так же можно указать направление сортировки каждого отдельного поля ASC (по умолчанию) или DESC:

```sql
CREATE UNIQUE NONCLUSTERED INDEX UQ_Employees_EmailDesc ON Employees(Email DESC)
```

При создании некластерного индекса опцию NONCLUSTERED можно отпустить, 
т.к. она подразумевается по умолчанию, здесь она показана просто, 
чтобы указать позицию опции CLUSTERED или NONCLUSTERED в команде.

Удалить индекс можно следующей командой:
```sql
DROP INDEX IDX_Employees_Name ON Employees
```

Простые индексы так же, как и ограничения, можно создать в контексте команды CREATE TABLE.

Создание таблицы со всеми созданными ограничениями и индексами одной командой CREATE TABLE:
```sql
CREATE TABLE Employees(
  ID int NOT NULL,
  Name nvarchar(30),
  Birthday date,
  Email nvarchar(30),
  PositionID int,
  DepartmentID int,
  HireDate date NOT NULL CONSTRAINT DF_Employees_HireDate DEFAULT SYSDATETIME(),
  ManagerID int,
CONSTRAINT PK_Employees PRIMARY KEY (ID), 
CONSTRAINT FK_Employees_DepartmentID FOREIGN KEY(DepartmentID) REFERENCES Departments(ID), 
CONSTRAINT FK_Employees_PositionID FOREIGN KEY(PositionID) REFERENCES Positions(ID), 
CONSTRAINT FK_Employees_ManagerID FOREIGN KEY (ManagerID) REFERENCES Employees(ID), 
CONSTRAINT UQ_Employees_Email UNIQUE(Email), 
CONSTRAINT CK_Employees_ID CHECK(ID BETWEEN 1000 AND 1999), 
INDEX IDX_Employees_Name(Name)
)
```

В итоге:

Индексы могут повысить скорость выборки данных (SELECT), но индексы уменьшают скорость модификации данных таблицы, 
т.к. после каждой модификации системе будет необходимо перестроить все индексы для конкретной таблицы.

---
