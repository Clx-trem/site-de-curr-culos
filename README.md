import React, { useState, useRef } from 'react';

export default function CurriculoImpecavelApp() {
  const exemplos = {
    tecnico: {
      profile: {
        nome: 'Carlos Fernando',
        cargo: 'Analista de TI Sênior',
        telefone: '+55 (11) 99999-0000',
        email: 'carlos.exemplo@email.com',
        endereco: 'São Paulo, SP',
        linkedin: 'linkedin.com/in/carlos',
        resumo: 'Analista de TI com 8 anos de experiência em infraestrutura, automação e segurança.'
      },
      experiencias: [
        { empresa: 'Empresa A', cargo: 'Analista de TI', inicio: 'Jan 2021', fim: 'Presente', descricao: 'Gerenciamento de servidores e automação.' },
        { empresa: 'Empresa B', cargo: 'Suporte Técnico', inicio: 'Fev 2018', fim: 'Dez 2020', descricao: 'Suporte e implantação de backups.' }
      ],
      educacao: [ { instituicao: 'Universidade Exemplo', curso: 'Sistemas de Informação', inicio: '2014', fim: '2018' } ],
      habilidades: ['Linux', 'PowerShell', 'Docker', 'Monitoramento']
    }
  };

  const [profile, setProfile] = useState({ nome: 'Seu Nome', cargo: 'Profissão', telefone: '', email: '', endereco: '', linkedin: '', resumo: '' });
  const [experiencias, setExperiencias] = useState([{ empresa: '', cargo: '', inicio: '', fim: '', descricao: '' }]);
  const [educacao, setEducacao] = useState([{ instituicao: '', curso: '', inicio: '', fim: '' }]);
  const [habilidades, setHabilidades] = useState([]);
  const [template, setTemplate] = useState('corporate');
  const previewRef = useRef(null);

  const updateProfile = (f, v) => setProfile(p => ({ ...p, [f]: v }));
  const addExperiencia = () => setExperiencias(p => [...p, { empresa: '', cargo: '', inicio: '', fim: '', descricao: '' }]);
  const updateExperiencia = (i, f, v) => setExperiencias(p => p.map((e, idx) => idx === i ? { ...e, [f]: v } : e));
  const removeExperiencia = i => setExperiencias(p => p.filter((_, idx) => idx !== i));
  const addEducacao = () => setEducacao(p => [...p, { instituicao: '', curso: '', inicio: '', fim: '' }]);
  const updateEducacao = (i, f, v) => setEducacao(p => p.map((e, idx) => idx === i ? { ...e, [f]: v } : e));
  const removeEducacao = i => setEducacao(p => p.filter((_, idx) => idx !== i));
  const addHabilidade = h => setHabilidades(p => [...p, h]);
  const removeHabilidade = i => setHabilidades(p => p.filter((_, idx) => idx !== i));

  async function ensurePdfLibs() {
    if (window.html2canvas && window.jspdf) return true;
    const load = src => new Promise((ok, err) => { const s = document.createElement('script'); s.src = src; s.onload = ok; s.onerror = err; document.head.appendChild(s); });
    if (!window.html2canvas) await load('https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js');
    if (!window.jspdf) await load('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js');
    if (!window.jsPDF && window.jspdf?.jsPDF) window.jsPDF = window.jspdf.jsPDF;
    return true;
  }

  async function exportPDF() {
    await ensurePdfLibs();
    const canvas = await window.html2canvas(previewRef.current, { scale: 2 });
    const img = canvas.toDataURL('image/png');
    const pdf = new window.jsPDF({ unit: 'pt', format: 'a4' });
    const w = pdf.internal.pageSize.getWidth() - 40;
    const h = (canvas.height * w) / canvas.width;
    pdf.addImage(img, 'PNG', 20, 20, w, h);
    pdf.save(`${profile.nome || 'curriculo'}.pdf`);
  }

  function exportJSON() {
    const blob = new Blob([JSON.stringify({ profile, experiencias, educacao, habilidades }, null, 2)], { type: 'application/json' });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = 'curriculo.json';
    a.click();
  }

  const carregarExemplo = () => {
    const e = exemplos.tecnico;
    setProfile(e.profile);
    setExperiencias(e.experiencias);
    setEducacao(e.educacao);
    setHabilidades(e.habilidades);
  };

  return (
    <div style={{ minHeight: '100vh', background: '#f8fafc', padding: 20, display: 'flex', gap: 20 }}>
      <div style={{ width: 360, background: '#fff', padding: 16, borderRadius: 8 }}>
        <h3>Dados</h3>
        <input placeholder="Nome" value={profile.nome} onChange={e => updateProfile('nome', e.target.value)} />
        <input placeholder="Cargo" value={profile.cargo} onChange={e => updateProfile('cargo', e.target.value)} />
        <input placeholder="Telefone" value={profile.telefone} onChange={e => updateProfile('telefone', e.target.value)} />
        <input placeholder="Email" value={profile.email} onChange={e => updateProfile('email', e.target.value)} />
        <textarea placeholder="Resumo" value={profile.resumo} onChange={e => updateProfile('resumo', e.target.value)} />
        <button onClick={carregarExemplo}>Carregar Exemplo</button>
        <button onClick={exportJSON}>Exportar JSON</button>
        <button onClick={exportPDF}>Exportar PDF</button>
      </div>
      <div ref={previewRef} style={{ flex: 1, background: '#fff', padding: 16, borderRadius: 8 }}>
        <h2>{profile.nome}</h2>
        <h4>{profile.cargo}</h4>
        <p>{profile.resumo}</p>
        <h3>Experiências</h3>
        {experiencias.map((e, i) => <div key={i}><b>{e.cargo}</b> - {e.empresa} ({e.inicio} - {e.fim})<p>{e.descricao}</p></div>)}
        <h3>Educação</h3>
        {educacao.map((e, i) => <div key={i}>{e.curso} - {e.instituicao} ({e.inicio}-{e.fim})</div>)}
        <h3>Habilidades</h3>
        <ul>{habilidades.map((h, i) => <li key={i}>{h}</li>)}</ul>
      </div>
    </div>
  );
}
