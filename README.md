# Archlinux tips


- Table of Contents
    
    - [Maintenance and Cleaning in Arch Linux](#maintenance-and-cleaning-in-arch-linux)
    - [Wine](#wine)
    - [Schedule an automatic shutdown in Linux](#schedule-an-automatic-shutdown-in-linux) 
    - [Pacman](#pacman)
    - [Video drivers](#video-drivers)
 


### **Maintenance and Cleaning in Arch Linux**

update mirrorlist

```bash
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Update System

```bash
sudo pacman -Syu
```

```bash
paru -Syu
```

Clear program download cache

```bash
du -sh /var/cache/pacman/pkg/
```

```bash
du -sh ~/.cache/paru
```

```bash
sudo pacman -Scc
```

```bash
paru -Scc
```

Remove orphaned packages

```bash
sudo pacman -Rns $(pacman -Qdtq)
```

clear cache

```bash
du -sh .cache
```

```bash
rm -rf .cache/*
```

Delete configurations (be careful)

```bash
du -sh .config
```

Be careful with this, it can permanently erase your settings.

```bash
rm -rf .config/<file-name>
``` 

Check systemD services

```bash
systemctl --failed
```

```bash
sudo journalctl -p 3 -xb
```

Delete journal record

```bash
du -sh /var/log/journal
```

```bash
rm -rf /var/log/journal/*
```

### Wine

1. Add multilib support

Get into /etc/pacman.conf

Uncomment the following line

```bash[multilib]
   Include = /etc/pacman.d/mirrorlist
```

2. **Install wine**

```bash
pacman -S wine wine-gecko  wine-mono lib32-libldap
```

**Intel**

```bash
sudo pacman -S lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader
```

4. **Install audio support**

Alsa

```bash
sudo pacman -S lib32-alsa-plugins
```

Pulse

```bash
sudo pacman -S lib32-libpulse
```

5. **Instar lutris**

```bash
sudo pacman -S lutris
```

### Completely Uninstall wine

1. Open a finish and copy and paste:

```bash
sudo pacman -Rs wine wine wine-mono lib32-libldap
rm -rf $HOME/.wine
rm -f $HOME/.config/menus/applications-merged/wine*
rm -rf $HOME/.local/share/applications/wine
rm -f $HOME/.local/share/desktop-directories/wine*
rm -f $HOME/.local/share/icons/????_*.xpm
```

### Schedule an automatic shutdown in Linux

An automatic shutdown can be programmed in different ways.

For example, if you want to perform automatic shutdown at 16:30 , you can use the command as follows:

```bash
sudo shutdown 16:30
```

To schedule a shutdown within 30 minutes, you can perform the shutdown using the following command:

```bash
sudo shutdown +30
```

Cancel a scheduled shutdown

Luckily this has an easy solution, you just have to use the -c option

```bash
sudo shutdown -c
```

### Pacman

Synchronize the database with the repositories.

```bash
pacman -Sy
```

Update the entire system.

```bash
pacman -Syu
```

Install a package.

```bash
pacman -S package
```

Uninstall a package.

```bash
pacman -R package
```

Uninstalls a package along with dependencies not used by other packages.

```bash
pacman -Rs package
```

Allows you to search for a specific package

```bash
pacman -Ss package
```

Download the package but don't install it

```bash
pacman -Sw package
```

Show information about a package not installed

```bash
pacman -Si package
```

Show information about a package already installed

```bash
pacman -Qi package
```

Install only package dependencies.

```bash
pacman -Se package
```

Shows all the files belonging to the package.

```bash
pacman -Ql package
```

Shows the system packages that can be updated, but does not install them.

```bash
pacman -Qu
```

Shows a list of all packages installed on the system.

```bash
pacman -Q
```

Shows which package a particular file belongs to.

```bash
pacman -Qo /ruta/del/archivo
```

Delete all old packages stored in the Pacman cache.

```bash
pacman -Sc
```

Delete all packages stored in the pacman cache located in /var/cache/pacman/pkg.

```bash
pacman -Scc
```

Install a package saved to a local folder.

```bash
pacman -U
```

Prevent pacman from installing programs even if it's already up to date

```bash
sudo pacman -Syu --needed
```

```bash
sudo pacman -S packages --needed
```

## Video drivers
**INTEL (Supports Vulkan gaming):**

**Graphics integrated to the processor**

```bash
sudo pacman -S mesa lib32-mesa
```

**classic OpenGL (non-Gallium3D) drivers**

```bash
sudo pacman -S mesa-amber lib32-mesa-amber 
```

```bash
sudo pacman -S xf86-video-intel vulkan-intel
```

>>> Hardware Acceleration <<<

Hardware video acceleration makes it possible for your video card to

decode and encode video using the GPU (graphics)

Thus unloading the CPU (processor) and saving power

```bash
sudo pacman -S intel-media-driver libva-intel-driver
```

```bash
sudo pacman -S libva-vdpau-driver libvdpau-va-gl
```

```bash
sudo pacman -S libva-utils vdpauinfo
```

>>> GPGPU <<<

/*Try to take advantage of the computing capabilities of a GPU.

/* Thus unloading the CPU and saving energy.

/*On Linux, there are currently two main GPGPU frameworks: OpenCL and CUDA

/*It is widely used in video editing, graphic design, etc...

/*OpenCL works with any program or Game,

/* even in Wine to work with the GPU its use is:

```bash
wine /ruta/a/juego_3d.exe -opengl
```

Installation:

```bash
pacman -S intel-compute-runtime beignet
```

```bash
pacman -S clinfo ocl-icd opencl-headers
```

##  Video codecs

```bash
sudo sudo pacman -S ffmpeg aom libde265 x265 x264 libmpeg2 xvidcore libtheora libvpx schroedinger sdl gstreamer gst-plugins-bad gst-plugins-base gst-plugins-base-libs gst-plugins-good gst-plugins-ugly xine-lib libdvdcss libdvdread dvd+rw-tools lame
```


## support for compressed files
```bash
sudo pacman -S xarchiver unarchiver binutils gzip lha lrzip lzip lz4 p7zip tar xz bzip2 p7zip lbzip2 arj lzop cpio unrar unzip zstd zip lzip unarj zstd
```

