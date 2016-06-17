## Curso de empaquetamiento

todos los manuales
https://www.debian.org/doc/

Esto es un manual de referencia
Debian Policy Manual https://www.debian.org/doc/debian-policy/

Este si es más como una guía
Guía del nuevo desarrollador de Debian https://www.debian.org/doc/manuals/maint-guide/

FHS
https://wiki.debian.org/es/FilesystemHierarchyStandard

Practiaca de diff

cat > A << EOF
gato
perro
pajaro
EOF

cp A B

diff -u A B

md5sum A B

vi B
gato
perro
pajaroT

diff -u A B

md5sum A B

diff -u A B > miPrimerDiff.diff

gzip -9 miPrimerDiff.diff

file miPrimerDiff.diff.gz

gunzip miPrimerDiff.diff.gz

zcat miPrimerDiff.diff.gz

zless miPrimerDiff.diff.gz

paquete gems: Shows a console session in several terminals  The gems system is a client/server application that allows one to show a single console session in different computers or terminals in real time. It can also be used to transmit any other kind of data to more than one computer at the same time, via a network connection.

It was designed as an educational tool for teachers that have to show in a computer lab how to do certain things with the console. Using the gems system, each student can observe in his/her own terminal everything the teacher does.

gems-server IP
gems-client IP

hay unas variables de entorno, que es bueno tenerlas configuradas

echo $DEBIANFULLNAME
echo $DEBIANMAIL

Una pausa para informar de algo que ayuda que son los colores en ls y el bash complite
en le .bashrc habilitar
# apt-get install bash-completion

# vi /root/.bashrc
export LS_OPTIONS='--color=auto'
alias ls='ls $LS_OPTIONS'
if [ -f /etc/bash_complition ]; then
	. /etc/bash_complition
fi

con bash_completion de completa los comandos y sus argumentos al igual que los paquetes si fuera aptitude. Cuidado consume mucha memoria y si lo colocas en .bashrc te puede dar problemas
 # aptitude install shutter


Debian paquete fuente, debianizar un paquete.s
el diff.gz tiene las Diferencia/Modificaciones de debian
solo si el paquete fue desarrollado fuera de debian

codigo fuente original 
origin.tar.gz

Debian source control
archivo de control de paquete
archivo.dsc

Paquete binario, realmente es un paquete deb


packages.debian.org lista de paquetes y sus ramas, para descargar binarios o fuentes.

un paquete all significa que no necesita ser compilado porque puede ser interpretado como esta en todas las arquitecturas (un desarrollo en python o perl)
un paquete any significa que debe ser compilado en todas las arquitecturas (un desarrollo en c)
no i386 - significa que debe ser compilado en todas las arquitecturas menos en menos i386

Descargo un binario.
$ wget http://ftp.us.debian.org/debian/pool/main/libb/libbiblio-isis-perl/libbiblio-isis-perl_0.24-1.1_all.deb

Detalle sobre la version
libbiblio-isis-perl_0.24-1.1_all.deb

Nombre del paquete = libbiblio-isis-perl
Versión del desarrollador = 0.24  	Esta es la version que vamos encontrar en la pagina del desarrollador.
Primera revisión en debian = 1 	Es unicamente sobre el empaquetamiento
No manteiner upload = .1 que otra persona que no es el mantenedor subió la modificación de Debian

Los fuentes originales siempre se llaman <nombre-xxx.xxx> fijate en el guion
nginx-1.9.15

Cuando esta debianizado <nombre_xx.xx-x_all.deb> fijate que el guion cambia por underscore y luego un guion para separar la versión de Debian. pero cuando descomprimes se respeta el formato original de la fuente
nginx_1.9.15-1.deb

$ dpkg --info libbiblio-isis-perl_0.24-1.1_all.deb | less

$ dpkg --contents libbiblio-isis-perl_0.24-1.1_all.deb

$ dpkg --install libbiblio-isis-perl_0.24-1.1_all.deb  

$ mkdir desastr
$ cd desastr/
$ dpkg --extract ../libbiblio-isis-perl_0.24-1.1_all.deb .
$ ls -l
total 4
drwxr-xr-x 3 cgome1 cgome1 4096 abr  5  2008 usr

$ find usr/

Forma barata de instalar un paquete de debian, Instala pero sin correr los post, es como un tar -xzfv archivo.tar.gz -C /
$ dpkg --extract ../libbiblio-isis-perl_0.24-1.1_all.deb /

