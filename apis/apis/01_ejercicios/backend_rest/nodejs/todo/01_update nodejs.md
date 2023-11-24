
si un paquete requiere una version de node distinta a la instalada el paquete se instala pero no significa que va a funcionar como lo esperado, para un crud de next estaba reciiendo este mensaje:

![[Pasted image 20230531144019.png]]

la version actual de node es la siguiente y aprovechando a borrar el cache de npm:

![[Pasted image 20230531144109.png]]

siguiendo este link: https://www.hostingadvice.com/how-to/update-node-js-latest-version/

para actualiar node a la version 14.20.1:

![[Pasted image 20230531144242.png]]

![[Pasted image 20230531144341.png]]

verificando la version siempre sale la version 14.20.0

Alternativamente estoy intentando instalar la version deseada con nvm:

![[Pasted image 20230531144901.png]]

![[Pasted image 20230531145438.png]]

mucho más facil hacerlo con nvm. Ahora verificando la actualizacion en el proyecto: 

![[Pasted image 20230531145745.png]]

efecivamente ya no hay el warning de los 3 paquetes. Si reinicio el sistema operativo estará como default la anterior version de node, entonces para establecer una especial:

![[Pasted image 20230604154836.png]]

![[Pasted image 20230604155001.png]]


Actualizando a la LTS de Node:

![[Pasted image 20230606105557.png]]

![[Pasted image 20230606110447.png]]