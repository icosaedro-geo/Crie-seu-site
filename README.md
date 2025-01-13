<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Biblioteca de Sites - Criar e Gerenciar</title>
    <style>
        /* Paleta de cores */
        :root {
            --primary-color: #f2eabc;
            --secondary-color: #54736e;
            --accent-color: #ff3b58;
            --dark-color: #194756;
            --background-color: #080000;
        }

        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background-color: var(--background-color);
            color: #fff;
        }
        h1 {
            text-align: center;
            color: var(--primary-color);
        }
        h2 {
            color: var(--primary-color);
        }
        .view {
            display: none;
        }
        .visible {
            display: block;
        }
        button {
            padding: 10px 15px;
            margin: 5px;
            cursor: pointer;
            font-size: 14px;
            background-color: var(--secondary-color);
            color: #fff;
            border: 2px solid var(--secondary-color);
            border-radius: 5px;
        }
        button:hover {
            background-color: var(--accent-color);
            border-color: var(--accent-color);
        }
        textarea {
            width: 100%;
            height: 200px;
            margin: 10px 0;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            background-color: #333;
            color: #fff;
            border: 1px solid var(--secondary-color);
            border-radius: 5px;
            white-space: pre-wrap;
            word-wrap: break-word;
        }
        #site-list {
            list-style: none;
            padding-left: 0;
        }
        #site-list li {
            margin-bottom: 10px;
            background-color: var(--secondary-color);
            padding: 10px;
            border-radius: 5px;
        }
        #site-list li span {
            color: #fff;
        }
        .delete-btn, .edit-btn {
            background-color: var(--accent-color);
            border: none;
            color: #fff;
            padding: 5px 10px;
            margin-left: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        .delete-btn:hover, .edit-btn:hover {
            background-color: var(--dark-color);
        }
        /* Estilos para o botão de apoiador */
        #support-button {
            width: 50px;
            height: 50px;
            background-color: var(--accent-color);
            border: none;
            border-radius: 50%;
            font-size: 24px;
            color: white;
            text-align: center;
            line-height: 50px;
            cursor: pointer;
            position: fixed;
            bottom: 20px;
            right: 20px;
        }
        /* Estilos para a mensagem de apoio */
        #support-message {
            display: none;
            position: fixed;
            top: 20%;
            left: 50%;
            transform: translateX(-50%);
            background-color: var(--secondary-color);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            text-align: center;
            width: 300px;
            font-size: 16px;
        }
        #support-message button {
            margin-top: 10px;
            background-color: var(--accent-color);
            border: none;
            color: #fff;
            padding: 5px 15px;
            cursor: pointer;
            border-radius: 5px;
        }
        #support-message button:hover {
            background-color: var(--dark-color);
        }
    </style>
