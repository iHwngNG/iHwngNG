<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GitHub Profile Preview</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;600;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0c10;
    --surface: #0d1117;
    --border: #1e2530;
    --border-glow: #00ff9d22;
    --green: #00ff9d;
    --green-dim: #00ff9d88;
    --blue: #58a6ff;
    --purple: #bc8cff;
    --orange: #f0883e;
    --red: #ff6b6b;
    --text: #e6edf3;
    --muted: #7d8590;
    --card: #161b22;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    padding: 40px 20px;
  }

  .container {
    width: 860px;
    max-width: 100%;
  }

  /* ── HEADER ── */
  .header {
    position: relative;
    padding: 48px 0 40px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 40px;
    overflow: hidden;
  }

  .header::before {
    content: '';
    position: absolute;
    top: -80px; right: -80px;
    width: 320px; height: 320px;
    background: radial-gradient(circle, #00ff9d18 0%, transparent 70%);
    pointer-events: none;
  }

  .badge-row {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }

  .badge {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    padding: 4px 10px;
    border-radius: 3px;
    background: var(--card);
    border: 1px solid var(--border);
    color: var(--muted);
    letter-spacing: 0.05em;
  }

  .badge.active {
    border-color: var(--green);
    color: var(--green);
    box-shadow: 0 0 8px var(--border-glow);
  }

  .name-block {
    margin-bottom: 16px;
  }

  .greeting {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    color: var(--green);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  h1 {
    font-family: 'Syne', sans-serif;
    font-size: 42px;
    font-weight: 800;
    line-height: 1.1;
    color: var(--text);
    letter-spacing: -0.02em;
  }

  h1 span {
    color: var(--green);
    position: relative;
  }

  .tagline {
    font-size: 14px;
    color: var(--muted);
    margin-top: 14px;
    line-height: 1.7;
    max-width: 560px;
  }

  .tagline strong {
    color: var(--blue);
    font-weight: 400;
  }

  /* ── TERMINAL BLOCK ── */
  .terminal {
    background: #050709;
    border: 1px solid var(--border);
    border-radius: 8px;
    margin: 32px 0;
    overflow: hidden;
  }

  .terminal-bar {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 10px 16px;
    background: #0d1117;
    border-bottom: 1px solid var(--border);
  }

  .dot { width: 11px; height: 11px; border-radius: 50%; }
  .dot.r { background: #ff5f57; }
  .dot.y { background: #febc2e; }
  .dot.g { background: #28c840; }

  .term-title {
    font-size: 11px;
    color: var(--muted);
    margin-left: 8px;
    letter-spacing: 0.05em;
  }

  .terminal-body {
    padding: 20px 24px;
    font-size: 13px;
    line-height: 2;
  }

  .prompt { color: var(--green); }
  .cmd { color: var(--text); }
  .comment { color: var(--muted); }
  .output-key { color: var(--blue); }
  .output-val { color: var(--orange); }
  .output-str { color: var(--purple); }
  .cursor {
    display: inline-block;
    width: 9px; height: 15px;
    background: var(--green);
    animation: blink 1s step-end infinite;
    vertical-align: middle;
    margin-left: 2px;
  }
  @keyframes blink { 50% { opacity: 0; } }

  /* ── STACK GRID ── */
  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  .stack-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 8px;
    margin-bottom: 40px;
  }

  .stack-item {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 10px 14px;
    font-size: 12px;
    display: flex;
    align-items: center;
    gap: 10px;
    transition: border-color 0.2s, transform 0.2s;
    cursor: default;
  }

  .stack-item:hover {
    border-color: var(--green-dim);
    transform: translateY(-1px);
  }

  .stack-icon { font-size: 16px; }

  .stack-label { color: var(--text); }
  .stack-sub { font-size: 10px; color: var(--muted); margin-top: 1px; }

  /* ── REPO CARDS ── */
  .repos-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 40px;
  }

  .repo-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.25s, box-shadow 0.25s;
    cursor: pointer;
  }

  .repo-card:hover {
    border-color: var(--green-dim);
    box-shadow: 0 0 20px #00ff9d0a;
  }

  .repo-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--accent, var(--green));
    opacity: 0;
    transition: opacity 0.25s;
  }

  .repo-card:hover::before { opacity: 1; }

  .repo-tag {
    font-size: 10px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--accent, var(--green));
    font-weight: 600;
  }

  .repo-name {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 700;
    color: var(--text);
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .repo-name .icon { font-size: 18px; }

  .repo-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.65;
    flex: 1;
  }

  .repo-meta {
    display: flex;
    align-items: center;
    gap: 14px;
    font-size: 11px;
    color: var(--muted);
    margin-top: 4px;
  }

  .repo-lang {
    display: flex;
    align-items: center;
    gap: 5px;
  }

  .lang-dot {
    width: 9px; height: 9px;
    border-radius: 50%;
  }

  .repo-stats { display: flex; gap: 10px; }
  .stat { display: flex; align-items: center; gap: 4px; }

  .repo-topics {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
  }

  .topic {
    font-size: 10px;
    padding: 2px 8px;
    border-radius: 20px;
    background: #1e2530;
    color: var(--blue);
    border: 1px solid #1e3a5f;
  }

  /* accent variants */
  .accent-blue { --accent: var(--blue); }
  .accent-purple { --accent: var(--purple); }
  .accent-orange { --accent: var(--orange); }
  .accent-red { --accent: var(--red); }

  /* ── STATS ROW ── */
  .stats-row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 12px;
    margin-bottom: 40px;
  }

  .stat-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 20px;
    text-align: center;
  }

  .stat-num {
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 800;
    color: var(--green);
  }

  .stat-label {
    font-size: 11px;
    color: var(--muted);
    margin-top: 4px;
    letter-spacing: 0.08em;
  }

  /* ── CONTRIBUTION ── */
  .contrib-bar {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 24px;
    margin-bottom: 40px;
  }

  .contrib-title {
    font-size: 12px;
    color: var(--muted);
    margin-bottom: 16px;
    display: flex;
    justify-content: space-between;
  }

  .contrib-grid {
    display: grid;
    grid-template-columns: repeat(52, 1fr);
    gap: 3px;
  }

  .contrib-cell {
    aspect-ratio: 1;
    border-radius: 2px;
    background: #161b22;
  }

  /* ── CONNECT ── */
  .connect-row {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    padding-top: 8px;
    border-top: 1px solid var(--border);
  }

  .connect-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    padding: 8px 16px;
    border-radius: 5px;
    text-decoration: none;
    border: 1px solid var(--border);
    color: var(--muted);
    background: var(--card);
    display: flex;
    align-items: center;
    gap: 7px;
    transition: all 0.2s;
    cursor: pointer;
  }

  .connect-btn:hover, .connect-btn.primary {
    border-color: var(--green);
    color: var(--green);
    box-shadow: 0 0 10px var(--border-glow);
  }

  .view-note {
    margin-top: 32px;
    padding: 14px 18px;
    background: #0d1117;
    border: 1px dashed var(--border);
    border-radius: 6px;
    font-size: 11px;
    color: var(--muted);
    line-height: 1.7;
  }

  .view-note strong { color: var(--green); }
