# Установка и настройка Ubuntu
## Установка Ubuntu
1. Открыть Windows Terminal.
2. Перейти в каталог с распакованным установщиком Ubuntu выполнив

		cd C:\wsl\Ubuntu-20.04

	(согласно нашему примеру).
3. Запустить установку введя название исполняемого файла. В нашем примере это `ubuntu2004.exe`:

		.\ubuntu2004.exe
4. По окончании установки Ubuntu в консоли появится запрос на создание нового пользователя. Нужно ввести имя нового пользователя и его пароль для Ubuntu (могут быть любыми).
5. Для проверки корректности можно открыть в Windows Terminal новую вкладку и ввести там

		wsl -l -v

	Должна появиться наша Ubuntu. В столбце STATE указано состояние виртуальной машины (`RUNNING` или `STOPPED`) В столбце `VERSION` должно быть указано `2`.
6. Отключить Microsoft Defender для активной сети. Вроде это нужно, чтоб виртуальная машина могла безпрепятственно связываться с интернетом, но вроде это не обязательно.

[Источник](https://habr.com/ru/post/522726/)

## Настройка Ubuntu
1. Если повторно запустить Windows Terminal, то для новой вкладки появится новый вариант с названием виртуальной машины, которую мы установили на предыдущем шаге (Ubuntu-20.04 в нашем примере). Нажав на него мы запускаем виртуальную машину с  открываем терминал Ubuntu. В нем можно выполнять почти все команды Linux.
2. В открытом терминале Ubuntu вводим

		sudo apt update && sudo apt upgrade

	для обновления системы. Потребуется ввести пароль пользователя и подтвердить обновление введя `y`.
3. Вводим

		sudo apt install kubuntu-desktop
	для установки среды рабочего стола KDE. Потребуется ввести пароль пользователя и подтвердить обновление введя `y`. В процессе в консоли еще будут запросы, но сложного выбора пользователю делать не придется.

4. **Опционально:** для установки русской локализации и словарей вводим

		sudo apt install language-pack-ru language-pack-kde-ru
		sudo apt install libreoffice-l10n-ru libreoffice-help-ru
		sudo apt install hunspell-ru mueller7-dict
		sudo update-locale LANG=ru_RU.UTF-8
		sudo dpkg-reconfigure locales

	(примечание: в списке Locales to be generated выбираем **ru_RU.UTF-8 UTF-8**, а в списке Default locale for the system enviroment -- **ru_RU.UTF-8** или другую на свой выбор)

		sudo apt-get install --reinstall locales

	Потребуется ввести пароль пользователя и подтвердить обновление введя `y`.
5. Выполняем

		sudo add-apt-repository ppa:kubuntu-ppa/backports
		sudo apt update && sudo apt full-upgrade

	для добавления репозитория с KDA и обновления до последней версии.
6. Открываем wsl.conf для редактирования

		sudo nano /etc/wsl.conf

	и вставляем в него

		[automount]
		enabled = true
		root = /mnt
		options = «metadata,umask=22,fmask=11»
		mountFsTab = true
		[network]
		generateHosts = true
		generateResolvConf = true
		[interop]
		enabled = true
		appendWindowsPath = true

	сохраняем изменения (`Ctrl+O` -- буква, а не цифра), подтверждаем операцию и выходим из текстового редактора (`Ctrl+X`).

7. Заранее поставим пакеты необходимые в дальнейшем. Выполняем

		sudo apt-get install build-essential
		sudo apt-get install gcc-multilib
		sudo apt-get install rpm
		sudo apt-get install openjdk-8-jre-headless
		sudo apt-get install csh
		sudo apt-get install tcsh
		sudo apt-get install libx11-dev
		sudo apt-get install m4

8. Создаем ярлыки для запуска Ubuntu с графическим интерфейсом Для этого образ системы экспортируется командой, выполненной в PowerShell

		wsl --export Ubuntu-20.04 c:\wsl\Ubuntu-plasma-desktop

	Затем создается .bat-файл со следующим содержимым:

		@echo off
		echo ===================================== Внимание! ============================================
		echo  Для корректной работы GUI Ubuntu 20.04 в WSL2 необходимо использовать X Server.
		echo  Примечание: в случае использования VcXsrv Windows X Server необходимо раскомментировать
		echo  строки в файле Start-Ubuntu-20.04-plasma-desktop.bat, содержащие "config.xlaunch" и
		echo  "vcxsrv.exe", и закомментировать все строки, содержащие "x410".
		echo ============================================================================================
		rem start "" /B "c:\wsl\vcxsrv\config.xlaunch" > nul
		start "" /B x410.exe /wm /public > nul
		rem start "" /B "c:\wsl\pulseaudio-1.1\bin\pulseaudio.exe" --use-pid-file=false -D > nul
		c:\wsl\Ubuntu-20.04\Ubuntu2004.exe run "if [ -z \"$(pidof plasmashell)\" ]; then cd ~ ; export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0 ; setxkbmap us,ru -option grp:ctrl_shift_toggle ; export LIBGL_ALWAYS_INDIRECT=1 ; export PULSE_SERVER=tcp:$(grep nameserver /etc/resolv.conf | awk '{print $2}') ; sudo /etc/init.d/dbus start &> /dev/null ; sudo service ssh start ; sudo service xrdp start ; plasmashell ; pkill '(gpg|ssh)-agent' ; fi;"
		rem taskkill.exe /F /T /IM vcxsrv.exe > nul
		taskkill.exe /F /T /IM x410.exe > nul
		rem taskkill.exe /F /IM pulseaudio.exe > nul
		rem pause > nul

	Файл называем, например `Start-Ubuntu-20.04-plasma-desktop.bat`

	Для запуска графического терминала Ubutntu без рабочего стола содержимое файла будет отличаться:

		@echo off
		echo ===================================== Внимание! ============================================
		echo  Для корректной работы GUI Ubuntu 20.04 в WSL2 необходимо использовать X Server.
		echo  Примечание: в случае использования VcXsrv Windows X Server необходимо раскомментировать
		echo  строки в файле Start-Ubuntu-20.04-plasma-desktop.bat, содержащие "config.xlaunch" и
		echo  "vcxsrv.exe", и закомментировать все строки, содержащие "x410".
		echo ============================================================================================
		rem start "" /B "c:\wsl\vcxsrv\config.xlaunch" > nul
		start "" /B x410.exe /wm /public > nul
		rem start "" /B "c:\wsl\pulseaudio-1.1\bin\pulseaudio.exe" --use-pid-file=false -D > nul
		c:\wsl\Ubuntu-20.04\Ubuntu2004.exe run "cd ~ ; export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0 ; export LIBGL_ALWAYS_INDIRECT=1 ; setxkbmap us,ru -option grp:ctrl_shift_toggle ; export PULSE_SERVER=tcp:$(grep nameserver /etc/resolv.conf | awk '{print $2}') ; sudo /etc/init.d/dbus start &> /dev/null ; sudo service ssh start ; sudo service xrdp start ; konsole ; pkill '(gpg|ssh)-agent' ;"
		taskkill.exe /F /T /IM x410.exe > nul
		rem taskkill.exe /F /T /IM vcxsrv.exe > nul
		rem taskkill.exe /F /IM pulseaudio.exe > nul
		rem pause > nul

	Можно создать для этого отдельный файл и назвать его, например `Start-Ubuntu-20.04-terminal.bat`

	Как сказано в самих батниках, при использовании `VcXsrv` вместо `x410`, необходимо в файле Start-Ubuntu-20.04-plasma-desktop.bat раскомментировать строки, содержащие "config.xlaunch" и "vcxsrv.exe", и закомментировать все строки, содержащие "x410" (8-9 и 12-13 строки).

	Если окно командной строки появляется и сразу исчесзает раскомментируйте последнюю строку. Таким образом можно посмотреть что за ошибку выдает скрипт.

	Если нужен PulseAudio, то раскомментируйте 10 и 14 строки.

9. **Опционально:** в источнике можно посмотреть как установленную\настроенную систему можно экспортировать и затем заново развернуть на другой машине с помощью других .bat-файлов.
	<details>
		<summary>Install-Ubuntu-20.04-plasma-desktop.bat</summary>
		<pre>
	@echo off
	wsl --set-default-version 2
	cls
	echo Ожидайте окончания установки дистрибутива Ubuntu-20.04...
	wsl --import Ubuntu-20.04 c:\wsl c:\wsl\Ubuntu-plasma-desktop
	wsl -s Ubuntu-20.04
	cls
	echo Дистрибутив Ubuntu-20.04 успешно установлен!
	echo Не забудьте сменить учетную запись по умолчанию «root» на существующую учетную запись пользователя,
	echo либо используйте предустановленную учетную запись «engineer», пароль: «password».
	pause
		</pre>
	</details>
	<details>
		<summary>Reinstall-Ubuntu-20.04-plasma-desktop.bat</summary>
		<pre>
	@echo off
	wsl --unregister Ubuntu-20.04
	wsl --set-default-version 2
	cls
	echo Ожидайте окончания переустановки дистрибутива Ubuntu-20.04...
	wsl --import Ubuntu-20.04 c:\wsl c:\wsl\Ubuntu-plasma-desktop
	wsl -s Ubuntu-20.04
	cls
	echo Дистрибутив Ubuntu-20.04 успешно переустановлен!
	pause
		</pre>
	</details>
	<details>
		<summary>Set-default-user.bat</summary>
		<pre>
	@echo off
	set /p answer=Введите существующую учетную запись в Ubuntu (engineer):
	c:\wsl\Ubuntu-20.04\ubuntu2004.exe config --default-user %answer%
	cls
	echo Учетная запись пользователя %answer% в Ubuntu-20.04 установлена по умолчанию!
	pause
		</pre>
	</details>

	Названия файлов мнемонические, их функции понятны.

Запуск виртуальной машины происходит с ярлыка (в графическом режиме) или из Windows Terminal открытием консоли Ubuntu в новой вкладке. Для выключения нужно ввести в PowerShell команду `wsl --shutdown -d Ubuntu-20.04`. Закрытие вкладки Ubuntu в Windows Terminal не выключает виртуальную машину. Когда она включена, в списке процессов присутствует Vmmem, который жрет оперативку.

[Источник](https://habr.com/ru/post/522726/)

## Важное замечание
По окончанию установки у нас есть две среды для ввода консольных команд. Одна -- вкладка в Windows Terminal, другая -- эмулятор терминала из графического окружения самого Ubuntu, запущенного с созданного нами ярлыка `Start-Ubuntu-20.04-plasma-desktop.bat`. Их возможности немного отличаются.

В эмуляторе консоли, выполняемой в графической среде Ubuntu (по умолчанию -- Konsole), будут выполняться команды, которые подразуменвают вывод информации в графической среде. Во вкладке Windows Terminal же такие команды выдадут ошибку `ERROR: Cannot establish a connection to the X Server`. Поэтому **все команды, которые запускают графические окна** (как и сама **Astra**), **надо выполнять из эмулятора консоли в графической среде Ubuntu**.
