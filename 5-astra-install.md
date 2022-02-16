# Установка Astra
1. Скопировать исходник в любую папку. Например, в `/home/%username`. Название папки с исходниками должно отличаться от `Astra7`. Например, пусть будет `astra7-source`.
2. Открыть консоль в папке с исходником (либо просто ввести в консоли `cd /home/%username%/astra7-source`).
3. Выполнить

	```sh
	make -C EQUIL/SPIDER/SRC/ -f Makefile_intel clean
	make -C EQUIL/SPIDER/SRC/ -f Makefile_intel
	make -C EQUIL/COUPLING_SCHEME/ -f Makefile_intel clean
	make -C EQUIL/COUPLING_SCHEME/ -f Makefile_intel
	```
4. В результате в папке с исходником должна появиться папка `MAstraN` со скомпилированным ядром. Ее нужно полностью скопировать в место, где будет храниться ядро Astra, например в `/home/%username%`. Проще всего это сделать в файловом менеджере Ubuntu.
5. Переименовать `MAstraN` в `Astra7`. Теперь ядро находится в директории `/home/%username%/Astra7`.
6. Вернуться в открытую консоль и запустить скрипт `MAKER`. Для его запуска указывается полный путь к нему (он находится в подпапке `bin_grp` исходников Astra) и два аргумента: полный путь к папке с ядром астры (Astra7) и название платформы, которое зависит от установленного компилятора Fortran. В нашем случае команда выглядит так:

	```sh
	/home/%username%/astra7-source/bin_grp/MAKER /home/%username%/Astra7 Intel
	```
7. Создать рабочую папку в которую будут линковаться файлы из папки с ядром Astra. Например, `/home/%username%/astra-int`.
8. Вернуться в открытую консоль и выполнить скрипт `MAUSER`. Его аргументы -- путь к ядру Astra, путь к рабочей папке, платформа. В нашем случае команда выглядит так:

	```sh
	/home/%username%/astra7-source/bin_grp/MAUSER /home/%username%/Astra7 /home/%username%/astra-int Intel
	```
9. Скопировать содержимое папки `expequ` из исходников Astra в подпапку `/exp/equ` в рабочей папке Astra7 (`/home/%username%/astra-int/exp/equ`).
10. Проверить, что все работает запустив тестовый расчет астры командой `.exe/astra readme showdata`. Для проверки работы `spider` можно в `/equ/showdata` заменить `NEQUIL=0;` на `NEQUIL=41.41;` и снова запустить тестовый расчет.
11. Если запустилось окно с графиками, то все работает. Как пользоваться Astra описано в мануале к нему.
