# Keycloak Setup Instructions

1. run docker compose up using the dev-env repo. this will install postgres, pgAdmin and Keycloak

2. Login to keycloak administration console through http://localhost:8080 and login using the credentials below

```
Username: admin
Password: admin
```

3. Click on the Master dropdown and the click on Create Realm to Create a new Realm for our app, can name it Showcase, just need to enter the Realm name can keep other fields as it is.
Note: check if .env variable KEYCLOAK_REALM at database-api corresponds to entered realm name (i.e. 'Showcase', in this particular example)

4. Go to the Realm Settings page, in General tab can enter displayname and frontend url (http://localhost:3000)

5. Go to Login tab and enable User registration, Forgot password, Remember me and Email as username

6. Go to Users Page in leftside menu and create a test user

7. Go to Clients Page in left side menu to create new client for our application. enter showcase-auth as Client ID and click on Next button and in Capability config enable Client authentication and Authorization and click on Save

8. Go to Clients Page in left side menu, choose created client (showcase-auth), go to 'Credentians' tab, check that 'Client Authenticator' is 'Client Id and Secret', reveal and copy Client secret. Use this secret in .env variable KEYCLOAK_CLIENTS at database-api.
For example 
```
KEYCLOAK_CLIENTS=showcase:abcabc,storybook:edfedf,showcase-auth:gJ1ynKzVynB0QgNWldmLianr2UyjTPkZ
```
9. in clients settings page, enter the values as below

```
Root URL: http://localhost:3000
Home URL: http://localhost:3000
Valid redirect URIs: http://localhost:3000/*
Valid post logout redirect URIs: http://localhost:3000/*
Web origins: http://localhost:3000 (this is required for CORS)
Client authentication: Off
```

then save the page.

9. We can create roles in Realm Roles page to setup different levels of permissions, for now you can create a role named 'showcase-user' (this role i used in the showcase project for testing protected page) and assign it to the user created,
