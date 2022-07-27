# Tips de archlinux

### **Mantenimiento y Limpieza en Arch Linux**

Actualizar lista de MirrorList

`sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist`

Actualizar Sistema

`sudo pacman -Syu ; yay -Syu; paru -Syu`

Eliminar cache de descargas de programas

`du -sh /var/cache/pacman/pkg/`

`du -sh ~/.cache/yay`

`sudo pacman -Scc`

`yay -Scc`

`paru -Scc`

Eliminar paquetes huerfanos

`sudo pacman -Rns $(pacman -Qdtq)`

Eliminar cache

`du -sh .cache`

`rm -rf .cache/*`

Eliminar configuraciones (cuidado)

`du -sh .config`

Cuidado con esto puede borrar de manera definitiva sus configuraciones

`rm -rf .config/nombre_del_archivo` 

Revisar servicios de systemD

`systemctl --failed`

`sudo journalctl -p 3 -xb`

Eliminar registro de journal

`du -sh /var/log/journal`

`rm -rf /var/log/journal/*`

Activar las Descargas paralelas en Arch Linux - Pacman V.6.0.1

Ejecutamos el editor de texto como administrador:

`sudo nano -l /etc/pacman.conf`

Dentro de [options] debemos agregar:

`ParallelDownloads = 5`

5 es el número de descargas

Podemos agregar otras modificaciones dentro de pacman.conf

### **Personalizando PACMAN**

Descomenta lo siguiente en el archivo /etc/pacman.conf

`Color`

`VerbosePkgLists`

`ILoveCandy →`  la I, L, C en mayusculas.

### **Sincronizar hora en Arch Linux**

`sudo pacman -Sy ntp --noconfirm`

`sudo systemctl enable --now ntpd`

`sudo timedatectl set-timezone $(curl https://ipapi.co/timezone)`

`sudo ntpd -qg`

`sudo hwclock --systohc`

### **Instalar Arch Linux en BIOS LEGACY en 25 pasos**

1.Ponemos el teclado en español, puede ser también:  es / la-latin1

`loadkeys es`

2.Revisamos que nuestro disco esté en MBR / Dos y cual es la ruta

`fdisk -l`

3.Creamos la tabla de partición en nuestro caso es /dev/vda esto borrará todo el contenido del disco:

4.Formateamos el disco

`mkfs.ext4 /dev/sda1`

5.Montamos el disco

`mount /dev/sda1 /mnt`

6.Instalamos los programas necesarios para el sistema

`pacstrap  /mnt  base  base-devel  nano  dhcpcd  netctl  iwd  net-tools  networkmanager  ifplugd  reflector  grub  os-prober  mkinitcpio  linux  linux-headers  linux-firmware`

7.Generamos el archivo FSTAB

`genfstab -p /mnt >> /mnt/etc/fstab`

8.Entramos al sistema

`arch-chroot /mnt`

9.Definimos idioma y localización

Aquí es donde usamos la abreviatura de nuestro país en mi caso es Venezuela = Ve

`echo es_VE.UTF-8 UTF-8 > /etc/locale.gen`

10.Establecemos localización

`locale-gen`

11.Establecemos idioma

`echo LANG=es_VE.UTF-8 > /etc/locale.conf`

12.Exportamos la variable para idioma

`export LANG=es_VE.UTF-8`

13.Definimos la entrada de teclado, puede ser también:  es / la-latin1

`echo KEYMAP=es > /etc/vconsole.conf`

14.Este comando asume que el reloj de hardware está configurado en UTC

`hwclock -w`

15.Establecemos la zona horaria

`ln -sf /usr/share/zoneinfo/$(curl https://ipapi.co/timezone)  /etc/localtime`

16.Establecemos nombre de nuestro Equipo

`echo nombre_de_pc > /etc/hostname`

17.Ponemos nuestra clave para el Administrador

`passwd root`

18.Creamos nuestro usuario

`useradd -m -g users -s /bin/bash nombre_de_usuario`

19.Para que nuestro usuario esté en la lista de SUDOERS y tenga permisos al ejecutar SUDO

`sed -i "80i nombre_de_usuario ALL=(ALL) ALL"  /etc/sudoers`

20.Ponemos nuestra clave para nuestro Usuario

`passwd nombre_de_usuario`

21.Activamos servicios respete mayus y minus

`systemctl enable dhcpcd NetworkManager`

22.Generamos una lista de Mirrors con la mejor velocidad

`reflector --verbose --latest 15 --sort rate --save /etc/pacman.d/mirrorlist`

21.Instalamos GRUB

`grub-install /dev/sda`

22.Generamos GRUB

`grub-mkconfig -o /boot/grub/grub.cfg`

23.Salimos

`exit`

24.Desmontamos los discos montados de /mnt

