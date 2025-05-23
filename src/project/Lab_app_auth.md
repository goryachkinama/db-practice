# Разработка приложения: пользователи и авторизация

[Алгоритмы шифрования. Хэширование. Статическая и динамическая соль.](../src/Lab_salt.md)

[Соединение с БД и форма входа на C#](../src/Lab14.md)

Ещё темы по теории, которые можно изучить:

* Авторизация, идентификация и аутентификация пользователей в ИС. Способы аутентификации. 
* Пользователи и права доступа.
* Уровни доступа к БД (средствами ОС и БД). Управление доступом. Разработчик, программист, пользователь, администратор БД.

---

Необходимо разработать функционал регистрации и авторизации в приложении.

Для этого:
* в БД добавляется таблица, которая будет хранить учетные данные (логин и пароль) пользователя и его роль в приложениии, если предполагается, что для разных пользователей интерфейс и функции будут различаться
* в приложении разрабатываются окна регистрации нового пользователя и входа уже существующего
* после успешного входа отображается главное окно с выбором дальнейших действий  

Основное требование - шифрование пароля. 
Предпочтительнее всего использовать не только хэш-функцию, но и статическую либо динамическую соль 

---

[К списку заданий](../../program-2-project.md)
