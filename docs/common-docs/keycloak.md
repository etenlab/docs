# Keycloak Setup Instructions


1. run docker compose up using the dev-env repo. this will install postgres, pgAdmin and Keycloak

2. Login to  keycloak administration console through http://localhost:8080 and login using the credentials below
```
Username: admin
Password: admin
````

3. Click on the Master dropdown and the click on Create Realm to Create a new Realm for our app, can name it Showcase, just need to enter the Realm name can keep other fields as it is.

4. Go to the Realm Settings page, in General tab can enter displayname and frontend url (http://localhost:3000)

5. Go to Login tab and enable User registration, Forgot password, Remember me and Email as username

6. Go to Users Page in leftside menu and create a test user

7. Go to Clients Page in left side menu to create new client for our application. enter showcase-auth as Client ID and click on Next button and in Capability config enable Client authentication and Authorization and click on Save 

8. in clients settings page, enter the values as below

```
Root URL: http://localhost:3000
Home URL: http://localhost:3000
Valid redirect URIs: http://localhost:3000/*
Valid post logout redirect URIs: http://localhost:3000/*
Web origins: http://localhost:3000 (this is required for CORS)

```

then save the page.

9. We can create roles in Realm Roles page to setup different levels of permissions, for now you can create a role named 'showcase-user' (this role i used in the showcase project for testing protected page) and assign it to the user created, 

