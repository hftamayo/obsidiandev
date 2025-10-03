
## Usuarios en bases de datos:

- Administrador:  administrador@tamayo.com at milucito1
- Supervisor: supervisor@tamayo.com at milucito2
- Usuario 1: bob@tamayo.com at milucito3
- Usuario 2: mary@tamayo.com at milucito4
## Usuarios para conexion del backend con la datalayer:

- administrador db: db11k+compuesto
- usuario conexion: sebastic
- clave conexion: milucito

## Nombre de las bases de datos: "nombreproyecto"_"branch"

- nodetodo_dev
- nodetodo_test
- nodetodo_prod

## Nombre de los db containers para desarrollo:
- hftamayo/pgdev
- hftamayo/mysqldev

## Roles por default:

### En formato enum: se muestran endpoints mediante un componente que maneja un listado
- administrator
- supervisor
- finaluser

## Puertos:
containers:
- 8001-8010: golang
- 8011-8020: jsbtodo
- 8021-8030: node
- 8031-8040: django
- 8041-8050: react
- 3300 - 3310: mysql
- 5400 - 5410: postgres

___
## AWS

### Roles AWS:
group: project_managers
usuarios:
- project_manager, ==tag==: role: pm, ==acceso a la consola==: aws+compuesto+$, ==alias del account id==: superadminues