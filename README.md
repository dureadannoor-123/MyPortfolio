
Replace `your-username`, `your-repo-name`, and contact details accordingly.

---

## 3) Simple interactive `index.html` (drop into repo)

Save this as `index.html`. It’s a single file with project cards, filter buttons, modal popup, and a theme toggle.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Dure — Portfolio</title>
<style>
  :root{
    --bg:#FDF6F4; --card:#fff; --accent:#DC6E87; --text:#222;
  }
  [data-theme="dark"]{
    --bg:#0f1720; --card:#0b1220; --accent:#ff6e9e; --text:#e6eef8;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:system-ui,Segoe UI,Roboto,Helvetica,Arial; background:var(--bg); color:var(--text); -webkit-font-smoothing:antialiased;}
  header{display:flex;align-items:center;justify-content:space-between;padding:20px 24px;}
  h1{margin:0;font-weight:700;letter-spacing:-0.5px}
  .controls{display:flex;gap:10px;align-items:center}
  button, .chip{cursor:pointer;border:0;padding:8px 12px;border-radius:999px;background:transparent}
  .chip{border:1px solid rgba(0,0,0,0.08); background:var(--card)}
  .wrap{max-width:1000px;margin:20px auto;padding:0 16px}
  .filters{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:18px}
  .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px}
  .card{background:var(--card);padding:14px;border-radius:12px;box-shadow:0 6px 18px rgba(0,0,0,0.06);cursor:pointer;transition:transform .15s}
  .card:hover{transform:translateY(-6px)}
  .tags{display:flex;gap:8px;margin-top:8px;flex-wrap:wrap}
  .tag{font-size:12px;padding:6px 8px;border-radius:8px;border:1px solid rgba(0,0,0,0.06)}
  .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:linear-gradient(0deg,rgba(2,6,23,0.6),rgba(2,6,23,0.6))}
  .modal .box{background:var(--card);padding:20px;border-radius:12px;max-width:740px;width:94%}
  .close{float:right;border:0;background:none;font-size:18px}
  footer{text-align:center;padding:28px 0;color:rgba(0,0,0,0.5)}
  .theme-toggle{border-radius:8px;padding:6px 8px}
  @media (max-width:600px){header{padding:12px}}
</style>
</head>
<body>
<header>
  <div>
    <h1>Dure Adan Noor</h1>
    <div style="font-size:13px;color:rgba(0,0,0,0.6)">AI student • Developer • Designer</div>
  </div>
  <div class="controls">
    <button class="chip" onclick="location.href='#projects'">Projects</button>
    <button class="theme-toggle" id="themeBtn">🌙</button>
  </div>
</header>

<main class="wrap">
  <section id="about" style="margin-bottom:22px">
    <h2 style="margin:0 0 6px 0">About</h2>
    <p style="margin:0 0 10px 0;max-width:70ch">I’m an AI student building practical projects — from data apps to UI. This page shows interactive demos and links to code. Click any project card to open details.</p>
  </section>

  <section id="projects">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
      <h2 style="margin:0">Projects</h2>
      <div style="font-size:13px;color:rgba(0,0,0,0.6)">Click a card — try filters</div>
    </div>

    <div class="filters" id="filters">
      <button class="chip" data-filter="all">All</button>
      <button class="chip" data-filter="web">Web</button>
      <button class="chip" data-filter="ml">ML</button>
      <button class="chip" data-filter="mobile">Mobile</button>
    </div>

    <div class="grid" id="grid">
      <!-- project card example -->
      <div class="card" data-type="ml" data-title="Period Tracker App" data-desc="A period tracking app UI built with CustomTkinter and Python. Includes calendar, notifications, and cycle settings.">
        <strong>Period Tracker App</strong>
        <div style="font-size:13px;color:rgba(0,0,0,0.6);margin-top:6px">Python • Tkinter • UI</div>
        <div class="tags">
          <span class="tag">Python</span><span class="tag">UI</span><span class="tag">Healthcare</span>
        </div>
      </div>

      <div class="card" data-type="web" data-title="SurakhshaAI UI" data-desc="Interactive UI for SurakhshaAI built with HTML/CSS/JS and Vercel deployment.">
        <strong>SurakhshaAI UI</strong>
        <div style="font-size:13px;color:rgba(0,0,0,0.6);margin-top:6px">HTML • JS</div>
        <div class="tags">
          <span class="tag">Frontend</span><span class="tag">Vercel</span>
        </div>
      </div>

      <div class="card" data-type="mobile" data-title="Disaster Alert App" data-desc="Prototype for flood-risk alerts with ML model demo, map routes and local language alerts.">
        <strong>Disaster Alert App</strong>
        <div style="font-size:13px;color:rgba(0,0,0,0.6);margin-top:6px">React Native • ML</div>
        <div class="tags">
          <span class="tag">Maps</span><span class="tag">ML</span>
        </div>
      </div>

      <!-- add more cards here -->
    </div>
  </section>
</main>

<!-- Modal -->
<div class="modal" id="modal">
  <div class="box" role="dialog" aria-modal="true">
    <button class="close" id="closeBtn">✕</button>
    <h3 id="mTitle"></h3>
    <p id="mDesc" style="color:rgba(0,0,0,0.7)"></p>
    <p style="margin-top:12px">
      <a id="codeLink" href="#" target="_blank">View code</a> •
      <a id="liveLink" href="#" target="_blank">Live demo</a>
    </p>
  </div>
</div>

<footer>
  Built with ❤️ • <a href="mailto:your.email@example.com">Contact me</a>
</footer>

<script>
  // Theme toggle
  const themeBtn = document.getElementById('themeBtn');
  function setTheme(t){
    document.documentElement.setAttribute('data-theme', t);
    themeBtn.textContent = t === 'dark' ? '☀️' : '🌙';
    localStorage.setItem('theme', t);
  }
  const saved = localStorage.getItem('theme') || (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark': 'light');
  setTheme(saved);
  themeBtn.addEventListener('click', ()=> setTheme(document.documentElement.getAttribute('data-theme') === 'dark' ? 'light' : 'dark'));

  // Filters
  document.getElementById('filters').addEventListener('click', (e)=>{
    const f = e.target.dataset?.filter;
    if(!f) return;
    document.querySelectorAll('.chip').forEach(c=> c.style.opacity = c.dataset.filter === f ? '1' : '0.7');
    document.querySelectorAll('#grid .card').forEach(card=>{
      card.style.display = (f==='all' || card.dataset.type === f) ? '' : 'none';
    });
  });

  // Modal open
  const modal = document.getElementById('modal');
  const mTitle = document.getElementById('mTitle');
  const mDesc = document.getElementById('mDesc');
  const codeLink = document.getElementById('codeLink');
  const liveLink = document.getElementById('liveLink');
  document.getElementById('grid').addEventListener('click', (e)=>{
    const card = e.target.closest('.card');
    if(!card) return;
    mTitle.textContent = card.dataset.title;
    mDesc.textContent = card.dataset.desc;
    codeLink.href = '#'; // replace with repo link
    liveLink.href = '#'; // replace with demo link
    modal.style.display = 'flex';
  });
  document.getElementById('closeBtn').addEventListener('click', ()=> modal.style.display='none');
  modal.addEventListener('click', (e)=> { if(e.target === modal) modal.style.display='none'; });
</script>
</body>
</html>
