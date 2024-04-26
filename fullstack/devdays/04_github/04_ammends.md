
1. hice el ultimo commit pero tenia un typo en el codigo, lo corrigo y grabo el archivo fuente pero no quiero hacer otro commit, entonces uso el comando:

git commit --amend --no-edit

2. recuperando un archivo borrado:
git rm -r CSS/

para deshacer eso:

git reset HEAD CSS/
git checkout -- CSS/

ls

en caso que yo ya lo hubiera commiteado:

git reset --hard HEAD^

tambien puedo hacer esto para revertir un commit:

git revert --no-edit HEAD

3.  sacar un archivo (unstage):
	1. git reset "nombre del archivo"
4. 