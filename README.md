# Your-Life-Matters-2.0
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>YOUR LIFE MATTERS</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .game-container {
      background: rgba(255,255,255,0.95);
      width: 92%;
      max-width: 650px;
      border-radius: 16px;
      padding: 25px 30px;
      box-shadow: 0 15px 30px rgba(0,0,0,0.3);
      animation: fadeIn 0.6s ease-in-out;
    }
    @keyframes fadeIn {
      from {opacity: 0; transform: translateY(10px);} 
      to {opacity: 1; transform: translateY(0);} 
    }
    h1 {
      text-align: center;
      color: #721e1e;
      margin-bottom: 10px;
    }
    .subtitle {
      text-align: center;
      font-size: 0.95rem;
      color: #555;
      margin-bottom: 20px;
    }
    .question {
      font-size: 1.1rem;
      margin-bottom: 15px;
    }
    .options button {
      width: 100%;
      margin: 7px 0;
      padding: 11px;
      font-size: 1rem;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background: #e3eaf5;
      transition: 0.3s;
    }
    .options button:hover {
      background: #c5d6f2;
    }
    .feedback {
      margin-top: 15px;
      padding: 12px;
      border-radius: 10px;
      display: none;
      font-size: 0.95rem;
    }
    .correct {
      background: #d4edda;
      color: #155724;
    }
    .wrong {
      background: #f8d7da;
      color: #721c24;
    }
    .next-btn {
      margin-top: 15px;
      width: 100%;
      padding: 11px;
      font-size: 1rem;
      border: none;
      border-radius: 10px;
      background: #1e3c72;
      color: #fff;
      cursor: pointer;
      display: none;
    }
    .next-btn:hover {
      background: #16315c;
    }
    .progress {
      text-align: center;
      margin-top: 12px;
      font-size: 0.85rem;
      color: #444;
    }
    .score {
      text-align: center;
      font-size: 0.95rem;
      font-weight: bold;
      margin-top: 5px;
      color: #1e3c72;
    }
    .footer {
      text-align: center;
      font-size: 0.75rem;
      margin-top: 20px;
      color: #777;
    }
  </style>
</head>
<body>

<div class="game-container">
  <h1>YOUR LIFE MATTERS</h1>
  <div class="subtitle">Um jogo de escolhas para refletir sobre sua saúde e bem-estar</div>

  <div class="question" id="question"></div>
  <div class="options" id="options"></div>
  <div class="feedback" id="feedback"></div>

  <button class="next-btn" id="nextBtn">Próxima fase ➡️</button>

  <div class="score" id="score"></div>
  <div class="progress" id="progress"></div>

  <div class="footer">Projeto educativo • Saúde, autocuidado e qualidade de vida</div>
</div>

