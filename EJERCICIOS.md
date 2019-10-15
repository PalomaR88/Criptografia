
## **TAREA 1**
Los algoritmos de cifrado asimétrico utilizan dos claves para el cifrado y descifrado de mensajes. Cada persona involucrada (receptor y emisor) debe disponer, por tanto, de una pareja de claves pública y privada. Para generar nuestra pareja de claves con gpg utilizamos la opción --gen-key:

Para esta práctica no es necesario que indiquemos frase de paso en la generación de las claves (al menos para la clave pública).

1. Genera un par de claves (pública y privada). ¿En que directorio se guarda las claves de un usuario?
~~~
gpg --gen-key
~~~

Tras el comando anterior e introducir la información que nos pide, aparece el siguiente mensaje donde nos indica dónde está nuestro certificado:
~~~
gpg: clave FC46CB5C8A677ED7 marcada como de confianza absoluta
gpg: creado el directorio '/home/paloma/.gnupg/openpgp-revocs.d'
gpg: certificado de revocación guardado como '/home/paloma/.gnupg/openpgp-revocs.d/AEBE54F703EF8342317B6782FC46CB5C8A677ED7.rev'
claves pública y secreta creadas y firmadas.

pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
      AEBE54F703EF8342317B6782FC46CB5C8A677ED7
uid                      Paloma R. Garcia Campon <palomagarciacampon@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
~~~

A continuación, se exportan las claves públicas y privadas con el certificado que hemos hecho anteriormente.:
~~~
$ gpg --export -a Paloma R. Garcia Campon > clave-pub.key
$ gpg --export-secret-key -a Paloma R. Garcia Campon > clave-pri.key
~~~

Al usar el segundo comando, el que crea la clave privada, nos pide la frase de paso, si la hemos introducido al crear el certificado. 


2. Lista las claves públicas que tienes en tu almacén de claves. Explica los distintos datos que nos muestra. ¿Cómo deberías haber generado las claves para indicar, por ejemplo, que tenga un 1 mes de validez?

~~~
$ gpg --list-key
gpg: comprobando base de datos de confianza
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: nivel: 0  validez:   1  firmada:   0  confianza: 0-, 0q, 0n, 0m, 0f, 1u
gpg: siguiente comprobación de base de datos de confianza el: 2021-10-06
/home/paloma/.gnupg/pubring.kbx
-------------------------------
# cuando fue creada y cuando caduca
pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
#certificado
      AEBE54F703EF8342317B6782FC46CB5C8A677ED7
# información sobre el dueño de la clave
uid        [  absoluta ] Paloma R. Garcia Campon <palomagarciacampon@gmail.com>
# subkeys
sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
~~~

Para indicar que la clave caduca en un mes hay que introducir el siquiente comando:
~~~
$ gpg --full-generate-key
~~~

Tras esto se piden la configuración de la clave que se va a crear, entre ellas la validez:
~~~
Por favor, especifique el período de validez de la clave.
         0 = la clave nunca caduca
      <n>  = la clave caduca en n días
      <n>w = la clave caduca en n semanas
      <n>m = la clave caduca en n meses
      <n>y = la clave caduca en n años
¿Validez de la clave (0)? 1m
La clave caduca mié 06 nov 2019 11:11:50 CET
¿Es correcto? (s/n) s
~~~

Al volver a listar las claves, aparece la nueva clave con la fecha de expiración correcta:
~~~
$ gpg --list-keys
gpg: comprobando base de datos de confianza
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: nivel: 0  validez:   1  firmada:   0  confianza: 0-, 0q, 0n, 0m, 0f, 1u
gpg: siguiente comprobación de base de datos de confianza el: 2019-11-06
/home/paloma/.gnupg/pubring.kbx
-------------------------------
pub   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
      FD52AA5481FD0E1518011D83952393CC31351D81
uid        [  absoluta ] Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2019-11-06
~~~



3. Lista las claves privadas de tu almacén de claves.
~~~
$ gpg --list-secret-keys
/home/paloma/.gnupg/pubring.kbx
-------------------------------
sec   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
      FD52AA5481FD0E1518011D83952393CC31351D81
uid        [  absoluta ] Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
ssb   rsa3072 2019-10-07 [E] [caduca: 2019-11-06]
~~~



## **TAREA 2**
Para enviar archivos cifrados a otras personas, necesitamos disponer de sus claves públicas. De la misma manera, si queremos que cierta persona pueda enviarnos datos cifrados, ésta necesita conocer nuestra clave pública. Para ello, podemos hacérsela llegar por email por ejemplo. Cuando recibamos una clave pública de otra persona, ésta deberemos incluirla en nuestro keyring o anillo de claves, que es el lugar donde se almacenan todas las claves públicas de las que disponemos.

