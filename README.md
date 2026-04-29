# Dhanush.N

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Assignment #03 — Dark Soul</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@400;700;900&family=Crimson+Text:ital,wght@0,400;0,600;1,400&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.2/es5/tex-mml-chtml.min.js"></script>
<style>
  :root {
    --obsidian: #0a0a0f;
    --ash: #1a1a2e;
    --ember: #c0392b;
    --ember-glow: #e74c3c;
    --gold: #d4a017;
    --gold-light: #f0c040;
    --bone: #e8dcc8;
    --fog: #8a8a9a;
    --soul-blue: #4a90d9;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--obsidian);
    color: var(--bone);
    font-family: 'Crimson Text', Georgia, serif;
    overflow-x: hidden;
  }

  /* ===== INTRO OVERLAY ===== */
  #intro {
    position: fixed;
    inset: 0;
    z-index: 9999;
    background: #000;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    transition: opacity 1.2s ease, visibility 1.2s ease;
  }

  #intro.fade-out {
    opacity: 0;
    visibility: hidden;
  }

  .bonfire {
    font-size: 5rem;
    animation: flicker 1.5s ease-in-out infinite alternate;
    margin-bottom: 2rem;
    filter: drop-shadow(0 0 30px #e74c3c) drop-shadow(0 0 60px #c0392b);
  }

  @keyframes flicker {
    0%   { transform: scale(1) rotate(-2deg);   filter: drop-shadow(0 0 20px #e74c3c); }
    50%  { transform: scale(1.08) rotate(1deg); filter: drop-shadow(0 0 50px #f39c12); }
    100% { transform: scale(0.97) rotate(-1deg);filter: drop-shadow(0 0 30px #c0392b); }
  }

  .soul-text {
    font-family: 'Cinzel Decorative', serif;
    font-size: clamp(1.4rem, 4vw, 3rem);
    color: transparent;
    background: linear-gradient(135deg, #d4a017, #f0c040, #c0392b, #d4a017);
    background-size: 300%;
    -webkit-background-clip: text;
    background-clip: text;
    letter-spacing: 0.15em;
    text-align: center;
    animation: shimmer 3s linear infinite, fadeInUp 1.2s ease forwards;
    opacity: 0;
    text-shadow: none;
  }

  @keyframes shimmer {
    0%   { background-position: 0% 50%; }
    100% { background-position: 300% 50%; }
  }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(30px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .soul-sub {
    font-family: 'Share Tech Mono', monospace;
    color: var(--fog);
    font-size: 0.95rem;
    letter-spacing: 0.4em;
    text-transform: uppercase;
    margin-top: 1rem;
    opacity: 0;
    animation: fadeInUp 1s ease 1.2s forwards;
  }

  .enter-btn {
    margin-top: 3rem;
    padding: 0.9rem 2.5rem;
    background: transparent;
    border: 1px solid var(--gold);
    color: var(--gold-light);
    font-family: 'Cinzel Decorative', serif;
    font-size: 0.9rem;
    letter-spacing: 0.3em;
    cursor: pointer;
    opacity: 0;
    animation: fadeInUp 1s ease 2s forwards;
    position: relative;
    overflow: hidden;
    transition: all 0.4s;
  }

  .enter-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--gold);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s ease;
    z-index: -1;
  }

  .enter-btn:hover::before { transform: scaleX(1); }
  .enter-btn:hover { color: #000; border-color: var(--gold-light); }

  .cinders {
    position: absolute;
    inset: 0;
    pointer-events: none;
    overflow: hidden;
  }

  .spark {
    position: absolute;
    bottom: -10px;
    width: 3px;
    height: 3px;
    border-radius: 50%;
    background: var(--ember-glow);
    animation: rise linear infinite;
    opacity: 0;
  }

  @keyframes rise {
    0%   { transform: translateY(0) scale(1);  opacity: 0.8; }
    100% { transform: translateY(-120vh) scale(0); opacity: 0; }
  }

  /* ===== MAIN CONTENT ===== */
  #main {
    opacity: 0;
    transition: opacity 1.5s ease;
    min-height: 100vh;
  }

  #main.visible { opacity: 1; }

  /* Atmospheric BG */
  .bg-texture {
    position: fixed;
    inset: 0;
    z-index: -1;
    background:
      radial-gradient(ellipse at 15% 20%, rgba(192,57,43,0.08) 0%, transparent 50%),
      radial-gradient(ellipse at 85% 80%, rgba(74,144,217,0.06) 0%, transparent 50%),
      radial-gradient(ellipse at 50% 50%, rgba(26,26,46,0.9) 0%, rgba(10,10,15,1) 100%);
  }

  /* Header */
  header {
    text-align: center;
    padding: 5rem 2rem 3rem;
    position: relative;
    border-bottom: 1px solid rgba(212,160,23,0.2);
  }

  header::before {
    content: '🔥';
    position: absolute;
    top: 2rem; left: 50%;
    transform: translateX(-50%);
    font-size: 2rem;
    filter: drop-shadow(0 0 15px #e74c3c);
    animation: flicker 2s ease-in-out infinite alternate;
  }

  .header-badge {
    display: inline-block;
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 0.5em;
    color: var(--ember);
    text-transform: uppercase;
    border: 1px solid rgba(192,57,43,0.4);
    padding: 0.3rem 1.2rem;
    margin-bottom: 1.5rem;
  }

  h1.page-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: clamp(1.8rem, 5vw, 3.5rem);
    background: linear-gradient(180deg, var(--gold-light) 0%, var(--gold) 60%, var(--ember) 100%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    line-height: 1.2;
    margin-bottom: 1rem;
  }

  .meta-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 1rem;
    max-width: 800px;
    margin: 2rem auto 0;
    text-align: left;
  }

  .meta-item {
    background: rgba(255,255,255,0.03);
    border: 1px solid rgba(212,160,23,0.15);
    padding: 0.8rem 1.2rem;
  }

  .meta-item .label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.4em;
    color: var(--ember);
    text-transform: uppercase;
    display: block;
    margin-bottom: 0.3rem;
  }

  .meta-item .value {
    font-size: 1.05rem;
    color: var(--bone);
  }

  /* Section divider */
  .divider {
    display: flex;
    align-items: center;
    gap: 1rem;
    max-width: 900px;
    margin: 3rem auto 1.5rem;
    padding: 0 2rem;
  }

  .divider::before, .divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(212,160,23,0.4), transparent);
  }

  .divider-icon { color: var(--gold); font-size: 1.2rem; }

  /* Question Cards */
  .question-card {
    max-width: 900px;
    margin: 0 auto 3rem;
    padding: 0 2rem;
  }

  .q-header {
    display: flex;
    align-items: baseline;
    gap: 1.2rem;
    margin-bottom: 1.5rem;
  }

  .q-number {
    font-family: 'Cinzel Decorative', serif;
    font-size: 2.5rem;
    color: var(--ember);
    line-height: 1;
    text-shadow: 0 0 30px rgba(192,57,43,0.5);
    flex-shrink: 0;
  }

  .q-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 1rem;
    color: var(--gold);
    letter-spacing: 0.05em;
  }

  .q-body {
    background: rgba(255,255,255,0.02);
    border: 1px solid rgba(212,160,23,0.12);
    border-left: 3px solid var(--ember);
    padding: 2rem 2.5rem;
    position: relative;
    overflow: hidden;
  }

  .q-body::before {
    content: '';
    position: absolute;
    top: 0; right: 0;
    width: 60px; height: 60px;
    background: radial-gradient(circle at top right, rgba(192,57,43,0.08), transparent 70%);
  }

  .q-statement {
    font-size: 1.1rem;
    line-height: 1.8;
    color: var(--fog);
    margin-bottom: 2rem;
    font-style: italic;
  }

  .answer-section {
    margin-top: 2rem;
    padding-top: 1.5rem;
    border-top: 1px solid rgba(212,160,23,0.15);
  }

  .answer-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.7rem;
    letter-spacing: 0.5em;
    color: var(--soul-blue);
    text-transform: uppercase;
    margin-bottom: 1.2rem;
    display: flex;
    align-items: center;
    gap: 0.8rem;
  }

  .answer-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: rgba(74,144,217,0.2);
  }

  .step {
    margin-bottom: 1.8rem;
    padding-left: 1.5rem;
    border-left: 1px solid rgba(212,160,23,0.2);
  }

  .step-title {
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.8rem;
    color: var(--gold);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 0.6rem;
  }

  .step p {
    font-size: 1.05rem;
    line-height: 1.9;
    color: var(--bone);
  }

  .math-block {
    background: rgba(0,0,0,0.4);
    border: 1px solid rgba(212,160,23,0.1);
    padding: 1.2rem 1.5rem;
    margin: 1rem 0;
    overflow-x: auto;
    font-size: 1rem;
    line-height: 2;
  }

  .highlight-box {
    background: rgba(192,57,43,0.08);
    border: 1px solid rgba(192,57,43,0.3);
    padding: 1rem 1.5rem;
    margin-top: 1.5rem;
    font-size: 1.1rem;
    font-style: italic;
    color: var(--ember-glow);
  }

  /* Footer */
  footer {
    text-align: center;
    padding: 3rem 2rem;
    border-top: 1px solid rgba(212,160,23,0.15);
    margin-top: 4rem;
  }

  footer .soul-sign {
    font-family: 'Cinzel Decorative', serif;
    font-size: 1.2rem;
    color: var(--ember);
    letter-spacing: 0.2em;
    animation: flicker 3s ease-in-out infinite alternate;
  }

  footer p {
    margin-top: 0.5rem;
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.75rem;
    color: var(--fog);
    letter-spacing: 0.3em;
  }
</style>
</head>
<body>

<!-- ========== INTRO ========== -->
<div id="intro">
  <div class="cinders" id="cinders"></div>
  <div class="bonfire">🔥</div>
  <div class="soul-text">I'M DARK SOUL<br>WELCOME YOU</div>
  <div class="soul-sub">Your journey into knowledge begins</div>
  <button class="enter-btn" onclick="enterSite()">⚔ KINDLE THE FLAME ⚔</button>
</div>

<!-- ========== MAIN ========== -->
<div id="main">
  <div class="bg-texture"></div>

  <header>
    <div class="header-badge">Dept. of Mathematics · Assignment</div>
    <h1 class="page-title">Assignment #03</h1>
    <div class="meta-grid">
      <div class="meta-item"><span class="label">Issued</span><span class="value">22.04.2026</span></div>
      <div class="meta-item"><span class="label">Due Date</span><span class="value">27.04.2026</span></div>
      <div class="meta-item"><span class="label">Total Marks</span><span class="value">30 Marks</span></div>
      <div class="meta-item"><span class="label">Subject</span><span class="value">Transforms & PDE</span></div>
    </div>
  </header>

  <!-- Q1 -->
  <div class="divider"><span class="divider-icon">⚔</span></div>
  <div class="question-card">
    <div class="q-header">
      <span class="q-number">Q1</span>
      <span class="q-title">Fourier Series · C201.4 · 10 Marks</span>
    </div>
    <div class="q-body">
      <div class="q-statement">
        Obtain the Fourier series for the function f(x) given by:
        <div class="math-block">
          \[f(x) = \begin{cases} 1 + \dfrac{2x}{\pi}, & -\pi \le x \le 0 \\[8pt] 1 - \dfrac{2x}{\pi}, & 0 \le x \le \pi \end{cases}\]
        </div>
      </div>

      <div class="answer-section">
        <div class="answer-label">Solution</div>

        <div class="step">
          <div class="step-title">Step 1 — Identify the function</div>
          <p>The function f(x) is defined on \([-\pi, \pi]\) and is <strong>even</strong> (symmetric about the y-axis), since f(–x) = f(x). Therefore, the Fourier series contains only cosine terms (bₙ = 0).</p>
        </div>

        <div class="step">
          <div class="step-title">Step 2 — Fourier Coefficients</div>
          <p>For an even function on \([-\pi, \pi]\):</p>
          <div class="math-block">
            \[a_0 = \frac{2}{\pi}\int_0^{\pi} f(x)\,dx = \frac{2}{\pi}\int_0^{\pi}\left(1 - \frac{2x}{\pi}\right)dx\]
            \[= \frac{2}{\pi}\left[x - \frac{x^2}{\pi}\right]_0^{\pi} = \frac{2}{\pi}\left[\pi - \pi\right] = 0\]
          </div>
        </div>

        <div class="step">
          <div class="step-title">Step 3 — Cosine Coefficients aₙ</div>
          <div class="math-block">
            \[a_n = \frac{2}{\pi}\int_0^{\pi}\left(1 - \frac{2x}{\pi}\right)\cos(nx)\,dx\]
            \[\text{Integrating by parts:}\]
            \[a_n = \frac{2}{\pi}\left[\frac{\left(1-\frac{2x}{\pi}\right)\sin(nx)}{n} + \frac{2\cos(nx)}{n^2\pi}\right]_0^{\pi}\]
            \[= \frac{2}{\pi}\cdot\frac{2}{n^2\pi}\left[\cos(n\pi) - 1\right] = \frac{4}{n^2\pi^2}\left[(-1)^n - 1\right]\]
          </div>
          <p>When n is odd: \(a_n = \dfrac{-8}{n^2\pi^2}\) &nbsp;|&nbsp; When n is even: \(a_n = 0\)</p>
        </div>

        <div class="step">
          <div class="step-title">Step 4 — Final Fourier Series</div>
          <div class="math-block">
            \[\boxed{f(x) = \frac{8}{\pi^2}\sum_{n=1,3,5,...}^{\infty} \frac{(-1)^{\frac{n-1}{2}}}{n^2}\cos(nx)}\]
            \[f(x) = \frac{8}{\pi^2}\left[\cos x - \frac{\cos 3x}{9} + \frac{\cos 5x}{25} - \cdots \right]\]
          </div>
        </div>

        <div class="highlight-box">
          ✦ Result: f(x) is even → only cosine terms survive. The series converges to f(x) at all points where f is continuous, and to the average at discontinuities.
        </div>
      </div>
    </div>
  </div>

  <!-- Q2 -->
  <div class="divider"><span class="divider-icon">🔥</span></div>
  <div class="question-card">
    <div class="q-header">
      <span class="q-number">Q2</span>
      <span class="q-title">Wave Equation / String Vibration · C201.5 · 10 Marks</span>
    </div>
    <div class="q-body">
      <div class="q-statement">
        A string is stretched between two fixed points at a distance 2l apart. The initial velocities are:
        <div class="math-block">
          \[v = \begin{cases} \dfrac{cx}{l}, & 0 < x < l \\[8pt] \dfrac{c}{l}(2l-x), & l < x < 2l \end{cases}\]
        </div>
        Find the displacement of the string at any point.
      </div>

      <div class="answer-section">
        <div class="answer-label">Solution</div>

        <div class="step">
          <div class="step-title">Step 1 — Setup</div>
          <p>The wave equation is \(\dfrac{\partial^2 y}{\partial t^2} = c^2 \dfrac{\partial^2 y}{\partial x^2}\), with boundary conditions y(0,t) = y(2l,t) = 0 and initial displacement y(x,0) = 0.</p>
        </div>

        <div class="step">
          <div class="step-title">Step 2 — General Solution</div>
          <p>Since initial displacement is zero, the solution takes the form:</p>
          <div class="math-block">
            \[y(x,t) = \sum_{n=1}^{\infty} B_n \sin\left(\frac{n\pi x}{2l}\right)\sin\left(\frac{n\pi c t}{2l}\right)\]
          </div>
        </div>

        <div class="step">
          <div class="step-title">Step 3 — Apply Initial Velocity Condition</div>
          <div class="math-block">
            \[\frac{\partial y}{\partial t}\bigg|_{t=0} = \sum_{n=1}^{\infty} B_n \cdot \frac{n\pi c}{2l}\sin\left(\frac{n\pi x}{2l}\right) = v(x)\]
          </div>
          <p>Using the Fourier sine series, the coefficient is:</p>
          <div class="math-block">
            \[B_n \cdot \frac{n\pi c}{2l} = \frac{2}{2l}\left[\int_0^{l}\frac{cx}{l}\sin\frac{n\pi x}{2l}dx + \int_l^{2l}\frac{c(2l-x)}{l}\sin\frac{n\pi x}{2l}dx\right]\]
          </div>
        </div>

        <div class="step">
          <div class="step-title">Step 4 — Evaluate Integrals</div>
          <p>By symmetry of the initial velocity (triangular profile), both integrals contribute equally:</p>
          <div class="math-block">
            \[B_n = \frac{8cl}{n^2\pi^2 c} \cdot \frac{2}{n\pi}\sin\frac{n\pi}{2} \cdot \frac{2l}{n\pi c}\]
            \[B_n = \frac{8cl}{n^3\pi^3}\sin\frac{n\pi}{2}\]
          </div>
          <p>Note: \(\sin\frac{n\pi}{2} = 0\) for even n, so only odd n contribute.</p>
        </div>

        <div class="step">
          <div class="step-title">Step 5 — Final Displacement</div>
          <div class="math-block">
            \[\boxed{y(x,t) = \frac{8cl}{\pi^3 c}\sum_{n=1,3,5,...}^{\infty} \frac{(-1)^{\frac{n-1}{2}}}{n^3}\sin\frac{n\pi x}{2l}\sin\frac{n\pi ct}{2l}}\]
          </div>
        </div>

        <div class="highlight-box">
          ✦ Result: The string vibrates in a superposition of harmonics. Only odd modes are excited due to the symmetric triangular initial velocity profile.
        </div>
      </div>
    </div>
  </div>

  <!-- Q3 -->
  <div class="divider"><span class="divider-icon">⚔</span></div>
  <div class="question-card">
    <div class="q-header">
      <span class="q-number">Q3</span>
      <span class="q-title">Heat Equation / Laplace Equation · C201.5 · 10 Marks</span>
    </div>
    <div class="q-body">
      <div class="q-statement">
        A rectangle plate with insulated surface is 10 cm wide (infinite length). Temperature at short edge y = 0:
        <div class="math-block">
          \[u = f(x) = \begin{cases} 20x, & 0 \le x \le 5 \\ 20(10-x), & 5 \le x \le 10 \end{cases}\]
        </div>
        Other three edges are at 0°C. Find the steady state temperature at any point.
      </div>

      <div class="answer-section">
        <div class="answer-label">Solution</div>

        <div class="step">
          <div class="step-title">Step 1 — Governing Equation</div>
          <p>The steady state temperature satisfies Laplace's equation:</p>
          <div class="math-block">
            \[\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0\]
          </div>
          <p>Boundary conditions: u(0,y) = 0, u(10,y) = 0, u(x,y)→0 as y→∞, u(x,0) = f(x).</p>
        </div>

        <div class="step">
          <div class="step-title">Step 2 — Solution by Separation of Variables</div>
          <p>With X(0) = X(10) = 0 and Y(∞) = 0:</p>
          <div class="math-block">
            \[u(x,y) = \sum_{n=1}^{\infty} B_n \sin\frac{n\pi x}{10} \cdot e^{-n\pi y/10}\]
          </div>
        </div>

        <div class="step">
          <div class="step-title">Step 3 — Fourier Sine Coefficients</div>
          <div class="math-block">
            \[B_n = \frac{2}{10}\left[\int_0^5 20x\sin\frac{n\pi x}{10}dx + \int_5^{10}20(10-x)\sin\frac{n\pi x}{10}dx\right]\]
          </div>
          <p>Evaluating the integrals (integration by parts):</p>
          <div class="math-block">
            \[\int_0^5 20x\sin\frac{n\pi x}{10}dx = 20\left[-\frac{10x\cos\frac{n\pi x}{10}}{n\pi} + \frac{100\sin\frac{n\pi x}{10}}{n^2\pi^2}\right]_0^5\]
            \[= 20\left[\frac{-50\cos\frac{n\pi}{2}}{n\pi} + \frac{100\sin\frac{n\pi}{2}}{n^2\pi^2}\right]\]
          </div>
          <p>By symmetry, the second integral gives an equal contribution, so:</p>
          <div class="math-block">
            \[B_n = \frac{1}{5}\cdot 2 \cdot 20\cdot\frac{100\sin\frac{n\pi}{2}}{n^2\pi^2} \cdot 2 = \frac{800}{n^2\pi^2}\sin\frac{n\pi}{2}\]
          </div>
          <p>Only odd n contribute; \(\sin\frac{n\pi}{2} = (\pm 1)\) for n = 1, 3, 5, ...</p>
        </div>

        <div class="step">
          <div class="step-title">Step 4 — Final Temperature Distribution</div>
          <div class="math-block">
            \[\boxed{u(x,y) = \frac{800}{\pi^2}\sum_{n=1,3,5,...}^{\infty} \frac{(-1)^{\frac{n-1}{2}}}{n^2}\sin\frac{n\pi x}{10}\cdot e^{-n\pi y/10}}\]
            \[u(x,y) = \frac{800}{\pi^2}\left[\sin\frac{\pi x}{10}e^{-\pi y/10} - \frac{1}{9}\sin\frac{3\pi x}{10}e^{-3\pi y/10} + \cdots\right]\]
          </div>
        </div>

        <div class="highlight-box">
          ✦ Result: Temperature decreases exponentially with depth y and varies sinusoidally in x. The triangular boundary condition excites only odd Fourier modes.
        </div>
      </div>
    </div>
  </div>

  <footer>
    <div class="soul-sign">🔥 Dark Soul — Guardian of Knowledge 🔥</div>
  
</div>

<script>
  // Generate sparks
  const cinders = document.getElementById('cinders');
  for (let i = 0; i < 30; i++) {
    const spark = document.createElement('div');
    spark.className = 'spark';
    spark.style.left = Math.random() * 100 + '%';
    spark.style.animationDuration = (3 + Math.random() * 5) + 's';
    spark.style.animationDelay = (Math.random() * 5) + 's';
    spark.style.width = spark.style.height = (2 + Math.random() * 3) + 'px';
    const hue = Math.random() > 0.5 ? '#e74c3c' : '#f39c12';
    spark.style.background = hue;
    cinders.appendChild(spark);
  }

  function enterSite() {
    document.getElementById('intro').classList.add('fade-out');
    document.body.style.overflow = 'auto';
    setTimeout(() => {
      document.getElementById('intro').style.display = 'none';
      document.getElementById('main').classList.add('visible');
    }, 1200);
  }

  // Prevent scroll while intro is showing
  document.body.style.overflow = 'hidden';
</script>
</body>
</html>
