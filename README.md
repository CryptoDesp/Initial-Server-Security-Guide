# Гайд по обеспечению начальной безопасности сервера.
### Безопасности сервера является важной составляюшей при работе с криптой, в данном гайде рассмотрим начальный уровень защиты, который поможет нам сохранить накопленные реварды (если мы говорим о тестнетах), а так же сберечь наши нервы.
1. Добавим не привилегированного пользователя с помощью команд useradd, для дальнейшей авторизации через него. </br>
   Таким образом мы будем авторизовываться на портале под пользователем не имеющим ни каких прав на портале, а значит и злоумышленик не сможет ничего сделать в случае проникновения.
```
sudo adduser ИМЯ ВАШЕГО ПОЛЬЗОВАТЕЛЯ
```
2. Следующим шагом, мы отключим возможность авторизации от имении root пользователя. </br>
Данная процедура лишает возможности злоумышленика подобрать пароль к root пользователю напряму. </br>
В конфигурационном файле /etc/ssh/sshd_config надо изменить значение строчки PermitRootLogin с Yes на No, и перезапустить службу.
```
nano /etc/ssh/sshd_config
```
```
systemctl restart sshd
```
3. Отключаем автообновление системы. </br>
С одной стороны автообновление функция нужная, однако мы с вами работаем с нодами, и часто, обновление какого либо компонента в системе может нарушить работоспособность ноды, поэтому отключаем.
```
sudo dpkg-reconfigure unattended-upgrades
```
В появившемя окне выбираем No и кликаем Enter.
4. Одна из самых важных настроек безопасности это Uncomplicated FireWall (UFW). </br>
По скольку на различных хостингах предоставляются различные образы в первую очередь проверим наличие и статус ufw.
```
ufw status
```
В случае если приходит ответ inactive - ufw установлен и выключен. </br>
В случае active - ufw активен, и нам так же отобразятся установленные правила. </br>
В случае если нам выдаст, что то вроде command not found, значит ufw не установлен. </br>
Рассмотри вариант с полным отсутсвием ufw. </br>
Устанавливаем ufw
```
sudo apt install ufw
```
Поскольку после установки ufw блокирует сразу все порты, обыязательно необходмо задать правило для активации 22 порта.</br>
22 порт отвечает за подключение к серверу по ssh.
```
ufw allow 22
```
Запускаем ufw.
```
sudo ufw enable
```
Проверяем статус ufw.
```
ufw status
```
Более подробно по использованию ufw вы можете почитать [тут](https://losst.pro/nastrojka-ufw-ubuntu)

5. Для еще большей безопасности, можно сменить порт авторизации.</br>
Для этого открываем через nano /etc/ssh/sshd_config, находим строку Port, раскоментируем ее, и указываем новый порт.</br>

**После всех настроек редактируем наш канал подключения, меняем пользователя, пароль и порт если меняли в процессе настройки.**</br>
**Входим на сервер.**</br>
**Поскольку пользователь не имеет прав, нужно перейти под root пользователя.** </br>
**Вводим команду su, затем вводим пароль root пользователя.** </br>
**Далее работаем с сервером.** </br>
**Не забываем, что все порты на нашем сервере закрыты, и в процессе установки нод, необходимо порты открыть, задав правило.** </br>
