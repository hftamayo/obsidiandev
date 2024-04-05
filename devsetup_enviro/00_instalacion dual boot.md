
**

Paso 1: desactivar actualizaciones

1. Desde el menu inicio: escribir “configuracion”
    
2. Actualizacion y seguridad
    
3. Opciones avanzadas
    
4. Pausar actualizaciones (maximo 35 días), esta funcion limita la busqueda de actualizaciones
    

  

Paso 2: desactivar descargar actualizaciones desde el group policy

1. Cargar el simbolo del sistema como administrador
    
2. Gpedit.msc
    
3. Configuracion del equipo
    
4. Plantillas administrativas
    
5. Componentes de windows
    
6. Windows Update
    
7. Doble click en la opcion “configurar actualizaciones automaticas” y luego paso al estado “deshabilitada”
    

  

Paso 3: deshabilitar el servicio desde el registro:

1. Cargar el simbolo del sistema como administrador
    
2. Services.msc
    
3. En servicios locales buscar al final “Windows Update”
    
4. Doble click
    
5. Tipo de inicio: deshabilitado
    

  

Paso 4: liberar espacio en disco:

1. Desde el menu inicio “disk”
    
2. Aparece liberar espacio en disco
    
3. Va a calcular la data que esta muerta, la selecciono y presionar ok
    

  

Paso 5: redimensionar el disco duro desde windows

1. Desde el menu inicio “administracion de equipos” como administrador
    
2. Opcion “administracion de equipos”
    
3. Inicialmente me aparecían disponibles nada más 20 GB para redimensionar
    
4. Defragmente y elimine un monton de archivos
    
5. Llegué a tener disponible 300 GB libres pero aun asi logre tener para redimension 63 GB 
    
6. Descargué una utilería que se llama minitool partition wizard y ahi pude darmke cuenta que el sistema de archivos reportado es bitlocker (en lugar de ntfs) por tanto en este herramienta no se puede redimensionar
    
7. Entonces voy a recurrir al servicio de soporte dell
    
8. El ID de soporte del equipo es 4J2C063, lo descubrí utilizando el enlace de soporte de la sección “acerca de” del id del producto de windows
    
9. Me pide abrir una cuenta en Dell EMC, las credenciales son [herbert.fernandez@oj.gob.sv](mailto:herbert.fernandez@oj.gob.sv) at Mh38103785$
    

  

Paso 6: como resolvi al final la redimensión del disco duro:

1. El soporte de Dell me atendió y me dijeron que ellos no bloquean la instalacion de Windows de fabrica, me refirieron a Microsoft
    
2. Desde la web de microsoft no hay un enlace que facilmente distinga la manera de cómo obtener soporte de chat para tech support, me dirijí a la cuenta de twitter de microsoft help y desde ahi me dieron los pasos para solicitar chat tech support
    
3. El lunes 3 un agente me dejo darle toda la historia para luego derivarme a tier 1
    
4. Intenté el miercoles 5 y no tuve forma de obtener cupo en linea
    
5. Viernes 7 obtuve espacio a las 8 am y una agente me dijo que usar la herramienta chkdsk, el test se tardó casi 3 horas
    
6. Luego hice cola desde las 10:30 y me atendieron a las 15:30, el agente se tardó casi 2 horas comingo, remotio la maquina e hizo de todo hasta darse por vencido
    
7. Diagnóstico: al parecer windows 10 establece de manera unilateral cual es el monto maximo para redimensionar un volumen, según me dijo el ultimo agente es por aspecto de seguridad.
    
8. Caso cerrado con los vendors: no sirvió de nada el soporte aun teniendo soporte del fabricante de hardware y software
    

  

Paso 7: redimensionando el disco duro:

1. Ente tantos intentos, el clavo principal era que el bitlocker esta pendiente de activarse, si no se quitaba eso era imposible utilizar una herramienta de tercero
    
2. Asi que si vuelve a pasar eso hay que quitarle ese bitlocker, recuerdo que una vez use un comando de consola el lunes 3, este comando la agente me lo recomendó, al siguiente día que use la compu ya no aparecía activo
    
3. Descargué un programa que se llama minitool partition wizard, redimensione la partition a 150 GB, despues de reiniciar windows funciona bien
    

  

Paso 8: pasos previos a cargar la distro seleccionada de linux para dual boot

1. Usé balenaetcher para extender la imagen de una distro de system76  que se llama popos, descargue la versión que no tiene los drivers nvidia 
    
2. Windows 10 previamente ya tiene desactivado actualizaciones automaticas
    
3. En el firmware de la compu tuve que desactivar secure boot, luego de eso la ditro cargó
    

  

Paso 9: instalación de popOS

1. Por default en esta dell hay 5 particiones: la primera es la UEFI de Windows, luego el Windows Volume, luego las 2 de recuperaciones
    
2. Desde el instalador de popOS no puede usarse el espacio sin particionar que hice pero si carga gparted
    
3. Por medio de 2 excelentes videos de youtube entendí que debo hacer con el espacio libre:
    
4. De 150 GB lo distribuí asi:
    
5. 512 MB para una partición Fat32 con flag de boot,esp
    
6. 8 GB para swap
    
7. El resto para root
    
