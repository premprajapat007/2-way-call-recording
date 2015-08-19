  1. Поставить linux (хотя бы в virtualbox/vmware), под windows+cygwin собрать ядро не получится.
  1. Скачать тулчейн/кросс-компилятор для arm. Подойдет даже тот, который есть в android NDK (там он в toolchains/arm-linux-androideabi-4.x.x/prebuilt/linux-x86/bin или типа того)
  1. Из командной строки задать переменные среды для кросс-компиляции:
```
export ARCH=arm
export SUBARCH=arm
export CROSS_COMPILE=/path_to_ndk/toolchains/arm-linux-androideabi-4.x.x/prebuilt/linux-x86/bin/arm-linux-androideabi-<< Здесь должен быть префикс к exe-файлам из тулчейна (gcc, as, ld и т.п.) >>
```
  1. Скачать с устройства /proc/config.gz, раззиповать и переименовать в ".config" в корневой директории исходников ядра. Если надо, отредактировать его с помощью "make menuconfig".
  1. Переписать vocpcm.c в директорию "arch/arm/mach-msm/qdsp5" исходников, и в Makefile в этой директории добавить строчку "obj-m += vocpcm.o"
  1. Запустить make, и все.

В "arch/arm/boot" будет ядро ("zImage"), в "arch/arm/mach-msm/qdsp5" будет модуль ("vocpcm.ko", его нужно положить в /system/lib/modules).

Патч лежит на: http://code.google.com/p/2-way-call-recording/source/browse/patches/vocpcm.c
Если новое ядро, придется вставить ближе к началу "#include <linux/slab.h>".
Удачи!