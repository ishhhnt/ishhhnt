<!DOCTYPE html><html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MUSIC PLAYER</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            background-color: #121212;
            color: white;
            height: 100vh;
        }
        .main {
            display: flex;
            flex: 1;
        }
        .sidebar {
            width: 250px;
            background: #000;
            height: 100vh;
            padding: 20px;
        }
        .content {
            flex: 1;
            padding: 20px;
        }
        .footer {
            position: fixed;
            bottom: 0;
            width: 100%;
            background: #181818;
            padding: 10px;
            text-align: center;
        }
        a {
            color: white;
            text-decoration: none;
            display: block;
            margin: 10px 0;
        }
        .file-upload {
            margin-top: 20px;
        }
        .controls {
            margin-top: 20px;
        }
        button {
            padding: 10px;
            margin: 5px;
            cursor: pointer;
        }
        input[type='range'] {
            width: 100%;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="main">
        <div class="sidebar">
            <h2>MUSIC PLAYER</h2>
            <label for="musicFile">Select Music:</label>
            <input type="file" id="musicFile" accept="audio/*">
            <a href="#">Search</a>
            <img src="./img/logo_line.gif" width="200" height="100">
            <h5>This is an ad-free application</h5>
            <h2>Made by Ishhnt</h2>
        </div>
        <div class="content">
            <h1>MUSIC PLAYER</h1>
            <p>Play your favorite music here.</p>
            <div class="controls">
                <button onclick="playMusic()">Play</button>
                <button onclick="pauseMusic()">Pause</button>
                <input type="range" id="seekBar" value="0" min="0" step="1">
            </div>
            <p id="musicTime">00:00 / 00:00</p>
            <audio id="audioPlayer" controls></audio>
        </div>
    </div>
    <div class="footer">
        <p id="nowPlaying">Now Playing: None</p>
    </div>
    <script>
        const audioPlayer = document.getElementById('audioPlayer');
        const musicFileInput = document.getElementById('musicFile');
        const nowPlaying = document.getElementById('nowPlaying');
        const musicTime = document.getElementById('musicTime');
        const seekBar = document.getElementById('seekBar');// Load last played music from localStorage
    window.onload = function() {
        const storedMusic = localStorage.getItem('musicFile');
        if (storedMusic) {
            audioPlayer.src = storedMusic;
            nowPlaying.textContent = `Now Playing: Saved Track`;
        }
    };

    musicFileInput.addEventListener('change', function() {
        const file = musicFileInput.files[0];
        if (file) {
            const objectURL = URL.createObjectURL(file);
            audioPlayer.src = objectURL;
            nowPlaying.textContent = `Now Playing: ${file.name}`;
            localStorage.setItem('musicFile', objectURL);
        }
    });

    function playMusic() {
        audioPlayer.play();
    }

    function pauseMusic() {
        audioPlayer.pause();
    }

    audioPlayer.addEventListener('timeupdate', function() {
        const currentMinutes = Math.floor(audioPlayer.currentTime / 60);
        const currentSeconds = Math.floor(audioPlayer.currentTime % 60);
        const durationMinutes = Math.floor(audioPlayer.duration / 60);
        const durationSeconds = Math.floor(audioPlayer.duration % 60);
        
        const formattedCurrent = `${currentMinutes}:${currentSeconds.toString().padStart(2, '0')}`;
        const formattedDuration = `${durationMinutes}:${durationSeconds.toString().padStart(2, '0')}`;
        
        musicTime.textContent = `${formattedCurrent} / ${formattedDuration}`;
        seekBar.value = audioPlayer.currentTime;
        seekBar.max = audioPlayer.duration;
    });

    seekBar.addEventListener('input', function() {
        audioPlayer.currentTime = seekBar.value;
    });
</script>

</body>
</html>