Para recuperar una maquina
# find . -type f -name '*.deb' -exec dpkg --extract '{}' / \;

$ rm -rf usr/

Este hace exactamente lo mismo
$ dpkg-deb -x ../libbiblio-isis-perl_0.24-1.1_all.deb .
$ ls -l
total 4
drwxr-xr-x 3 cgome1 cgome1 4096 abr  5  2008 usr

como un paquete deb es un ar, podemos usar cosas como esto.

$ rm -rf usr/
$ ar -x ../libbiblio-isis-perl_0.24-1.1_all.deb 
$ ls -l
total 44
-rw-r--r-- 1 cgome1 cgome1  1163 abr 24 10:15 control.tar.gz
-rw-r--r-- 1 cgome1 cgome1 36576 abr 24 10:15 data.tar.gz
-rw-r--r-- 1 cgome1 cgome1     4 abr 24 10:15 debian-binary

$ cat debian-binary 
2.0

$ tar -tzf control.tar.gz 
./
./control
./md5sums

$ tar -tzf data.tar.gz # igual a dpkg --contents
./
./usr/
./usr/share/
./usr/share/doc/
./usr/share/doc/libbiblio-isis-perl/
./usr/share/doc/libbiblio-isis-perl/README
./usr/share/doc/libbiblio-isis-perl/changelog.Debian.gz
./usr/share/doc/libbiblio-isis-perl/copyright
./usr/share/doc/libbiblio-isis-perl/changelog.gz
./usr/share/perl5/
./usr/share/perl5/Biblio/
./usr/share/perl5/Biblio/Isis/
./usr/share/perl5/Biblio/Isis/Manual.pod
./usr/share/perl5/Biblio/Isis.pm
./usr/share/man/
./usr/share/man/man3/
./usr/share/man/man3/Biblio::Isis::Manual.3pm.gz
./usr/share/man/man3/Biblio::Isis.3pm.gz


data.tar.gz, contiene los archivos que se van a instalar
control.tar.gz, contienen la debianizacion


$ tar xvzf control.tar.gz 
./
./control
./md5sums
$ ls 
control  control.tar.gz  data.tar.gz  debian-binary  md5sums

$ cat md5sums # Contiene la suma de comprobacion de los archivos
60f9b193aede419292a2221647258838  usr/share/doc/libbiblio-isis-perl/README
8e89898b65590bd3207d3f44b61655a1  usr/share/doc/libbiblio-isis-perl/changelog.Debian.gz
bd246bb6e3315ac902c2f1fdda4abc80  usr/share/doc/libbiblio-isis-perl/copyright
9fd8eb5c6dcf30c3f6530c450ae5784f  usr/share/doc/libbiblio-isis-perl/changelog.gz
879a2c79ab1783d5f00ecb2eb0b599bc  usr/share/perl5/Biblio/Isis/Manual.pod
cf50758117d0a3758996fef88353dc1a  usr/share/perl5/Biblio/Isis.pm
d6abb65ef42d2b3d59f3ab3e1db3f75a  usr/share/man/man3/Biblio::Isis::Manual.3pm.gz
7abc3a365eacfb5fc8fca1f8ac73cf11  usr/share/man/man3/Biblio::Isis.3pm.gz

si modificas los archivos y no modificar el md5sum cuando instales dara error (Esto si es en un binario)

El archivo control.tar.gz dentro de el esta el famoso .dsc (Debian source control)

$ cat control # dpkg --info

Una forma facil de descargar un paquete binario .deb

$ aptitude download nginx 

o 

$ apt-get download nginx

En /usr/share/doc/nombredelpaquete por convención es donde se guardan los archivos de documentación y ahí debe estar el copyright que es obligatorio.

El copyright tiene quien lo debianizo, de donde se descargo, quien es el desarrollador y quien tiene el copyright que fue el que hizo la aplicación y luego la licencia BSD

ISO_2 209

vamos a repasar

# aptitude download nginx
Des: 1 http://ftp.debian.org/debian/ jessie/main nginx all 1.6.2-5 [72,1 kB]
Descargados 72,1 kB en 1s (71,9 kB/s)

# ls -l
total 72
-rw-r--r-- 1 root root 72122 dic  1  2014 nginx_1.6.2-5_all.deb

# mkdir nginx

