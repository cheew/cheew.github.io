
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Start Page (Vanilla + Icons)</title>
  <meta name="color-scheme" content="light dark">
  <style>
    :root {
      --bg: #f6f6f7;
      --card: rgba(255,255,255,0.9);
      --text: #111;
      --muted: #6b7280;
      --border: rgba(0,0,0,0.12);
      --shadow: 0 1px 2px rgba(0,0,0,0.06), 0 8px 24px rgba(0,0,0,0.08);
      --accent: #111;
      --accent-contrast: #fff;
    }
    html[data-theme="dark"] {
      --bg: #0b0b0d;
      --card: rgba(17,17,19,0.9);
      --text: #f5f5f6;
      --muted: #9ca3af;
      --border: rgba(255,255,255,0.15);
      --shadow: 0 1px 2px rgba(0,0,0,0.4), 0 8px 24px rgba(0,0,0,0.3);
      --accent: #fff;
      --accent-contrast: #111;
    }
    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body { margin: 0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; background: var(--bg); color: var(--text); }
    a { color: inherit; }
    .container { max-width: 1120px; margin: 0 auto; padding: 32px 16px; position: relative; }
    .header { display:flex; justify-content: space-between; align-items:center; margin-bottom: 24px; }
    .title { font-size: 24px; font-weight: 700; }
    .sub { font-size: 13px; color: var(--muted); }
    .btn { display:inline-flex; align-items:center; gap:8px; border:1px solid var(--border); background: var(--card); padding:10px 12px; border-radius: 14px; font-size: 14px; cursor: pointer; box-shadow: var(--shadow); }
    .btn:hover { filter: brightness(0.98); }
    .row { display:grid; grid-template-columns: 1fr; gap: 12px; margin-bottom: 24px; }
    @media (min-width: 900px) { .row { grid-template-columns: 1fr 1fr; } }
    .search { display:flex; align-items:center; gap:8px; border:1px solid var(--border); background: var(--card); padding:8px; border-radius: 16px; box-shadow: var(--shadow); }
    .chip { font-size: 12px; padding:6px 8px; background:#e5e7eb; color:#111; border-radius: 10px; }
    html[data-theme="dark"] .chip { background:#1f2937; color:#e5e7eb; }
    .search input { flex:1; border:0; background: transparent; color: var(--text); outline:none; font-size:14px; padding:6px; }
    .search button { border:0; background: var(--accent); color: var(--accent-contrast); border-radius: 10px; padding:8px 12px; font-weight:600; cursor:pointer; }
    .grid { display:grid; gap:16px; grid-template-columns: 1fr; }
    @media (min-width: 900px) { .grid { grid-template-columns: 1fr 1fr; } }
    .card { border:1px solid var(--border); background: var(--card); box-shadow: var(--shadow); border-radius: 22px; padding:16px; }
    .card h2 { margin:0 0 10px; font-size: 16px; }
    .link { display:flex; align-items:center; justify-content:space-between; gap:8px; border:1px solid var(--border); background: rgba(255,255,255,0.6); padding:10px 12px; border-radius: 14px; }
    html[data-theme="dark"] .link { background: rgba(0,0,0,0.3); }
    .tools { opacity:0; transition: .2s; display:flex; gap:8px; }
    .link:hover .tools { opacity:1; }
    .danger { color: #e11d48; border-color: rgba(225,29,72,0.3); }
    .muted { color: var(--muted); font-size: 13px; }
    .favicon { width: 18px; height: 18px; border-radius: 4px; flex: 0 0 18px; }
    .link-title { text-decoration:none; font-weight:600; overflow:hidden; text-overflow:ellipsis; white-space:nowrap; max-width:360px; }
    /* Modal */
    .modal-backdrop { position: fixed; inset:0; background: rgba(0,0,0,0.45); display:none; }
    .modal { position: fixed; inset:0; display:none; align-items:center; justify-content:center; padding:16px; }
    .modal.show, .modal-backdrop.show { display:flex; }
    .modal .panel { background: var(--bg); color: var(--text); border:1px solid var(--border); border-radius: 22px; max-height: 90vh; overflow:auto; width: min(1024px, 100%); padding: 16px; box-shadow: var(--shadow); }
    .row3 { display:grid; gap:12px; grid-template-columns: 1fr; }
    @media (min-width: 1100px) { .row3 { grid-template-columns: 1fr 1fr 1fr; } }
    .input, .select { width: 100%; border:1px solid var(--border); background: var(--card); color: var(--text); border-radius: 12px; padding:10px; font-size:14px; }
    .small { font-size: 12px; color: var(--muted); }
    .handle { opacity: .6; cursor: grab; }
    .bg-image, .bg-dim { position: fixed; inset: 0; z-index: -2; }
    .bg-dim { z-index: -1; background: transparent; }
    .hidden { display:none !important; }
    .icon-toggle { display:flex; align-items:center; gap:8px; margin-top:8px; }
  </style>
</head>
<body>
  <div class="bg-image hidden"><img id="bgImg" alt="bg" style="width:100%;height:100%;object-fit:cover;filter:blur(0px)"></div>
  <div class="bg-dim hidden" id="bgDim"></div>

  <div class="container">
    <div class="header">
      <div>
        <div class="title">Home</div>
        <div class="sub" id="clock"></div>
      </div>
      <div style="display:flex; gap:8px;">
        <button class="btn" id="toggleTheme">Dark</button>
        <button class="btn" id="openOptions">Options</button>
      </div>
    </div>

    <div class="row">
      <form class="search" id="googleForm">
        <span class="chip">Google</span>
        <input id="googleInput" placeholder="Search the web">
        <button>Search</button>
      </form>
      <form class="search" id="aiForm">
        <span class="chip">AI</span>
        <input id="aiInput" placeholder="Ask ChatGPT | Perplexity">
        <button>Ask</button>
      </form>
    </div>

    <div class="grid" id="sections"></div>
  </div>

  <!-- Modal -->
  <div class="modal-backdrop" id="backdrop"></div>
  <div class="modal" id="modal">
    <div class="panel">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
        <div style="font-weight:700;">Options</div>
        <div style="display:flex;gap:8px;">
          <button class="btn" id="exportBtn">Export</button>
          <button class="btn" id="importBtn">Import</button>
          <button class="btn" id="closeOptions">Close</button>
        </div>
      </div>

      <div class="row3" style="margin-bottom:16px;">
        <div class="card">
          <div style="font-weight:600;margin-bottom:8px;">AI</div>
          <div style="display:flex; gap:12px; flex-wrap:wrap; margin-bottom:8px; font-size:14px;">
            <label><input type="radio" name="provider" value="chatgpt"> ChatGPT</label>
            <label><input type="radio" name="provider" value="perplexity"> Perplexity</label>
          </div>
          <div style="display:flex; gap:12px; flex-wrap:wrap; margin-bottom:8px; font-size:14px;">
            <label><input type="radio" name="aimode" value="site"> Open provider site</label>
            <label><input type="radio" name="aimode" value="api"> Use ChatGPT API</label>
          </div>
          <input id="apiKey" class="input" type="password" placeholder="sk-... your OpenAI API key">
          <div class="small" style="margin-top:6px;">Key stays in your browser storage.</div>
          <div style="margin-top:8px;"><button class="btn" id="testKey">Test key</button></div>
        </div>

        <div class="card">
          <div style="font-weight:600;margin-bottom:8px;">Layout</div>
          <label style="display:flex;align-items:center;gap:8px;"><input type="checkbox" id="compact"> Compact spacing</label>
          <label style="display:flex;align-items:center;gap:8px;margin-top:8px;"><input type="checkbox" id="dark"> Dark mode</label>
          <label class="icon-toggle"><input type="checkbox" id="showIcons"> Show site icons</label>
          <div class="small" style="margin-top:8px;">Shortcuts: g focuses Google, a focuses AI.</div>
        </div>

        <div class="card">
          <div style="font-weight:600;margin-bottom:8px;">Background</div>
          <div style="display:flex; gap:12px; flex-wrap:wrap; margin-bottom:8px; font-size:14px;">
            <label><input type="radio" name="bgkind" value="gradient"> Gradient</label>
            <label><input type="radio" name="bgkind" value="color"> Solid</label>
            <label><input type="radio" name="bgkind" value="image"> Image</label>
          </div>
          <div id="bgControls"></div>
        </div>
      </div>

      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
        <div style="font-weight:600;">Sections & links</div>
        <button class="btn" id="addSection"><span>＋</span> Add section</button>
      </div>

      <div id="editor"></div>
    </div>
  </div>

  <script>
    const STORAGE = {
      sections: "startpage.sections.v3",
      settings: "startpage.settings.v3",
    };

    const defaultSections = [
      { id: uid(), title: "Daily", links: [
        { id: uid(), label: "Gmail", url: "https://mail.google.com" },
        { id: uid(), label: "YouTube", url: "https://youtube.com" },
        { id: uid(), label: "Drive", url: "https://drive.google.com" },
        { id: uid(), label: "Calendar", url: "https://calendar.google.com" },
      ]},
      { id: uid(), title: "Build", links: [
        { id: uid(), label: "GitHub", url: "https://github.com" },
        { id: uid(), label: "Stack Overflow", url: "https://stackoverflow.com" },
        { id: uid(), label: "ChatGPT", url: "https://chatgpt.com" },
      ]}
    ];

    const defaultSettings = {
      aiProvider: "chatgpt",
      aiMode: "site",
      apiKey: "",
      compact: false,
      showIcons: true,
      dark: window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches,
      background: { kind: "gradient", gradient: "linear-gradient(135deg, #fafafa, #e5e7eb)" }
    };

    let sections = read(STORAGE.sections, defaultSections);
    let settings = read(STORAGE.settings, defaultSettings);

    applyTheme();
    applyBackground();

    // Clock
    const clockEl = document.getElementById("clock");
    function tickClock(){
      const d = new Date();
      const days = ["Sun","Mon","Tue","Wed","Thu","Fri","Sat"];
      const hh = String(d.getHours()).padStart(2,"0");
      const mm = String(d.getMinutes()).padStart(2,"0");
      clockEl.textContent = days[d.getDay()]+" "+hh+":"+mm;
    }
    tickClock(); setInterval(tickClock, 30000);

    const sectionsEl = document.getElementById("sections");
    function renderSections(){
      sectionsEl.innerHTML = "";
      sectionsEl.style.gridTemplateColumns = settings.compact ? "repeat(3,1fr)" : "repeat(2,1fr)";
      if (window.innerWidth < 900) sectionsEl.style.gridTemplateColumns = "1fr";
      sections.forEach((s, sIdx) => {
        const card = el("div",{class:"card"});
        const head = el("div",{style:"display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;"});
        head.append(el("h2",{}, txt(s.title)), el("span",{class:"muted"}, txt("Edit in Options")));
        card.append(head);
        if (s.links.length === 0) {
          card.append(el("div",{class:"muted"}, txt("No links yet")));
        } else {
          const list = el("div",{style:"display:grid;gap:10px;grid-template-columns:1fr 1fr;"});
          if (window.innerWidth < 900) list.style.gridTemplateColumns = "1fr";
          s.links.forEach((lnk, idx) => {
            const row = el("div",{class:"link", draggable:"true"});
            const left = el("div",{style:"display:flex;align-items:center;gap:8px;"});
            const handle = el("span",{class:"handle", title:"Drag to reorder"}, txt("⋮⋮"));
            if (settings.showIcons) {
              const host = hostname(lnk.url);
              const iconUrl = host ? `https://www.google.com/s2/favicons?domain=${encodeURIComponent(host)}&sz=64` : "";
              const img = el("img",{class:"favicon", src: iconUrl, alt:""});
              img.onerror = () => { img.style.visibility = "hidden"; };
              left.append(img);
            }
            const a = el("a",{href: lnk.url, target:"_blank", rel:"noreferrer", class:"link-title"}, txt(lnk.label));
            left.append(handle, a);
            const tools = el("div",{class:"tools"});
            const del = el("button",{class:"btn danger", onclick:()=>{ s.links.splice(idx,1); save(); renderSections(); renderEditor(); }}, txt("Delete"));
            tools.append(del);
            row.append(left, tools);
            list.append(row);
          });
          card.append(list);
        }
        sectionsEl.append(card);
      });
    }
    window.addEventListener("resize", renderSections);

    // Options modal
    const modal = document.getElementById("modal");
    const backdrop = document.getElementById("backdrop");
    document.getElementById("openOptions").onclick = () => showModal(true);
    document.getElementById("closeOptions").onclick = () => showModal(false);
    backdrop.onclick = () => showModal(false);
    function showModal(v){
      modal.classList.toggle("show", v);
      backdrop.classList.toggle("show", v);
      if (v) { renderEditor(); syncOptionsUI(); }
    }

    function syncOptionsUI(){
      setRadio("provider", settings.aiProvider);
      setRadio("aimode", settings.aiMode);
      document.getElementById("apiKey").value = settings.apiKey;
      document.getElementById("compact").checked = settings.compact;
      document.getElementById("dark").checked = settings.dark;
      document.getElementById("showIcons").checked = settings.showIcons;
      setRadio("bgkind", settings.background.kind);
      renderBgControls();
    }

    function renderBgControls(){
      const box = document.getElementById("bgControls");
      box.innerHTML = "";
      if (settings.background.kind === "color"){
        const color = input("color", settings.background.color || "#f6f6f7", (v)=>{ settings.background.color=v; save(); applyBackground(); });
        const text = input("text", settings.background.color || "#f6f6f7", (v)=>{ settings.background.color=v; save(); applyBackground(); });
        box.append(el("div",{}, txt("Pick color: ")), color, el("div",{style:"height:6px"}), text);
      } else if (settings.background.kind === "gradient"){
        const grad = input("text", settings.background.gradient, (v)=>{ settings.background.gradient=v; save(); applyBackground(); });
        grad.placeholder = "CSS gradient, e.g. linear-gradient(135deg,#fafafa,#e5e7eb)";
        box.append(el("div",{}, txt("CSS gradient:")), grad);
      } else {
        const url = input("text", settings.background.url || "https://images.unsplash.com/photo-1501785888041-af3ef285b470?q=80&w=1920", (v)=>{ settings.background.url=v; save(); applyBackground(); });
        const blur = range(0,10, settings.background.blur ?? 2, (v)=>{ settings.background.blur=+v; save(); applyBackground(); });
        const dim = range(0,0.8, settings.background.dim ?? 0.2, (v)=>{ settings.background.dim=+v; save(); applyBackground(); }, 0.05);
        box.append(el("div",{}, txt("Image URL:")), url, el("div",{}, txt("Blur:")), blur, el("div",{}, txt("Dim:")), dim);
      }
    }

    // Editor
    const editor = document.getElementById("editor");
    function renderEditor(){
      editor.innerHTML = "";
      sections.forEach((s, sIdx) => {
        const wrap = el("div",{class:"card", draggable:"true"});
        wrap.addEventListener("dragstart", (e)=>{ e.dataTransfer.setData("text/section", String(sIdx)); });
        wrap.addEventListener("dragover", (e)=> e.preventDefault());
        wrap.addEventListener("drop", (e)=>{
          e.preventDefault();
          const fromStr = e.dataTransfer.getData("text/section");
          if (fromStr){
            const from = +fromStr;
            if (!Number.isNaN(from) && from !== sIdx){
              const next = [...sections];
              const [moved] = next.splice(from,1);
              next.splice(sIdx,0,moved);
              sections = next; save(); renderEditor(); renderSections();
            }
          }
        });
        const bar = el("div",{style:"display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;"});
        const left = el("div",{style:"display:flex;align-items:center;gap:8px;"});
        left.append(el("span",{class:"handle", title:"Drag to reorder sections"}, txt("⋮⋮")));
        const title = input("text", s.title, (v)=>{ s.title=v; save(); renderSections(); }); title.className = "input";
        left.append(title);
        const del = button("Delete", ()=>{ sections.splice(sIdx,1); save(); renderEditor(); renderSections(); }, "btn danger");
        bar.append(left, del);
        wrap.append(bar);

        const addRow = el("div",{style:"display:grid;grid-template-columns: 1fr 2fr auto; gap:8px; margin-bottom:8px;"});
        const nameI = input("text","",()=>{}); nameI.placeholder="Link name"; nameI.className="input";
        const urlI = input("text","",()=>{}); urlI.placeholder="https://example.com"; urlI.className="input";
        const addBtn = button("Add", ()=>{
          const l = nameI.value.trim(); const u = urlI.value.trim();
          if(!l||!u) return;
          s.links.push({ id: uid(), label: l, url: u });
          nameI.value=""; urlI.value="";
          save(); renderEditor(); renderSections();
        });
        addRow.append(nameI, urlI, addBtn);
        wrap.append(addRow);

        const list = el("div",{style:"display:grid;gap:8px;grid-template-columns:1fr 1fr;"});
        if (window.innerWidth < 900) list.style.gridTemplateColumns = "1fr";
        s.links.forEach((lnk, li) => {
          const item = el("div",{class:"link", draggable:"true"});
          item.addEventListener("dragstart", (e)=>{ e.dataTransfer.setData("text/link", JSON.stringify({sIdx, from: li})); });
          item.addEventListener("dragover", (e)=> e.preventDefault());
          item.addEventListener("drop", (e)=>{
            e.preventDefault();
            const payload = e.dataTransfer.getData("text/link");
            if (payload){
              const { sIdx: fromS, from } = JSON.parse(payload);
              const to = li;
              if (fromS === sIdx){
                const arr = sections[sIdx].links;
                const [moved] = arr.splice(from,1);
                arr.splice(to,0,moved);
                save(); renderEditor(); renderSections();
              }
            }
          });
          const name = input("text", lnk.label, (v)=>{ lnk.label=v; save(); renderSections(); }); name.className="input";
          const url = input("text", lnk.url, (v)=>{ lnk.url=v; save(); renderSections(); }); url.className="input";
          const rm = button("Delete", ()=>{ s.links.splice(li,1); save(); renderEditor(); renderSections(); }, "btn danger");
          const grid = el("div",{style:"display:grid;grid-template-columns: 1fr 2fr auto; gap:8px; width:100%;"});
          grid.append(name, url, rm);
          item.append(grid);
          list.append(item);
        });
        wrap.append(list);

        editor.append(wrap);
      });
    }
    document.getElementById("addSection").onclick = ()=>{ sections.push({ id: uid(), title: "New Section", links: [] }); save(); renderEditor(); renderSections(); };

    // Export / Import
    document.getElementById("exportBtn").onclick = ()=>{
      const safe = { sections, settings: { ...settings, apiKey: "" } };
      const data = new Blob([JSON.stringify(safe,null,2)], { type: "application/json" });
      const url = URL.createObjectURL(data);
      const a = document.createElement("a");
      a.href = url; a.download = "startpage.json"; a.click();
      URL.revokeObjectURL(url);
    };
    document.getElementById("importBtn").onclick = ()=>{
      const inputF = document.createElement("input");
      inputF.type = "file"; inputF.accept = "application/json";
      inputF.onchange = async () => {
        const file = inputF.files?.[0]; if (!file) return;
        const text = await file.text();
        try {
          const parsed = JSON.parse(text);
          if (parsed.sections) sections = parsed.sections;
          if (parsed.settings) settings = { ...settings, ...parsed.settings, apiKey: settings.apiKey };
          save(); syncOptionsUI(); renderEditor(); renderSections(); applyTheme(); applyBackground();
        } catch { alert("Invalid JSON"); }
      };
      inputF.click();
    };

    // Settings interactions
    document.getElementById("toggleTheme").onclick = ()=>{ settings.dark = !settings.dark; save(); applyTheme(); };
    document.getElementById("dark").onchange = (e)=>{ settings.dark = e.target.checked; save(); applyTheme(); };
    document.getElementById("compact").onchange = (e)=>{ settings.compact = e.target.checked; save(); renderSections(); };
    document.getElementById("showIcons").onchange = (e)=>{ settings.showIcons = e.target.checked; save(); renderSections(); };

    document.getElementsByName("provider").forEach(r=> r.onchange = (e)=>{ settings.aiProvider = e.target.value; save(); syncAIPlaceholder(); });
    document.getElementsByName("aimode").forEach(r=> r.onchange = (e)=>{ settings.aiMode = e.target.value; save(); syncAIPlaceholder(); });
    document.getElementsByName("bgkind").forEach(r=> r.onchange = (e)=>{ settings.background.kind = e.target.value; save(); renderBgControls(); applyBackground(); });

    document.getElementById("apiKey").oninput = (e)=>{ settings.apiKey = e.target.value; save(); };

    // Search forms
    document.getElementById("googleForm").onsubmit = (e)=>{
      e.preventDefault();
      const q = document.getElementById("googleInput").value.trim();
      if (!q) return;
      window.open("https://www.google.com/search?q="+encodeURIComponent(q), "_blank");
    };
    document.getElementById("aiForm").onsubmit = async (e)=>{
      e.preventDefault();
      const q = document.getElementById("aiInput").value.trim();
      if (!q) return;
      if (settings.aiMode === "site" || settings.aiProvider === "perplexity"){
        const url = settings.aiProvider === "perplexity"
          ? "https://www.perplexity.ai/search?q="+encodeURIComponent(q)
          : "https://chat.openai.com/?q="+encodeURIComponent(q);
        window.open(url, "_blank");
      } else {
        if (!settings.apiKey) { alert("Add your OpenAI API key in Options."); return; }
        try {
          const res = await fetch("https://api.openai.com/v1/chat/completions", {
            method: "POST",
            headers: { "Content-Type":"application/json", "Authorization":"Bearer "+settings.apiKey },
            body: JSON.stringify({ model:"gpt-4o-mini", messages:[{role:"user", content:q}], temperature:0.2 })
          });
          const data = await res.json();
          alert((data.choices && data.choices[0] && data.choices[0].message && data.choices[0].message.content) || "No content");
        } catch(err){
          alert("API request failed: "+err);
        }
      }
    };
    function syncAIPlaceholder(){
      const aiI = document.getElementById("aiInput");
      aiI.placeholder = settings.aiMode === "api" ? "Ask ChatGPT (API)" : "Ask ChatGPT | Perplexity";
    }

    // Shortcuts
    window.addEventListener("keydown", (e)=>{
      const tag = document.activeElement && document.activeElement.tagName;
      if (tag === "INPUT" || tag === "TEXTAREA") return;
      if (e.key.toLowerCase() === "g") document.getElementById("googleInput").focus();
      if (e.key.toLowerCase() === "a") document.getElementById("aiInput").focus();
    });

    // Helpers
    function hostname(u){ try { return new URL(u).hostname; } catch { return ""; } }
    function uid(){ return Math.random().toString(36).slice(2,10); }
    function read(k, d){ try{ const v = localStorage.getItem(k); return v ? JSON.parse(v) : d; }catch{ return d; } }
    function save(){ localStorage.setItem(STORAGE.sections, JSON.stringify(sections)); localStorage.setItem(STORAGE.settings, JSON.stringify(settings)); }
    function el(tag, attrs={}, ...children){
      const node = document.createElement(tag);
      if (attrs) for (const k in attrs){
        if (k === "class") node.className = attrs[k];
        else if (k === "style") node.setAttribute("style", attrs[k]);
        else if (k === "onclick") node.onclick = attrs[k];
        else if (k === "draggable") node.draggable = attrs[k];
        else node.setAttribute(k, attrs[k]);
      }
      children.forEach(c => node.append(c instanceof Node ? c : document.createTextNode(c)));
      return node;
    }
    function txt(s){ return document.createTextNode(s); }
    function input(type, value, oninput){
      const i = document.createElement("input");
      i.type = type; i.value = value;
      i.oninput = (e)=> oninput(e.target.value);
      i.className = "input";
      return i;
    }
    function range(min,max,val,oninput,step=1){
      const i = document.createElement("input");
      i.type = "range"; i.min=min; i.max=max; i.step=step; i.value = val;
      i.oninput = (e)=> oninput(e.target.value);
      i.style.width = "100%";
      return i;
    }
    function button(label, onclick, cls="btn"){
      const b = document.createElement("button"); b.textContent = label; b.onclick = onclick; b.className = cls; return b;
    }
    function setRadio(name, value){
      document.getElementsByName(name).forEach(r => r.checked = (r.value === value));
    }

    function applyTheme(){
      document.documentElement.setAttribute("data-theme", settings.dark ? "dark" : "light");
      document.getElementById("toggleTheme").textContent = settings.dark ? "Light" : "Dark";
    }
    function applyBackground(){
      const kind = settings.background.kind;
      const body = document.body;
      const imgWrap = document.querySelector(".bg-image");
      const img = document.getElementById("bgImg");
      const dim = document.getElementById("bgDim");
      if (kind === "color"){
        body.style.background = settings.background.color || "#f6f6f7";
        imgWrap.classList.add("hidden");
        dim.classList.add("hidden");
      } else if (kind === "gradient"){
        body.style.background = settings.background.gradient || "linear-gradient(135deg, #fafafa, #e5e7eb)";
        imgWrap.classList.add("hidden");
        dim.classList.add("hidden");
      } else {
        body.style.background = "";
        imgWrap.classList.remove("hidden");
        dim.classList.remove("hidden");
        img.src = settings.background.url || "";
        img.style.filter = "blur(" + ((settings.background.blur??2)|0) + "px)";
        dim.style.background = "rgba(0,0,0," + (settings.background.dim??0.2) + ")";
      }
    }

    document.getElementById("testKey").onclick = async () => {
      if (!settings.apiKey) { alert("Add your key first."); return; }
      try {
        const res = await fetch("https://api.openai.com/v1/models", { headers: { "Authorization":"Bearer "+settings.apiKey } });
        if (res.ok) alert("Key looks valid.");
        else alert("Key error: HTTP " + res.status);
      } catch(err){ alert("Network error: "+err); }
    };

    renderSections();
    syncAIPlaceholder();
  </script>
</body>
</html>