1. Exporta tu clave pública en formato ASCII y guardalo en un archivo nombre_apellido.asc y envíalo al compañero con el que vas a hacer esta práctica.

~~~
$ gpg --export -a Paloma R. Garcia Campon > PalomaR.Garcia.asc
$ scp PalomaR.Garcia.asc ftirado@172.22.9.13:
~~~


2. Importa las claves públicas recibidas de vuestro compañero.

~~~
gpg --import Fernando_tirado.asc 
~~~


3. Comprueba que las claves se han incluido correctamente en vuestro keyring.

~~~
$ gpg --list-keys
/home/paloma/.gnupg/pubring.kbx
-------------------------------
pub   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
      FD52AA5481FD0E1518011D83952393CC31351D81
uid        [  absoluta ] Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2019-11-06]

pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
      6C5D8AB436F523FE8A329B2EB9199E388C3AD7D5
uid        [desconocida] Fernando Tirado <fernando.tb.95@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
~~~


## **TAREA 3**
Tras realizar el ejercicio anterior, podemos enviar ya documentos cifrados utilizando la clave pública de los destinatarios del mensaje.

1. Cifraremos un archivo cualquiera y lo remitiremos por email a uno de nuestros compañeros que nos proporcionó su clave pública.
~~~
paloma@coatlicue:~$ touch prueba.txt
paloma@coatlicue:~$ gpg -e -u "Paloma R. Garcia Campon" -r "Fernando Tirado" prueba.txt 
gpg: 3E2F77802034ED53: No hay seguridad de que esta clave pertenezca realmente
al usuario que se nombra

sub  rsa3072/3E2F77802034ED53 2019-10-07 Fernando Tirado <fernando.tb.95@gmail.com>
 Huella clave primaria: 6C5D 8AB4 36F5 23FE 8A32  9B2E B919 9E38 8C3A D7D5
      Huella de subclave: D904 FECE 4E2D ACC9 5F3E  9BA9 3E2F 7780 2034 ED53

No es seguro que la clave pertenezca a la persona que se nombra en el
identificador de usuario. Si *realmente* sabe lo que está haciendo,
puede contestar sí a la siguiente pregunta.

¿Usar esta clave de todas formas? (s/N) s
~~~


2. Nuestro compañero, a su vez, nos remitirá un archivo cifrado para que nosotros lo descifremos.
~~~
paloma@coatlicue:~$ ls
 palomita.txt.gpg
~~~


3. Tanto nosotros como nuestro compañero comprobaremos que hemos podido descifrar los mensajes recibidos respectivamente.
~~~
paloma@coatlicue:~$ gpg -d palomita.txt.gpg 
gpg: cifrado con clave de 3072 bits RSA, ID D7F9FDBB7151CA51, creada el 2019-10-07
      "Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>"
~~~


4. Por último, enviaremos el documento cifrado a alguien que no estaba en la lista de destinatarios y comprobaremos que este usuario no podrá descifrar este archivo.
~~~
$ gpg -d /tmp/mozilla_paloma0/prueba.txt.gpg 
gpg: cifrado con clave RSA, ID EDDAA98D51C90308
gpg: descifrado fallido: No secret key
~~~

5. Para terminar, indica los comandos necesarios para borrar las claves públicas y privadas que posees.
Para eliminar una clave pública se utiliza la opción --dekete-key:
~~~
gpg --delete-key Fernando Tirado
~~~


## **Tarea 4**: Exportar clave a un servidor público de claves PGP (2 puntos)

Para distribuir las claves públicas es mucho más habitual utilizar un servidor específico para distribuirlas, que permite a los clientes añadir las claves públicas a sus anillos de forma mucho más sencilla.

1. Genera la clave de revocación de tu clave pública para utilizarla en caso de que haya problemas.

~~~
paloma@coatlicue:~$ gpg --output revoke.paloma.asc --gen-revoke FD52AA5481FD0E1518011D83952393CC31351D81

sec  rsa3072/952393CC31351D81 2019-10-07 Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>

¿Crear un certificado de revocación para esta clave? (s/N) s
Por favor elija una razón para la revocación:
  0 = No se dio ninguna razón
  1 = La clave ha sido comprometida
  2 = La clave ha sido reemplazada
  3 = La clave ya no está en uso
  Q = Cancelar
(Probablemente quería seleccionar 1 aquí)
¿Su decisión? 0
Introduzca una descripción opcional; acábela con una línea vacía:
> Tarea 4
> 
Razón para la revocación: No se dio ninguna razón
Tarea 4
¿Es correcto? (s/N) s
se fuerza salida con armadura ASCII.
Certificado de revocación creado.

