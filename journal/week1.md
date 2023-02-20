# Week 1 — App Containerization

To run the Backend application - Python(flask), I had to install all components in the requirements.txt file with the command:

```
cd backend-flask/
pip3 install -r requirements.txt
python3 -m flask run --host=0.0.0.0 --port=4567
```
On Gitpod, I opened port 4567 to the public and tested the application with the URL `https://4567-chinedujo-awsbootcampcr-tu0h0msihgu.ws-eu87.gitpod.io/api/activities/home` and got error below:

```
Not Found

The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.
```

So I stopped the running application and set the required variables found in `app.py` file:

```
FRONTEND_URL="*"
BACKEND_URL="*"
```
Then restarted the application

```
python3 -m flask run --host=0.0.0.0 --port=4567
```
![](images/)