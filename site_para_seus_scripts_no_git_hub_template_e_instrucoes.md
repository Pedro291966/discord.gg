# Template de site para guardar scripts (GitHub Pages)

Aqui está um template completo e instruções passo a passo para criar um site estático onde **apenas você** poderá editar os arquivos (no repositório GitHub). O conteúdo abaixo contém todos os arquivos necessários (index.html, styles.css, scripts.json) e instruções para criar o repositório, configurar o GitHub Pages e garantir que só você tenha permissão de escrita.

> **Como funciona (resumo):**
> - Você cria um repositório **privado** no GitHub e é o único colaborador/administrador.
> - Publica o site com **GitHub Pages** a partir da branch `main` (ou `gh-pages`).
> - Só quem tiver permissão de escrita no repositório (você) poderá alterar o conteúdo (scripts.json ou HTML).

---

## Arquivos do site

### `index.html`

```html
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Meus Scripts — GIHUB</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>Meus Scripts</h1>
    <p>Site de scripts — somente edição via GitHub (apenas eu).</p>
  </header>

  <main>
    <section id="scripts-list">
      <p>Carregando scripts...</p>
    </section>
  </main>

  <footer>
    <small>Gerenciado no GitHub — mantenha este repositório privado para impedir edições por terceiros.</small>
  </footer>

  <script>
    async function loadScripts() {
      try {
        const res = await fetch('scripts.json', {cache: 'no-store'});
        if (!res.ok) throw new Error('Não foi possível carregar scripts.json');
        const data = await res.json();
        const el = document.getElementById('scripts-list');
        if (!data.scripts || data.scripts.length === 0) {
          el.innerHTML = '<p>Nenhum script cadastrado.</p>';
          return;
        }
        const ul = document.createElement('ul');
        data.scripts.forEach(s => {
          const li = document.createElement('li');
          li.innerHTML = `<strong>${escapeHtml(s.name)}</strong> — <code>${escapeHtml(s.filename)}</code><br>${escapeHtml(s.description)}<br><a href="${s.fileurl}" target="_blank" rel="noopener">Baixar / Ver</a>`;
          ul.appendChild(li);
        });
        el.innerHTML = '';
        el.appendChild(ul);
      } catch (err) {
        document.getElementById('scripts-list').innerHTML = '<p>Erro ao carregar scripts.</p>';
        console.error(err);
      }
    }

    function escapeHtml(text){
      if (!text) return '';
      return text.replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":"&#39;"})[c]);
    }

    loadScripts();
  </script>
</body>
</html>
```

---

### `styles.css`

```css
:root{--bg:#0f1724;--card:#0b1220;--text:#e6eef8}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,Arial;background:linear-gradient(180deg,var(--bg),#061021);color:var(--text);padding:24px}
header{margin-bottom:16px}
h1{margin:0 0 4px 0}
main{background:rgba(255,255,255,0.03);padding:18px;border-radius:12px;max-width:900px}
ul{list-style:none;padding:0;margin:0}
li{padding:12px;border-bottom:1px solid rgba(255,255,255,0.03)}
code{background:rgba(255,255,255,0.04);padding:2px 6px;border-radius:4px}
footer{margin-top:14px;font-size:12px;opacity:0.8}
```

---

### `scripts.json` (exemplo)

```json
{
  "scripts": [
    {
      "name": "Script Exemplo 1",
      "filename": "exemplo1.lua",
      "description": "Descrição curta do script.",
      "fileurl": "exemplo1.lua"
    },
    {
      "name": "Script Exemplo 2",
      "filename": "exemplo2.lua",
      "description": "Outro script.",
      "fileurl": "exemplo2.lua"
    }
  ]
}
```

> Observação: os `fileurl` podem ser links relativos (arquivos no mesmo repositório) ou URLs externas (ex.: Google Drive/Raw GitHub links).

---

## Instruções passo a passo (GitHub)

1. **Crie o repositório no GitHub**
   - Vá para GitHub ➜ New repository.
   - Dê um nome (ex.: `meus-scripts`), marque como **Private** para impedir que outros editem.
   - Não adicione colaboradores — somente sua conta terá acesso de escrita.

2. **Adicione os arquivos**
   - Clone o repositório localmente ou use o botão **Add file → Create new file** no site.
   - Coloque `index.html`, `styles.css`, `scripts.json` e (se quiser) os arquivos de script `.lua`, `.js` etc.

3. **Habilite o GitHub Pages**
   - Vá em Settings → Pages (ou Code and automation → Pages) do repositório.
   - Source: selecione `main` branch e root (`/`) como pasta (ou `gh-pages` se preferir uma branch específica).
   - Salve — em alguns minutos o site estará disponível em `https://<seunome>.github.io/<repositorio>/` ou em um domínio personalizado.

4. **Controle de edição**
   - Como o repositório é privado e apenas sua conta tem permissão de escrita, **só você** poderá editar arquivos pelo GitHub Web ou por `git push`.
   - Se quiser adicionar colaboradores no futuro, faça isso com cuidado (Settings → Collaborators) e apenas dê permissão de leitura quando quiser que vejam mas não editem.

5. **Dicas de segurança**
   - Ative **2FA (autenticação de dois fatores)** na sua conta GitHub.
   - Não compartilhe tokens de acesso ou chaves privadas.

---

## Opções avançadas (automação)

- **Deploy automático com GitHub Actions**: se você quiser gerar/empacotar ou processar arquivos antes de publicar, eu posso adicionar um workflow de Actions que roda a cada push e publica para `gh-pages`.
- **Mostrar scripts somente para visitantes logados**: páginas estáticas não têm login embutido. Se quiser um painel restrito com login (ex.: via Google/Gmail), precisaríamos criar um back-end ou usar um serviço (Netlify Identity, Firebase Auth, ou solução custom). Posso preparar isso se desejar.

---

## Pronto para eu criar os arquivos pra você?

Se quiser, eu já crio aqui o conjunto de arquivos prontos (index.html, styles.css, scripts.json e um README com instruções) e você só copia para um repositório. Diga "Sim, crie os arquivos" e já deixo tudo pronto para baixar/copiar.


---

*Observação:* este documento contém tudo o que você precisa para publicar e controlar edição. Se preferir, também posso gerar um repositório pronto (ZIP) com estes arquivos.

