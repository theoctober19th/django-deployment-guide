## Heroku Django App Deployment Checklist

*Every command provided from now onwards are to be run on the project root directory unless specified otherwise.*

We will  use git to deploy apps to Heroku. So, your project should have initialize a git repository. If you haven't, simply do `git init` at your project root directory.
1. Create a `.gitignore` file. The items to ignore in a Django Project can be found from [this site](https://gitignore.io).

2. Install Heroku CLI for your machine from [here](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

3. Create a Heroku account.

4.  After you have installed Heroku CLI in your machine, run command `heroku login`, which will open a browser for you to login to Heroku.

5. Run command `heroku create app_name`, replacing `app_name` with the desired name. Your app will later be available at `app_name.herokuapp.com`.
6. Add Heroku site (`app_name.herokuapp.com` to `ALLOWED_HOSTS` list in `settings.py`. To enable that your app runs on localhost too, you have to add `localhost` and `127.0.0.1` in that list too.
7. Create a file named `Procfile`  in the root directory with this as content:
```web: gunicorn project_name.wsgi``` (Don't forget to replace `project_name` with your own project folder name)

8. Define `STATIC_ROOT` in `settings.py` to point to the directory where static files are to be saved. For instance, 
```STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')```

9. Run `python manage.py collectstatic` to collect static files to the designated directory and see if it works in your machine. If it fails in your machine, it will fail in Heroku too. So, try to find out the problem if it fails.

10. Install a few libraries with command:
 ```pip install dj_database_url whitenoise```
`dj_database_url` is used to generate database configuration for PostgreSQL we will install later, wherease `whitenoise` is a library that helps us serve the static files from the web server itself.

11. Now we need to do some changes to configure `whitenoise`. In `settings.py`, there is a list named `MIDDLEWARE`. On that list, add this entry as a second element (i. e. just after `'django.middleware.security.SecurityMiddleware'` ):
```'whitenoise.middleware.WhiteNoiseMiddleware'```

12. Add `STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'` to enable caching and compression offered by `whitenoise`.
13. Run `pip freeze -l > requirements.txt`

14. Create a file named `runtime.txt` on the project root directory with content: `python-3.8.0`

15. Add PostgreSQL configuration in last line of `settings.py` as:
```
if 'DATABASE_URL' in os.environ: #means heroku
    import dj_database_url
    DATABASES = {'default': dj_database_url.config()} 
```

16. Add `psycopg2`, `gunicorn` on `requirements.txt` file.

17. Commit all changes: `git add . && git commit -m "message"`.

18. Run `git push heroku master`

19. Run `heroku ps:scale web=1`

20. Run `heroku run bash` and there inside the shell, run `python manage.py makemigrations && python manage.py migrate`
21. Create superuser using command `python manage.py createsuperuser` and finally exit bash using `exit` command.

22. Run `heroku open` to open the website and see everything is fine. If something is missing go back, correct it, commit the changes and run `git push heroku master`.

23. Finally after everythign else is set up, set `DEBUG = False` in `settings.py`. This means we are now no longer in DEBUG mode but in production mode.

24. Commit, and push to heroku using `git push heroku master`.

Congratulations, Heroku Deployment complete!

*Note that Django doesn't serve media and static files in the production environment by itself. As a result, we had to use `whitenoise` to serve static files, and will have to use a separate server for hosting media files. One such server Amazon S3. These things are not available for free (You can try AWS Educate account if you have an `edu` email address, but that comes with limitations too). You can also use Cloudinary for free but you will need a credit card, anyway. That is the reason that you will see the media files disappear when the dynos restart in Heroku.*
