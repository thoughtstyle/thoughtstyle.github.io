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
        <div class
