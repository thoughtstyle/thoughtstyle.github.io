<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My GitHub Photo Gallery</title>
    <style>
        body { font-family: sans-serif; background: #f4f4f4; margin: 0; padding: 20px; }
        h1 { text-align: center; color: #333; }
        
        /* The Grid Layout */
        .gallery {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            grid-gap: 15px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .gallery-item img {
            width: 100%;
            height: 200px;
            object-fit: cover; /* Keeps images uniform */
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.2s;
        }

        .gallery-item img:hover { transform: scale(1.05); }
    </style>
</head>
<body>

    <h1>Project Memories</h1>
    <div class="gallery" id="photo-grid"></div>

    <script>
        // 1. LIST YOUR PHOTOS HERE
        // Add the exact filenames of the photos in your 'photos' folder
        const photos = [
            'photo1.jpg',
            'vacation.png',
            'dog.jpeg'
        ];

        // 2. THE PATH TO YOUR FOLDER
        const folder = 'photos/'; 

        const gallery = document.getElementById('photo-grid');

        // 3. GENERATE THE HTML
        photos.forEach(filename => {
            const div = document.createElement('div');
            div.className = 'gallery-item';
            div.innerHTML = `<img src="${folder}${filename}" alt="${filename}">`;
            gallery.appendChild(div);
        });
    </script>
</body>
</html>
