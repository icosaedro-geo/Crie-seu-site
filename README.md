<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de CPF</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
        }
        .container {
            margin: 20px auto;
            max-width: 400px;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 8px;
            background-color: #f9f9f9;
        }
        button {
            padding: 10px 20px;
            margin: 10px;
            border: none;
            background-color: #4CAF50;
            color: white;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .output {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Gerador de CPF</h1>
        <p>Escolha o tipo de CPF que deseja gerar:</p>
        <button onclick="gerarCPF('maior')">CPF de Maior</button>
        <button onclick="gerarCPF('menor')">CPF de Menor</button>
        <div class="output" id="resultado"></div>
    </div>
    <script>
        function gerarCPF(tipo) {
            const gerarDigito = (base) => {
                let soma = 0;
                for (let i = 0; i < base.length; i++) {
                    soma += parseInt(base[i]) * ((base.length + 1) - i);
                }
                const resto = soma % 11;
                return resto < 2 ? 0 : 11 - resto;
            };

            // Gerar os 9 primeiros dígitos aleatórios
            let baseCPF = Array.from({ length: 9 }, () => Math.floor(Math.random() * 10)).join("");

            // Gerar os dois dígitos verificadores
            const digito1 = gerarDigito(baseCPF);
            const digito2 = gerarDigito(baseCPF + digito1);

            // Montar o CPF completo
            let cpf = baseCPF + digito1 + digito2;

            // Ajustar o CPF de acordo com o tipo solicitado
            if (tipo === 'maior' && digito2 < 5) {
                cpf = cpf.slice(0, -1) + (5 + Math.floor(Math.random() * 5));
            } else if (tipo === 'menor' && digito2 >= 5) {
                cpf = cpf.slice(0, -1) + Math.floor(Math.random() * 5);
            }

            // Formatar o CPF com pontos e traço
            const cpfFormatado = cpf.replace(/(\d{3})(\d{3})(\d{3})(\d{2})/, "$1.$2.$3-$4");

            // Exibir o CPF gerado
            document.getElementById("resultado").textContent = `CPF Gerado: ${cpfFormatado}`;
        }
    </script>
</body>
</html>
