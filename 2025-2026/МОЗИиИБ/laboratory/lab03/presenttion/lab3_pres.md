# РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ
### Факультет физико-математических и естественных наук
### Кафедра прикладной информатики и теории вероятностей 


# ПРЕЗЕНТАЦИЯ
# ПО ЛАБОРАТОРНОЙ РАБОТЕ  № 3
### дисциплина: Математические основы защиты информации и информационной безопасности


### Студент: Пиняева Анна Андреевна
### Группа: НПИмд-01-24

### МОСКВА
### 2025 

-----------


# Цель работы

Изучение и реализация шифрования гаммированием на языке `Julia`. 

-----------


# Ход работы
1. Функция получения номера буквы

```julia
alphabet=collect("АБВГДЕЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ")

function letter(c::Char)
    pos = findfirst(isequal(c), alphabet)
    if pos === nothing
        error("Буквы в алфавите нет")
    end
    return pos
end
```

-----------


2. Функция получения буквы по номеру

```julia
function num(n::Int)
    if n ==0
        n=32
    elseif n<1 || n>32
        error("Номер не в диапазоне")
    end
    return alphabet[n]
end
```

-----------


3. Функция шифрования

```julia
function gamma(message::String, key::String, mod_val::Int=32)
    p_nums = [letter(c) for c in message]
    k_nums=[letter(c) for c in key]
    k_len=length(k_nums)

    c_nums=Vector{Int}(undef, length(p_nums))
    for i in 1:length(p_nums)
        ki=k_nums[(i-1) % k_len+1]
        c_nums[i]=(p_nums[i] + ki) % mod_val
        if c_nums[i] ==0
            c_nums[i]=mod_val
        end
    end

    return join([num(n) for n in c_nums])
end
```
-----------


4. Функция дешифрования

```julia
function gamma_de(ciphertext::String, key::String, mod_val::Int=32)
    c_nums=[letter(c) for c in ciphertext]
    k_nums=[letter(c) for c in key]
    k_len=length(k_nums)

    p_nums=Vector{Int}(undef, length(c_nums))
    for i in 1:length(c_nums)
        ki=k_nums[(i-1) % k_len+1]
        p_nums[i]=(c_nums[i] - ki) % mod_val
        if p_nums[i] ==0
            p_nums[i]=mod_val
        end
    end
    return join([num(n) for n in p_nums])
end
```

-----------


5. Вывод результатов
```julia
message="ПРИКАЗ"
key="ГАММА"
encrypted=gamma(message,key)
decrypted=gamma_de(encrypted, key)

println("открытый текст: $message")
println("ключ: $key")
println("зашифрованный текст: $encrypted")
println("расшифрованный текст: $decrypted")
```
Результат тестирования представлен на рис.1 


*Рис. 1 Тестирование шифрования гаммированием:*

![N|Solid](https://sun9-79.userapi.com/s/v1/if2/B6E2Ya8Cy7vp_rDhaKc39Xcbe-VtRJuk_yZlk4tfib8CbXvQOMc3bSPP_63T7Kw1C_Tg6yidXr0JflFfoqNJC39u.jpg?quality=95&as=32x5,48x8,72x12,108x18,160x26,240x39,360x59,480x78,540x88,640x104,720x117,1080x176,1280x209,1440x235,1900x310&from=bu&cs=1900x0)



-----------

Вывод: В ходе данной работы мной был изучен и реализован метод шифрования гаммированием.   Написан программный код на языке Julia и протестирован. 
