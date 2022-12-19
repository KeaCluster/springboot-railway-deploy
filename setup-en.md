# Test

## EcommerceDB

### Desc

This is a test Spring Boot app meant to be a base for future deploys on Railway

Not ready for production
> ?Could be ready for production

### Info

There's an ```application-prod.yml``` file used to save basic production config data. This will have different info depending on the platform you're making the deploy. Check it for more context. What's in there however is specifically for railway.

## Part 1

### SETUP - railway account

- Login or setup new railway account.
![Railway login screen](/img/login.png)
- Recommended to be setup from github for easier access to repos.
- [Railway.app](railway.app)

### SETUP - New project

- Create a new project
- Either from the top right icon or center action area
![New project screen](/img/newProject.png)
- I recommend an empty project
![empty project screen](/img/empty.png)

### SETUP - Services


![Add Service](/img/addService.png)
Services work as plugins you can add to your project. They vary from Database services such as ```Mysql, PostgreSQL, MongoDB``` all the way to ```Django, Laravel, DiscordJS bot, Kotlin, Vite, Vite + Vue + TS```, etc... For this project we'll be using ```Mysql``` and our own service ```(Spring Boot App)``` fetched directly from ```GitHub```.

- Select add service -> Database -> Add MySQL
![Add Mysql](/img/mysql.png)

- Your service might take some time to setup 
- Once created, click on your service. It will have several info tabs.
- You can create tables directly from ```railway``` but we don't want that right now.
![MySQL tabs](/img/mysqltabs.png)

***Check this out***

- Travel to your Variables tab. Once there, you'll realize that's all the info to make your connection from your API to your DB.
- Keep it **private**
![Mysql Variables](/img/mysqlvariables.png)

## Environment variables

These properties define your DB/Service environment variables. They're necessary info for a proper connection between your APP and the DB.

- Copy your info from railway given by the ```MySQL``` service and paste them in the format down bellow here, although it's not safe for your app because it will be visible from github.
- All these environment variables are to be pasted in your service Variables before deploying
- Save the bellow information in a .txt file or somewhere it won't get lost
- Keep ```spring-profiles_active``` AND ```PROD_DB_NAME``` unmodified.

```properties
spring_profiles_active=prod
PROD_DB_HOST=HOST_HERE
PROD_DB_PORT=POST_HERE
PROD_DB_NAME=railway
PROD_DB_PASSWORD=PASSWORD_HERE
PROD_DB_USERNAME=USER_HERE
```

### SETUP - Spring Boot app

### SETUP - Spring Boot App - application-prod.yml

- Check your app structure. It should look similar (although not quite the same) as the image bellow
![App structure](/img/springAppStructure.png)
- Navigate untill you reach ```src/main/resources```
- Once there, create a new file inside ```/resources``` called ```application-prod.yml```
- Verify the extension

- Copy and paste in there the following data
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
- Check tabulations. ```YAML``` files are weird

### SETUP - Spring Boot App - application.properties

- Update ```src/main/resources/application.properties``` 
and verify info.

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

This is a werid one.
- On the root directory of your app. Create a new file **without** extension called ```Procfile```
- Triple check the name
- Check the name and extension again

Paste the following line inside
```
web: java -jar -Dserver.port=$PORT build/libs/ecommerceDB-0.0.1-SNAPSHOT.jar
```

- This is basic info for railway to identify the ```.jar``` file and the Port it should be working on.

## Github APP 

- Push your entire project on to a github repo
- It can be public or private
![Github repository](/img/githubProject.png)

- Leave it there

## Adding our App as a service

- One of the few last steps is to add our App on railway as a service
- Click on new service inside your railway project
- Select GitHub repo and look for your Spring Boot App
![GitHub Repo](/img/githubRepo.png)
- Once clicked it will begin a build and then a deploy
- This first deploy will most likely fail due to environment variables not being Setup
![Build v1](/img/buildv1.png)
- Click on your service and select the variables tab.
- Once there, click on Raw Editor on the right handside
- And paste our saved Environment Variables from MySLQ **Already edited and filled with your info**

```properties
spring_profiles_active=prod
PROD_DB_HOST=HOST_HERE
PROD_DB_PORT=POST_HERE
PROD_DB_NAME=railway
PROD_DB_PASSWORD=PASSWORD_HERE
PROD_DB_USERNAME=postgres
```

It should look something like this:
![Environment Variables V2](/img/EnvironmentVar2.png)

- Click on update Variables
- Wait a bit and a new build should begin.


### Generate Domain

- On your app menu inside railway, click on ```Settings```
- You'll see a button to generate a Domain.
- Click on it
- Now you can access you app from the web by accessing this url

![domain](/img/domain.png)

## Other info

- Check your available uptime hours on the top right corner
- Be careful not to overdo this period
![Data period availability](/img/period.png)

## References

[Spring boot Railway - Dan Vega](https://www.youtube.com/watch?v=5sVxvF47dcU&t=820s)