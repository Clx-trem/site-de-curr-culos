<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Currículo Impecável — Profissional</title>

  <!-- html2canvas + jspdf via CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --bg:#f3f6f9;
      --card:#ffffff;
      --muted:#6b7280;
      --accent:#0f172a; /* dark blue/gray professional */
      --accent-2:#2563eb;
      --border:#e6e9ee;
    }
    .dark {
      --bg:#0b1220;
      --card:#071025;
      --muted:#9aa4b2;
      --accent:#e6eefc;
      --accent-2:#60a5fa;
      --border:#132033;
    }

    html,body{height:100%;margin:0;background:var(--bg);font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;}

    .wrap{max-width:1200px;margin:26px auto;padding:20px;display:grid;grid-template-columns:360px 1fr;gap:20px;}
    .panel{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:16px;box-shadow:0 6px 18px rgba(2,6,23,0.04);}
    h1{margin:0 0 8px 0;font-size:20px;color:var(--accent);}
    label{display:block;font-size:13px;color:var(--muted);margin-top:12px;}
    input[type="text"], input[type="email"], textarea, select{width:100%;padding:9px;border:1px solid var(--border);border-radius:8px;background:transparent;color:var(--accent);box-sizing:border-box;}
    textarea{min-height:90px;resize:vertical;}
    .btn{display:inline-block;padding:10px 12px;border-radius:8px;border:none;background:var(--accent-2);color:white;font-weight:600;cursor:pointer;margin-top:10px;}
    .btn-ghost{background:transparent;border:1px solid var(--border);color:var(--accent);margin-left:8px;}
    .row{display:flex;gap:8px;margin-top:8px;}
    .small{font-size:13px;color:var(--muted);}
    .templates{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px;}
    .templateOption{padding:8px 10px;border-radius:8px;border:1px solid var(--border);cursor:pointer;font-size:13px;}
    .templateOption.active{box-shadow:0 4px 12px rgba(37,99,235,0.12);border-color:var(--accent-2);}

    /* preview area */
    .previewWrap{display:flex;flex-direction:column;gap:12px;}
    .previewArea{background:var(--card);padding:18px;border-radius:8px;border:1px solid var(--border);min-height:680px;}
    .previewHeader{display:flex;justify-content:space-between;align-items:flex-start;}
    .name{font-size:22px;font-weight:700;color:var(--accent);}
    .job{font-size:14px;color:var(--muted);margin-top:6px;}
    .contact{font-size:13px;color:var(--muted);text-align:right;}
    .sectionTitle{margin-top:14px;color:var(--accent-2);font-weight:700;font-size:12px;letter-spacing:1px;}
    .expItem{margin-top:8px;}
    .skillTag{display:inline-block;padding:6px 8px;margin:6px 6px 0 0;border-radius:999px;border:1px solid var(--border);font-size:13px;color:var(--accent);}

    /* layout templates */
    .tpl-classic .previewHeader{border-bottom:1px solid var(--border);padding-bottom:12px;}
    .tpl-modern .previewArea{padding:22px;}
    .tpl-minimal .name{font-size:20px}
    .tpl-creative .previewArea{border-left:6px solid var(--accent-2);padding-left:14px;}

    /* utilities */
    .flex{display:flex;gap:8px;align-items:center;}
    .right{margin-left:auto;}
    footer{max-width:1200px;margin:12px auto;color:var(--muted);font-size:13px;text-align:center}
    @media(max-width:940px){
      .wrap{grid-template-columns:1fr; padding:12px;}
      .previewArea{min-height:520px;}
    }
  </style>
