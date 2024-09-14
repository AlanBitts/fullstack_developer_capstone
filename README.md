# Environment setup
1. `git clone` repository
```bash
# 2. Run the following commands in /server to setup the Django environment:
pip install virtualenv
virtualenv djangoenv
source djangoenv/bin/activate
```
3. Install the required packages:
	- `python3 -m pip install -U -r requirements.txt`
```bash
# 4. Perform models migrations:
python3 manage.py makemigrations
python3 manage.py migrate <or> python3 manage.py migrate --run-syncdb
```
Note: The `--run-syncdb` allows creating tables for apps without migrations.  

```bash
# 5. build your client by running the following commands:
cd server/frontend
npm install
npm run build
```
> ### Do this everytime you make changes to app.js:
>Inside `/server/database` directory run: `docker build . -t nodeapp && docker-compose up`

### Do this everytime you make changes to the database
1. restart the database server after rebuilding the frontend
2. restart the django server

### Deploy sentiment analysis on Code Engine as a microservice
1. In the code engine CLI, change to server/djangoapp/microservices directory
2. Run the following command to docker build the sentiment analyzer app: `docker build . -t us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer`
>Please note the code engine instance is transient and is attached to your lab space username
3. Push the docker image by running the following command: `docker push us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer`
4. Deploy the senti_analyzer application on code engine: `ibmcloud ce application create --name sentianalyzer --image us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer --registry-secret icr-secret --port 5000`
5. Connect to the URL that is generated to access the microservices and check if the deployment is successful.
6. Open djangoapp/.env and replace your code engine deployment url with the deployment URL you obtained above.
>It is essential to include the / at the end of the URL. Please ensure that it is copied
