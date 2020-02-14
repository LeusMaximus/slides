# Новелла о прагматичном тестировании

- О себе
- О своём пути
- О цели выступления

# Типичные проблемы разработки

- Баги
- Деньги
- Сроки
- Менеджеры

# Борьба с багами

- Пиши без багов! (с) Наивный Тимлид
- Поднять отряд ручных тестировщиц! (c) Типичный Менеджер
- Опишем всё типами! (с) Читатель Хабра
- Кажется нам нужны тесты.. (с) Безысходность

# Виды контрактов

- На словах
- На типах
- На примерах

# Нелюбовь к тестам

- Долго писать
- Сложно писать
- Всё время ломаются
- Бесполезны

## Долго писать

- Медленный запуск
- Много кода
- Много кейсов
- Сложно писать

## Сложно писать

- Нетестируемая архитектура
- Ограниченный тестовый фреймворк

## Всё время ломаются

- Меняются требования
- Меняется понимание требований
- Фиксируется неважное поведение

## Бесполезные тесты

- Проверяют не важное поведение
- Не проверяют важное поведение
- Недостаточный охват

## Недостаточный охват

- Разработчики не умеют в тестирование
- Вредные практики (TDD, GWT)
- Лень

# Идеальные тесты

- Проверяют лишь важное
- Не фиксируют неважное
- Минимальное число
- Охватывают все кейсы
- Исполняются быстро
- Писать просто

# Задачи тестов

- Обнаружение дефектов
- Локализация дефектов
- Ускорение разработки
- Актуальная документация

# Объекты тестирования

- Модуль - единица кода
- Компонент - единица функциональности
- Система - едница развёртывания

# Типы тестов

- Функциональные
- Интеграционные
- Нагрузочные

# Процессы тестирования

- Приёмочный - то, что меняли
- Регрессионный - то, что не меняли
- Дымовой - только важное
- Полный - всё

# Полнота тестирования

```typescript
function isEquilateral( a : number , b : number , c : number ) {
    return ( a === b )&&( b === c )
}
```

## Позитивные сценарии

```typescript
isEquilateral( 2 , 2 , 2 ) === true
isEquilateral( 3 , 2 , 2 ) === false
```

Это чёрный ящик

## Ветки логики

> **Белый ящик даёт лучшее покрытие**

```typescript
function isEquilateral( a : number , b : number , c : number ) {
    if( a !== b ) { // first branch
        return false
    } else { // second branch
        return b !== Math.random()
        // return b !== c
    }
}
```

```typescript
function isEquilateral(
    ... sides : [ number , number , number ]
) {
    const [ a , b , c ] = sides.sort( compareNumbers ) 
    return a === c // single branch
}
```

## Негативные сценарии

```typescript
isEquilateral( 0 , 2 , 2 ) 🔥 // not a triangle
```

## Классы эквивалентности

```typescript
c : [ -∞ .. 0 .. a+b .. ∞ ]
```

```typescript
isEquilateral( -2 , 2 , 2 ) 🔥 // [ -∞ .. 0 ]
isEquilateral( 3 , 2 , 2 ) === false // [ 0 .. 3 ]
isEquilateral( 5 , 2 , 2 ) 🔥 // [ 3 .. +∞ ]
```

## Граничные условия

```
NaN
-∞
0
MIN_VALUE
a + b - MIN_VALUE'
a + b
MAX_VALUE
+∞
```

# Соотношение тестов к коду

Полное покрытие - не менее **11** тестов

```typescript
isEquilateral( Number.NEGATIVE_INFINITY , 2 , 2 ) 🔥
isEquilateral( 0 , 2 , 2 ) 🔥

isEquilateral( Number.MIN_VALUE , Number.MIN_VALUE , Number.MIN_VALUE ) === true
isEquilateral( Number.MIN_VALUE , 2 , 2 ) === false

isEquilateral( 2 , 2 , 2 ) === true
isEquilateral( 3 , 2 , 2 ) === false

isEquilateral( 4 - 0.5 ** 51 , 2 , 2 ) === false
isEquilateral( 4 - 0.5 ** 51 , 4 - 0.5 ** 51 , 4 - 0.5 ** 51 ) === true

isEquilateral( 4 , 2 , 2 ) 🔥
isEquilateral( Number.POSITIVE_INFINITY , 2 , 2 ) 🔥

isEquilateral( Number.NaN , 2 , 2 ) 🔥
```

# Суть TDD

1. Red
2. Green
3. Refactor
4. Go to (1)

# Блеск и нищета TDD

```typescript
function checkedSide( a :  number ) {
    if( a > 0 && Number.isFinite(a) ) return a
    throw 'wrong input'
}

function isEquilateral(
    ... sides : [ number , number , number ]
) {
    const [ a , b , c ] = sides.map( checkSide ).sort( compareNumbers )
    if( c >= a + b ) throw 'wrong input'
    return a === c
}
```

> **После 5 цикла тесты будут сразу зелёные**

# Правильный TDD - не TDD

1. All cases checked? Done.
2. Write test.
3. Green? Go to (1).
4. Fix it.
5. Go to (3).

# Когда тесты лишние

- Когда требования не ясны
- Когда дохода дают меньше, чем расходов

Переписывать код - обидно. Переписывать тесты - обидно вдвойне.

# 




# Резюме

- Белый ящик предпочтительнее чёрного
- TDD фундаментально непригоден
- Модульные тесты бессмысленны
- Фиксировать надо важное поведение
- А невавжное фиксироваться не должно
- Тесты нужны самому же разаботчику
- Разработчиков не знают как тестировать
