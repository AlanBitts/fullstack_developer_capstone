# Architecture
- The Django application provides the following microservices for the end user:  
`get_cars/` - To get the list of cars from  
`get_dealers/` - To get the list of dealers  
`get_dealers/:state` - To get dealers by state  
`dealer/:id` - To get dealer by id  
`review/dealer/:id` - To get reviews specific to a dealer  
`add_review/` - To post review about a dealer  

- The Django application uses SQLite database to store the Car Make and the Car Model data.  

- The "Dealerships and Reviews Service" is an Express Mongo service running in a Docker container. It provides the following services::  
`/fetchDealers` - To fetch the dealers  
`/fetchDealer/:id` - To fetch the dealer by id  
`fetchReviews` - To fetch all the reviews  
`fetchReview/dealer/:id` - To fetch reviews for a dealer by id  
`/insertReview` - To insert a review  
"Dealerships Website" interacts with the "Dealership and Reviews Service" through the "Django Proxy Service" contained within the Django Application.  

- The "Sentiment Analyzer Service" is deployed on IBM Cloud Code Engine, it provides the following service:  

`/analyze/:text` - To analyze the sentiment of the text passed. It returns positive, negative or neutral.  
The "Dealerships Website" consumes the "Sentiment Analyzer Service" to analyze the sentiments of the reviews through the Django Proxy contained within the Django application.  

# Environment setup
1. `git clone` repository
2. Run the following commands in /server to setup the Django environment:
```bash
pip install virtualenv
virtualenv djangoenv
source djangoenv/bin/activate
```
3. Install the required packages:
`python3 -m pip install -U -r requirements.txt`
4. Perform models migrations:
```bash
python3 manage.py makemigrations
python3 manage.py migrate <or> python3 manage.py migrate --run-syncdb
```
>The `--run-syncdb` allows creating tables for apps without migrations.  
5. build your client by running the following commands:
```bash
cd server/frontend
npm install
npm run build
```
### Do this everytime you make changes to app.js:
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
