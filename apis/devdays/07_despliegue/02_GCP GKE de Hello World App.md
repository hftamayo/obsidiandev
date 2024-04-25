
Basado en este tutorial: https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app


el proyecto se llama gobankrestapi

export PROJECT_ID=gobankrestapi

gcloud config set project $PROJECT_ID

gcloud artifacts repositories create gobankrestapirepo \
   --repository-format=docker \
   --location=us-west2 \
   --description="Gobank RestAPI Docker repository"

![[Pasted image 20231211110904.png]]
   
   verificar si fue creado el repo: gcloud artifacts repositories list


![[Pasted image 20231211110956.png]]

![[Pasted image 20231211111126.png]]

![[Pasted image 20231211111317.png]]

...

![[Pasted image 20231211111355.png]]

![[Pasted image 20231211111821.png]]

![[Pasted image 20231211112104.png]]
![[Pasted image 20231211112234.png]]

esta imagen la obtuve sin tener el proxy activo.

![[Pasted image 20231211112702.png]]

...

![[Pasted image 20231211112727.png]]

![[Pasted image 20231211113225.png]]

![[Pasted image 20231211115116.png]]

![[Pasted image 20231211115510.png]]

![[Pasted image 20231211115640.png]]

![[Pasted image 20231211120008.png]]

![[Pasted image 20231211120127.png]]
![[Pasted image 20231211120322.png]]


