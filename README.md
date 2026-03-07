<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Random Photo 275x404</title>
    <style>
        body { 
            font-family: sans-serif; 
            background: #1a1a1a; 
            margin: 0; 
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh; 
            color: white;
        }

        .photo-container {
            /* This forces the container to your specific dimensions */
            width: 275px;
            height: 404px;
            border: 4px solid #fff;
            border-radius: 8px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            background: #333;
            overflow: hidden;
        }

        #random-photo {
            width: 100%;
            height: 100%;
            /* 'cover' crops the edges slightly to prevent stretching */
            object-fit: cover; 
            display: block;
        }

        h1 { margin-bottom: 15px; font-size: 1.2rem; font-weight: 300; }
    </style>
</head>
<body>

    <h1>Distant Early Warning Card of the Day</h1>

    <div class="photo-container">
        <img id="random-photo" src="" alt="Loading random image...">
    </div>

    <script>
        // 1. ADD YOUR FILENAMES HERE
        const photos = [
            'photo1.jpg',
            'vacation.jpg',
            'dog.jpg',
            'sunset.jpg'
        ];

        const folder = 'photos/'; 

        // 2. LOGIC TO PICK ONE
        const randomIndex = Math.floor(Math.random() * photos.length);
        const selectedPhoto = photos[randomIndex];

        // 3. APPLY TO THE IMAGE TAG
        const imgElement = document.getElementById('random-photo');
        imgElement.src = folder + selectedPhoto;
    </script>

</body>
</html>
