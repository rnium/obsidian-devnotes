1. Install postgresql `sudo apt install postgresql`
2. Enable startup at boot: `sudo systemctl enable postgresql`
3. **Access the PostgreSQL Shell**: `sudo -i -u postgres`
4. **Create a Database**: `createdb <your_database_name>`
5. Create a Database User: `createuser --interactive --pwprompt`
6. Grant access of the database to the user. First enter the postgresql shell by entering `psql`. Then Run the query: `GRANT ALL PRIVILEGES ON DATABASE <your_database_name> TO <your_username>`
7. Set the database in django app's settings file:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'your_database_name',
        'USER': 'your_database_user',
        'PASSWORD': 'your_database_password',
        'HOST': 'localhost',
        'PORT': '5432'
    }
}
```