# dpkg --extract nginx_1.6.2-5_all.deb nginx/
# cd nginx/
# ls -l
total 4
drwxr-xr-x 3 root root 4096 dic  1  2014 usr

# cd ..
# ls
nginx  nginx_1.6.2-5_all.deb

# ar -x nginx_1.6.2-5_all.deb 
root@debian:/tmp/paquete# ls -l
total 156
-rw-r--r-- 1 root root   741 abr 25 14:38 control.tar.gz
-rw-r--r-- 1 root root 71188 abr 25 14:38 data.tar.xz
-rw-r--r-- 1 root root     4 abr 25 14:38 debian-binary
drwxr-xr-x 3 root root  4096 dic  1  2014 nginx

# tar tzf control.tar.gz 
./
./control
./md5sums

# ls
control.tar.gz	data.tar.xz  debian-binary  nginx  nginx_1.6.2-5_all.deb

# mkdir desastr
# dpkg --control nginx_1.6.2-5_all.deb desastr/
# ls -l desastr/
total 8
-rw-r--r-- 1 root root 713 dic  1  2014 control
-rw-r--r-- 1 root root 205 dic  1  2014 md5sums

hay una forma de construir un deb a partir de los binarios, también hay un concepto llamado metapaquetes.

## Antinomia de un paquete fuente como modificarlo

para descargar los fuentes puede ser de tres maneras, desde la pagina y descargando los tres archivos con wget, desde la pagina y ubicar el dsc y utilizar el dget o con apt-get source.

$ apt-get source paquete
y este los descomprime por nosotros, antes de etch no lo descompirmia y debiamos hacer dpkg-deb 

otra forma seria con dget y el link del paquete.dsc
$ dget http://http.debian.net/debian/pool/main/n/nginx/nginx_1.9.14-1.dsc

esta es valida pero lleva mas trabajo. debo descargar los tres paquetes uno a uno.
$ wget http://http.debian.net/debian/pool/main/n/nginx/nginx_1.9.14-1.dsc
$ wget http://http.debian.net/debian/pool/main/n/nginx/nginx_1.9.14.orig.tar.gz
$ http://http.debian.net/debian/pool/main/n/nginx/nginx_1.9.14-1.debian.tar.xz


Para saber en que paquete superior o contenedor se encuetra un paquete.

Si ya esta instalado, identifico cual es su ruta absoluta
# which dget
/usr/bin/dget

Y en package.debian.org coloco http://package.debian.org/file:rutaypaquete y ahi me dira cual es el paquete superior o contenedor
http://package.debian.org/file:/usr/bin/dget

o tambien, si ya esta instalado

# dpkg -S /usr/bin/dget
devscript: /usr/bin/dget

o tambien, buscando en los repositorios

# apt-cache show dget
Suggests: devscripts # buscar esta linea

Para ver el contenido de un paquete instalado.

# dpkg -L dpkg-dev | less
/.
/usr
/usr/share
/usr/share/dpkg
/usr/share/dpkg/default.mk
/usr/share/dpkg/vendor.mk
/usr/share/dpkg/buildflags.mk
/usr/share/dpkg/architecture.mk
[....}
/usr/share/dpkg/pkg-info.mk
/usr/share/doc



Una de las cosas de APT son las dependencias, las dependencia son relaciones que existen entre los paquetes
paquetes reales o paquetes virtuales.
los reales son los .deb y los virtuales podria ser lo creados con update-alternatives
x-window-manager, flash, etc.
httpd, httpd-cgi, pgp, pop, smtp
los paquetes virtuales satisfacen dependencias.
Aqui esta un listado de paquetes virtuales, https://www.debian.org/doc/packaging-manuals/virtual-package-names-list.txt

un paquete real tiene:  15:00min
dependencias
sugerencias
recomendaciones # puede recomendar y pueden ser aceptadas dependiendo como esta configurado aptitude
pre-dependencias # casi siempre es el base, debe estar instalado y configurado antes de instalar el otro
conflictos
reemplazos # ejemplo puedo hacer que instale un paquete y desistele el otro , thunderbird - evolution

Metapaquete, es un paquete real que esta vacio, que tiene un data.tar.gz vacio y un archivo de control que especifica muchas dependencias, por lo normal ese es el funcionamiento. eso te ayuda hacer tasksel.
por ejemplo perfiles de un desarrollador o de un usuario de escritorio, con un metapaquete simplemente puede decir que dependencias va instalar dependiendo del perfil

