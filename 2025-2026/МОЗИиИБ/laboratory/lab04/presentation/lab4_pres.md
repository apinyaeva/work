# РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ
### Факультет физико-математических и естественных наук
### Кафедра прикладной информатики и теории вероятностей 


# ПРЕЗЕНТАЦИЯ
# ПО ЛАБОРАТОРНОЙ РАБОТЕ  № 4
### дисциплина: Математические основы защиты информации и информационной безопасности


### Студент: Пиняева Анна Андреевна
### Группа: НПИмд-01-24

### МОСКВА
### 2025 

-----------


# Цель работы

Изучение и реализация вычисления НОД с помощью алгоритма Еклида на языке `Julia`. 

-----------

# Ход работы
1. Алгоритм Евклида

```julia
function euclidean_gcd(a::Int, b::Int)
    a, b = abs(a), abs(b)
    while b != 0
        a, b = b, a % b
    end
    return a
end
```

-----------

2. Бинарный алгоритм Евклида

```julia
function binary_gcd(a::Int, b::Int)
    a, b = abs(a), abs(b)
    
    a == 0 && return b
    b == 0 && return a
    
    shift = 0
    while iseven(a) && iseven(b)
        a ÷= 2
        b ÷= 2
        shift += 1
    end
    
    while a != 0
        while iseven(a)
            a ÷= 2
        end
        while iseven(b)
            b ÷= 2
        end
        
        if a >= b
            a = a - b
        else
            b = b - a
        end
    end
    
    return b << shift  # b * 2^shift
end
```

________________
3. Расширенный алгоритм Евклида

```julia
function extended_euclidean(a::Int, b::Int)
    a, b = abs(a), abs(b)
    
    if b == 0
        return (a, 1, 0)
    end
    
    x0, x1 = 1, 0
    y0, y1 = 0, 1
    
    while b != 0
        q = a ÷ b
        a, b = b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    end
    
    return (a, x0, y0)
end
```

________

4. Расширенный бинарный алгоритм Евклида

```julia
function extended_binary_gcd(a::Int, b::Int)
    a, b = abs(a), abs(b)
    
    if a == 0
        return (b, 0, 1)
    elseif b == 0
        return (a, 1, 0)
    end
    
    g = 1
    while iseven(a) && iseven(b)
        a ÷= 2
        b ÷= 2
        g *= 2
    end
    
    u, v = a, b
    A, B, C, D = 1, 0, 0, 1
    
    while u != 0
        while iseven(u)
            u ÷= 2
            if iseven(A) && iseven(B)
                A ÷= 2
                B ÷= 2
            else
                A = (A + b) ÷ 2
                B = (B - a) ÷ 2
            end
        end
        while iseven(v)
            v ÷= 2
            if iseven(C) && iseven(D)
                C ÷= 2
                D ÷= 2
            else
                C = (C + b) ÷ 2
                D = (D - a) ÷ 2
            end
        end
        
        if u >= v
            u -= v
            A -= C
            B -= D
        else
            v -= u
            C -= A
            D -= B
        end
    end
    
    d = g * v
    return (d, C, D)
end
```

___________

5. Вывод результатов
```julia
function test_algorithms()
    test_cases = [
        (54, 24),
        (12345, 24690),
        (12345, 54321),
        (12345, 12541),
        (91, 105),
        (105, 154),
        (17, 13),  # Простые числа
        (100, 25), # Одно делит другое
        (0, 15),   # Ноль
        (15, 0),   # Ноль
        (-54, 24), # Отрицательные числа
        (54, -24)  # Отрицательные числа
    ]
    
    println("ТЕСТИРОВАНИЕ АЛГОРИТМОВ НОД")
    println("="^50)
    
    for (i, (a, b)) in enumerate(test_cases)
        println("\nТест $i: НОД($a, $b)")
        println("-"^30)
        
        # Алгоритм Евклида
        gcd1 = euclidean_gcd(a, b)
        println("Алгоритм Евклида: $gcd1")
 # Бинарный алгоритм
        gcd2 = binary_gcd(a, b)
        println("Бинарный алгоритм: $gcd2")
        
        # Расширенный алгоритм
        gcd3, x3, y3 = extended_euclidean(a, b)
        println("Расширенный алгоритм: $gcd3")
        println("Коэффициенты Безу: $a×$x3 + $b×$y3 = $(a*x3 + b*y3)")
        
        # Расширенный бинарный алгоритм
        gcd4, x4, y4 = extended_binary_gcd(a, b)
        println("Расшир. бинарный: $gcd4")
        println("Коэффициенты Безу: $a×$x4 + $b×$y4 = $(a*x4 + b*y4)")
        
        # Проверка согласованности
        if gcd1 == gcd2 == gcd3 == gcd4
            println("✓ Все алгоритмы дали одинаковый результат")
        else
            println("✗ Ошибка: результаты различаются!")
        end
        
        # Проверка взаимной простоты
        if gcd1 == 1
            println("✓ Числа взаимно просты")
        else
            println("Числа не взаимно просты")
        end
    end
end

function main()
    
    println()
    
    test_algorithms()  
end
main()

```
Результат тестирования представлен на рис.1 


*Рис. 1 Тестирование:*

![N|Solid](https://sun9-44.userapi.com/s/v1/if2/gNsn9zyMpSRSEiz2IRX3fXSSlRxNH0WzVA8eCTbnmI7F3HqA2g_4Lfx7nEMz3gUscTlNqDF4wKqBSQ0c0OHJifED.jpg?quality=95&as=32x29,48x43,72x65,108x98,160x144,240x217,360x325,480x433,540x488,640x578,720x650,1080x975,1280x1156,1360x1228&from=bu&u=SpgNRDveXATHxv2dzVwwbFYdfMFVj8HXt26G1-wyEms&cs=1360x0)



-----------

____________

Вывод: В ходе данной работы мной были изучены и реализованы различные вариации метода Евклида для вычисления НОД. Написан программный код на языке Julia и протестирован. 
