# Программирование на Python
*Курс на платформе `stepik.org`. Ссылка на курс: [https://stepik.org/course/67](https://stepik.org/course/67).*

---

# Задачи блока «Циклы. Строки. Списки»

## Задача 1

Напишите программу, которая считывает с клавиатуры два числа `a` и `b`, считает и выводит на консоль среднее арифметическое всех чисел из отрезка `[a; b]`, которые кратны числу `3`.

В приведенном ниже примере среднее арифметическое считается для чисел на отрезке `[-5; 12]`. Всего чисел, делящихся на `3`, на этом отрезке `6`: `-3, 0, 3, 6, 9, 12`. Их среднее арифметическое равно `4.5`.

На вход программе подаются интервалы, внутри которых всегда есть хотя бы одно число, которое делится на `3`.

### ***Примитивное решение***

```python
a, b = [int(input()) for i in range(2)]
mean = 0
count = 0
for num in range(a, b + 1):
    if num % 3 == 0:
        mean += num
        count += 1
mean /= count
print(mean)
```

### ***Математическое решение***
```python
a, b = int(input()), int(input())
a = a + (-a % 3)
b = b - (b % 3)
print((a + b) / 2)
```

Подробное объяснение:  
1. [Деление с остатком отрицательных чисел](https://skysmart.ru/articles/mathematic/delenie-chisel-s-ostatkom)

---

## Задача 2
Напишите программу, которая выводит часть последовательности `1 2 2 3 3 3 4 4 4 4 5 5 5 5 5 ...` (число повторяется столько раз, чему равно). На вход программе передаётся неотрицательное целое число `n` — столько элементов последовательности должна отобразить программа. На выходе ожидается последовательность чисел, записанных через пробел в одну строку.

Например, если `n = 7`, то программа должна вывести `1 2 2 3 3 3 4`.

### ***Примитивное решение***
```python
n = int(input())
printed = 0
for i in range(1, n + 1):
    for j in range(i):
        print(i, end=' ')
        printed += 1
        if printed == n:
            break
    if printed == n:
        break
```

### ***Улучшенное решение с циклом***
```python
n = int(input())
count = 0
curr = 1
for i in range(n):
    print(curr, end=' ')
    count += 1
    if count == curr:
        curr += 1
        count = 0
```

### ***Решение с использованием свойств списков***
```python
n = int(input())
a = []
i = 0
while len(a) < n:
    a += [i] * i
    i += 1
print(*a[:n])
```

### ***Математическое решение с одним циклом***
```python
n = int(input())
for i in range(1, n + 1):
    print(int(0.5 + (2 * n) ** 0.5), sep=' ')
```
Формула для последовательности: $a(i) = floor(1/2 + \sqrt{2i})$

---

## Задача 3
Выведите таблицу размером $n \times n$, заполненную числами от $1$ до $n^2$ по спирали, выходящей из левого верхнего угла и закрученной по часовой стрелке, как показано в примере (здесь $n=5$):
```
1	2	3	4	5
16	17	18	19	6
15	24	25	20	7
14	23	22	21	8
13	12	11	10	9
```

### ***Решение с циклами***
```python
n = int(input())
mtx = [[0 for __ in range(n)] for _ in range(n)] # matrix
num = 1
idnt = 0 # indent
while num <= n*n:
    for i in range(idnt, n - idnt - 1, 1): # right
        mtx[idnt][i] = num
        num += 1
        if num > n*n: break
    for i in range(idnt, n - idnt, 1): # down
        mtx[i][n-idnt-1] = num
        num += 1
        if num > n*n: break
    for i in range(n - idnt - 1, idnt + 1, -1): # left
        mtx[n-idnt-1][i-1] = num
        num += 1
        if num > n*n: break
    for i in range(n - idnt - 1, idnt, -1): # up
        mtx[i][idnt] = num
        num += 1
        if num > n*n: break
    idnt += 1

for i in range(n):
    print(*mtx[i])
```

### ***Укороченное решение с условиями вместо циклов***
```python
n = int(input())
mtx = [[0] * n for i in range (n)]
i, j = 0, 0

for num in range(1, n * n + 1):
    mtx[i][j] = num
    if num == n * n:
        break
    if i <= j+1 and i+j < n-1:   # right
        j += 1
    elif i < j and i+j >= n-1:   # down
        i += 1
    elif i >= j and i+j > n-1:   # left
        j -= 1
    elif i > j+1 and i+j <= n-1: # up
        i -= 1

for i in range(n):
    print(*mtx[i])
```

---

# Полезные ссылки
1. [Полезный визуализатор выполнения кода в python](https://pythontutor.com/render.html#mode=display)