lee sobre debian epoch que es los <:> en el versionado, es cuando crean mal las versiones y es para evitar errores. por ejemplo paquete-2008.09.21, si luego al desarrollador se el ocurre versionar de forma correcta y utiliza paquete-1.0 este sera visto como uno anterior y para evitar eso se utiliza los epoch, paquete-1:1.0

Vamos a trabajar con paquetes fuentes.

# apt-get source falselogin
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho
Se necesita descargar 9.664 B de archivos fuente.
Des:1 http://ftp.debian.org/debian/ testing/main falselogin 0.3-4 (dsc) [971 B]
Des:2 http://ftp.debian.org/debian/ testing/main falselogin 0.3-4 (tar) [3.361 B]
Des:3 http://ftp.debian.org/debian/ testing/main falselogin 0.3-4 (diff) [5.332 B]
Descargados 9.664 B en 0s (11,5 kB/s) 
gpgv: Firmado el vie 25 jul 2008 04:13:42 UTC usando clave DSA ID 005C3B82
gpgv: Imposible comprobar la firma: clave pública no encontrada
dpkg-source: aviso: se ha detectado un fallo al verificar la firma de ./falselogin_0.3-4.dsc
dpkg-source: información: extrayendo falselogin en falselogin-0.3
dpkg-source: información: desempaquetando falselogin_0.3.orig.tar.gz
dpkg-source: información: aplicando «falselogin_0.3-4.diff.gz»
dpkg-source: información: ficheros de la fuente original que se han modificado: 
 falselogin-0.3/Makefile
 falselogin-0.3/falselogin.c
 falselogin-0.3/falselogin.conf

# ls -l
total 20
drwxr-xr-x 3 root root 4096 abr 25 20:22 falselogin-0.3
-rw-r--r-- 1 root root 5332 jul 25  2008 falselogin_0.3-4.diff.gz
-rw-r--r-- 1 root root  971 jul 25  2008 falselogin_0.3-4.dsc
-rw-r--r-- 1 root root 3361 ago 25  2006 falselogin_0.3.orig.tar.gz

Siempre hay un diff.gz que puede tener parches del codigo fuente, pero por lo normal tiene los cambios de la debianizacion, lo que este dentro del diff no necesariamente debe existir, si no existe el lo crea.

Si modifico el paquete fuente el diff se me va actualizar, claro si utilizo los devscript.

# zless falselogin_0.3-4.diff.gz

Dentro de la carpeta debian, esta la configuración que define el comportamiento de instalacion y configuracion de un paquete. hay tres archivos importante
changelog	registro de cambios
control	tal como lo dice su nombre, control
rules		es un ejecutable que va construir el paquete binario. el le pasa los parametros al configure y lo envoca.

El archivo changelog, ste es un fichero requerido, con un formato especial descrito en Debian Policy Manual, 4.4 "debian/changelog". Este es el formato utilizado por dpkg y otros programas para obtener el número de versión, revisión, distribución y urgencia de tu paquete.

Para ti es también importante, ya que es bueno tener documentados todos los cambios que hayas hecho. Esto ayudará a las personas que se descarguen tu paquete para ver si hay temas pendientes en el paquete que deberían conocer de forma inmediata. Se guardará como /usr/share/doc/gentoo/changelog.Debian.gz en el paquete binario.

No se detalla que hizo el desarrollador, se detalla lo que hizo el mantenedor. cuando mucho decir que es un <new upstream release> cuando el desarrollador realizo modificaciones

