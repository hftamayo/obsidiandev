![[Pasted image 20230106123208.png]]

![[Pasted image 20230619212425.png]]

![[Pasted image 20230619212450.png]]

![[Pasted image 20230619212548.png]]

![[Pasted image 20230619212604.png]]

me voy al exploit server:

![[Pasted image 20230619212719.png]]

![[Pasted image 20230619212857.png]]

luego me voy a una entrada de blog y pongo un comentario:

![[Pasted image 20230619213128.png]]

![[Pasted image 20230619213153.png]]

luego me paso al repeater de Burp y agrego en el header:

![[Pasted image 20230619213414.png]]

envio el response:

![[Pasted image 20230619213439.png]]

ahora me regreso al lab y le verifico los comments:

![[Pasted image 20230619213542.png]]

ahora me vuelvo al exploit server y guardo el payload, luego me voy al access log:

![[Pasted image 20230619213759.png]]

user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36

entonces esta linea la cambio en el header y envio el request:

![[Pasted image 20230619214011.png]]

![[Pasted image 20230619214027.png]]

![[Pasted image 20230619214052.png]]