</head>
<body>
  <div style="max-width:1200px;margin:10px auto;padding:0 16px;display:flex;align-items:center;justify-content:space-between;">
    <div>
      <h1 style="margin-bottom:2px">Currículo Impecável — Profissional</h1>
      <div class="small">Crie currículos prontos para enviar — exporte como PDF com um clique.</div>
    </div>
    <div style="display:flex;gap:8px;align-items:center;">
      <button id="themeBtn" class="btn btn-ghost" title="Alternar modo escuro/claro">Modo Escuro</button>
      <a class="small" href="#" onclick="carregarExemplo();return false;">Carregar exemplo</a>
    </div>
  </div>

  <main class="wrap">
    <!-- left panel: form + controls -->
    <aside class="panel">
      <label>Nome completo</label>
      <input id="nome" type="text" placeholder="Ex: Carlos Fernando" />
      <label>Cargo / Profissão</label>
      <input id="cargo" type="text" placeholder="Ex: Analista de TI Sênior" />
      <label>Resumo profissional (2–4 linhas)</label>
      <textarea id="resumo" placeholder="Breve resumo com resultados e foco profissional"></textarea>
      <label>Experiências (um por linha - Formato: Cargo | Empresa | Período | Breve realização)</label>
      <textarea id="experienciasInput" placeholder="Ex: Analista de TI | Empresa X | 2021–Presente | Reduziu downtime em 40%"></textarea>
      <label>Formação (um por linha - Formato: Curso | Instituição | Período)</label>
      <textarea id="educacaoInput" placeholder="Ex: Sistemas de Informação | Universidade Y | 2015–2019"></textarea>
      <label>Habilidades (separadas por vírgula)</label>
      <input id="habilidadesInput" placeholder="Ex: Linux, Docker, SQL, Automação" />
      <label style="margin-top:12px">Modelo</label>
      <div class="templates" id="templates">
        <div class="templateOption active" data-tpl="classic">Clássico</div>
        <div class="templateOption" data-tpl="modern">Moderno</div>
        <div class="templateOption" data-tpl="minimal">Minimal</div>
        <div class="templateOption" data-tpl="creative">Criativo</div>
      </div>
      <div class="row" style="margin-top:12px">
        <button id="btnRender" class="btn">Atualizar Preview</button>
        <button id="btnExportPdf" class="btn btn-ghost" style="width:150px">Exportar PDF</button>
      </div>
      <div class="row" style="margin-top:8px">
        <button id="btnExportJson" class="btn btn-ghost" style="width:150px">Exportar JSON</button>
        <button id="btnClear" class="btn btn-ghost" style="width:150px">Limpar</button>
      </div>
      <p class="small" style="margin-top:12px">Dica: Use verbos de ação, quantifique resultados e priorize experiência relevante.</p>
    </aside>
    <!-- right panel: preview -->
    <section class="panel previewWrap">
      <div id="previewAreaRoot" class="previewArea tpl-classic" aria-live="polite">
        <!-- preview content gerada via JS -->
      </div>
      <div style="display:flex;gap:10px;align-items:center;">
        <div class="small">Nome do arquivo ao exportar:</div>
        <div class="small" id="fileNamePreview">Curriculo_Nome.pdf</div>
        <div class="right"></div>
      </div>
    </section>
  </main>

  <footer>Currículo Impecável • Modelo profissional corporativo • Copie e use</footer>

  <script>
    // --- Helpers ---
    const $ = id => document.getElementById(id);
    const sanitizeFilename = s => (s || 'curriculo').replace(/\s+/g,'_').replace(/[^\w\-\.]/g,'');
    const formatArrayFromTextarea = (text, delim='\\n') => text.split(new RegExp(delim)).map(l=>l.trim()).filter(Boolean);

    // initial state
    const state = {
      template: 'classic'
    };

    // template switching UI
    document.querySelectorAll('.templateOption').forEach(el=>{
      el.addEventListener('click', ()=>{
        document.querySelectorAll('.templateOption').forEach(x=>x.classList.remove('active'));
        el.classList.add('active');
        state.template = el.dataset.tpl;
        renderPreview();
      });
    });

    // theme toggle
    const themeBtn = document.getElementById('themeBtn');
    const root = document.documentElement;
    const setDark = (d) => {
      if(d){ document.documentElement.classList.add('dark'); themeBtn.textContent='Modo Claro'; }
      else { document.documentElement.classList.remove('dark'); themeBtn.textContent='Modo Escuro'; }
    };
    // remember preference (basic)
    const savedTheme = localStorage.getItem('cv_theme_dark') === '1';
    setDark(savedTheme);
    themeBtn.onclick = () => { const isDark = !document.documentElement.classList.contains('dark'); setDark(isDark); localStorage.setItem('cv_theme_dark', isDark? '1':'0'); };

    // load example quick
    function carregarExemplo(){
      $('nome').value = 'Carlos Fernando';
      $('cargo').value = 'Analista de TI Sênior';
      $('resumo').value = 'Analista de TI com 8 anos de experiência em operações, infraestrutura e automação. Foco em disponibilidade, automação de processos e redução de custos.';
      $('experienciasInput').value = 'Analista de TI | Empresa A | Jan 2021 – Presente | Implementou automações que reduziram tempo de deploy em 60%\\nTécnico de Suporte | Empresa B | Fev 2018 – Dez 2020 | Gerenciamento de backups e suporte a 200+ usuários';
      $('educacaoInput').value = 'Sistemas de Informação | Universidade Exemplo | 2014 – 2018';
      $('habilidadesInput').value = 'Linux, Docker, Bash, Monitoramento, Automação';
      renderPreview();
    }

    // render preview according to template and inputs
    function renderPreview(){
      const nome = $('nome').value.trim() || 'Seu Nome';
      const cargo = $('cargo').value.trim() || 'Profissão';
      const resumo = $('resumo').value.trim() || 'Resumo profissional...';
      const experiencias = formatArrayFromTextarea($('experienciasInput').value, '\\n');
      const educacao = formatArrayFromTextarea($('educacaoInput').value, '\\n');
      const habilidades = $('habilidadesInput').value.split(',').map(s=>s.trim()).filter(Boolean);

      // Build structured content
      const expHTML = experiencias.map(line=>{
        // try to split "Cargo | Empresa | Período | Realização"
        const parts = line.split('|').map(p=>p.trim());
        const title = parts[0] || '';
        const company = parts[1] || '';
        const period = parts[2] || '';
        const bullets = parts.slice(3).join(' • ');
        return `<div class="expItem"><div style="display:flex;justify-content:space-between;"><div style="font-weight:600">${escapeHtml(title)} ${company? '— '+escapeHtml(company):''}</div><div style="color:var(--muted)">${escapeHtml(period)}</div></div>${bullets? `<div style="color:var(--muted);margin-top:6px">${escapeHtml(bullets)}</div>`:''}</div>`;
      }).join('');

      const eduHTML = educacao.map(line=>{
        const parts = line.split('|').map(p=>p.trim());
        const course = parts[0] || '';
        const inst = parts[1] || '';
        const period = parts[2] || '';
        return `<div style="margin-top:6px">${escapeHtml(course)}${inst? ' — '+escapeHtml(inst):''} <span style="color:var(--muted)">${escapeHtml(period)}</span></div>`;
      }).join('');

      const skillsHTML = habilidades.map(h => `<span class="skillTag">${escapeHtml(h)}</span>`).join('');

      // Template variations (corporate look variations)
      const tpl = state.template || 'classic';
      const previewRoot = $('previewAreaRoot');
      previewRoot.className = 'previewArea tpl-' + tpl;

      // content base
      let content = `
        <div class="previewHeader">
          <div>
            <div class="name">${escapeHtml(nome)}</div>
            <div class="job">${escapeHtml(cargo)}</div>
            <div style="margin-top:8px;color:var(--muted);font-size:13px;">${escapeHtml(resumo)}</div>
          </div>
          <div class="contact">
            <div>${escapeHtml($('nome').value? $('nome').value : '')}</div>
            <div style="margin-top:6px">${escapeHtml($('nome').value? $('nome').value : '')}</div>
          </div>
        </div>
        <div>
          <div class="sectionTitle">Experiência</div>
          <div>${expHTML || '<div style="color:var(--muted);margin-top:6px">Adicione suas experiências na caixa à esquerda e clique em Atualizar Preview.</div>'}</div>

          <div class="sectionTitle">Formação</div>
          <div>${eduHTML || '<div style="color:var(--muted);margin-top:6px">Adicione sua formação na caixa à esquerda.</div>'}</div>

          <div class="sectionTitle">Habilidades</div>
          <div style="margin-top:8px">${skillsHTML || '<div style="color:var(--muted)">Adicione habilidades separadas por vírgula.</div>'}</div>
        </div>
      `;

      // adjust small differences per template
      if(tpl === 'modern'){
        content = `<div style="display:flex;gap:20px;"><div style="flex:1">${content}</div><aside style="width:240px;padding-left:8px;border-left:1px solid var(--border)"><div class="sectionTitle">Contato</div><div style="margin-top:8px;color:var(--muted)}">${escapeHtml($('nome').value||'')}</div><div style="margin-top:12px">${skillsHTML}</div></aside></div>`;
      } else if(tpl === 'minimal'){
        // minimal: smaller margins
        previewRoot.style.padding = '12px';
      } else if(tpl === 'creative'){
        // creative: accent column applied by CSS
      } else {
        // classic default: keep content
      }

      previewRoot.innerHTML = content;

      // update filename preview
      const fname = 'Curriculo_' + sanitizeFilename(nome) + '.pdf';
      $('fileNamePreview').textContent = fname;
    }

    // escape HTML helper
    function escapeHtml(s){ return String(s)
        .replace(/&/g,'&amp;')
        .replace(/</g,'&lt;')
        .replace(/>/g,'&gt;')
        .replace(/"/g,'&quot;')
        .replace(/'/g,'&#039;'); }

    // export PDF using html2canvas + jsPDF
    async function exportPDF(){
      const preview = $('previewAreaRoot');
      // temporarily increase width for better print when on small screens
      const originalWidth = preview.style.width;
      preview.style.width = preview.offsetWidth + 'px';
      try {
        const canvas = await html2canvas(preview, { scale: 2, useCORS: true, allowTaint: false });
        const imgData = canvas.toDataURL('image/png');
        // jsPDF: UMD exposes window.jspdf.jsPDF
        const jsPDF = window.jspdf?.jsPDF || window.jsPDF;
        const pdf = new jsPDF({ unit: 'pt', format: 'a4' });
        const pageWidth = pdf.internal.pageSize.getWidth();
        const margin = 40;
        const maxW = pageWidth - margin * 2;
        const ratio = Math.min(maxW / canvas.width, (pdf.internal.pageSize.getHeight() - margin * 2) / canvas.height);
        const w = canvas.width * ratio;
        const h = canvas.height * ratio;
        pdf.addImage(imgData, 'PNG', margin, margin, w, h);
        const nome = $('nome').value.trim() || 'Nome';
        const fileName = 'Curriculo_' + sanitizeFilename(nome) + '.pdf';
        pdf.save(fileName);
      } catch (err){
        alert('Erro ao gerar PDF: ' + (err.message || err));
        console.error(err);
      } finally {
        preview.style.width = originalWidth;
      }
    }

    // export JSON
    function exportJSON(){
      const payload = {
        nome: $('nome').value,
        cargo: $('cargo').value,
        resumo: $('resumo').value,
        experiencias: formatArrayFromTextarea($('experienciasInput').value,'\\n'),
        educacao: formatArrayFromTextarea($('educacaoInput').value,'\\n'),
        habilidades: $('habilidadesInput').value.split(',').map(s=>s.trim()).filter(Boolean),
        template: state.template
      };
      const blob = new Blob([JSON.stringify(payload, null, 2)], { type: 'application/json' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'curriculo_' + sanitizeFilename(payload.nome || 'export') + '.json';
      a.click();
      URL.revokeObjectURL(a.href);
    }

    // clear all
    function clearAll(){
      $('nome').value=''; $('cargo').value=''; $('resumo').value=''; $('experienciasInput').value=''; $('educacaoInput').value=''; $('habilidadesInput').value='';
      renderPreview();
    }

    // wire buttons
    $('btnRender').addEventListener('click', renderPreview);
    $('btnExportPdf').addEventListener('click', exportPDF);
    $('btnExportJson').addEventListener('click', exportJSON);
    $('btnClear').addEventListener('click', clearAll);

    // initial render
    renderPreview();

    // keyboard: Ctrl+Enter to export PDF
    document.addEventListener('keydown', (e)=>{
      if(e.ctrlKey && e.key === 'Enter') exportPDF();
    });
  </script>
</body>
</html>