</head>
<body>

    <!-- Tela Inicial -->
    <div id="library-view" class="view visible">
        <header>
            <h1>Biblioteca de Sites</h1>
            <button id="create-new-site">Criar Novo Site</button>
        </header>
        <section>
            <h2>Sites Criados</h2>
            <ul id="site-list">
                <!-- Lista de Sites Salvos -->
            </ul>
        </section>
    </div>

    <!-- Tela de Criação de Novo Site -->
    <div id="create-site-view" class="view">
        <header>
            <h1>Criar Novo Site</h1>
            <button id="back-to-library">Voltar para Biblioteca</button>
        </header>
        <section>
            <label for="project-name">Nome do Projeto</label>
            <input type="text" id="project-name" placeholder="Digite o nome do seu projeto">
        </section>
        <section>
            <h2 id="project-title"></h2>
            <textarea id="html-code" placeholder="Escreva seu código HTML aqui..."></textarea>
        </section>
        <section>
            <button id="save-project">Salvar Projeto</button>
            <button id="test-project">Testar Projeto</button>
            <button id="download-project">Fazer Download</button>
            <button id="reset-script">Resetar Script</button>
        </section>
    </div>

    <!-- Visualização do Projeto (Testar) -->
    <div id="test-site-view" class="view">
        <header>
            <h1>Visualizar o Site</h1>
            <button id="back-to-editor">Voltar ao Editor</button>
        </header>
        <iframe id="site-preview" width="100%" height="600px"></iframe>
    </div>

    <!-- Caixa de Mensagem do Apoiador -->
    <div id="support-message">
        <p>Você poderia apoiar o criador do site? Obrigado!</p>
        <p><strong>Chave Pix: 513197db-499f-4fcc-9648-d481567340ba</strong></p>
        <button id="close-support-message">Voltar</button>
    </div>

    <!-- Botão de Apoiador -->
    <button id="support-button">◇</button>

    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const createNewSiteButton = document.getElementById('create-new-site');
            const backToLibraryButton = document.getElementById('back-to-library');
            const saveProjectButton = document.getElementById('save-project');
            const testProjectButton = document.getElementById('test-project');
            const downloadProjectButton = document.getElementById('download-project');
            const resetScriptButton = document.getElementById('reset-script');
            const backToEditorButton = document.getElementById('back-to-editor');
            const siteList = document.getElementById('site-list');
            const projectNameInput = document.getElementById('project-name');
            const htmlCodeTextarea = document.getElementById('html-code');
            const projectTitle = document.getElementById('project-title');
            const sitePreview = document.getElementById('site-preview');
            const supportButton = document.getElementById('support-button');
            const supportMessage = document.getElementById('support-message');
            const closeSupportMessageButton = document.getElementById('close-support-message');

            let savedSites = [];
            let currentSite = null;

            // Funções de Tela
            function showLibraryView() {
                document.getElementById('library-view').classList.add('visible');
                document.getElementById('create-site-view').classList.remove('visible');
                document.getElementById('test-site-view').classList.remove('visible');
                resetCreateSiteView();
            }

            function showCreateSiteView() {
                document.getElementById('create-site-view').classList.add('visible');
                document.getElementById('library-view').classList.remove('visible');
                document.getElementById('test-site-view').classList.remove('visible');
            }

            function showTestSiteView() {
                document.getElementById('test-site-view').classList.add('visible');
                document.getElementById('create-site-view').classList.remove('visible');
                sitePreview.srcdoc = htmlCodeTextarea.value; // Carregar o código HTML no iframe
            }

            // Limpar os campos ao criar novo site
            function resetCreateSiteView() {
                projectNameInput.value = '';
                htmlCodeTextarea.value = '';
                projectTitle.textContent = '';
                currentSite = null;
            }

            // Renderizar a lista de sites
            function renderSiteList() {
                siteList.innerHTML = '';
                savedSites.forEach((site, index) => {
                    const li = document.createElement('li');
                    li.innerHTML = `
                        <span>${site.name}</span>
                        <button class="delete-btn" data-index="${index}">Deletar</button>
                        <button class="edit-btn" data-index="${index}">Editar</button>
                    `;
                    siteList.appendChild(li);
                });

                // Adicionar event listeners para os botões de editar e deletar
                document.querySelectorAll('.delete-btn').forEach(button => {
                    button.addEventListener('click', (event) => {
                        const index = event.target.getAttribute('data-index');
                        deleteSite(index);
                    });
                });

                document.querySelectorAll('.edit-btn').forEach(button => {
                    button.addEventListener('click', (event) => {
                        const index = event.target.getAttribute('data-index');
                        editSite(index);
                    });
                });
            }

            // Deletar site
            function deleteSite(index) {
                savedSites.splice(index, 1);
                renderSiteList();
                resetCreateSiteView();  // Limpar as informações do site deletado
            }

            // Editar site
            function editSite(index) {
                const site = savedSites[index];
                projectNameInput.value = site.name;
                htmlCodeTextarea.value = site.html;
                projectTitle.textContent = site.name;
                currentSite = site;
                showCreateSiteView();
            }

            // Função para salvar o projeto
            function saveProject() {
                const projectName = projectNameInput.value;
                const htmlCode = htmlCodeTextarea.value;
                if (projectName && htmlCode) {
                    if (currentSite) {
                        // Se estamos editando um site existente
                        currentSite.name = projectName;
                        currentSite.html = htmlCode;
                    } else {
                        // Caso seja um novo site
                        const newProject = { name: projectName, html: htmlCode };
                        savedSites.push(newProject);
                    }
                    renderSiteList();
                    showLibraryView();
                }
            }

            // Resetar o script (limpar os campos)
            function resetScript() {
                resetCreateSiteView(); // Limpar campos
                // Manter na tela de criação de site sem voltar para a biblioteca
            }

            // Função para salvar o site como arquivo HTML
            function downloadProject() {
                const blob = new Blob([htmlCodeTextarea.value], { type: 'text/html' });
                const link = document.createElement('a');
                link.href = URL.createObjectURL(blob);
                link.download = `${projectNameInput.value}.html`;
                link.click();
            }

            // Eventos de Botões
            createNewSiteButton.addEventListener('click', showCreateSiteView);
            backToLibraryButton.addEventListener('click', showLibraryView);
            saveProjectButton.addEventListener('click', saveProject);
            testProjectButton.addEventListener('click', showTestSiteView);
            downloadProjectButton.addEventListener('click', downloadProject);
            resetScriptButton.addEventListener('click', resetScript);
            backToEditorButton.addEventListener('click', showCreateSiteView);

            // Exibir a mensagem de apoio
            supportButton.addEventListener('click', () => {
                supportMessage.style.display = 'block';
            });

            // Fechar a mensagem de apoio
            closeSupportMessageButton.addEventListener('click', () => {
                supportMessage.style.display = 'none';
            });

            // Inicializa a tela de biblioteca
            showLibraryView();
        });
    </script>
</body>
</html>
