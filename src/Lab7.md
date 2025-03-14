---
title: Лабораторная работа 7. Переменные и управляющие конструкции
---

[Таблица стран](assets/lab3/Страны.xlsx)

[Таблица учеников](assets/lab5/Students.xlsx)

[Таблицы студентов](assets/lab6/Students.xlsx)

[К списку лабораторных >>>](../README.md)

---

### Часть 1. Задания не по таблицам

1. Даны числа A и B. Найти и вывести их произведение. 
2. Даны два целых числа D (день) и M (месяц), определяющие правильную
дату невисокосного года. Вывести значения D и M для даты, следующей за указанной
3. Дано число. Вывести сумму квадратов всех нечетных чисел до него включительно.
4. Дано четырехзначное число. Вывести сумму его цифр.
5. Даны случайные целые числа a, b и c. Найти наименьшее из них.
6. Дано случайное целое число a. Проверить, делится ли данное число на 11.
7. Дано случайное целое число N (N < 1000). Если оно является степенью числа 3, то
вывести «Да», если не является – вывести «Нет».
8. Даны случайные целые числа a и b. Найти наименьший общий кратный (НОК).
9. Даны два целых числа A и B (A<B). Найти сумму квадратов всех целых чисел от A
до B включительно.
10. Найти первое натуральное число, которое при делении на 2, 3, 4, 5, и 6 дает остаток
1, но делится на 7.
11. Вывести свою фамилию на экран столько раз, сколько в нем букв.
12. Выведите на экран информацию о сервере, о базе данных, о текущем пользователе и о текущем времени.
13. Напишите код для вывода на экран с помощью цикла:

Н

иНи

жиНиж

нжиНижн

енжиНижне

венжиНижнев

авенжиНижнева

равенжиНижневар

травенжиНижневарт

отравенжиНижневарто

вотравенжиНижневартов

свотравенжиНижневартовс

ксвотравенжиНижневартовск

---

### Часть 2. Задание по таблице учеников

1. Используя переменные, в таблице «Ученики» найти разницу между наибольшими и наименьшими баллами
2. Используя переменные, в таблице «Ученики» найти разницу между средними баллами лицеистов и гимназистов.
3. Используя переменные, в таблице «Ученики» проверить на четность количество строк.
4. Используя переменные, вывести список учеников, средний балл которых попадает в заданный интервал. 

---

### Часть 3. Задание по таблице стран

Переписать следующие задания, используя переменные:

1. Вывести список стран мира, которые находятся в тех частях света, средняя плотность населения которых превышает общемировую.
2. Вывести список стран и процентное соотношение площади каждой из них к общей площади всех стран мира.
3. Вывести список стран мира, плотность населения которых больше, чем средняя плотность населения всех стран мира.

---

### Часть 4. Задание по таблице студентов

Переписать следующие задания, используя переменные:

1. Вывести из таблиц «Кафедра», «Специальность» и «Студент» данные о студентах,
   которые обучаются на данном факультете (например, «фит»)(1).
3. Вывести список студентов, не сдавших ни одного экзамена в указанной дате(13).
4. Вывести название дисциплины, фамилию, должность и степень преподавателя,
   дату и место проведения экзаменов в хронологическом порядке в заданном интервале даты(10).
5. Вывести из таблиц «Студент» и «Экзамен» учетные номера и фамилии студентов,
   а также количество сданных экзаменов и средний балл для каждого студента только для тех студентов,
   у которых средний балл не меньше заданного (например, 4)(7).
7. Вывести список студентов, сдавших экзамены в заданной аудитории(6).
   
---

### Часть 5. Задание на TRY/CATCH

Написать код, добавляющий в любую свою таблицу данные, которые не соответствуют ограничениям столбцов. 
Обработать возможные ошибки.

---

### [Локальные переменные](https://metanit.com/sql/sqlserver/9.1.php)

Несмотря на то, что T-SQL – декларативный язык, у него есть расширение, 
позволяющее обрабатывать ошибки, создавать и выполнять хранимые процедуры и пользовательские функции, 
триггеры и сценарии с использованием локальных переменных, операторов присваивания, ветвлений и циклов.

Объявление переменной осуществляется с помощью оператора DECLARE. 

Упрощенный синтаксис команды имеет следующий вид:
```sql
DECLARE <@название> AS <тип>
```

Имена переменных в Transact-SQL начинаются с символа @.

Объявить сразу несколько переменных одним оператором DECLARE можно так:
```sql
DECLARE <@название1> AS <тип1>, …, <@названиеN> AS <типN>
```

Ключевое слово AS необязательно.

При объявлении переменной можно ее инициализировать:
```sql
DECLARE <@название> AS <тип> = <значение>
```

Объявленным переменным можно присвоить различные значения с помощью оператора присваивания SET. 

Переменным должны присваиваться значения того типа данных, с каким они были объявлены. 

Упрощенный синтаксис команды имеет следующий вид:
```sql
SET <@название> = <значение>
```

