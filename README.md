<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quel personnage de Spider-Man êtes-vous ?</title>
  <style>
    body {
      font-family: 'Courier New', monospace;
      background-color: #001f3f;
      color: #fff;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    header img {
      max-width: 150px;
      margin-bottom: 20px;
    }

    h1 {
      color: #ffcc00;
      text-shadow: 2px 2px 5px #ff5733;
      font-size: 36px;
      margin-bottom: 20px;
    }

    .question {
      margin: 20px auto;
      background-color: #3399ff;
      color: #fff;
      padding: 20px;
      border-radius: 10px;
      max-width: 600px;
      box-shadow: 0 0 10px rgba(255, 204, 0, 0.7);
    }

    .options button {
      background-color: #005f99;
      color: #fff;
      padding: 10px 20px;
      border: none;
      margin: 5px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
      box-shadow: 0 0 5px #003366;
      transition: all 0.3s ease;
    }

    .options button:hover {
      background-color: #003366;
      transform: scale(1.05);
    }

    .result {
      background-color: #3399ff;
      color: #000;
      padding: 20px;
      border-radius: 10px;
      margin-top: 20px;
      display: none;
      box-shadow: 0 0 10px rgba(255, 204, 0, 0.7);
    }

    .score {
      font-size: 16px;
      margin-top: 10px;
      color: #001f3f;
    }
  </style>
</head>
<body>
  <header>
    <img src="https://zupimages.net/up/25/15/z1p4.png" alt="Logo Spider-Man">
  </header>

  <h1>Quel personnage de Spider-Man êtes-vous ?</h1>

  <div id="quiz-container">
    <div class="question" id="question-container">
      <h2 id="question-text"></h2>
      <div class="options" id="options-container"></div>
    </div>
  </div>

  <div class="result" id="result-container">
    <h2>Vous êtes...</h2>
    <p id="result-text"></p>
    <p class="score">Votre résultat est basé sur vos choix !</p>
    <button onclick="startQuiz()">Recommencer le quiz</button>
  </div>

  <script>
    const questions = [
      { question: "Quel est votre plus grand atout ?", options: ["Force brute", "Intelligence", "Vitesse", "Invisibilité"], result: [2, 0, 1, 3] },
      { question: "Quel est votre mobile ?", options: ["Vengeance", "Justice", "La découverte", "Vol"], result: [2, 0, 1, 4] },
      { question: "Quel environnement préférez-vous ?", options: ["Les toits", "Les égouts", "Un laboratoire", "Un studio télé"], result: [0, 4, 1, 5] },
      { question: "Quel est votre style de combat ?", options: ["Puissant", "Rapide", "Tactique", "Insaisissable"], result: [2, 1, 0, 3] },
      { question: "Quelle est votre faiblesse ?", options: ["Colère", "Responsabilité", "Solitude", "Passé trouble"], result: [2, 0, 3, 4] },
      { question: "Quelle est votre obsession ?", options: ["Faire le bien", "Prouver sa valeur", "Contrôler", "Se venger d'humiliations"], result: [0, 1, 3, 6] },
      { question: "Votre transformation vient de :", options: ["Expérience scientifique", "Symbiote", "Mutation", "Accident"], result: [1, 3, 0, 2] },
      { question: "Quelle créature vous inspire ?", options: ["Araignée", "Caméléon", "Faucon", "Poulpe"], result: [0, 3, 4, 6] },
      { question: "Comment décririez-vous votre esprit ?", options: ["Logique", "Créatif", "Impulsif", "Stratégique"], result: [0, 1, 2, 3] },
      { question: "Votre point faible ?", options: ["Vos proches", "Votre fierté", "Vos souvenirs", "Aucun !"], result: [0, 1, 2, 4] }
    ];

    const characters = [
      { name: "Spider-Man (Peter Parker)", description: "Héros responsable et rusé, guidé par sa conscience et son devoir.", image: "https://upload.wikimedia.org/wikipedia/en/0/0c/Spiderman50.jpg" },
      { name: "Miles Morales", description: "Jeune, audacieux et puissant, vous apportez une nouvelle vision au rôle de Spider-Man.", image: "https://upload.wikimedia.org/wikipedia/en/d/dc/Miles_Morales_Spider-Man.png" },
      { name: "L'homme-sable", description: "Imposant et résilient, vous êtes imprévisible comme les grains de sable.", image: "https://upload.wikimedia.org/wikipedia/en/6/65/Sandman88.png" },
      { name: "Venom", description: "Sombre et redoutable, votre force vient de votre fusion unique.", image: "https://upload.wikimedia.org/wikipedia/en/5/5e/Venom_first_appearance.png" },
      { name: "Le Caïd (Kingpin)", description: "Maître du crime, vous dominez avec autorité et stratégie.", image: "https://upload.wikimedia.org/wikipedia/en/1/10/Kingpinart.jpg" },
      { name: "Mysterio", description: "Illusionniste rusé, vous manipulez la réalité à votre avantage.", image: "https://upload.wikimedia.org/wikipedia/en/e/e3/Mysterio_Quentin_Beck.png" },
      { name: "Docteur Octopus", description: "Scientifique brillant, parfois mal compris, souvent redouté.", image: "https://upload.wikimedia.org/wikipedia/en/f/fd/Doctor_Octopus_%28Marvel_Comics_character%29.png" },
      { name: "Green Goblin", description: "Instable, dangereux et imprévisible, la peur est votre arme.", image: "https://upload.wikimedia.org/wikipedia/en/0/01/Green_Goblin.jpg" },
      { name: "Black Cat", description: "Agile et insaisissable, vous jouez selon vos propres règles.", image: "https://upload.wikimedia.org/wikipedia/en/1/1d/Black_Cat_%28Felicia_Hardy%29.png" },
      { name: "Gwen Stacy / Spider-Woman", description: "Forte, brillante et loyale, vous alliez force et compassion.", image: "https://upload.wikimedia.org/wikipedia/en/5/5d/Spider-Gwen_%28Gwen_Stacy%29.png" }
    ];

    let currentQuestion = 0;
    let userAnswers = [];

    function startQuiz() {
      currentQuestion = 0;
      userAnswers = [];
      document.getElementById('result-container').style.display = 'none';
      document.getElementById('quiz-container').style.display = 'block';
      showQuestion();
    }

    function showQuestion() {
      const question = questions[currentQuestion];
      document.getElementById('question-text').innerText = question.question;
      const optionsContainer = document.getElementById('options-container');
      optionsContainer.innerHTML = '';

      question.options.forEach((option, index) => {
        const button = document.createElement('button');
        button.innerText = option;
        button.onclick = () => {
          userAnswers.push(question.result[index]);
          currentQuestion++;
          if (currentQuestion < questions.length) {
            showQuestion();
          } else {
            showResult();
          }
        };
        optionsContainer.appendChild(button);
      });
    }

    function showResult() {
      const counts = Array(characters.length).fill(0);
      userAnswers.forEach(index => counts[index]++);
      const maxIndex = counts.indexOf(Math.max(...counts));
      const result = characters[maxIndex];

      document.getElementById('result-text').innerHTML = `
        <strong>${result.name}</strong> - ${result.description}<br><br>
        <img src="${result.image}" alt="${result.name}" style="max-width: 200px; border-radius: 10px; box-shadow: 0 0 10px #000;">
      `;
      document.getElementById('quiz-container').style.display = 'none';
      document.getElementById('result-container').style.display = 'block';
    }

    startQuiz();
  </script>
</body>
</html>
