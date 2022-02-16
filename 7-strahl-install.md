# Установка  Strahl

После установки всех необходимых зависимостей возвращаемся в обычный режим пользователя. Для этого надо закрыть терминал или его вкладку, в которой мы в прошлом разделе переключились на root, и открыть новое окно\вкладку терминала.

Для установки Strahl необходимо скопировать его исходники на виртуальную машину. После этого можно воспользоваться простым скриптом по копированию этих файлов в нужные директории Astra:

```sh
#!/bin/csh -f

set PATH1 = /home/%username/strahl_files
set PATH2 = /sdb/%username/astra-int


mkdir ${PATH2}/atomdat 
mkdir ${PATH2}/nete
mkdir ${PATH2}/param_files
mkdir ${PATH2}/pec
mkdir ${PATH2}/result 
mkdir ${PATH2}/str 

cp -v ${PATH1}/*.atomdat ${PATH2}
cp -v ${PATH1}/*.inc ${PATH2}
cp -v ${PATH1}/strahl.control ${PATH2}
cp -v ${PATH1}/atomdat/* ${PATH2}/atomdat -r
cp -v ${PATH1}/nete/* ${PATH2}/nete
cp -v ${PATH1}/param_files/* ${PATH2}/param_files
cp -v ${PATH1}/pec/* ${PATH2}/pec
cp -v ${PATH1}/result/* ${PATH2}/result
cp -v ${PATH1}/str/* ${PATH2}/str

cp -v ${PATH1}/sbr/averfs.f ${PATH2}/sbr
cp -v ${PATH1}/sbr/param.inc ${PATH2}/sbr
cp -v ${PATH1}/sbr/simson.f ${PATH2}/sbr
cp -v ${PATH1}/sbr/spline1.f ${PATH2}/sbr

```

Осталось добавить пару строчек в конфиг-файл `.exe/astrarc`. Открываем его (`sudo nano .exe/astrarc`), находим в нем `case "Intel":` и в этом блоке кода необходимо закомментировать стандартные настройки (`setenv AFL` и `AFC`) и вставить новые значения:

```
setenv AFL  "ifort -traceback -O0 -w -I. -I/usr/local/netcdff_intel/include -o"
setenv AFC  "ifort -traceback -O0 -w -I. -I/usr/local/netcdff_intel/include -c"
setenv XLBR  "-L/lib64 -lX11  .usr/Intel/user.a -L/usr/lib -lstdc++ -lgcc_s  -L/usr/local/netcdff_intel/lib -lnetcdff -L/usr/local/hdf5_intel/lib -lhdf5 .lbr/Intel/esp.a"
```
Сохранить, выйти. Готово.