Переменным можно присваивать скалярный результат выполнения запросов:
```sql
SET <@название> = (SELECT <значение> FROM <таблица>)
```

Неинициализированные переменные имеют значение NULL, их нельзя использовать в выражениях.

Переменным можно присваивать значения с помощью команды SELECT:
```sql
SELECT <@переменная1> = <столбец1>, …, <@переменнаяN> = <столбецN>
FROM <таблица>
```
  
Значения переменных можно вывести с помощью команды PRINT.
Синтаксис команды имеет следующий вид:
```sql
PRINT <сообщение>
```

Сообщение может быть символьной константой, переменной символьного типа, переменной, 
неявно преобразуемой в последовательность символов, или выражения, возвращающего символьный результат.

Значения переменных можно вывести с помощью команды SELECT.
Синтаксис команды имеет следующий вид:
```sql
SELECT <@переменная1> [AS псевдоним1], …, <@переменнаяN> [AS псевдонимN]
```
---

### Глобальные переменные

Глобальные переменные используются сервером для отслеживания информации уровня сервера или базы данных,
относящейся к конкретному сеансу.

Для глобальных переменных невозможно явное присваивание или объявление.

Некоторые глобальные переменные:
* @@ERROR - Код ошибки для последней команды SQL
* @@FETCH_STATUS - Статус предыдущей команды выборки для курсора
* @@IDENTITY - Последнее значение счетчика, используемое в операции вставки
* @@NESTLEVEL - Количество уровней вложенности для хранимой процедуры или триггера
* @@ROWCOUNT - Количество записей, обработанных предыдущей командой
* @@SERVERNAME - Имя локального сервера
* @@SPID - Идентификатор текущего процесса
* @@TRANCOUNT - Уровень вложенности транзакции
* @@VERSION - Номер версии SQL Server, дата и тип процессора

Пример вывода значения локальной или глобальной переменной:

```sql
SELECT @@VERSION
```

```sql
SELECT @MyDate
```

---

### [Условные выражения](https://metanit.com/sql/sqlserver/9.3.php)

Для выполнения команды в зависимости от условия используется управляющая команда IF ... ELSE … . 
Инструкция, следующая за ключевым словом IF и его условием, выполняется только в том случае, 
если логическое выражение возвращает TRUE. 
Необязательное ключевое слово ELSE представляет другую инструкцию, которая выполняется, 
если условие IF не удовлетворяется и логическое выражение возвращает FALSE. 

Упрощенный синтаксис команды имеет следующий вид:
```sql
IF <условие>
[BEGIN]
<команды>
[END]
[ ELSE
[BEGIN]
<команды>
[END]
]
```

Например, таблица Orders представляет заказы, а столбец CreatedAt - дату заказов. Узнаем, были ли заказы за последние 10 дней:

```sql
DECLARE @lastDate DATE
 
SELECT @lastDate = MAX(CreatedAt) FROM Orders
 
IF DATEDIFF(day, @lastDate, GETDATE()) > 10
    PRINT 'За последние десять дней не было заказов'
```

Условие должно возвращать только TRUE (ИСТИНА) или FALSE (ЛОЖЬ).

Если в блоке более чем одна команда, использование [BEGIN] … [END] обязательно.

```sql
DECLARE @lastDate DATE, @count INT, @sum MONEY
 
SELECT @lastDate = MAX(CreatedAt), 
        @count = SUM(ProductCount) ,
        @sum = SUM(ProductCount * Price)
FROM Orders
 
IF @count > 0
    BEGIN
        PRINT 'Дата последнего заказа: ' + CONVERT(NVARCHAR, @lastDate) 
        PRINT 'Продано ' + CONVERT(NVARCHAR, @count) + ' единиц(ы)'
        PRINT 'На общую сумму ' + CONVERT(NVARCHAR, @sum)
    END;
ELSE
    PRINT 'Заказы в базе данных отсутствуют'
```

---

### [Циклы](https://metanit.com/sql/sqlserver/9.4.php)

Для выполнения повторяющихся операций применяется цикл WHILE.
Упрощенный синтаксис команды имеет следующий вид:

```sql
WHILE <условие>
[BEGIN]
<команды| BREAK | CONTINUE >
[END]
```

Команда BREAK приводит к выходу из цикла и вызывает инструкции, 
следующие за ключевым словом END, обозначающим конец цикла.

Команда CONTINUE пропускает все команды после себя до конца цикла и переводит
цикл на следующий шаг.

Например, вычислим факториал числа:

```sql
DECLARE @number INT, @factorial INT
SET @factorial = 1;
SET @number = 5;
 
WHILE @number > 0
    BEGIN
        SET @factorial = @factorial * @number
        SET @number = @number - 1
    END;
 
PRINT @factorial
```

---

### [TRY/CATCH и Обработка ошибок](https://metanit.com/sql/sqlserver/9.5.php)

Для обработки ошибок в T-SQL применяется конструкция TRY...CATCH. 

Она имеет следующий формальный синтаксис:
```sql
BEGIN TRY
    инструкции
END TRY
BEGIN CATCH
    инструкции
END CATCH
```