dh_make genera uno predeterminado
1  gentoo (0.9.12-1) unstable; urgency=low
2
3   * Initial release (Closes: #nnnn)  <nnnn 
    is the bug number of your ITP>
4
5  -- Josip Rodin <joy-mg@debian.org>  Mon, 22 Mar 2010 00:37:31 +0100 

tambien con dch podemos hacerlo

# dpkg -S `which dch`
devscripts: /usr/bin/dch

# dch -i


Editemos algo en el archivo fuente , puede ser un println

# vi falselogin.c

Ahora vamos a compilar el paquete, una forma de hacerlo es

# debian/rules binary	# esto es porque es un make, pero deja por fuera toda la configuracion contenida en debian/

realmente para compilarlo es 

# dpkg-buildpackage 

o 

# debuild

Luego que lo compila se vera la misma estructura solo que se le incremento la version. es decir, otros diff, dsc y tar.gz

# which debdiff
/usr/bin/debdiff
# dpkg -S /usr/bin/debdiff
devscripts: /usr/bin/debdiff


repasamos algo

# mkdir desastre
# dpkg --extract nginx_1.6.2-5_all.deb desastre/
# ls desastre/
usr/

podiamos hacer cosas con ar, cuando es un binario

# ar -x  ../nginx_1.6.2-5_all.deb 
root@debian:/paquete/desastre# ls 
control.tar.gz  data.tar.xz  debian-binary  usr

# apt-get download nginx
Des:1 http://ftp.debian.org/debian/ jessie/main nginx all 1.6.2-5 [72,1 kB]
Descargados 72,1 kB en 0s (73,9 kB/s)

# mkdir desastre
# dpkg --control nginx_1.6.2-5_all.deb desastre/
# ls desastre/
control  md5sums


## Backports 

vamos a ver como descomprimir el .dsc y el tar.gz. lo podemos hacer a mano pero tenemos esta herramienta dpkg-source -x paquete.
buscamos un paquete en testing para hacer el ejercicio

# dget http://http.debian.net/debian/pool/main/x/xterm/xterm_324-1.dsc
# ls -l 
total 1316
drwxr-xr-x 8 root root    4096 abr 25 21:48 xterm-324
-rw-r--r-- 1 root root  102088 mar 12 09:47 xterm_324-1.diff.gz
-rw-r--r-- 1 root root    2055 mar 12 09:47 xterm_324-1.dsc
-rw-r--r-- 1 root root 1235312 mar 12 09:47 xterm_324.orig.tar.gz
# rm -rf xterm-324/
# ls -l
total 1312
-rw-r--r-- 1 root root  102088 mar 12 09:47 xterm_324-1.diff.gz
-rw-r--r-- 1 root root    2055 mar 12 09:47 xterm_324-1.dsc
-rw-r--r-- 1 root root 1235312 mar 12 09:47 xterm_324.orig.tar.gz

# dpkg-source -x xterm_324-1.dsc 
dpkg-source: información: extrayendo xterm en xterm-324
dpkg-source: información: desempaquetando xterm_324.orig.tar.gz
dpkg-source: información: aplicando «xterm_324-1.diff.gz»
root@debian:/paquete# ls -l
total 1316
drwxr-xr-x 8 root root    4096 abr 25 21:52 xterm-324
-rw-r--r-- 1 root root  102088 mar 12 09:47 xterm_324-1.diff.gz
-rw-r--r-- 1 root root    2055 mar 12 09:47 xterm_324-1.dsc
-rw-r--r-- 1 root root 1235312 mar 12 09:47 xterm_324.orig.tar.gz


# cd xterm-324

y lo compilamos

# dpkg-buildpackage

Nos dara error por las dependencias, pues simple vamos instalando las dependencias que nos indique.

Recordemos que hay tres formas de compilar un paquete
# debian/rule binary
# dpkg-buildpackage
# debuild

Los tres anteriores debemos tener las dependecias cumplidas, por eso es bueno utilizar jaulas para no embasurar nuestro sistema y tambien podemos utilizar pbuilder (jaula), instalamos pbuilder y simplemente le decimos

# rm -rf xterm-324
# ls 
xterm_324-1.diff.gz  xterm_324-1.dsc  xterm_324.orig.tar.gz

# apt-get install pbuilder

# pbuilder build xterm_324-1.dsc

No hay que descomprimir, no hay que hacer nada el descomprime su tar.gz base donde crea la jaula y continua con el proceso de resolver dependencias, debianza y crear el paquete, cuando termina de crear el paquete el borra todo.
si este metodo falla es muy probable que sea dificil de crear el backport.

Siempre da error en las dependencias de control (build-dependes) lo que se hace pero no siempre funciona es editar el archivo de control y en build-dependes buscar las dependencia y bajarle la version a la que tenemos en nustra version de debian estable.

Al modificar debian/control, debemos ejecutar debuild o dpkg-buildpackage, pero podemos integrar todo nuevamente dentro del archivo.dsc con:

# dpkg-source -b xterm_324-1  # es la ruta de la carpeta

y ejecutamos nuevamente 

# pbuilder build xterm_324-1
I: Configuring cpp-5...
I: Configuring libstdc++-5-dev:amd64...
I: Configuring cpp...
I: Configuring libdpkg-perl...
I: Configuring dpkg-dev...
I: Configuring gcc-5...
I: Configuring g++-5...
I: Configuring gcc...
I: Configuring g++...
The following NEW packages will be installed:
  aptitude aptitude-common libboost-filesystem1.58.0 libboost-iostreams1.58.0 libboost-system1.58.0 libcwidget3v5
  libsigc++-2.0-0v5 libsqlite3-0 libxapian22v5
0 upgraded, 9 newly installed, 0 to remove and 0 not upgraded.
Need to get 5054 kB of archives.
After this operation, 19.3 MB of additional disk space will be used.
Get:1 http://ftp.debian.org/debian sid/main amd64 libboost-iostreams1.58.0 amd64 1.58.0+dfsg-5+b1 [51.1 kB]
Get:2 http://ftp.debian.org/debian sid/main amd64 libxapian22v5 amd64 1.2.23-1 [979 kB]
Get:3 http://ftp.debian.org/debian sid/main amd64 libsqlite3-0 amd64 3.12.2-1 [540 kB]
Get:4 http://ftp.debian.org/debian sid/main amd64 aptitude-common all 0.8-1 [1572 kB]
Get:5 http://ftp.debian.org/debian sid/main amd64 libboost-system1.58.0 amd64 1.58.0+dfsg-5+b1 [31.1 kB]
Get:6 http://ftp.debian.org/debian sid/main amd64 libboost-filesystem1.58.0 amd64 1.58.0+dfsg-5+b1 [61.0 kB]
Get:7 http://ftp.debian.org/debian sid/main amd64 libsigc++-2.0-0v5 amd64 2.8.0-1 [59.6 kB]
Get:8 http://ftp.debian.org/debian sid/main amd64 libcwidget3v5 amd64 0.5.17-4+b1 [313 kB]
Get:9 http://ftp.debian.org/debian sid/main amd64 aptitude amd64 0.8-1 [1447 kB]
I: unmounting dev/pts filesystem
I: unmounting run/shm filesystem
I: unmounting proc filesystem
I: creating base tarball [/var/cache/pbuilder/base.tgz]
I: cleaning the build env 
I: removing directory /var/cache/pbuilder/build//20698 and its subdirectories


Mira donde se crea la jaula.
I: creating base tarball [/var/cache/pbuilder/base.tgz]
I: cleaning the build env 
I: removing directory /var/cache/pbuilder/build//20698 and its subdirectories
Aqui coloca los binarios
# ls -l /var/cache/pbuilder/result/



11:50

Veamos otra forma de tener control sobre el fuente, puede darse el caso que queramos activar funciones o al reves quitarle funciones al aplicativo.


# dget http://http.debian.net/debian/pool/main/n/nginx/nginx_1.9.10-1.dsc

fijate que en nginx-1.9.10 esta el configure que tiene los parametros del fuente y el es llamado desde el debian/rule, vamos editar al debian/rule y le agregamos o quitamos funciones, buscamos la linea de configure y ahi quitamos o ponemos.
# configure --help para que nos muestre las opciones y queremos activar, por ejemplo --with-debug. entonces editamos debian/rules y buscamos la linea de configure y ahi lo colocamos --with-debug

# vi debian/rules

Si despues de esta modificacion sabemos que requerimos un paquete adicional, es decir dependencias, nos vamos a debian/control y en build-depends se agrega las dependencias.

# vi debian/control

Siempre despues de hacer un cambio ejecutamos es bueno registrarlo en changelog y como es un backport por convencion se el coloca bpo.

# dch -i # parar modificar el changelog
# debchange --bpo # otra forma de modificar changelog

Despues de crear nuestro paquete backport vamos a crear una rama personalizada en nuestro repo para tenerlo separado.

# cd /srv/www/onuva/repositorios/
# mkdir backports
# cd backports/
# mkdir conf
# cp -a chamos/conf/distributions backports/conf/
# cd conf/
# ls 
distributions
# vi distributions
Origin: Onuva
Label: Backports
Suite: backports
Codename: backports
Version: 1.0
Architectures: i386
Components: main
Description: Backports para Debian 4.0

# cd ..
# pwd
/srv/www/onuva/repositorios/backports
# reprepro
# find /var/cache/pbuilder/result/ -type f -name '*.deb' -exec reprepro includedeb backports '{}' \;

Ahora puedes ir al mirror y ver ahi tus paquetes backporteados

todos los manuales
https://www.debian.org/doc/


Esto es un manual de referencia
Debian Policy Manual https://www.debian.org/doc/debian-policy/

Este si es más como una guia
Guía del nuevo desarrollador de Debian https://www.debian.org/doc/manuals/maint-guide/

FHS
https://wiki.debian.org/es/FilesystemHierarchyStandard

resumen
tener configurado el source.list con el deb-src
Descargar paquete fuente 
descomprimir el paquete fuente
modificar
cambiar el changelog
compilar



ISO_3 214

Un Fuente lo vamos a Debianizar

dh-make - Herramienta para convertir archivos de código fuente en paquetes de código fuente de Debian

# apt-get install dh-make

root@debian:/paquete# mkdir nginx-1.9.10 
root@debian:/paquete# cp nginx/nginx_1.9.10.orig.tar.gz nginx-1.9.10/
root@debian:/paquete# cd nginx-1.9.10/ls
bash: cd: nginx-1.9.10/ls: No existe el fichero o el directorio
root@debian:/paquete# cd nginx-1.9.10/
root@debian:/paquete/nginx-1.9.10# ls -l
total 872
-rw-r--r-- 1 root root 889267 abr 27 20:52 nginx_1.9.10.orig.tar.gz
root@debian:/paquete/nginx-1.9.10# 
root@debian:/paquete/nginx-1.9.10# dh_make -f nginx_1.9.10.orig.tar.gz -c gpl

Type of package: single binary, indep binary, multiple binary, library, kernel module, kernel patch?
 [s/i/m/l/k/n] s

Maintainer name  : root
Email-Address    : root@debian 
Date             : Wed, 27 Apr 2016 20:53:25 +0000
Package Name     : nginx
Version          : 1.9.10
License          : gpl3
Type of Package  : Single
Hit <enter> to confirm: 
Currently there is no top level Makefile. This may require additional tuning.
Done. Please edit the files in the debian/ subdirectory now. You should also
check that the nginx Makefiles install into $DESTDIR and not in / .

root@debian:/paquete/nginx-1.9.10# ls
debian  nginx_1.9.10.orig.tar.gz
root@debian:/paquete/nginx-1.9.10# cd debian/
root@debian:/paquete/nginx-1.9.10/debian# ls
changelog  copyright  manpage.1.ex     menu.ex           nginx.doc-base.EX  preinst.ex     README.source  watch.ex
compat     docs       manpage.sgml.ex  nginx.cron.d.ex   postinst.ex        prerm.ex       rules
control    init.d.ex  manpage.xml.ex   nginx.default.ex  postrm.ex          README.Debian  source

root@debian:/paquete/nginx-1.9.10/debian# rm *.ex
root@debian:/paquete/nginx-1.9.10/debian# ls
changelog  compat  control  copyright  docs  nginx.doc-base.EX  README.Debian  README.source  rules  source
root@debian:/paquete/nginx-1.9.10/debian# cat compat 
9
root@debian:/paquete/nginx-1.9.10/debian# cat docs 
root@debian:/paquete/nginx-1.9.10/debian# cat control 
Source: nginx
Section: unknown
Priority: optional
Maintainer: root <root@debian>
Build-Depends: debhelper (>= 9)
Standards-Version: 3.9.5
Homepage: <insert the upstream URL, if relevant>
#Vcs-Git: git://anonscm.debian.org/collab-maint/nginx.git
#Vcs-Browser: http://anonscm.debian.org/?p=collab-maint/nginx.git;a=summary

Package: nginx
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: <insert up to 60 chars description>
 <insert long description, indented with spaces>


El lo que hace es copiar sus plantillas y remplazarlas con el nombre de la aplizacion.

# ls /usr/share/debhelper/
autoscripts  dh_make
# ls /usr/share/debhelper/dh_make/
debian  debiani  debiank  debianl  debianm  debiann  debians  emacs  licenses  native


los .ex significan example

En control vemos que no estan las dependencias, porque el no detecta eso. para identificar las dependencia podemos ir a la pagina del desarrollador y ver cuales son o crear una jaunal e instalarlo e ir viendo las dependencias

compilamos

# debuild

genera error por las firmas

# dpkg-buildpackage -us -us
root@debian:/paquete/xterm-324# cd ..

root@debian:/paquete# ls *deb
xterm_324-1_amd64.deb

termina exitosamente 

El constructor del paquete envoca al debian/rule que llama al .configure, si aqui nos da error leemos bien y podemos editar el rules


