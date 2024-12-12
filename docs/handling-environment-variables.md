# Handling environment variables

## Backend

**Beginning of instructions referenced in the [guide to running the app locally](https://github.com/Ohtu-Tyoturvallisuus/TTS-documentation/blob/main/docs/running-app-locally.md)**

Create a .env file in the TTS-backend directory:

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
TRANSLATOR_KEY='<TRANSLATOR_KEY>'
TRANSLATOR_SERVICE_REGION='<TRANSLATOR_SERVICE_REGION>'
TRANSLATOR_ENDPOINT='<TRANSLATOR_ENDPOINT>'
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
the correct one, you should come back to this after completing part **4** of 
the frontend startup instructions 
[here](https://github.com/Ohtu-Tyoturvallisuus/TTS-documentation/blob/main/docs/running-app-locally.md), 
as the value for LOCAL_IP can be found easier after frontend startup.

- The rest of the values needed in the .env file can be found from the key 
vault resource inside the dev resource group on Azure portal.


### How to find the values in the key vault on Azure portal

In Azure portal, navigate to 
`Resource groups -> rg-tts-dev -> tts-dev-kv -> Objects -> Secrets`. 
Here you will find all the rest of the needed values. 
You can copy the value for a key by clicking the key, then clicking the current 
version, and clicking *Copy to clipboard* on the **Secret value** field.

**End of instructions referenced in [the guide to running the app locally](https://github.com/Ohtu-Tyoturvallisuus/TTS-documentation/blob/main/docs/running-app-locally.md).**


## How to add a new environment variable

### For local use

1. 
Add the key-value pair to the .env file in the same way as the previous 
evironment variables: 
`<YOUR_ENVIRONMENT_VARIABLE_NAME>=<YOUR_ENVIRONMENT_VARIABLE_VALUE>`

2. 
Navigate to **TTS-backend/tts** and open the **settings.py** file. 
Add a line that retrieves the wanted variable from your .env file:

```
<YOUR_ENVIRONMENT_VARIABLE_NAME> = os.getenv('<YOUR_ENVIRONMENT_VARIABLE_NAME>')
```

As an example, here is how this is done to the **SPEECH_KEY** variable:

```
SPEECH_KEY = os.getenv('SPEECH_KEY')
```

3. 
When you need to use the environment variable in one of your files, import 
the settings module at the top of the file:

```
from django.conf import settings
```

You can then use your environment variable in the file for example by 
initializing it as a variable: 

```
<your_variable_name> = settings.<YOUR_ENVIRONMENT_VARIABLE_NAME>
```

For reference, here is how this is done with the **SPEECH_KEY**:

```
speech_key = settings.SPEECH_KEY
```


### For use in the deployed version of the app

These are instructions for adding environment variables to the tts-dev app 
service. Remember that when deploying to uat and production, you need to also 
add the necessary environment variables to those app services as well.

1. 
Go to Azure portal. Navigate:
`Resource groups -> rg-tts-dev -> tts-dev-kv -> Objects -> Secrets`. 
Add a key by clicking **Generate/Import**, inserting the secret name and 
value, and clicking **Create**.

2. 
Open **TTS-backend/tts/production.py**. Add a line that retrieves the environment 
variable from the key vault: 

```
<YOUR_ENVIRONMENT_VARIABLE_NAME> = key_vault.get_secret('<YOUR_ENVIRONMENT_VARIABLE_NAME>')
```


## Frontend

The directory **TTS-frontend/tts** should contain a .env file with the following 
environment variables: 

```
EXPO_PUBLIC_LOCAL_IP='<YOUR_LOCAL_IP>'
EXPO_PUBLIC_LOCAL_SETUP=true
EXPO_PUBLIC_ENVIRONMENT='main'
EXPO_PUBLIC_EAS_PROJECT_ID='<EAS_PROJECT_ID>'
```

### How to find the value for EAS_PROJECT_ID

To find the value for **EAS_PROJECT_ID**, follow these steps:

1. Go to the [Expo Developer Dashboard](https://expo.dev/).
2. Log in with the Expo account used for the project.
3. Navigate to the **Projects** section in the dashboard.
4. Select the project related to the **HazardHunt** application.
5. In the **Project Overview** section, locate the **ID** field. This is the unique identifier for the project.
6. Copy the **ID** value and replace the placeholder `<EAS_PROJECT_ID>` in your `.env` file with it for the `EXPO_PUBLIC_EAS_PROJECT_ID` key:

```
EXPO_PUBLIC_EAS_PROJECT_ID=<ID>
```

### Adding a new frontend environment variable

Insert the following into your .env file:

```
EXPO_PUBLIC_<YOUR_ENV_VARIABLE_NAME>=<YOUR_ENV_VARIABLE_VALUE>
```

Note that all environment variable names in the frontend .env file should start 
with **EXPO_PUBLIC_**. 
After adding your variable to the .env file, you can access it in the code like 
this:

```
const <YOUR_ENV_VARIABLE_NAME> = process.env.EXPO_PUBLIC_<YOUR_ENV_VARIABLE_NAME>
```

