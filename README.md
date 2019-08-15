# HoverCAR
![alt text](https://user-images.githubusercontent.com/12184628/62218561-60714800-b3b5-11e9-8184-1faa804bc68f.GIF)
![alt text](https://user-images.githubusercontent.com/12184628/62303317-94676e80-b484-11e9-9b25-ae05e1663225.GIF)
![alt text](https://user-images.githubusercontent.com/12184628/62218568-6404cf00-b3b5-11e9-97c1-1cb046346676.GIF)
![alt text](https://user-images.githubusercontent.com/12184628/62214088-02d8fd80-b3ad-11e9-8853-05cbcfa491de.JPG)
# Компоненты: 

Контроллер гироскутера. На процессоре STM32.  
2 Мотор колеса.  
АКБ + зарядное.  
Программатор ST-link v2 для прошивки платы.  
Ардуино уно.  
DC-DC конвертор для понижения с 15v до 9v.  
Логический преобразователь 5v - 3.3v.  
Модуль на 2 реле - для поворота руля.  
Радио Аппаратура с PWM. Я использовал FlySky CT6B. Так же есть возможность изменения чувствительности верхними рычажками. Проверено - проблем нет. С остальными возможны проблемы.   

Различные провода dupont пап-мама, мама-мама, папа-папа и просто провода силовые.  
Разьем для питания ардуино через гнездо.
Кнопка тактовая для включения и разъем зарядки изъял из корпуса гироскутера.  
Соединители Ваги, термоусадки, стяжки и т.п.  
Рама гироскутера - пригодится для создания кастомного крепления мотор колес. Можно вырезать болгаркой квадраты крепления мотор колес и использовать в своих целях.  

!!!ССЫЛКИ --->>>> на данные детали приложу в самом низу Инструкции, гироскутер можно взять б/у на авито =)  

# Программы и файлы
Все файлы для прошивки можно скачать в общем архиве - https://github.com/vfear/HoverCAR/blob/master/FILES.rar  
ST-LINK Utility для заливки модифицированной прошивки в контроллер гироскутера.  
Ссылка для скачивания https://www.st.com/en/development-tools/stsw-link004.html (необходимо пройти регистрацию)  
Arduino ide для прошивки контроллера arduino https://www.arduino.cc/download_handler.php  
Файл bin - прошивка для платы гироскутера. https://github.com/vfear/HoverCAR/blob/master/hover.rar  
Скетч для ардуино. https://github.com/vfear/HoverCAR/blob/master/hover-ide.ino  

Для калибровки пульта - программа T6config. (Добавлю в архив)  
Ссылка с оф. сайта https://www.flysky-cn.com/s/CT6B.rar  
Желательно пульт подключить к ПК и выставить диапазоны газа на максимальные отклонения 120%  
Проверить калибровки нейтральных положений. Все выставить в нейтраль максимально точно.  
  
# Подготовка платы к прошивке
 
Находим 4 контакта для программирования  
![alt text](https://user-images.githubusercontent.com/12184628/62279402-b0521c80-b452-11e9-91b6-ad3b4ea77c9f.jpg)  
Подпаиваемся согласно схеме выше. Эти контакты на картинке подписаны, как SWD Programming  
Готовим программатор ST-Link v2  
Подключаем провода к пинам SWCLK, SWDIO, GND, 3.3V  

# Прошивка

Устанавливаем все выше перечисленные программы на ПК.  
Прошивка платы:  
Запускаем STM32 ST-LINK Utility.  
Подключаем ST-Link v2 к USB вашего ПК.  
Нажимаем на кнопку Connect.  
Далее (Если вы прошиваете плату первый раз) Идем: Target -> Option bytes, там выбираем в поле Read out protection "disabled" и нажимаем apply. Родная прошивка при этом стирается.  
Далее идем во вкладку file-> Open file и выбираем наш файл прошивки "hover.bin"  
Далее возвращаемся к вкладке Target -> выбираем Program&Verify ничего не меняя, соглашаемся и прошиваем.  
После окончания прошивки нажимаем Disconnect и выходим из программы. Отсоединяем плату и можно отпаять провода.  
Наша плата прошита!  

Теперь займемся Ардуино:  
Подключаем Ардуино к USB вашего ПК.  
Запускаем Arduino IDE.  
Идем инструменты -> плата и проверяем, что бы плата соответствовала вашей,у меня например arduino uno.  
Проверяем порт, идем инструменты -> порт (порт присваивается Windows, у всех по разному обычно нижний)  
Далее можете либо открыть hover-ide.ino, либо взять код из текстового файла который я приложил и вставить в поле редактирования кода, предварительно очистив его.  
И наконец нажимаем кнопку со стрелкой - загрузка.  
После окончания процесса загрузки прошивки закрываем программу и отсоединяем контроллер.  
Ардуино готов!  

И не забудьте проверить калибровку пульта, как я писал в самом начале. (Рекомендуется)  

# Подключение

Связь ардуино с контроллером гироскутера осуществляется по UART.  
Двумя проводами от RX на ардуино к TX на плате гироскутера и TX ардуино к RX платы (Смотри общюю схему - левый уарт платы гироскутера (LEFT SENSOR BOARD) именно левый!).  
Подключать UART необходимо через логический преобразователь 5v - 3.3v так как логика платы гироскутера работает на 3.3в, а ардуино на 5в.  
С того же левого уарта берем питание ("-" GND берем с аккумулятора! провод желательно получше), там 15в - понижаем купленной DC-DC понижайкой до 9v, для питания ардуино.  
Подпаиваем к выходу стабилизатора штекер под гнездо питания ардуино и подключаем.
Выход 5v из ардуино разводим на питание приемника и питание логики рулевого реле.  
Входы ардуино 2,3 вход каналов приемника. Подключаем во второй канал и четвертый приемника.  
Выходы ардуино 7,8 управление на реле. (подключаем к логике реле)
Выходы 0,1 RX TX

Реле у меня управляет заводским мотором рулевого редуктора.   
Аккумулятор для рулевого я оставил родной.   
Подключил так - минус от свинцового аккумулятора 6v на NC1 NC2 входы реле, плюс на NO1 NO2, а на мотор выходит из COM1 и COM2.  
Полярность меняется программно и мотор крутит в нужную сторону.  
# СХЕМА
![alt text](https://user-images.githubusercontent.com/12184628/62285827-193f9180-b45f-11e9-889a-c86e35f25e6d.jpeg)

# Примечания

Если вы разбираете готовый гироскутер, проверьте фазные провода которые идут к колесам - иногда они идут не по цветам.   Зафиксируйте себе эту информацию, что бы потом не путаться при подключении.  

Если выбираете бу гироскутер, есть большая вероятность что вам попадется модель, которую невозможно прошить. Например это могут быть модели с двумя платами без основной, на процессоре другого типа или самый непонятный вариант, двухплатник меньшей мощности на 24v.  

Я думаю, что нужно выбирать модели не младше 18 года.  
Смотреть, параметры зарядного блока - что бы было указано 42v.  
Модели со встроенной блютус колонкой.  
6.5 дюймов колеса (даже мощнее чем большие).  
Может кому попадется фирмы eboard, черные 6.5 с блютусом - 3 штуки таких покупали все подходят.  
Я лично выбирал по этим параметрам и все было хорошо. Но шанс нарваться, я думаю есть всегда) 

Как определить STM32 или нет: что бы узнать подходит ли ваш контроллер для перепрошивки, можно посмотреть на маркировку на самом процессоре (должно быть написано stm32...). 


# Итоги

Я безумно доволен полученным результатом.  
Тяги у моторов хватает с большим запасом минимум 120 кг.   
Высокая скорость.   
Плавность управления и минимум шума.   
Применений можно найти кучу. 
Сейчас занимаемся полноприводным вариантом, но там еще есть свои нюансы с которыми предстоит разобраться.

Огромное спасибо авторам прошивки и всем, кто помог в реализации моего проекта!!!

vfear_777@mail.ru для связи

# Ссылки
ST-Link v2  
https://amperkot.ru/msk/catalog/programmator_stlink_v2_dlya_stm8_stm32-24309549.html  
Arduino  
https://amperkot.ru/msk/catalog/plata_uno_r3__arduinosovmestimaya__usb_kabel-24254833.html  
DC-DC   
https://tpai.ru/elektropitanie-skhem/preobrazovateli-elektricheskogo-toka/konverter-dc-dc-ponizhayushhij-lm2596  
Логический преобразователь  
https://tpai.ru/provodnye-i-besprovodnye-moduli-obmena-dannymi-rfid/provodnye-protokoly/konverter-urovnya-i2c-5v-33v  
Модуль реле  
https://www.chipdip.ru/product/rdc1-2ra-relay  
Пульт   
https://rc-go.ru/cat/ct6br6b/  
Штеккер питания Ардуино  
https://tpai.ru/maketirovanie-i-proektirovanie/razyomy/razyom-dlya-krony-so-shtekerom  
Провода  
https://tpai.ru/maketirovanie-i-proektirovanie/soedinitelnye-provoda-i-peremychki/nabor-provodov-dupont-3-vida-po-10-shtuk-20-sm  


