# speech-to-text
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>محول الصوت إلى نص</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            flex-direction: column;
            font-family: Arial, sans-serif;
        }
        h1 {
            margin-bottom: 20px;
        }
        #result {
            width: 80%;
            padding: 15px;
            border: 2px solid #007BFF;
            border-radius: 5px;
            background-color: #fff;
            min-height: 150px;
            font-size: 18px;
            direction: rtl;
            text-align: right;
        }
        #startBtn {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            color: white;
            background-color: #007BFF;
            cursor: pointer;
            border-radius: 5px;
        }
        #startBtn:disabled {
            background-color: #6c757d;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <h1>محول الصوت إلى نص</h1>
    <div id="result" contenteditable="true">النص سيظهر هنا...</div>
    <button id="startBtn">ابدأ الاستماع</button>

    <script>
        const startBtn = document.getElementById('startBtn');
        const resultDiv = document.getElementById('result');
        let recognition;

        // التحقق من دعم المتصفح لـ Web Speech API
        if ('webkitSpeechRecognition' in window) {
            recognition = new webkitSpeechRecognition(); // إنشاء كائن جديد
            recognition.continuous = true; // الاستمرار في الاستماع حتى يتم الإيقاف يدويًا
            recognition.interimResults = false; // عرض النتائج النهائية فقط
            recognition.lang = 'ar-SA'; // تعيين اللغة إلى العربية

            recognition.onstart = function() {
                startBtn.disabled = true;
                startBtn.textContent = 'الاستماع...';
            };

            recognition.onresult = function(event) {
                let transcript = '';
                for (let i = event.resultIndex; i < event.results.length; i++) {
                    transcript += event.results[i][0].transcript;
                }
                resultDiv.textContent = transcript;
            };

            recognition.onerror = function(event) {
                console.error('خطأ في التعرف على الصوت', event.error);
                alert('حدث خطأ في التعرف على الصوت: ' + event.error);
            };

            recognition.onend = function() {
                startBtn.disabled = false;
                startBtn.textContent = 'ابدأ الاستماع';
            };

            startBtn.addEventListener('click', function() {
                recognition.start();
            });
        } else {
            startBtn.disabled = true;
            alert('متصفحك لا يدعم التعرف على الصوت.');
        }
    </script>
</body>
</html>
