Django To-Do Web Application 📝

A simple, fast, and light-weight task management web application built using Python and Django. This application allows users to create, view, edit inline, and delete tasks with ease, featuring a clean user interface powered by custom CSS.

Features

-Add Tasks: Easily create new to-do items using a simple form.

-Inline Editing: Update task descriptions directly on the main page.

-Delete & Clear All: Remove individual completed tasks or purge the entire list in one click.

-Dynamic Task Counter: Keep track of total active tasks in real-time.

-Clean UI: Simple card layout with custom CSS styles.

-Django ORM & CSRF Protection: Secure database interactions and form submissions.

Tech Stack

-Language: Python 3.x

-Framework: Django

-Frontend: HTML5, CSS3, JavaScript

-Database: SQLite (Default Django Database)

Project Structure

```text
todo_project/
├── manage.py
├── db.sqlite3
├── static/
│   ├── css/
│   │   └── style.css
│   └── images/
│       └── bg.png
├── todo/
│   ├── migrations/
│   ├── templates/
│   │   └── todo/
│   │       └── list.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── urls.py
│   └── views.py
└── todo_project/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py

Installation & Setup
1. Clone the repository:

git clone https://github.com/your-username/django-todo-app.git
cd django-todo-app

2.Create and activate a virtual environment:

Windows:

python -m venv venv
venv\Scripts\activate


macOS / Linux:

python3 -m venv venv
source venv/bin/activate

3.Install Django:

pip install django

4.Apply Database Migrations:

python manage.py makemigrations
python manage.py migrate

5.Start the Development Server:

python manage.py runserver

6.Access the Application:
Open your browser and navigate to http://127.0.0.1:8000/.

