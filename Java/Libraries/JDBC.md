---
Interview graded: true
Last edited time: 2023-07-16T20:26
Needs Rework: false
Status: Not started
Topic:
  - "[Hibernate](Hibernate/Hibernate%201%5C%5C)"
---
# **JDBC**

Платформенно независимый стандарт взаимодействия Java-приложений с различными СУБД, реализованный в виде пакета java.sql

[![](https://lh6.googleusercontent.com/8QVvPK9kioVDCkA4m2JqSCmCwOQAkg7UXV2PArbnZkACWSIUeAvUVFZWA-TH5zHMGhYKGbYRsqWGw2sHEYdAZMha_d7NJFhamZtUnohTeni8K4WiqGGHb4GpRF3EZhm-g7yn-ie4xrP5AuUAShUF3CttDxMKORjZzHluAZ-y1zhe_B-gNcBC2UxalpAA)](https://lh6.googleusercontent.com/8QVvPK9kioVDCkA4m2JqSCmCwOQAkg7UXV2PArbnZkACWSIUeAvUVFZWA-TH5zHMGhYKGbYRsqWGw2sHEYdAZMha_d7NJFhamZtUnohTeni8K4WiqGGHb4GpRF3EZhm-g7yn-ie4xrP5AuUAShUF3CttDxMKORjZzHluAZ-y1zhe_B-gNcBC2UxalpAA)

Подключение к бд реализуется с помощью JDBC драйверов и драйвер менеджера. Осуществить подключение можно с помощью:

- **DriverManager** - позволяет подключиться к базе данных по указанному URL, а также загружает JDBC Driver'ы, который он нашёл в CLASSPATH
- **DataSource** позволяет подключиться к менеджеру ConnectionPool

## **ConnectionPool**

Менеджер соединений с бд. По умолчанию Hikari

## **Statement**

Объект, который содержит в себе запрос. Бывает трех видов:

**Statement**

SQL выражение без параметров

**Prepared statement**

Предварительно компилирует запросы, которые могут содержать ? и подставляет туда входные параметры

**CallableStatement**

способ вызова хранимых в субд процедур(функций)

## **executors**

### **executeQuery**

Используется в запросах, результатом которых является один единственный набор значений, таких как запросов типа SELECT.

### **execute**

Используется, когда операторы SQL возвращают более одного набора данных, более одного счетчика обновлений или и то, и другое.

### **executeUpdate**

следует использовать, как для выполнения операторов управления данными типа INSERT, UPDATE или DELETE (DML - Data Manipulation Language), так и для операторов определения структуры базы данных CREATE TABLE, DROP TABLE (DDL - Data Definition Language). Возвращают количество измененных строчек

## **Вспомогательные классы**

### **Driver**

Компонент, который позволяет взаимодействовать Java программе с бд

### **Сonnection**

Класс, который представляет соединение с бд