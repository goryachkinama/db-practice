---
title: Лабораторная работа 1
---

## Создание и заполнение таблиц с помощью скриптов

![learn_sql.png](assets/lab0/learn_sql.png)

[все лабораторные](https://github.com/goryachkinama/db-practice/README.md)
---

### Создание новой БД с помощью запроса

Новую БД можно создать, используя стандартные команды языка T-SQL.

Все команды языка T-SQL набираются на вкладке нового запроса (SQLQuery).

Для создания запроса на панели инструментов необходимо нажать кнопку ![new_query.png](assets/lab0/new_query.png)
либо выбрать соответствующую команду в контекстном меню баз данных

![new_query_cntx.png](assets/lab0/new_query_cntx.png)

Для выполнения команд языка T-SQL на панели инструментов необходимо нажать кнопку ![go.png](assets/lab0/go.png)
или на вкладке нового запроса набрать команду GO.

Для создания нового файла данных используется команда CREATE DATABASE, которая имеет следующий синтаксис:

```sql
CREATE DATABASE usersdb
```

 Если мы хотим задать вышеперечисленные настройки, то в скрипт добавится их перечень с необходимыми значениями:

```sql
CREATE DATABASE [Имя БД] ON PRIMARY
(
NAME = <Логическое имя>,
FILENAME = <Имя файла>,
SIZE = <Нач.размер>,
MAXSIZE = <Макс.размер>,
FILEGROWTH = <Шаг> )
LOG ON
9(
NAME = <Логическое имя>,
FILENAME = <Имя файла>,
SIZE = <Нач.размер>,
MAXSIZE = <Макс.размер>,
FILEGROWTH = <Шаг> )
)
```
---

### Прикрепление базы данных из файла

Возможна ситуация, что у нас уже есть файл базы данных с расширением mdf. И этот файл мы можем переносить сюда с указанием параметра FILENAME.

```sql
CREATE DATABASE название_базы_данных
ON PRIMARY(FILENAME='путь_к_файлу_mdf_на_локальном_компьютере')
FOR ATTACH;
```

Например, вот таким запросом:

```sql
CREATE DATABASE contactsdb
  ON PRIMARY(FILENAME='C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\userstoredb.mdf')
  FOR ATTACH;
```
---

### Пример создания базы из файла с указанием других параметров:

```sql
  CREATE DATABASE [publishing] ON PRIMARY
  (
  NAME = 'publishing',
  FILENAME = 'D:\publishing.mdf' ,
  SIZE = 5120KB ,
  MAXSIZE = UNLIMITED,
  FILEGROWTH = 1024KB )
  LOG ON
  (
  NAME = 'publishing_log',
  FILENAME = 'D:\publishing_log.ldf' ,
  SIZE = 1024KB ,
  MAXSIZE = 2048GB ,
  FILEGROWTH = 10%
  )
  GO
```

---

### Удаление базы данных

Для удаления базы данных применяется команда DROP DATABASE, которая имеет следующий синтаксис:

```sql
DROP DATABASE database_name1 [, database_name2]...
```

После команды через запятую мы можем перечислить все удаляемые базы данных. Например, удаление базы данных contactsdb:

```sql
DROP DATABASE contactsdb
```

Стоит учитывать, что даже если удаляемая база данных была прикреплена, то все равно будут удалены все файлы базы данных.

---

### [Создание таблиц c помощью запроса](https://metanit.com/sql/sqlserver/3.2.php)

Для создания таблиц в SQL Server в первую очередь необходимо сделать активной ту БД, в которой создается таблица.
Для этого можно в новом запросе можно набрать команду: USE <Имя БД>,
либо на панели инструментов необходимо выбрать в выпадающем списке рабочую БД.
После выбора БД можно создавать таблицы.

Таблицы создаются командой

```sql
CREATE TABLE <Имя таблицы>
(
<Имя поля1> <Тип1> IDENTITY(NULL|NOTNULL),
<Имя поля1> <Тип1> NOT NULL,
)
```

Здесь:

* <Имя таблицы> – имя создаваемой таблицы;

* <Имя поля> – имена столбцов таблицы;

* <Тип> – типы полей;

* <IDENTITY NULL|NOT NULL> – поле счётчик.

* Пример создания таблицы «employees» с помощью запроса:

```sql
CREATE TABLE employees(
E_ID int IDENTITY(1,1) NOT NULL,
E_SURNAME NVARCHAR(20) NOT NULL,
E_NAME NVARCHAR (20) NOT NULL,
E_FNAME NVARCHAR (30) NULL,
E_BIRTHDAY DATE NULL,
E_SEX BIT NULL,
E_POST INT NULL,
E_ROOM INT NULL,
E_PHONE NVARCHAR (10) NULL,
E_INN NVARCHAR (12) NOT NULL,
E_PASSP_NUMBER NVARCHAR (12) NOT NULL,
E_PASSP_ORG NVARCHAR (50) NOT NULL,
E_PASSP_DATE DATE NOT NULL,
E_ADDRESS NVARCHAR (80) NULL,
CONSTRAINT PK_employees PRIMARY KEY E_ID
)
```
---

### [Удаление и переименование таблиц](https://metanit.com/sql/sqlserver/3.2.php)

---

### Связывание таблиц с помощью конструкции [FOREIGN KEY](https://metanit.com/sql/sqlserver/3.5.php)

Для указания внешнего ключа таблицы нужно добавить конструкцию FOREIGN KEY в конструкцию CREATE TABLE :

```sql
  FOREIGN KEY <имя внешнего ключа> REFERENCES <таблица первичного ключа>
  ON UPDATE <операция обновления>
  ON DELETE <операция удаления>
```
 
Имена столбцов внешних ключей указываются после ключевых слов FOREIGN KEY. 
В конструкции REFERENCES содержится имя таблицы того первичного ключа, на который производится ссылка.
Если столбцы первичного ключа заданы в конструкции PRIMARY KEY своей таблицы, нет необходимости перечислять их имена. 
Если же имена столбцов не являются частью конструкции PRIMARY KEY, 
необходимо перечислить столбцы первичного ключа в конструкции REFERENCES .

Тогда создание таблицы Employees с помощью запроса будет выглядеть следующим образом:

```sql
CREATE TABLE Employees
(
E_ID INT IDENTITY (1, 1) NOT NULL,
E_SURNAME NVARCHAR(20) NOT NULL,
E_NAME NVARCHAR (20) NOT NULL,
E_FNAME NVARCHAR (30) NULL,
E_BIRTHDAY DATE NULL,
E_SEX BIT NULL,
E_POST INT NULL,
E_ROOM INT NULL,
E_PHONE NVARCHAR(10) NULL,
E_INN NVARCHAR(12) NOT NULL,
E_PASSP_NUMBER NVARCHAR (12) NOT NULL,
E_PASSP_ORG NVARCHAR (50) NOT NULL,
E_PASSP_DATE DATE NOT NULL,
E_ADDRESS NVARCHAR (80) NULL,
PRIMARY KEY E_ID,
FOREIGN KEY (E_POST) REFERENCES Posts(P_ID)
ON UPDATE NO ACTION
ON DELETE SET NULL
);
```

Если таблица уже создана для добавления в нее внешнего ключа необходимо воспользоваться конструкцией ALTER TABLE.
Добавление внешнего ключа с помощью конструкции ALTER TABLE будет выглядеть следующим образом:

```sql
ALTER TABLE Employees
ADD FOREIGN KEY (E_POST)
REFERENCES Posts (P_ID)
```

Аналогично для всех остальных таблиц необходимо проделать те же самые действия.

---

### [Другие атрибуты и ограничения для столбцов](https://metanit.com/sql/sqlserver/3.4.php)

---

### [Редактирование таблиц и столбцов](https://metanit.com/sql/sqlserver/3.6.php)

---

### Добавление данных в таблицу

В SQL Server 20XX заполнение таблиц производится при помощи следующей команды:

[INSERT](https://metanit.com/sql/sqlserver/4.1.php)

```sql
INSERT <Имя таблицы> (<Список полей>)
VALUES (<Значения полей>)
```

* <Имя таблицы> – таблица, куда вводим данные;

* <Список полей> – список полей, куда вводим данные, если не указываем, то подразумевается заполнение всех полей, 
  в списке полей поля указываются через запятую;

* <Значения полей> – значение полей через запятую

Пример:

```sql
INSERT INTO Posts (P_POST, P_SAL)
VALUES ('Менеджер', 15000)
Или для добавления множества записей можно использовать конструкция
из следующего примера:
INSERT INTO Posts VALUES
('Менеджер', 15000),
('Редактор', 20000),
('Программист', 50000),
('Главный редактор', 150000)
```

В качестве значений можно указать константу DEFAULT, то есть будет поставлено значение по умолчанию, 
либо можно подставить оператор SELECT.

Также возможно добавить множество записей из текстового файла.

Для этого создайте текстовый файл с именем (например: title.txt), содержащий по одной записи в каждой строке 
(значения столбцов должны быть разделены символами табуляции (клавиша TAB) и даны в том порядке, 
который был определен командой CREATE TABLE).

Незаполненным полям можно присвоить значение NULL. В текстовом файле это значение представляется символами \N. 

Загрузить файл title.txt в таблицу можно с помощью следующей команды:

```sql
LOAD DATA LOCAL INFILE "title.txt" INTO TABLE title;
```
---

### Удаление отдельных столбцов и отдельных строк из таблицы

Из таблицы можно удалить все столбцы, либо отдельные записи. Это осуществляется командой

[DELETE](https://metanit.com/sql/sqlserver/4.8.php)

```sql
DELETE <Имя таблицы>
WHERE <Условие>
```

Либо для удаления таблицы полностью:

```sql
DROP TABLE <Имя таблицы>
```

Для полной очистки таблицы и сброса счетчика AUTOINCREMENT используется конструкция

```sql
TRUNCATE TABLE <Имя таблицы>
```

Пример использования этой конструкции представлен ниже:

```sql
TRUNCATE TABLE Posts;
```

Если условие указано, то удаляются записи поля, которые соответствуют условию.

Пример: Удалить записи из таблицы Posts, у которых значение поля P_SAL > 100000 .

```sql
DELETE Posts
WHERE P_SAL > 100000
```

Пример: Удалить записи из таблицы Posts, у которых значение поля P_Post будет содержать часть слова «джер»

```sql
DELETE FROM Posts WHERE P_Post LIKE ‘джер’;
```

Пример: Удалить записи из таблицы Posts, у которых значение поля P_ID будет между 5 и 8.

```sql
DELETE FROM Posts WHERE P_ID BETWEEN 5 AND 8;
```
---

### Изменение данных в таблице

Для этого используется следующая команда:

[UPDATE](https://metanit.com/sql/sqlserver/4.7.php)

```sql
UPDATE <Имя таблицы>
SET
<Имя поля1> = <Выражение1>,
[<Имя поля2> = <Выражение2>,]
[WHERE <Условие>]
```

* <Имя поля1>, <Имя поля2> - имена изменяемых полей,

* <Выражение1>, <Выражение2> - либо конкретные значения, либо NULL , либо операторы SELECT.
  Здесь SELECT применяется как функция.

* <Условие> – условие, которым должны соответствовать записи, поля которых изменяем.

Пример: В таблице posts переименовать «Менеджера» в «Редактора» название

```sql
UPDATE Pposts
SET P_POST = 'Редактор'
WHERE P_POST = 'Менеджер'
```
---

[Следующие задания >>>](Lab1_design.md)

[К списку лабораторных >>>](../README.md)