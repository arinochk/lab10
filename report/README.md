## Report Laboratory work X

Данная лабораторная работа посвещена изучению процесса создания и конфигурирования виртуальной среды разработки с использованием **Vagrant**

Для начала необходимо произвести устновку Vagrant. Ввиду проблем поддержки данного приложения в нашем регионе устновка была произведена через dmg-файл, который был загружен из по ссылке из консоли с использованием прокси-сервера.

После установки давайте проверим версию установленного vagrant-а.
```sh
$ vagrant version
```
Но получаем ошибку, так как требуется плагин vagrant-vbguest для работы, который на прямую через рекомендуемую команду установить не можем
```sh
$ vagrant plugin install --local
```
![Ошибка при получении версии](img/version-error.png?raw=true "Ошибка при получении версии")

Ввиду того, что данное решение - это гемы (библеотеки на языке Ruby), указанные плагины мы можем загрзуить с офицального хранилища "гемов", а именно https://rubygems.org

Соотвеветсвенно команда для загрузки будет выглядеть следующим образом:
```sh
$ vagrant plugin install vagrant-vbguest --plugin-clean-sources --plugin-source https://rubygems.org
```
Сразу после чего пробуем получить версию vagrant-а:
![Установка плагина](img/install-plugin.png?raw=true "Установка плагина")

После чего создаем конфигурационный файл со скриптом поднятие докер-контейнера с приложением и конфигурацией виртуальной машины, основаная часть настроек связана с логинами для доступа и выбором операционной системы для созданной виртуальной машины.
Финальный файл представлен в папке с отчетом.
После составления файла можно командой произвести проверку указанного файлами средствами самого vagrant-а:
```sh
$ vagrant validate
```
![Валидация](img/validate.png?raw=true "Валидация")

Даллее пытаемся запустить:
```sh
vagrant up --provider virtualbox
```

Но при попытке запустить данную сборку получаем ошибку, связанную с загрузкой образа ("box"-а) операционной системы для vagrant.
![Ошибка запуска](img/erorr-run.png?raw=true "Ошибка запуска")

Для решения данной проблемы использовалась команда локальной установки образа (сам образ был загружен отдельно c официального сайта https://app.vagrantup.com/bento/boxes/ubuntu-19.10):
```sh
$ vagrant box add bento/ubuntu-19.10 virtualbox
```

Первый параметр - это как называется образ
Второй - это название загруженного файла
![Установка](img/install-ubuntu.png?raw=true "Установка")

После чего статрт с помощью команды происходит успешно
![Старт](img/start.png?raw=true "Старт")


Далле открывается virtualbox с созданной виртуальной машиной и запущенным приложением
![virtual-box](img/virtual-box.png?raw=true "virtual-box")

Далле пробуем сменить тип запускаемой виртуальной машины на vmware, согласно заданию лабоработной работы.
Для этого так же устанавливаем плагин из https://rubygems.org
```sh
$ vagrant plugin install vagrant-vmware-esxi --plugin-clean-sources --plugin-source https://rubygems.org
```
![vmware-plugin](img/vmware-plugin.png?raw=true "vmware-plugin")

После чего уничтожаем созданный образ на основе VirtualBox (так как установленная версия поддерживает в активном состоянии только один образ)

```sh
$ vagrant destroy default
```
![destroy](img/destroy.png?raw=true "destroy")

После чего с помощью команды можем успешно запустить образ на основе vmware, где запрашивается пароль и открывается ssh подключение к созаддной виртуальной машине с приложением
![vmware](img/vmware.png?raw=true "vmware")

