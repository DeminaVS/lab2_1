# lab2_1
# Цель
Научиться работать с функциями и подпрограммами в Python,
а также исследовать их преимущества, особенности применения и возможные
недостатки. Закрепить навыки создания, вызова и применения различных видов
функций, включая глобальные, локальные, рекурсивные и анонимные.
# 2.1.1 Определить значение функции
```
import math

def sgn(a):
  if a > 0:
    return 1
  elif a == 0:
    return 0
  else:
    return -1

def calculate_z(x, y):
  chisl = sgn(x) + y**2
  znam = sgn(y) - (abs(x))**(1/2)

  if znam == 0:
    return None

  return chisl / znam

try:
  x = float(input())
  y = float(input())

  result = calculate_z(x, y)
  print(round(result, 2) if result is not None else "Undefined")

except ValueError:
  print("Ошибка: x и y должны быть числами!")
```
# 2.1.2 Дан список температурных изменений в течение дня (целые числа). Известно, что измеряющее устройство иногда сбоит и записывает отсутствие температуры (значение None ). Выведите среднюю температуру за наблюдаемый промежуток времени, предварительно очистив список от неопределенных значений. Гарантируется, что хотя бы одно определенное значение в списке есть.
```
def avg(data):
    return sum(data) / len(data)

def cleared_data(data):
    return [x for x in data if x is not None]

n = int(input("Кол-во измерений: "))

data = []
for i in range(n):
    value = input(f"Измерение {i+1}-е: ")
    if value == '-':
        data.append(None)
    else:
        data.append(int(value))

cleaned = cleared_data(data)

average_temp = avg(cleaned)

print(f"Средняя температура: {average_temp:.2f}")
```
# 2.1.3 Выведите все счастливые номера билетов в диапазоне от a до b (положительные целые числа, a<b), если известно, что счастливым считается номер, у которого количество четных цифр равно количеству нечетных.
```
def is_lucky(num):
    even_count = 0
    odd_count = 0

    for digit in str(num):
        if int(digit) % 2 == 0:
            even_count += 1
        else:
            odd_count += 1

    return even_count == odd_count

def lucky_numbers(a, b):
  return[num for num in range(a, b + 1) if is_lucky(num)]

a = int(input())
b = int(input())

if a < b:
  result = lucky_numbers(a, b)
  print(result)
else:
  print("Ошибка: a должно быть меньше b")
```
# 2.1.4 Дата характеризуется тремя натуральными числами: день, месяц и год. Учитывая, что год может быть високосным, реализуйте две функции, которые определяют вчерашнюю и завтрашнюю дату.
```
def is_leap(year):
# является ли високосным
    return (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)


def days(month, year):

    if month in [1, 3, 5, 7, 8, 10, 12]:
        return 31
    elif month in [4, 6, 9, 11]:
        return 30
    elif month == 2:
        return 29 if is_leap(year) else 28
    else:
        return 0  # на случай некорректного месяца


def previous_date(day, month, year):

    if day > 1:
        return (day - 1, month, year)
    else:
        if month > 1:
            prev_month = month - 1
            prev_day = days(prev_month, year)
            return (prev_day, prev_month, year)
        else:
            # Переход к предыдущему году
            prev_year = year - 1
            return (31, 12, prev_year)


def next_date(day, month, year):

    days_in_month = days(month, year)

    if day < days_in_month:
        return (day + 1, month, year)
    else:
        if month < 12:
            return (1, month + 1, year)
        else:
            # Переход к следующему году
            return (1, 1, year + 1)

day, month, year = map(int, input("День, месяц, год через пробел: ").split())

prev_day, prev_month, prev_year = previous_date(day, month, year)
next_day, next_month, next_year = next_date(day, month, year)

print(f"Предыдущий день: {prev_day:02d}/{prev_month:02d}/{prev_year}")
print(f"Следующий день: {next_day:02d}/{next_month:02d}/{next_year}")
```
# 2.1.5 В задаче № 3.1.4 замените функции получения вчерашней и завтрашней даты на одну: def another_date(day, month, year, delta=1): pass где delta - ключевой параметр, определяющий сколько дней необходимо добавить или вычесть (если аргумент отрицательный) из переданной даты. Функции, реализованные в задаче № 5.2.4 сделайте локальными для another_date() , вызывая их внутри необходимое количество раз.
```
def another_date(day, month, year, delta=1):

    def is_leap(year):
    # является ли високосным
        return (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)


    def days(month, year):

        if month in [1, 3, 5, 7, 8, 10, 12]:
            return 31
        elif month in [4, 6, 9, 11]:
            return 30
        elif month == 2:
            return 29 if is_leap(year) else 28
        else:
            return 0


    def next_date(day, month, year):

        days_in_month = days(month, year)

        if day < days_in_month:
            return (day + 1, month, year)
        else:
            if month < 12:
                return (1, month + 1, year)
            else:
                return (1, 1, year + 1)


    def previous_date(day, month, year):

        if day > 1:
            return (day - 1, month, year)
        else:
            if month > 1:
                prev_month = month - 1
                prev_day = days(prev_month, year)
                return (prev_day, prev_month, year)
            else:
                prev_year = year - 1
                return (31, 12, prev_year)

    current_day, current_month, current_year = day, month, year

    if delta >= 0:
        # Добавляем дни
        for _ in range(delta):
            current_day, current_month, current_year = next_date(current_day, current_month, current_year)
    else:
        # Вычитаем дни (delta отрицательный)
        for _ in range(abs(delta)):
            current_day, current_month, current_year = previous_date(current_day, current_month, current_year)

    return current_day, current_month, current_year


day, month, year = map(int, input("День, месяц, год через пробел: ").split())
delta = int(input("Сдвиг (может быть отрицательным): "))

new_day, new_month, new_year = another_date(day, month, year, delta)

print(f"Новый день: {new_day:02d}/{new_month:02d}/{new_year}")
```
# 2.1.6 Дан список из чисел. Определите их НОК (наименьшее общее кратное) и НОД (наибольший общий делитель).
```
def gcd(first, second):

    a = abs(first)
    b = abs(second)
    while b:
        a, b = b, a % b
    return a


def lcm(first, second):

    if first == 0 or second == 0:
        return 0
    return abs(first * second) // gcd(first, second)


def gcd_nums(nums):

    if not nums:
        return 0

    result = nums[0]
    for i in range(1, len(nums)):
        result = gcd(result, nums[i])
    return result


def lcm_nums(nums):

    if not nums:
        return 0

    result = nums[0]
    for i in range(1, len(nums)):
        result = lcm(result, nums[i])
    return result


# Ввод данных
nums = list(map(int, input("Введите числа через пробел: ").split()))

# Вычисление НОД и НОК
gcd_result = gcd_nums(nums)
lcm_result = lcm_nums(nums)

# Вывод результатов
print(f"НОД = {gcd_result}")
print(f"НОК = {lcm_result}")
```
# 2.1.7 Даны n предложений. Определите, сколько из них содержат хотя бы одну цифру.
```
def has_digits(sentence):

    for char in sentence:
        if char.isdigit():
            return True
    return False


def sentences_with_digits_count(sentences):

    count = 0
    for sentence in sentences:
        if has_digits(sentence):
            count += 1
    return count


n = int(input("Введите количество предложений: "))

sentences = []
for i in range(n):
    sentence = input(f"Введите предложение №{i + 1}:\n")
    sentences.append(sentence)

result = sentences_with_digits_count(sentences)
print(f"Предложений с цифрой = {result}")
```
# 2.1.8 Дана строка s и символ k. Реализуйте функцию, рисующую рамку из
символа k вокруг данной строки, например:
***************
*Текст в рамке*
***************
```
def print_with_border(string, char):
    border = char * (len(string) + 2)
    print(border)
    print(f"{char}{string}{char}")
    print(border)

s = input("Введите строку: ")
k = input("Введите символ: ")

print_with_border(s, k)
```
# 2.1.9 Напишите функции для перевода числа (N∈[2;16]):  из 10-й системы счисления в N-ю;  из N-й системы счисления в 10-ю.
```
LETTERS_EX = {10: "A", 11: "B", 12: "C", 13: "D", 14: "E", 15: "F"}
DIGITS_EX = {"A": 10, "B": 11, "C": 12, "D": 13, "E": 14, "F": 15}


def to_base(number, base):
    """Преобразовать десятичное число 'number' в с.с. 'base'."""
    if number == 0:
        return "0"

    d = []
    n = number
    while n > 0:
        c = n % base
        if c < 10:
            d.append(str(c))
        else:
            d.append(LETTERS_EX[c])
        n = n // base
    d.reverse()
    return ''.join(d)


def from_base(number, base):
    """Преобразовать число в десятичную систему счисления"""
    number_str = str(number).upper()
    values = []

    for position, digit_char in enumerate(number_str[::-1]):
        if digit_char in DIGITS_EX:
            digit_value = DIGITS_EX[digit_char]
        else:
            digit_value = int(digit_char)

        values.append(digit_value * (base ** position))
    return sum(values)


base = int(input("Введите основание: "))
number = int(input("Введите число для перевода: "))
number_2 = input("Введите число для перевода в десятичную систему: ")

print(to_base(number, base))
print(from_base(number_2, base))
```
# 2.1.10 Для введенного предложения выведите статистику символ=количество . Регистр букв не учитывается.
```
def sentence_stats(sentence):

    stats = {}
    # Приводим все символы к нижнему регистру и перебираем
    for char in sentence.lower():
        if char in stats:
            stats[char] += 1
        else:
            stats[char] = 1
    return stats


# Ввод предложения
s = input("Введите предложение: ")

# Получение статистики
result = sentence_stats(s)

# Вывод результата
print(result)
```
# 2.1.11 Используя шифр Цезаря (достаточно только букв русского алфавита, знаки препинания не изменяются), зашифруйте, а затем расшифруйте введенную строку.
```

def ceasar(text, shift):

    # Набор кириллических букв (строчные)
    letters = [chr(i) for i in range(ord('а'), ord('я') + 1)]

    result = []

    for char in text:
        if char.lower() in letters:
            # Определяем индекс буквы в алфавите
            if char.isupper():
                # Для заглавных букв
                index = letters.index(char.lower())
                new_index = (index + shift) % len(letters)
                result.append(letters[new_index].upper())
            else:
                # Для строчных букв
                index = letters.index(char)
                new_index = (index + shift) % len(letters)
                result.append(letters[new_index])
        else:
            # Не буквы оставляем без изменений
            result.append(char)

    return ''.join(result)

text = input("Введите предложение: ")
shift = int(input("Введите сдвиг: "))

encoded = ceasar(text, shift)
decoded = ceasar(encoded, -shift)  # сдвиг в обратную сторону для расшифровки

print("Зашифрованная строка:", encoded)
print("Расшифрованная строка:", decoded)
```
# 2.1.12 В вагоне-купе имеется некоторое количество купе, в каждом из которых по 4 места. Разработчик хранит информацию о занятости одного купе в виде словаря: {1: 'м', 2: None, 3: None, 4: 'ж'} где: 6  ключ определяет номер места (нечетные номера - нижние места, четные - верхние);  значение может быть одно из трех: None , "м" и "ж" , если место не занято, занято мужчиной или женщиной соответственно. Информация о занятости всего вагона хранится как список указанных словарей. Определите:  список полностью свободных купе;  список свободных мест в вагоне;  список свободных нижних или верхних мест;  список свободных мест в купе с исключительно мужской компанией;  список свободных мест в купе с исключительно женской компанией.
```
data = [
    {1: 'ж', 2: 'м', 3: 'м', 4: 'ж'},
    {1: 'ж', 2: 'м', 3: 'ж', 4: 'ж'},
    {1: 'ж', 2: 'ж', 3: 'ж', 4: 'ж'},
    {1: 'ж', 2: 'м', 3: 'м', 4: 'м'},
    {1: None, 2: None, 3: None, 4: None},
    {1: 'м', 2: None, 3: None, 4: 'ж'},
    {1: None, 2: None, 3: None, 4: None},
    {1: 'м', 2: 'м', 3: None, 4: 'м'},
    {1: 'ж', 2: None, 3: None, 4: 'ж'}
]


def vacant_compartments(data):

    result = []
    for i, compartment in enumerate(data, 1):
        # Проверяем, что все места в купе свободны (None)
        if all(seat is None for seat in compartment.values()):
            result.append(i)
    return result


def vacant_seats(data, compartments_condition=None, seat_condition=None):

    result = []
    for compartment_num, compartment in enumerate(data, 1):
        # Проверяем условие для купе (если задано)
        if compartments_condition is None or compartments_condition(compartment):
            for seat_num, seat_value in compartment.items():
                # Место свободно и удовлетворяет условию для места
                if seat_value is None and (seat_condition is None or seat_condition(seat_num, seat_value)):
                    result.append((compartment_num, seat_num))
    return result


def is_same_sex_and_vacant(compartment, sex):

    # Проверяем, что все занятые места заняты пассажирами указанного пола
    occupied_seats = [value for value in compartment.values() if value is not None]
    vacant_seats = [value for value in compartment.values() if value is None]

    # Есть свободные места и все занятые места заняты пассажирами нужного пола
    return len(vacant_seats) > 0 and all(s == sex for s in occupied_seats)


# список полностью свободных купе
print(vacant_compartments(data))
# список свободных мест в вагоне
print(vacant_seats(data))
# список свободных нижних мест
print(vacant_seats(data, seat_condition=lambda seat, value: seat % 2 != 0))
# список свободных верхних мест
print(vacant_seats(data, seat_condition=lambda seat, value: seat % 2 == 0))
# список свободных мест в купе с исключительно мужской компанией
print(vacant_seats(data, lambda x: is_same_sex_and_vacant(x, "м")))
# список свободных мест в купе с исключительно женской компанией
print(vacant_seats(data, lambda x: is_same_sex_and_vacant(x, "ж")))
```
# 2.1.13 Дан список с результатами голосования на выборах в виде: [1, 3, 2, 2, 2, 5, -1, ...] где номер определяет голос за партию из списка:

