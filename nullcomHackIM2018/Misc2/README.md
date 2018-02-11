**Задание**

Find the transferred file

[challenge.pcapng](https://s3.amazonaws.com/hackim18/misc/pcap/challenge.pcapng)


-----


**Решение**

Откроем `challenge.pcapng` с помощью Wireshark

Посмотрим Statistics -> Protocol Hierarchy

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/statistics.jpg)

Видим, что есть обмен по HTTP. Для начала выгрузим файлы, которые передавались

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/objects.png)

Интересными показались файлы `follem.JPG` и `metloof.JPG` с одинаковой картинкой

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/metloof.JPG)

Проверим файлы на вариант стеганографии, `stegdetect` показал, что в файле `metloof.JPG` есть вложенный архив

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/stegdetect.png)

Выгрузим его с помощью `binwalk` и распакуем

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/binwalk.png)

Внутри архива находим файл `e2fc7ad1c912c04b0247cb9a710e82cd.txt` с содержанием `Flag isn't here!`

Продолжим поиски. Остальные файлы также не сожержат флаг.

Вернемся к `challenge.pcapng` и исследуем другие протоколы.
DNS и NTP выглядят штатно, а вот в ICMP есть интересные данные

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/icmp.png)

Выгрузим их с помощью `tshark` и приведем к бинарному виду

`tshark -Y "(data.len==2 ) && (icmp.type == 8)" -T fields  -r challenge.pcapng -e data | xxd -r -p |xxd -r -p > bin.dat`

Полученый файл `bin.dat` оказался архивом, содержащий файл `flag.txt`

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/tar.png)

Flag: `hackim18{'51mpL3st_Ch4ll3ng3_s0lv3d'}`

Альтернативное решение с помощью [CyberChef](https://gchq.github.io/CyberChef/)
На вход подаем данные, выгруженные `tshark` и соберем "рецепт"

![](https://github.com/ambalabanov/writeups/raw/master/nullcomHackIM2018/Misc2/cyberchef.png)


-----
