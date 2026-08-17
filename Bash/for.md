# for
 Что это -  цикл, который позволяет выполнить одну или несколько команд несколько раз для каждого значения из списка

## Синтаксис

```bash
for переменная in список
do
    команда
done
```

---

# Пример 1 
```bash
#!/bin/bash
for ip in 192.168.3.1 192.168.3.109 192.168.3.116
do
    ping -c1 "$ip"
done
```

# Пример 2 
```bash
#!/bin/bash
for ip in 192.168.3.1 192.168.3.109 192.168.3.116
do
    ping -c1 "$ip" > /dev/null
    if [ $?  -eq 0 ]; then
        echo " "$ip" - Хост доступен" 
    else
        echo " "$ip" - Хост недоступен"
    fi
done
```

# Пример 3
```bash
#!/bin/bash
for  ip in $(cat ips.txt)
do
    ping -c1 "$ip" > /dev/null
    if [ $?  -eq 0 ]; then
        echo " "$ip" - Хост доступен" 
    else
        echo " "$ip" - Хост недоступен"
    fi
done
```

# Пример 4 
```bash
file=$1

for ip in $(cat "$file" )
do
    echo "$ip"
done
```

# Пример 5
```bash
file=$1

for ip in $(cat "$file")
do
    ping -c1 "$ip" > /dev/null
    if [ $? -eq 0 ]; then
        echo " "$ip" - Хост доступен" 
    else
         echo " "$ip" - Хост недоступен"
    fi
done
```
