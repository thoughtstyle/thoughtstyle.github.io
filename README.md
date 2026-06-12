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
            justify-content: flex-start;
            min-height: 100vh;           
            padding: 40px 20px;          
            color: white;
            overflow-y: auto;            
        }

        .card-wrapper {
            display: flex;
            gap: 20px; 
            flex-wrap: wrap; 
            justify-content: center;
            width: 100%;
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

        /* ROW 2 ONLY: Changes height dynamically for the lower 3 cards */
        .row-2-wrapper .card {
            height: auto; 
            aspect-ratio: 2 / 3; 
        }

        .card img {
            width: 100%;
            height: 100%;
            object-fit: cover; 
            display: block;
        }

        /* ROW 2 IMAGES ONLY: Keeps full frame uncropped */
        .row-2-wrapper .card img {
            object-fit: fill; 
        }

        /* LEFT JUSTIFIED TEXT */
        .info-text {
            margin-top: 20px;
            margin-bottom: 40px; 
            text-align: left;
            font-size: 0.7rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 1px;
            line-height: 1.6;
            width: 100%;
            max-width: 410px; 
        }

        .info-text a { color: #aaa; text-decoration: underline; }

        /* HEADER TEXT BETWEEN THE TWO ROWS */
        .deck-header-text {
            margin-top: 30px;
            margin-bottom: 20px;
            font-size: 0.75rem;
            color: #aaa;
            text-transform: uppercase;
            letter-spacing: 2px;
            font-weight: 600;
            text-align: center;
        }

        /* LIGHTBOX */
        #lightbox {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.92);
            z-index: 200;
            justify-content: center;
            align-items: center;
            cursor: zoom-out;
        }

        #lightbox img {
            max-height: 85vh;
            max-width: 90vw;
            border-radius: 12px;
            box-shadow: 0 0 40px rgba(0,0,0,1);
        }
    </style>
</head>
<body>

    <!-- ROW 1: Contains exactly 2 cards -->
    <div class="card-wrapper">
        <div class="card">
            <img src="photos/distant.jpg" alt="Distant">
        </div>
        <div class="card">
            <img id="daily-photo" src="" alt="Daily Random">
        </div>
    </div>

    <!-- TEXT AREA BELOW ROW 1 -->
    <div class="info-text">
        One Marshall McLuhan card per day.<br>
        Inspired by <a href="https://www.weirdstudies.com/112" target="_blank">episode 112 of Weird Studies</a>
    </div>

    <!-- MIDDLE TITLE HEADER -->
    <div class="deck-header-text">Lenormand Deck Cards of the Day</div>

    <!-- ROW 2: Contains exactly 3 cards -->
    <div class="card-wrapper row-2-wrapper">
        <div class="card">
            <img id="row2-card-1" src="" alt="Daily Card 1">
        </div>
        <div class="card">
            <img id="row2-card-2" src="" alt="Daily Card 2">
        </div>
        <div class="card">
            <img id="row2-card-3" src="" alt="Daily Card 3">
        </div>
    </div>

    <!-- POPUP ZOOM LIGHTBOX -->
    <div id="lightbox">
        <img id="lightbox-img" src="" alt="Full Size">
    </div>

    <script>
        const folder = 'photos/';
        const photoPool = [
            '2heart.jpg', '2club.jpg', '2spade.jpg', '3club.jpg', '3dia.jpg', '3spade.jpg',
            '4dia.jpg', '4spade.jpg', '4heart.jpg', '5club.jpg', '5dia.jpg', '5heart.jpg', 
            '5spade.jpg', '6club.jpg', '6dia.jpg', '6heart.jpg', '6spade.jpg', '7club.jpg', 
            '7dia.jpg', '7heart.jpg', '8club.jpg', '8heart.jpg', '8spade.jpg', '9club.jpg', 
            '9dia.jpg', '9heart.jpg', '9spade.jpg', '10club.jpg', '10dia.jpg', '10heart.jpg', 
            '10spade.jpg', 'aclub.jpg', 'aspade.jpg', 'jclub.jpg', 'jdia.jpg', 'jheart.jpg', 
            'joker.jpg', 'jspade.jpg', 'kclub.jpg', 'kdia.jpg', 'kheart.jpg', 'kspade.jpg',
            'qclub.jpg', 'qdia.jpg', 'qspade.jpg'
        ];

        function seededRandom(seed) {
            const x = Math.sin(seed) * 10000;
            return x - Math.floor(x);
        }

        const now = new Date();
        const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));

        // 1. SELECT DAILY IMAGE FOR ROW 1
        const row1Seed = daysSinceEpoch + 100;
        const dailyIndex = Math.floor(seededRandom(row1Seed) * photoPool.length);
        const dailyPhoto = photoPool[dailyIndex];
        document.getElementById('daily-photo').src = folder + dailyPhoto;

        // 2. GENERATE POOL & RANDOMIZE 3 UNIQUE CARDS FOR ROW 2 EACH DAY
        const cardsFolder = 'cards/';
        let cardsPool = [];
        
        for (let i = 1; i <= 26; i++) {
            let paddedNum = i.toString().padStart(2, '0');
            cardsPool.push(`${paddedNum}.jpg`);
        }

        const selectedRow2Cards = [];
        for (let i = 0; i < 3; i++) {
            const row2Seed = daysSinceEpoch + 500 + i; 
            const pickIndex = Math.floor(seededRandom(row2Seed) * cardsPool.length);
            
            selectedRow2Cards.push(cardsPool[pickIndex]);
            cardsPool.splice(pickIndex, 1); 
        }

        document.getElementById('row2-card-1').src = cardsFolder + selectedRow2Cards[0];
        document.getElementById('row2-card-2').src = cardsFolder + selectedRow2Cards[1];
        document.getElementById('row2-card-3').src = cardsFolder + selectedRow2Cards[2];

        // 3. LIGHTBOX SYSTEM FOR IMAGES
        const lightbox = document.getElementById('lightbox');
        const lightboxImg = document.getElementById('lightbox-img');

        document.querySelectorAll('.card img').forEach(img => {
            img.style.cursor = 'zoom-in';
            img.onclick = () => {
                if (img.src) {
                    lightboxImg.src = img.src;
                    lightbox.style.display = 'flex';
                }
            };
        });

        lightbox.onclick = () => lightbox.style.display = 'none';
    </script>
</body> 
</html>
