---
title: Лабораторная работа 14. C#
---

[Вариант 2](assets/lab/v2.md)

[К списку лабораторных >>>](../README.md)

---

### Задание 1

Продолжить приложение для [Варианта №2](assets/lab/v2.md) либо взять своё из курса по C#

1. Разработать приложение, соединиться с базой (класс DataBase приветствуется)
2. Реализовать выполнение всех запросов в нем (для варианта 2 их уже делали в Lab6, Lab8, Lab9)
   Как отображать запросы в приложении - на ваше усмотрение :)

### Задание 2

1. Реализовать поиск по нескольким критериям (пример: клиента по полям фио, email, телефон) 
2. Реализовать сортировку (товара по стоимости, клиентов по фио и т.п.) 
3. Реализовать фильтрацию (товара по производителю, клиента по тегам и т.п.)

### Задание 3 (со звездочкой)

1. Авторизация (проверка роли)/регистрация пользователя (клиент/покупатель могут входить в личный кабинет)
2. Добавление, изменение и удаление (товара/клиента и т.п.) в окне администратора/менеджера
3. Для случая с товарами реализовать возможность добавления товаров в корзину

---

### [Работа с БД в C#](https://metanit.com/sharp/adonetcore/)

Все функции приведены для WinForms без LINQ, потому что это самый
очевидный и прямолинейный вариант, ради которого обычно много думать не надо.

#### Разделение кода по папкам

Как только вы создали проект, сделайте 3 папки: CLASSES, FORMS и UTILS. 

В первой будут лежать классы по сущностям из БД, во второй все формы, в третьей
строка подключения и, если вы его делаете, класс со всеми запросами.
Чтобы обратиться к классу в папке из другого места, можно написать *название папки*.*название класса* или using *название проекта*.*название папки*.

Примеры:
```cs
private CLASSES.Task selectedTask = null;
using demo4.UTILS;
```

#### Создание класса со строкой подключения

Этот класс делается в папке UTILS. Формально, он состоит из одной публичной
статичной переменной, в которой лежит строка подключения к вашей БД. 
Не забывайте, что названия должны отражать суть.

Как быстро получить строку подключения:

Найти «добавить новый источник данных». Если слева сбоку нет вкладки
«Источники данных», можно найти сверху во вкладке «Проект»
Если в списке не будет необходимой базы данных, нажать «Создать подключение...», 
в появившемся окне ввести имя сервера (на компьютерах колледжа localhost не сработает) и выбрать/ввести имя БД. 
Не забудьте проверить подключение. 
Если проверка не проходит, значит неправильно написано имя сервера.
На окне с выбором появится название вашей БД. Если нажать эту галочку,
появится строчка, которую можно скопировать.
Формально, на этом можно и закончить, но, если захочется, можно пойти дальше и добавить DataSet.

Пример класса:
```cs
namespace demo4.UTILS {
  internal class ConnectionString {
  public static string ConnStr = @"Data Source=localhost;Initial Catalog=demo1;Integrated
  Security=True";
  }
}
```

Если объявить элемент класса как static, это позволяет не создавать экземпляр класса, когда он вам нужен.

@ перед строкой означает, что она будет использоваться именно в таком виде,
игнорируя все специальные символы, применяемые для форматирования (например, /), внутри.

#### Создание классов для БД

По-хорошему, на каждую сущность (таблицу) в БД создается свой класс. 
На практике это может не понадобиться, поэтому, если чувствуете, что вам не хватает
времени, делайте только те классы, которые необходимы.
Класс в программе просто повторяет все поля таблицы. Да, это всё. 
Как обычно, имена должны отражать суть каждого поля. Обратите внимание, что это свойства
класса, а не просто переменные, поэтому они пишутся с заглавной буквы. 
У свойств есть более полная запись, но зачем тратить на это время, если краткая работает
абсолютно также?

Пример:
Таблица (получена через «Выбрать первые 1000 строк»):

Класс:
```sql
SELECT TOP (1000) [ID]
,[Password]
,[FirstName]
,[MiddleName]
,[LastName]
,[Login]
,[IsDeleted]
FROM [demo1].[dbo].[User]
```

```cs
public class User
{
  public int ID { get; set; }
  public string Password { get; set; }
  public string FirstName { get; set; }
  public string MiddleName { get; set; }
  public string LastName { get; set; }
  public string Login { get; set; }
  public bool IsDeleted { get; set; }
}
```

Как класс, так и его свойства должны быть public, чтобы другие части кода могли с ними работать.

#### Создание класса для работы с данными

