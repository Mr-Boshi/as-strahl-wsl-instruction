# Установка зависимостей Strahl

Для работы Strahl необходима библиотека NetCDF. Она имеет следующие зависимости:

<img src="https://docs.unidata.ucar.edu/netcdf-c/current/deptree.jpg" alt="NetCDF dependencies" width="300"/>

Поэтому перед установкой Strahl необходимо скомпилировать нескроткр библиотек: `ZLib`, `hdf5` и `netcdf`. Это необходимо делать под суперпользователем, поэтому сразу перейдем в этот режим и заодно укажем окружению суперпользователя путь к компилятору Fortran:

```sh
sudo su
source /opt/intel/bin/compilervars.sh intel64
```

## Компиляция **Zlib**
Последнюю версию исходников можно найти [на сайте проекта](http://www.zlib.net/). На момент написания инструкции последней была версия 1.2.11. Скачиваем её:

```sh
wget http://www.zlib.net/zlib-1.2.11.tar.gz
```
1. Скопировать архив `zlib-1.2.11.tar.gz` в папку с правом записи (например, `\home\%username%\zlib`)
2. Открыть терминал в этой папке (или перейти в нее `cd \home\%username%\zlib`), разархивировать архив выполнив

	```sh
	tar -xf zlib-1.2.11.tar.gz
	```
3. Перейти в папку с содержимым архива и скомпилировать библиотеку:
	```sh
	cd zlib-1.2.11
	./configure --prefix=/usr/local/zlib
	```
	
4. Устанавливаем скомпилированный hdf5:

	```sh
	make
    make install
	```

## Компиляция **hdf5**

Последнюю версию исходников можно найти [на сайте проекта](https://confluence.hdfgroup.org/display/support/Downloads). На момент написания инструкции последней была версия 1.13.
1. Скопировать архив `hdf5-1.13.0.tar.gz` на виртуальную машину в папку с правом записи (например, `\home\%username%\hdf5`)
2. Открыть терминал в этой папке (или перейти в нее `cd \home\%username%\hdf5`), разархивировать архив выполнив

	```sh
	tar -xf hdf5-1.13.0.tar.gz
	```
3. Перейти в папку с содержимым архива (`cd hdf5-1.13.0`) и создать в ней скрипт с настройками конфигурации:
	```sh
	cd hdf5-1.13.0
	nano config-intel.sh
	```

4. . Вставить в файл следующий текст:

	```sh
	export CC=icc
	export FC=ifort
	export CXX=icpc
	#
	./configure --prefix=/usr/local/hdf5_intel --enable-fortran
	```
	
	Запускаем скрипт:
	```sh
	chmod a+x config-intel.sh
	./config-intel.sh
	```

5. Устанавливаем скомпилированный hdf5:

	```sh
	make
	make check
    make install
	```
   
6. Проверяем версию установленной библиотеки:
   ```sh
    PATH="/usr/local/hdf5_intel/bin:$PATH"
	h5stat -V
   ```

   Если команда выполнилась до конца, а команда показывает ту версию библиотеки, которую вы установили, то установка завершена. Папку `\home\%username%\hdf5` можно удалить, если вы уверены, что она вам больше не понадобится.

## Компиляция **netcdf-c**

Последнюю версию исходников можно найти [на сайте проекта](https://www.unidata.ucar.edu/downloads/netcdf/). На момент написания инструкции последней была версия для C была 4.8.1.
1. Скачать `netcdf-c-4.8.1.tar.gz` на виртуальную машину в папку с правом записи (например, `\home\%username%\netcdf`)
2. Открыть терминал в этой папке (или перейти в нее `cd \home\%username%\netcdf`), разархивировать архив выполнив

	```sh
	tar -xf netcdf-c-4.8.1.tar.gz
	```
3. Перейти в папку с содержимым архива и создать в ней скрипт с настройками конфигурации:
	```sh
	cd netcdf-c-4.8.1
	nano config-intel.sh
	```

4. . Вставить в файл следующий текст:
   	```sh
	export CC=icc
	export CXX=icpc
	export CFLAGS='-O3 -xHost -ip -no-prec-div -static-intel -I/usr/local/hdf5_intel/include'
	export CXXFLAGS='-O3 -xHost -ip -no-prec-div -static-intel -I/usr/local/hdf5_intel/include'
	export F77=ifort
	export FC=ifort
	export F90=ifort
	export FFLAGS='-O3 -xHost -ip -no-prec-div -static-intel -I/usr/local/hdf5_intel/include'
	export FCLAGS='-O3 -xHost -ip -no-prec-div -static-intel -I/usr/local/hdf5_intel/include'
	export CPP='icc -E'
	export CXXCPP='icpc -E'
	export LDFLAGS="-L/usr/local/hdf5_intel/lib"

	./configure --prefix=/usr/local/netcdf_intel --disable-dap --with-zlib=/usr/local/zlib --disable-hdf4 --enable-fortran
	```
 	Запускаем скрипт:
	```sh
	chmod a+x config-intel.sh
	./config-intel.sh
	```

5. Устанавливаем скомпилированный netcdf-c:

	```sh
	make
	make check
    make install
	```
6. Проверяем версию установленной библиотеки:
   ```sh
    PATH="/usr/local/netcdf_intel/bin:$PATH"
	nc-config --all
   ```
	Если команда выполнилась до конца, а команда показывает ту версию библиотеки, которую вы установили, то установка завершена. Папку `\home\%username%\netcdf\netcdf-c-4.8.1` можно удалить, если вы уверены, что она вам больше не понадобится.

## Компиляция **netcdf-fortran**

1. Скачать `netcdf-fortran-4.5.4.tar.gz` на виртуальную машину в папку с правом записи (например, `\home\%username%\netcdf`)
2. Открыть терминал в этой папке (или перейти в нее `cd \home\%username%\netcdf`), разархивировать архив выполнив

	```sh
	tar -xf netcdf-fortran-4.5.4.tar.gz
	```
3. Перейти в папку с содержимым архива и создать в ней скрипт с настройками конфигурации:
	```sh
	cd netcdf-fortran-4.5.4
	nano config-intel.sh
	```

4. . Вставить в файл следующий текст:
	```sh
	export LDFLAGS="-L${CDFROOT}/lib -I${CDFROOT}/include -L/usr/local/netcdf_intel/lib -lnetcdf"
	export OPTIM="-O3 -mcmodel=large -fPIC ${LDFLAGS} -I/usr/local/hdf5_intel/include"
	#
	export NCDIR="/usr/local/netcdf_intel"
	export LD_LIBRARY_PATH="${NCDIR}/lib:${LD_LIBRARY_PATH}"

	export CC=icc
	export CXX=icpc
	export FC=ifort
	export F77=ifort
	export F90=ifort
	export CPP='icc -E -mcmodel=large'
	export CXXCPP='icpc -E -mcmodel=large'
	export CPPFLAGS="-DNDEBUG -DpgiFortran ${LDFLAGS} -I/usr/local/netcdf_intel/include"
	#
	export CFLAGS=" ${OPTIM}"
	export CXXFLAGS=" ${OPTIM}"
	export FCFLAGS=" ${OPTIM}"
	export F77FLAGS=" ${OPTIM}"
	export F90FLAGS=" ${OPTIM}"
	#
	./configure --prefix=/usr/local/netcdff_intel --enable-large-file-tests --with-pic
	```
	
	Запускаем скрипт:
	```sh
	chmod a+x config-intel.sh
	./config-intel.sh
	```
5. Устанавливаем скомпилированный netcdf-c:

	```sh
	make
	make check
    make install
	```	

6. Проверяем версию установленной библиотеки:
	```sh
    PATH="/usr/local/netcdff_intel/bin:$PATH"
	nf-config --all
	```
	Если команда выполнилась до конца, а команда показывает ту версию библиотеки, которую вы установили, то установка завершена. Папку `\home\%username%\netcdf\netcdf-fortran-4.5.4` можно удалить, если вы уверены, что она вам больше не понадобится.

## Добавление путей к установленным библиотекам в **.bashrc**

Открыть .bashrc для редактирования (например, `nano \home\%username%\.beshrc`) и дописать в конец файла следующий текст:
	
``` sh
# HDF5 and NETCDF
PATH="/usr/local/zlib/bin:$PATH"
PATH="/usr/local/hdf5_intel/bin:$PATH"
PATH="/usr/local/netcdf_intel/bin:$PATH"
PATH="/usr/local/netcdff_intel/bin:$PATH"
LD_LIBRARY_PATH="/usr/local/hdf5_intel/lib:$LD_LIBRARY_PATH"
LD_LIBRARY_PATH="/usr/local/netcdff_intel/lib:$LD_LIBRARY_PATH"
```
