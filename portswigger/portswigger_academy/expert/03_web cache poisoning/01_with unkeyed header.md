![[Pasted image 20230104124248.png]]

![[Pasted image 20230104125458.png]]

mandamos la traza del root al repeater:

notese en el response la variable X-Cache:

![[Pasted image 20230104125631.png]]

tratamos de analizar el request con estos cambios:
/?cb=1234
X-Forwarded-Host: example.com

![[Pasted image 20230104125942.png]]

la primera vez se tardo un resto este request y al final obtuve un timeout, borrando los agregados que si le funcionaron a Michel Sommer pero a mi no. Por algun motivo ese repeater se trabo, hice otro request y lo mande al repeater, al enviarlo por segunda vez ya X-Cache aparece en hit:

![[Pasted image 20230104130319.png]]

notese que hay un script que hace el tracking:

![[Pasted image 20230104130440.png]]

tratando de envenenar ese tracking.js:

![[Pasted image 20230104130702.png]]

copio la url del lab y la pego en el repeater:

![[Pasted image 20230104131025.png]]

la primera vez sali miss y luego hit:

![[Pasted image 20230104131101.png]]

![[Pasted image 20230104131126.png]]

aca hay un error, tengo que pegar la url del exploit server con el payload que ya ejecute

![[Pasted image 20230104131717.png]]

cuando ya aparce el X-Cache en hit es que me dirigo al navegador y refresco.


refrescando el sitio se despliega la cookie pues el tracking.js esta envenenado:

![[Pasted image 20230104131629.png]]

![[Pasted image 20230104131648.png]]
