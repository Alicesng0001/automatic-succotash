<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Analise Emocional - AI Resposta</title>
<style>
  :root {
    --bg-light: #ffcefa;
    --text-light: #222;
    --bg-dark: #1e1e2f;
    --text-dark: #ddd;
    --primary: #fc4c90;
    --secondary: #ff5c9b;
    --border-radius: 8px;
  }

  body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: var(--bg-light);
    color: var(--text-light);
    transition: background-color 0.3s, color 0.3s;
  }
  body.dark {
    background-color: var(--bg-dark);
    color: var(--text-dark);
  }

  header {
    padding: 1rem;
    background-color: var(--primary);
    color: white;
    text-align: center;
  }

  main {
    max-width: 600px;
    margin: 2rem auto;
    padding: 1rem;
    background: white;
    border-radius: var(--border-radius);
    box-shadow: 0 0 15px rgba(0,0,0,0.1);
    transition: background 0.3s, color 0.3s;
  }
  body.dark main {
    background: #2c2c44;
  }

  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 600;
  }

  textarea, select, input[type="text"] {
    width: 100%;
    padding: 0.5rem;
    margin-bottom: 1rem;
    border: 2px solid var(--primary);
    border-radius: var(--border-radius);
    font-size: 1rem;
    resize: vertical;
    background: inherit;
    color: inherit;
    transition: background-color 0.3s, color 0.3s, border-color 0.3s;
  }

  button {
    background-color: var(--secondary);
    border: none;
    color: white;
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
    border-radius: var(--border-radius);
    cursor: pointer;
    transition: background-color 0.3s;
  }
  button:hover {
    background-color: #e07b50;
  }

  #result {
    margin-top: 1rem;
    padding: 1rem;
    border: 2px solid var(--primary);
    border-radius: var(--border-radius);
    background-color: #f0e9ff;
    color: #3a026a;
    display: none;
  }
  body.dark #result {
    background-color: #463973;
    color: #d1c4e9;
    border-color: #9575cd;
  }

  #copyBtn {
    margin-top: 1rem;
    background-color: var(--primary);
  }
  #copyBtn:hover {
    background-color: #5a058c;
  }

  .footer {
    margin-top: 2rem;
    font-size: 0.9rem;
    text-align: center;
    color: var(--primary);
  }
  body.dark .footer {
    color: #d1c4e9;
  }

  /* Toggle switch */
  .toggle-switch {
    position: fixed;
    top: 1rem;
    right: 1rem;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-weight: 600;
    cursor: pointer;
    user-select: none;
  }
  .toggle-switch input {
    width: 40px;
    height: 20px;
    appearance: none;
    background: #ccc;
    border-radius: 20px;
    position: relative;
    outline: none;
    cursor: pointer;
    transition: background 0.3s;
  }
  .toggle-switch input:checked {
    background: var(--primary);
  }
  .toggle-switch input::before {
    content: "";
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    top: 1px;
    left: 1px;
    background: white;
    transition: 0.3s;
  }
  .toggle-switch input:checked::before {
    left: 21px;
  }
</style>
</head>
<body>
<header>
  <h1>🧠 Análise Emocional + Resposta AI</h1>
</header>

<div class="toggle-switch">
  <label for="darkModeToggle">Modo Escuro</label>
  <input type="checkbox" id="darkModeToggle" />
</div>

<main>
  <label for="emailInput">Cole seu e-mail para análise:</label>
  <textarea id="emailInput" rows="6" placeholder="Escreva ou cole seu e-mail aqui..."></textarea>

  <label for="toneSelect">Escolha o estilo da resposta:</label>
  <select id="toneSelect">
    <option value="profissional">Profissional</option>
    <option value="amigável">Amigável</option>
    <option value="formal">Formal</option>
    <option value="criativo">Criativo</option>
    <option value="motivador">Motivador</option>
  </select>

  <button id="analyzeBtn">Analisar e Gerar Resposta</button>

  <div id="result">
    <p><strong>Emoção detectada:</strong> <span id="emotion"></span></p>
    <p><strong>Resposta da IA:</strong></p>
    <p id="response"></p>
    <button id="copyBtn">Copiar resposta</button>
  </div>
</main>

<footer class="footer">
  Feito com dedicação pela Alice. 🌟 <br />
  <a href="https://www.linkedin.com/in/alice-araujo-souza-556378301" target="_blank" rel="noopener noreferrer">Meu LinkedIn</a>
</footer>

<script>
  const apiKey = "AIzaSyB1XccN3q7WBh2SOp-9Df_8aBtj81TwC3c";

  document.getElementById("analyzeBtn").addEventListener("click", async () => {
    const email = document.getElementById("emailInput").value.trim();
    const tone = document.getElementById("toneSelect").value;
    const emotionEl = document.getElementById("emotion");
    const responseEl = document.getElementById("response");
    const resultBox = document.getElementById("result");

    if (!email) {
      alert("Cole um e-mail para analisar.");
      return;
    }

    resultBox.style.display = "block";
    emotionEl.textContent = "Analisando com olhos atentos...";
    responseEl.textContent = "Gerando resposta com sensibilidade...";

    const prompt = `Você é uma IA treinada para interpretar emoções e responder e-mails com base em diferentes estilos. Analise o tom emocional geral do seguinte e-mail e gere uma resposta no estilo '${tone}':\n\n${email}\n\nResposta:`;

    try {
      const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          contents: [{ role: "user", parts: [{ text: prompt }] }]
        })
      });

      const data = await res.json();
      const output = data.candidates?.[0]?.content?.parts?.[0]?.text || "Não foi possível gerar resposta.";

      const partes = output.split("Resposta:");
      emotionEl.textContent = partes[0]?.trim() || "Emoção não detectada.";
      responseEl.textContent = partes[1]?.trim() || "Resposta não encontrada.";

    } catch (err) {
      emotionEl.textContent = "Erro na análise emocional.";
      responseEl.textContent = "Erro na geração de resposta.";
      console.error(err);
    }
  });

  document.getElementById("copyBtn").addEventListener("click", () => {
    const text = document.getElementById("response").textContent;
    navigator.clipboard.writeText(text)
      .then(() => alert("Resposta copiada com sucesso!"))
      .catch(() => alert("Erro ao copiar."));
  });

  // Modo escuro toggle
  const toggle = document.getElementById("darkModeToggle");
  toggle.addEventListener("change", () => {
    document.body.classList.toggle("dark", toggle.checked);
  });

  // Detecta preferencia do sistema e ativa modo escuro automaticamente
  if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    toggle.checked = true;
    document.body.classList.add("dark");
  }
</script>
</body>
</html>
