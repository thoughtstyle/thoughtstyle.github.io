<script>
    const folder = 'photos/';

    // 1. LIST YOUR POOL OF RANDOM PHOTOS HERE
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
    // We get a unique number for "today" (days since 1970)
    const now = new Date();
    // Using UTC date ensures the card changes at the same time for everyone
    const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));
    
    // 3. PICK IMAGE BASED ON THE DAY
    const dailyIndex = daysSinceEpoch % photoPool.length;
    const selectedPhoto = photoPool[dailyIndex];

    // 4. APPLY TO IMAGE
    // Ensure the ID exists before setting src
    window.onload = function() {
        document.getElementById('daily-photo').src = folder + selectedPhoto;
    };
</script><html lang="en">
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
            gap: 20px; /* Space between the cards */
            flex-wrap: wrap; /* Allows stacking on small mobile screens */
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

        .label {
            text-align: center;
            margin-top: 10px;
            font-size: 0.4rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
    </style>
</head>
<body>

    <div class="card-wrapper">
        <div>
            <div class="card">
                <img src="photos/distant.jpg" alt="Distant">
            </div>
            <div class="label">One Marshall McLuhan card per day</div>
        </div>

        <div>
            <div class="card">
                <img id="daily-photo" src="" alt="Daily Random">
            </div>
            <div class="label">Inspired by <a href="https://www.weirdstudies.com/112">episode 112 of Weird Studies</a></div>
        </div>
    </div>

    <script>
        // 1. LIST YOUR POOL OF RANDOM PHOTOS HERE
        // (Do not include distant.jpg here if you don't want it to repeat on the right)
        const photoPool = [
            '2heart.jpg',
            '2club.jpg',
            '2spade.jpg',
            '3club.jpg',
            '3dia.jpg',
            '3spade.jpg',
            '4dia.jpg',
            '4spade.jpg',
            '4heart.jpg',
            '4spade.jpg',
            '5club.jpg',
            '5dia.jpg',
            '5spade.jpg',
            '5club.jpg',
            '6club.jpg',
            '6dia.jpg',
            '6heart.jpg',
            '6spade.jpg',
            '7club.jpg',
            '7dia.jpg',
            '7heart.jpg',
            '8club.jpg',
            '8heart.jpg',
            '8spade.jpg',
            '9club.jpg',
            '9dia.jpg',
            '9heart.jpg',
            '9spade.jpg',
            '10club.jpg',
            '10dia.jpg',
            '10heart.jpg',
            '10spade.jpg',
            'aclub.jpg',
            'aspade.jpg',
            'jclub.jpg',
            'jdia.jpg',
            'jheart.jpg',
            'joker.jpg',
            'jspade.jpg',
            'kclub.jpg',
            'kdia.jpg',
            'kheart.jpg',
            'kspade.jpg',
            'qclub.jpg',
            'qdia.jpg',
            'qspade.jpg',
        ];

        const folder = 'photos/';

        // 2. DAILY LOGIC
        // We get a unique number for "today" (number of days since 1970)
        const now = new Date();
        const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));
        
        // 3. PICK IMAGE BASED ON THE DAY
        // Using the modulus (%) operator ensures we stay within the list length
        const dailyIndex = daysSinceEpoch % photoPool.length;
        const selectedPhoto = photoPool[dailyIndex];

        // 4. APPLY TO IMAGE
        document.getElementById('daily-photo').src = folder + selectedPhoto;
    </script>

</body> 
</html>
