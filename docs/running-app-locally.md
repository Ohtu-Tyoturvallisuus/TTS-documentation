## Running frontend + backend locally

### Database

Start by making sure you have PostgreSQL installed on your computer. 
If you are using a fresher laptop provided by the University of Helsinki, 
you can install PostgreSQL using the installation script [here.](https://github.com/hy-tsoha/local-pg)
Once you have PostgreSQL running, go to the psql shell: 

```
psql
```

Create a database:

```
CREATE DATABASE tts_testing;
```

After creating the database, you can connect to it using:

```
\c tts_testing
```

After connecting to the database, there will be a notification that says 
*You are now connected to database "tts_testing" as user "<your-username>"*. 
The username that is displayed here will be used later. You can now close the psql shell.


### Backend

1. Clone the [backend repository](https://github.com/Ohtu-Tyoturvallisuus/TTS-backend) 
onto your computer.

2. Make sure that you have installed poetry on your computer, and navigate to 
the root of the backend repository (the directory named *TTS-backend*).
Run the following command:

```
poetry install
```

3. Configure your .env file according to the instructions [here](https://github.com/Ohtu-Tyoturvallisuus/TTS-documentation/blob/main/docs/handling-environment-variables.md)


4. After configuring the .env file, go back to the root directory of the 
backend repository. Run: 

```
poetry run invoke migrate
```

If you have installed PostgreSQL with the University of Helsinki installation 
script, you may run into an error that looks like this:

File "/home/myuser/.cache/pypoetry/virtualenvs/tts-Gnq5Yd8m-py3.10/lib/python3.10/site-packages/django/db/backends/base/base.py", line 200, in check_database_version_supported
    raise NotSupportedError(
django.db.utils.NotSupportedError: PostgreSQL 13 or later is required (found 12.15).

This error can be bypassed by navigating to the base.py file using the path 
displayed in the error message, and commenting out lines 194-203. Once that 
is done, running the migrations should work.

5. After successfully running the migrations, you can start the backend server 
by running:

```
poetry run invoke server
```


### Frontend

1. Clone the [frontend repository](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend) 
onto your computer.

2. Navigate into *TTS-frontend/TTS*. Run:

```
npm install
```

3. Create a .env file:

```
touch .env
```

Insert the following into your .env file:

```
EXPO_PUBLIC_LOCAL_IP='<YOUR_LOCAL_IP>'
EXPO_PUBLIC_LOCAL_SETUP=true
EXPO_PUBLIC_ENVIRONMENT='main'
```

4. Make sure your computer is connected to your mobile device's hotspot. Run:

```
npm start
```

After running the command above, a print containing a QR code is displayed. 
Under the QR code, there is a line that says 
*Metro waiting on exp://<YOUR_LOCAL_IP>:8081*. 
Your local ip is between *exp://* and *:8081*. This needs to be the same as the 
value of LOCAL_IP in the backend .env file, and the value of EXPO_PUBLIC_LOCAL_IP 
in the frontend .env file. 
**If you change values in either of your .env files, you should restart that part of the app.** 
For example, if you replace the LOCAL_IP value in the backend .env file, you 
have to stop running the backend server using Ctrl+C, and start it up again 
with 

```
poetry run invoke server
```

5. After everything is done, you can use the app on your mobile device by 
downloading the Expo Go app either on the Google Play Store or the App Store, 
and using the app to scan the QR code printed after running *npm start* in the 
frontend repository.


## Making Microsoft login work in local testing

After starting the frontend locally, you were displayed the string 'Metro 
waiting on `exp://<YOUR_LOCAL_IP>:8081`'. 
To make logging in with Microsoft possible when using the app on a local setup, 
you need to do the following:

1. Open **Azure portal** (`portal.azure.com`), and log in.

2. Navigate to 
`App registrations -> Owned applications -> HazardHunt-dev -> Manage -> Authentication`

3. You will see a box with mobile and desktop application redirect URIs. 
Click `Add URI`, and add your own redirect uri 
`exp://<YOUR_LOCAL_IP>:8081/--/redirect`

4. Click Save. You should be able to use Microsoft login on your local setup 
shortly after.


## Running only the frontend locally

Sometimes it is necessary to test that new backend features also work after 
deploying the backend to the Azure app service. In these scenarios, you can 
run the frontend locally and connect it to the backend that is running online. 
If you have followed the frontend instructions above, you only need to change 
the value for *EXPO_PUBLIC_LOCAL_SETUP* to *false* in the frontend .env file. 
After this, you can use `npm start`, and the connection to the cloud backend 
will be made.


### Using deployed uat and prod backends with frontend running locally

The previous instructions were given assuming that you have 
`EXPO_PUBLIC_ENVIRONMENT='main'` 
in your .env file in the frontend. If you want to connect to the deployed uat 
version of the backend, you need to set `EXPO_PUBLIC_ENVIRONMENT='uat'`. 
Likewise, if you want to connect to the deployed prod version of the backend, 
you need to set `EXPO_PUBLIC_ENVIRONMENT='production'`. 
**Note that for this to work, you also need to set** 
`EXPO_PUBLIC_LOCAL_SETUP=false`. 
Additionally, if you want Microsoft login to also work in the uat and prod 
environments, you need to add `exp://<YOUR_LOCAL_IP>:8081/--/uat/redirect` to 
the redirect URIs of the **HazardHunt-uat** app registration, and 
`exp://<YOUR_LOCAL_IP>:8081/--/prod/redirect` to the redirect URIs of the 
**HazardHunt** app registration, in the same way as you added your redirect URI 
to the **HazardHunt-dev** app registration.
