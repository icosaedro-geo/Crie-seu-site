<meta name='viewport' content='width=device-width, initial-scale=1'/><!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Construa Seu Site</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #F7F8FC;
      color: #010300;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
    }

    .container {
      width: 80%;
      max-width: 900px;
      background-color: #BBDEC6;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      text-align: center;
    }

    h1 {
      font-size: 32px;
      color: #314C53;
      margin-bottom: 20px;
    }

    #projectNameInput {
      padding: 10px;
      font-size: 16px;
      width: 100%;
      max-width: 400px;
      margin-bottom: 20px;
      border-radius: 5px;
      border: 2px solid #5A7F78;
    }

    .explode {
      animation: explode 0.8s forwards;
    }

    .zoomIn {
      animation: zoomIn 1s ease-in-out forwards;
    }

    @keyframes explode {
      0% {
        transform: scale(1);
        opacity: 1;
      }
      25% {
        transform: scale(1.2);
        opacity: 0.7;
      }
      50% {
        transform: scale(0.8);
        opacity: 0.5;
      }
      75% {
        transform: scale(1.5);
        opacity: 0.3;
      }
      100% {
        transform: scale(0);
        opacity: 0;
      }
    }

    @keyframes zoomIn {
      0% {
        transform: scale(0);
        opacity: 0;
      }
      100% {
        transform: scale(1);
        opacity: 1;
      }
    }

    #htmlEditor {
      width: 100%;
      height: 300px;
      padding: 10px;
      margin: 10px 0;
      background-color: #F7F8FC;
      color: #010300;
      border: 2px solid #5A7F78;
      border-radius: 5px;
      font-size: 16px;
      font-family: 'Courier New', Courier, monospace;
      resize: none;
      white-space: pre-wrap;
      overflow-y: auto;
      display: none;
    }

    button {
      padding: 10px 15px;
      font-size: 16px;
      color: #fff;
      background-color: #314C53;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      margin: 10px;
      transition: 0.3s;
    }

    button:hover {
      background-color: #5A7F78;
    }

    button:focus {
      outline: none;
    }

    .project-title {
      font-size: 24px;
      color: #314C53;
      margin-top: 20px;
    }

    /* Estilo do botão de apoio */
    .support-button {
      position: fixed;
      top: 20px;
      right: 20px;
      background-color: #5A7F78;
      color: white;
      padding: 15px;
      font-size: 30px;
      border-radius: 50%;
      cursor: pointer;
      box-shadow: 0 0 10px rgba(90, 127, 120, 0.6);
      transition: 0.3s;
      z-index: 9999;
      animation: pulsate 2s infinite;
    }

    .support-button:hover {
      background-color: #314C53;
    }

    /* Animação de pulsação do botão */
    @keyframes pulsate {
      0% {
        transform: scale(1);
        box-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
      }
      50% {
        transform: scale(1.1);
        box-shadow: 0 0 20px rgba(0, 255, 0, 1);
      }
      100% {
        transform: scale(1);
        box-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
      }
    }

    /* Estilo para o botão de testar site */
    #testButton {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background-color: #BBDEC6;
      color: #010300;
      padding: 12px 20px;
      font-size: 16px;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      transition: 0.3s;
    }

    #testButton:hover {
      background-color: #5A7F78;
    }

    /* Estilo para a mensagem de apoio */
    #supportMessage {
      display: none;
      position: fixed;
      top: 20%;
      left: 50%;
      transform: translateX(-50%);
      background-color: #BBDEC6;
      color: #010300;
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
      text-align: center;
      z-index: 100;
      width: 70%;
      max-width: 400px;
    }

    #supportMessage button {
      margin-top: 15px;
      background-color: #314C53;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 5px;
      cursor: pointer;
      transition: 0.3s;
    }

    #supportMessage button:hover {
      background-color: #5A7F78;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Construa Seu Site</h1>

    <!-- Formulário para nomear o projeto -->
    <input type="text" id="projectNameInput" placeholder="Dê um nome ao seu projeto" maxlength="16" />
    <button onclick="startProject()">Iniciar Projeto</button>

    <!-- Título do Projeto -->
    <div id="projectTitle" class="project-title"></div>

    <!-- Editor de código (inicialmente escondido) -->
    <textarea id="htmlEditor" placeholder="Digite seu código HTML aqui..."></textarea>

    <button onclick="saveProject()">Salvar Projeto</button>
  </div>

  <!-- Botão de Apoiadores -->
  <div id="supportButton" class="support-button">
    ♡
  </div>

  <!-- Botão de Testar Site -->
  <button id="testButton" onclick="testProject()">Testar Site</button>

  <!-- Mensagem de apoio -->
  <div id="supportMessage">
    <p>Você poderia apoiar o criador do site com qualquer valor! Obrigado!</p>
    <p><strong>Chave Pix:</strong> 513197db-499f-4fcc-9648-d481567340ba</p>
    <button onclick="closeSupportMessage()">Fechar</button>
  </div>

  <script>
    // Função para iniciar o projeto
    function startProject() {
      const projectName = document.getElementById('projectNameInput').value;
      if (projectName) {
        if (projectName.length > 16) {
          alert("O nome do projeto não pode ter mais de 16 caracteres!");
          return;
        }

        // Título do projeto
        document.getElementById('projectTitle').innerText = `Projeto: ${projectName}`;

        // Animação de explosão do campo de nome
        document.getElementById('projectNameInput').classList.add('explode');
        document.querySelector('button').classList.add('explode');

        // Esperar a animação terminar para esconder o campo de nome
        setTimeout(function() {
          document.getElementById('projectNameInput').style.display = 'none';
          document.querySelector('button').style.display = 'none';

          // Animação de zoom-in do editor de código
          const editor = document.getElementById('htmlEditor');
          editor.style.display = 'block';
          editor.classList.add('zoomIn');
        }, 800);
      } else {
        alert("Por favor, insira um nome para o projeto!");
      }
    }

    // Função para salvar o projeto
    function saveProject() {
      const htmlContent = document.getElementById('htmlEditor').value;
      const projectName = document.getElementById('projectNameInput').value;

      if (projectName && htmlContent) {
        const blob = new Blob([htmlContent], { type: "text/html" });
        const link = document.createElement("a");
        link.href = URL.createObjectURL(blob);
        link.download = `${projectName}.html`;
        link.click();
        alert("Projeto salvo com sucesso!");
      } else {
        alert("Por favor, nomeie o projeto e escreva algo no código!");
      }
    }

    // Função para testar o site
    function testProject() {
      const htmlContent = document.getElementById('htmlEditor').value;

      if (htmlContent) {
        const newWindow = window.open();
        newWindow.document.open();
        newWindow.document.write(`
          <html>
            <head>
              <title>Projeto Testado</title>
              <style>
                body { font-family: 'Courier New', Courier, monospace; background-color: #f5f5f5; color: #333; }
              </style>
            </head>
            <body>
              ${htmlContent}
            </body>
          </html>
        `);
        newWindow.document.close();
      } else {
        alert("Por favor, escreva algum código HTML antes de testar!");
      }
    }

    // Função para exibir a mensagem de apoio
    document.getElementById('supportButton').onclick = function() {
      document.getElementById('supportMessage').style.display = 'block';
    };

    // Função para fechar a mensagem de apoio
    function closeSupportMessage() {
      document.getElementById('supportMessage').style.display = 'none';
    }
    

    // Exibir a mensagem de apoio a cada 20 minutos (1200000 milissegundos)
    setInterval(function() {
      document.getElementById('supportMessage').style.display = 'block';
    }, 1200000);
  </script>

</body>
</html>
