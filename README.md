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
            margin-bottom: 20px; /* Space between the two rows */
        }

        /* Pull the second row up slightly if you want them tighter together */
        .card-wrapper.row-2 {
            margin-top: 0px;
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
            max-width: 625px; /* Aligns clean with three cards across */
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
            <img id="daily-photo-1" src="" alt="Daily Random 1">
        </div>
        <div class="card">
            <img id="daily-photo-2" src="" alt="Daily Random 2">
        </div>
    </div>

    <div class="card-wrapper row-2">
        <div class="card">
            <img id="new-row-card-1" src="" alt="New Row Card 1">
        </div>
        <div class="card">
            <img id="new-row-card-2" src="" alt="New Row Card 2">
        </div>
        <div class="card">
            <img id="new-row-card-3" src="" alt="New Row Card 3">
        </div>
    </div>

    <div class="info-text">
        Marshall McLuhan cards.<br>
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
        const folderPhotos = 'photos/';
        const folderCards = 'cards/'; // Target folder for the brand new row
        
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

        // Shared date calculator
        const now = new Date();
        const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));

        // Seeded random number generator function for consistent refreshes throughout the single day
        function seededRandom(seed) {
            const x = Math.sin(seed) * 10000;
            return x - Math.floor(x);
        }

        // --- ROW 1 INITIALIZATION (photos/ folder) ---
        let poolRow1 = [...photoPool];
        const selectedRow1 = [];
        
        for (let i = 0; i < 2; i++) {
            const seed = daysSinceEpoch + i;
            const idx = Math.floor(seededRandom(seed) * poolRow1.length);
            selectedRow1.push(poolRow1[idx]);
            poolRow1.splice(idx, 1);
        }
        document.getElementById('daily-photo-1').src = folderPhotos + selectedRow1[0];
        document.getElementById('daily-photo-2').src = folderPhotos + selectedRow1[1];


        // --- ROW 2 INITIALIZATION (cards/ folder) ---
        let poolRow2 = [...photoPool];
        const selectedRow2 = [];
        
        for (let i = 0; i < 3; i++) {
            // Added offset (+50) to the seed ensures completely different results from Row 1
            const seed = daysSinceEpoch + 50 + i;
            const idx = Math.floor(seededRandom(seed) * poolRow2.length);
            selectedRow2.push(poolRow2[idx]);
            poolRow2.splice(idx, 1);
        }
        document.getElementById('new-row-card-1').src = folderCards + selectedRow2[0];
        document.getElementById('new-row-card-2').src = folderCards + selectedRow2[1];
        document.getElementById('new-row-card-3').src = folderCards + selectedRow2[2];


        // --- GRID HOVER OVERLAY ---
        const btn = document.getElementById('view-deck-btn');
        const overlay = document.getElementById('grid-overlay');
        const closeBtn = document.getElementById('close-grid');
        const gridContent = document.getElementById('grid-content');
        const lightbox = document.getElementById('lightbox');
        const lightboxImg = document.getElementById('lightbox-img');

        // Combines both row exclusion lists so the grid doesn't duplicate what's featured on screen
        const activelyDisplayed = [...selectedRow1, ...selectedRow2];

        btn.onclick = () => {
            gridContent.innerHTML = ''; 
            photoPool.forEach(photo => {
                if (!activelyDisplayed.includes(photo)) { 
                    const div = document.createElement('div');
                    div.className = 'mini-card';
                    const img = document.createElement('img');
                    
                    // Defaulting grid view to your new cards directory. Swap with folderPhotos if preferred.
                    img.src = folderCards + photo; 
                    
                    div.onclick = () => {
                        lightboxImg.src = folderCards + photo;
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
</html><html lang="en">
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

        // 1. SELECT DAILY IMAGE
        const now = new Date();
        const daysSinceEpoch = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));
        const dailyIndex = daysSinceEpoch % photoPool.length;
        const dailyPhoto = photoPool[dailyIndex];
        document.getElementById('daily-photo').src = folder + dailyPhoto;

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
