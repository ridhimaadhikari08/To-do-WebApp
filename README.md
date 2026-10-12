Django To-Do Web Application 📝

A simple, fast, and light-weight task management web application built using Python and Django. This application allows users to create, view, edit inline, and delete tasks with ease, featuring a clean user interface powered by custom CSS.

Features

Add Tasks: Easily create new to-do items using a simple form.

Inline Editing: Update task descriptions directly on the main page.

Delete & Clear All: Remove individual completed tasks or purge the entire list in one click.

Dynamic Task Counter: Keep track of total active tasks in real-time.

Clean UI: Simple card layout with custom CSS styles.

Django ORM & CSRF Protection: Secure database interactions and form submissions.

Tech Stack

Language: Python 3.x

Framework: Django

Frontend: HTML5, CSS3, JavaScript

Database: SQLite (Default Django Database)

Project Structure

├── todo_project/
│   ├── manage.py
│   ├── todo_project/          # Project configuration directory
│   │   ├── __init__.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   └── todo/                  # To-Do Application directory
│       ├── migrations/
│       ├── templates/
│       │   └── todo/
│       │       └── index.html # Main UI view template
│       ├── admin.py
│       ├── apps.py
│       ├── models.py          # Task data structure
│       ├── views.py           # Application logic
│       └── urls.py            # App route handling
└── README.md


Getting Started

Follow these instructions to set up and run the project on your local machine.

Prerequisites

Ensure you have Python installed on your computer. You can verify this by running:

python --version


Installation & Setup

Clone the repository:

git clone https://github.com/your-username/django-todo-app.git
cd django-todo-app


Create and activate a virtual environment:

Windows:

python -m venv venv
venv\Scripts\activate


macOS / Linux:

python3 -m venv venv
source venv/bin/activate


Install Django:

pip install django


Apply Database Migrations:

python manage.py makemigrations
python manage.py migrate


Start the Development Server:

python manage.py runserver


Access the Application:
Open your browser and navigate to http://127.0.0.1:8000/
