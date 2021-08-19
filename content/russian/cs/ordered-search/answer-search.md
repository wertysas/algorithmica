---
title: Бинарный поиск по ответу
weight: 3
---

## Бинарный поиск по ответу

Рассмотрим такую задачу:

### Пример: "Корова в стойла"

**Условие:** На прямой расположены N стойл (даны их координаты на прямой), в которые необходимо расставить K коров так, чтобы минимальное расcтояние между коровами было как можно больше. Гарантируется, что $1 < K < N$.

**Решение: **

Если решать задачу в лоб, то вообще неясно что делать. Нужно решать **обратную** задачу: давайте предположим, что мы знаем это расстояние X, ближе которого коров ставить нельзя. Тогда сможем ли мы расставить самих коров?

Ответ - да, можно ставить их довольно просто: самую первую ставим в самое левое стойло, это всегда выгодно. Следующие несколько стойл надо оставить пустыми, если они на расстоянии меньше X. В самое левое стойло из оставшихся надо поставить вторую корову и так далее. Даже ясно как это писать: надо идти слева направо по отсортированному массиву стойл, хранить координату последней коровы, и либо пропускать стойло, либо ставить в него новую корову.

То есть если мы знаем расстояние X, то мы можем за O(n) проверить, можно ли расставить K коров на таком расстоянии. Ну так давайте запустим бинпоиск по X, ведь при слишком маленьком X коров точно можно расставить, а при слишком большом - нельзя, и как раз эту границу и просят найти в задаче ("как можно больше").

Осталось точно определить границы, то есть изначальные значения *left* и *right*. Нам точно хватит расстояния 0, так как гарантируется, что коров меньше, чем стойл. И точно не хватит расстояния max_coord - min_coord + 1, так как по условию есть хотя бы 2 коровы.

```
coords = [2, 5, 7, 11, 15, 20] # координаты стойл
k = 3 # число коров

def is_correct(x): # проверяем, можно ли поставить K коров в стойла, если между коровами расстояние хотя бы x
    cows = 1
    last_cow = coords[0]
    for c in coords:
        if c - last_cow >= x:
            cows += 1
            last_cow = c
    return cows >= k

left = 0 # расставить коров на расстоянии хотя бы 0 можно всегда
right = max(coords) - min(coords) + 1 # при таком расстоянии даже 2 коровы поставить нельзя
while right - left != 1:
    middle = (left + right) // 2
    if is_correct(middle): # проверяем, можно ли поставить K коров в стойла, если между коровами расстояние хотя бы middle
        left = middle # left всегда должна указывать на ситуацию, когда можно поставить коров
    else:
        right = middle # right всегда должна указывать на ситуацию, когда нельзя поставить коров
print left # left - максимальное расстояние, на котором можно расставить коров в стойла
```

```
9
```

### Общий принцип

Такой метод и называется **бинпоиск по ответу**. Он очень важный и очень распространен на олимпиадах, очень рекомендую решать на него задачи.

По сути мы просто взяли задачу "найдите максимальное X, такое что какое-то свойство от X выполняется" и решили её бинпоиском. Самое сложное - увидеть такую формулировку в задаче. Поэтому рассмотрим еще один пример.

### Пример: "Очень Легкая Задача"

**Условие:** есть два принтера, один печатает лист раз в $x$ минут, другой раз в $y$ минут. За сколько минут они напечатают $N$ листов? $N > 0$

**Решение**:
Здесь, в отличие от предыдущей задачи, кажется, существует прямое решение с формулой. Но вместо того, чтобы о нем думать, можно просто свести задачу к обратной. Давайте подумаем, как по числу минут $T$ (ответу) понять, сколько листов напечатается за это время? Очень легко:
$$\lfloor\frac{T}{x}\rfloor + \lfloor\frac{T}{y}\rfloor$$

Ясно, что за $0$ минут $N$ листов распечатать нельзя, а за $xN$ минут один только первый принтер успеет напечатать $N$ листов. Поэтому $0$ и $xN$ - это подходящие первые границы для бинарного поиска.

**Примечание:** заметьте, что задача в контесте немного отличается! Прочитайте внимательно условие.

### Задание

Решите как можно больше задач в практическом контесте:

https://informatics.msk.ru/mod/statements/view.php?id=34097

Там могут встречаться задачи как на бинпоиск по ответу, так и на тернарный поиск по ответу.
