# STM8S-VUSB-KEYBOARD

Проект основан на исходниках Дениса Железнякова. Ссылки: http://ziblog.ru/  https://github.com/ZiB/STM8S-USB

В проекте реализованы:
- USB стэк (с ограничениями)
- универсальная USB HID клавиатура (3 страницы скан-кодов)
- антидребезговый фильтр на клавиши
- возможность работы от внешнего кварца на 16 МГц, и от внутреннего RC генератора с подстройкой

Недостатки:
- Алгоритм не умеет вырезать незначащие биты на приёме, поэтому, если USB хост назначает девайсу на шине адрес 0xXF, девайс не способен корректно принимать пакеты, и отвечает хосту в духе "моя твоя не понимать", после чего хост сбрасывает девайс, и назначает ему другой адрес. Для юзера это проходит незаметно.
- Пины порта С можно использовать только как GPIO входы.

Среда разработки - STVD 4.3.11

Компилятор - Cosmic cross compiler v.4.4.7

Схема: https://github.com/BBS215/STM8S-VUSB-KEYBOARD/blob/master/SCHEME/Scheme.pdf

При запуске прошивки, следующие настройки OPTION BYTE записываются автоматически:
1) AFR0: PORT C7 Alternate Function = TIM1_CH2
2) HSITRIM: 4 bit on-the-fly trimming

Для использования HSI, нужно установить #define USB_CLOCK_HSI в 1. После прошивки, контроллер будет пробовать подключиться к USB хосту при разных значениях HSI_TRIM_VAL, с периодичностью 8 секунд. При удачном подключении, рабочее значение HSI_TRIM_VAL будет сохраненно в EEPROM. Это значение в дальнейшем будет использоваться при перезапуске контроллера. Для повторной настройки HSI_TRIM_VAL, запишите нули в первые 2 байта EEPROM и перезапустите контроллер.


