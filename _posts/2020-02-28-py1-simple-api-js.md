---
layout: post
title: Simple JS Python application
categories: [js]
tags: [javascript, api]
---


In VS Code

mkdir my_website
cd my_website
python -m venv venv   # Создаем изолированное окружение

Windows: venv\Scripts\activate

(my_website\venv\Scripts\Activate.ps1 cannot be 
loaded because running scripts is disabled on this system  
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser)


macOS/Linux: source venv/bin/activate

pip install flask

my_website/
│
├── venv/                # (Папка с окружением, не трогаем)
├── static/              # Сюда кладем Ванильный JS, CSS, картинки
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── script.js    # Ваш ванильный JavaScript
│
├── templates/           # Сюда кладем HTML (шаблоны Jinja2)
│   └── index.html
│
├── app.py               # Главный файл Python (точка входа)
└── requirements.txt     # Список зависимостей


app.py (Бэкенд на Python):

from flask import Flask, render_template, jsonify

app = Flask(__name__)

@app.route('/')
def home():
    # Отдаем HTML-шаблон
    return render_template('index.html', title="Мой сайт")

@app.route('/api/data')
def get_data():
    # Пример API, который JS может запросить
    return jsonify({"message": "Привет от Python!", "status": "success"})

if __name__ == '__main__':
    # debug=True заставляет сервер перезагружаться при изменении кода
    app.run(debug=True) 


templates/index.html

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>{{ title }}</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
</head>
<body>
    <h1>Привет, это Python + JS!</h1>
    <button id="myButton">Получить данные с бэкенда</button>
    <p id="result"></p>

    <!-- Подключаем ванильный JS -->
    <script src="{{ url_for('static', filename='js/script.js') }}"></script>
</body>
</html>

static/js/script.js

document.addEventListener('DOMContentLoaded', () => {
    const btn = document.getElementById('myButton');
    const resultText = document.getElementById('result');

    btn.addEventListener('click', async () => {
        // Делаем запрос к Python без перезагрузки страницы
        const response = await fetch('/api/data');
        const data = await response.json();
        
        resultText.textContent = data.message;
    });
});

python app.py


http://127.0.0.1:5000