Между выражениями BEGIN TRY и END TRY помещаются инструкции, 
которые потенциально могут вызвать ошибку, например, какой-нибудь запрос. 
И если в этом блоке TRY возникнет ошибка, то управление передается в блок CATCH, где можно обработать ошибку.

В блоке CATCH для обаботки ошибки мы можем использовать ряд функций:

* ERROR_NUMBER(): возвращает номер ошибки
* ERROR_MESSAGE(): возвращает сообщение об ошибке
* ERROR_SEVERITY(): возвращает степень серьезности ошибки. 
  Степень серьезности представляет числовое значение.
  И если оно равно 10 и меньше, то такая ошибка рассматривается как предупреждение и не обрабатывается конструкцией TRY...CATCH.
  Если же это значение равно 20 и выше, то такая ошибка приводит к закрытию подключения к базе данных, если она не обрабатывается конструкцией TRY...CATCH.
* ERROR_STATE(): возвращает состояние ошибки

Например, добавим в таблицу данные, которые не соответствуют ограничениям столбцов:
```sql
CREATE TABLE Accounts (FirstName NVARCHAR NOT NULL, Age INT NOT NULL)
 
BEGIN TRY
    INSERT INTO Accounts VALUES(NULL, NULL)
    PRINT 'Данные успешно добавлены!'
END TRY
BEGIN CATCH
    PRINT 'Error ' + CONVERT(VARCHAR, ERROR_NUMBER()) + ':' + ERROR_MESSAGE()
END CATCH
```
В данном случае для столбцов таблицы вставляются недопустимые данные - значения NULL, поэтому обработка программы перейдет к блоку CATCH

---

### [GO](https://metanit.com/sql/sqlserver/3.7.php)

GO \[count\]  - команда
Count - целое положительное число. Пакет, предшествующий команде GO, будет выполняться заданное количество раз.

GO — это не инструкция Transact-SQL; это команда, распознаваемая программами sqlcmd и osql, а также редактором кода среды SQL Server Management Studio.

Программы SQL Server интерпретируют команду GO как сигнал о том, 
что им следует отправить текущий пакет инструкций Transact-SQL экземпляру SQL Server. 
Текущий пакет инструкций состоит из всех инструкций, введенных за время, 
прошедшее с момента обработки последней команды GO, или, если данная команда GO является первой,
с момента начала нерегламентированного сеанса или скрипта.

Инструкция Transact-SQL не может располагаться на той же строке, что и команда GO. 
Тем не менее строка с командой GO может содержать комментарии.

При использовании команды GO нужно соблюдать требования, предъявляемые к пакетам.
Например, при любом вызове хранимой процедуры после первой инструкции пакета нужно использовать ключевое слово EXECUTE. 
Область видимости локальных (пользовательских) переменных ограничена пакетом, и к ним нельзя обращаться после команды GO.

```sql
USE Ag2022_learningsql;  
GO  
DECLARE @MyMsg VARCHAR(50)  
SELECT @MyMsg = 'Hello, World.'  
GO -- @MyMsg недействительна после того, как этот GO завершит пакет.  
  
-- Ниже обращение к @MyMsg выдаст ошибку, потому что @MyMsg не объявлен в этом пакете. (Yields an error)  
PRINT @MyMsg  
GO  
  
SELECT @@VERSION;  
-- Выдаст ошибку: должен быть EXEC sp_who, если не первый оператор в пакете sp_who  
GO  
```

Приложения SQL Server могут отправлять экземпляру Transact-SQL множественные инструкции SQL Server, 
чтобы они были выполнены как пакет. Инструкции пакета компилируются в единый план выполнения. 
Программисты, выполняющие в программах SQL Server нерегламентированные инструкции 
или составляющие из инструкций Transact-SQL скрипты для программ SQL Server, 
используют команду GO как сигнал об окончании пакета.

Приложения, основанные на API-интерфейсах ODBC или OLE DB, 
при попытке выполнить команду GO получают уведомление о синтаксической ошибке. 
Программы SQL Server никогда не отправляют команду GO серверу.

Не используйте точку с запятой в качестве признака конца инструкции после команды GO.

В следующем примере создаются два пакета. Первый содержит только инструкцию USE Ag2022_learningsql, которая задаёт контекст базы данных. 
Остальные инструкции выполняют те или иные операции над локальной переменной и должны быть сгруппированы в один пакет.
Поэтому следующая команда GO указывается только после последней инструкции, в которой используется переменная.

```sql
USE Ag2022_learningsql;  
GO  
DECLARE @NmbrPeople INT  
SELECT @NmbrPeople = COUNT(*)  
FROM Person.Person;  
PRINT 'The number of people as of ' +  
      CAST(GETDATE() AS CHAR(20)) + ' is ' +  
      CAST(@NmbrPeople AS CHAR(10));  
GO  
```

В следующем примере инструкции в пакете выполняются дважды.
```sql
SELECT DB_NAME();  
SELECT USER_NAME();  
GO 2  
```

---
