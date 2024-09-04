# Environment setup
`cd server`

`pip install virtualenv`

`virtualenv djangoenv`

`source djangoenv/bin/activate`

`python3 -m pip install -U -r requirements.txt`

`python3 manage.py makemigrations`

`python3 manage.py migrate`

### Do this everytime you make changes to app.js
`/server/database`
`docker build . -t nodeapp`
`docker-compose up`
