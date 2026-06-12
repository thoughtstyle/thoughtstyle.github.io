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
            overflow: hidden;
        }

        .card-wrapper {
            display: flex;
            gap: 20px; 
            flex-wrap: wrap; 
            justify-content: center;
            margin-bottom: 20px; /* Adds spacing between your original row and the new row */
        }
        
        /* Optional: adjust spacing on the last wrapper block */
        .card-wrapper:nth-of-type(2) {
            margin-bottom: 0px;
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

        /* LEFT JUSTIFIED TEXT */
        .info-text {
            margin-top: 30px;
            text-align: left;
            font-size: 0.7rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 1px;
            line-height: 1.6;
            width: 100%;
            max-width: 410px; /* Aligns roughly with the width of two cards + gap */
        }

        .info-text a { color: #aaa; text-decoration: underline; }

        #view-deck-btn {
            margin-top: 20px;
            padding: 8px 16px;
            background: transparent;
            color: #888;
            border: 1px solid #444;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.7rem;
            text-transform: uppercase;
            align-self: center;
        }

        /* GRID OVERLAY */
        #grid-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(10, 10, 10, 0.98);
            z-index: 100;
            overflow-y: auto;
            padding: 60px 20px;
            box-sizing: border-box;
        }

        /* BIGGER GRID IMAGES */
        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .mini-card {
            aspect-ratio: 2/3;
            border: 2px solid #333;
            border-radius: 8px;
            overflow: hidden;
            cursor: pointer;
            transition: transform 0.2s, border-color 0.2s;
        }

        .mini-card:hover { transform: scale(1.05); border-color: #777; }
        .mini-card img { width: 100%; height: 100%; object-fit: cover; }

        .close-btn {
            position: fixed;
            top: 20px;
            right: 30px;
            font-size: 2rem;
            color: white;
            cursor: pointer;
            background: rgba(40,40,40,0.8);
            border: none;
            border-radius: 50%;
            width: 45px;
            height: 45px;
            z-index: 110;
            display: flex;
            align-items: center;
            justify-content: center;
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

    <div class="card-wrapper">
        <div class="card">
            <img src="photos/distant.jpg" alt="Distant">
        </div>
        <div class="card">
            <img id="daily-photo" src="" alt="Daily Random">
        </div>
    </div>

    <div class="card-wrapper">
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

    <div class="info-text">
        One Marshall McLuhan card per day.<br>
        Inspired by <a href="https://www.weirdstudies.com/112" target="_blank">episode 112 of Weird Studies</a>
    </div>

    <button id="view-deck-btn">View Full Deck</button>

    <div id="grid-overlay">
        <button class="close-btn" id="close-grid">&times;</button>
        <div class="grid-container" id="grid-content"></div>
    </div>

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

        // 1. SELECT DAILY IMAGE FOR ROW 1 (UNCHANGED)
        const now = new Date();
        const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));
        const dailyIndex = daysSinceEpoch % photoPool.length;
        const dailyPhoto = photoPool[dailyIndex];
        document.getElementById('daily-photo').src = folder + dailyPhoto;

        // --- NEW CODE: SELECT 3 RANDOM UNIQUE CARDS FOR ROW 2 ---
        const cardsFolder = 'cards/';
        let cardsPool = [...photoPool]; // Copy deck pool to track duplicates
        
        // Simple predictable randomizer function based on date seed
        function seededRandom(seed) {
            const x = Math.sin(seed) * 10000;
            return x - Math.floor(x);
        }

        const selectedRow2Cards = [];
        for (let i = 0; i < 3; i++) {
            // Adjusting seed index safely per card slot so they generate randomly but stay static today
            const currentSeed = daysSinceEpoch + 77 + i; 
            const pickIndex = Math.floor(seededRandom(currentSeed) * cardsPool.length);
            
            selectedRow2Cards.push(cardsPool[pickIndex]);
            cardsPool.splice(pickIndex, 1); // Remove card from temporary sub-pool to eliminate duplicate draws
        }

        // Apply images to Row 2 image elements
        document.getElementById('row2-card-1').src = cardsFolder + selectedRow2Cards[0];
        document.getElementById('row2-card-2').src = cardsFolder + selectedRow2Cards[1];
        document.getElementById('row2-card-3').src = cardsFolder + selectedRow2Cards[2];
        // --- END NEW CODE ---

        // 2. GRID ELEMENTS
        const btn = document.getElementById('view-deck-btn');
        const overlay = document.getElementById('grid-overlay');
        const closeBtn = document.getElementById('close-grid');
        const gridContent = document.getElementById('grid-content');
        const lightbox = document.getElementById('lightbox');
        const lightboxImg = document.getElementById('lightbox-img');

        // Open Grid (Excluding the daily photo)
        btn.onclick = () => {
            gridContent.innerHTML = ''; 
            photoPool.forEach(photo => {
                if (photo !== dailyPhoto) { 
                    const div = document.createElement('div');
                    div.className = 'mini-card';
                    const img = document.createElement('img');
                    img.src = folder + photo;
                    
                    div.onclick = () => {
                        lightboxImg.src = folder + photo;
                        lightbox.style.display = 'flex';
                    };
                    
                    div.appendChild(img);
                    gridContent.appendChild(div);
                }
            });
            overlay.style.display = 'block';
        };

        closeBtn.onclick = () => overlay.style.display = 'none';
        lightbox.onclick = () => lightbox.style.display = 'none';
    </script>
</body> 
</html>
