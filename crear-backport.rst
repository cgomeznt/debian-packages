crear jaula

entrar en la jaula

modificar repositorios agregar src del testing

instalar paquetes para hacer el build::
# apt-get install build-essential devscripts

instalar y configurar locales

Descargar el paquete source::
$ apt-get source package

Instalar las dependencias del package::
apt-get build-dep package

Cambiar el changelog del package::
debchange --bpo

Modificar los debian/rules, debian/control

Construir el nuevo package::
$ dpkg-buildpackage -rfakeroot -us -uc

instalar el package

Bloquear el package para que no se actualice::
echo package hold | dpkg --set-selections 

