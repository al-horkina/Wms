Вот пример HTML-страницы, которая включает визуализацию для проверки работоспособности WMS-ссылок. Она использует JavaScript для выполнения запросов и выводит результаты в удобном формате:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WMS URL Checker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1 {
            color: #4CAF50;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f4f4f4;
        }
        .success {
            color: green;
        }
        .error {
            color: red;
        }
    </style>
</head>
<body>
    <h1>WMS URL Checker</h1>
    <p>Введите ссылки WMS для проверки их работоспособности:</p>
    <textarea id="urls" rows="5" style="width: 100%;"></textarea>
    <button onclick="checkUrls()">Проверить ссылки</button>
    
    <table id="results">
        <thead>
            <tr>
                <th>URL</th>
                <th>Статус</th>
                <th>Описание</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>

    <script>
        async function checkUrls() {
            const urls = document.getElementById('urls').value.split('\n');
            const resultsTable = document.getElementById('results').getElementsByTagName('tbody')[0];
            resultsTable.innerHTML = ""; // Очистка таблицы перед новой проверкой
            
            for (const url of urls) {
                if (url.trim() === "") continue; // Пропуск пустых строк

                const row = resultsTable.insertRow();
                const urlCell = row.insertCell(0);
                const statusCell = row.insertCell(1);
                const descriptionCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url.trim());
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        descriptionCell.textContent = text.substring(0, 200) + "...";
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        descriptionCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    descriptionCell.textContent = error.message;
                }
            }
        }
    </script>
</body>
</html>
```

### Особенности этой HTML-страницы:
1. **Ввод URL**:
   - Пользователь может ввести несколько ссылок в текстовое поле, каждая ссылка на отдельной строке.

2. **Кнопка проверки**:
   - Нажатие на кнопку запускает проверку всех введённых URL.

3. **Таблица результатов**:
   - Для каждой ссылки отображается её статус (`Работает` или `Ошибка`) и первые 200 символов ответа.

4. **Стиль**:
   - Приятный дизайн с цветами для отображения успеха (зелёный) и ошибок (красный).

### Как использовать:
1. Скопируйте код в файл, например, `wms_checker.html`.
2. Откройте файл в браузере.
3. Введите ссылки WMS в текстовое поле и нажмите "Проверить ссылки".

Если хотите доработать этот интерфейс, например, добавить экспорт результатов в файл или более сложный анализ, я готов помочь! 🚀
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WMS URL Checker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1 {
            color: #4CAF50;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f4f4f4;
        }
        .success {
            color: green;
        }
        .error {
            color: red;
        }
    </style>
</head>
<body>
    <h1>WMS URL Checker</h1>
    <p>Выберите файл со списком URL для проверки:</p>
    <input type="file" id="fileInput" />
    <button onclick="readFile()">Проверить ссылки</button>

    <table id="results">
        <thead>
            <tr>
                <th>URL</th>
                <th>Статус</th>
                <th>Описание</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>

    <script>
        function readFile() {
            const fileInput = document.getElementById('fileInput');
            const file = fileInput.files[0];

            if (!file) {
                alert("Пожалуйста, выберите файл.");
                return;
            }

            const reader = new FileReader();
            reader.onload = function (event) {
                const urls = event.target.result.split('\n');
                checkUrls(urls);
            };
            reader.readAsText(file);
        }

        async function checkUrls(urls) {
            const resultsTable = document.getElementById('results').getElementsByTagName('tbody')[0];
            resultsTable.innerHTML = ""; // Очистка таблицы перед новой проверкой

            for (const url of urls) {
                if (url.trim() === "") continue; // Пропуск пустых строк

                const row = resultsTable.insertRow();
                const urlCell = row.insertCell(0);
                const statusCell = row.insertCell(1);
                const descriptionCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url.trim());
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        descriptionCell.textContent = text.substring(0, 200) + "...";
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        descriptionCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    descriptionCell.textContent = error.message;
                }
            }
        }
    </script>
</body>
</html>



Для проверки ссылок из файла с помощью JavaScript и HTML можно использовать встроенный функционал браузера для чтения локальных файлов. Вот пример кода:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WMS URL Checker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1 {
            color: #4CAF50;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f4f4f4;
        }
        .success {
            color: green;
        }
        .error {
            color: red;
        }
    </style>
</head>
<body>
    <h1>WMS URL Checker</h1>
    <p>Выберите файл со списком URL для проверки:</p>
    <input type="file" id="fileInput" />
    <button onclick="readFile()">Проверить ссылки</button>

    <table id="results">
        <thead>
            <tr>
                <th>URL</th>
                <th>Статус</th>
                <th>Описание</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>

    <script>
        function readFile() {
            const fileInput = document.getElementById('fileInput');
            const file = fileInput.files[0];

            if (!file) {
                alert("Пожалуйста, выберите файл.");
                return;
            }

            const reader = new FileReader();
            reader.onload = function (event) {
                const urls = event.target.result.split('\n');
                checkUrls(urls);
            };
            reader.readAsText(file);
        }

        async function checkUrls(urls) {
            const resultsTable = document.getElementById('results').getElementsByTagName('tbody')[0];
            resultsTable.innerHTML = ""; // Очистка таблицы перед новой проверкой

            for (const url of urls) {
                if (url.trim() === "") continue; // Пропуск пустых строк

                const row = resultsTable.insertRow();
                const urlCell = row.insertCell(0);
                const statusCell = row.insertCell(1);
                const descriptionCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url.trim());
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        descriptionCell.textContent = text.substring(0, 200) + "...";
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        descriptionCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    descriptionCell.textContent = error.message;
                }
            }
        }
    </script>
</body>
</html>
```

### Особенности:
1. **Чтение файла**:
   - Файл со списком URL загружается через элемент `<input type="file">`.
   - Файл должен быть в текстовом формате, где каждая строка содержит один URL.

2. **Анализ URL**:
   - После загрузки файл читается с помощью `FileReader`.
   - Ссылки проверяются через HTTP-запросы, и результаты выводятся в таблицу.

3. **Визуализация**:
   - Таблица отображает URL, статус проверки (например, "Работает" или "Ошибка") и первые 200 символов ответа.

### Как использовать:
1. Сохраните код в файл, например, `wms_checker_with_file.html`.
2. Откройте файл в браузере.
3. Выберите текстовый файл со списком URL через кнопку "Выберите файл".
4. Нажмите "Проверить ссылки" для начала проверки.

Если потребуется добавить дополнительные функции (например, проверку специфичных параметров WMS), пишите, я помогу доработать! 🚀[43dcd9a7-70db-4a1f-b0ae-981daa162054](https://github.com/buribalazs/smooth-drag-order/tree/7b40d21d076c3e31765f61481f537beaf4c5ec9f/README.md?citationMarker=43dcd9a7-70db-4a1f-b0ae-981daa162054 "1")
