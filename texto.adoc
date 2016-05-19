= Compilación cruzada del kernel de Linux


== Objetivo. Llevar a cabo la compilación cruzada del kernel de Linux para la tarjeta de desarrollo utilizada durante el diplomado.

== Introducción

Desde un punto de vista general el kernel de un sistema operativo es el encargado de que el software y hardware trabajen juntos óptimamente llevando a cabo tareas como la administración de memoria, el tiempo de ejecución de procesos y el acceso a periféricos entre otras. 

La estructura básica del kernel de Linux esta compuesta de muchos submódulos ejecutando cada uno una tarea especifica como la planificación y administración de tareas, el control de tiempo, etc.

En otro orden de ideas, la esencia del software libre permite conocer y modificar el código del kernel a conveniencia del usuario, con lo que, es capaz de adaptarlo a sus necesidades. Entre las modificaciones más comunes al kernel de Linux esta la adición y sustracción de drivers con lo que se mejora el desempeño del hardware y software. Algunos de los resultados inmediatos de este tipo de modificación es que el SO puede manejar nuevo hardware y/o haciéndo más liviano aplicaciones informáticas preexistentes. A la integración de estos cambios se le conoce como compilación del kernel.

Básicamente existen dos formas de llevar a cabo la compilación de un kernel y son la compilación directa y la compilación cruzada. La compilación directa es llevada a cabo en el hardware anfitrión donde trabajara el sistema operativo. Debido a la gran variedad de aplicaciones posibles, en muchas ocasiones tal plataforma tiene recursos limitados (procesador "lento", memoria mínima, etc.) en comparación a los encontrados en una computadora de escritorio por lo que la compilación directa requiere gran cantidad de tiempo.

Por otro lado, se denomina compilación cruzada a la compilación de código fuente que realizada bajo una determinada arquitectura genera código ejecutable para una arquitectura diferente. 

El presente documento enumera los pasos seguidos durante la compilación cruzada del kernel de la tarjeta Raspberry Pi 3 en una maquina virtual con sistema operativo Lubuntu. Para llevar a cabo la tarea se tomó como base la información presentada en [1], la cual muestra los pasos a seguir para la construcción de un nuevo kernel para ese dispositivo


.1. El primer paso fue instalar la herramienta Toolchain mediante el comando

    git clone https://github.com/raspberrypi/tools 

Como resultado se genera un directorio de nombre tools que contiene los siguientes directorios
 
	arm-bcm2708  armstubs  configs  mkimage  pkg  sysidk  test_code  usbboot

.2. Una vez instalada la Toolchain es necesario modificar la variable PATH para que el interprete de comandos busque en ella los comandos introducidos. La ruta añadida a fue 

	/tools/arm-bcm2708/gcc-linaro-arm-Linux-gnueabihf-raspbian-x64/bin

y utilizé el comando para su adición 

	export PATH=$PATH:/tools/arm-bcm2708/gcc-linaro-arm-Linux-gnueabihf-raspbian-x64/bin

.3. El paso siguiente es la descarga del kernel mediante el comando 

	git clone --depth=1 https://github.com/raspberrypi/Linux

Como resultado de la ejecución del comando anterior se genera una carperta de nombre linux conteniendo los siguientes directorios.
	
	arch           firmware  lib              README          usr
	block          fs        MAINTAINERS      REPORTING-BUGS  virt
	certs          include   Makefile         samples         vmlinux
	COPYING        init      mm               scripts         vmlinux.o
	CREDITS        ipc       modules.builtin  security
	crypto         Kbuild    modules.order    sound
	Documentation  Kconfig   Module.symvers   System.map
	drivers        kernel    net              tools

.4. Como paso siguiente [1] sugiere ejecutar los siguientes comandos 
	
	cd Linux
	KERNEL=kernel
	make ARCH=arm CROSS_COMPILE=arm-Linux-gnueabihf- bcmrpi_defconfig

.5. Finalmente para iniciar la compilación cruzada se ejecuta 

	make ARCH=arm CROSS_COMPILE=arm-Linux-gnueabihf- zImage modules dtbs

Al momento de ejecutarse estos comandos, shell presenta un error debido a que no se había instalado el programa gcc-linaro-arm-Linux-gnueabihf, por lo que se procedió a hacerlo con la siguiente linea

	sudo apt-get install gcc-arm-linux-gnueabi make git-core ncurses-dev

Finalmente se obtiene dentro del directorio kernel el archivo .config el cual contiene todos los parámetros de compilación del kernel.	

Con lo anterior se da por finalizada la compilación cruzada del kernel de la tarjeta Raspberry Pi en una plataforma x86.

Bibliografía.

[1] https://www.raspberrypi.org/documentation/Linux/kernel/building.md

[2] http://www.muylinux.com/2010/11/10/como-compilar-el-kernel-Linux-en-ubuntu-fedora-y-otras

[3] http://ozzmaker.com/how-to-cross-compile-the-kernel-for-the-raspberry-pi/



