# SQLite Issue When Installing chromadb

If you get an error that the `sqlite3` version is too old when installing `chromadb`, follow the instructions [here](https://docs.trychroma.com/troubleshooting#sqlite), which says to:

1. Ensure you have the latest version of Python (>= 3.10).
2. Download the latest sqlite-3 zip file of version (>= 3.35) [here](https://www.sqlite.org/download.html).
3. Unpack the zip file and copy the files to the `/usr/bin` folder on your machine.
4. Run `python -m pip install pysqlite3-binary` while you're in your virtual environment.
5. Add the following lines to the django `settings.py` file, which should have a path similar to `./.venv/lib/python3.11/site-packages/debugpy/_vendored/pydevd/tests_python/my_django_proj_21/my_django_proj_21/settings.py`.

```{python}
# these three lines swap the stdlib sqlite3 lib with the pysqlite3 package
__import__('pysqlite3')
import sys
sys.modules['sqlite3'] = sys.modules.pop('pysqlite3')

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

6. Add the following lines to the beginning of the `./.venv/lib/python3.11/site-packages/chromadb/__init__.py` file.

```{python}
__import__('pysqlite3')
import sys
sys.modules['sqlite3'] = sys.modules.pop('pysqlite3')
```

After following their instructions, you can proceed with installing the required packages by running 

```
python -m pip install -r requirements_venv.txt
```
