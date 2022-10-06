# Deployment

When the docker compose is deployed to a server the keycloak is reachable [here](https://nico.jakobrathberger.eu/)

# Testing

1. Start the quarkus app in dev mode with ``mvn quarkus:dev``
2. Log into a normal user with 
    ```
    export access_token=$(\
    curl --insecure -X POST https://nico.jakobrathberger.eu/realms/quarkus/protocol/openid-connect/token \
    --user backend-service:secret \       
    -H 'content-type: application/x-www-form-urlencoded' \
    -d 'username=alice&password=alice&grant_type=password' | jq --raw-output '.access_token' \
    )
    ```
   or into an admin user with
   ```
   export access_token=$(\
    curl --insecure -X POST https://nico.jakobrathberger.eu/realms/quarkus/protocol/openid-connect/token \
    --user backend-service:secret \       
    -H 'content-type: application/x-www-form-urlencoded' \
    -d 'username=admin&password=admin&grant_type=password' | jq --raw-output '.access_token' \
    )
   ```
   this will store an access token in a environment variable called `access_token`
3. Send a http request to one of the quarkus endpoints with either
   ```
   curl -v -X GET \       
   http://localhost:8080/api/users/me \                                                                    
   -H "Authorization: Bearer "$access_token
   ```
   to get information about the current user

   **or with**

   ```
   curl -v -X GET \
   http://localhost:8080/api/admin \
   -H "Authorization: Bearer "$access_token
   ```
   to check if you are an admin
