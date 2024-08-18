<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Catatan Saya 😌</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: #f0f0f0;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            height: 100vh;
        }
        .header {
            width: 100%;
            padding: 20px;
            background-color: #007bff;
            color: white;
            text-align: center;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        .header h1 {
            margin: 0;
            font-size: 2em;
        }
        .container {
            width: 90%;
            max-width: 900px;
            margin-top: 50px;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
        }
        .menu {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }
        .menu button {
            background-color: #007bff;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.3s, transform 0.3s;
            margin: 0 5px;
        }
        .menu button:hover {
            background-color: #0056b3;
            transform: scale(1.05);
        }
        #notesArea, #notesDisplay {
            display: none;
        }
        #notes {
            width: 100%;
            height: 200px;
            padding: 10px;
            font-size: 1em;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        #notesDisplay {
            white-space: pre-wrap; /* Preserve whitespace and line breaks */
        }
        .footer {
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: white;
            text-align: center;
            position: fixed;
            bottom: 0;
            box-shadow: 0 -4px 8px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Catatan Saya😌</h1>
    </div>

    <div class="container">
        <div class="menu">
            <button onclick="showInput()">Input Notes</button>
            <button onclick="showNotes()">View Notes</button>
        </div>

        <div id="notesArea">
            <textarea id="notes" placeholder="Paste your notes here..."></textarea>
            <button onclick="saveNotes()">Save Notes</button>
        </div>

        <div id="notesDisplay"></div>
    </div>

    <div class="footer">
        <p>Current Date: <span id="currentDate"></span> | Current Time: <span id="currentTime"></span></p>
    </div>

    <script>
        function updateDateTime() {
            const now = new Date();
            document.getElementById('currentDate').textContent = now.toLocaleDateString();
            document.getElementById('currentTime').textContent = now.toLocaleTimeString();
        }
        setInterval(updateDateTime, 1000);
        updateDateTime();

        function showInput() {
            document.getElementById('notesArea').style.display = 'block';
            document.getElementById('notesDisplay').style.display = 'none';
        }

        function showNotes() {
            document.getElementById('notesArea').style.display = 'none';
            document.getElementById('notesDisplay').style.display = 'block';
            displayNotes();
        }

        function saveNotes() {
            const notes = document.getElementById('notes').value;
            const notesList = JSON.parse(localStorage.getItem('notesList') || '[]');
            notesList.push(notes);
            localStorage.setItem('notesList', JSON.stringify(notesList));
            alert('Notes saved!');
            document.getElementById('notes').value = ''; // Clear the textarea
        }

        function displayNotes() {
            const notesList = JSON.parse(localStorage.getItem('notesList') || '[]');
            const notesDisplay = document.getElementById('notesDisplay');
            notesDisplay.innerHTML = '';

            if (notesList.length === 0) {
                notesDisplay.textContent = 'No notes saved.';
            } else {
                notesList.forEach((note, index) => {
                    const noteItem = document.createElement('div');
                    noteItem.textContent = `Note ${index + 1}: ${note}`;
                    notesDisplay.appendChild(noteItem);
                });
            }
        }
    </script>
</body>
</html>
<script>
    // Fungsi untuk menampilkan efek hover pada tombol
    function addButtonHoverEffect() {
        const buttons = document.querySelectorAll('.menu button');
        buttons.forEach(button => {
            button.addEventListener('mouseover', () => {
                button.style.backgroundColor = '#0056b3'; // Efek hover
                button.style.transform = 'scale(1.05)';
            });
            button.addEventListener('mouseout', () => {
                button.style.backgroundColor = '#007bff'; // Warna default
                button.style.transform = 'scale(1)';
            });
        });
    }

    // Fungsi untuk menampilkan pesan konfirmasi dengan animasi
    function showConfirmationMessage(message) {
        const confirmation = document.createElement('div');
        confirmation.className = 'confirmation-message';
        confirmation.textContent = message;
        document.body.appendChild(confirmation);

        // Animasi masuk
        confirmation.style.opacity = 0;
        confirmation.style.transform = 'translateY(-20px)';
        setTimeout(() => {
            confirmation.style.transition = 'opacity 0.5s ease, transform 0.5s ease';
            confirmation.style.opacity = 1;
            confirmation.style.transform = 'translateY(0)';
        }, 100);

        // Hapus pesan setelah beberapa detik
        setTimeout(() => {
            confirmation.style.transition = 'opacity 0.5s ease, transform 0.5s ease';
            confirmation.style.opacity = 0;
            confirmation.style.transform = 'translateY(-20px)';
            setTimeout(() => {
                document.body.removeChild(confirmation);
            }, 500);
        }, 3000);
    }

    // Fungsi untuk menyimpan catatan
    function saveNotes() {
        const notes = document.getElementById('notes').value;
        const notesList = JSON.parse(localStorage.getItem('notesList') || '[]');
        notesList.push(notes);
        localStorage.setItem('notesList', JSON.stringify(notesList));
        showConfirmationMessage('Notes saved successfully!');
        document.getElementById('notes').value = ''; // Clear the textarea
    }

    // Fungsi untuk menampilkan catatan
    function displayNotes() {
        const notesList = JSON.parse(localStorage.getItem('notesList') || '[]');
        const notesDisplay = document.getElementById('notesDisplay');
        notesDisplay.innerHTML = '';

        if (notesList.length === 0) {
            notesDisplay.textContent = 'No notes saved.';
        } else {
            notesList.forEach((note, index) => {
                const noteItem = document.createElement('div');
                noteItem.textContent = `Note ${index + 1}: ${note}`;
                notesDisplay.appendChild(noteItem);
            });
        }
    }

    // Inisialisasi efek hover pada tombol
    addButtonHoverEffect();
</script>

<style>
    /* Gaya untuk pesan konfirmasi */
    .confirmation-message {
        position: fixed;
        top: 20px;
        right: 20px;
        padding: 10px 20px;
        background-color: #28a745;
        color: white;
        border-radius: 4px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        font-size: 1em;
        z-index: 1000;
        transition: opacity 0.5s ease, transform 0.5s ease;
    }
</style>
<script>
    // Fungsi untuk animasi fade-in pada elemen
    function fadeIn(element) {
        element.style.opacity = 0;
        element.style.transform = 'translateY(20px)';
        element.style.transition = 'opacity 0.5s ease-out, transform 0.5s ease-out';
        setTimeout(() => {
            element.style.opacity = 1;
            element.style.transform = 'translateY(0)';
        }, 100);
    }

    // Fungsi untuk animasi bounce pada tombol
    function addButtonBounceEffect() {
        const buttons = document.querySelectorAll('.menu button');
        buttons.forEach(button => {
            button.addEventListener('mouseover', () => {
                button.style.animation = 'bounce 0.5s';
            });
            button.addEventListener('animationend', () => {
                button.style.animation = '';
            });
        });
    }

    // Tambahkan animasi fade-in pada konten
    function applyFadeInToContent() {
        const contentElements = document.querySelectorAll('#content, #fileList, #notesDisplay');
        contentElements.forEach(element => {
            fadeIn(element);
        });
    }

    // Definisikan keyframes untuk animasi bounce
    const style = document.createElement('style');
    style.innerHTML = `
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {
                transform: translateY(0);
            }
            40% {
                transform: translateY(-30px);
            }
            60% {
                transform: translateY(-15px);
            }
        }
    `;
    document.head.appendChild(style);

    // Inisialisasi efek animasi
    addButtonBounceEffect();
    applyFadeInToContent();
</script>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Website with Welcome Animation</title>
    <style>
        /* Gaya animasi welcome */
        .welcome-box {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #007bff;
            color: #fff;
            padding: 20px 40px;
            border-radius: 10px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            opacity: 0;
            z-index: 9999;
            animation: fadeInOut 5s ease-in-out forwards;
        }

        @keyframes fadeInOut {
            0% {
                opacity: 0;
                transform: translate(-50%, -60%);
            }
            10% {
                opacity: 1;
                transform: translate(-50%, -50%);
            }
            90% {
                opacity: 1;
                transform: translate(-50%, -50%);
            }
            100% {
                opacity: 0;
                transform: translate(-50%, -40%);
            }
        }
    </style>
</head>
<body>

    <!-- Animasi Welcome -->
    <div class="welcome-box">
        <h2>Welcome</h2>
        <p>to My Website</p>
    </div>

    <script>
        // Menghilangkan animasi setelah 5 detik agar tidak mengganggu
        setTimeout(() => {
            const welcomeBox = document.querySelector('.welcome-box');
            if (welcomeBox) {
                welcomeBox.style.display = 'none';
            }
        }, 5000);
    </script>

</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome Sound</title>
</head>
<body>
    <!-- Element Audio -->
    <audio id="welcome-audio" autoplay>
        <source src="https://www.soundjay.com/button/sounds/button-1.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <script>
        // Untuk memastikan suara diputar saat website dibuka
        window.addEventListener('load', function () {
            const audio = document.getElementById('welcome-audio');
            audio.play().catch(error => {
                console.error('Suara tidak dapat diputar secara otomatis:', error);
            });
        });
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome Sound</title>
</head>
<body>
    <!-- Element Audio -->
    <audio id="welcome-audio" autoplay>
        <source src="https://www.soundjay.com/button/sounds/button-4.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <script>
        window.addEventListener('load', function () {
            const audio = document.getElementById('welcome-audio');
            audio.play().catch(error => {
                console.error('Suara tidak dapat diputar secara otomatis:', error);
            });
        });
    </script>
</body>
</html>

        
                                <!DOCTYPE html>
                                <html lang="en">
                                <head>
                                    <meta charset="UTF-8">
                                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                                    <title>Interactive Clock and Weather</title>
                                    <style>
                                        body {
                                            font-family: Arial, sans-serif;
                                            background-color: #f0f0f0;
                                            margin: 0;
                                            padding: 0;
                                        }
                                        .clock-weather-container {
                                            position: fixed;
                                            top: 70px;
                                            left: 5px;
                                            background-color: #fff;
                                            display: flex;
                                            justify-content: center;
                                            align-items: center;
                                            height: 50px;
                                            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
                                            padding: 5px 10px;
                                            border-radius: 10px;
                                            animation: slideIn 0.5s ease-in-out;
                                            z-index: 9999;
                                        }
                                        .clock {
                                            font-size: 0.9em;
                                            margin-right: 10px;
                                            animation: fadeIn 2s ease-in-out;
                                        }
                                        .emoticon {
                                            font-size: 1.2em;
                                            margin-right: 10px;
                                            animation: bounce 1s infinite;
                                        }
                                        .weather-info {
                                            font-size: 0.8em;
                                            animation: fadeIn 3s ease-in-out;
                                        }
                                        .morning { color: #FFD700; }
                                        .afternoon { color: #FF6347; }
                                        .evening { color: #FF4500; }
                                        .night { color: #1E90FF; }

                                        /* Animations */
                                        @keyframes slideIn {
                                            from { transform: translateY(-100%); }
                                            to { transform: translateY(0); }
                                        }
                                        @keyframes fadeIn {
                                            from { opacity: 0; }
                                            to { opacity: 1; }
                                        }
                                        @keyframes bounce {
                                            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
                                            40% { transform: translateY(-5px); }
                                            60% { transform: translateY(-3px); }
                                        }
                                    </style>
                                </head>
                                <body>

                                    <div class="clock-weather-container">
                                        <div class="clock" id="clock"></div>
                                        <div class="emoticon" id="emoticon">🌅</div>
                                        <div class="weather-info" id="weatherInfo">Loading weather...</div>
                                    </div>

                                    <script>
                                        function updateClock() {
                                            const now = new Date();
                                            const hours = now.getHours();
                                            const minutes = now.getMinutes();
                                            const seconds = now.getSeconds();

                                            const timeString = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
                                            document.getElementById('clock').textContent = timeString;

                                            const emoticon = document.getElementById('emoticon');
                                            const clockWeatherContainer = document.querySelector('.clock-weather-container');

                                            if (hours >= 6 && hours < 12) {
                                                emoticon.textContent = '🌅';
                                                clockWeatherContainer.className = 'clock-weather-container morning';
                                            } else if (hours >= 12 && hours < 18) {
                                                emoticon.textContent = '☀️';
                                                clockWeatherContainer.className = 'clock-weather-container afternoon';
                                            } else if (hours >= 18 && hours < 21) {
                                                emoticon.textContent = '🌇';
                                                clockWeatherContainer.className = 'clock-weather-container evening';
                                            } else {
                                                emoticon.textContent = '🌙';
                                                clockWeatherContainer.className = 'clock-weather-container night';
                                            }
                                        }

                                        function fetchWeather() {
                                            const apiKey = f60d9255b4187b2c0a4cccf9435c2eea; // Replace with your API key
                                            const city = 'Jember';
                                            const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

                                            fetch(url)
                                                .then(response => response.json())
                                                .then(data => {
                                                    const weatherInfo = document.getElementById('weatherInfo');

                                                    const description = data.weather[0].description;
                                                    const temp = data.main.temp;
                                                    const humidity = data.main.humidity;
                                                    const windSpeed = data.wind.speed;

                                                    weatherInfo.innerHTML = `${description}, ${temp}°C, Humidity: ${humidity}%`;
                                                })
                                                .catch(error => {
                                                    document.getElementById('weatherInfo').textContent = 'Unable to retrieve weather data.';
                                                });
                                        }

                                        setInterval(updateClock, 1000);
                                        updateClock();
                                        fetchWeather();
                                    </script>
                                </body>
                                </html>