Опять же, я не знаю, сколько баллов дадут за это (и дадут ли), но этот класс
довольно хорошо «вычищает» код от запросов, оставляя только понятные названия
функций. Еще такой подход позволяет спокойно писать только запросы или только код
в формах, не задумываясь о другой части. В первом случае вы просто потом
используете эти методы, во втором закрываете комментариями нерабочую часть и
открываете, когда нужная функция появляется. К тому же, зная, где лежат все запросы,
их легко редактировать.

Все дальнейшие объяснения подразумевают, что у вас есть такой класс.
Если вы не хотите его делать, принцип работы функций будет похожий, достаточно
заменить return на какое-либо нужное вам действие.
Класс создается в UTILS. Я всегда его называю DataWork, но это, понятное
дело, дело предпочтения. К созданным using надо добавить using
System.Data.SqlClient; (это получится единственный класс, в котором нужна эта
библиотека) и using *название проекта*.CLASSES (для быстрого доступа). Сразу же
можно сделать глобальную переменную для строки подключения (берется из класса со
строкой). Также можно в целом иметь строку подключения в этом классе, но примеры
берутся из готовых работ, которые я сейчас переделывать не собираюсь.

Пример:
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data;
using System.Data.SqlClient;
using System.Security.Cryptography;
using demo4.CLASSES;
namespace demo4.UTILS
{
  internal class DataWork {
  static string ConnStr = ConnectionString.ConnStr;
  }
}
```

Шаблон функции:
```cs
public static *тип возвращаемого значения* *название*(*параметры*)
{
  using (SqlConnection conn = new SqlConnection(ConnStr))
  {
    conn.Open();
    string sql = *запрос*;
    *действия с запросом*
    if (*успешно*) {
       return *значение*;
    }
    throw new Exception(*текст исключения*);
  }
}
```

Исключение нужно, во-первых, чтобы визуалка не ругалась, что «не все пути к коду возвращают значение», 
когда функция любого типа, кроме void, во-вторых, так можно сразу задать сообщение, 
которое будет выводиться пользователю при ошибке.

#### Перенос главной формы в папку и изменение стартовой формы

После того, как были созданы папки, можно перенести форму, созданную при
генерации проекта, в нужную папку. Однако, просто перенести файл в папку почему-то
не работает. Нужно перейти в файл и подписать название папки после namespace *название проекта*
```cs
namespace demo4.FORMS
{
public partial class LoginForm : Form
```
Теперь в program.cs нужно изменить параметр в Application.Run на необходимую форму:
```cs
static void Main()
{
   Application.EnableVisualStyles();
   Application.SetCompatibleTextRenderingDefault(false);
   Application.Run(new FORMS.LoginForm());
}
```

Визуалка будет ругаться, причем на очень много вещей сразу. Чинится простым
запуском программы и перепросмотром файлов. Почему – вопрос хороший.

#### Импорт изображений

Если в импорте были даны изображения, создайте еще одну папку для них:
Далее просто перетащите папку с импортом (или только изображения) в проект:
Теперь изображения будут сохранены в проекте.

#### Создание формы авторизации

Внешний вид:
Для начала нужно создать форму. Обычно на ней не много элементов,
достаточно двух textbox`ов (с label к каждому из них) и кнопки.

Функция для получения пользователя по его логину и паролю:
```cs
public static User GetUser(string login, string password)
{
   using (SqlConnection conn = new SqlConnection(ConnStr))
   {
      conn.Open();
   
      string sql = "SELECT [ID], [Password], [FirstName], [MiddleName], [LastName], [Login],
      [IsDeleted] " +
      
      "FROM [User] " +
      "WHERE [Login] = @login " +
      "AND [Password] = @password " +
      "AND [IsDeleted] = 0";
      SqlCommand cmd = new SqlCommand(sql, conn);
      cmd.Parameters.AddWithValue("login", login);
      cmd.Parameters.AddWithValue("password", password);
      SqlDataReader reader = cmd.ExecuteReader();
      if (reader.Read())
      {
         User user = new User
         {
            ID = reader.GetInt32(0),
            Password = reader.GetString(1),
            FirstName = reader.GetString(2),
            MiddleName = reader.GetString(3),
            LastName = reader.GetString(4),
            Login = reader.GetString(5),
            IsDeleted = reader.GetBoolean(6)
         };
         return user;
      }
      throw new Exception("Пользователь не найден. Проверьте правильность данных и повторите
      попытку входа.");
   }
}
```

Сначала все соответствует шаблону. Используется подключение, оно же открывается. 
В запросе стоит перечислить все получаемые поля, это пригодится позже.

@login и @password – это параметры. Они гораздо безопаснее просто вставки строк. В остальном ничем не отличаются.

Поскольку это просто запрос, который не должен получать DataSet, создается команда cmd. 
После этого можно присвоить параметрам значения, используя AddWithValue("имя параметра", значения). 
Учтите, что имя параметра должно писаться полностью идентично запросу, но без символа @. 
После присвоения параметрам значений создается reader.
Если этот reader читает что-нибудь, создается новая переменная типа User,
заполняется через функции Get*название типа*(ряд) и возвращается. 
Если же ничего не прочиталось, кидается исключение, что такого пользователя нет.

Код формы:

Весь код на данной форме находится в кнопке:
```cs
private void LoginBtn_Click(object sender, EventArgs e)
{
   User user;
   try
   {
      user = DataWork.GetUser(LoginTxt.Text, PasswordTxt.Text);
   }
   catch (Exception ex)
   {
      MessageBox.Show(ex.Message);
      return;
   }
   this.Hide();
   MainForm MF = new MainForm(user);
   MF.ShowDialog();
   LoginTxt.Text = String.Empty;
   PasswordTxt.Text = String.Empty;
   this.Show();
}
```

Код состоит, по сути, из трех частей, не считая объявления переменной user.
Первая часть пытается с помощью try..catch присвоить пользователю значение,
используя написанную ранее функцию. Если у неё не получается, catch выводит
сообщение полученной ошибки и завершает работу функции (return).
Вторая часть прячет данную форму, создает следующую и показывает в
режиме диалогового окна (это важно). Заодно, при создании следующей формы, мы
передаем ей полученного пользователя, чтобы она могла дальше с ним работать.
Третья часть срабатывает тогда, когда закрывается следующая форма, в
данном случае – MainForm. Она очищает поля и снова показывает форму авторизации.

#### Шифрование пароля

Для начала нужно подключить библиотеку: using System.Security.Cryptography;

Для шифрования используйте функцию:
```cs
public static string GetHash(string login, string password)
{
   string salt1 = "^8{-";
   string salt2 = "&>nm";
   string pass = login + salt1 + password + salt2;
   using (var hash = SHA256.Create())
   {
      return string.Concat(hash.ComputeHash(Encoding.UTF8.GetBytes(pass)).Select(x =>
      x.ToString("X2")));
   }
}
```

Salt1 и salt2 – это соль – неизменная часть, прибавляющаяся к паролю спереди и/или сзади. 
Логин передается как изменяемая часть – перец. 
Сама функция шифрует получившуюся строку по алгоритму SHA256.
Для того, чтобы проверять пароль с шифрованием, надо изменить параметр,
подставляемый вместо пароля в запрос на пользователя:
cmd.Parameters.AddWithValue("password", GetHash(login, password));
Это, правда, может создать проблему того, что теперь в базе пароли тоже
должны быть зашифрованы по тому же самому алгоритму. Учитывая, что интернетом
для этого пользоваться не получится, можно, например, переписать все пароли такой
функцией (объяснений не будет, потому что никто в здравом уме такое делать не будет):

```cs
public static void HashAllPasswords()
{
   List<User> list = GetUserList();
   foreach (var user in list)
   {
      HashPassword(user);
   }
}
public static List<User> GetUserList()
{
List<User> list = new List<User>();
   using (SqlConnection conn = new SqlConnection(ConnStr))
   {
      conn.Open();
      string sql = "select ID, [Login], [Password] " +
      "from[User] ";
      SqlCommand cmd = new SqlCommand(sql, conn);
      SqlDataReader reader = cmd.ExecuteReader();
      while (reader.Read())
      {
         User user = new User();
         user.ID = reader.GetInt32(0);
         user.Login = reader.GetString(1);
         user.Password = reader.GetString(2);
         list.Add(user);
      }
      return list;
   }
}
public static void HashPassword(User user)
{

   using (SqlConnection conn = new SqlConnection(ConnStr))
   {
      conn.Open();
      string sql = "update [User] set [Password] = @password where [User].ID = @id";
      SqlCommand cmd = new SqlCommand(sql,conn);
      cmd.Parameters.AddWithValue("password", GetHash(user.Login, user.Password));
      cmd.Parameters.AddWithValue("id", user.ID);
      cmd.ExecuteNonQuery();
   }
}
```

Учитывайте, что её нужно прогнать только ОДИН раз.

Другой вариант: проверять, зашифрован ли пароль и шифровать его, если нет.
Проблема этого метода в том, что, если действовать самым очевидным методом и
просто проверять, совпадает ли напрямую пароль с тем, который уже в БД, если
какой-нибудь умник попробует войти с тем шифром, который вы получили в итоге, он
не только это сделает, но и перепишет пароль. Поскольку реализация этого займет
только больше времени, а результат получается отрицательный, никакого кода для
него не будет.

#### Проверка роли пользователя
Для того, чтобы узнать роль пользователя, когда они разнесены по разным
таблицам, можно воспользоваться таким запросом:
```cs
public static bool IsExec(User user)
{
   using (SqlConnection conn = new SqlConnection(ConnStr))
   {
      conn.Open();
      string sql = "select * from [Executor] where ID = @id";
      SqlCommand cmd = new SqlCommand(sql,conn);
      cmd.Parameters.AddWithValue("id", user.ID);
      SqlDataReader reader = cmd.ExecuteReader();
      return reader.HasRows();
   }
```
Функция bool, т.е. логическая. Запросом мы выбираем все подходящие поля в
другой таблице. В данном случае ID пользователей и ID исполнителей совпадали,
поэтому запрос достаточно простой. Здесь используется HasRows() – тоже логическая
функция, которая возвращает, есть ли ряды в полученных по запросу данных.
Используется эта функция максимально прямо:
```cs
if (IsExec(user))
{
   *делаете, что вам нужно*
}
```

Загрузка вариантов (например, для фильтрации) в
ComboBox
Функция:
```cs
public static List<string> GetExecList(int managerID)
{
   List<string> list = new List<string>();
   using (SqlConnection conn = new SqlConnection(ConnStr))
   {
      conn.Open();
      string sql = "select [User].FirstName, [User].MiddleName, [User].LastName " +
      "from[User],[Executor] " +
      "where Executor.ID = [User].ID " +
      "and Executor.ManagerID = @ManagerID";
      SqlCommand cmd = new SqlCommand(sql, conn);
      cmd.Parameters.AddWithValue("ManagerID", managerID);
      SqlDataReader reader = cmd.ExecuteReader();
      while (reader.Read())
      {
         list.Add($"{reader.GetString(0)} {reader.GetString(1)} {reader.GetString(2)}");
      }
      return list;
   }
}
```

Эта функция возвращает список из строк, в данном случае – ФИО исполнителей, подчиняющихся определенному менеджеру. 
Список создается до чтения, чтобы в него можно было добавлять ряды. 
Пока (while) reader читает, он добавляет каждый ряд в список через list.Add(элемент). 
После того, как reader закончит читать, список возвращается функцией.

Код:
Этот код должен прогоняться каждый раз, когда вам нужно загрузить варианты в
combobox. Обычно это один раз, при создании формы, однако, если в какой-то момент
варианты должны меняться, тогда при каждой смене.
```cs
ExecutorCmb.Items.Clear();
ExecutorCmb.Items.Add("");
foreach (string execName in DataWork.GetExecList(User.ID))
{
   ExecutorCmb.Items.Add(execName);
}
```

Сначала вычищаются все варианты, которые были до этого через Clear().
После этого добавляется один пустой вариант (если он нужен), он же может быть вариантом «все». 
Теперь используется цикл foreach(*элемент* in *коллекция*), который
срабатывает для каждого элемента в списке (список является коллекцией) и добавляет его в варианты combobox`а.

#### Вывод данных в DataGrid

Сразу предупреждаю, здесь не особо учитывается сортировка, фильтрация,
поиск, постраничный вывод и все прочее, поскольку это просто одна рандомно
выбранная функция из нескольких демо. Для меня простым решением является то, что
расписано на пару пунктов ниже, если вы не хотите с этим разбираться, я верю, что вы
найдете свой способ.

Для начала добавьте элемент DataGrid на форму. Дайте ему нормальное
название, это пригодится позже.

Функция для получения данных:
```cs
public static DataSet GetExecTasks(User user, string statusFilter)
{
   DataSet ds = new DataSet();
   using (SqlConnection conn = new SqlConnection(ConnStr))
   {
      conn.Open();
      string sql = " select Task.Title, Task.[Status], [User].FirstName " +
      "from[User], Executor, Task " +
      "where Task.ExecutorID = Executor.ID " +
      "and Executor.ID = [User].ID " +
      "and Executor.ID = @id";
      if (!String.IsNullOrEmpty(statusFilter))
      sql += " and Task.Status = @status";
      sql += " order by Task.CreateDateTime desc";
      SqlDataAdapter ada = new SqlDataAdapter(sql, conn);
      ada.SelectCommand.Parameters.AddWithValue("id", user.ID);
      ada.SelectCommand.Parameters.AddWithValue("status", statusFilter);
      ada.Fill(ds);
      return ds;
   }
}
```

Это единственный случай, когда используется адаптер, а не ридер, потому что
нужно засунуть данные в DataSet. Здесь немного по-другому добавляются параметры,
не просто Parameters.AddWithValue(), а SelectCommand.Parameters.AddWithValue().

Использование в формах:

Поскольку возвращается DataSet, можно для отрисовки делать так:
```cs
private void Render()
{
   TasksDataGrid.DataSource = DataWork.GetExecTasks(this.User, this.statusFilter).Tables[0];
}
```

Теперь при каждом изменении параметров нужно вызывать эту функцию Render().

---
