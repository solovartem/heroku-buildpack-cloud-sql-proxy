# Google Cloud SQL proxy buildpack
Connect to Google Cloud SQL from an external application running outside of Google's infrastructure and that does not have a static or public IP address.  

This buildpack will install the proxy on the localhost, and configure it with the provided credentials. Your application will then connect to the proxy and thru it have query access to the Google Cloud SQL server. Learn more: https://cloud.google.com/sql/docs/postgres/connect-external-app  

Note: Documentation based on proxy for postgress and may vary for mysql database type.  

## Install

### 1. Create a user in the Cloud SQL instance that the proxy are to use for authorization.
---


### 2. Enable the Cloud SQL Admin API
https://cloud.google.com/sql/docs/postgres/connect-external-app#enable-api

---

### 3. Create credential key file
Go to Google Cloud Console -> Menu -> API & Services -> Credentials -> Create "Service account key" -> Follow instructions to generate a json credential file. If required, create a service account: https://cloud.google.com/sql/docs/postgres/connect-external-app#4_if_required_by_your_authentication_method_create_a_service_account

---

### 4. Add buildpack to server and run proxy on startup.
E.g. for heroku: `heroku buildpacks:add https://github.com/DanielZambelli/heroku-buildpack-cloud-sql-proxy` And to run the proxy when starting a Node.js web process add this to the .procfile `web: bin/run_cloud_sql_proxy &>null && npm start`.

---

### 5. Encode key file into a base64 string
In the terminal run: `base64 <key-filename.json>`

---

### 6. Add envs to server
`GCLOUD_CREDENTIALS=<base64 encoded key file as a string>`  
  
`GCLOUD_INSTANCE=<instance-connection-name>=tcp:5432`  
**instance-connection-name** is looked up on the Cloud SQL instance  

`YOUR_DB_CONNECTION_URI=postgres://<username>:<password>@localhost:5432/<database-name>`

---
