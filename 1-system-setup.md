# Подготовка системы

1. Обновить Windows 10 до версии 1903 или выше (для проверки: нажать `Win+R`, ввести `winver`)
2. Установить из Microsoft Store бесплатный Windows Terminal (или Windows Terminal Preview)
3. Для Windows 10 необходимо купить в Microsoft Store и установить X Server X410 или скачать и установить бесплатный X Server VcXsrv. Для Windows 11 этого делать не нужно, для неё WSL имеет встроенный X Server.
4. Скачать автономный установщик Ubuntu [отсюда](https://docs.microsoft.com/en-us/windows/wsl/install-manual) (раздел *Downloading distributions*), распаковываем полученный файл с помощью архиватора (7-zip, например). По желанию переименовываем во что-то более приемлемое, например Ubuntu-20.04 и копируем его в каталог C:\wsl (в качестве примера).
5. **Опционально:** если нужно принимать звуковые сигналы из виртуальной машины, то скачиваем [отсюда](https://wikiprograms.org/pulseaudio/) кроссплатформенный звуковой сервер PulseAudio v.1.1. Распаковываем в `C:\wsl` и вносим исправления в его конфигурационные файлы.
	<details>
		<summary>Настройка PulseAudio</summary>
		В файле \wsl\pulseaudio-1.1\etc\pulse\default.pa в разделе Load audio drivers statically редактируем строку:
		<pre>
		load-module module-waveout sink_name=output source_name=input record=0
		</pre>
		а в разделе Network access редактируем строку:
		<pre>
		load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1
		</pre>
		В файле \wsl\pulseaudio-1.1\etc\pulse\daemon.conf раскомментируем и изменяем строку
		<pre>
		exit-idle-time = -1
		</pre>
	</details>

[Источник](https://habr.com/ru/post/522726/)
