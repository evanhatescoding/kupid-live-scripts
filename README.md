# kupid-live-scripts
Internal script library for Kupid.live creators to upload, use, and check off Omegle-style scripts.
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kupid.live Script Library</title>
  <style>
    :root { --bg:#0b0b10; --card:#131320; --muted:#9aa0aa; --text:#eef0f4; --accent:#ff3b81; --accent2:#4d7cff; }
    * { box-sizing:border-box; }
    body { margin:0; font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial; background: radial-gradient(1200px 600px at 20% -10%, rgba(255,59,129,.25), transparent 60%), radial-gradient(900px 500px at 90% 0%, rgba(77,124,255,.22), transparent 55%), var(--bg);
           color:var(--text); }
    a { color:inherit; }
    header { padding:28px 18px 14px; max-width:1100px; margin:0 auto; }
    h1 { margin:0; font-size:22px; letter-spacing:.2px; }
    .sub { margin-top:6px; color:var(--muted); font-size:13px; line-height:1.35; }

    main { max-width:1100px; margin:0 auto; padding:10px 18px 38px; }
    .grid { display:grid; grid-template-columns: 360px 1fr; gap:16px; }
    @media (max-width: 980px){ .grid{ grid-template-columns:1fr; } }

    .card { background: linear-gradient(180deg, rgba(255,255,255,.04), rgba(255,255,255,.02)); border:1px solid rgba(255,255,255,.08); border-radius:18px; padding:14px; box-shadow: 0 10px 30px rgba(0,0,0,.25); }
    .card h2 { margin:0 0 10px; font-size:14px; color:#fff; opacity:.95; }

    label { display:block; font-size:12px; color:var(--muted); margin:10px 0 6px; }
    input, textarea, select { width:100%; border-radius:14px; border:1px solid rgba(255,255,255,.10); background:rgba(10,10,18,.65); color:var(--text); padding:10px 12px; outline:none; }
    textarea { min-height:110px; resize:vertical; }
    .row { display:flex; gap:10px; }
    .row > * { flex:1; }
    .btns { display:flex; gap:10px; flex-wrap:wrap; margin-top:12px; }
    button { border:0; border-radius:14px; padding:10px 12px; background:rgba(255,255,255,.08); color:var(--text); cursor:pointer; font-weight:600; }
    button.primary { background: linear-gradient(135deg, rgba(255,59,129,.95), rgba(77,124,255,.9)); }
    button.danger { background: rgba(255,70,70,.22); }
    button:disabled { opacity:.45; cursor:not-allowed; }

    .toolbar { display:flex; gap:10px; flex-wrap:wrap; align-items:center; }
    .toolbar input { max-width:360px; }

    .list { display:flex; flex-direction:column; gap:10px; margin-top:12px; }
    .item { padding:12px; border-radius:16px; border:1px solid rgba(255,255,255,.08); background:rgba(255,255,255,.03); }
    .topline { display:flex; gap:10px; align-items:flex-start; justify-content:space-between; }
    .title { font-size:14px; font-weight:800; }
    .meta { color:var(--muted); font-size:12px; margin-top:2px; }
    .tags { display:flex; gap:6px; flex-wrap:wrap; margin-top:8px; }
    .tag { font-size:11px; color:#dfe3ea; padding:4px 8px; border-radius:999px; border:1px solid rgba(255,255,255,.10); background:rgba(0,0,0,.2); }

    .actions { display:flex; gap:8px; flex-wrap:wrap; margin-top:10px; }
    .check { display:flex; align-items:center; gap:8px; }
    .check input { width:auto; }

    pre { margin:10px 0 0; white-space:pre-wrap; word-wrap:break-word; background:rgba(0,0,0,.25); border:1px solid rgba(255,255,255,.07); padding:10px; border-radius:14px; font-size:13px; line-height:1.35; }

    .foot { margin-top:14px; color:var(--muted); font-size:12px; line-height:1.35; }
    .small { font-size:11px; color:var(--muted); }
    .pill { display:inline-block; padding:4px 8px; border-radius:999px; background:rgba(255,255,255,.06); border:1px solid rgba(255,255,255,.10); }
  </style>
</head>
<body>
  <header>
    <h1>Kupid.live Script Library</h1>
    <div class="sub">Upload Omegle style scripts, assign categories, and let creators check them off as they complete them. All data is stored locally in your browser (no backend). Use Export/Import to share with the team.</div>
  </header>

  <main>
    <div class="grid">
      <section class="card" aria-label="Upload scripts">
        <h2>Upload a script</h2>

        <label for="title">Title</label>
        <input id="title" placeholder="Ex: 3 spicy openers for tonight" maxlength="80" />

        <div class="row">
          <div>
            <label for="channel">Channel</label>
            <select id="channel">
              <option value="kupid.live">kupid.live</option>
              <option value="kupiddating">kupiddating</option>
              <option value="founder">founder</option>
              <option value="other">other</option>
            </select>
          </div>
          <div>
            <label for="category">Category</label>
            <select id="category">
              <option value="Openers">Openers</option>
              <option value="Questions">Questions</option>
              <option value="Challenges">Challenges</option>
              <option value="Bits">Bits</option>
              <option value="Closing">Closing</option>
              <option value="Other">Other</option>
            </select>
          </div>
        </div>

        <label for="tags">Tags (comma separated)</label>
        <input id="tags" placeholder="Ex: funny, flirty, fast" />

        <label for="script">Script text</label>
        <textarea id="script" placeholder="Paste the script here..."></textarea>

        <div class="btns">
          <button class="primary" id="addBtn">Add script</button>
          <button id="clearFormBtn">Clear</button>
          <button id="seedBtn" title="Adds a few example scripts">Add examples</button>
        </div>

        <hr style="border:0;border-top:1px solid rgba(255,255,255,.08);margin:14px 0;" />

        <h2>Share / Backup</h2>
        <div class="btns">
          <button id="exportBtn">Export JSON</button>
          <button id="importBtn">Import JSON</button>
          <button class="danger" id="wipeBtn">Wipe all data</button>
        </div>
        <div class="foot">
          <div class="small">Tip: Export on your laptop, send the JSON to your team in Slack, then they Import. Everyone keeps their own checkoff state locally.</div>
        </div>
      </section>

      <section class="card" aria-label="Scripts list">
        <div class="toolbar">
          <div class="pill" id="countPill">0 scripts</div>
          <input id="search" placeholder="Search title, text, tag..." />
          <select id="filterChannel" title="Filter by channel">
            <option value="">All channels</option>
            <option value="kupid.live">kupid.live</option>
            <option value="kupiddating">kupiddating</option>
            <option value="founder">founder</option>
            <option value="other">other</option>
          </select>
          <select id="filterCategory" title="Filter by category">
            <option value="">All categories</option>
            <option value="Openers">Openers</option>
            <option value="Questions">Questions</option>
            <option value="Challenges">Challenges</option>
            <option value="Bits">Bits</option>
            <option value="Closing">Closing</option>
            <option value="Other">Other</option>
          </select>
          <select id="sort" title="Sort">
            <option value="new">Newest</option>
            <option value="old">Oldest</option>
            <option value="title">Title A → Z</option>
            <option value="checked">Unchecked first</option>
          </select>
        </div>

        <div class="list" id="list"></div>

        <div class="foot">
          <div><span class="pill">Checkoff</span> is per browser. If you want shared checkoff, you will need a tiny backend (Supabase/Firebase). I can convert this to that in one step.</div>
        </div>
      </section>
    </div>
  </main>

  <input id="file" type="file" accept="application/json" style="display:none" />

  <script>
    const LS_SCRIPTS = 'ktv_scripts_v1';
    const LS_CHECKED = 'ktv_scripts_checked_v1';

    /** @type {{id:string,title:string,channel:string,category:string,tags:string[],text:string,createdAt:number}[]} */
    let scripts = loadScripts();
    /** @type {Record<string, boolean>} */
    let checked = loadChecked();

    const el = (id) => document.getElementById(id);

    function uid(){
      return 's_' + Math.random().toString(16).slice(2) + '_' + Date.now().toString(16);
    }

    function loadScripts(){
      try{ return JSON.parse(localStorage.getItem(LS_SCRIPTS) || '[]'); }
      catch{ return []; }
    }

    function saveScripts(){
      localStorage.setItem(LS_SCRIPTS, JSON.stringify(scripts));
    }

    function loadChecked(){
      try{ return JSON.parse(localStorage.getItem(LS_CHECKED) || '{}'); }
      catch{ return {}; }
    }

    function saveChecked(){
      localStorage.setItem(LS_CHECKED, JSON.stringify(checked));
    }

    function normalizeTags(str){
      return (str || '')
        .split(',')
        .map(t => t.trim())
        .filter(Boolean)
        .slice(0, 10);
    }

    function matchesQuery(s, q){
      if(!q) return true;
      const hay = (s.title + ' ' + s.text + ' ' + s.tags.join(' ') + ' ' + s.channel + ' ' + s.category).toLowerCase();
      return hay.includes(q.toLowerCase());
    }

    function render(){
      const q = el('search').value.trim();
      const fc = el('filterChannel').value;
      const fcat = el('filterCategory').value;
      const sort = el('sort').value;

      let list = scripts
        .filter(s => matchesQuery(s, q))
        .filter(s => !fc || s.channel === fc)
        .filter(s => !fcat || s.category === fcat);

      if(sort === 'new') list.sort((a,b) => b.createdAt - a.createdAt);
      if(sort === 'old') list.sort((a,b) => a.createdAt - b.createdAt);
      if(sort === 'title') list.sort((a,b) => a.title.localeCompare(b.title));
      if(sort === 'checked') list.sort((a,b) => (checked[a.id] === true) - (checked[b.id] === true));

      el('countPill').textContent = `${list.length} script${list.length === 1 ? '' : 's'}`;

      const wrap = el('list');
      wrap.innerHTML = '';

      if(list.length === 0){
        const empty = document.createElement('div');
        empty.className = 'item';
        empty.innerHTML = `<div class="meta">No scripts found. Add one on the left, or hit <b>Add examples</b>.</div>`;
        wrap.appendChild(empty);
        return;
      }

      for(const s of list){
        const item = document.createElement('div');
        item.className = 'item';

        const isDone = !!checked[s.id];

        item.innerHTML = `
          <div class="topline">
            <div>
              <div class="title">${escapeHtml(s.title)}</div>
              <div class="meta">${escapeHtml(s.channel)} · ${escapeHtml(s.category)} · ${new Date(s.createdAt).toLocaleString()}</div>
              <div class="tags">${s.tags.map(t => `<span class="tag">${escapeHtml(t)}</span>`).join('')}</div>
            </div>
            <div class="check">
              <input type="checkbox" id="ck_${s.id}" ${isDone ? 'checked' : ''} />
              <label for="ck_${s.id}" style="margin:0;color:var(--muted);">Done</label>
            </div>
          </div>
          <pre>${escapeHtml(s.text)}</pre>
          <div class="actions">
            <button data-copy="${s.id}">Copy</button>
            <button data-edit="${s.id}">Edit</button>
            <button class="danger" data-del="${s.id}">Delete</button>
          </div>
        `;

        wrap.appendChild(item);

        item.querySelector(`#ck_${CSS.escape(s.id)}`).addEventListener('change', (e) => {
          checked[s.id] = e.target.checked;
          saveChecked();
          if(el('sort').value === 'checked') render();
        });

        item.querySelector(`[data-copy="${CSS.escape(s.id)}"]`).addEventListener('click', async () => {
          await navigator.clipboard.writeText(s.text);
          toast('Copied');
        });

        item.querySelector(`[data-edit="${CSS.escape(s.id)}"]`).addEventListener('click', () => {
          // Load into form for quick edit
          el('title').value = s.title;
          el('channel').value = s.channel;
          el('category').value = s.category;
          el('tags').value = s.tags.join(', ');
          el('script').value = s.text;
          el('addBtn').textContent = 'Save changes';
          el('addBtn').dataset.editing = s.id;
          window.scrollTo({ top: 0, behavior: 'smooth' });
        });

        item.querySelector(`[data-del="${CSS.escape(s.id)}"]`).addEventListener('click', () => {
          if(!confirm('Delete this script?')) return;
          scripts = scripts.filter(x => x.id !== s.id);
          delete checked[s.id];
          saveScripts();
          saveChecked();
          render();
        });
      }
    }

    function resetForm(){
      el('title').value = '';
      el('tags').value = '';
      el('script').value = '';
      el('channel').value = 'kupid.live';
      el('category').value = 'Openers';
      el('addBtn').textContent = 'Add script';
      delete el('addBtn').dataset.editing;
    }

    function addOrSave(){
      const title = el('title').value.trim();
      const channel = el('channel').value;
      const category = el('category').value;
      const tags = normalizeTags(el('tags').value);
      const text = el('script').value.trim();

      if(!title || !text){
        toast('Title and script text are required');
        return;
      }

      const editingId = el('addBtn').dataset.editing;
      if(editingId){
        const idx = scripts.findIndex(s => s.id === editingId);
        if(idx >= 0){
          scripts[idx] = { ...scripts[idx], title, channel, category, tags, text };
          saveScripts();
          resetForm();
          render();
          toast('Saved');
          return;
        }
      }

      scripts.push({
        id: uid(),
        title,
        channel,
        category,
        tags,
        text,
        createdAt: Date.now()
      });
      saveScripts();
      resetForm();
      render();
      toast('Added');
    }

    function seedExamples(){
      const examples = [
        {
          title: '3 fast openers (30 seconds)',
          channel: 'kupid.live',
          category: 'Openers',
          tags: ['fast','funny','cold-open'],
          text: '1) Quick question: are you the type to answer honestly or be funny first?\n\n2) You have 5 seconds to convince me you are not an NPC. Go.\n\n3) I am rating vibes 1 to 10. Say one sentence and I will lock it in.'
        },
        {
          title: 'Spicy but safe questions',
          channel: 'kupid.live',
          category: 'Questions',
          tags: ['spicy','safe','viral'],
          text: '1) What is the most unhinged green flag you have?\n2) What is one thing you are weirdly good at?\n3) If we went on a date tomorrow, what is the first place you would take me?' 
        },
        {
          title: 'Ending that pushes the next action',
          channel: 'kupid.live',
          category: 'Closing',
          tags: ['cta','close'],
          text: 'I am gonna rotate in 10 seconds. If you want to keep talking, say your best one-liner right now and I will stay. 3...2...1...'
        }
      ];

      for(const ex of examples){
        scripts.push({ id: uid(), createdAt: Date.now(), ...ex });
      }
      saveScripts();
      render();
      toast('Examples added');
    }

    function exportJson(){
      const payload = {
        version: 1,
        exportedAt: Date.now(),
        scripts
      };
      const blob = new Blob([JSON.stringify(payload, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `kupid-live-scripts-${new Date().toISOString().slice(0,10)}.json`;
      a.click();
      URL.revokeObjectURL(url);
    }

    function importJson(){
      el('file').click();
    }

    el('file').addEventListener('change', async (e) => {
      const file = e.target.files?.[0];
      if(!file) return;
      const text = await file.text();
      try{
        const data = JSON.parse(text);
        if(!data || !Array.isArray(data.scripts)) throw new Error('Invalid file');
        // Merge by id, keep newest by createdAt
        const byId = new Map(scripts.map(s => [s.id, s]));
        for(const s of data.scripts){
          if(!s?.id) continue;
          const prev = byId.get(s.id);
          if(!prev || (s.createdAt || 0) > (prev.createdAt || 0)) byId.set(s.id, s);
        }
        scripts = Array.from(byId.values());
        saveScripts();
        render();
        toast('Imported');
      }catch(err){
        toast('Import failed');
      }finally{
        el('file').value = '';
      }
    });

    function wipeAll(){
      if(!confirm('This will delete all scripts and checkoffs on this browser. Continue?')) return;
      scripts = [];
      checked = {};
      saveScripts();
      saveChecked();
      resetForm();
      render();
      toast('Wiped');
    }

    // Tiny toast
    let toastTimer;
    function toast(msg){
      clearTimeout(toastTimer);
      let t = document.getElementById('toast');
      if(!t){
        t = document.createElement('div');
        t.id = 'toast';
        t.style.position = 'fixed';
        t.style.left = '50%';
        t.style.bottom = '18px';
        t.style.transform = 'translateX(-50%)';
        t.style.background = 'rgba(0,0,0,.65)';
        t.style.border = '1px solid rgba(255,255,255,.12)';
        t.style.padding = '10px 12px';
        t.style.borderRadius = '14px';
        t.style.color = 'white';
        t.style.fontWeight = '700';
        t.style.fontSize = '13px';
        t.style.backdropFilter = 'blur(10px)';
        t.style.zIndex = 9999;
        document.body.appendChild(t);
      }
      t.textContent = msg;
      t.style.opacity = '1';
      toastTimer = setTimeout(() => { t.style.opacity = '0'; }, 1200);
    }

    function escapeHtml(str){
      return String(str)
        .replaceAll('&', '&amp;')
        .replaceAll('<', '&lt;')
        .replaceAll('>', '&gt;')
        .replaceAll('"', '&quot;')
        .replaceAll("'", '&#039;');
    }

    // Wire up
    el('addBtn').addEventListener('click', addOrSave);
    el('clearFormBtn').addEventListener('click', resetForm);
    el('seedBtn').addEventListener('click', seedExamples);
    el('exportBtn').addEventListener('click', exportJson);
    el('importBtn').addEventListener('click', importJson);
    el('wipeBtn').addEventListener('click', wipeAll);

    ['search','filterChannel','filterCategory','sort'].forEach(id => {
      el(id).addEventListener('input', render);
      el(id).addEventListener('change', render);
    });

    // Initial
    render();
  </script>
</body>
</html>
