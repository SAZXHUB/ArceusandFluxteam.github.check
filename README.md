<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Status Updater</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
            font-family: 'Roboto', sans-serif;
            color: white;
            text-align: center;
            background: linear-gradient(135deg, #000428, #004e92);
            background-size: cover;
            background-repeat: no-repeat;
            background-attachment: fixed;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            position: relative;
        }
        body::before {
            content: '';
            position: absolute;
            top: -10px;
            left: -10px;
            width: calc(100% + 20px);
            height: calc(100% + 20px);
            border: 10px solid;
            border-image: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047) 1;
            pointer-events: none;
            animation: galaxy-border 10s ease infinite;
            box-sizing: border-box;
        }
        h1 {
            font-size: 3em;
            margin: 0;
            z-index: 1;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.7);
        }
        #status {
            font-size: 2em;
            margin-top: 20px;
            z-index: 1;
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: center;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.7);
        }
        .online {
            color: lightgreen;
        }
        .offline {
            color: red;
        }
        .button {
            margin-top: 20px;
            padding: 15px 30px;
            background: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047);
            background-size: 600% 600%;
            color: white;
            border: 2px solid transparent;
            border-radius: 30px;
            cursor: pointer;
            font-size: 1.2em;
            font-weight: bold;
            animation: galaxy-button 10s ease infinite;
            transition: all 0.3s ease;
            z-index: 1;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        .button:hover {
            border-color: white;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.7);
            transform: scale(1.05);
        }
        @keyframes galaxy-border {
            0% { border-image-source: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047); }
            50% { border-image-source: linear-gradient(225deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047); }
            100% { border-image-source: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047); }
        }
        @keyframes galaxy-button {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        @keyframes falling-stars {
            0% { transform: translateY(-1500px) translateX(-1500px); }
            100% { transform: translateY(1500px) translateX(1500px); }
        }
        .falling-star {
            position: absolute;
            top: -10px;
            left: 50%;
            width: 3px;
            height: 1500px;
            background: linear-gradient(to bottom, rgba(255, 255, 255, 0), rgba(255, 255, 255, 1));
            opacity: 0.7;
            transform: translateY(-1500px) translateX(-1500px);
            animation: falling-stars 5s linear infinite;
            box-shadow: 0 0 15px 5px rgba(255, 255, 255, 0.7);
        }
        .falling-star:nth-child(2) {
            left: 25%;
            animation-delay: 1s;
        }
        .falling-star:nth-child(3) {
            left: 75%;
            animation-delay: 2s;
        }
        .falling-star:nth-child(4) {
            left: 40%;
            animation-delay: 3s;
        }
        .falling-star:nth-child(5) {
            left: 60%;
            animation-delay: 4s;
        }
        .falling-star::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            width: 10px;
            height: 10px;
            background: radial-gradient(circle, rgba(255, 255, 255, 1) 0%, rgba(255, 255, 255, 0) 70%);
            border-radius: 50%;
            transform: translateX(-50%);
            box-shadow: 0 0 20px 10px rgba(255, 255, 255, 0.7);
        }
    </style>
</head>
<body>
    <div class="falling-star"></div>
    <div class="falling-star"></div>
    <div class="falling-star"></div>
    <div class="falling-star"></div>
    <div class="falling-star"></div>
    <h1>Status Updater</h1>
    <div id="status">Checking status for: spdmteam.com</div>
    <button class="button" onclick="checkStatus('spdmteam.com', 'https://i.postimg.cc/3rPvbDhY/Mobile-Image-7a84dac27ba539ba6cf9.webp')">Check spdmteam.com Status</button>
    <button class="button" onclick="checkStatus('flux.li', 'https://i.postimg.cc/yYLz80Nz/Screenshot-2024-0605-215849.jpg')">Check flux.li Status</button>
    <script>
        function updateStatus(message) {
            const statusDiv = document.getElementById('status');
            statusDiv.textContent = message;
        }

        function checkStatus(website, background) {
            updateStatus("Checking status for: " + website);
            document.body.style.backgroundImage = "url('" + background + "')";
            fetch('https://' + website, {mode: 'no-cors'})
                .then(response => {
                    const statusDiv = document.getElementById('status');
                    if (response.ok || response.type === 'opaque') {
                        updateStatus(website + " is online");
                        statusDiv.className = 'online';
                    } else {
                        updateStatus(website + " is offline");
                        statusDiv.className = 'offline';
                    }
                })
                .catch(error => {
                    console.error('Error fetching status:', error);
                    updateStatus(website + " is offline");
                    const statusDiv = document.getElementById('status');
                    statusDiv.className = 'offline';
                });
        }
    </script>
</body>
</html>
