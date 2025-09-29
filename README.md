<h1 name="content" align="center"><a href="">
</a> MSSQL</h1>

<p align="center">
  <a href="#-lab1"><img alt="lab1" src="https://img.shields.io/badge/Lab1-blue"></a> 
  <a href="#-lab2"><img alt="lab2" src="https://img.shields.io/badge/Lab2-blue"></a> 
</p>
<h3 align="center">
  <a href="#client"></a>
  Вариант 15. Гостиница
  
Список номеров: вместимость, комфортность, этаж, цена.
Список клиентов: ФИО, паспорт, пол.
Информация о сдаче номеров: дата заселения, срок поселения, фактическая дата освобождения, предварительная стоимость проживания
Информация о бронировании номеров: дата предполагаемого заселения, срок поселения, предварительная стоимость проживания.


Реализовать:
- Поиск свободных номеров по введенным критериям (Комфортность, дата заезда, срок поселения);
- подсчет стоимости проживания при выезде клиента;
- бронирование номера;
- поиск постоянных клиентов (заселявшихся более 1 раза)
- поиск клиентов с максимальным сроком проживания

</h3>

# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab1
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
  Разработать ER-модель данной предметной области: выделить сущности, их атрибуты, связи между сущностями. 
Для каждой сущности указать ее имя, атрибут (или набор атрибутов), являющийся первичным ключом, список остальных атрибутов.
Для каждого атрибута указать его тип, ограничения, может ли он быть пустым, является ли он первичным ключом.
Для каждой связи между сущностями указать: 
- тип связи (1:1, 1:M, M:N)
- обязательность

ER-модель д.б. представлена в виде ER-диаграммы (картинка)

По имеющейся ER-модели создать реляционную модель данных и отобразить ее в виде списка сущностей с их атрибутами и типами атрибутов,  для атрибутов указать, явл. ли он первичным или внешним ключом 
</h3>

## №1. ER-модель
![image](/pictures/ER.png)

## №1.1. Реляционная модель
![image](/pictures/REL.png)

# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab2
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
  В соответствии с реляционной моделью данных, разработанной в Лаб.№1, создать реляционную БД на учебном сервере БД:
  
- создать таблицы, определить первичные ключи и иные ограничения
  
- определить связи между таблицами
  
- создать диаграмму

- заполнить все таблицы адекватной информацией (не меньше 10 записей в таблицах, наличие примеров для связей типа 1:M )

</h3>

## №2. Создание таблиц

```
CREATE TABLE Клиент (
    клиент_id INT PRIMARY KEY IDENTITY,
    ФИО NVARCHAR(150) NOT NULL,
    Паспортные_данные CHAR(15) UNIQUE NOT NULL,
    Пол CHAR(1) CHECK (Пол IN ('М', 'Ж')) NOT NULL
);

CREATE TABLE Бронирование (
    бронь_id INT PRIMARY KEY IDENTITY,
    Дата_создания DATE NOT NULL DEFAULT GETDATE(),
    Статус NVARCHAR(20) NOT NULL CHECK (Статус IN ('создана','отменена','завершена')),
    Дата_последнего_изменения DATETIME NOT NULL DEFAULT GETDATE(),
    Дата_предположительного_заселения DATE NOT NULL,
    Предварительная_стоимость DECIMAL(10,2) NOT NULL,
    клиент_id INT NOT NULL FOREIGN KEY REFERENCES Клиент(клиент_id)
);

CREATE TABLE Номер (
    номер_id INT PRIMARY KEY IDENTITY,
    Вместимость INT NOT NULL,
    Этаж INT NOT NULL,
    Комфортность NVARCHAR(15) NOT NULL,
    Цена_сутки DECIMAL(10,2) NOT NULL
);

CREATE TABLE Номер_Бронирование (
    номер_id INT NOT NULL FOREIGN KEY REFERENCES Номер(номер_id),
    бронь_id INT NOT NULL FOREIGN KEY REFERENCES Бронирование(бронь_id),
    PRIMARY KEY (номер_id, бронь_id)
);

CREATE TABLE Заселение (
    заселение_id INT PRIMARY KEY IDENTITY,
    Предварительная_стоимость DECIMAL(10,2),
    Фактическая_дата_выезда DATE NULL,
    Итоговая_стоимость DECIMAL(10,2),
    Дата_заселения DATE NULL,
    клиент_id INT NOT NULL FOREIGN KEY REFERENCES Клиент(клиент_id)
);

CREATE TABLE Заселение_Номер (
    заселение_id INT NOT NULL FOREIGN KEY REFERENCES Заселение(заселение_id),
    номер_id INT NOT NULL FOREIGN KEY REFERENCES Номер(номер_id),
    PRIMARY KEY (заселение_id, номер_id)
);

CREATE TABLE Услуга (
    услуга_id INT PRIMARY KEY IDENTITY,
    Название NVARCHAR(50) NOT NULL,
    Цена DECIMAL(10,2) NOT NULL
);

CREATE TABLE Заселение_Услуга (
    заселение_id INT NOT NULL FOREIGN KEY REFERENCES Заселение(заселение_id),
    услуга_id INT NOT NULL FOREIGN KEY REFERENCES Услуга(услуга_id),
    PRIMARY KEY (заселение_id, услуга_id)
);

CREATE TABLE Оплата (
    оплата_id INT PRIMARY KEY IDENTITY,
    Дата_оплаты DATE NOT NULL,
    Сумма DECIMAL(10,2) NOT NULL,
    Способ_оплаты NVARCHAR(30) NOT NULL,
    заселение_id INT NOT NULL FOREIGN KEY REFERENCES Заселение(заселение_id)
);
```

## №2.1. Диаграмма
![image](/pictures/mod.png)

## №2.2. Заполненные таблицы
### Клиент
![image](/pictures/client.png)
### Номер
![image](/pictures/nomer.png)
### Бронирование
![image](/pictures/bron.png)
### Номер_Бронирование
![image](/pictures/nombro.png)
### Заселение
![image](/pictures/zasel.png)
### Заселение_Номер
![image](/pictures/zasnom.png)
### Услуга
![image](/pictures/yslygi.png)
### Заселение_Услуга
![image](/pictures/zasysl.png)
### Оплата
![image](/pictures/oplata.png)
