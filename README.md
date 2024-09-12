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
