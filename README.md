<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Currículo Impecável</title>
  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", Roboto, sans-serif;
      background: #f1f5f9;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }
    .container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      width: 100%;
      max-width: 1200px;
    }
    .form, .preview {
      background: #fff;
      border-radius: 8px;
      box-shadow: 0 0 10px #ccc;
      padding: 16px;
      flex: 1;
      min-width: 320px;
    }
    input, textarea {
      width: 100%;
      margin-bottom: 8px;
      padding: 6px 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 14px;
    }
    button {
      width: 100%;
      padding: 10px;
      margin-top: 8px;
      border: none;
      border-radius: 4px;
      background: #2563eb;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover {
      background: #1d4ed8;
    }
    h2, h3 {
      margin-bottom: 6px;
      color: #111827;
    }
    ul {
      padding-left: 20px;
    }
  </style>
</head>
<body>
  <h1>📄 Currículo Impecável</h1>
  <div class="container">
    <div class="form">
      <h3>Seus Dados</h3>
      <input id="nome" placeholder="Nome completo" />
      <input id="cargo" placeholder="Cargo / Profissão" />
      <input id="telefone" placeholder="Telefone" />
      <input id="email" placeholder="E-mail" />
      <textarea id="resumo" placeholder="Resumo profissional"></textarea>
      <button onclick="carregarExemplo()">Carregar Exemplo</button>
      <button onclick="exportJSON()">Exportar JSON</button>
      <button onclick="exportPDF()">Exportar PDF</button>
    </div>
    <div class="preview" id="preview">
      <h2 id="pNome">Seu Nome</h2>
      <h4 id="pCargo">Profissão</h4>
      <p id="pResumo">Aqui aparecerá o resumo do seu perfil profissional.</p>
      <h3>Experiências</h3>
      <div id="experiencias">
        <p>Exemplo: Empresa X - Cargo - 2020 a 2023</p>
      </div>
      <h3>Educação</h3>
      <div id="educacao">
        <p>Exemplo: Curso - Universidade</p>
      </div>
      <h3>Habilidades</h3>
      <ul id="habilidades">
        <li>Comunicação</li>
        <li>Trabalho em equipe</li>
      </ul>
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <script>
    const dados = {
      nome: "",
      cargo: "",
      telefone: "",
      email: "",
      resumo: "",
      experiencias: [
        { empresa: "Empresa A", cargo: "Analista de TI", periodo: "2021 - Atual" },
        { empresa: "Empresa B", cargo: "Suporte Técnico", periodo: "2018 - 2020" }
      ],
      educacao: [
        { curso: "Sistemas de Informação", instituicao: "Universidade Exemplo", periodo: "2014 - 2018" }
      ],
      habilidades: ["Linux", "PowerShell", "Docker", "Monitoramento"]
    };

    function atualizarPreview() {
      document.getElementById("pNome").textContent = document.getElementById("nome").value || "Seu Nome";
      document.getElementById("pCargo").textContent = document.getElementById("cargo").value || "Profissão";
      document.getElementById("pResumo").textContent = document.getElementById("resumo").value || "Resumo profissional";
    }

    document.querySelectorAll("input, textarea").forEach(el => {
      el.addEventListener("input", atualizarPreview);
    });

    function carregarExemplo() {
      document.getElementById("nome").value = "Carlos Fernando";
      document.getElementById("cargo").value = "Analista de TI Sênior";
      document.getElementById("telefone").value = "+55 (11) 99999-0000";
      document.getElementById("email").value = "carlos@email.com";
      document.getElementById("resumo").value = "Profissional com 8 anos de experiência em infraestrutura e automação.";

      const expDiv = document.getElementById("experiencias");
      expDiv.innerHTML = dados.experiencias
        .map(e => `<p><b>${e.cargo}</b> - ${e.empresa} (${e.periodo})</p>`)
        .join("");

      const eduDiv = document.getElementById("educacao");
      eduDiv.innerHTML = dados.educacao
        .map(e => `<p>${e.curso} - ${e.instituicao} (${e.periodo})</p>`)
        .join("");

      const habUl = document.getElementById("habilidades");
      habUl.innerHTML = dados.habilidades.map(h => `<li>${h}</li>`).join("");

      atualizarPreview();
    }

    function exportJSON() {
      const obj = {
        nome: document.getElementById("nome").value,
        cargo: document.getElementById("cargo").value,
        telefone: document.getElementById("telefone").value,
        email: document.getElementById("email").value,
        resumo: document.getElementById("resumo").value,
      };
      const blob = new Blob([JSON.stringify(obj, null, 2)], { type: "application/json" });
      const a = document.createElement("a");
      a.href = URL.createObjectURL(blob);
      a.download = "curriculo.json";
      a.click();
    }

    async function exportPDF() {
      const { jsPDF } = window.jspdf;
      const preview = document.getElementById("preview");
      const canvas = await html2canvas(preview, { scale: 2 });
      const img = canvas.toDataURL("image/png");
      const pdf = new jsPDF("p", "pt", "a4");
      const width = pdf.internal.pageSize.getWidth() - 40;
      const height = (canvas.height * width) / canvas.width;
      pdf.addImage(img, "PNG", 20, 20, width, height);
      pdf.save("curriculo.pdf");
    }
  </script>
</body>
</html>
