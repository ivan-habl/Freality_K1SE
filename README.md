# 📑 Проект Freality K1SE

## 📘 Оглавление

[1. Введение](#1-введение)  
[2. Установка debian 12](#2-установка-debian-12)  
[3. Установка kiauh](#3-установка-kiauh)  
[4. Конфигурация принтера](#4-конфигурация-принтера)  
[5. Установка библиотек в виртуальное окружение klipper](#5-установка-библиотек-в-виртуальное-окружение-klipper)  

---

## 1. Введение

В инструкции описывается перевод принтера K1SE на vanila klipper через установку debian 12 и тензомод (тип датчика оси Z - `load_cell_probe`).

## 2. Установка debian 12

Для установки чистого debian 12 нужно проделать операции согласно инструкции:  
https://github.com/k1-debian/images/blob/master/README.md

После установки выполняем:

```bash
sudo apt update && sudo apt upgrade
```

> ⚠️ eMMC материнской платы принтера **полностью очистится!**

## 3. Установка kiauh

```bash
cd ~ && git clone https://github.com/dw-0/kiauh.git
```

В конфиге kiauh добавляем источник получения klipper:  
https://github.com/cryoz/klipper/tree/dev-debian

Устанавливаем:
- klipper
- moonraker
- fluidd
- klipperscreen

## 4. Конфигурация принтера

Копируем через `fluidd` конфигурационные файлы принтера из папки `config`.

## 5. Установка библиотек в виртуальное окружение klipper

Устанавливаем пакеты:

```bash
pip install numpy matplotlib scipy
```

(лучше использовать заранее собранные `wheels`)
