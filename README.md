<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Comparison</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: #121212; 
            margin: 0; 
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh; 
            color: white;
        }

        /* Container for both cards */
        .card-wrapper {
            display: flex;
            gap: 20px; 
            flex-wrap: wrap; 
            justify-content: center;
        }

        .card {
            width: 195px;
            height: 300px;
            border: 2px solid #444;
            border-radius: 12px;
            overflow: hidden;
            background: #222;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .card img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }

        /* The new separate text row */
        .info-text {
            margin-top: 30px;
            text-align: left;
            font-size: 0.7rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 1px;
            line-height: 1.6;
            max-width: 80%;
        }

        .info-text a {
            color: #aaa;
            text-decoration: underline;
        }
    </style>
</head>
<body>

    <div class="card-wrapper">
        <div class="card">
            <img src="photos/distant.jpg" alt="Distant">
        </div>

        <div class="card">
            <img id="daily-photo" src="" alt="Daily Random">
        </div>
    </div>

    <div class="info-text">
        One Marshall McLuhan card per day.<br>
        Inspired by <a href="https://www.weirdstudies.com/112" target="_blank">episode 112 of Weird Studies</a>
    </div>

    <script>
        const folder = 'photos/';

        // 1. POOL OF RANDOM PHOTOS
        const photoPool = [
            '2heart.jpg', '2club.jpg', '2spade.jpg',
            '3club.jpg', '3dia.jpg', '3spade.jpg',
            '4dia.jpg', '4spade.jpg', '4heart.jpg',
            '5club.jpg', '5dia.jpg', '5heart.jpg', '5spade.jpg',
            '6club.jpg', '6dia.jpg', '6heart.jpg', '6spade.jpg',
            '7club.jpg', '7dia.jpg', '7heart.jpg',
            '8club.jpg', '8heart.jpg', '8spade.jpg',
            '9club.jpg', '9dia.jpg', '9heart.jpg', '9spade.jpg',
            '10club.jpg', '10dia.jpg', '10heart.jpg', '10spade.jpg',
            'aclub.jpg', 'aspade.jpg',
            'jclub.jpg', 'jdia.jpg', 'jheart.jpg', 'joker.jpg', 'jspade.jpg',
            'kclub.jpg', 'kdia.jpg', 'kheart.jpg', 'kspade.jpg',
            'qclub.jpg', 'qdia.jpg', 'qspade.jpg'
        ];

        // 2. DAILY LOGIC
        const now = new Date();
        const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));
        
        // 3. PICK IMAGE BASED ON THE DAY
        const dailyIndex = daysSinceEpoch % photoPool.length;
        const selectedPhoto = photoPool[dailyIndex];

        // 4. APPLY TO IMAGE
        // We can run this immediately since the script is at the bottom of the body
        document.getElementById('daily-photo').src = folder + selectedPhoto;
    </script>

</body> 
</html>
