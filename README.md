Вот обновлённая версия кода, которая добавляет проверку для ссылок из файла: если у ссылки нет параметров `service=WMS&request=GetCapabilities`, они будут автоматически добавлены.

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
        .layers {
            color: #333;
            font-size: 0.9em;
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
                <th>Слои</th>
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

            for (let url of urls) {
                if (url.trim() === "") continue; // Пропуск пустых строк

                // Проверяем и исправляем URL, если нужно
                url = url.trim();
                if (!url.includes("service=WMS&request=GetCapabilities")) {
                    if (url.includes("?")) {
                        url += "&service=WMS&request=GetCapabilities";
                    } else {
                        url += "?service=WMS&request=GetCapabilities";
                    }
                }

                const row = resultsTable.insertRow();
                const urlCell = row.insertCell(0);
                const statusCell = row.insertCell(1);
                const layersCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url);
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        const layers = extractLayers(text);
                        layersCell.innerHTML = layers.length > 0 ? layers.join("<br>") : "Слои не найдены";
                        layersCell.classList.add('layers');
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        layersCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    layersCell.textContent = error.message;
                }
            }
        }

        function extractLayers(xmlText) {
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlText, "application/xml");
            const layerNodes = xmlDoc.getElementsByTagName("Layer");
            const layers = [];

            for (const layer of layerNodes) {
                const titleNode = layer.getElementsByTagName("Title")[0];
                if (titleNode) {
                    layers.push(titleNode.textContent);
                }
            }

            return layers;
        }
    </script>
</body>
</html>
```

### Что изменено:
1. **Проверка и исправление URL**:
   - Если параметры `service=WMS&request=GetCapabilities` отсутствуют, они добавляются автоматически.
   - Логика добавления учитывает наличие символа `?`.

2. **Работа с файлом**:
   - Ссылки считываются из файла (по одной на строку).
   - Если строки пустые, они пропускаются.

3. **Извлечение слоёв**:
   - Слои извлекаются из XML-ответа, как и в предыдущем примере.

4. **Результаты**:
   - Исправленные URL отображаются в таблице.
   - Список слоёв выводится построчно.

### Как использовать:
1. Создайте текстовый файл со списком URL (по одному на строку).
2. Загрузите файл через кнопку "Выберите файл".
3. Нажмите "Проверить ссылки", чтобы увидеть исправленные URL, их статус и список слоёв.

Если хотите добавить что-то ещё, например, более детальную обработку или экспорт данных, дайте знать! 🚀[43dcd9a7-70db-4a1f-b0ae-981daa162054](https://github.com/EleanorEllingson/web-dev/tree/b2f2a382e77a20fd6895677c8b8f402ac4bae352/7-bank-project%2F1-template-route%2Ftranslations%2FREADME.ko.md?citationMarker=43dcd9a7-70db-4a1f-b0ae-981daa162054 "1")



Вот обновлённая версия вашего кода с добавлением проверки: если у ссылки нет параметров `service=WMS&request=GetCapabilities` в конце, они будут автоматически добавлены:

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
        .layers {
            color: #333;
            font-size: 0.9em;
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
                <th>Слои</th>
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
            
            for (let url of urls) {
                if (url.trim() === "") continue; // Пропуск пустых строк

                // Проверяем, заканчивается ли URL на параметры `service=WMS&request=GetCapabilities`
                url = url.trim();
                if (!url.includes("service=WMS&request=GetCapabilities")) {
                    if (url.includes("?")) {
                        url += "&service=WMS&request=GetCapabilities";
                    } else {
                        url += "?service=WMS&request=GetCapabilities";
                    }
                }

                const row = resultsTable.insertRow();
                const urlCell = row.insertCell(0);
                const statusCell = row.insertCell(1);
                const layersCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url);
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        const layers = extractLayers(text);
                        layersCell.innerHTML = layers.length > 0 ? layers.join("<br>") : "Слои не найдены";
                        layersCell.classList.add('layers');
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        layersCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    layersCell.textContent = error.message;
                }
            }
        }

        function extractLayers(xmlText) {
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlText, "application/xml");
            const layerNodes = xmlDoc.getElementsByTagName("Layer");
            const layers = [];

            for (const layer of layerNodes) {
                const titleNode = layer.getElementsByTagName("Title")[0];
                if (titleNode) {
                    layers.push(titleNode.textContent);
                }
            }

            return layers;
        }
    </script>
</body>
</html>
```

