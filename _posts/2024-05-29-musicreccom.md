---
toc: true
comments: false
layout: post
title: Music recommendation 
courses: { compsci: {week: 26} }
type: hacks
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Recommendation</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h1, h2 {
            color: #333;
        }
        form {
            margin-bottom: 20px;
        }
        .genre-input {
            margin-bottom: 10px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
        }
        #recommendations {
            font-size: 18px;
            color: #555;
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
            'Rock': ['Queen', 'Nirvana', 'The Rolling Stones', 'Pink Floyd', 'AC/DC'],
            'Pop': ['Taylor Swift', 'Ariana Grande', 'Billie Eilish', 'Ed Sheeran', 'Dua Lipa'],
            'Jazz': ['Miles Davis', 'John Coltrane', 'Louis Armstrong', 'Ella Fitzgerald', 'Duke Ellington'],
            'Classical': ['Beethoven', 'Johann Sebastian Bach', 'Mozart', 'John Williams', 'Joshua Bell'],
            'Hip-Hop': ['Kendrick Lamar', 'Drake', 'J. Cole', 'Travis Scott', 'Nicki Minaj'],
            'Country': ['Johnny Cash', 'Dolly Parton', 'Luke Bryan', 'Carrie Underwood', 'Morgan Wallen']
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

