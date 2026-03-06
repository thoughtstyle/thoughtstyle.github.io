<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Random Photo of the Day</title>
    <style>
        /* Centers everything on the page */
        body { 
            font-family: sans-serif; 
            background: #1a1a1a; /* Dark background looks great for photos */
            margin: 0; 
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh; 
            color: white;
        }

        .photo-container {
            max-width: 90%;
            max-height: 80vh;
            border: 5px solid white;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            overflow: hidden;
            background: #333;
        }

        #random-photo {
            display: block;
            max-width: 100%;
            max-height: 80vh;
            object-fit: contain; /* Ensures the whole photo fits */
        }

        h1 { margin-bottom: 20px; font-weight: 300; }
    </style>
</head>
<body>

    <h1>Surprise Photo</h1>

    <div class="photo-container">
        <img id="random-photo" src="" alt="Loading...">
    </div>

    <script>
        // 1. LIST YOUR PHOTOS HERE
        const photos = [
            'photo1.jpg',
            'vacation.png',
            'dog.jpeg',
            'sunset.jpg',
            'forest.webp'
        ];

        // 2. THE PATH TO YOUR FOLDER
        const folder = 'photos/'; 

        // 3. PICK ONE RANDOM PHOTO
        // Math.random() gives a number between 0 and 1
        // We multiply by the list length and round down to get a valid index
        const randomIndex = Math.floor(Math.random() * photos.length);
        const selectedPhoto = photos[randomIndex];

        // 4. DISPLAY IT
        const imgElement = document.getElementById('random-photo');
        imgElement.src = folder + selectedPhoto;
        imgElement.alt = "Random Image: " + selectedPhoto;
    </script>

</body>
</html>
