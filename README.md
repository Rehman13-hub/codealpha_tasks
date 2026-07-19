<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>image gallery </title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@400;500;600;700&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
<style>
  :root{
    --paper: #EDE7DA;
    --paper-dim: #E2DBCB;
    --ink: #1C1A17;
    --film: #0D0C0B;
    --safelight: #B23A2E;
    --grey: #8A8478;
    --amber: #D9A441;
    --frame-gap: 14px;
  }
 
  * { box-sizing: border-box; }
 
  html, body {
    margin: 0;
    padding: 0;
    background: var(--film);
    color: var(--ink);
    font-family: 'Space Mono', monospace;
  }
 
  .sheet {
    max-width: 1180px;
    margin: 0 auto;
    background: var(--paper);
    background-image:
      repeating-linear-gradient(0deg, transparent, transparent 39px, rgba(28,26,23,0.025) 39px, rgba(28,26,23,0.025) 40px);
    min-height: 100vh;
    padding: 0 0 60px 0;
  }
 
  
  header {
    padding: 42px 48px 24px;
    border-bottom: 3px solid var(--ink);
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    flex-wrap: wrap;
    gap: 16px;
  }
 
  .masthead h1 {
    font-family: 'Oswald', sans-serif;
    font-weight: 700;
    letter-spacing: 0.02em;
    font-size: clamp(2rem, 4vw, 3.2rem);
    margin: 0;
    text-transform: uppercase;
    line-height: 0.95;
  }
 
  .masthead .roll-tag {
    font-family: 'Space Mono', monospace;
    font-size: 0.78rem;
    letter-spacing: 0.12em;
    color: var(--safelight);
    text-transform: uppercase;
    margin-top: 6px;
    display: inline-block;
  }
 
  .roll-meta {
    text-align: right;
    font-size: 0.75rem;
    color: var(--grey);
    line-height: 1.6;
    letter-spacing: 0.03em;
  }
  .roll-meta strong { color: var(--ink); }
 
  
  .tabs {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    padding: 22px 48px 4px;
  }
 
  .tab {
    position: relative;
    font-family: 'Oswald', sans-serif;
    font-size: 0.85rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    background: var(--ink);
    color: var(--paper);
    border: none;
    padding: 9px 20px;
    cursor: pointer;
    clip-path: polygon(6px 0, 100% 0, calc(100% - 6px) 100%, 0 100%);
    transition: transform 0.2s ease, background 0.2s ease;
  }
 
  .tab:hover { transform: translateY(-2px); background: #333029; }
 
  .tab.active { background: var(--paper); color: var(--ink); }
 
  .tab.active::after {
    content: "";
    position: absolute;
    inset: -8px -10px;
    border: 2.5px solid var(--safelight);
    border-radius: 50%;
    transform: rotate(-4deg);
    pointer-events: none;
    opacity: 0.85;
  }
 
  
  .filmstrip {
    display: flex;
    align-items: stretch;
    margin: 28px 0 0;
  }
 
  .sprockets {
    flex: 0 0 22px;
    background: var(--film);
    background-image: repeating-linear-gradient(
      0deg,
      var(--paper-dim) 0 10px,
      var(--film) 10px 26px
    );
    background-size: 100% 26px;
    position: relative;
  }
  .sprockets::before, .sprockets::after {
    content: "";
    position: absolute;
    inset: 0;
    background-image: repeating-linear-gradient(0deg, transparent 0 8px, var(--paper) 8px 18px, transparent 18px 26px);
    background-size: 100% 26px;
    opacity: 0;
  }
 
  .grid {
    flex: 1;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--frame-gap);
    padding: 0 22px;
  }
 
  .frame {
    position: relative;
    background: var(--film);
    padding: 8px 8px 0;
    cursor: pointer;
    transition: transform 0.35s cubic-bezier(.2,.8,.2,1);
    opacity: 0;
    animation: develop 0.6s ease forwards;
  }
 
  @keyframes develop {
    from { opacity: 0; filter: brightness(0.4) contrast(1.4); transform: translateY(6px); }
    to   { opacity: 1; filter: brightness(1) contrast(1); transform: translateY(0); }
  }
 
  .frame:hover { transform: translateY(-5px); }
 
  .frame-img-wrap {
    position: relative;
    overflow: hidden;
    aspect-ratio: 4 / 3;
    background: #000;
  }
 
  .frame img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    filter: grayscale(1) contrast(1.05);
    transition: transform 0.6s cubic-bezier(.2,.8,.2,1), filter 0.5s ease;
  }
 
  .frame:hover img {
    transform: scale(1.08);
    filter: grayscale(0.15) contrast(1.08);
  }
 
  .frame-glow {
    position: absolute;
    inset: 0;
    box-shadow: inset 0 0 0 0 var(--amber);
    transition: box-shadow 0.35s ease;
    pointer-events: none;
  }
  .frame:hover .frame-glow {
    box-shadow: inset 0 0 0 3px var(--amber);
  }
 
  .frame-caption {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    padding: 8px 2px 10px;
    color: var(--paper);
    font-size: 0.68rem;
    letter-spacing: 0.03em;
  }
  .frame-caption .fno { color: var(--safelight); font-weight: 700; }
  .frame-caption .cat { color: var(--grey); text-transform: uppercase; }
 
  .frame.hidden { display: none; }
 
  
  @media (max-width: 880px) {
    .grid { grid-template-columns: repeat(2, 1fr); }
    header { padding: 30px 24px 20px; }
    .tabs { padding: 20px 24px 4px; }
    .grid { padding: 0 12px; }
    .sprockets { flex-basis: 14px; }
  }
 
  @media (max-width: 520px) {
    .grid { grid-template-columns: 1fr; }
    .masthead h1 { font-size: 1.7rem; }
    .roll-meta { text-align: left; }
    header { flex-direction: column; align-items: flex-start; }
  }
 
  
  .lightbox {
    position: fixed;
    inset: 0;
    background: rgba(6,5,4,0.96);
    display: none;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 100;
    padding: 24px;
  }
  .lightbox.open { display: flex; }
 
  .lb-stage {
    position: relative;
    max-width: 900px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
 
  .lb-img-wrap {
    position: relative;
    width: 100%;
    max-height: 68vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #000;
    border: 10px solid var(--paper);
  }
 
  .lb-img-wrap img {
    max-width: 100%;
    max-height: 68vh;
    display: block;
    filter: grayscale(1) contrast(1.05);
    animation: lbfade 0.35s ease;
  }
 
  @keyframes lbfade {
    from { opacity: 0; transform: scale(0.98); }
    to { opacity: 1; transform: scale(1); }
  }
 
  .lb-nav {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: transparent;
    border: none;
    cursor: pointer;
    color: var(--paper);
    font-family: 'Oswald', sans-serif;
    font-size: 2rem;
    transition: color 0.2s ease, background 0.2s ease;
  }
  .lb-nav:hover { color: var(--amber); background: rgba(255,255,255,0.04); }
  .lb-prev { left: -60px; }
  .lb-next { right: -60px; }
 
  @media (max-width: 720px) {
    .lb-prev { left: 0; }
    .lb-next { right: 0; }
    .lb-nav { background: rgba(0,0,0,0.35); width: 44px; }
  }
 
  .lb-close {
    position: absolute;
    top: -46px;
    right: 0;
    background: none;
    border: none;
    color: var(--paper);
    font-family: 'Space Mono', monospace;
    font-size: 0.85rem;
    letter-spacing: 0.1em;
    cursor: pointer;
    text-transform: uppercase;
  }
  .lb-close:hover { color: var(--safelight); }
 
  .lb-meta {
    margin-top: 16px;
    text-align: center;
    color: var(--paper);
  }
  .lb-meta .fno { color: var(--safelight); font-weight: 700; letter-spacing: 0.05em; }
  .lb-meta .cap { display: block; margin-top: 4px; font-size: 0.8rem; color: var(--grey); }
 
  .lb-strip {
    display: flex;
    gap: 8px;
    margin-top: 22px;
    max-width: 900px;
    overflow-x: auto;
    padding-bottom: 6px;
  }
  .lb-thumb {
    flex: 0 0 auto;
    width: 58px;
    height: 44px;
    border: 2px solid transparent;
    cursor: pointer;
    opacity: 0.5;
    position: relative;
    transition: opacity 0.2s ease, border 0.2s ease;
  }
  .lb-thumb img { width: 100%; height: 100%; object-fit: cover; filter: grayscale(1); display: block; }
  .lb-thumb.active { opacity: 1; border-color: var(--safelight); }
  .lb-thumb.active::after {
    content: "";
    position: absolute;
    inset: -4px;
    border: 2px solid var(--safelight);
    border-radius: 50%;
    transform: rotate(-3deg);
  }
 
  footer {
    text-align: center;
    color: var(--grey);
    font-size: 0.68rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding-top: 30px;
  }
</style>
</head>
<body>
 
<div class="sheet">
  <header>
    <div class="masthead">
      <h1>Image Gallery</h1>
      <span class="roll-tag">  &nbsp;·&nbsp; Developed by Sheikh AbdulRehman</span>
    </div>
    <div class="roll-meta">
     
    </div>
  </header>
 
  <div class="tabs" id="tabs">
    <button class="tab active" data-cat="all">All Frames</button>
    <button class="tab" data-cat="landscape">Landscape</button>
    <button class="tab" data-cat="street">Street</button>
    <button class="tab" data-cat="portrait">Portrait</button>
    <button class="tab" data-cat="architecture">Architecture</button>
  </div>
 
  <div class="filmstrip">
    <div class="sprockets"></div>
    <div class="grid" id="grid"></div>
    <div class="sprockets"></div>
  </div>
 
  <footer>— end of roll —</footer>
</div>
 
<div class="lightbox" id="lightbox">
  <div class="lb-stage">
    <button class="lb-nav lb-prev" id="lbPrev" aria-label="Previous">&#8249;</button>
    <div class="lb-img-wrap">
      <button class="lb-close" id="lbClose">Close ✕</button>
      <img id="lbImg" src="" alt="">
    </div>
    <button class="lb-nav lb-next" id="lbNext" aria-label="Next">&#8250;</button>
  </div>
  <div class="lb-meta">
    <span class="fno" id="lbFno"></span>
    <span class="cap" id="lbCap"></span>
  </div>
  <div class="lb-strip" id="lbStrip"></div>
</div>
 
<script>
  const photos = [
    { seed: 'ridge-14', cat: 'landscape', place: 'Basalt Ridge, before dawn', note: 'f/11 · 1/125' },
    { seed: 'harbor-9',  cat: 'landscape', place: 'North Harbor, low tide', note: 'f/8 · 1/250' },
    { seed: 'valley-22', cat: 'landscape', place: 'Slate Valley overlook', note: 'f/16 · 1/60' },
    { seed: 'dunes-3',   cat: 'landscape', place: 'Dunes past the treeline', note: 'f/11 · 1/250' },
    { seed: 'corner-51', cat: 'street',    place: '5th & Vine, rush hour', note: 'f/5.6 · 1/500' },
    { seed: 'market-8',  cat: 'street',    place: 'Wet market, early open', note: 'f/4 · 1/250' },
    { seed: 'transit-6', cat: 'street',    place: 'Platform 2, waiting', note: 'f/2.8 · 1/125' },
    { seed: 'alley-12',  cat: 'street',    place: 'Back alley, delivery hour', note: 'f/5.6 · 1/250' },
    { seed: 'face-41',   cat: 'portrait',  place: 'Studio, window light', note: 'f/2 · 1/200' },
    { seed: 'face-77',   cat: 'portrait',  place: 'Doorway, borrowed light', note: 'f/2.8 · 1/160' },
    { seed: 'hands-19',  cat: 'portrait',  place: 'Workshop, mid-repair', note: 'f/2.8 · 1/125' },
    { seed: 'face-63',   cat: 'portrait',  place: 'Rooftop, golden hour', note: 'f/1.8 · 1/500' },
    { seed: 'facade-30', cat: 'architecture', place: 'Old exchange building', note: 'f/9 · 1/250' },
    { seed: 'stair-27',  cat: 'architecture', place: 'Spiral stair, atrium', note: 'f/8 · 1/60' },
    { seed: 'bridge-44', cat: 'architecture', place: 'Underpass, steel ribs', note: 'f/11 · 1/125' },
    { seed: 'lobby-15',  cat: 'architecture', place: 'Terminal lobby, noon', note: 'f/9 · 1/250' }
  ];
 
  const grid = document.getElementById('grid');
  const tabs = document.getElementById('tabs');
  let active = 'all';
  let openIndex = 0;
 
  function imgUrl(seed, w, h) {
    return `https://picsum.photos/seed/${seed}/${w}/${h}`;
  }
 
  function frameNo(i) {
    return String(i + 1).padStart(2, '0') + 'A';
  }
 
  function buildGrid() {
    grid.innerHTML = '';
    photos.forEach((p, i) => {
      const el = document.createElement('div');
      el.className = 'frame';
      el.style.animationDelay = (i * 0.03) + 's';
      if (active !== 'all' && p.cat !== active) el.classList.add('hidden');
      el.innerHTML = `
        <div class="frame-img-wrap">
          <img src="${imgUrl(p.seed, 480, 360)}" alt="${p.place}" loading="lazy">
          <div class="frame-glow"></div>
        </div>
        <div class="frame-caption">
          <span class="fno">${frameNo(i)}</span>
          <span class="cat">${p.cat}</span>
        </div>
      `;
      el.addEventListener('click', () => openLightbox(i));
      grid.appendChild(el);
    });
  }
 
  tabs.addEventListener('click', (e) => {
    const btn = e.target.closest('.tab');
    if (!btn) return;
    tabs.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    btn.classList.add('active');
    active = btn.dataset.cat;
    buildGrid();
  });
 
  buildGrid();
 
  
  const lightbox = document.getElementById('lightbox');
  const lbImg = document.getElementById('lbImg');
  const lbFno = document.getElementById('lbFno');
  const lbCap = document.getElementById('lbCap');
  const lbStrip = document.getElementById('lbStrip');
 
  function visibleIndices() {
    return photos
      .map((p, i) => ({ p, i }))
      .filter(o => active === 'all' || o.p.cat === active)
      .map(o => o.i);
  }
 
  function openLightbox(i) {
    openIndex = i;
    renderLightbox();
    lightbox.classList.add('open');
    buildStrip();
  }
 
  function renderLightbox() {
    const p = photos[openIndex];
    lbImg.src = imgUrl(p.seed, 1200, 900);
    lbImg.alt = p.place;
    lbFno.textContent = 'FRAME ' + frameNo(openIndex);
    lbCap.textContent = p.place + '  ·  ' + p.note;
    lbStrip.querySelectorAll('.lb-thumb').forEach(t => {
      t.classList.toggle('active', Number(t.dataset.idx) === openIndex);
    });
  }
 
  function buildStrip() {
    const indices = visibleIndices();
    lbStrip.innerHTML = '';
    indices.forEach(i => {
      const p = photos[i];
      const t = document.createElement('div');
      t.className = 'lb-thumb' + (i === openIndex ? ' active' : '');
      t.dataset.idx = i;
      t.innerHTML = `<img src="${imgUrl(p.seed, 120, 90)}" alt="">`;
      t.addEventListener('click', () => { openIndex = i; renderLightbox(); buildStrip(); });
      lbStrip.appendChild(t);
    });
  }
 
  function step(delta) {
    const indices = visibleIndices();
    const pos = indices.indexOf(openIndex);
    const nextPos = (pos + delta + indices.length) % indices.length;
    openIndex = indices[nextPos];
    renderLightbox();
    buildStrip();
  }
 
  document.getElementById('lbPrev').addEventListener('click', () => step(-1));
  document.getElementById('lbNext').addEventListener('click', () => step(1));
  document.getElementById('lbClose').addEventListener('click', () => lightbox.classList.remove('open'));
 
  lightbox.addEventListener('click', (e) => {
    if (e.target === lightbox) lightbox.classList.remove('open');
  });
 
  document.addEventListener('keydown', (e) => {
    if (!lightbox.classList.contains('open')) return;
    if (e.key === 'Escape') lightbox.classList.remove('open');
    if (e.key === 'ArrowLeft') step(-1);
    if (e.key === 'ArrowRight') step(1);
  });
</script>
 
</body>
</html>
