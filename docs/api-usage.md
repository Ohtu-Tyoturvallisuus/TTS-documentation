# Api usage

The API uses standard RESTful conventions, utilizing HTTP methods such as **GET** 
for retrieving data, **POST** for sending data etc. 
Requests and responses are structured using JSON. 

## Making requests from the frontend to the backend

- All requests are made in the [**apiService.js file**](https://github.com/Ohtu-Tyoturvallisuus/TTS-frontend/blob/main/TTS/src/services/apiService.js)

- The request link consists of **API_BASE_URL** followed by the url configured in the backend [**urls.py file**](https://github.com/Ohtu-Tyoturvallisuus/TTS-backend/blob/main/api/urls.py)

- Note that when making **POST, PUT, PATCH,** or **DELETE** requests, it is necessary to include the user's access token in the headers of the request like this: 

```
  {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  }
```

- All requests to the backend go through [**this middleware**](https://github.com/Ohtu-Tyoturvallisuus/TTS-backend/blob/main/api/middleware/access_token_middleware.py), which verifies that all POST, PUT, PATCH and DELETE have the correct authorization header