Партия №1.
Партия №2.
Партия №3.
Партия №4.
Партия №5. -1. Испорченный бланк. Подведите итоги выборов, выведя на экран список партий в соответствии с убыванием количества полученных голосов и их процентным соотношением:
Партия №2 | 1111 | 58.21%
Партия №4 | 999 | 38.14% ...
```
votes = [
    5, 4, -1, 3, 4, 2, 1, 1, 1, 4, 1, 3, 1, 3, 5, -1, 5, 2, 5, 5,
    5, 3, 2, 3, 3, 2, 2, 1, 3, 2, 5, 5, 2, 4, 1, 1, 3, 2, 2, 3, 3,
    3, 1, 4, 2, 1, 4, 2, 2, 4, 1, -1, 5, 3, 1, 4, 5, 2, 2, 3, 3,
    4, -1, 2, 3, 3, 2, 4, 1, 1, 5, -1, 4, 2, 2, 3, -1, 4, 2, 5, 4,
    2, -1, 3, 1, 4, 3, 5, 4, 1, 5, 3, 2, 4, 2, 1, 5, 5, 1, 1, 3, 4
]

parties = {
    1: "Партия №1", 2: "Партия №2", 3: "Партия №3",
    4: "Партия №4", 5: "Партия №5", -1: "Испорчено"
}


def parties_votes(parties, votes):

    result = {}
    for vote in votes:
        party_name = parties[vote]
        if party_name in result:
            result[party_name] += 1
        else:
            result[party_name] = 1
    return result


def print_results(votes_for_p):

    # Общее количество голосов
    total_votes = sum(votes_for_p.values())

    # Сортируем партии по убыванию количества голосов
    sorted_parties = sorted(votes_for_p.items(), key=lambda x: x[1], reverse=True)

    # Выводим результаты
    for i, (party_name, votes_count) in enumerate(sorted_parties, 1):
        percentage = (votes_count / total_votes) * 100
        # Форматируем вывод: выравниваем числа и проценты
        print(f"{i}. {party_name} | {votes_count:2d} | {percentage:5.2f}%")


print_results(parties_votes(parties, votes))
```
# 2.1.14 Напишите функцию, которая принимает неограниченное количество числовых аргументов и возвращает кортеж из двух списков:  отрицательных значений (отсортирован по убыванию);  неотрицательных значений (отсортирован по возрастанию).
```
def split_numbers(*args):

    negative = []
    non_negative = []

    for num in args:
        if num < 0:
            negative.append(num)
        else:
            non_negative.append(num)

    # Сортировка отрицательных по убыванию (от -1 до -∞)
    negative.sort(reverse=True)
    # Сортировка неотрицательных по возрастанию (от 0 до ∞)
    non_negative.sort()

    return negative, non_negative


print(split_numbers(1, 3, -8, 0, -23))
```
# 2.1.15 Валовой внутренний продукт может определяться по расходам: Y=C+I+G+Ex−Im, где:  C - конечное потребление;  I - валовое накопление капитала (инвестиции в фирму, то есть покупка станков, оборудования, запасов, места производства);  G - государственные расходы;  Ex - экспорт;  Im - импорт. Все величины - целые числа. Выполните:  введите с клавиатуры все 5 значений формулы и сохраните их в список и словарь вида {'c': 3, 'i': 1, ..., 'im': 2}  напишите функцию для вычисления ВВП, которая содержит 5 параметров, соответствующих формуле;  используя распаковку аргументов, вызовите функцию с данными введенных списка и словаря и выведите их на экран.
```
def gdp(c, i, g, ex, im):

    return c + i + g + ex - im


# Ввод данных с клавиатуры
info_dict = {}
info_list = []

info_dict['c'] = int(input("Конечное потребление: "))
info_dict['i'] = int(input("Валовое накопление капитала: "))
info_dict['g'] = int(input("Государственные расходы: "))
info_dict['ex'] = int(input("Экспорт: "))
info_dict['im'] = int(input("Импорт: "))

# Сохраняем значения в список в том же порядке, что и параметры функции
info_list = [info_dict['c'], info_dict['i'], info_dict['g'], info_dict['ex'], info_dict['im']]

# Вычисление ВВП с использованием распаковки аргументов
result_from_list = gdp(*info_list)  # распаковка списка
result_from_dict = gdp(**info_dict)  # распаковка словаря

# Вывод результатов
print(f"ВВП = {result_from_list}")
print(f"ВВП = {result_from_list}")
```
# 2.1.16 Составьте две функции для возведения числа в степень: один из вариантов реализуйте в рекурсивном стиле.
```
def pow1(value, power):

    result = 1
    for _ in range(power):
        result *= value
    return result


def pow2(value, power):

    if power == 0:
        return 1
    elif power < 0:
        return 1 / pow2(value, -power)
    else:
        return value * pow2(value, power - 1)


print(pow1(2, 4))
print(pow2(2, 4))
```
# 2.1.17 Дано натуральное число. Напишите рекурсивные функции для определения:  суммы цифр числа;  количества цифр в числе.
```
def digits_sum(value):
    if value == 0:
        return 0
    else:
        return value % 10 + digits_sum(value // 10)


def digits_count(value):

    if value == 0:
        return 0
    else:
        return 1 + digits_count(value // 10)


print(digits_sum(43435))
print(digits_count(43435))
```
# 2.1.18 Разработайте рекурсивную функцию, которая вычисляет сумму цифр натурального числа. Например, для числа 12345 сумма будет 1+2+3+4+5 = 15.
```
def sum_digits_rec(number):

    if number == 0:
        return 0
    else:
        return number % 10 + sum_digits_rec(number // 10)


print(sum_digits_rec(36823))
print(sum_digits_rec(4536))
```
# 2.1.19 Напишите рекурсивную функцию для нахождения n-го числа в последовательности Фибоначчи. Последовательность Фибоначчи — это ряд чисел, в котором первые два числа равны 0 и 1, а каждое последующее число равно сумме двух предыдущих.
```
def fibonacci_rec(n):

    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci_rec(n - 1) + fibonacci_rec(n - 2)


print(fibonacci_rec(9))
print(fibonacci_rec(4))
```
# 2.1.20 Создайте рекурсивную функцию, которая переворачивает строку. Например, для строки "hello" результат должен быть "olleh".
```
def reverse_string_rec(s):

    if len(s) == 0:
        return s
    else:
        return reverse_string_rec(s[1:]) + s[0]


print(reverse_string_rec("hello"))
print(reverse_string_rec("ahahh"))
```
# 2.1.21 Напишите рекурсивную функцию, которая проверяет, является ли строка палиндромом. Палиндром — это строка, которая читается одинаково в обоих направлениях (например, "racecar").
```
def is_palindrome_rec(s):

    s = s.lower()

    if len(s) <= 1:
        return True
    elif s[0] == s[-1]:
        return is_palindrome_rec(s[1:-1])
    else:
        return False


print(is_palindrome_rec("sdfsg"))
print(is_palindrome_rec("fdbbdf"))
print(is_palindrome_rec("wefoe"))
```
# 2.1.22 Разработайте рекурсивную функцию для вычисления суммы всех элементов в списке чисел.
```
def sum_list_rec(numbers):

    if len(numbers) == 0:
        return 0
    else:
        return numbers[0] + sum_list_rec(numbers[1:])


my_list = [3, 2, 5, 3, 2, 7]
print(sum_list_rec(my_list))
print(sum_list_rec([]))
```
# 2.1.23 Напишите рекурсивную функцию, которая выводит на экран все числа от N до 1.
```
def print_numbers_down_rec(n):

    if n >= 1:
        print(n)
        print_numbers_down_rec(n - 1)


print_numbers_down_rec(23)
```
# 2.1.24 Реализуйте рекурсивную функцию для нахождения наибольшего общего делителя (НОД) двух чисел, используя алгоритм Евклида.
```

def gcd_rec(a, b):

    if b == 0:
        return abs(a)
    else:
        return gcd_rec(b, a % b)


print(gcd_rec(37, 2))
print(gcd_rec(153, 10))
```
# 2.1.25 Напишите рекурсивную функцию, которая преобразует десятичное число в его двоичное представление в виде строки.
```
def to_binary_rec(n):

    if n == 0:
        return "0"
    elif n == 1:
        return "1"
    else:
        return to_binary_rec(n // 2) + str(n % 2)


print(to_binary_rec(234))
print(to_binary_rec(434))
print(to_binary_rec(0))
```
# 2.1.26 Напишите рекурсивную функцию, которая "сплющивает" вложенный список, то есть делает из списка с подсписками один плоский список.
```
def flatten_list(nested_list):
    result = []
    
    for item in nested_list:
        if isinstance(item, list):
            result.extend(flatten_list(item))
        else:
            result.append(item)
    
    return result
```
# 2.1.27 Создайте рекурсивную функцию для подсчета количества вхождений определенного элемента в список.
```
def count_occurrences(lst, element):
  
    if not lst:
        return 0
    
    if lst[0] == element:
        return 1 + count_occurrences(lst[1:], element)
    else:
        return count_occurrences(lst[1:], element)
```
# 2.1.28 Реализуйте рекурсивную функцию digital_root(n), которая находит цифровой корень числа. Цифровой корень — это однозначное число, полученное путем итеративного процесса суммирования цифр. Например, для 65536: 6+5+5+3+6=25, затем 2+5=7. Цифровой корень — 7.
```
def digital_root_rec(n):

    if n < 10:
        return n
    else:
        # Суммируем цифры числа
        digit_sum = 0
        for digit in str(n):
            digit_sum += int(digit)
        # Рекурсивно находим цифровой корень суммы
        return digital_root_rec(digit_sum)


print(digital_root_rec(23453))
print(digital_root_rec(3242))
print(digital_root_rec(353))
```
# 2.1.29 Напишите рекурсивную функцию для нахождения максимального элемента в списке чисел.
```
def find_max_rec(numbers):

    if len(numbers) == 1:
        return numbers[0]
    else:
        # Находим максимум в оставшейся части списка
        max_rest = find_max_rec(numbers[1:])
        # Сравниваем первый элемент с максимумом оставшейся части
        return numbers[0] if numbers[0] > max_rest else max_rest


my_list = [23, 35, 25, 64, 53, 43]
print(find_max_rec(my_list))
```
# 2.1.30 Представьте, что вы рассчитываете сумму вклада с ежегодным начислением процентов. Напишите рекурсивную функцию, которая вычисляет итоговую сумму на счете через определенное количество лет.
```
def calculate_deposit_rec(principal, rate, years):

    if years == 0:
        return principal
    else:
        new_principal = principal * (1 + rate)
        return calculate_deposit_rec(new_principal, rate, years - 1)

final_amount = calculate_deposit_rec(2384, 0.04, 3)
print(f"{final_amount:.2f}")
```
# Вывод
Научились работать с функциями и подпрограммами в Python
