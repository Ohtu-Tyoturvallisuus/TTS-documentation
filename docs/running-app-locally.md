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

3. Create a .env file:

```
touch .env
```

Insert the following into your .env file:

```
# .env
SECRET_KEY=<SECRET_KEY>
DB_NAME=tts_testing
DB_USER=<YOUR_LOCAL_DB_USER>
DB_PASSWORD=<YOUR_LOCAL_DB_PASSWORD>
DB_HOST=<YOUR_LOCAL_DB_HOST>
LOCAL_IP=<YOUR_LOCAL_IP>
SPEECH_KEY='<SPEECH_KEY>'
SPEECH_SERVICE_REGION='<SPEECH_SERVICE_REGION>'
AZURE_STORAGE_ACCOUNT_NAME='<AZURE_STORAGE_ACCOUNT_NAME>'
AZURE_STORAGE_ACCOUNT_KEY='<AZURE_STORAGE_ACCOUNT_KEY>'
AZURE_CONTAINER_NAME='<AZURE_CONTAINER_NAME>'
CLIENT_ID='<CLIENT_ID>'
TENANT_ID='<TENANT_ID>'
ERP_CLIENT_ID='<ERP_CLIENT_ID>'
ERP_CLIENT_SECRET='<ERP_CLIENT_SECRET>'
ERP_RESOURCE='<ERP_RESOURCE>'
ERP_SANDBOX_RESOURCE='<ERP_SANDBOX_RESOURCE>'
ERP_TENANT_ID='<ERP_TENANT_ID>'
```

Here are provided all the keys that you need in your .env file, along with 
placeholder values. You need to replace all the 'angle brackets' (< >) 
and the placeholder strings inside the angle brackets according to the instructions 
below.

- In your local .env-file, the value of **SECRET_KEY** can be any random string 
that contains letters and numbers.
    - Optional: The secret key can be created in and copied from your terminal by:
        ```bash
        python3 -c "from django.core.management.utils import get_random_secret_key;
        print(get_random_secret_key())"
        ```

- The value for DB_USER should be the one that was displayed when you connected 
to the database using `\c tts_testing`.

- The value for DB_PASSWORD should be your local database password.
If you don't have one, the value should be ''

- The value for DB_HOST can be found displayed on the command line instance 
that was used to start PostgreSQL. If PostgreSQL has been installed using 
the installation script intended for Linux computers of University of Helsinki, 
the print can look like this: 
*2024-09-16 18:28:46.908 EEST [83876] LOG:  listening on Unix socket "/home/myuser/pgsql/sock/.s.PGSQL.5432"*.
In this case the value you need to use for DB_HOST is 
**/home/myuser/pgsql/sock**


Before finding the value for LOCAL_IP, it should be noted that when running 
the app locally, if you want to use the app on your mobile device, that device 
and your computer should be connected to the same network. The recommended 
way to do this is by starting up a hotspot on your mobile device, and sharing 
the connection with your computer. Once you have connected your computer 
to your mobile hotspot, you can find the value for LOCAL_IP for example by 
using the following command on a linux computer:

```
ifconfig
```

This displays multiple different values. If you don't know which of these is 
the correct one, you should look at instruction **4** of the frontend startup 
instructions below, as the value for LOCAL_IP can be found easier after 
frontend startup.

- The rest of the values needed in the .env file can be found from the key 
vault resource inside the dev resource group on Azure portal.


4. After inserting the values for the keys in the .env file, go back to the root 
directory of the backend repository. Run: 

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

1. Open [Azure portal](portal.azure.com), and log in

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
*Note that for this to work, you also need to set* 
`EXPO_PUBLIC_LOCAL_SETUP=false`. 
Additionally, if you want Microsoft login to also work in the uat and prod 
environments, you need to add `exp://<YOUR_LOCAL_IP>:8081/--/uat/redirect` to 
the redirect URIs of the *HazardHunt-uat* app registration, and 
`exp://<YOUR_LOCAL_IP>:8081/--/prod/redirect` to the redirect URIs of the 
*HazardHunt* app registration, in the same way as you added your redirect URI 
to the *HazardHunt-dev* app registration.
