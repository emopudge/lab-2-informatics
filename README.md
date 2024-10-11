# Отчет по лабораторной работе 2

## Цель
Написать скрипт, который принимает IP-адрес в десятичном формате и выводит его в двоичном формате.

## Код скрипта
![image](https://github.com/user-attachments/assets/4e946346-13af-4228-bf79-aa20057caaaa)
![image](https://github.com/user-attachments/assets/1086f857-7d48-4c64-8d24-b9b0b7e22402)

```
#!/bin/bash

# Запрашивает у пользователя ввод IP-адреса
read -p "input your ip: " ip 

# Устанавливает точку (.) в качестве разделителя для разделения IP-адреса на октеты
IFS="." 

# Разбивает введенный IP-адрес на массив "mass", где каждый элемент - октет
mass=($ip) 

# Инициализирует счетчик "count", который будет использоваться для проверки корректности IP-адреса
count=0 

# Проходит по всем октетам IP-адреса в массиве "mass"
for el in ${mass[@]}; do
  # Проверяет, не превышает ли октет значение 256 и является ли количество октетов равным 4
  if [ $el -gt 256 ] || [ 4 -ne ${#mass[@]} ]; then
    # Если найдено некорректное значение, увеличивает счетчик "count"
    count=$((count + 1))
  fi
done

# Проверяет, был ли найден хотя бы один некорректный октет в IP-адресе
if [ $count -ne 0 ]; then
  # Выводит сообщение о некорректном IP-адресе
  echo "wrong ip"
else
  # Преобразует каждый октет в двоичный формат с помощью команды "bc"
  for i in "${!mass[@]}"; do
    mass[$i]=$(bc <<< "obase=2; ibase=10; ${mass[$i]}")
  done

  # Выводит двоичное представление IP-адреса
  for i in  "${!mass[@]}"; do
    # Проверяет, является ли текущий октет последним в IP-адресе
    if [ $i -eq 3 ]; then
      # Если это последний октет, выводит его без точки
      echo  "${mass[3]}"
    else
      # Если это не последний октет, выводит его с точкой
      echo -n "${mass[$i]}."
    fi
  done
fi
```

**Описание кода:**

Этот скрипт принимает IP-адрес от пользователя, проверяет его на корректность и выводит его двоичное представление.

1. **Ввод IP-адреса:** Скрипт запрашивает у пользователя ввод IP-адреса с помощью ``` read -p "input your ip: " ip.```
2. **Разделение IP-адреса на октеты:** IP-адрес разбивается на отдельные октеты с помощью ```IFS="." и mass=($ip).```
3. **Проверка корректности IP-адреса:** Скрипт проверяет, не превышает ли значение каждого октета 256 и не равно ли количество октетов 4. Если найдено некорректное значение, скрипт выводит сообщение ```"wrong ip".```
4. **Преобразование октетов в двоичный формат:** Скрипт преобразует каждый октет в двоичный формат с помощью команды``` bc.```
5. **Вывод двоичного представления:** Скрипт выводит двоичное представление IP-адреса, разделяя октеты точками. Последний октет выводится без точки.

**Пример использования:**

```
Введите ваш IP-адрес: 192.168.1.1
11000000.10101000.1.1
```
![image](https://github.com/user-attachments/assets/e5a905c3-5656-4570-b800-eb2721f345b7)


### Основные концепции
- **IP-адрес**: Это уникальный идентификатор устройства в сети. IPv4 состоит из 4 частей, каждая из которых принимает значения от 0 до 255.
  
- **Маска подсети**: Используется для определения сетевой части IP-адреса. Например, маска 255.255.255.0 соответствует `/24`, где первые 24 бита используются для идентификации сети, а оставшиеся 8 - для идентификации узлов в сети.


