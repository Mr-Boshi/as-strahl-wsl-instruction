# Включение WSL 2
1. Открыть Windows Terminal **от администратора**. В нем по умолчанию новая вкладка -- терминал PowerShell. Если нет -- открыть терминал PowerShell в новой вкладке.
2. Выполнить

		dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

	для включения подсистемы WSL.
3. Выполнить

		dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

	для включения компонента Платформы виртуальных машин.
4. Перезагрузить компьютер.
5. Скачать [отсюда](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) и установить Пакет обновления ядра Linux в WSL 2 для 64-разрядных компьютеров.
6. Открыть Windows Terminal **от администратора**. Открыть PowerShell в новой вкладке (если необходимо).
7. Выполнить

		wsl --set-default-version 2

	для настройки WSL2 в качестве версии по умолчанию.

[Источник](https://docs.microsoft.com/ru-ru/windows/wsl/install-win10)