Por favor consérvelo en un medio que pueda esconder; si alguien consigue
acceso a este certificado puede usarlo para inutilizar su clave.
Es inteligente imprimir este certificado y guardarlo en otro lugar, por
si acaso su medio resulta imposible de leer. Pero precaución: ¡el sistema
de impresión de su máquina podría almacenar los datos y hacerlos accesibles
a otras personas!
~~~


2. Exporta tu clave pública al servidor pgp.rediris.es

~~~
paloma@coatlicue:~$ gpg --keyserver pgp.rediris.es --send-key FD52AA5481FD0E1518011D83952393CC31351D81
gpg: enviando clave 952393CC31351D81 a hkp://pgp.rediris.es
~~~

3. Borra la clave pública de alguno de tus compañeros de clase e impórtala ahora del servidor público de rediris.

~~~
paloma@coatlicue:~$ gpg --delete-key A615146E82E272C899A34100F405106DBBABFB72
gpg (GnuPG) 2.2.12; Copyright (C) 2018 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.


pub  rsa3072/F405106DBBABFB72 2019-10-07 Fernando Tirado <fernando.tb.95@gmail.com>

¿Eliminar esta clave del anillo? (s/N) s
paloma@coatlicue:~$ gpg --list-public-keys 
/home/paloma/.gnupg/pubring.kbx
-------------------------------
pub   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
      FD52AA5481FD0E1518011D83952393CC31351D81
uid        [  absoluta ] Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2019-11-06]

paloma@coatlicue:~$ gpg --keyserver pgp.rediris.es --search-keys Fernando Tirado
gpg: data source: http://130.206.1.8:11371
(1)	Fernando Tirado <fernando.tb.95@gmail.com>
	  3072 bit RSA key F405106DBBABFB72, creado: 2019-10-07, caduca: 2019-11-06
(2)	Luis Fernando Tirado Correa <luisftc@geocities.com>
	  1024 bit DSA key 692E4E36BAB56E2E, creado: 1998-08-03
Keys 1-2 of 2 for "Fernando Tirado".  Introduzca número(s), O)tro, o F)in > 1
gpg: clave F405106DBBABFB72: clave pública "Fernando Tirado <fernando.tb.95@gmail.com>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1
paloma@coatlicue:~$ gpg --list-public-keys 
/home/paloma/.gnupg/pubring.kbx
-------------------------------
pub   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
      FD52AA5481FD0E1518011D83952393CC31351D81
uid        [  absoluta ] Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2019-11-06]

pub   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
      A615146E82E272C899A34100F405106DBBABFB72
uid        [desconocida] Fernando Tirado <fernando.tb.95@gmail.com>
sub   rsa3072 2019-10-07 [E] [caduca: 2019-11-06]
~~~


## **TAREA 5**
1. Genera un par de claves (pública y privada).
Para obtener la privada:
~~~
paloma@coatlicue:~$ sudo openssl genrsa -out paloma.pem 2048
[sudo] password for paloma: 
Generating RSA private key, 2048 bit long modulus (2 primes)
..........................+++++
...............................................................................+++++
e is 65537 (0x010001)
~~~

Para la pública
~~~
paloma@coatlicue:~$ sudo openssl rsa -in paloma.pem -out paloma.pub.pem
writing RSA key
~~~

2. Envía tu clave pública a un compañero.
~~~
paloma@coatlicue:~$ sudo scp paloma.pub.pem ftirado@172.22.7.162:
The authenticity of host '172.22.7.162 (172.22.7.162)' can't be established.
ECDSA key fingerprint is SHA256:r4+K2WQuci48+03tIUgQRoaoA52Y1RNBvXZtmwP97jg.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.22.7.162' (ECDSA) to the list of known hosts.
ftirado@172.22.7.162's password: 
paloma.pub.pem
~~~


3. Utilizando la clave pública cifra un fichero de texto y envíalo a tu compañero.
~~~
paloma@coatlicue:~$ openssl rsautl -encrypt -in fichero.txt -out fich.enc -inkey miclave-pub.pem 
~~~


4. Tu compañero te ha mandado un fichero cifrado, muestra el proceso para el descifrado.
~~~
paloma@coatlicue:~$ cat paloma.txt.enc 
U2FsdGVkX1/R8nWjMed34IyJNPkyZTadbtfWA3VSb71OQRjMAD/g7b6zYhv4f5Vp

paloma@coatlicue:~$ openssl aes-256-cbc -d -a -in paloma.txt.enc -out paloma.txt.new
enter aes-256-cbc decryption password:
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.

paloma@coatlicue:~$ cat paloma.txt.new 
fuera de mi trono
~~~

