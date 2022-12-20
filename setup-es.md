# Test

## EcommerceDB

### Descripción

Ésta es una aplicación Spring Boot a utilizar como base para deploys futuros en Railway

No es una app lista para producción

### Info

Hay un archivo llamado ```application-prod.yml``` utilizado para guardar configuración básica de producción. Éste archivo varará de contenido dependiento de la plataforma donde se haga el deploy. Se puede revisar para tener contexto. Su contenido en éste caso es epecíficamente para Railway


## Parte 1

### SETUP - Cuenta railway

- Iniciar sesión o crear una nueva cuenta en railway
- ![Railway login screen](/img/login.png)
- Es recomendado crear cuenta desde GitHub para tener un más rápido acceso a repos.
- [Railway.app](railway.app)

### SETUP - Nuevo proyecto

- Crear un nuevo proyecto
- Se puede hacer desde el botón en la parte superior derecha o dando click en el centro de la pantalla
- ![New project screen](/img/newProject.png)
- Recomiendo un proyecto vacío
![empty project screen](/img/empty.png)

### SETUP - Servicios

![Add Service](/img/addService.png)
Los servicios trabajan similar a plugins que se agregan al proyecto. Pueden variar desde servicios de bases de datos como ```Mysql, PostgreSQL, MongoDB``` hasta ```Django, Laravel, DiscordJS bot, Kotlin, Vite, Vite + Vue + TS```, etc... Para éste proyecto se utilizará ```Mysql``` y nuestro propio servicio ```(Spring Boot App)``` el cual implementaremos directamente desde ```GitHub```.


- Seleccionar agregar servicio -> Database -> Agregar MySQL
![Add Mysql](/img/mysql.png)

- El servicio puede tardar en cofigurarse
- Una vez creado, dar click en el servicio. Habrá varias tabs de información.
- Se pueden crear tablas y DB directamente desde ```railway``` pero no lo necesitamos en este momento.
![MySQL tabs](/img/mysqltabs.png)

***Importante***

- Dar click en la tab de Variables. Una vez ahí, trendremos toda la información para realizar nuestra conexión de la API hacia la BD.
- Hay que mantener ésta información **privada**
![Mysql Variables](/img/mysqlvariables.png)

### Variables de entorno

Éstas propiedades definen nuestro Servicio/BD. Son necesarias para una conexión apropiada entre nuestra APP y la BD.

- Copia la info de railway dada por el serivcio ```MySQL``` y sustituye en el formato de abajo, ésto no es seguro para tu app ya que estará visible desde Github. 
- Todas las variables de entorno se pegarán en las VE de la App antes del deploy.
- Guarda la información de abajo en un .txt o en algún lugar donde no se perderá
- Mantén ```spring-profiles_active``` Y ```PROD_DB_NAME``` sin modificar.

```properties
spring_profiles_active=prod
PROD_DB_HOST=HOST_HERE
PROD_DB_PORT=POST_HERE
PROD_DB_NAME=railway
PROD_DB_PASSWORD=PASSWORD_HERE
PROD_DB_USERNAME=USER_HERE
```
---
### SETUP - Spring Boot App
--- 

### SETUP - Spring Boot App - application-prod.yml

- Revisa la estructura de tu app en tu IDE. Debería ser similar (aunque no igual) a la imagen siguiente.
![App structure](/img/springAppStructure.png)
- Navega hasta la carpeta ```src/main/resources```
- Una vez ahí, crea un nuevo archivo dentro de ```/resources``` llamado ```application-prod.yml```
- Verifica la extensión

- Copia y pega dentro del archivo la siguente información
```yml
spring:
  datasource:
    url: jdbc:mysql://${PROD_DB_HOST}:${PROD_DB_PORT}/${PROD_DB_NAME}
    username: ${PROD_DB_USERNAME}
    password: ${PROD_DB_PASSWORD}
    name: ecommerceDB

  sql:
    init:
      mode: always # you won't do this in prod, I'm just doing this for demo purposes
```
- Revisa la tabulación de los datos. Archivos ```YAML``` son extraños


### SETUP - Spring Boot App - application.properties

- Actualiza el archivo ```src/main/resources/application.properties```  y verifica la información.
```application.properties```
```properties
# spring.datasource.url= jdbc:mysql://localhost:3306/ecommercedb
# The previous line is an example of a diff type of config for this file.
spring.datasource.url= jdbc:mysql://${{ MYSQLUSER }}:${{ MYSQLPASSWORD }}@${{ MYSQLHOST }}:${{ MYSQLPORT }}/${{ MYSQLDATABASE }}
spring.datasource.username=yourUser
spring.datasource.password=yourPassword

logging.level.org.hibernate.SQL=DEBUG
```
--- 
### SETUP - Spring Boot App - Procfile

Atención al detalle
- En el directorio root del proyecto (el inicio), crea un nuevo archivo **sin** extensión llamado ```Procfile```
- Revisa 3 veces el nombre
- Revisa una vez mas el nombre y su extensión

Pega lo siguiente dentro del archivo
```
web: java -jar -Dserver.port=$PORT build/libs/ecommerceDB-0.0.1-SNAPSHOT.jar
```

- Ésta es información básica para que railway identifique el archivo ```.jar``` y el puerto sobre el cuál debe trabajar.

## Parte 3

### Github App

- Push de toda tu app a un repo de github
- Puede ser pública o privada
![Github repository](/img/githubProject.png)

- Mantén la app ahí

### Agregar nuestra app como servicio

- Uno de los últimos pasos es agregar nuestra app a railway como un servicio
- Da click en el botón de nuevo servicio dentro del proyecto en railway
- Seleccionar Repo de Github y busca el repositorio con la app
![GitHub Repo](/img/githubRepo.png)
- Una vez seleccionada comenzará a hacer un build y después intentará hacer deploy
- Lo más seguro es que el deploy falle debido a la falta de configuración de variables de entorno
![Build v1](/img/buildv1.png)
- Da click en el servicio y selecciona la tab de variabes.
- Una vez ahí, selecciona Raw Editor de la parte derecha
- Pega las variables de entorno guardadas de ```MySQL``` **Ya editadas y con la información dada en el servicio**

```properties
spring_profiles_active=prod
PROD_DB_HOST=HOST_HERE
PROD_DB_PORT=POST_HERE
PROD_DB_NAME=railway
PROD_DB_PASSWORD=PASSWORD_HERE
PROD_DB_USERNAME=USER_HERE
```

Debería verse algo así:
![Environment Variables V2](/img/EnvironmentVar2.png)

- Da click en actualizar variables
- Espera un poco y un nuevo build debería comenzar

### Generar dominio

- En el menú de la app en railway, da click en ```Settings```
- Hay un botón para generar un dominio
- Click
- Ahora se puede acceder a la app desde la web por medio de éste URL
- Todos los endpoints deben ser accedidos a partir de ésta URL inicial

![domain](/img/domain.png)

## Otra información

- Revisar horas disponibles de actividad
- Tener cuidado de no sobrepasar éstas horas
![Data period availability](/img/period.png)

## Referencias

[Spring boot Railway - Dan Vega](https://www.youtube.com/watch?v=5sVxvF47dcU&t=820s)