
![[Pasted image 20230615183043.png]]

sitios utiles:
- https://portswigger.net/web-security/authentication/auth-lab-passwords
- https://www.md5online.org/md5-decrypt.html

![[Pasted image 20230615183156.png]]

activo el proxy con el intercept en modo off y refresco, luego me paso a las credenciales recordando el login:

![[Pasted image 20230615183324.png]]

aca no hay token de recordar login.

![[Pasted image 20230615183357.png]]

activo el intercept:

![[Pasted image 20230615183448.png]]

presiono forward y esta es la que me interesa:

![[Pasted image 20230615183553.png]]

notese que en esta version de Burp (2022) ya aparece decodificado asi que me ahorro pasarlo al decoder:

d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw

![[Pasted image 20230615183643.png]]

wiener:51dc30ddc473d43a6011e9ebba6ca770

este Hash lo desencripto, seguidamente le doy forward y apago el intercept:

![[Pasted image 20230615183833.png]]

en el lab ya estoy en esta pantalla:

![[Pasted image 20230615183900.png]]

el paso siguiente es hacer un logout, busco en mi history el endpoint raiz pero que tengo el token de remember, para ello es necesario refrescar la pagina aun despues de hacer logout. Hice varias veces el intento pero siempre me aparecio en blanco el stay logged in, aca un ejemplo:

![[Pasted image 20230615184517.png]]

al final lo que hice fue logearme y modificar el url del lab a mano quitando el endpoint my account y dando f5, al fin si me aparecio el token:

![[Pasted image 20230615184601.png]]

![[Pasted image 20230615184624.png]]

este lo mando al intruder, aca el unico payload que debo tener es el token del stay logged in:

![[Pasted image 20230615184752.png]]

agrego detalles del payload:

![[Pasted image 20230615184836.png]]

![[Pasted image 20230615184930.png]]

![[Pasted image 20230615185005.png]]

![[Pasted image 20230615185039.png]]

![[Pasted image 20230615185112.png]]

en la pestana options del intruder:

![[Pasted image 20230615185212.png]]

y finalmente Start Attack:

![[Pasted image 20230615185251.png]]

efectivamente la configuracion del payload permitio encontrar el hash, ahora vamos a tratar de hacerlo para la cuenta de carlos, aca las modificaciones:

![[Pasted image 20230615185411.png]]

![[Pasted image 20230615185456.png]]

estos passwords son de la lista propiorcionada por el lab. Seguidamente le doy Start Attack:

![[Pasted image 20230615185811.png]]

lo raro fue que me dio que todos los resultados fueron 1 cuando en el video hay una sola coincidencia, revisando el proceso otra vez pero no funciono.

Revise este tutorial > https://www.youtube.com/watch?v=8xVI-99D0xs  y aca los cambios:

el endpoint que use fue my-account (estando logeado con las credenciales de wiener pero sin incluirlas en el url):

![[Pasted image 20230617174311.png]]

notese que el id=wiener se lo quite a mano

Entonces este link lo envie al intruder:

![[Pasted image 20230617174350.png]]

aca los detalles del ataque:

![[Pasted image 20230617174428.png]]

![[Pasted image 20230617174447.png]]

![[Pasted image 20230617174502.png]]

![[Pasted image 20230617174523.png]]

muy importante que en lugar de utilizar my account utilizamos Update email (el boton del formulario cuando caemos en la redireccion despues de logearnos)

paso muy imporante antes de dar Start Attack es cerrar la sesion (hacer logout de la sesion de wiener) y ya  cuando paso eso start attack y se resuelve el lab:

![[Pasted image 20230617174652.png]]

![[Pasted image 20230617174720.png]]









