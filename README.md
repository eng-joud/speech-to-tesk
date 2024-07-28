إنشاء واجهة المستخدم
<!DOCTYPE html>
<html>
<head>
    <title>Speech to Text</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
        }
        .container {
            margin-top: 50px;
        }
        button {
            padding: 10px 20px;
            font-size: 20px;
            margin-top: 20px;
            cursor: pointer;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Speech to Text</h1>
    <div class="container">
        <button id="start-recording">Start Recording</button>
        <div id="result"></div>
    </div>

    <script>
        const startButton = document.getElementById('start-recording');
        const resultDiv = document.getElementById('result');

        startButton.addEventListener('click', () => {
            const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            recognition.lang = 'en-US';
            recognition.start();

            recognition.onresult = (event) => {
                const text = event.results[0][0].transcript;
                resultDiv.innerText = text;

                // Send the text to the server
                const xhr = new XMLHttpRequest();
                xhr.open('POST', 'save_text.php', true);
                xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
                xhr.onreadystatechange = function() {
                    if (xhr.readyState === 4 && xhr.status === 200) {
                        alert('Text saved to database.');
                    }
                };
                xhr.send('text=' + encodeURIComponent(text));
            };

            recognition.onerror = (event) => {
                console.error('Speech recognition error:', event);
            };
        });
    </script>
</body>
</html>
