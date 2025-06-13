
## Sinergia entre las cuentas hftamayo y htues:
- en la fase de escritura del codigo y archivos devops todos son creados desde hftamayo/vanillatstodo.git
- la ejecucion del workflow devops será desde htues debido a que tiene la vinculacion con el student pack
- si hay necesidad de cambios se mantendrá ese flujo para tener sincronizados ambos

## Actualizar htues/vanillatstodo con cambios desde hftamayo/vanillatstodo

* cambios en fase de desarrollo son desde experimental siempre:

![[Pasted image 20250313134746.png]]

![[Pasted image 20250313134902.png]]

==borrando los cambios locales en favor del repo actualizado:

![[Pasted image 20250313135546.png]]

==verificando si tengo los commit más recientes:

![[Pasted image 20250313135712.png]]

![[Pasted image 20250313140012.png]]

verificando en la interface web:

![[Pasted image 20250313140055.png]]

en efecto este es el ultimo cambio desde hftamayo

resumen de los comandos:
```
 2010  git remote add upstream git@github.com:hftamayo/vanillatstodo.git
 2011  git fetch upstream 
 
 git remote -v

2025  git checkout main
 2026  git branch -D experimental
 2027  git checkout -b experimental upstream/experimental
 2028  git checkout -vv
 2029  clear
 2030  git branch -vv
 2031  git status
 2032  git push -u origin experimental

```


para futuras actualizaciones ya me qyueda sincronizado el hftamayo/vanillatstodo:

![[Pasted image 20250313140427.png]]

## descargar actualizaciones de upstream:

```
git checkout experimental
git branch -vv
git fetch upstream
git branch
git merge upstream/experimental
git push origin experimental
```

### metodo alternativo:

```
git fetch upstream experimental
git rebase upstream/experimental
git status
git push origin experimental
```

## por si la riego con algun procedimiento y necesito hacer rebase con remote:

```
git checkout main
git fetch origin
git checkout experimental
git reset --hard origin/experimental
git status
```

## por si necesito sincronizar las ramas de mi repo

el flujo de actualizacion es el siguiente:

- upstream/experimental -> experimental
- experimental -> unstable
- unstable -> staging

por algun error puedo toparme con esto:

![[Pasted image 20250327115500.png]]

==lo mejor es sincronizar local y luego subir los cambios:

```
- git checkout unstable
- git merge experimental
- corrigo los conflictos
- git status
- git add .
- git commit -m "merging from experimental"
- git push origin unstable
```


![[Pasted image 20250327115655.png]]

![[Pasted image 20250327120158.png]]

![[Pasted image 20250327120241.png]]

