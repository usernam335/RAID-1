#Создание RAID-1

1. **Комманда для создания и проверки состояния RAID 1 массива:**

<img width="1074" height="635" alt="fdisk_2" src="https://github.com/user-attachments/assets/34757233-6020-46e1-99c2-1db7408399da" />


    mdadm --create md127 -l 1 -n 2 /dev/sdb /dev/sdc
mdadm - утилита для работы с программными RAID-массивами.
--create - создать RAID массив.




md127 - имя массива.




-l 1 - уровень, 1.




-n 2 - количество устройств, 2.
        
    cat /proc/mdstat
  
* При создании массива я назвал его md01, но он получил имя md127, потому что система не всегда строго следует тому имени, которое вы задали при создании массива - иногда она сама назначает «внутреннее» имя, и md127 - один из таких вариантов.

**2. Команда для просмотра более подробной информации о массиве:**

  <img width="729" height="561" alt="fdisk_3" src="https://github.com/user-attachments/assets/2676a62f-c764-4450-81ee-f6a8c19a1adb" />

     mdadm -D /dev/md127
-D - сокращение от --detail



#Создание файловой системы в массиве

**1. Команда для создания файловой систему ext4:**

<img width="806" height="258" alt="fdisk_4" src="https://github.com/user-attachments/assets/a2bc1e8a-bea0-4709-8b15-044b49e00ede" />

    mkfs.ext4 /dev/md127

   #Монтирование массива к диску /mnt/01:
<img width="475" height="93" alt="fdisk_5" src="https://github.com/user-attachments/assets/db7f8370-b2a8-4ff6-9676-8411cd1ecc95" />

   **1.Команда для монтирования:**
         
         mount /dev/md127 /mnt/01

# Проверка массива:

**1.Команда для проверки дискового пространства:**
    

<img width="641" height="208" alt="fdisk_6" src="https://github.com/user-attachments/assets/be0886ce-e5b4-4f9e-bd36-cd1903fd4b8e" />

df -hT

df (Disk Free) — показывает занятое и свободное место на разделах.


-h (Human-readable) — выводит размеры в ГБ, МБ, КБ вместо блоков (например, 10G вместо 10485760).


-T (Type) — добавляет колонку с типом файловой системы (например, ext4, xfs, ntfs, tmpfs).

#Удаление и замена диска в массиве:

**1. Команда для пометки диска сбойным:**

<img width="545" height="45" alt="fdisk_7" src="https://github.com/user-attachments/assets/652199aa-2bf6-40b4-813d-7402794c1969" />

<img width="716" height="575" alt="fdisk_7 1" src="https://github.com/user-attachments/assets/b1eb5742-b1ec-46fb-b1fb-d3b64c70bd42" />

       mdadm /dev/md127 --fail /dev/sdb
       
**2. Команда для удаления диска из массива:**

<img width="720" height="590" alt="fdisk_8" src="https://github.com/user-attachments/assets/caf22947-7523-4929-81ae-90b4b479a2e5" />


    mdadm /dev/md127 --remove /dev/sdb

**2.1 Проверяем состояние массива:**

    mdadm -D /dev/md127    

**3. Команда для добавления нового диска в массив:** 

<img width="602" height="44" alt="fdisk_9" src="https://github.com/user-attachments/assets/a177abb5-51b4-43f7-958e-9262229112a5" />



        mdadm /dev/md127 --add /dev/sdd

**3.1 Проверяем состояние массива:**

<img width="797" height="523" alt="fdisk_10" src="https://github.com/user-attachments/assets/ee7af073-bc2d-41e6-a22c-4c4efd375ac0" />
     
      mdadm -D /dev/md127    
    
