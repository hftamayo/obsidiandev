
==esta experiencia es el relato, hallazgo y resolucion sobre cómo controlar el billing:

1. antes de probar el servicio EKS unicamente tenía un EC2 que por ratos la encendía, así mismo tenía un par de IAM Roles
2. despues que empezé a probar deployar 2 clusters de manera fallida y los dejé encendidos 3 días logré destruirlos, eliminé todos los recursos relacionados en VPC además de mi VM y aca un par de resultados

==saldo a las 16 horas del 04/09

![[Pasted image 20240904152928.png]]


==saldo a las 7:30 del 05/09

![[Pasted image 20240905075725.png]]

==que chingados es?

- punto bien importante es fijarse en que region vas a deployar
- en mi caso seleccioné la region us-west-2 para deployar los clusters
- ahora, revisando la VPC se supone que ya no tengo nada:

![[Pasted image 20240905083956.png]]

pero me fijé que dice US-East, al dar click en "See all regions"

![[Pasted image 20240905084150.png]]


y cabal!!!

![[Pasted image 20240905085600.png]]

![[Pasted image 20240905091025.png]]

![[Pasted image 20240905091229.png]]

![[Pasted image 20240905091324.png]]


![[Pasted image 20240905091435.png]]


![[Pasted image 20240905091622.png]]

este servicio pertenece a EC2, cuando se crea un cluster se asigna una IP Elastica:

![[Pasted image 20240905093655.png]]

![[Pasted image 20240905093836.png]]


==tabla de costos a las 20:32 del dia 05-09-2024

![[Pasted image 20240905203312.png]]

aca ya puedo ver que la diferencia son 0.50 y esta en el rubro de EC2-other

En Billing and Cost Management -> Bills abajo hay un detalle por cada serbicio:

![[Pasted image 20240905214913.png]]


![[Pasted image 20240905214933.png]]

==reporte a las 7:58 del 06-09:

![[Pasted image 20240906075858.png]]




3. 