<script>
  const quiz = [
    { q: "1️⃣ Ao acordar, qual hábito contribui mais para um dia saudável?", options: ["Checar redes sociais imediatamente","Beber um copo de água","Pular o café da manhã","Voltar a dormir várias vezes"], answer: 1, feedback: "Beber água ao acordar hidrata o corpo e ativa o metabolismo." },
    { q: "2️⃣ Qual é a melhor opção para um café da manhã equilibrado?", options: ["Apenas café preto","Biscoitos recheados","Frutas, uma fonte de proteína e carboidrato","Somente alimentos industrializados"], answer: 2, feedback: "Um café equilibrado fornece energia e melhora a concentração." },
    { q: "3️⃣ Quanto tempo diário de atividade física moderada é recomendado, em média, para adultos?", options: ["10 minutos","30 minutos","2 horas","Não é necessário praticar exercícios"], answer: 1, feedback: "30 minutos diários já trazem grandes benefícios à saúde." },
    { q: "4️⃣ Durante o dia de estudos ou trabalho, o que ajuda a manter o foco e a saúde?", options: ["Ficar horas sem levantar","Pular refeições","Fazer pausas curtas para alongar","Beber apenas refrigerante"], answer: 2, feedback: "Pausas melhoram a circulação, postura e atenção." },
    { q: "5️⃣ Qual atitude ajuda a reduzir o estresse no dia a dia?", options: ["Dormir menos para produzir mais","Respirar fundo e organizar tarefas","Ignorar sentimentos","Usar o celular antes de dormir"], answer: 1, feedback: "Organização e respiração ajudam no controle emocional." },
    { q: "6️⃣ Sobre o consumo de água ao longo do dia, o ideal é:", options: ["Beber só quando sentir muita sede","Substituir água por sucos artificiais","Beber água regularmente ao longo do dia","Evitar água à noite"], answer: 2, feedback: "A hidratação constante mantém o corpo funcionando bem." },
    { q: "7️⃣ Qual hábito noturno contribui para uma boa qualidade do sono?", options: ["Usar telas até pegar no sono","Dormir em horários irregulares","Manter um horário regular para dormir","Consumir cafeína à noite"], answer: 2, feedback: "Horários regulares ajudam o relógio biológico." },
    { q: "8️⃣ Na alimentação diária, qual escolha é mais saudável?", options: ["Priorizar alimentos naturais","Consumir fast food todos os dias","Evitar frutas e verduras","Comer apenas uma vez ao dia"], answer: 0, feedback: "Alimentos naturais fornecem nutrientes essenciais." },
    { q: "9️⃣ Qual comportamento ajuda a cuidar da saúde mental?", options: ["Guardar todos os problemas para si","Conversar sobre sentimentos com alguém de confiança","Evitar qualquer descanso","Comparar-se sempre com outras pessoas"], answer: 1, feedback: "Compartilhar sentimentos ajuda a aliviar tensões." },
    { q: "🔟 Qual atitude demonstra responsabilidade com a própria saúde a longo prazo?", options: ["Ignorar sinais do corpo","Fazer exames e check-ups quando indicado","Automedicar-se sempre","Procurar ajuda só em casos graves"], answer: 1, feedback: "A prevenção é essencial para uma vida longa e saudável." }
  ];

  let current = 0;
  let score = 0;

  const questionEl = document.getElementById('question');
  const optionsEl = document.getElementById('options');
  const feedbackEl = document.getElementById('feedback');
  const nextBtn = document.getElementById('nextBtn');
  const progressEl = document.getElementById('progress');
  const scoreEl = document.getElementById('score');

  function loadQuestion() {
    feedbackEl.style.display = 'none';
    nextBtn.style.display = 'none';
    questionEl.textContent = quiz[current].q;
    optionsEl.innerHTML = '';
    progressEl.textContent = `Fase ${current + 1} de ${quiz.length}`;
    scoreEl.textContent = `Pontuação: ${score} / ${quiz.length}`;

    quiz[current].options.forEach((opt, i) => {
      const btn = document.createElement('button');
      btn.textContent = opt;
      btn.onclick = () => checkAnswer(i);
      optionsEl.appendChild(btn);
    });
  }

  function checkAnswer(i) {
    feedbackEl.style.display = 'block';
    if (i === quiz[current].answer) {
      score++;
      feedbackEl.className = 'feedback correct';
      feedbackEl.textContent = '✅ Escolha saudável! ' + quiz[current].feedback;
    } else {
      feedbackEl.className = 'feedback wrong';
      feedbackEl.textContent = '❌ Reflita sobre isso. ' + quiz[current].feedback;
    }
    scoreEl.textContent = `Pontuação: ${score} / ${quiz.length}`;
    nextBtn.style.display = 'block';
  }

  nextBtn.onclick = () => {
    current++;
    if (current < quiz.length) {
      loadQuestion();
    } else {
      questionEl.textContent = '🎉 Jogo concluído!';
      optionsEl.innerHTML = '';
      feedbackEl.style.display = 'block';
      feedbackEl.className = 'feedback correct';
      feedbackEl.textContent = `Você fez ${score} de ${quiz.length} pontos. Pequenas escolhas constroem uma vida mais saudável.`;
      nextBtn.style.display = 'none';
      progressEl.textContent = 'Obrigado por jogar!';
    }
  };

  loadQuestion();
</script>

</body>
</html>
