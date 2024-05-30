---
toc: true
comments: false
layout: post
title: Music recommendation 
courses: { compsci: {week: 26} }
type: hacks
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Recommendation</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            text-align: center;
        }
        h1 {
            color: #444;
            font-size: 36px;
            margin-bottom: 10px;
        }
        h2 {
            color: #666;
            font-size: 28px;
            margin-bottom: 20px;
        }
        form {
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            display: inline-block;
            text-align: left;
            margin-bottom: 20px;
        }
        .genre-input {
            margin-bottom: 10px;
        }
        label {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 18px;
            color: #333;
        }
        select {
            padding: 5px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        #recommendations {
            font-size: 22px;
            color: #333;
            margin-top: 20px;
        }
        .music-note {
            width: 50px;
            height: 50px;
            margin: 0 10px;
            vertical-align: middle;
        }
    </style>
</head>
<body>
    <h1>Music Recommendation</h1>
    <form id="genreForm" onsubmit="event.preventDefault(); getRecommendations();">
        <h2>Rate Your Enjoyment of These Genres (1 to 5)</h2>
        <div id="genreInputs"></div>
        <button type="submit">Get Recommendations</button>
    </form>
    <h2>Recommended Artist</h2>
    <div id="recommendations"></div>
    <script>
        // Sample data for genres and artists
        const genres = ['Rock', 'Pop', 'Jazz', 'Classical', 'Hip-Hop', 'Country'];
        const artists = {
            'Rock': ['Bruce Springsteen', 'Foo Fighters', 'Green Day', 'Nirvana', 'The Eagles'],
            'Pop': ['Taylor Swift', 'Ariana Grande', 'Billie Eilish', 'Katy Perry', 'Bruno Mars'],
            'Jazz': ['Herbie Hancock', 'Wynton Marsalis', 'Norah Jones', 'Esperanza Spalding', 'Diana Krall'],
            'Classical': ['Leonard Bernstein', 'John Williams', 'Philip Glass', 'Yo-Yo Ma', 'Joshua Bell'],
            'Hip-Hop': ['Kendrick Lamar', 'Drake', 'J. Cole', 'Cardi B', 'Post Malone'],
            'Country': ['Garth Brooks', 'Carrie Underwood', 'Blake Shelton', 'Luke Bryan', 'Miranda Lambert']
        };
        let currentArtistIndex = 0;
        // Populate the genre input fields with rating scales
        const genreInputs = document.getElementById('genreInputs');
        genres.forEach(genre => {
            const div = document.createElement('div');
            div.className = 'genre-input';
            div.innerHTML = `<label>${genre}: 
                <select id="${genre}">
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                </select>
            </label>`;
            genreInputs.appendChild(div);
        });
        function getRecommendations() {
            const ratings = {};
            genres.forEach(genre => {
                ratings[genre] = parseInt(document.getElementById(genre).value);
            });
            const recommendedArtists = recommendArtists(ratings);
            displayRecommendations(recommendedArtists);
        }
        function recommendArtists(ratings) {
            let maxRating = -1;
            let favoriteGenre = '';
            for (const genre in ratings) {
                if (ratings[genre] > maxRating) {
                    maxRating = ratings[genre];
                    favoriteGenre = genre;
                }
            }
            return artists[favoriteGenre] || [];
        }
        function displayRecommendations(artistList) {
            const recommendationsDiv = document.getElementById('recommendations');
            if (artistList.length > 0) {
                const artist = artistList[currentArtistIndex % artistList.length];
                recommendationsDiv.innerHTML = artist;
                currentArtistIndex++;
            } else {
                recommendationsDiv.innerHTML = 'No recommendations available.';
            }
        }
        // Initialize recommendations on page load
        document.addEventListener('DOMContentLoaded', getRecommendations);
    </script>
</body>
</html>
