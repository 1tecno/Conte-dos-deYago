<!DOCTYPE html>
<html lang="pt-BR" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Portal TechYago</title>
  <!-- SEO & Open Graph -->
  <meta name="description" content="Portal TechYago - conteúdos de tecnologia sobre arquitetura, POO, CSS & HTML, lógica, robótica e mais.">
  <meta property="og:title" content="Portal TechYago">
  <meta property="og:description" content="Explore conteúdos de tecnologia sobre arquitetura, POO, CSS & HTML, lógica e muito mais.">
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://seusite.com">
  <meta property="og:image" content="https://seusite.com/og-image.png">
  <meta name="theme-color" content="#4a90e2">
  <link rel="icon" href="/favicon.ico">
  <!-- Google Font -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <!-- Font Awesome -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <style>
    /* Core Variables */
    :root {
      --primary: #4a90e2;
      --secondary: #283e4a;
      --light-bg: #f4f4f4;
      --dark-bg: #121212;
      --text-light: #fff;
      --text-dark: #333;
      --accent: #f5a623;
      --border: #e0e0e0;
      --transition: 0.3s ease;
      --card-bg-light: #fff;
      --card-bg-dark: #1e1e1e;
      --card-bg-galaxy: rgba(10,10,25,0.6);
    }
    [data-theme="light"] { --bg: var(--light-bg);  --text: var(--text-dark);  --card-bg: var(--card-bg-light); }
    [data-theme="dark"]  { --bg: var(--dark-bg);   --text: var(--text-light);  --card-bg: var(--card-bg-dark); }
    [data-theme="galaxy"]{ --bg: #000;            --text: var(--text-light);  --card-bg: var(--card-bg-galaxy); }

    *, *::before, *::after { box-sizing: border-box; margin:0; padding:0; }
    html { scroll-behavior: smooth; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Inter', sans-serif;
      transition: background var(--transition), color var(--transition);
      overflow-x: hidden;
      position: relative;
    }
    a { text-decoration: none; color: inherit; }
    /* Focus styles for acessibilidade */
    a:focus, button:focus, input:focus { outline: 3px solid var(--accent); outline-offset: 2px; }

    /* Constellation Canvas - only visible in galaxy theme */
    canvas#constellation {
      position: fixed; top:0; left:0;
      width:100%; height:100%; pointer-events:none;
      opacity: 0; transition: opacity 0.5s ease;
      z-index:-1;
    }
    [data-theme="galaxy"] canvas#constellation {
      opacity: 1;
    }

    /* Hero Section */
    .hero {
      background: var(--primary);
      color: var(--text-light);
      padding: 4rem 2rem;
      text-align: center;
      opacity: 0; transform: translateY(20px);
    }
    .hero-content h2 { font-size: 2.5rem; margin-bottom:1rem; }
    .hero-content p  { font-size: 1.2rem; margin-bottom:2rem; }
    .btn { display:inline-block; padding:0.8rem 1.5rem; border-radius:8px; border:2px solid var(--text-light); font-weight:600; cursor:pointer; transition: background var(--transition); }
    .btn:hover { background: rgba(255,255,255,0.2); }

    /* Header */
    header {
      display: flex; align-items: center; justify-content: space-between;
      background: var(--primary);
      padding: 1rem 2rem;
      position: sticky; top: 0; z-index: 1000;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    header h1 { color: var(--text-light); font-size: 1.8rem; font-weight: 700; }
    .menu-btn {
      background: none; border: none;
      font-size: 1.5rem; color: var(--text-light);
      cursor: pointer; transition: transform var(--transition);
    }
    .menu-btn:hover { transform: scale(1.2); }

    /* Sidebar Menu */
    aside.menu {
      position: fixed; top:0; right:-260px;
      width:260px; height:100%; background: var(--secondary);
      padding:2rem 1.5rem; transition: right var(--transition);
      z-index:1001; display:flex; flex-direction:column;
    }
    aside.menu.open { right: 0; }
    aside.menu h2 { color: var(--text-light); font-size:1.4rem; margin-bottom:1rem; }
    aside.menu nav ul { list-style:none; display:flex; flex-direction:column; gap:1rem; }
    aside.menu nav a { color: var(--text-light); font-weight:600; font-size:1.1rem; display:flex; align-items:center; gap:0.5rem; transition: color var(--transition); }
    aside.menu nav a:hover { color: var(--accent); }
    .theme-switch { margin-top:auto; display:flex; flex-direction:column; gap:0.8rem; }
    .theme-switch button { background:none; border:1px solid var(--text-light); color: var(--text-light); padding:0.6rem; border-radius:4px; cursor:pointer; font-size:1rem; transition: background var(--transition); }
    .theme-switch button:hover { background: rgba(255,255,255,0.2); }

    /* Main Content */
    main { padding:2rem; max-width:1200px; margin:auto; }
    .search-container { display:flex; justify-content:center; margin-bottom:1rem; }
    .search-container input {
      width:100%; max-width:600px; padding:0.8rem; font-size:1rem;
      border:1px solid var(--border); border-radius:8px;
      transition: box-shadow var(--transition);
    }
    .search-container input:focus { box-shadow:0 0 6px var(--accent); }
    #no-results { display:none; text-align:center; margin:1rem 0; color: var(--text); }
    .mochila { display:grid; grid-template-columns: repeat(auto-fit, minmax(280px,1fr)); gap:2rem; }

    /* Cards */
    .card { background: var(--card-bg); border:1px solid var(--border); border-radius:12px; overflow:hidden;
      transform: translateY(20px); opacity:0; transition: opacity 0.6s ease, transform 0.6s ease;
    }
    .card.show { opacity:1; transform: translateY(0); }
    .card:hover { transform: translateY(-6px); box-shadow: 0 0 12px var(--accent); }
    .card summary { padding:1.5rem; display:flex; align-items:center; gap:0.7rem; font-size:1.3rem; font-weight:600; list-style:none; cursor:pointer; transition: color var(--transition); }
    .card summary:hover i { transform: scale(1.2) rotate(5deg); color: var(--accent); }
    .card p { padding:0 1.5rem 1.5rem; line-height:1.6; }
    details>summary::-webkit-details-marker { display:none; }

    /* Scroll to Top */
    #toTop {
      position:fixed; bottom:2rem; right:2rem; background: var(--accent); color: var(--text-light);
      border:none; border-radius:50%; width:3rem; height:3rem; font-size:1.5rem;
      cursor:pointer; display:none; align-items:center; justify-content:center;
    }
    #toTop.show { display:flex; }

    /* Footer */
    footer { text-align:center; padding:2rem; background: var(--secondary); color: var(--text-light); }
    footer a { color: var(--accent); }
  </style>
