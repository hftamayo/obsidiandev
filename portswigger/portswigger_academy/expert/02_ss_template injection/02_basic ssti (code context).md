![[Pasted image 20221019134025.png]]

![[Pasted image 20221019134156.png]]

seleccionando nickname:

![[Pasted image 20221019134258.png]]

esta post request la mando al repeater:

![[Pasted image 20221019134402.png]]


esta aplicacion usa los template Tornado y las expresiones como esta: {{7*7}} puede lograrse su ejeucion


![[Pasted image 20221019134722.png]]

poniendo un comment en un post para verificar como sale mi nickname

![[Pasted image 20221019134814.png]]

![[Pasted image 20221019134845.png]]

en el post display nickname vamos a intentar tener un RCE

![[Pasted image 20221019135215.png]]


![[Pasted image 20221019135309.png]]

verificando si en el comment que puse sale algo distinto

![[Pasted image 20221019135353.png]]

efectivamente hay RCE, probando un payload mas agresivo

user.nickname}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')&csrf=QSnswHdGi8JivDELMuAj0r5MHzw37W7p

![[Pasted image 20221019135841.png]]

recargo la pagina del comentario y listo

![[Pasted image 20221019135941.png]]



