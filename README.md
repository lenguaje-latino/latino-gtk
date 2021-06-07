# latino-gtk
Liberia dinamica para crear ventanas con GTK

## Instalar en Ubuntu / Linux

Se requieren los siguientes paquetes adicionales

```
apt-get install libgtk-3-dev libgtkmm-3.0-dev

git clone https://github.com/lenguaje-latino/latino-gtk
cd latino-gtk
cmake .
make
make install
```

## Instalar en Windows (MSYS)

Primero instalar MSYS desde aqui https://www.msys2.org/
Posteriormente abrir terminal MSYS y ejecutar los siguientes comandos:

```
pacman -S gcc cmake
pacman -S mingw-w64-x86_64-toolchain

pacman -S mingw-w64-x86_64-gtk3
pacman -S mingw-w64-x86_64-toolchain base-devel

# Configurar PATH
export PKG_CONFIG=/usr/bin/pkg-config
export PKG_CONFIG_PATH=/c/msys64/mingw64/lib/pkgconfig

export PATH=/c/msys64/mingw64/bin:$PATH

# agregar la ruta de la compilacion de latino con msys
# export PATH=/d/work/lenguaje-latino/latino/msys2:$PATH

# Clonar e instalar
git clone https://github.com/lenguaje-latino/latino-gtk
cd latino-gtk
cmake .
make
make install
```

NOTA: Si se corre con msys se debe de compilar latino con MSYS para que sea compatible

## Para compilar latino en msys
```
pacman -S libreadline libreadline-devel

git clone https://github.com/lenguaje-latino/latino
git submodule update --init --recursive
cmake .
make
make install
```

## IMPORTANTE
Esta libreria esta en desarrollo y el ejemplo solo crea una ventana con un boton