</head>
<body>
  <canvas id="constellation"></canvas>
  <header>
    <h1>Portal TechYago</h1>
    <button class="menu-btn" aria-label="Abrir Menu" aria-expanded="false"><i class="fas fa-bars"></i></button>
  </header>
  <aside class="menu" id="menu" role="navigation">
    <h2>Menu</h2>
    <nav>
      <ul>
        <li><a href="#arquitetura"><i class="fas fa-desktop"></i> Arquitetura</a></li>
        <li><a href="#poo"><i class="fab fa-java"></i> Java POO</a></li>
        <li><a href="#css-html"><i class="fab fa-css3-alt"></i> CSS & HTML</a></li>
        <li><a href="#logica"><i class="fas fa-brain"></i> Lógica</a></li>
        <li><a href="#web"><i class="fas fa-code"></i> Web</a></li>
        <li><a href="#robotica"><i class="fas fa-robot"></i> Robótica</a></li>
        <li><a href="#so"><i class="fas fa-cogs"></i> Sistemas</a></li>
        <li><a href="#gestao"><i class="fas fa-clock"></i> Gestão Tempo</a></li>
      </ul>
    </nav>
    <div class="theme-switch">
      <button data-theme="light"><i class="fas fa-sun"></i> Claro</button>
      <button data-theme="dark"><i class="fas fa-moon"></i> Escuro</button>
      <button data-theme="galaxy"><i class="fas fa-star"></i> Galaxy</button>
    </div>
  </aside>

  <!-- Hero Section -->
  <section class="hero" role="banner">
    <div class="hero-content">
      <h2>Bem-vindo ao Portal TechYago</h2>
      <p>Explore conteúdos de tecnologia sobre arquitetura, POO, CSS & HTML, lógica e muito mais!</p>
      <a href="#mochila" class="btn">Começar</a>
    </div>
  </section>

  <main>
    <div class="search-container">
      <input type="text" id="search" placeholder="Buscar conteúdos..." />
    </div>
    <p id="no-results">Nenhum resultado encontrado.</p>
    <div class="mochila" id="mochila">
      <details class="card" style="--i:0" id="arquitetura"><summary><i class="fas fa-desktop"></i>Arquitetura e Manutenção</summary><p>Conteúdos em breve...</p></details>
      <details class="card" style="--i:1" id="poo"><summary><i class="fab fa-java"></i>Programação Orientada a Objetos</summary><p>Conteúdos em breve...</p></details>
      <details class="card" style="--i:2" id="css-html"><summary><i class="fab fa-css3-alt"></i>CSS & HTML</summary><p>Conteúdos em breve...</p></details>
      <details class="card" style="--i:3" id="logica"><summary><i class="fas fa-brain"></i>Lógica de Programação</summary><p>Conteúdos em breve...</p></details>
      <details class="card" style="--i:4" id="web"><summary><i class="fas fa-code"></i>Programação Web</summary><p>Conteúdos em breve...</p></details>
      <details class="card" style="--i:5" id="robotica"><summary><i class="fas fa-robot"></i>Robótica</summary><p>Conteúdos em<br>em breve...</p></details>
      <details class="card" style="--i:6" id="so"><summary><i class="fas fa-cogs"></i>Sistemas Operacionais</summary><p>Conteúdos em breve...</p></details>
      <details class="card" style="--i:7" id="gestao"><summary><i class="fas fa-clock"></i>Gestão de Tempo</summary><p>Conteúdos em breve...</p></details>
    </div>
  </main>

  <button id="toTop" aria-label="Voltar ao topo">↑</button>

  <footer>
    Siga-me no Instagram: <a href="https://instagram.com/nauanloures" target="_blank">@nauanloures</a>
  </footer>

  <script>
    /*** Theme Initialization ***/
    const saved = localStorage.getItem('theme');
    if(saved) {
      document.documentElement.setAttribute('data-theme', saved);
    } else if(window.matchMedia('(prefers-color-scheme: dark)').matches) {
      document.documentElement.setAttribute('data-theme', 'dark');
    }

    /*** Constellation animation ***/
    const canvas = document.getElementById('constellation');
    const ctx = canvas.getContext('2d');
    function resize() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
    window.addEventListener('resize', resize);
    resize();
    const isMobile = window.innerWidth < 600 || 'ontouchstart' in window;
    const stars = Array.from({ length: isMobile ? 50 : 100 }, () => ({
      x: Math.random()*canvas.width,
      y: Math.random()*canvas.height,
      vx: (Math.random()-0.5)*0.2,
      vy: (Math.random()-0.5)*0.2
    }));
    function draw() {
      if(!isMobile || document.documentElement.dataset.theme === 'galaxy') {
        ctx.clearRect(0,0,canvas.width,canvas.height);
        stars.forEach(s => {
          s.x += s.vx; s.y += s.vy;
          if(s.x < 0 || s.x > canvas.width) s.vx *= -1;
          if(s.y < 0 || s.y > canvas.height) s.vy *= -1;
          ctx.fillStyle = 'rgba(255,255,255,0.8)';
          ctx.beginPath(); ctx.arc(s.x, s.y, 2, 0, Math.PI*2); ctx.fill();
        });
        for(let i=0; i<stars.length; i++) {
          for(let j=i+1; j<stars.length; j++) {
            const a=stars[i], b=stars[j]; const dist = Math.hypot(a.x-b.x,a.y-b.y);
            if(dist < 120) {
              ctx.strokeStyle = `rgba(255,255,255,${1 - dist/120})`;
              ctx.beginPath(); ctx.moveTo(a.x,a.y); ctx.lineTo(b.x,b.y); ctx.stroke();
            }
          }
        }
      }
      requestAnimationFrame(draw);
    }
    draw();

    /*** Debounce Function ***/
    function debounce(fn, delay = 300) {
      let timer;
      return (...args) => {
        clearTimeout(timer);
        timer = setTimeout(() => fn.apply(this, args), delay);
      };
    }

    /*** Search Filter with Feedback ***/
    const search = document.getElementById('search');
    const mochila = document.getElementById('mochila');
    const noResults = document.getElementById('no-results');
    function filtrar() {
      const val = search.value.toLowerCase();
      let count = 0;
      mochila.querySelectorAll('.card').forEach(card => {
        const show = card.textContent.toLowerCase().includes(val);
        card.style.display = show ? '' : 'none';
        if(show) count++;
      });
      noResults.style.display = count ? 'none' : 'block';
    }
    search.addEventListener('input', debounce(filtrar, 250));

    /*** Menu Toggle with ARIA ***/
    const menuBtn = document.querySelector('.menu-btn');
    const menu = document.querySelector('aside.menu');
    menuBtn.addEventListener('click', () => {
      const open = menu.classList.toggle('open');
      menuBtn.setAttribute('aria-expanded', open);
    });

    /*** Theme Switch ***/
    document.querySelectorAll('.theme-switch button').forEach(btn => {
      btn.addEventListener('click', () => {
        document.documentElement.setAttribute('data-theme', btn.dataset.theme);
        localStorage.setItem('theme', btn.dataset.theme);
        menu.classList.remove('open');
      });
    });

    /*** Scroll Reveal ***/
    const observer = new IntersectionObserver((entries, obs) => {
      entries.forEach(entry => {
        if(entry.isIntersecting) {
          entry.target.classList.add('show');
          obs.unobserve(entry.target);
        }
      });
    }, { threshold: 0.1 });
    document.querySelectorAll('.card, .hero').forEach(el => observer.observe(el));

    /*** Scroll to Top Button ***/
    const toTop = document.getElementById('toTop');
    window.addEventListener('scroll', () => {
      toTop.classList.toggle('show', window.scrollY > 300);
    });
    toTop.addEventListener('click', () => window.scrollTo({ top: 0, behavior: 'smooth' }));
  </script>
</body>
</html>