</style>
</head>
<body>
<div class="container">

  <!-- HEADER -->
  <div class="header">
    <div class="badge-row">
      <span class="badge active">● OPEN TO WORK</span>
      <span class="badge">Ho Chi Minh City, VN 🇻🇳</span>
      <span class="badge">Python • AI • Data</span>
    </div>

    <div class="name-block">
      <div class="greeting">// Hello, World!</div>
      <h1>I build things<br>with <span>data & AI</span></h1>
    </div>

    <p class="tagline">
      AI Engineer · Data Engineer · Python Developer<br>
      Specializing in <strong>AI-powered solutions</strong>, <strong>data pipelines</strong> &amp; <strong>web crawling systems</strong>.
      Turning raw data into intelligent products.
    </p>
  </div>

  <!-- TERMINAL -->
  <div class="terminal">
    <div class="terminal-bar">
      <div class="dot r"></div>
      <div class="dot y"></div>
      <div class="dot g"></div>
      <span class="term-title">~/profile — zsh</span>
    </div>
    <div class="terminal-body">
      <div><span class="prompt">❯ </span><span class="cmd">cat about.json</span></div>
      <div class="comment">{</div>
      <div>&nbsp;&nbsp;<span class="output-key">"name"</span>: <span class="output-str">"[Your Name]"</span>,</div>
      <div>&nbsp;&nbsp;<span class="output-key">"role"</span>: <span class="output-str">"AI Engineer / Data Engineer"</span>,</div>
      <div>&nbsp;&nbsp;<span class="output-key">"focus"</span>: [<span class="output-str">"LLM Apps"</span>, <span class="output-str">"ETL Pipelines"</span>, <span class="output-str">"Web Crawlers"</span>],</div>
      <div>&nbsp;&nbsp;<span class="output-key">"stack"</span>: <span class="output-str">"Python · FastAPI · Airflow · Scrapy · LangChain"</span>,</div>
      <div>&nbsp;&nbsp;<span class="output-key">"currently"</span>: <span class="output-str">"Building AI agents that actually work in prod"</span>,</div>
      <div>&nbsp;&nbsp;<span class="output-key">"coffee_per_day"</span>: <span class="output-val">3</span></div>
      <div class="comment">}</div>
      <div style="margin-top:8px"><span class="prompt">❯ </span><span class="cursor"></span></div>
    </div>
  </div>

  <!-- TECH STACK -->
  <div class="section-title">Tech Stack</div>
  <div class="stack-grid">
    <div class="stack-item">
      <span class="stack-icon">🐍</span>
      <div><div class="stack-label">Python</div><div class="stack-sub">Primary language</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🤖</span>
      <div><div class="stack-label">LangChain</div><div class="stack-sub">LLM orchestration</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">⚡</span>
      <div><div class="stack-label">FastAPI</div><div class="stack-sub">API layer</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🌊</span>
      <div><div class="stack-label">Apache Airflow</div><div class="stack-sub">Pipeline orchestration</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🕷️</span>
      <div><div class="stack-label">Scrapy / Playwright</div><div class="stack-sub">Web crawling</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🗄️</span>
      <div><div class="stack-label">PostgreSQL</div><div class="stack-sub">Relational DB</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🔴</span>
      <div><div class="stack-label">Redis</div><div class="stack-sub">Cache & queue</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🐳</span>
      <div><div class="stack-label">Docker</div><div class="stack-sub">Containerization</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">☁️</span>
      <div><div class="stack-label">AWS / GCP</div><div class="stack-sub">Cloud infra</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">📊</span>
      <div><div class="stack-label">Pandas / Polars</div><div class="stack-sub">Data processing</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🔍</span>
      <div><div class="stack-label">Elasticsearch</div><div class="stack-sub">Search & analytics</div></div>
    </div>
    <div class="stack-item">
      <span class="stack-icon">🧠</span>
      <div><div class="stack-label">OpenAI / Gemini</div><div class="stack-sub">LLM APIs</div></div>
    </div>
  </div>

  <!-- PINNED REPOS -->
  <div class="section-title">Pinned Repositories</div>
  <div class="repos-grid">

    <div class="repo-card">
      <div class="repo-tag">AI · LLM</div>
      <div class="repo-name"><span class="icon">🤖</span> llm-agent-framework</div>
      <div class="repo-desc">Production-ready LLM agent with tool-use, memory, and multi-step reasoning. Built on LangChain + FastAPI. Supports OpenAI, Gemini, and local models.</div>
      <div class="repo-topics">
        <span class="topic">langchain</span>
        <span class="topic">openai</span>
        <span class="topic">fastapi</span>
        <span class="topic">agents</span>
      </div>
      <div class="repo-meta">
        <div class="repo-lang"><span class="lang-dot" style="background:#3776ab"></span>Python</div>
        <div class="repo-stats">
          <span class="stat">⭐ 128</span>
          <span class="stat">🍴 34</span>
        </div>
      </div>
    </div>

    <div class="repo-card accent-blue">
      <div class="repo-tag">DATA · ETL</div>
      <div class="repo-name"><span class="icon">🌊</span> data-pipeline-kit</div>
      <div class="repo-desc">Modular ETL framework with Airflow DAGs, schema validation, and alerting. Processes 10M+ records/day in production environments.</div>
      <div class="repo-topics">
        <span class="topic">airflow</span>
        <span class="topic">etl</span>
        <span class="topic">postgresql</span>
        <span class="topic">docker</span>
      </div>
      <div class="repo-meta">
        <div class="repo-lang"><span class="lang-dot" style="background:#3776ab"></span>Python</div>
        <div class="repo-stats">
          <span class="stat">⭐ 87</span>
          <span class="stat">🍴 21</span>
        </div>
      </div>
    </div>

    <div class="repo-card accent-orange">
      <div class="repo-tag">CRAWLER · SCRAPY</div>
      <div class="repo-name"><span class="icon">🕷️</span> async-crawler-engine</div>
      <div class="repo-desc">High-performance async web crawler built on Playwright + Redis queue. Anti-detect, proxy rotation, JS-rendering. Crawls 500k pages/hour.</div>
      <div class="repo-topics">
        <span class="topic">playwright</span>
        <span class="topic">scrapy</span>
        <span class="topic">redis</span>
        <span class="topic">async</span>
      </div>
      <div class="repo-meta">
        <div class="repo-lang"><span class="lang-dot" style="background:#3776ab"></span>Python</div>
        <div class="repo-stats">
          <span class="stat">⭐ 203</span>
          <span class="stat">🍴 61</span>
        </div>
      </div>
    </div>

    <div class="repo-card accent-purple">
      <div class="repo-tag">RAG · VECTOR DB</div>
      <div class="repo-name"><span class="icon">🔍</span> rag-search-engine</div>
      <div class="repo-desc">Retrieval-Augmented Generation system with hybrid search (BM25 + embedding). Supports multiple vector stores: Qdrant, Pinecone, Chroma.</div>
      <div class="repo-topics">
        <span class="topic">rag</span>
        <span class="topic">qdrant</span>
        <span class="topic">embedding</span>
        <span class="topic">llm</span>
      </div>
      <div class="repo-meta">
        <div class="repo-lang"><span class="lang-dot" style="background:#3776ab"></span>Python</div>
        <div class="repo-stats">
          <span class="stat">⭐ 156</span>
          <span class="stat">🍴 43</span>
        </div>
      </div>
    </div>

    <div class="repo-card accent-red">
      <div class="repo-tag">MLOPS · INFRA</div>
      <div class="repo-name"><span class="icon">🐳</span> ml-serving-template</div>
      <div class="repo-desc">Docker Compose stack for ML model serving. Includes model registry, A/B testing router, metrics with Prometheus + Grafana.</div>
      <div class="repo-topics">
        <span class="topic">mlops</span>
        <span class="topic">docker</span>
        <span class="topic">prometheus</span>
      </div>
      <div class="repo-meta">
        <div class="repo-lang"><span class="lang-dot" style="background:#3776ab"></span>Python</div>
        <div class="repo-stats">
          <span class="stat">⭐ 74</span>
          <span class="stat">🍴 18</span>
        </div>
      </div>
    </div>

    <div class="repo-card">
      <div class="repo-tag">PYTHON · TOOL</div>
      <div class="repo-name"><span class="icon">⚙️</span> py-data-utils</div>
      <div class="repo-desc">A growing collection of battle-tested Python utilities for data cleaning, normalization, date parsing, and text preprocessing. Used across 10+ projects.</div>
      <div class="repo-topics">
        <span class="topic">utils</span>
        <span class="topic">pandas</span>
        <span class="topic">polars</span>
      </div>
      <div class="repo-meta">
        <div class="repo-lang"><span class="lang-dot" style="background:#3776ab"></span>Python</div>
        <div class="repo-stats">
          <span class="stat">⭐ 45</span>
          <span class="stat">🍴 12</span>
        </div>
      </div>
    </div>

  </div>

  <!-- CONNECT -->
  <div class="section-title">Connect</div>
  <div class="connect-row">
    <a class="connect-btn primary">📧 Email</a>
    <a class="connect-btn">💼 LinkedIn</a>
    <a class="connect-btn">✍️ Blog / Portfolio</a>
    <a class="connect-btn">🐦 Twitter / X</a>
  </div>

  <!-- NOTE -->
  <div class="view-note">
    <strong>📄 README.md được tạo từ file này.</strong> Phía dưới là nội dung Markdown thực tế mày copy vào <code>github.com/[username]/[username]/README.md</code><br>
    Thiết kế trên là visual preview — GitHub README dùng Markdown + badge shields.io để đạt gần với look này.
  </div>

</div>
</body>
</html>
