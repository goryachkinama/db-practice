---
title: Лабораторная работа. Семестровое задание
---

# Описание предметной области

Имеется БД, описывающая орг.структуру крупной компании-холдинга. 
Предлагается создать систему, позволяющую управлять правами доступа сотрудников к различным приложениям.

Для этого в БД должна храниться следующая информация:

* Данные сотрудника
* Его кадровые назначения
* Ограничения доступа
* Права доступа и роли в системе
* Учетные данные
* Структура холдинга

## Данные сотрудника

В качестве личных данных указывается: 
* ФИО,
* Пол,
* Дата рождения,
* СНИЛС,
* Тип пользователя,
* Идентификатор (GUID),
* Контактная информация (Мобильный телефон, Адрес электронной почты, Дополнительные контакты),
* Дополнительная информация
* Статус (активный, уволен, в архиве, истёк срок действия, больничный, отпуск)

## Кадровые назначения

Помимо личной информации сотрудников, к каждому из них привязаны кадровые назначения.
Обязательно существует одно основное, но может быть и несколько дополнительных,
означающие, что сотрудник совмещает несколько функций.

В понятие кадрового назначения входят:

* Организация (выбирается одна из списка организаций)
* Подразделение (отдел)
* Должность
* Вид занятости (основное место работы, работа по совместительству и пр.)
* Руководитель (выбирается из этого же списка сотрудников, по идентификатору)
* Период трудоустройства (дата приёма и дата увольнения, если она есть)
* Период исполнения должностных обязанностей
* Табельный номер
* Кабинет
* Рабочий телефон
* Рабочий адрес электронной почты

 Период исполнения должностных обязанностей вычисляется исходя из периода трудоустройства 
 за вычетом периодов неисполнения (сотрудник был в отпуске, болел либо брал отгулы).
 Информация об этих периодах (их может быть несколько) должна быть отражена здесь.
 

## Ограничения доступа

* Период доступа (даты с и по)
* Доступ по IP-адресам (ограничен конкретными адресами, либо не ограничен вовсе, если они не указаны)
* Рабочее расписание (график доступа)

Рабочее расписание (график доступа) представляет собой таблицу для каждого для недели, 
с какого время по какое данному сотруднику глобально разрешено входить в систему.
Он может редактироваться для каждого дня отдельно, может задаваться шаблоном (а-ля "будние дни с 9 до 18"), 
либо может быть указано "круглосуточно".

## Права доступа и роли в системе

Каждый сотрудник так или иначе является пользователем корпоративной системы.
Ему могут быть предоставлены права доступа к различным ее частям в зависимости от исполняемых им ролей.

* Роли (выбираются из списка и действуют с какого-то определенного дня по какой-то)
* Права доступа (выбираются из другого списка, действуют определенный период и относятся к тому или иному приложению)

## Учетные данные

Чтобы доступ был предоставлен, для каждого сотрудника создаются ученые записи.
Их может быть несколько и они могут быть созданы в различных хранилищах, в зависимости от рабочих задач.

По учеткам хранится следующая информация:
* Последний вход в систему (дата и время)
* Идентификатор
* Срок действия учетки (с и по)
* Срок действия пароля (с и по)
* Последняя смена пароля
* Статус (активная, неактивная, заблокирована, в архиве)

## Организация

* Название
* ИНН (уникальный)
* КПП
* ОГРН
* Код
* Тип (правовая форма: ООО, АО, ИП)
* Родительская организация (идентификатор из этого же списка)
* Статус (активная, заблокирована, не действует, в архиве)
* Дата закрытия (для закрытых)

---