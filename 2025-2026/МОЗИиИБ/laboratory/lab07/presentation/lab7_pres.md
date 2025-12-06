### Факультет физико-математических и естественных наук
### Кафедра прикладной информатики и теории вероятностей 


# ПРЕЗЕНТАЦИЯ
# ПО ЛАБОРАТОРНОЙ РАБОТЕ  № 7
### дисциплина: Математические основы защиты информации и информационной безопасности


### Студент: Пиняева Анна Андреевна
### Группа: НПИмд-01-24

### МОСКВА
### 2025 
______

# Цель работы

Ознакомиться с алгоритмом дискретного логарифмирования в конечном поле.

_____

# Ход работы
## Реализовать алгоритм дискретного логарифмирования в конечном поле

```julia
function searching_for_gamma(a_diff, b_diff, p)
    for i in 1:p
        if b_diff*i %p == a_diff
            return i
        end
    end
    return "Not found"
end 
function new_xab(x, a, b, p, alph, bett)
    if x % 3 == 0
        return x^2 % p, a*2 % (p-1), b*2 % (p-1)
    elseif x % 3 == 1
        return x*alph % p, (a+1) % (p-1), b
    else
        return x*bett % p, a, (b+1) % (p-1)
    end
end
function metodPollarda(p, alp, bet)# , any_func::Function)
    if p % 2 == 0
        return "Incorrect input: p must be simple"
    end
    a_i = 0; b_i = 0; x_i = 1
    a_2i = 0; b_2i = 0; x_2i = 1
    i = 1
    tries = 1000
    data = zeros(Int64, (3, tries))
    data2 = zeros(Int64, (3, tries))
    while i <= tries
        x_i, a_i, b_i = new_xab(x_i, a_i, b_i, p, alp, bet)
        data[:, i] = [x_i, a_i, b_i]
        
        x_2i, a_2i, b_2i = new_xab(x_2i, a_2i, b_2i, p, alp, bet)
        x_2i, a_2i, b_2i = new_xab(x_2i, a_2i, b_2i, p, alp, bet)
        data2[:, i] = [x_2i, a_2i, b_2i]

        if x_i == x_2i
            display(data[:, 1:i])
            display(data2[:, 1:i])
            r = b_2i - b_i
            if r == 0
                return "Не найдено"
            else
                return searching_for_gamma(a_i - a_2i, r, p)
            end
        end
        i += 1
    end
    return "Делитель не найден"
end
```

______

### Тестирование
```julia
p=107
alp=10
bet=64
metodPollarda(p,alp,bet)

p=1011
alp=7
bet=3
metodPollarda(p,alp,bet)
```
Результат тестирования представлен на рис.1 



![*Рис. 1 Тестирование:*](https://sun9-63.userapi.com/s/v1/ig2/r5c29avD8J6t-v8HpeCVm7HUXfFLgxshHmNmtIkIym2rdjuKVg-zo19VJahdUggJ8UepZPCOW_iNRDpjBEaU8U-_.jpg?quality=95&as=32x22,48x34,72x51,108x76,160x112,240x168,360x253,480x337,540x379,640x449,720x505,1080x758,1280x898,1440x1011,1556x1092&from=bu&cs=1556x0)

_______

Вывод: В результате работы мы ознакомились с алгоритмом дискретного логарифмирования в конечном поле и реализовали его на языке программирования `Julia`.


