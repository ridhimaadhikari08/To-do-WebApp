Step 1: Initialize Project & Virtual Environment
# Install Django
pip install django

# Create project
django-admin startproject todo_project
cd todo_project

# Create app
python manage.py startapp todo

Step 2: Configure settings.py
Open todo_project/settings.py:
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todo',  # Registered app
]

import os

STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]

Step 3: Define the Model
from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=200)

    def __str__(self):
        return self.title

Step 4: Write the Views
from django.shortcuts import render, redirect, get_object_or_404
from .models import Todo

def todo_list(request):
    todos = Todo.objects.all()

    if request.method == "POST":
        # Add Task
        if "add_task" in request.POST:
            title = request.POST.get("title")
            if title:
                Todo.objects.create(title=title)
            return redirect("todo_list")

        # Edit Task
        if "edit_task" in request.POST:
            task_id = request.POST.get("task_id")
            new_title = request.POST.get("new_title")
            todo = get_object_or_404(Todo, id=task_id)
            todo.title = new_title
            todo.save()
            return redirect("todo_list")

        # Delete Task
        if "delete_task" in request.POST:
            task_id = request.POST.get("task_id")
            todo = get_object_or_404(Todo, id=task_id)
            todo.delete()
            return redirect("todo_list")

        # Clear All Tasks
        if "clear_all" in request.POST:
            Todo.objects.all().delete()
            return redirect("todo_list")

    return render(request, "todo/list.html", {
        "todos": todos,
        "task_count": todos.count()
    })

Step 5: Configure URLs

from django.urls import path
from . import views

urlpatterns = [
    path('', views.todo_list, name='todo_list'),
]

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('todo.urls')),
]

Step 6: Create the Template
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <title>Todo App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
</head>
<body>
    <div class="todo-container">
        <h2>Todo App</h2>

        <!-- ADD TASK FORM -->
        <form method="POST">
            {% csrf_token %}
            <div class="input-group">
                <input type="text" name="title" placeholder="Add your new todo" required>
                <button type="submit" name="add_task" class="add-btn">+</button>
            </div>
        </form>

        <!-- TASK LIST -->
        {% for todo in todos %}
        <div class="task">
            <!-- DISPLAY MODE -->
            <div id="text-{{ todo.id }}" class="task-text">
                {{ todo.title }}
            </div>

            <!-- EDIT MODE -->
            <form method="POST" id="edit-{{ todo.id }}" class="edit-form" style="display:none;">
                {% csrf_token %}
                <input type="hidden" name="task_id" value="{{ todo.id }}">
                <input type="text" name="new_title" value="{{ todo.title }}" class="edit-input" required>
                <button type="submit" name="edit_task" class="btn btn-save">✔</button>
            </form>

            <!-- ACTION BUTTONS -->
            <div class="btn-group">
                <button type="button" onclick="enableEdit({{ todo.id }})" class="btn btn-edit">✏</button>
                <form method="POST">
                    {% csrf_token %}
                    <input type="hidden" name="task_id" value="{{ todo.id }}">
                    <button type="submit" name="delete_task" class="btn btn-delete">🗑</button>
                </form>
            </div>
        </div>
        {% empty %}
        <p>No tasks yet</p>
        {% endfor %}

        <!-- FOOTER -->
        <div class="footer">
            <span>You have {{ task_count }} tasks</span>
            <form method="POST">
                {% csrf_token %}
                <button type="submit" name="clear_all" class="clear-btn">Clear All</button>
            </form>
        </div>
    </div>

    <script>
        function enableEdit(id) {
            document.getElementById("text-" + id).style.display = "none";
            document.getElementById("edit-" + id).style.display = "flex";
        }
    </script>
</body>
</html>

Step 7: Create Static CSS Styling
body {
    margin: 0;
    font-family: "Segoe UI", Arial, sans-serif;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: url("../images/bg.png") no-repeat center center/cover;
    position: relative;
}

body::before {
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.25);
    z-index: 0;
}

.todo-container {
    position: relative;
    z-index: 1;
    width: 500px;
    padding: 35px;
    border-radius: 18px;
    background: rgba(255, 255, 255, 0.95);
    box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
}

h2 {
    text-align: center;
    margin-bottom: 30px;
    font-weight: 600;
}

.input-group {
    display: flex;
    margin-bottom: 30px;
}

.input-group input {
    flex: 1;
    padding: 14px;
    border-radius: 12px 0 0 12px;
    border: 1px solid #ddd;
    font-size: 15px;
    outline: none;
}

.input-group input:focus {
    border-color: #6c5ce7;
}

.add-btn {
    background: #6c5ce7;
    color: white;
    border: none;
    padding: 0 24px;
    border-radius: 0 12px 12px 0;
    font-size: 20px;
    cursor: pointer;
    transition: 0.2s ease;
}

.add-btn:hover {
    background: #574bce;
}

.task {
    background: #f4f4f4;
    padding: 18px;
    border-radius: 14px;
    margin-bottom: 18px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 18px;
    transition: background 0.2s ease;
}

.task:hover {
    background: #ededed;
}

.task-text {
    flex: 1;
    font-size: 16px;
}

.edit-input {
    flex: 1;
    padding: 10px 14px;
    border-radius: 10px;
    border: 1px solid #ccc;
    font-size: 14px;
}

.btn-group {
    display: flex;
    gap: 12px;
}

.btn {
    border: none;
    padding: 8px 14px;
    border-radius: 10px;
    cursor: pointer;
    font-size: 14px;
    transition: 0.2s ease;
}

.btn-edit {
    background: #f39c12;
    color: white;
}

.btn-edit:hover {
    background: #d68910;
}

.btn-save {
    background: #2ecc71;
    color: white;
}

.btn-save:hover {
    background: #27ae60;
}

.btn-delete {
    background: #e74c3c;
    color: white;
}

.btn-delete:hover {
    background: #c0392b;
}

.footer {
    margin-top: 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 15px;
}

.clear-btn {
    background: #6c5ce7;
    color: white;
    border: none;
    padding: 10px 18px;
    border-radius: 12px;
    cursor: pointer;
    transition: 0.2s ease;
}

.clear-btn:hover {
    background: #574bce;
}

p {
    text-align: center;
    color: #777;
}

Step 8: Database Migrations & Server Startup
python manage.py makemigrations
python manage.py migrate
python manage.py runserver



