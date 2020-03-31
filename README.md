## Notes

- Used Mysql-8.0
- Used Celery with Redis for background tasks
- Configured production and local environment. For real world scenario, one can also add development and staging environment.
- Used Gunicorn with Supervisor to run the server
- Used nginx for API server
- Used two tables:
    1. Email for storing Email data (from, to, subject, body etc.)
    2. EmailEvent for storing all click and open event happening on sent emails.
- Flow of email:
    ```
   1. API call for Send Emails
   2. Loop in all the emails (~100000)
   3. Create email data for bulk create in list(dict)  
   3. Use bulk_create to create all the emails
   4. Post-signal gets triggered on Email Save
   5. Signals trigger Celery task for sending Email
   6. Celery send each mail one by one.
   ```
- For better performance we can increase celery worker count by putting celery-worker server in Auto Scaling.