`umount -R /mnt`

25.Reiniciamos e iniciamos como root y con la clave root

`reboot`

DESCANSA Y PROTEGE TU VISTA CON REDSHIFT

Instalación:

`sudo pacman -S redshift`

Ejecución:

`redshift`

Ver más parámetros y opciones que permite el programa en terminal:

`redshift -h`

Para saber que Shell estamos usando actualmente ejecutamos:

`echo $SHELL`

O también:

`echo $0`

Para instalar diferentes Shell

bash (Bourne-again Shell):

`sudo pacman -S bash bash-completion autojump command-not-found`

zsh (Z-Shell):

`sudo pacman -S zsh zsh-completions zsh-syntax-highlighting`

fish (friendly interactive shell):

`sudo pacman -S fish pkgfile`

dash (Debian Almquist shell):

`sudo pacman -S dash checkbashisms`

mksh (KornShell):

`sudo pacman -S  mksh`

tcsh (C shell):

`sudo pacman -S tcsh`

Una vez instalado para ver una vista previa ejecutamos:

`exec zsh`

Para definir la SHELL predeterminada ejecutamos:

`sudo chsh -s /bin/zsh nombre_de_usuario`

### **Evitar que pacman instale programas incluso si ya está actualizado**

`sudo pacman -Syu --needed`

`sudo pacman -S paquetes --needed`

### Wine

1. Agregar soporte multilib

Entrar a /etc/pacman.conf

Descomentar la siguiente línea

`[multilib]`

`Include = /etc/pacman.d/mirrorlist`

2. Instalar wine

`pacman -S wine wine-gecko  wine-mono lib32-libldap`

Intel

`sudo pacman -S lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader`

4. Instalar soporte para audio

Alsa

`sudo pacman -S lib32-alsa- plugins`

Pulse

`sudo pacman -S lib32-libpulse`

5. Instar lutris

`sudo pacman -S lutris`

### Desintalar Completamente wine

1. Abrir una terminar y teclear (o copiar y pegar):

`sudo pacman -Rs wine wine wine-mono lib32-libldap`

2. `rm -rf $HOME/.wine`

3. `rm -f $HOME/.config/menus/applications-merged/wine*`

4. `rm -rf $HOME/.local/share/applications/wine`

5. `rm -f $HOME/.local/share/desktop-directories/wine*`

6. `rm -f $HOME/.local/share/icons/????_*.xpm`

### Instalación de Samba.

Instalamos samba:

`sudo pacman -S samba`

Creamos un archivo vacío para la configuración:

`sudo touch /etc/samba/smb.conf`

Luego copiamos el contenido de la configuración de ejemplo en el archivo que acabamos de crear.

Configuración de ejemplo:

https://git.samba.org/samba.git/?p=samba.git;a=blob_plain;f=examples/smb.conf.default;hb=HEAD

Al archivo de configuración de samba le cambiamos donde dice:

log file = /usr/local/samba/var/log.%m

Lo cambiamos por:

log file = /var/log/samba/%m.log

Para probar si el archivo de configuración está bien, ocupamos:

testparm /etc/samba/smb.conf

Creamos el usuario para ingresar:

`sudo smbpasswd -a tu_usuario`

Habilitamos los servicios:

`sudo systemctl enable smb.service`

`sudo systemctl enable nmb.service`

Nos desloguear y volvemos a entrar.

Esto fue testeado en un celular con la app MiXplorer, pero se puede ocupar cualquier cliente Samba.

### Programar un apagado automático en Linux

Un apagado automático se puede programar de diferentes formas.

Por ejemplo si quieres realizar el apagado automático a las 16:30 PM, puedes utilizar el comando de la siguiente manera:

`sudo shutdown 16:30`

Para programar un apagado dentro de 30 minutos, puedes realizar el apagado utilizando el siguiente comando:

`sudo shutdown +30`

Cancelar un apagado programado

Por suerte esto tiene una fácil solución, sólo tienes que usar la opción -c

`sudo shutdown -c`

### Pacman

Sincroniza la base de datos con los repositorios.

`pacman -Sy`

Actualiza el sistema completo.

`pacman -Syu`

Instala un paquete.

`pacman -S Paquete`

Desinstala un paquete.

`pacman -R paquete`

Desinstala un paquete junto a las dependencias no utilizadas por otros paquetes.

`pacman -Rs paquete`

Permite buscar a un paquete específico

`pacman -Ss Paquete`

Descarga el paquete pero no lo instala

`pacman -Sw paquete`

Muestra información sobre un paquete no instalado

`pacman -Si paquete`

Muestra información sobre un paquete ya instalado

`pacman -Qi paquete`

Instala solamente las dependencias del paquete.

`pacman -Se paquete`

Muestra todos los archivos pertenecientes al paquete.