8. A partir de ahi siguió la instalacion sin problemas y al reiniciar cargó PopOS
    

**

**![](https://lh7-us.googleusercontent.com/BWPHSvNFFTmMDRleqNKmoJpLmHKf5YyQk3R3hHennjQr2WSGcVfqDzQd7TNhKuFUcrRfgAr_JahF4bTyCJImW1Qe3LfJRH4jqQbFUwkHcVkcsATtfY20F9FOq6xKdvl92Bj8ohXGfnMRjcO9CBrPQdk)**

**

Paso 10: instalar grub para dual boot

1. Para tener una instalación dual boot si y solo si se necesita grub, las versiones actuales de linux instalan boot loader pero éste no funciona para cargar mas de un sistema operativo
    
2. Estos son los comandos para instalar grub y configurarlo
    
3. sudo apt install os-prober
    
4. sudo apt install grub-efi grub2-common 
    
5. sudo grub-install
    
6. sudo cp /boot/grub/x86_64-efi/grub.efi /boot/efi/EFI/pop/grubx64.efi
    
7. Instalar grub customizer:
    
8. sudo add-apt-repository ppa:danielrichter2007/grub-customizer
    
9. Sudo apt install grub-customizer
    
10. Inmediatamente ejecutar grub-customizer, generar el menu grub y ajustar el tiempo de elección de las opciones, si no hago esto voy a caer en el prompt de grub cuando reinicie.
    
11. En caso que eso pase en mi equipo desktop intelli3 el firmware en f12 tiene la opcion de seleccionar la partición que quiero arrancar, elijo pop os, ejeucto grub customizer y me va a cargar grub
    

  

Fin del proceso.

  
  

Paso 11: escribir en la partición Windows desde Linux

  

1. Desde windows abro un cmd en modo administrador
    
2. Powercfg.exe -h off
    
3. Powercfg.exe /hibernate off
    

  

Paso 12: clonar mi disco de 256GB a uno de 480 GB

sudo dd if=/dev/sdb3 of=/media/hftamayo/OS/epi6/lolo/home.img bs=1k conv=noerror, sync status=progress

  

sudo dd if=/dev/sdb5 of=/media/hftamayo/OS/epi6/lolo/kali.img bs=1k conv=noerror, sync status=progress

  

sudo dd if=/media/hftamayo/OS/epi6/lolo/kali.img of=/dev/sdb5 bs=1k conv=noerror, sync status=progress

  

sudo dd if=/media/hftamayo/OS/epi6/lolo/home.img of=/dev/sdb6 bs=1k conv=noerror, sync status=progress

  

Revisar y corregir la partición de kali que me daba error:

  

sudo fsck.ext4 -c -f -v /dev/sdb5

  

09/08-> al inicio de la jornada no habia conectividad en los access point y eso afectaba el core de los switches, les toco reiniciar el core y los switches tocó al menos 3 veces -> DoS para los Access point se cae todo, mas o menos tardó como 1 hora

  

Si instalo otro sistema operativo y se formatea la swap, al iniciar popOS me va a generar un warning que esta esperando un dispositivo en /dev/cryptswap, para corregir esto:

  

lsblk -f -> para obtener el SUUID actual de la swap

Sudo nano /etc/crypttab -> actualizo el nuevo SUUID

En /etc/fstab no aparece la referencia directa del SUUID de la swap asi que no es necesario cambiarla

**

**

1. Instalacion de windows y linux como uefi:
    

1. Antes de instalar verificar estos valores en el bios:
    

1. Legacy boot: disabled
    
2. Secure boot: enable
    

3. Instalas windows, incluso aca solo podes crear la porcion asignada a este SO
    
4. El instalador automaticamente va a crear una particion de aprox 500 MB para el UEFI
    
5. Se sigue la instalacion normalmente
    
6. Instalo Linux, grub se va a instalar sin problemas
    
7. Si necesito un tercer OS igual se repite el proceso
    

  
  

2. Evitar que windows 10 borre grub en la hp desktop del docmolina, esto pasa en ciertos bios de marca:
    

1. Desde windows:
    

1. Desactivar fast boot (eso esta en la gestion de energia y sleep, opcion “que hace le boton apagar”)
    
2. Cargar el cmd con derechos de administrador
    
3. Mountvol P: /S
    
4. Dir p:\s -> ya deberia visualizar los grubs de mis distros ubuntu y kali
    
5. Copiar textualmente: bcdedit /set {bootmgr} path \EFI\kali\grubx64.efi
    
6. Apagar la compu
    

  

3. Actualizar el fstab en caso que la swap se haya vuelto a formatear:
    

1. Sudo blkid
    
2. Ubicar la particion swap
    
3. Hacer cambio en el /etc/fstab comentariando el anterior id y agregando el nuevo
    

  

Si usa cryptswap:

**

**![](https://lh7-us.googleusercontent.com/epjV0_ChFIKmZeerz9sfG2QtPOVq6NE0FqLsjafZhI1GelguoR906lXekl1YFHuK7YLb7r13A4oUSpPCqaMFvXafD95ZSfW7k2CwsRT-RgBsrJsDPtNFo2iQWECopIM2yhq--zclu5-spDtKBsOKWXg)**