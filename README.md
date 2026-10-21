
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Meu Despertador IA</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: #101827;
      color: white;
      padding: 35px 15px;
    }
    .painel {
      max-width: 350px;
      margin: auto;
      padding: 25px;
      border-radius: 20px;
      background: #1e293b;
    }
    h1 { color: #7dd3fc; }
    input, button {
      font-size: 18px;
      padding: 12px;
      margin: 8px 0;
      border-radius: 10px;
      border: 0;
    }
    button {
      width: 100%;
      background: #38bdf8;
      color: #082f49;
    }
    #status { color: #a7f3d0; }
  </style>
</head>
<body>
  <div class="painel">
    <h1>⏰ Despertador IA</h1>
    <h2 id="relogio">07:00</h2>

    <label for="hora">Horário do alarme</label><br>
    <input id="hora" type="time" value="07:00">

    <button onclick="ativar()">Ativar alarme</button>
    <button onclick="testar()">Testar voz da IA</button>

    <p id="status">Alarme ainda não ativado.</p>
  </div>

  <script>
    let temporizador;
    let horarioAtivo = "";

    function falar(mensagem) {
      if (!("speechSynthesis" in window)) {
        alert(mensagem);
        return;
      }

      speechSynthesis.cancel();
      const voz = new SpeechSynthesisUtterance(mensagem);
      voz.lang = "pt-BR";
      voz.rate = 0.9;
      speechSynthesis.speak(voz);
    }

    function testar() {
      falar("Bom dia! Está na hora de levantar.
      Hoje é mais uma oportunidade para aprender e
      conquistar seus objetivos. Vamos começar!");
    }

    function ativar() {
      clearInterval(temporizador);
      horarioAtivo = document.getElementById("hora").value;

      if (!horarioAtivo) {
        alert("Escolha um horário.");
        return;
      }

      document.getElementById("status").textContent =
        "Alarme programado para " + horarioAtivo;

      temporizador = setInterval(() => {
        const agora = new Date();
        const horaAtual =
          String(agora.getHours()).padStart(2, "0") + ":" +
          String(agora.getMinutes()).padStart(2, "0");

        document.getElementById("relogio").textContent =
          horaAtual;

        if (horaAtual === horarioAtivo &&
            agora.getSeconds() < 2) {
          clearInterval(temporizador);
          document.getElementById("status").textContent =
            "Bom dia! Seu alarme tocou!";
          falar("Bom dia! São sete horas. Levante e
          comece seu dia com energia!");
        }
      }, 1000);
    }
  </script>
</body>
</html>
# despertador