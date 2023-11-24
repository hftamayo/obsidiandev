
durante el desarrollo de la API de Java decidí excluir los 2 archivos properties, sin embargo seguían saliendo en el mainstream:

![[Pasted image 20231004081835.png]]

![[Pasted image 20231004081902.png]]

el motivo es que estos archivos ya estan replicados en el github:

![[Pasted image 20231004082017.png]]

para eso tengo que hacer el siguiente procedimiento:

![[Pasted image 20231004082156.png]]

![[Pasted image 20231004082311.png]]

![[Pasted image 20231004082406.png]]

los properties files se borraron del repo pero no del codigo fuente local:

![[Pasted image 20231004082504.png]]