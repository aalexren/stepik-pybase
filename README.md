# Программирование на Python
*Курс на платформе `stepik.org`. Ссылка на курс: [https://stepik.org/course/67](https://stepik.org/course/67).*

---

# Неделя 1. «Циклы. Строки. Списки»

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
    for i in range(idnt, n - idnt, 1): # down
        mtx[i][n-idnt-1] = num
        num += 1
    for i in range(n - idnt - 1, idnt + 1, -1): # left
        mtx[n-idnt-1][i-1] = num
        num += 1
    for i in range(n - idnt - 1, idnt, -1): # up
        mtx[i][idnt] = num
        num += 1
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

# Неделя 2. «Функции. Словари. Интерпретатор. Файлы. Модули»

## Задача 1
Напишите функцию `modify_list(l)`, которая принимает на вход список целых чисел, удаляет из него все нечётные значения, а чётные нацело делит на два. Функция не должна ничего возвращать, требуется только изменение переданного списка, например:
```python
lst = [1, 2, 3, 4, 5, 6]
print(modify_list(lst))  # None
print(lst)               # [1, 2, 3]
modify_list(lst)
print(lst)               # [1]

lst = [10, 5, 8, 3]
modify_list(lst)
print(lst)               # [5, 4]
```

### ***Обычное решение***
```python
def modify_list(l):
    for i in range(len(l)-1, -1, -1):
        if l[i] % 2 != 0:
            l.pop(i)
        else:
            l[i] //= 2
```

### ***Красивое решение***
```python
def modify_list(l):
    l[:] = [i//2 for i in l if not i % 2]
```

---

## Задача 2
Напишите программу, которая считывает из файла строку, соответствующую тексту, сжатому с помощью кодирования повторов, и производит обратную операцию, получая исходный текст.  
**Sample Input**
```
a3b4c2e10b1
```
**Sample Output**
```
aaabbbbcceeeeeeeeeeb
```

### ***Стандартное решение***
```python
s = input().strip()
i = 0
while i < len(s):
    j = i + 1
    while j < len(s) and s[j].isdigit():
        j += 1
    print(s[i] * int(s[i+1:j]), end='')
    i = j
```

### ***Красивое решение***
```python
s, d = input(), []
for i in s:
    if not i.isdigit(): d.append(i)
    else: d[-1] += i
print(*[i[0] * int(i[1:]) for i in d], sep='')
```

### ***Моё решение***
```python
inp = open('input.txt', 'r')

s = inp.readline()
iter = filter(str.isalpha, s)
idx = 0
n_char = next(iter, None)
while n_char != None:
    n_char = next(iter, None)
    if n_char == None: break
    n_idx = s.find(n_char, idx + 1)
    if n_idx == -1:
        break
    count = int(s[idx + 1:n_idx])
    print(s[idx] * count, sep='')
    idx = n_idx

print(s[idx:][0] * int(s[idx+1:]), sep='') # выводим последнюю букву, которую не вывели в цикле
inp.close()
```

---

## Задача 3
Напишите программу, которая считывает текст из файла (в файле может быть больше одной строки) и выводит самое частое слово в этом тексте и через пробел то, сколько раз оно встретилось. Если таких слов несколько, вывести лексикографически первое (можно использовать оператор < для строк).  
**Sample Input**
```
abc a bCd bC AbC BC BCD bcd ABC
```
**Sample Output**
```
abc 3
```

### ***Моё решение***
```python
with open('input.txt', 'r') as f:
    data = []
    for line in f:
        data += line.strip().lower().split(' ')

d = {key: data.count(key) for key in set(data)}
print(*sorted(d.items(), reverse=True, key=lambda item: item[1] or item[0])[0], sep=' ')
```

### ***Другое красивое решение***
```python
with open('input.txt') as f:
    text = f.read().lower().split()

popular_word = max(set(text), key=text.count)
print(popular_word, text.count(popular_word))
```
Суть в том, что `text.count` будет работать как замыкание и максимум выполнится на данных списка `text`.

*Ремарка* ---> решение может работать за $O(n)$, если сделать один проход по массиву с данными, чтобы заполнить словарь, попутно запоминая максимум. Если же считать, что на создание словаря уйдёт $O(log(n))$ времени, то общее время выполнение займёт $O(n\cdot log(n))$, что может быть сравнимо со сложностью предложенных выше решений.

---

## Задача 4
Имеется файл с данными по успеваемости абитуриентов. Он представляет из себя набор строк, где в каждой строке записана следующая информация:

Фамилия;Оценка_по_математике;Оценка_по_физике;Оценка_по_русскому_языку

Поля внутри строки разделены точкой с запятой, оценки — целые числа.

Напишите программу, которая считывает исходный файл с подобной структурой и для каждого абитуриента записывает его среднюю оценку по трём предметам на отдельной строке, соответствующей этому абитуриенту, в файл с ответом.

Также вычислите средние баллы по математике, физике и русскому языку по всем абитуриентам и добавьте полученные значения, разделённые пробелом, последней строкой в файл с ответом.  
**Sample Input:**
```
Петров;85;92;78
Сидоров;100;88;94
Иванов;58;72;85
```
**Sample Output:**
```
85.0
94.0
71.666666667
81.0 84.0 85.666666667
```

### ***Лаконичное, но прожорливое решение***
```python
with open('input.txt', 'r') as f:
    data = []
    for line in f:
        data.append(list(map(int, line.split(';')[1:])))

print(*[sum(data[i]) / 3 for i in range(len(data))], sep='\n')
print(*[sum([data[i][j] for i in range(len(data))])/len(data) for j in range(3)]) # здесь во вложенном списковом включении происходит "транспонирование" матрицы, поэтому sum корректно работает
```


---

# Полезные ссылки
1. [Полезный визуализатор выполнения кода в python](https://pythontutor.com/render.html#mode=display)
