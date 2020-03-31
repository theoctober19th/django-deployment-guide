## PythonAnywhere Django App Deployment Checklist

_We will use git to deploy apps to Heroku. So, your project should have initialize a git repository. If you haven't, simply do `git init` at your project root directory. To add all files, do `git add .` and to commit the changes, do `git commit -m "message"`._

1.  Make sure you have git initialized in your project. If not, do `git init`.
2.  Create a `requirements.txt` file with command: `pip freeze > requirements.txt`.
3.  Commit all changes you have made with `git add . & git commit -m "message"`.
4.  Create a git repository in your favorite host (eg, GitHub, BitBucket, GitLab, etc.) and add remote link to that repo using `git remote add origin repo_url`.
5.  Push your code to the Remote Repository: `git push origin master`.
6.  Create a Python Anywhere account from [here](https://pythonanywhere.com) and login to pythonanywhere.com.
7.  In your dashboard page after logging in, start a new console selecting `bash`. Every command from here onwards are to be run on this remote bash in the web browser.
8.  Clone your project repository using `git clone repo_url`. Replace `repo_url` with your project URL. Navigate to that directory using command `cd repo_name`.
9.  Create a virtual environment using command `mkvirtualenv --python=/usr/bin/python3.8 env_name`.
10. Install the modules from requirements.txt file using command `pip install -r requirements.txt`. (In cases you could not install using requirements.txt, you can install libraries manually, like `pip install Django Pillow` etc.)
11. Go to `web` tab in Python Anywhere dashboard and click on _Add a new web app_. Click next.
12. Choose `Manual Configuration` and then select the Python version. Click next.
13. Now after your web app gets initialized, go to `Virtualenv` section in the `Web` tab to add the path to your virtual environment. Click on `Enter path to a virtualenv, if desired.` and then enter the path to the virtual environment you created earlier. Note that it is generally created at the location `/home/username/.virtualenvs/env_name`.
14. Now, in the `Code` section of the `Web` tab, you can see a link with title `WSGI configuration file:`. Click on that link and an editor will open for you.
15. Delete everything there, except the `Django` section. (This is usually around 74/75th line of the code). Uncomment the code on the Django section.
16. Edit the code so that it contains correct project name instead of `mysite` and save it.
17. Go back to the bash console (the one in which you cloned the repo earlier) and then run: `python manage.py makemigrations && python manage.py migrate`. Also, create super user using command `python manage.py createsuperuser`.
18. Go to the `Files` tab, navigate to `settings.py` file (which is located inside a directory with the name of your project) and then add an entry with `'username.pythonanywhere.com'` inside the `ALLOWED_HOSTS` list.
19. Add `STATIC_ROOT = os.path.join(BASE_URL, 'staticfiles')` in `settings.py` file to specify where Django should collect the static files.
20. Collect the static files to a directory by running command: `python manage.py collectstatic`.
21. Go to `Static files` section under `Web` tab and then enter the static URL and directory parameters. Remember that static url should contain the same value as `STATIC_URL` in `settings.py`. The directory should be the full path of the directory specified by `STATIC_ROOT` in `settings.py`. For eg, `/home/username/projectname/staticfiles`.
22. You can enable `Forced HTTPS` option from the `Security` section in `Web` Tab. This makes all requests sent to your web app via `HTTPS` protocol instead of `HTTP` protocol.
23. In the first section of `Web` tab, you could find an option to reload your app. Click it to reload your app.
24. Go to the URL given to your app (usually username.pythonanywhere.com) to launch your app in the browser. Check if everything is working as expected. If not, go back to find the error and correct it.
25. After everything works as expected, change `DEBUG=True` to `DEBUG=False` in the `settings.py`. You can navigate to `settings.py` file from the `Files` tab.

Congratulations, PythonAnywhere Deployment complete!
