Тестовое задание для AIMCo "Калькулятор"
======================================

Задание
========

## Написать сервис "Калькулятор" производящий вычисления
сохраняющий в БД дату вычисления, условия задания на вычисление, результат вычисления; формирующий статистику по произведённым вычислениям
- БД: любая in-memory
- Архитектура: REST

Поддерживаемые форматы чисел:
- целые, дробные с разделителем “.”; 
- положительные или отрицательные
Поддерживаемые операции:
- +[сложение], 
- -[вычитание], 
- *[умножение], 
- /[деление], 
- ^[возведение в степень], 
- ()[группировка]
 
Поддерживаемые запросы на статистику: 
- COUNT(%дата%) - количество вычислений на дату,
- OPERATION(%операция%) - количество заданий содержащих заданную операцию,
- ONDATE(%дата%) - список всех заданий на дату,
- ONOPERATION(%операция%) - список всех заданий с операцией,
- POPULAR() - самое используемое число

Ограничения: средства Java SE

Оцениваемый результат: код в виде maven проекта в репозитории на github/bitbacket с сохранением истории vcs

Артефакт сборки проекта: war

Пример входных данных:
- (-7*8+9-(9/4.5))^2
- 9*1+4.5

Пример выходных данных: 
- 2401
- 13.5

Примерный вид записи в БД:
- 16-10-2017| (-7*8+9-(9/4.5))^2 | 2401 
- 17-11-2017| 9*1+4.5                   |13.5

Пример статистических запросов:
- COUNT(16-10-2017) должно вернуть 1
- ONOPERATION(-) должно вернуть (-7*8+9-(9/4.5))^2
- POPULAR() должно вернуть 9

*Валидация отсутствует в задании*

Запросы
=======
Все параметры передаются через JSON

Контроллер:
```
  @RestController
  @RequestMapping("/calculator")
  public class CalculatorController {
```
**Вычисение выражения:**
```
  @PostMapping()
  public CalculatorDTO setExpression(@RequestBody CalculatorDTO dto)
```
*json*
```
{
  "expression":"(-7*8+9-(9/4.5))^2"
}
```
**Количество вычислений на дату:**
```
  @GetMapping("/count")
  public int countForData(@RequestParam(name = "date")
```
*json*
```
{
  "date":"16-10-2017"
}
```
**Kоличество заданий содержащих заданную операцию:**
```
  @GetMapping("/operation")
  public int operation(@RequestParam(name = "expression") final String expression) {
```
*json*
```
{
  "expression":"(-7*8+9-(9/4.5))^2"
}
```
**Cписок всех заданий на дату:**
```
  @GetMapping("/onDate")
  public List<String> onDate(@RequestParam(name = "date")
```
*json*
```
{
  "date":"16-10-2017"
}
```
**Cписок всех заданий с операцией:**
```
  @GetMapping("/onOperation")
  public List<String> onOperation(@RequestParam(name = "expression") final String expression) {
```
*json*
```
{
  "expression":"-"
}
```
**Cамое используемое число:**
```
  @GetMapping("/popular")
  public String popular() {      
```
Инструменты
===========
- in-memory H2
- jdbc
- spring-boot
- Reverse Polish notation
- шаблон проектирования при реализации RPN Implementation из книги Design Patterns and Best Practices in Java


Итого
===========

Отличное задание! Когда я увидел формулу которую требуется вычислить понял "Тут нужно, что-то серьезное...". Поразмыслив, впомнил, когда читал книгу встречался где-то интересный математический метод, который назывался Обратной польской записью. В данной задаче реализовать его удалось в полном объеме. Это одно из самых увлекательных заданий которое я решал за последний год !

С уважением Анатолий.