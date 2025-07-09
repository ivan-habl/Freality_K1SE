# 📑 Проект Freality K1SE

## 📘 Оглавление

[1. Введение](#1-введение)  
[2. Делаем тензомод](#2-делаем-тензомод)  
[3. Установка debian 12](#3-установка-debian-12)  
[4. Установка kiauh](#4-установка-kiauh)  
[5. Прошивка трех mcu](#5-прошивка-трех-mcu)  
[6. Конфигурация принтера](#6-конфигурация-принтера)  
[7. Установка библиотек в виртуальное окружение klipper](#7-установка-библиотек-в-виртуальное-окружение-klipper)

---

## 1. Введение

В инструкции описывается перевод принтера K1SE на vanila klipper через установку debian 12 и тензомод (тип датчика оси Z - `load_cell_probe`).

## 2. Делаем тензомод

Согласно п.1 инструкции https://github.com/cryoz/K1_tenso_manual/blob/main/README.md

## 3. Установка debian 12

Для установки чистого debian 12 нужно проделать операции согласно инструкции:  
https://github.com/k1-debian/images/blob/master/README.md

После установки выполняем:

```bash
sudo apt update && sudo apt upgrade
```

> ⚠️ eMMC материнской платы принтера **полностью очистится!**

## 4. Установка kiauh

```bash
cd ~ && git clone https://github.com/dw-0/kiauh.git
```

-   В конфиге kiauh добавляем источник получения klipper:
    в файле /home/printer/kiauh/kiauh.cfg приводим секцию к виду

```[klipper]
repositories:
    https://github.com/Klipper3d/klipper, master
    https://github.com/cryoz/klipper, dev-debian
```

-   В настройках kiauh **выбираем добавленный репозиторий** вместо стандартного

Устанавливаем:

-   klipper
-   moonraker
-   fluidd
-   klipperscreen

## 5. Прошивка трех mcu

```bash
cd ~/klipper
./buld.sh
```

В папке klipper/outfw появятся файлы прошивок

```bash
cd ~
git clone https://github.com/Fiddl3/k1_mcu_flasher.git
cd k1_mcu_flasher

## 1) Во fluidd делаем FIRMWARE RESTART

## и в течение 15 секунд, пока mcu в загрузчике

sudo systemctl stop klipper

python3 mcu_util.py -c -i /dev/ttyS7 -u -f ~/klipper/outfw/mcu_klipper.bin

## 2) Во fluidd делаем RESTART KLIPPER

## Во fluidd делаем FIRMWARE RESTART

##  и в течение 15 секунд, пока mcu в загрузчике

sudo systemctl stop klipper

python3 mcu_util.py -c -i /dev/ttyS9 -u -f ~/klipper/outfw/bed_klipper.bin

## 3) Во fluidd делаем RESTART KLIPPER

## Во fluidd делаем FIRMWARE RESTART

## и в течение 15 секунд, пока mcu в загрузчике

sudo systemctl stop klipper

python3 mcu_util.py -c -i /dev/ttyS1 -u -f ~/klipper/outfw/noz_klipper.bin
```

Новые бинарники поддерживают вход в загрузчик по команде и далее прошивать можно по команде

```bash
sudo systemctl stop klipper

python3 mcu_util.py -b -br 921600 -i /dev/ttyS7 -u -f ~/klipper/outfw/mcu_klipper.bin -v

python3 mcu_util.py -b -br 921600 -i /dev/ttyS9 -u -f ~/klipper/outfw/bed_klipper.bin -v

python3 mcu_util.py -b -br 921600 -i /dev/ttyS1 -u -f ~/klipper/outfw/noz_klipper.bin -v
```

## 6. Конфигурация принтера

Копируем через `fluidd` или scp конфигурационные файлы принтера из папки `config` этого репозитория.

Меняем в loadcell_probe.cfg пины настройки канала тензомода https://github.com/cryoz/K1_tenso_manual/blob/main/README.md#42-%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B3%D1%83%D1%80%D0%B0%D1%86%D0%B8%D1%8F

## 7. Установка библиотек в виртуальное окружение klipper

Устанавливаем пакеты wheels:

Нам нужно установить в виртуальное окружение klippy-env предварительно кросс-скомпилированные библиотеки для поддержки визуализации частотных характеристик: numpy matplotlib scipy (на плате принтера они компилировались бы сутками).

```bash
cd ~
git clone https://github.com/ivan-habl/Freality_K1SE.git
sudo apt update
sudo apt install libblas-dev liblapack-dev
source ~/klipper/klippy-env/bin/activate
pip install --no-index --no-deps \
    --find-links=~/Freality_K1SE/wheels \
    numpy matplotlib scipy
```
