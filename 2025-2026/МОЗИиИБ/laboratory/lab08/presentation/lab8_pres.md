# РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ
### Факультет физико-математических и естественных наук
### Кафедра прикладной информатики и теории вероятностей 


# Презентация
# ПО ЛАБОРАТОРНОЙ РАБОТЕ  № 8
### дисциплина: Математические основы защиты информации и информационной безопасности


### Студент: Пиняева Анна Андреевна
### Группа: НПИмд-01-24

### МОСКВА
### 2025 

_____

# Цель работы

Ознакомиться с алгоритмами и реализовать их. 

_________

# Ход работы
## 1. Сложение неотрицательных целых чисел

```julia
function sum_accurate(u, v, b=10)
    k = 0
    u_str = parse.(Integer, only.(split(string(u), ""))); v_str = parse.(Integer, only.(split(string(v), "")))
    n_u = length(u_str); n_v = length(v_str)
    j = max(n_u, n_v)
    w = zeros(Int64, j+1)
    if n_u < n_v
        temp = zeros(Int64, j)
        temp[n_v - n_u + 1:j] = u_str
        u_str = [i for i in temp]
    elseif n_v < n_u
        temp = zeros(Int64, j)
        temp[n_u - n_v + 1:j] = v_str
        v_str = [i for i in temp]
    end
    while j != 0
        k_temp = (u_str[j] + v_str[j] + k) % b
        w[j+1] = k_temp
        k = round(Int, (u_str[j] + v_str[j] + k - k_temp) / b)
        j -= 1
    end
    w[1] = k
    return parse(Int, join(string.(w)))
end 
```

_________


## 2. Вычитание неотрицательных целых чисел
```julia
function raz_accurate(u, v, b=10)
    if u < v
        return string(u) * " should be greater than " * string(v)
    end
    k = 0
    u_str = parse.(Integer, only.(split(string(u), ""))); v_str = parse.(Integer, only.(split(string(v), "")))
    n_u = length(u_str); n_v = length(v_str)
    j = max(n_u, n_v)
    w = zeros(Int64, j)
    if n_v < n_u
        temp = zeros(Int64, j)
        temp[n_u - n_v + 1:j] = v_str
        v_str = [i for i in temp]
    end
    while j != 0
        if u_str[j] < v_str[j]
            k = b
            u_str[j-1] -= 1
        else
            k = 0
        end
        k_temp = (u_str[j] - v_str[j] + k) % b
        w[j] += k_temp
        j -= 1
    end
    return parse(Int, join(string.(w)))
end 
```

________

## 3. Умножение неотрицательных целых чисел столбиком¶
```julia
function umn_accurate(u, v, b=10)
    k = 0
    u_str = parse.(Integer, only.(split(string(u), ""))); v_str = parse.(Integer, only.(split(string(v), "")))
    i = length(u_str); j = length(v_str)
    w = zeros(Int64, i + j)
    while j > 0
        i = length(u_str)
        k = 0
        while i > 0
            k_temp = u_str[i] * v_str[j] + w[i+j] + k
            w[i+j] = k_temp % b
            k = round(Int, (k_temp - w[i+j]) / b)
            i -= 1
        end
        w[j] = k
        j -= 1
    end
    return parse(Int, join(string.(w)))
end
```

_______

## 4. Быстрый столбик

```julia
function umn_fast(u, v, b=10)
    u_str = parse.(Integer, only.(split(string(u), ""))); v_str = parse.(Integer, only.(split(string(v), "")))
    n = length(u_str); m = length(v_str)
    w = zeros(Int64, n + m)
    t = 0
    for s in 0:m+n-1
        for i in 0:s
            if n-i <= 0 || m-s+i <= 0
                continue
            end
            t += u_str[n-i] * v_str[m-s+i]
        end
        w[n+m-s] = t % b
        t = round(Int64, (t - w[n+m-s]) / b)
    end
    return parse(Int, join(string.(w)))
end
```

_______


### Тестирование
```julia
sum_accurate(12533,989)
raz_accurate(12533,989)
umn_accurate(12533,989)
umn_fast(12533,989)
```


Результат тестирования представлен на рис.1 



![*Рис. 1 Тестирование:*](https://sun9-23.userapi.com/s/v1/ig2/QNYS12Eek5gWlWZRO4K7ZZAfVmRAZ9JndSq7cPcGI2rn3cLhbUvN_t4rEphcvtHWTmZH18xmSlv7JjQtEr4pysVz.jpg?quality=95&as=32x25,48x38,72x57,108x85,160x126,240x189,360x284,480x379,540x426,640x505,720x568,1080x852,1280x1010,1384x1092&from=bu&u=KhuVJVrxMrKRYWWuhuBU1iotOLE3S1m2tQhV80J4RXM&cs=1384x0)

________-



#### Вывод: В результате работы мы ознакомились с алгоритмами и реализовали их на языке программирования `Julia`.


