---
toc: true
comments: false
layout: post
title: Titanic ML test
courses: { compsci: {week: 26} }
type: hacks
---


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titanic Survival Prediction</title>
<style>
    body {
        font-family: 'Courier New', monospace;
        background-color: #0D1F38; /* Naval Blue background */
        color: #FFFFFF; /* White text */
        margin: 0;
        padding: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
    }

    .container {
        background-color: #1E416E; /* Deeper blue container */
        padding: 20px;
        border-radius: 16px; /* Slightly rounded corners */
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3); /* Enhanced shadow */
        width: 80%; /* Wider container */
        max-width: 800px; /* Max width increased */
        text-align: center; /* Center the content */
    }

    h2 {
        color: #6BB7E3; /* Lighter blue for headings */
    }

    form {
        display: flex;
        flex-direction: column;
    }

    label {
        margin-top: 10px;
        color: #BDD7EE; /* Lighter blue for labels */
    }

    input,
    select,
    button {
        padding: 12px; /* Slightly larger padding */
        margin-top: 8px; /* Slightly increased spacing */
        border: 2px solid #2E5A8E; /* Darker blue border */
        border-radius: 8px; /* Slightly rounded corners */
        background-color: #0F304E; /* Deeper blue background */
        color: #FFFFFF; /* White text */
    }

    input::placeholder {
        color: #8AB6D6; /* Lighter blue for placeholder text */
        opacity: 1;
    }

    button {
        background-color: #4FA2C4; /* Lighter blue button */
        color: #FFFFFF; /* White text */
        margin-top: 20px;
        transition: background-color 0.3s;
    }

    button:hover {
        background-color: #85C0D9; /* Lighter blue hover */
    }

    #predictionResult {
        margin-top: 20px;
        text-align: center;
        padding: 12px; /* Slightly larger padding */
        background-color: #2E5A8E; /* Darker blue result background */
        color: #FFFFFF; /* White text */
        border-radius: 8px; /* Slightly rounded corners */
        display: none;
    }

    .images {
        display: flex;
        justify-content: space-around;
        margin-top: 20px;
    }

    iframe {
        width: 45%;
        height: auto;
    }
</style>
</head>
<body>
    <div class="container">
        <h2>Titanic Survival Predictor</h2>
        <form id="predictionForm">
            <!-- Your form inputs here -->
            <label for="pclass">Class:</label>
            <select id="pclass">
                <option value="1">1st Class</option>
                <option value="2" selected>2nd Class</option>
                <option value="3">3rd Class</option>
            </select>

            <label for="sex">Sex:</label>
            <select id="sex">
                <option value="1">Male</option>
                <option value="0">Female</option>
            </select>

            <label for="age">Age:</label>
            <input type="number" id="age" placeholder="Age" required>

            <label for="sibsp">Siblings/Spouses Aboard:</label>
            <input type="number" id="sibsp" placeholder="Siblings/Spouses" required>

            <label for="parch">Parents/Children Aboard:</label>
            <input type="number" id="parch" placeholder="Parents/Children" required>

            <label for="fare">Fare:</label>
            <input type="number" step="0.01" id="fare" placeholder="Ticket Fare" required>

            <label for="alone">Traveling Alone:</label>
            <input type="checkbox" id="alone">

            <label for="embarked">Embarked From:</label>
            <select id="embarked">
                <option value="S">Southampton</option>
                <option value="C">Cherbourg</option>
                <option value="Q">Queenstown</option>
            </select>

            <button type="submit">Predict Survival</button>
        </form>
        <div id="predictionResult"></div>
    </div>

    <script>
        document.getElementById('predictionForm').onsubmit = async function(e) {
            e.preventDefault();

            const formData = {
                pclass: document.getElementById('pclass').value,
                sex: document.getElementById('sex').value,
                age: document.getElementById('age').value,
                sibsp: document.getElementById('sibsp').value,
                parch: document.getElementById('parch').value,
                fare: document.getElementById('fare').value,
                alone: document.getElementById('alone').checked ? 1 : 0,
                embarked: document.getElementById('embarked').value
            };

            const response = await fetch('http://127.0.0.1:8086/api/titanic/predict', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(formData),
            });

            const result = await response.json();
            const resultDiv = document.getElementById('predictionResult');
            resultDiv.style.display = 'block';
            resultDiv.innerText = `Survival Probability: ${result['LogisticRegression Survival Probability']}%`;
        };
    </script>
</body>
</html>
