Вот полностью готовый HTML файл, который обрабатывает ситуацию, когда WMS-сервис возвращает файл без расширения, открывая содержимое прямо в браузере:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WMS Viewer</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1 {
            color: #4CAF50;
        }
        button {
            margin-bottom: 20px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
        pre {
            background-color: #f4f4f4;
            padding: 10px;
            border: 1px solid #ddd;
            overflow: auto;
            white-space: pre-wrap;
            max-height: 600px;
        }
        .error {
            color: red;
        }
    </style>
</head>
<body>
    <h1>WMS Viewer</h1>
    <p>Просмотр содержимого файла, возвращённого WMS-сервисом</p>
    <button onclick="loadWMS()">Загрузить содержимое WMS</button>
    <pre id="responseContainer">Здесь появится содержимое файла...</pre>

    <script>
        async function loadWMS() {
            const wmsUrl = "https://ваш_сервис/wms?service=WMS&request=GetCapabilities";

            try {
                const response = await fetch(wmsUrl, { method: "GET" });
                if (response.ok) {
                    // Считываем текст независимо от типа файла
                    const text = await response.text();
                    document.getElementById("responseContainer").textContent = text;
                } else {
                    document.getElementById("responseContainer").textContent = 
                        `Ошибка: сервер вернул статус ${response.status}`;
                }
            } catch (error) {
                document.getElementById("responseContainer").textContent = 
                    `Произошла ошибка: ${error.message}`;
            }
        }
    </script>
</body>
</html>
```

### Особенности:
1. **Считывание текста независимо от расширения файла**:
   - Файл обрабатывается как текст, даже если сервер не указывает MIME-тип или возвращает файл без расширения.

2. **Вывод содержимого**:
   - Содержимое файла отображается в блоке `<pre>` для удобного чтения с сохранением форматирования.

3. **Обработка ошибок**:
   - Если сервер возвращает ошибку (например, неверный статус HTTP), она выводится на экран.

4. **Интерфейс**:
   - Простая кнопка для загрузки содержимого файла.
   - Вывод происходит в текстовом блоке с возможностью прокрутки.

### Как использовать:
1. Сохраните код в HTML-файл, например, `wms_viewer.html`.
2. Замените `https://ваш_сервис/wms?service=WMS&request=GetCapabilities` на ваш реальный URL WMS-сервиса.
3. Откройте HTML-файл в браузере.
4. Нажмите кнопку "Загрузить содержимое WMS", чтобы увидеть ответ сервера прямо в браузере.

Если потребуется добавить дополнительные возможности, например, улучшенное форматирование или проверку других типов содержимого, дайте знать! 😊
[43dcd9a7-70db-4a1f-b0ae-981daa162054](https://github.com/buribalazs/smooth-drag-order/tree/7b40d21d076c3e31765f61481f537beaf4c5ec9f/README.md?citationMarker=43dcd9a7-70db-4a1f-b0ae-981daa162054 "1")





Привет! Да, можно настроить так, чтобы файл, возвращаемый WMS-сервисом, открывался прямо в браузере вместо загрузки на компьютер. Всё зависит от типа файла и настроек сервера, а также браузера. Вот несколько способов:

### 1. **Использование MIME-типа на сервере**
Если вы контролируете сервер WMS, убедитесь, что заголовок ответа HTTP включает соответствующий MIME-тип, чтобы файл интерпретировался браузером и не загружался:
- Для XML файла используйте `Content-Type: application/xml` или `text/xml`.
- Для других форматов (например, JSON) укажите соответствующий MIME-тип, например, `application/json`.

Пример заголовка:
```
Content-Type: application/xml
Content-Disposition: inline
```
`Content-Disposition: inline` заставляет браузер открывать файл, а не сохранять его.

### 2. **Ручная проверка через браузер**
Попробуйте вставить WMS-ссылку прямо в адресную строку браузера. Например:
```
https://ваш_сервис/wms?service=WMS&request=GetCapabilities
```
Если сервер настроен правильно, браузер откроет содержимое файла в виде текста (для XML и JSON это стандартное поведение).

### 3. **Форсирование открытия через iframe**
Вы можете обернуть ссылку в HTML-код, чтобы файл открывался внутри страницы через тег `<iframe>`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Открыть WMS файл</title>
</head>
<body>
    <h1>Просмотр WMS-ответа</h1>
    <iframe 
        src="https://ваш_сервис/wms?service=WMS&request=GetCapabilities" 
        style="width: 100%; height: 600px; border: none;">
    </iframe>
</body>
</html>
```

### 4. **Скрипт для динамического отображения содержимого**
Если WMS возвращает текстовый файл (например, XML), вы можете обработать и отобразить его с помощью JavaScript:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>WMS Viewer</title>
    <style>
        pre {
            background-color: #f4f4f4;
            padding: 10px;
            border: 1px solid #ddd;
            overflow: auto;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <h1>WMS Viewer</h1>
    <button onclick="loadWMS()">Загрузить WMS</button>
    <pre id="responseContainer">Здесь появится ответ от WMS...</pre>

    <script>
        async function loadWMS() {
            const wmsUrl = "https://ваш_сервис/wms?service=WMS&request=GetCapabilities";

            try {
                const response = await fetch(wmsUrl);
                if (response.ok) {
                    const text = await response.text();
                    document.getElementById("responseContainer").textContent = text;
                } else {
                    document.getElementById("responseContainer").textContent = `Ошибка: ${response.status}`;
                }
            } catch (error) {
                document.getElementById("responseContainer").textContent = `Произошла ошибка: ${error.message}`;
            }
        }
    </script>
</body>
</html>
```

### Как работает:
1. При нажатии кнопки происходит HTTP-запрос по WMS-ссылке.
2. Ответ сервера (текстовый файл) выводится на страницу в удобном формате.

### Примечания:
- Если сервер отдаёт файл с заголовком `Content-Disposition: attachment`, то открыть файл в браузере будет невозможно без изменения конфигурации сервера.
- В некоторых случаях можно настроить обработку через прокси-сервер, который меняет заголовки ответа.

Если вам нужно ещё больше возможностей, например, автоматическая подгрузка и форматирование содержимого, дайте знать! 😊

¹1111111111111111

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>WMS Viewer</title>
    <style>
        pre {
            background-color: #f4f4f4;
            padding: 10px;
            border: 1px solid #ddd;
            overflow: auto;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <h1>WMS Viewer</h1>
    <button onclick="loadWMS()">Загрузить WMS</button>
    <pre id="responseContainer">Здесь появится ответ от WMS...</pre>

    <script>
        async function loadWMS() {
            const wmsUrl = "https://ваш_сервис/wms?service=WMS&request=GetCapabilities";

            try {
                const response = await fetch(wmsUrl);
                if (response.ok) {
                    const text = await response.text();
                    document.getElementById("responseContainer").textContent = text;
                } else {
                    document.getElementById("responseContainer").textContent = `Ошибка: ${response.status}`;
                }
            } catch (error) {
                document.getElementById("responseContainer").textContent = `Произошла ошибка: ${error.message}`;
            }
        }
    </script>
</body>
</html>



Вот полностью готовый HTML файл, который обрабатывает ситуацию, когда WMS-сервис возвращает файл на запрос HTTP. Этот файл позволяет загружать содержимое, проверять ссылки и загружать файлы автоматически:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WMS File Checker</title>
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
    <h1>WMS File Checker</h1>
    <p>Введите ссылки WMS вручную или выберите файл:</p>
    <div>
        <input type="radio" id="manualInput" name="inputType" value="manual" checked>
        <label for="manualInput">Ввести вручную</label>
        <br>
        <input type="radio" id="fileInput" name="inputType" value="file">
        <label for="fileInput">Загрузить из файла</label>
    </div>
    
    <div id="manualInputDiv">
        <p>Введите ссылки WMS:</p>
        <textarea id="urls" rows="5" style="width: 100%;"></textarea>
    </div>
    
    <div id="fileInputDiv" style="display: none;">
        <p>Выберите файл со списком ссылок:</p>
        <input type="file" id="fileInputField" />
    </div>
    
    <button onclick="startCheck()">Проверить ссылки</button>
    
    <table id="results">
        <thead>
            <tr>
                <th>URL</th>
                <th>Статус</th>
                <th>Содержимое файла</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>

    <script>
        document.getElementsByName('inputType').forEach(input => {
            input.addEventListener('change', () => {
                const manualDiv = document.getElementById('manualInputDiv');
                const fileDiv = document.getElementById('fileInputDiv');
                if (input.value === "manual") {
                    manualDiv.style.display = "block";
                    fileDiv.style.display = "none";
                } else {
                    manualDiv.style.display = "none";
                    fileDiv.style.display = "block";
                }
            });
        });

        function startCheck() {
            const selectedInputType = document.querySelector('input[name="inputType"]:checked').value;
            if (selectedInputType === "manual") {
                const urls = document.getElementById('urls').value.split('\n');
                checkUrls(urls);
            } else {
                const fileInput = document.getElementById('fileInputField').files[0];
                if (!fileInput) {
                    alert("Пожалуйста, выберите файл.");
                    return;
                }

                const reader = new FileReader();
                reader.onload = function (event) {
                    const urls = event.target.result.split('\n');
                    checkUrls(urls);
                };
                reader.readAsText(fileInput);
            }
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
                const fileContentCell = row.insertCell(2);

                urlCell.textContent = url;

                try {
                    const response = await fetch(url);
                    if (response.ok) {
                        statusCell.textContent = "Работает";
                        statusCell.classList.add('success');
                        
                        // Проверяем, возвращает ли сервер файл
                        const blob = await response.blob();
                        const fileReader = new FileReader();
                        fileReader.onload = function () {
                            const content = fileReader.result;
                            fileContentCell.innerHTML = content.length > 0 
                                ? content.substring(0, 500) + "..." // Ограничиваем вывод содержимого файла
                                : "Содержимое файла отсутствует.";
                        };
                        fileReader.readAsText(blob);
                    } else {
                        statusCell.textContent = "Ошибка";
                        statusCell.classList.add('error');
                        fileContentCell.textContent = `Статус: ${response.status}`;
                    }
                } catch (error) {
                    statusCell.textContent = "Ошибка";
                    statusCell.classList.add('error');
                    fileContentCell.textContent = error.message;
                }
            }
        }
    </script>
</body>
</html>
```

### Как работает:
1. **Выбор способа ввода**:
   - Вы можете ввести ссылки вручную или загрузить файл со списком ссылок.
   - Если выбран режим "Ввести вручную", текстовое поле отображается. Если выбран режим "Загрузить из файла", появляется поле для загрузки файла.

2. **Проверка URL**:
   - Ссылки проверяются на наличие параметров `service=WMS&request=GetCapabilities`. Если параметры отсутствуют, они автоматически добавляются.

3. **Обработка файлов**:
   - Если сервер возвращает файл, его содержимое считывается (в случае текстового файла, например, XML).
   - Выводится максимум 500 символов содержимого файла, чтобы избежать перегрузки интерфейса.

4. **Результаты**:
   - Таблица отображает URL, статус запроса и первые строки содержимого возвращённого файла.

### Как использовать:
1. Сохраните код в HTML-файл, например, `wms_file_checker.html`.
2. Откройте файл в браузере.
3. Введите ссылки вручную или загрузите файл со списком ссылок.
4. Нажмите "Проверить ссылки", чтобы увидеть результаты проверки.

Если потребуется доработка, например, добавление автоматической загрузки файла на компьютер, дайте знать! 😊



Вот доработанный код, который позволяет выбрать один из двух способов ввода ссылок: вручную в текстовое поле или загрузкой из файла:

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
    <p>Выберите один из способов ввода ссылок:</p>
    <div>
        <input type="radio" id="manualInput" name="inputType" value="manual" checked>
        <label for="manualInput">Ввести вручную</label>
        <br>
        <input type="radio" id="fileInput" name="inputType" value="file">
        <label for="fileInput">Загрузить из файла</label>
    </div>
    
    <div id="manualInputDiv">
        <p>Введите ссылки WMS:</p>
        <textarea id="urls" rows="5" style="width: 100%;"></textarea>
    </div>
    
    <div id="fileInputDiv" style="display: none;">
        <p>Выберите файл со списком ссылок:</p>
        <input type="file" id="fileInputField" />
    </div>
    
    <button onclick="startCheck()">Проверить ссылки</button>
    
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
        document.getElementsByName('inputType').forEach(input => {
            input.addEventListener('change', () => {
                const manualDiv = document.getElementById('manualInputDiv');
                const fileDiv = document.getElementById('fileInputDiv');
                if (input.value === "manual") {
                    manualDiv.style.display = "block";
                    fileDiv.style.display = "none";
                } else {
                    manualDiv.style.display = "none";
                    fileDiv.style.display = "block";
                }
            });
        });

        function startCheck() {
            const selectedInputType = document.querySelector('input[name="inputType"]:checked').value;
            if (selectedInputType === "manual") {
                const urls = document.getElementById('urls').value.split('\n');
                checkUrls(urls);
            } else {
                const fileInput = document.getElementById('fileInputField').files[0];
                if (!fileInput) {
                    alert("Пожалуйста, выберите файл.");
                    return;
                }

                const reader = new FileReader();
                reader.onload = function (event) {
                    const urls = event.target.result.split('\n');
                    checkUrls(urls);
                };
                reader.readAsText(fileInput);
            }
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

### Что добавлено:
1. **Выбор способа ввода**:
   - Радиокнопки позволяют выбрать между ручным вводом ссылок и загрузкой файла.
   - При выборе режима интерфейс адаптируется: отображается либо текстовое поле, либо поле загрузки файла.

2. **Обработка выбранного метода**:
   - Для ручного ввода ссылки разделяются по строкам.
   - Для файла ссылки считываются из текстового файла, каждая строка — одна ссылка.

3. **Коррекция URL**:
   - Если в URL отсутствуют параметры `service=WMS&request=GetCapabilities`, они автоматически добавляются.

4. **Универсальность**:
   - Независимо от способа ввода, вывод результатов формируется в одной таблице.

### Как использовать:
1. Выберите способ ввода:
   - "Ввести вручную" — заполните текстовое поле ссылками.
   - "Загрузить из файла" — выберите файл с ссылками (одна ссылка на строку).
2. Нажмите кнопку "Проверить ссылки".
3. В таблице появятся исправленные ссылки, их статус и список слоёв.

Если потребуется дальнейшее улучшение или добавление новых возможностей, дайте знать! 🚀



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