### Что изменено:
1. **Проверка и добавление параметров**:
   - Перед выполнением запроса проверяется, есть ли в URL параметры `service=WMS&request=GetCapabilities`.
   - Если их нет, они добавляются. URL корректируется в зависимости от наличия символа `?`.

2. **Гибкость работы**:
   - Поддерживаются URL с уже существующими параметрами.
   - Если URL не содержит параметров, они добавляются корректно.

### Как использовать:
1. Введите ссылки (можно без параметров `service=WMS&request=GetCapabilities`).
2. Нажмите "Проверить ссылки".
3. Таблица выведет статус и список слоёв для каждого исправленного URL.

Если нужно ещё что-то улучшить, пишите! 😊


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



Да, конечно, вы можете вывести список слоёв (Layers) из ответа WMS в описании, чтобы сделать результат более информативным. Для этого можно выполнить разбор XML-ответа и извлечь теги `<Layer>` и `<Title>`. Вот обновлённый пример с добавлением этой функциональности:

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
        .layers {
            color: #333;
            font-size: 0.9em;
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
                <th>Слои</th>
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
                const layersCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url.trim());
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        const layers = extractLayers(text);
                        layersCell.innerHTML = layers.length > 0 ? layers.join("<br>") : "Слои не найдены";
                        layersCell.classList.add('layers');
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        layersCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    layersCell.textContent = error.message;
                }
            }
        }

        function extractLayers(xmlText) {
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlText, "application/xml");
            const layerNodes = xmlDoc.getElementsByTagName("Layer");
            const layers = [];

            for (const layer of layerNodes) {
                const titleNode = layer.getElementsByTagName("Title")[0];
                if (titleNode) {
                    layers.push(titleNode.textContent);
                }
            }

            return layers;
        }
    </script>
</body>
</html>
```

### Что добавлено:
1. **Функция `extractLayers`**:
   - Эта функция анализирует XML-ответ и извлекает все теги `<Title>` внутри `<Layer>`.
   - Результаты формируются в виде массива, который отображается в таблице.

2. **Отображение слоёв**:
   - В таблице появилась дополнительная колонка "Слои", где выводятся названия всех слоёв, найденных в ответе.

3. **Чистота кода**:
   - Слои выводятся построчно с помощью HTML (`<br>`), чтобы их легко было читать.

### Как использовать:
1. Сохраните этот код как файл, например, `wms_checker_layers.html`.
2. Откройте файл в браузере.
3. Загрузите файл со списком WMS-ссылок (по одной ссылке на строку).
4. В колонке "Слои" для каждой ссылки появится список доступных слоёв.

Если потребуется что-то ещё, например, извлечение других метаданных или улучшение интерфейса, обращайтесь! 🚀




Вот обновлённая версия вашего кода, которая включает функцию извлечения списка слоёв из XML-ответа и отображает их в описании:

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
        .layers {
            color: #333;
            font-size: 0.9em;
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
                <th>Слои</th>
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
                const layersCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url.trim());
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        const text = await response.text();
                        const layers = extractLayers(text);
                        layersCell.innerHTML = layers.length > 0 ? layers.join("<br>") : "Слои не найдены";
                        layersCell.classList.add('layers');
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        layersCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    layersCell.textContent = error.message;
                }
            }
        }

        function extractLayers(xmlText) {
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlText, "application/xml");
            const layerNodes = xmlDoc.getElementsByTagName("Layer");
            const layers = [];

            for (const layer of layerNodes) {
                const titleNode = layer.getElementsByTagName("Title")[0];
                if (titleNode) {
                    layers.push(titleNode.textContent);
                }
            }

            return layers;
        }
    </script>
</body>
</html>
```

### Что добавлено:
1. **Функция `extractLayers`**:
   - Разбирает XML-ответ и извлекает список названий слоёв (`<Title>` внутри `<Layer>`).

2. **Отображение слоёв**:
   - В таблице добавлена колонка "Слои", где перечислены все доступные слои для каждой WMS-ссылки.

3. **Чистота вывода**:
   - Названия слоёв отображаются построчно с помощью `<br>`, чтобы сделать список читаемым.

### Как использовать:
1. Сохраните этот код в HTML-файл, например, `wms_checker_with_layers.html`.
2. Откройте файл в браузере.
3. Введите ссылки в текстовое поле, нажмите "Проверить ссылки", и вы увидите список слоёв для каждой ссылки в колонке "Слои".

Если хотите добавить ещё больше возможностей, например, вывод дополнительных данных о слоях, дайте знать! 😊
