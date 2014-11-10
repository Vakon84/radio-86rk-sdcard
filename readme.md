# Адаптер SD-CARD для Радио-86РК_SRAM

Данный проект является развитием проекта адаптера SD-CARD  от **Vinxru** 
[http://vinxru.livejournal.com](http://vinxru.livejournal.com), исходный код проекта расположен по адресу [https://github.com/vinxru/86RKSD](https://github.com/vinxru/86RKSD)


## Изменения в схеме и печатной плате
- Добавлен LDO регулятор LM1117 3v3
- Вместо резистивных делителей используется буфер HEF4050BT
- добавлен 26-контактный IDC разъем под кабель для сопряжения модуля с Радио-86РК_SRAM
- Добавлен 6-контактный разъем для программатора
- Вместо Atmega8L используется МК Atmega328P

Плата выполнена в форм-факторе 5x5 см, что дает возможность заказать ее производство у китайцев. Пробная первая партия была заказа у [http://www.dfrobot.com](http://www.dfrobot.com).


## Фото готового устройства

![Фото](https://farm8.staticflickr.com/7503/15748066262_4a058c6886_c.jpg)

## Адаптация исходного кода

Автор оригинальной разработки подготовил файлы boot.rk, sdbios.rk и shell.rk для Радио-86РК (меняются, как минимум, адреса портов).

Прошивка микроконтроллера Atmega8L остается без изменений, можно спокойно заливать авторскую.

Если же проект собирается на базе Atmega328P, то прошивку можно взять из данного репозитария, в директории *firmware/src/atmega328p/Exe*. Вся соль не только в изменившихся адресах портов этого камня, а также в смещении переменной rom, которая в оригинальной конфигурации сидела по адресу 0100h, а теперь сидит по адресу 0200h. 

Также оказалось, что коммандер **shell.rk** не совсем корректно работает с *адаптером PS/2 клавиатуры*, который при вызове функции биоса **getch()** возвращает странные значения для кнопок курсора. Соответственно, пришлось придумать альтернативное решение управления курсором, а именно: клавиши **O,A,O,P**. Модифицированная версия shell.rk расположена в директории *firmware/src/shell_rk/*.

### Fuse bits для МК

На сайте [http://www.engbedded.com/fusecalc/](http://www.engbedded.com/fusecalc/) можно посчитать fuse bits для Вашего МК. 
Необходимо выбрать 

- Internal RC OSC 8MHz, 
- убрать галочку Divide clock by 8 internally


Все остальное можно не трогать или по желанию (например параметры brown out detection).