`pacman -Ql paquete`

Muestra los paquetes del sistema que pueden ser actualizados, pero no los instala.

`pacman -Qu`

Muestra una lista de todos los paquetes instalados en el sistema.

`pacman -Q`

Muestra a cuál paquete pertenece un archivo en especial.

`pacman -Qo /ruta/del/archivo`

Borra todos los paquetes antiguos guardados en la caché de Pacman.

`pacman -Sc`

Borra todos los paquetes guardados en la caché de pacman ubicado en /var/cache/pacman/pkg.

`pacman -Scc`

Instala un paquete guardado en una carpeta local.

`pacman -U`

### INTEL (Soporta Vulkan gaming):

/*Gráficos integrados al procesador

`pacman -S mesa lib32-mesa`

`pacman -S xf86-video-intel vulkan-intel`

>>> Aceleración por Hardware <<<

La aceleración de vídeo por hardware hace posible que la tarjeta de vídeo

decodifique y codifique vídeo usando la GPU (gráfica)

Descargando así la CPU (procesador) y ahorrando energía

`pacman -S intel-media-driver libva-intel-driver`

`pacman -S libva-vdpau-driver libvdpau-va-gl`

`pacman -S libva-utils vdpauinfo`

>>> GPGPU <<<

/*Trata de aprovechar las capacidades de cómputo de una GPU.

/*Descargando así la CPU y ahorrando energía.

/*En Linux, actualmente hay dos marcos principales de GPGPU: OpenCL y CUDA

/*Se usa mucho en edición de vídeo, diseño gráfico, etc...

/*OpenCL funciona con cualquier programa o Juego,

/*hasta en Wine para trabajar con la GPU su uso es:

`wine /ruta/a/juego_3d.exe -opengl`

Instalación:

`pacman -S intel-compute-runtime beignet`

`pacman -S clinfo ocl-icd opencl-headers`

### Códecs de vídeo

`sudo pacman -S ffmpeg aom libde265 x265 x264 libmpeg2 xvidcore libtheora libvpx schroedinger sdl gstreamer gst-plugins-bad gst-plugins-base gst-plugins-base-libs gst-plugins-good gst-plugins-ugly xine-lib libdvdcss libdvdread dvd+rw-tools lame`

Con ese comando vemos todos los códecs de vídeo21 disponibles

ffmpeg -formats -E

Compresión y Descompresión

- ark: Interfaz Gráfica de KDE
- xarchiver: Interfaz Gráfica en GTK+

`sudo pacman -S ark xarchiver unarchiver binutils gzip lha lrzip lzip lz4 p7zip tar xz bzip2 p7zip lbzip2 arj lzop cpio unrar unzip zstd zip lzip unarj zstd`

### Lectura de cualquier formato de Discos

(Mac-windows-android-discos 3.0,etc)

Esto nos permite tener lectura y escritura en algunos casos de varios formatos de discos

`sudo pacman -S android-file-transfer android-tools android-udev msmtp libmtp libcddb gvfs gvfs-afc gvfs-smb gvfs-gphoto2 gvfs-mtp gvfs-goa gvfs-nfs gvfs-google dosfstools jfsutils f2fs-tools btrfs-progs exfat-utils ntfs-3g reiserfsprogs udftools xfsprogs nilfs-utils polkit gpart mtools`

Instalación de Kernel Linux

Para Linux Estable:

La versión vanilla del kernel y los módulos, con pocas modificaciones aplicadas.

`sudo pacman -S linux-firmware mkinitcpio linux linux-headers`

Para Linux Hardened:

Un kernel de Linux enfocado en seguridad, aplica parches para mitigar la explotación en el kernel o en el espacio del usuario. También activa más características de seguridad en comparación con linux, entre otros: namespaces, audit y SELinux.

`sudo pacman -S linux-firmware mkinitcpio linux-hardened linux-hardened-headers`

Para Linux LTS:

Kernel de Linux y módulos con soporte de larga duración (LTS).

`sudo pacman -S linux-firmware mkinitcpio linux-lts linux-lts-headers`

Para Linux Zen:

Es el resultado de un esfuerzo colaborativo de varios hackers para hacer el mejor kernel para el uso en sistemas de uso diario, el mejor en múltiples procesos.

`sudo pacman -S linux-firmware mkinitcpio linux-zen linux-zen-headers`

Después de la instalación del Kernel es necesario volver a generar grub para que reconozca las nuevas  *IMG del kernel instalado:

`grub-mkconfig -o /boot/grub/grub.cfg`

Si usa otro gestor de arranque consulte la documentación.

Información hardware.

`lspci -vv`

(se pueden filtrar datos con el comando grep)

`Ejemplo: lspci -vv | grep VGA`

- v = Verbose
- vv = Very Verbose
