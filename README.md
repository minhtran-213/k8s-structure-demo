# k8s-deployment-demo

## Overview
This is a demo project for deploying a Kubernetes cluster with Spring boot application and Spring Cloud Gateway

## Prerequisite
Docker desktop with wsl and kubernetes enabled
**DON'T** use minikube cluster for this demo

## Steps
### 1. Deploy all Kubernetes config file
```
kubectl apply -f .
```
### 2. Go to ./etc/hosts and add these lines
```
127.0.0.1 gateway
127.0.0.1 keycloak.keycloak
```
This will cheat your machine when call to http://gateway:30333 or http://keycloak.keycloak it will point to 127.0.0.1
### 3. Create realm in Keycloak
Go to http://keycloak.keycloak/ to create
![image](https://user-images.githubusercontent.com/71303303/233992699-f177af1d-2a19-4065-abde-6583a3cd669e.png)
### 4. Create a client in Keycloak
Your final result in config client for Keycloak will be like this
![image](https://user-images.githubusercontent.com/71303303/233992932-f6b6efeb-c023-4311-9c35-0d3106ea0d09.png)
![image](https://user-images.githubusercontent.com/71303303/233992975-ab3db1a3-7f0d-4b14-b97c-c5049e2c2b67.png)
### 5. Now you can test your result
#### a. Create roles admin and user in realm roles
#### b. Create a user in Keycloak
#### c. Send a request to Keycloak Authorization Server to get access token
```
curl --location 'http://keycloak.keycloak/realms/keycloak-poc/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=public' \
--data-urlencode 'username=<username>' \
--data-urlencode 'password=<password>' \
--data-urlencode 'grant_type=password'
```
#### d. Send a request to gateway to test
```
curl --location 'http://gateway:30333/book-service/api/v1/books' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ1YTAxZUxyei1CTG5nX2Q1aGczdEtmcHNBSGJETUJjTHp5ZHZ6V3F2elYwIn0.eyJleHAiOjE2ODE5OTU5OTIsImlhdCI6MTY4MTk5NTY5MiwianRpIjoiNzFkZWMzYTEtMDAyMy00ZjUxLWFmN2QtYWJhNWVlYjQ4YjVjIiwiaXNzIjoiaHR0cDovL2tleWNsb2FrLmtleWNsb2FrL3JlYWxtcy9rZXljbG9hay1wb2MiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMzBhODI5NmUtYzM4Yy00N2JmLWI5MDAtYmE4MWFiODdkYjAzIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoicHVibGljIiwic2Vzc2lvbl9zdGF0ZSI6ImUzNWI0MGE2LTgzYmItNDUzOS04YjAxLWU0MmVkOGU4ZmVmOSIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiaHR0cDovL2RlbW8tcHJvamVjdDozMDg4OCJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1rZXljbG9hay1wb2MiLCJvZmZsaW5lX2FjY2VzcyIsImFkbWluIiwidW1hX2F1dGhvcml6YXRpb24iLCJ1c2VyIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIiwic2lkIjoiZTM1YjQwYTYtODNiYi00NTM5LThiMDEtZTQyZWQ4ZThmZWY5IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJuYW1lIjoiTWluaCBUcmFuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoibWluaHRyYW4iLCJnaXZlbl9uYW1lIjoiTWluaCIsImZhbWlseV9uYW1lIjoiVHJhbiIsImVtYWlsIjoibWluaHRyYW4yMTMwMUBnbWFpbC5jb20ifQ.hb8EyxQHnLqjo1o9OjK1xNtXkVnhnvUW3owlhPLVa17mRWCrbzQXZHUeXak0RBgp3XbCrJ3PBAVXCCAXFGh-fqJKnIVWPzcnJwdMsaIhVvFoVcmXEVZsfZbI8I54qY0bECCru0tn1VfzLGKqOE-2r2TSNgE1Ta2nzY1J_dh9wNaxV-FQWGgbAyxuvjtBnpXA2Xp-_VbUuB6g6VBvgcLVtgsMiIShECjJwUnUA_JvdhU9Cp2nVBSyEL_acxJGzcgu0rJOHg9ALpvKTSfIrKSg1JBAAO70AKkUpWRGT3Eo79BAto2gPvVlreMfC9Uj8e7LH8yds8jcv0A2wS8yfidEGQ' \
--data '{
    "title": "Demo",
    "author": "Demo author",
    "publishDate": "2022-03-22"
}'
```
**Note:** The token is only for demo, you should put your access token which has been created before in step 5.c
