<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Networking Flashcards</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d0f14;
    --surface: #161921;
    --surface2: #1e2330;
    --border: #2a3040;
    --accent: #4fffb0;
    --accent2: #3a8fff;
    --text: #e8eaf0;
    --text-muted: #6b7280;
    --card-front: #161921;
    --card-back: #0f1f18;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Syne', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 2rem 1rem;
  }

  /* Subtle grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(79,255,176,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(79,255,176,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  .wrapper { position: relative; z-index: 1; max-width: 960px; margin: 0 auto; }

  header {
    text-align: center;
    margin-bottom: 2.5rem;
  }

  header h1 {
    font-size: clamp(1.8rem, 4vw, 2.8rem);
    font-weight: 800;
    letter-spacing: -0.02em;
    color: var(--text);
  }

  header h1 span { color: var(--accent); }

  header p {
    font-family: 'JetBrains Mono', monospace;
    color: var(--text-muted);
    font-size: 0.8rem;
    margin-top: 0.5rem;
    letter-spacing: 0.05em;
  }

  /* Stats bar */
  .stats {
    display: flex;
    gap: 1rem;
    justify-content: center;
    margin-bottom: 2rem;
    flex-wrap: wrap;
  }

  .stat {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem;
    padding: 0.4rem 1rem;
    border: 1px solid var(--border);
    border-radius: 4px;
    color: var(--text-muted);
    background: var(--surface);
  }

  .stat strong { color: var(--accent); }

  /* Filter bar */
  .filters {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 2rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .filter-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    padding: 0.35rem 0.9rem;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--text-muted);
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.15s;
    letter-spacing: 0.04em;
  }

  .filter-btn:hover, .filter-btn.active {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(79,255,176,0.06);
  }

  /* Search */
  .search-wrap {
    margin-bottom: 1.5rem;
    display: flex;
    justify-content: center;
  }

  .search-wrap input {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.82rem;
    width: 100%;
    max-width: 500px;
    padding: 0.6rem 1rem;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    color: var(--text);
    outline: none;
    transition: border-color 0.15s;
  }

  .search-wrap input:focus { border-color: var(--accent2); }
  .search-wrap input::placeholder { color: var(--text-muted); }

  /* Cards grid */
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.2rem;
  }

  /* Flip card */
  .flip-card {
    height: 200px;
    perspective: 1000px;
    cursor: pointer;
  }

  .flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    transition: transform 0.45s cubic-bezier(0.4,0,0.2,1);
  }

  .flip-card.flipped .flip-card-inner {
    transform: rotateY(180deg);
  }

  .flip-card-front, .flip-card-back {
    position: absolute;
    inset: 0;
    backface-visibility: hidden;
    border-radius: 8px;
    padding: 1.2rem;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  /* Front */
  .flip-card-front {
    background: var(--card-front);
    border: 1px solid var(--border);
    transition: border-color 0.2s;
  }

  .flip-card:hover .flip-card-front {
    border-color: var(--accent2);
  }

  .flip-card-front .q-num {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent2);
    letter-spacing: 0.08em;
    margin-bottom: 0.6rem;
    flex-shrink: 0;
  }

  .flip-card-front .q-text {
    font-size: 0.88rem;
    line-height: 1.5;
    color: var(--text);
    flex: 1;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 6;
    -webkit-box-orient: vertical;
  }

  .flip-card-front .flip-hint {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.62rem;
    color: var(--text-muted);
    margin-top: 0.8rem;
    text-align: right;
    flex-shrink: 0;
    letter-spacing: 0.04em;
  }

  /* Back */
  .flip-card-back {
    background: var(--card-back);
    border: 1px solid #1e4035;
    transform: rotateY(180deg);
  }

  .flip-card-back .a-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent);
    letter-spacing: 0.08em;
    margin-bottom: 0.6rem;
    flex-shrink: 0;
  }

  .flip-card-back .a-text {
    font-size: 0.82rem;
    line-height: 1.55;
    color: #b8f0d8;
    flex: 1;
    overflow-y: auto;
    scrollbar-width: thin;
    scrollbar-color: var(--border) transparent;
  }

  .flip-card-back .a-text::-webkit-scrollbar { width: 3px; }
  .flip-card-back .a-text::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }

  .correct-answer {
    font-weight: 700;
    color: var(--accent);
  }

  /* No results */
  .no-results {
    text-align: center;
    padding: 4rem 1rem;
    color: var(--text-muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    grid-column: 1/-1;
  }

  /* Learned section */
  .section-title {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 0.1em;
    color: var(--text-muted);
    text-transform: uppercase;
    margin: 0 0 1rem;
    display: flex;
    align-items: center;
    gap: 0.6rem;
  }
  .section-title strong { color: var(--accent); }
  .section-title .line {
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  .learned-zone {
    border: 1px dashed var(--border);
    border-radius: 8px;
    padding: 1rem;
    min-height: 140px;
    margin-bottom: 2.5rem;
    background: rgba(79,255,176,0.02);
    transition: border-color 0.15s, background 0.15s;
  }
  .learned-zone.drag-over {
    border-color: var(--accent);
    background: rgba(79,255,176,0.07);
  }
  .learned-zone .empty-hint {
    text-align: center;
    color: var(--text-muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem;
    padding: 2rem 1rem;
    grid-column: 1/-1;
    pointer-events: none;
  }

  .grid.drop-target.drag-over {
    outline: 1px dashed var(--accent2);
    outline-offset: 8px;
    border-radius: 8px;
  }

  .flip-card { position: relative; }
  .flip-card.dragging { opacity: 0.4; }
  .drag-handle {
    position: absolute;
    top: 6px;
    right: 8px;
    z-index: 3;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.7rem;
    color: var(--text-muted);
    cursor: grab;
    padding: 2px 6px;
    border-radius: 3px;
    background: rgba(0,0,0,0.3);
    user-select: none;
    letter-spacing: 0.1em;
  }
  .drag-handle:hover { color: var(--accent); }
  .drag-handle:active { cursor: grabbing; }

  /* Footer */
  footer {
    text-align: center;
    margin-top: 3rem;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.7rem;
    color: var(--text-muted);
  }

  /* Nav tabs */
  .nav-tabs { display: flex; gap: 0.4rem; justify-content: center; margin-bottom: 2rem; flex-wrap: wrap; }
  .nav-tab {
    font-family: 'JetBrains Mono', monospace; font-size: 0.78rem;
    padding: 0.55rem 1.2rem; border: 1px solid var(--border);
    background: var(--surface); color: var(--text-muted);
    border-radius: 4px; cursor: pointer; transition: all 0.15s;
    letter-spacing: 0.06em; text-transform: uppercase;
  }
  .nav-tab:hover { color: var(--text); border-color: var(--accent2); }
  .nav-tab.active { color: var(--accent); border-color: var(--accent); background: rgba(79,255,176,0.08); }
  .view { display: none; }
  .view.active { display: block; }

  /* Study view */
  .study-card {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 10px; padding: 2rem; min-height: 340px;
    display: flex; flex-direction: column; gap: 1.2rem;
  }
  .study-meta {
    font-family: 'JetBrains Mono', monospace; font-size: 0.7rem;
    color: var(--accent2); letter-spacing: 0.08em;
    display: flex; justify-content: space-between; flex-wrap: wrap; gap: 0.5rem;
  }
  .study-meta .due-tag { color: var(--text-muted); }
  .study-q { font-size: 1.1rem; line-height: 1.55; color: var(--text); }
  .study-opts { list-style: none; padding: 0; margin: 0.5rem 0; display: flex; flex-direction: column; gap: 0.4rem; }
  .study-opts li {
    font-family: 'JetBrains Mono', monospace; font-size: 0.82rem;
    padding: 0.55rem 0.9rem; border: 1px solid var(--border);
    border-radius: 4px; color: var(--text-muted); background: var(--bg);
    cursor: pointer; transition: all 0.12s; display: flex; align-items: center; gap: 0.6rem;
    user-select: none;
  }
  .study-opts li:hover:not(.locked) { border-color: var(--accent2); color: var(--text); }
  .study-opts li .box {
    width: 14px; height: 14px; border: 1px solid var(--border); border-radius: 3px;
    display: inline-flex; align-items: center; justify-content: center; flex-shrink: 0;
    font-size: 0.7rem; line-height: 1;
  }
  .study-opts li.selected { border-color: var(--accent2); color: var(--text); background: rgba(79,176,255,0.05); }
  .study-opts li.selected .box { background: var(--accent2); border-color: var(--accent2); color: #001; }
  .study-opts li.selected .box::before { content: '✓'; }
  .study-opts li.locked { cursor: default; }
  .study-opts li.correct { border-color: var(--accent); color: var(--accent); background: rgba(79,255,176,0.06); }
  .study-opts li.correct .box { background: var(--accent); border-color: var(--accent); color: #001; }
  .study-opts li.correct .box::before { content: '✓'; }
  .study-opts li.incorrect { border-color: #ff5a6e; color: #ff5a6e; background: rgba(255,90,110,0.06); }
  .study-opts li.incorrect .box { background: #ff5a6e; border-color: #ff5a6e; color: #001; }
  .study-opts li.incorrect .box::before { content: '✕'; }
  .study-opts li.missed { border-style: dashed; border-color: var(--accent); color: var(--accent); }
  .study-opts li.missed .box::before { content: '✓'; color: var(--accent); }
  .study-hint-line { font-family: 'JetBrains Mono', monospace; font-size: 0.7rem; color: var(--text-muted); }
  .auto-check-toggle {
    display: inline-flex; align-items: center; gap: 0.5rem; cursor: pointer;
    font-family: 'JetBrains Mono', monospace; font-size: 0.72rem;
    color: var(--text-muted); letter-spacing: 0.06em; text-transform: uppercase;
    padding: 0.4rem 0.7rem; border: 1px solid var(--border); border-radius: 4px;
    background: var(--surface); user-select: none; transition: all 0.15s;
  }
  .auto-check-toggle:hover { color: var(--text); border-color: var(--accent2); }
  .auto-check-toggle.on { color: var(--accent); border-color: var(--accent); background: rgba(79,255,176,0.06); }
  .auto-check-toggle .dot { width: 8px; height: 8px; border-radius: 50%; background: var(--border); }
  .auto-check-toggle.on .dot { background: var(--accent); box-shadow: 0 0 6px var(--accent); }
  .study-answer {
    border-top: 1px dashed var(--border); padding-top: 1rem;
    color: #b8f0d8; font-size: 0.88rem; line-height: 1.55;
  }
  .study-answer.wrong { color: #ffb0b8; }
  .study-answer .explain { color: var(--text-muted); font-size: 0.78rem; margin-top: 0.6rem; display: block; }
  .study-btn.check { border-color: var(--accent2); color: var(--accent2); width: 100%; }
  .study-btn.check:disabled { opacity: 0.4; cursor: not-allowed; }
  .study-btn.next { border-color: var(--accent2); color: var(--accent2); width: 100%; }
  .study-actions { margin-top: auto; display: flex; gap: 0.6rem; flex-wrap: wrap; }
  .study-btn {
    font-family: 'JetBrains Mono', monospace; font-size: 0.82rem;
    padding: 0.75rem 1.2rem; border: 1px solid var(--border);
    background: var(--surface2); color: var(--text);
    border-radius: 6px; cursor: pointer; transition: all 0.15s;
    letter-spacing: 0.05em; flex: 1; min-width: 110px;
    display: flex; flex-direction: column; align-items: center; gap: 0.2rem;
  }
  .study-btn:hover { transform: translateY(-1px); }
  .study-btn.reveal { border-color: var(--accent2); color: var(--accent2); width: 100%; }
  .study-btn.rate-1 { border-color: #ff5a6e; color: #ff5a6e; }
  .study-btn.rate-1:hover { background: rgba(255,90,110,0.1); }
  .study-btn.rate-2 { border-color: #ffb84f; color: #ffb84f; }
  .study-btn.rate-2:hover { background: rgba(255,184,79,0.1); }
  .study-btn.rate-3 { border-color: var(--accent); color: var(--accent); }
  .study-btn.rate-3:hover { background: rgba(79,255,176,0.1); }
  .study-btn .hint { font-size: 0.6rem; opacity: 0.7; letter-spacing: 0.06em; }
  .study-empty { text-align: center; padding: 4rem 1rem; color: var(--text-muted); font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; }

  /* Progress view */
  .progress-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 1rem; margin-bottom: 2rem; }
  .progress-stat { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 1rem 1.2rem; }
  .progress-stat .label { font-family: 'JetBrains Mono', monospace; font-size: 0.65rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.08em; }
  .progress-stat .value { font-size: 1.6rem; font-weight: 700; color: var(--text); margin-top: 0.3rem; }
  .progress-stat .value.accent { color: var(--accent); }
  .progress-stat .value.warn { color: #ffb84f; }
  .progress-stat .value.bad { color: #ff5a6e; }
  .chart-card { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 1.4rem; margin-bottom: 1.5rem; }
  .chart-card h3 { font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 1rem; font-weight: 600; }
  .chart-card svg { width: 100%; height: auto; display: block; }
  .chart-card .empty { text-align: center; color: var(--text-muted); font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; padding: 2rem 0; }
  .topic-row { display: flex; align-items: center; gap: 0.7rem; font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; padding: 0.4rem 0; }
  .topic-row .name { width: 140px; color: var(--text); flex-shrink: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .topic-row .bar-bg { flex: 1; height: 8px; background: var(--bg); border-radius: 4px; overflow: hidden; }
  .topic-row .bar-fill { height: 100%; background: linear-gradient(90deg, var(--accent2), var(--accent)); border-radius: 4px; }
  .topic-row .pct { color: var(--text-muted); width: 70px; text-align: right; }
  .reset-btn { font-family: 'JetBrains Mono', monospace; font-size: 0.7rem; padding: 0.45rem 1rem; background: transparent; border: 1px solid var(--border); color: var(--text-muted); border-radius: 4px; cursor: pointer; transition: all 0.15s; }
  .reset-btn:hover { border-color: #ff5a6e; color: #ff5a6e; }

  /* ============ EXAM MODE ============ */
  .exam-start-screen { text-align: center; padding: 3rem 1rem; }
  .exam-start-screen h2 { font-family: 'Syne', sans-serif; font-size: 2rem; font-weight: 800; color: var(--text); margin-bottom: 0.5rem; }
  .exam-start-screen .sub { color: var(--text-muted); font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; margin-bottom: 2rem; }
  .exam-info-cards { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; margin-bottom: 2.5rem; }
  .exam-info-card { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 1.2rem 1.8rem; min-width: 130px; }
  .exam-info-card .eic-label { font-family: 'JetBrains Mono', monospace; font-size: 0.62rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 0.4rem; }
  .exam-info-card .eic-val { font-size: 1.5rem; font-weight: 700; color: var(--accent); font-family: 'JetBrains Mono', monospace; }
  .exam-start-btn { background: var(--accent); color: #0d0f14; border: none; border-radius: 6px; padding: 0.9rem 2.8rem; font-family: 'Syne', sans-serif; font-weight: 700; font-size: 1rem; cursor: pointer; transition: opacity 0.15s; }
  .exam-start-btn:hover { opacity: 0.85; }

  .exam-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.5rem; padding-bottom: 1rem; border-bottom: 1px solid var(--border); flex-wrap: wrap; gap: 0.8rem; }
  .exam-timer { font-family: 'JetBrains Mono', monospace; font-size: 1.3rem; font-weight: 600; color: var(--accent); letter-spacing: 0.05em; }
  .exam-timer.warn { color: #ffb84f; }
  .exam-timer.danger { color: #ff5a6e; animation: pulse 1s infinite; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.5} }
  .exam-progress-bar { flex: 1; min-width: 120px; height: 6px; background: var(--bg); border-radius: 3px; overflow: hidden; }
  .exam-progress-fill { height: 100%; background: linear-gradient(90deg, var(--accent2), var(--accent)); border-radius: 3px; transition: width 0.3s; }
  .exam-qcount { font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; color: var(--text-muted); }
  .exam-nav { display: flex; gap: 0.5rem; justify-content: space-between; margin-top: 1.5rem; }
  .exam-nav-btn { font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; padding: 0.5rem 1.2rem; border-radius: 4px; border: 1px solid var(--border); background: transparent; color: var(--text-muted); cursor: pointer; transition: all 0.15s; }
  .exam-nav-btn:hover { border-color: var(--accent2); color: var(--text); }
  .exam-nav-btn.primary { border-color: var(--accent); color: var(--accent); }
  .exam-nav-btn.primary:hover { background: rgba(79,255,176,0.1); }
  .exam-nav-btn:disabled { opacity: 0.3; cursor: default; }
  .exam-submit-btn { background: var(--accent); color: #0d0f14; border: none; border-radius: 4px; padding: 0.5rem 1.4rem; font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; font-weight: 700; cursor: pointer; transition: opacity 0.15s; }
  .exam-submit-btn:hover { opacity: 0.85; }

  .exam-q-card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1.5rem; }
  .exam-q-num { font-family: 'JetBrains Mono', monospace; font-size: 0.65rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: 0.7rem; }
  .exam-q-text { font-size: 1rem; color: var(--text); line-height: 1.6; margin-bottom: 1.2rem; }
  .exam-q-img { max-width: 100%; border-radius: 6px; margin-bottom: 1rem; display: block; }
  .exam-option { display: flex; align-items: flex-start; gap: 0.7rem; padding: 0.65rem 0.9rem; border: 1px solid var(--border); border-radius: 6px; cursor: pointer; margin-bottom: 0.5rem; transition: all 0.12s; font-size: 0.88rem; color: var(--text); background: var(--bg); }
  .exam-option:hover { border-color: var(--accent2); background: rgba(58,143,255,0.06); }
  .exam-option.selected { border-color: var(--accent); background: rgba(79,255,176,0.08); color: var(--text); }
  .exam-option .opt-letter { font-family: 'JetBrains Mono', monospace; font-size: 0.72rem; color: var(--text-muted); min-width: 18px; margin-top: 1px; }
  .exam-option.selected .opt-letter { color: var(--accent); }
  .exam-dot-nav { display: flex; flex-wrap: wrap; gap: 0.35rem; margin: 1rem 0; }
  .exam-dot { width: 22px; height: 22px; border-radius: 50%; border: 1.5px solid var(--border); background: transparent; font-family: 'JetBrains Mono', monospace; font-size: 0.55rem; color: var(--text-muted); cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.12s; }
  .exam-dot.answered { background: var(--accent2); border-color: var(--accent2); color: #fff; }
  .exam-dot.current { border-color: var(--accent); box-shadow: 0 0 0 2px rgba(79,255,176,0.3); }

  /* Results screen */
  .exam-results { text-align: center; padding: 2rem 0; }
  .exam-score-ring { position: relative; display: inline-flex; align-items: center; justify-content: center; margin-bottom: 1.5rem; }
  .exam-score-ring svg { transform: rotate(-90deg); }
  .exam-score-inner { position: absolute; text-align: center; }
  .exam-score-pct { font-family: 'JetBrains Mono', monospace; font-size: 2rem; font-weight: 700; }
  .exam-score-label { font-family: 'JetBrains Mono', monospace; font-size: 0.65rem; color: var(--text-muted); }
  .exam-verdict { font-family: 'Syne', sans-serif; font-size: 1.6rem; font-weight: 800; margin-bottom: 0.4rem; }
  .exam-verdict.pass { color: #22c55e; }
  .exam-verdict.fail { color: #ff5a6e; }
  .exam-result-stats { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; margin: 1.5rem 0; }
  .exam-result-stat { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 0.9rem 1.4rem; min-width: 100px; }
  .exam-result-stat .ers-label { font-family: 'JetBrains Mono', monospace; font-size: 0.62rem; color: var(--text-muted); text-transform: uppercase; }
  .exam-result-stat .ers-val { font-size: 1.4rem; font-weight: 700; margin-top: 0.2rem; }
  .exam-review-btn { font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; padding: 0.6rem 1.5rem; border-radius: 5px; border: 1px solid var(--accent); color: var(--accent); background: transparent; cursor: pointer; margin: 0.4rem; transition: all 0.15s; }
  .exam-review-btn:hover { background: rgba(79,255,176,0.1); }
  .exam-retry-btn { font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; padding: 0.6rem 1.5rem; border-radius: 5px; background: var(--accent); color: #0d0f14; border: none; cursor: pointer; font-weight: 700; margin: 0.4rem; transition: opacity 0.15s; }
  .exam-retry-btn:hover { opacity: 0.85; }

  /* Review wrong answers */
  .exam-review-card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1.3rem; margin-bottom: 1rem; }
  .exam-review-card.wrong { border-left: 3px solid #ff5a6e; }
  .exam-review-q { font-size: 0.92rem; color: var(--text); margin-bottom: 0.8rem; line-height: 1.5; }
  .exam-review-your { font-family: 'JetBrains Mono', monospace; font-size: 0.72rem; color: #ff5a6e; margin-bottom: 0.3rem; }
  .exam-review-correct { font-family: 'JetBrains Mono', monospace; font-size: 0.72rem; color: #22c55e; margin-bottom: 0.5rem; }
  .exam-review-explain { font-family: 'JetBrains Mono', monospace; font-size: 0.70rem; color: var(--text-muted); line-height: 1.5; padding-top: 0.5rem; border-top: 1px solid var(--border); }

  /* ============ MATCHING SYSTEM ============ */
  .matching-container {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    margin: 0.6rem 0;
  }
  .matching-col {
    flex: 1;
    min-width: 180px;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
  .matching-col-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.62rem;
    color: var(--accent2);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 0.2rem;
  }
  .match-item {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem;
    padding: 0.55rem 0.8rem;
    border: 1px solid var(--border);
    border-radius: 4px;
    background: var(--bg);
    color: var(--text-muted);
    cursor: pointer;
    transition: all 0.15s;
    user-select: none;
    min-height: 40px;
    display: flex;
    align-items: center;
    line-height: 1.3;
  }
  .match-item:hover:not(.locked):not(.matched-done) {
    border-color: var(--accent2);
    color: var(--text);
  }
  .match-item.selected {
    border-color: var(--accent2);
    color: var(--text);
    background: rgba(58,143,255,0.08);
    box-shadow: 0 0 0 1px var(--accent2);
  }
  .match-item.matched-correct {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(79,255,176,0.06);
  }
  .match-item.matched-wrong {
    border-color: #ff5a6e;
    color: #ff5a6e;
    background: rgba(255,90,110,0.06);
  }
  .match-item.matched-done {
    cursor: default;
  }
  .match-item.reveal-correct {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(79,255,176,0.06);
  }
  .match-item.reveal-wrong {
    border-color: #ff5a6e;
    color: #ff5a6e;
    background: rgba(255,90,110,0.06);
  }
  .match-item.reveal-missed {
    border-style: dashed;
    border-color: var(--accent);
    color: var(--accent);
  }
  .match-connector {
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--text-muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.7rem;
    padding: 0 0.2rem;
    flex-shrink: 0;
    align-self: center;
    margin-top: 0.5rem;
  }
  .matching-score {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem;
    padding: 0.4rem 0.8rem;
    border-radius: 4px;
    margin-top: 0.4rem;
    text-align: center;
  }
  .matching-score.perfect { color: var(--accent); border: 1px solid var(--accent); background: rgba(79,255,176,0.06); }
  .matching-score.partial { color: #ffb84f; border: 1px solid #ffb84f; background: rgba(255,184,79,0.06); }
  .matching-score.wrong { color: #ff5a6e; border: 1px solid #ff5a6e; background: rgba(255,90,110,0.06); }
  .matching-pairs {
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
    margin-top: 0.6rem;
  }
  .matching-pair-row {
    display: flex;
    align-items: stretch;
    gap: 0;
    border-radius: 4px;
    overflow: hidden;
  }
  .matching-pair-left, .matching-pair-right {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.76rem;
    padding: 0.5rem 0.75rem;
    flex: 1;
    line-height: 1.35;
    border: 1px solid var(--border);
  }
  .matching-pair-left { border-right: none; border-radius: 4px 0 0 4px; background: var(--surface2); color: var(--text); }
  .matching-pair-right { border-radius: 0 4px 4px 0; background: var(--bg); }
  .matching-pair-arrow {
    font-size: 0.7rem;
    padding: 0.5rem 0.4rem;
    display: flex;
    align-items: center;
    background: var(--surface);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    color: var(--text-muted);
    flex-shrink: 0;
  }

</style>

</head>
<body>
<div class="wrapper">
  <header>
    <h1>Networking <span>Flashcards</span></h1>
    <p>// CLICK CARD TO REVEAL ANSWER</p>
  </header>

  <nav class="nav-tabs">
    <button class="nav-tab active" data-view="browse">// Browse</button>
    <button class="nav-tab" data-view="study">// Study</button>
    <button class="nav-tab" data-view="progress">// Progress</button>
    <button class="nav-tab" data-view="exam">// Exam</button>
  </nav>

  <!-- BROWSE VIEW -->
  <section class="view active" id="view-browse">
    <div class="stats">
      <div class="stat">Total: <strong id="total-count">0</strong></div>
      <div class="stat">Shown: <strong id="shown-count">0</strong></div>
      <div class="stat">Flipped: <strong id="flipped-count">0</strong></div>
      <div class="stat">Learned: <strong id="learned-count">0</strong></div>
    </div>

    <div class="search-wrap">
      <input type="text" id="search" placeholder="Search questions..." oninput="filterCards()">
    </div>

    <div class="filters" id="filters"></div>

    <h2 class="section-title">
      <span>// Learned <strong id="learned-count-inline">0</strong></span>
      <span class="line"></span>
      <span style="font-size:0.65rem;">drag cards here</span>
    </h2>
    <div class="learned-zone" id="learned-zone"></div>

    <h2 class="section-title">
      <span>// To Learn</span>
      <span class="line"></span>
    </h2>
    <div class="grid drop-target" id="grid"></div>
  </section>

  <!-- STUDY VIEW -->
  <section class="view" id="view-study">
    <div class="stats">
      <div class="stat">Due now: <strong id="study-due">0</strong></div>
      <div class="stat">Studied today: <strong id="study-today">0</strong></div>
      <div class="stat">Streak: <strong id="study-streak">0</strong>d</div>
      <label class="auto-check-toggle" id="auto-check-toggle" onclick="toggleAutoCheck()">
        <span class="dot"></span><span>Auto-check</span>
      </label>
    </div>
    <div id="study-container"></div>
  </section>

  <!-- PROGRESS VIEW -->
  <section class="view" id="view-progress">
    <div class="progress-grid">
      <div class="progress-stat"><div class="label">Total reviews</div><div class="value accent" id="p-total">0</div></div>
      <div class="progress-stat"><div class="label">Unique questions</div><div class="value" id="p-unique">0</div></div>
      <div class="progress-stat"><div class="label">Known well</div><div class="value accent" id="p-good">0</div></div>
      <div class="progress-stat"><div class="label">Need work</div><div class="value bad" id="p-bad">0</div></div>
      <div class="progress-stat"><div class="label">Current streak</div><div class="value warn" id="p-streak">0</div></div>
    </div>

    <div class="chart-card">
      <h3>Reviews per day (last 14 days)</h3>
      <div id="chart-daily"></div>
    </div>

    <div class="chart-card">
      <h3>Mastery by topic</h3>
      <div id="chart-topics"></div>
    </div>

    <div style="text-align:center;margin-top:1.5rem;">
      <button class="reset-btn" onclick="resetProgress()">// Reset all progress</button>
    </div>
  </section>

  <!-- ============ EXAM VIEW ============ -->
  <section class="view" id="view-exam">
    <div id="exam-start">
      <div class="exam-start-screen">
        <h2>// Exam Mode</h2>
        <p class="sub">Simulate the real CCNA 1 final exam</p>
        <div class="exam-info-cards">
          <div class="exam-info-card"><div class="eic-label">Questions</div><div class="eic-val">60</div></div>
          <div class="exam-info-card"><div class="eic-label">Time Limit</div><div class="eic-val">1:10:00</div></div>
          <div class="exam-info-card"><div class="eic-label">Pass Score</div><div class="eic-val">70%</div></div>
          <div class="exam-info-card"><div class="eic-label">To Pass</div><div class="eic-val">42/60</div></div>
        </div>
        <p style="font-family:'JetBrains Mono',monospace;font-size:0.72rem;color:var(--text-muted);margin-bottom:1.5rem;">
          60 questions are randomly selected from all 159 questions.<br>Questions with multiple correct answers require all correct answers selected.
        </p>
        <button class="exam-start-btn" onclick="startExam()">Start Exam →</button>
      </div>
    </div>
    <div id="exam-active" style="display:none;">
      <div class="exam-header">
        <div class="exam-timer" id="exam-timer">1:10:00</div>
        <div class="exam-progress-bar"><div class="exam-progress-fill" id="exam-progress-fill" style="width:0%"></div></div>
        <div class="exam-qcount" id="exam-qcount">Q 1 / 60</div>
        <button class="exam-nav-btn" onclick="submitExam()" style="border-color:#ffb84f;color:#ffb84f;">Submit</button>
      </div>
      <div class="exam-dot-nav" id="exam-dot-nav"></div>
      <div id="exam-q-area"></div>
      <div class="exam-nav">
        <button class="exam-nav-btn" id="exam-prev-btn" onclick="examNav(-1)">← Prev</button>
        <button class="exam-nav-btn primary" id="exam-next-btn" onclick="examNav(1)">Next →</button>
      </div>
    </div>
    <div id="exam-results" style="display:none;"></div>
  </section>

  <footer>
    <p>Cisco CCNA / ITN v7 — Tap any card to flip · Drag the ⋮⋮ handle to move</p>
  </footer>


</div>

<script>
const questions = [
  {
    num: 1,
    topic: "Protocols",
    q: "Which two traffic types use the Real-Time Transport Protocol (RTP)? (Choose two.)",
    opts: ["video", "web", "file transfer", "voice", "peer to peer"],
    a: ["video", "voice"],
    explain: "RTP is designed for end-to-end delivery of real-time audio and video. Voice and video traffic (VoIP, live streaming) use RTP because they prioritize low latency over guaranteed delivery."
  },
  {
    num: 2,
    topic: "Wireless",
    q: "Which wireless technology has low-power and data rate requirements making it popular in home automation applications?",
    opts: ["ZigBee", "LoRaWAN", "5G", "Wi-Fi"],
    a: ["ZigBee"],
    explain: "ZigBee (IEEE 802.15.4) is designed for personal-area networks with low energy, power, and data rate requirements — ideal for home automation devices."
  },
  {
    num: 3,
    topic: "TCP/IP Model",
    q: "Which layer of the TCP/IP model provides a route to forward messages through an internetwork?",
    opts: ["application", "network access", "internet", "transport"],
    a: ["internet"],
    explain: "The Internet layer of the TCP/IP model (corresponding to the OSI Network layer) addresses and routes messages through an internetwork."
  },
  {
    num: 4,
    topic: "DNS",
    q: "Which type of server relies on record types such as A, NS, AAAA, and MX in order to provide services?",
    opts: ["DNS", "email", "file", "web"],
    a: ["DNS"],
    explain: "A DNS server stores records: A (IPv4 address), NS (name server), AAAA (IPv6 address), MX (mail exchange)."
  },
  {
    num: 5,
    topic: "Protocols",
    q: "What are proprietary protocols?",
    opts: ["protocols developed by private organizations to operate on any vendor hardware", "protocols that can be freely used by any organization or vendor", "protocols developed by organizations who have control over their definition and operation", "a collection of protocols known as the TCP/IP protocol suite"],
    a: ["protocols developed by organizations who have control over their definition and operation"],
    explain: "Proprietary protocols have their definition and operation controlled by one company or vendor. TCP/IP is an open standard, not proprietary."
  },
  {
    num: 6,
    topic: "DNS",
    q: "What service is provided by DNS?",
    opts: ["Resolves domain names, such as cisco.com, into IP addresses.", "A basic set of rules for exchanging multimedia files on the web.", "Allows for data transfers between a client and a file server.", "Uses encryption to secure the exchange of web content."],
    a: ["Resolves domain names, such as cisco.com, into IP addresses."],
    explain: "DNS translates human-readable domain names (e.g. cisco.com) into numeric IP addresses required for network routing."
  },
  {
    num: 7,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 110. What service is the client requesting?",
    opts: ["DNS", "DHCP", "SMTP", "POP3"],
    a: ["POP3"],
    explain: "Port 110 is reserved for POP3 (Post Office Protocol v3) — used to retrieve email from a server to the client's local application."
  },
  {
    num: 8,
    topic: "Windows CLI",
    q: "What command can be used on a Windows PC to see the IP configuration of that computer?",
    opts: ["show ip interface brief", "ping", "show interfaces", "ipconfig"],
    a: ["ipconfig"],
    explain: "The ipconfig command displays the current IPv4/IPv6 address, subnet mask, and default gateway on a Windows PC. Use ipconfig /all for more details."
  },
  {
    num: 9,
    topic: "Network Models",
    q: "A wired laser printer is attached to a home computer. That printer has been shared so that other computers on the home network can also use the printer. What networking model is in use?",
    opts: ["client-based", "master-slave", "point-to-point", "peer-to-peer (P2P)"],
    a: ["peer-to-peer (P2P)"],
    explain: "P2P networks allow two or more devices to share resources (like printers or files) without a dedicated server."
  },
  {
    num: 10,
    topic: "Security",
    q: "What characteristic describes a virus?",
    opts: ["a network device that filters access and traffic coming into a network", "the use of stolen credentials to access private data", "an attack that slows or crashes a device or network service", "malicious software or code running on an end device"],
    a: ["malicious software or code running on an end device"],
    explain: "A virus is malicious software running on an end device that spreads by attaching to programs or files and activating when run."
  },
  {
    num: 11,
    topic: "QoS",
    q: "Three bank employees are using the corporate network. The first employee uses a web browser to view a company web page in order to read some announcements. The second employee accesses the corporate database to perform some financial transactions. The third employee participates in an important live audio conference with other corporate managers in branch offices. If QoS is implemented on this network, what will be the priorities from highest to lowest of the different data types?",
    opts: ["financial transactions, web page, audio conference", "audio conference, financial transactions, web page", "financial transactions, audio conference, web page", "audio conference, web page, financial transactions"],
    a: ["audio conference, financial transactions, web page"],
    explain: "QoS prioritizes voice (real-time, sensitive to delay) > transaction data > web page content."
  },
  {
    num: 12,
    topic: "IPv6",
    q: "Match the description to the IPv6 addressing component. (Not all options are used.)",
    opts: ["global routing prefix", "subnet ID", "interface ID", "anycast address", "link-local prefix"],
    a: ["the last 64 bits, identifies a specific device → interface ID", "used by the ISP to identify the customer network → global routing prefix", "used by the organization to identify subnets → subnet ID"],
    explain: "In an IPv6 address: the global routing prefix (first 48 bits) is assigned by the ISP to identify the network. The subnet ID (next 16 bits) is used by organizations to define subnets. The interface ID (last 64 bits) identifies the specific device.",
    img: "https://itexamanswers.net/wp-content/uploads/2022/10/2022-11-01_100633.png"
  },
  {
    num: 13,
    topic: "TCP/IP Model",
    q: "Refer to the exhibit. If Host1 were to transfer a file to the server, what layers of the TCP/IP model would be used?",
    opts: ["only application and Internet layers", "only Internet and network access layers", "only application, Internet, and network access layers", "application, transport, Internet, and network access layers"],
    a: ["application, transport, Internet, and network access layers"],
    explain: "File transfer (FTP) uses all four TCP/IP layers: application, transport, internet, and network access.",
    img: "https://itexamanswers.net/wp-content/uploads/2016/03/i275370v1n1_275370-2.png"
  },
  {
    num: 14,
    topic: "Switching",
    q: "Match the characteristic to the forwarding method. (Not all options are used.)",
    opts: ["low latency", "always stores the entire frame", "may forward runt frames", "begins forwarding when the destination address is received"],
    a: ["always stores the entire frame"],
    explain: "Store-and-forward always stores the entire frame, then checks the CRC and frame length before forwarding. Cut-through starts forwarding immediately (lower latency, may pass corrupted/runt frames).",
    img: "https://itexamanswers.net/wp-content/uploads/2016/03/2017-05-22_175719.jpg"
  },
  {
    num: 15,
    topic: "Routing",
    q: "Refer to the exhibit. The IP address of which device interface should be used as the default gateway setting of host H1?",
    opts: ["R1: S0/0/0", "R2: S0/0/1", "R1: G0/0", "R2: S0/0/0"],
    a: ["R1: G0/0"],
    explain: "The default gateway for a host is the router interface directly attached to the host's local LAN — in this case, R1's G0/0 interface.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/35.jpg"
  },
  {
    num: 16,
    topic: "Services",
    q: "What service is provided by Internet Messenger?",
    opts: ["An application that allows real-time chatting among remote users.", "Allows remote access to network devices and servers.", "Resolves domain names into IP addresses.", "Uses encryption to provide secure remote access."],
    a: ["An application that allows real-time chatting among remote users."],
    explain: "Internet Messenger (instant messaging) enables real-time synchronous text communication between remote users."
  },
  {
    num: 17,
    topic: "Subnetting",
    q: "Refer to the exhibit. Match the network with the correct IP address and prefix that will satisfy the usable host addressing requirements for each network.",
    opts: [],
    a: ["192.168.0.128 /25 → Network A", "192.168.0.0 /26 → Network B", "192.168.0.96 /27 → Network C", "192.168.0.80 /30 → Network D"],
    explain: "Network A needs 192.168.0.128/25 (128 hosts). Network B needs 192.168.0.0/26 (64 hosts). Network C needs 192.168.0.96/27 (32 hosts). Network D needs 192.168.0.80/30 (4 hosts).",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i304956v6n1_207918.png"
  },
  {
    num: 18,
    topic: "ARP",
    q: "Refer to the exhibit. Which protocol was responsible for building the table that is shown?",
    opts: ["DHCP", "ARP", "DNS", "ICMP"],
    a: ["ARP"],
    explain: "The 'arp -a' command on a Windows PC displays the ARP table — mappings of IP addresses to MAC addresses.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/2021-03-22_150538.jpg"
  },
  {
    num: 19,
    topic: "Cabling",
    q: "A network administrator notices that some newly installed Ethernet cabling is carrying corrupt and distorted data signals. The new cabling was installed in the ceiling close to fluorescent lights and electrical equipment. Which two factors may interfere with the copper cabling and result in signal distortion and data corruption? (Choose two.)",
    opts: ["crosstalk", "extended length of cabling", "RFI", "EMI", "signal attenuation"],
    a: ["RFI", "EMI"],
    explain: "EMI (electromagnetic interference) and RFI (radio frequency interference) from external sources like fluorescent lights and motors can corrupt data signals on copper cabling."
  },
  {
    num: 20,
    topic: "ARP",
    q: "A host is trying to send a packet to a device on a remote LAN segment, but there are currently no mappings in its ARP cache. How will the device obtain a destination MAC address?",
    opts: ["It will use its own MAC address as destination.", "It will send an ARP request for the destination device's MAC.", "It will send the frame with a broadcast MAC address.", "It will query DNS for the MAC address.", "It will send an ARP request for the MAC address of the default gateway."],
    a: ["It will send an ARP request for the MAC address of the default gateway."],
    explain: "For remote destinations, the host sends an ARP request for its default gateway's MAC address — not the final destination — since Layer 2 frames are local only."
  },
  {
    num: 21,
    topic: "Transport Layer",
    q: "A client packet is received by a server. The packet has a destination port number of 21. What service is the client requesting?",
    opts: ["FTP", "LDAP", "SLP", "SNMP"],
    a: ["FTP"],
    explain: "Port 21 is the well-known port used by FTP (File Transfer Protocol) for its control connection, used to send commands to the server and receive replies."
  },
  {
    num: 22,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 53. What service is the client requesting?",
    opts: ["DNS", "NetBIOS", "POP3", "IMAP"],
    a: ["DNS"],
    explain: "Port 53 is reserved for DNS (Domain Name System) — used to resolve domain names to IP addresses."
  },
  {
    num: 23,
    topic: "Subnetting",
    q: "A network administrator is adding a new LAN to a branch office. The new LAN must support 25 connected devices. What is the smallest network mask that the network administrator can use for the new network?",
    opts: ["255.255.255.128", "255.255.255.192", "255.255.255.224", "255.255.255.240"],
    a: ["255.255.255.224"],
    explain: "/27 (255.255.255.224) provides 2⁵−2 = 30 usable host addresses — enough for 25 devices. /28 only gives 14 usable addresses."
  },
  {
    num: 24,
    topic: "Security",
    q: "What characteristic describes a Trojan horse?",
    opts: ["malicious software or code running on an end device", "an attack that slows or crashes a device or network service", "the use of stolen credentials to access private data", "a network device that filters access and traffic coming into a network"],
    a: ["malicious software or code running on an end device"],
    explain: "A Trojan horse disguises itself as legitimate software. Once executed by the user, it performs harmful actions like data theft or creating backdoors."
  },
  {
    num: 25,
    topic: "Services",
    q: "What service is provided by HTTPS?",
    opts: ["Uses encryption to provide secure remote access to network devices.", "Resolves domain names into IP addresses.", "Uses encryption to secure the exchange of text, graphic images, sound, and video on the web.", "Allows remote access to network devices and servers."],
    a: ["Uses encryption to secure the exchange of text, graphic images, sound, and video on the web."],
    explain: "HTTPS uses SSL/TLS to encrypt web traffic (text, images, video) between client and server, protecting against interception."
  },
  {
    num: 26,
    topic: "Ports",
    q: "A technician with a PC is using multiple applications while connected to the Internet. How is the PC able to keep track of the data flow between multiple application sessions and have each application receive the correct packet flows?",
    opts: ["Based on the destination MAC address.", "Based on the source port number used by each application.", "Based on the source IP address.", "Based on the destination IP address."],
    a: ["Based on the source port number used by each application."],
    explain: "Each application gets a unique randomly-generated source port number to track individual sessions simultaneously."
  },
  {
    num: 27,
    topic: "Subnetting",
    q: "A network administrator is adding a new LAN to a branch office. The new LAN must support 61 connected devices. What is the smallest network mask that the network administrator can use for the new network?",
    opts: ["255.255.255.240", "255.255.255.224", "255.255.255.192", "255.255.255.128"],
    a: ["255.255.255.192"],
    explain: "/26 (255.255.255.192) provides 2⁶−2 = 62 usable host addresses — just enough for 61 devices."
  },
  {
    num: 28,
    topic: "Subnetting",
    q: "Refer to the exhibit. Match the network with the correct IP address and prefix that will satisfy the usable host addressing requirements for each network. (Not all options are used.)",
    opts: [],
    a: ["192.168.0.128 /25 → Network A", "192.168.0.0 /26 → Network B", "192.168.0.96 /27 → Network C", "192.168.0.80 /30 → Network D"],
    explain: "Same subnetting match as Q17: A=/25, B=/26, C=/27, D=/30.",
    img: "https://itexamanswers.net/wp-content/uploads/2016/03/i207918v1n1_207918-1-1.jpg"
  },
  {
    num: 29,
    topic: "Security",
    q: "What characteristic describes a DoS attack?",
    opts: ["the use of stolen credentials to access private data", "a network device that filters access and traffic coming into a network", "software that is installed on a user device and collects information", "an attack that slows or crashes a device or network service"],
    a: ["an attack that slows or crashes a device or network service"],
    explain: "A Denial of Service (DoS) attack floods resources to prevent legitimate users from accessing network services."
  },
  {
    num: 30,
    topic: "Transport Layer",
    q: "Match the application protocols to the correct transport protocols.",
    opts: [],
    a: ["FTP → TCP", "HTTP → TCP", "TFTP → UDP", "DHCP → UDP", "DNS → UDP (TCP for zone transfer)"],
    explain: "TCP provides reliable delivery for FTP, HTTP, SMTP, Telnet. UDP provides low-overhead delivery for TFTP, DHCP, DNS, SNMP."
  },
  {
    num: 31,
    topic: "Services",
    q: "What service is provided by SMTP?",
    opts: ["Allows clients to send email to a mail server and servers to send email to other servers.", "Allows remote access to network devices and servers.", "Uses encryption to provide secure remote access.", "An application for real-time chatting among remote users."],
    a: ["Allows clients to send email to a mail server and servers to send email to other servers."],
    explain: "SMTP (Simple Mail Transfer Protocol) handles outgoing email delivery from clients to servers and between mail servers."
  },
  {
    num: 32,
    topic: "Transport Layer",
    q: "Which scenario describes a function provided by the transport layer?",
    opts: ["A student using a VoIP phone — the unique identifier burned into the phone is a transport layer address.", "A student playing a web-based movie — the movie and sound are encoded within the transport layer header.", "A student has two web browser windows open — the transport layer ensures the correct web page is delivered to the correct browser window.", "A worker accessing a web server — the transport layer formats the screen so the web page appears properly."],
    a: ["A student has two web browser windows open — the transport layer ensures the correct web page is delivered to the correct browser window."],
    explain: "The transport layer uses source and destination port numbers to direct data to the correct application and window."
  },
  {
    num: 33,
    topic: "IPv6",
    q: "Refer to the exhibit. Host B on subnet Teachers transmits a packet to host D on subnet Students. Which Layer 2 and Layer 3 addresses are contained in the PDUs that are transmitted from host B to the router?",
    opts: [],
    a: ["Layer 2 destination = 00-00-0c-94-36-ab (router MAC); Layer 2 source = 00-00-0c-94-36-bb (Host B MAC); Layer 3 destination = 172.16.20.200 (Host D); Layer 3 source = 172.16.10.200 (Host B)"],
    explain: "Crossing subnets: Layer 3 addresses remain end-to-end (B → D). Layer 2 destination is the default gateway MAC (router), source is Host B's MAC.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i204796v11n1_204796-TOPOLOGY-ARP.png"
  },
  {
    num: 34,
    topic: "Cabling",
    q: "What does the term “attenuation” mean in data communication?",
    opts: ["strengthening of a signal by a networking device", "leakage of signals from one cable pair to another", "time for a signal to reach its destination", "loss of signal strength as distance increases"],
    a: ["loss of signal strength as distance increases"],
    explain: "Attenuation is the deterioration of signal strength over distance on copper cables."
  },
  {
    num: 35,
    topic: "IOS CLI",
    q: "Refer to the exhibit. An administrator is trying to configure the switch but receives the error message that is displayed in the exhibit. What is the problem?",
    opts: ["The entire command 'configure terminal' must be used.", "The administrator is already in global configuration mode.", "The administrator must first enter privileged EXEC mode before issuing the command.", "The administrator must connect via the console port."],
    a: ["The administrator must first enter privileged EXEC mode before issuing the command."],
    explain: "The '>' prompt indicates User EXEC mode. 'configure terminal' requires Privileged EXEC mode (entered with 'enable'). The '#' prompt confirms privileged mode.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i208399v1n1_Question-1.png"
  },
  {
    num: 36,
    topic: "TCP/IP Model",
    q: "Which two protocols operate at the top layer of the TCP/IP protocol suite? (Choose two.)",
    opts: ["TCP", "IP", "UDP", "POP", "DNS", "Ethernet"],
    a: ["POP", "DNS"],
    explain: "The top layer of TCP/IP is the application layer. DNS and POP operate here. TCP/UDP are transport layer; IP is internet layer; Ethernet is network access layer."
  },
  {
    num: 37,
    topic: "Security",
    q: "A company has a file server that shares a folder named Public. The network security policy specifies that the Public folder is assigned Read-Only rights to anyone who can log into the server while the Edit rights are assigned only to the network admin group. Which component is addressed in the AAA network service framework?",
    opts: ["automation", "accounting", "authentication", "authorization"],
    a: ["authorization"],
    explain: "Authorization determines what resources and operations (read/edit) an authenticated user can access after logging in."
  },
  {
    num: 38,
    topic: "Protocols",
    q: "What three requirements are defined by the protocols used in network communcations to allow message transmission across a network? (Choose three.)",
    opts: ["message size", "message encoding", "connector specifications", "media selection", "delivery options", "end-device installation"],
    a: ["message size", "message encoding", "delivery options"],
    explain: "Protocols define message encoding (format conversion), message size (breaking into manageable pieces), and delivery options (unicast, multicast, broadcast)."
  },
  {
    num: 39,
    topic: "IPv4",
    q: "What are two characteristics of IP? (Choose two.)",
    opts: ["does not require a dedicated end-to-end connection", "operates independently of the network media", "retransmits packets if errors occur", "re-assembles out of order packets into correct order at receiver", "guarantees delivery of packets"],
    a: ["does not require a dedicated end-to-end connection", "operates independently of the network media"],
    explain: "IP is connectionless (no dedicated end-to-end connection), best-effort (no guaranteed delivery), and media independent."
  },
  {
    num: 40,
    topic: "Network Characteristics",
    q: "An employee of a large corporation remotely logs into the company using the appropriate username and password. The employee is attending an important video conference with a customer concerning a large sale. It is important for the video quality to be excellent during the meeting. The employee is unaware that after a successful login, the connection to the company ISP failed. The secondary connection, however, activated within seconds. The disruption was not noticed by the employee or other employees. What three network characteristics are described in this scenario? (Choose three.)",
    opts: ["security", "quality of service", "scalability", "powerline networking", "integrity", "fault tolerance"],
    a: ["security", "quality of service", "fault tolerance"],
    explain: "Username/password = security. Excellent video = QoS. Seamless ISP failover = fault tolerance."
  },
  {
    num: 41,
    topic: "Cabling",
    q: "What are two common causes of signal degradation when using UTP cabling? (Choose two.)",
    opts: ["improper termination", "low-quality shielding in cable", "installing cables in conduit", "low-quality cable or connectors", "loss of light over long distances"],
    a: ["improper termination", "low-quality cable or connectors"],
    explain: "Improper termination and low-quality cable/connectors are primary causes of UTP signal degradation."
  },
  {
    num: 42,
    topic: "Subnetting",
    q: "Which subnet would include the address 192.168.1.96 as a usable host address?",
    opts: ["192.168.1.64/26", "192.168.1.32/27", "192.168.1.32/28", "192.168.1.64/29"],
    a: ["192.168.1.64/26"],
    explain: "192.168.1.64/26 has host range 192.168.1.65–192.168.1.126, which includes .96."
  },
  {
    num: 43,
    topic: "Subnetting",
    q: "Refer to the exhibit. On the basis of the output, which two statements about network connectivity are correct? (Choose two.)",
    opts: ["This host does not have a default gateway configured.", "There are 4 hops between this device and the device at 192.168.100.1.", "There is connectivity between this device and the device at 192.168.100.1.", "The connectivity between these two hosts allows for videoconferencing calls.", "The average transmission time between the two hosts is 2 milliseconds."],
    a: ["There are 4 hops between this device and the device at 192.168.100.1.", "There is connectivity between this device and the device at 192.168.100.1."],
    explain: "The output displays a successful Layer 3 connection between a host computer and a host at 19.168.100.1. It can be determined that 4 hops exist between them and the average transmission time is 1 milliseconds. Layer 3 connectivity does not necessarily mean that an application can run between the hosts.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i275429v1n1_chapter-9-WAN-images2.jpg"
  },
  {
    num: 44,
    topic: "Troubleshooting",
    q: "Which two statements describe how to assess traffic flow patterns and network traffic types using a protocol analyzer? (Choose two.)",
    opts: ["Capture traffic on weekends when most employees are off.", "Capture traffic during peak utilization times.", "Only capture traffic in areas like the data center.", "Perform the capture on different network segments.", "Only capture WAN traffic."],
    a: ["Capture traffic during peak utilization times.", "Perform the capture on different network segments."],
    explain: "Capture during peak times for a representative sample, and on different segments since some traffic is local to specific segments."
  },
  {
    num: 45,
    topic: "IPv6",
    q: "What is the consequence of configuring a router with the ipv6 unicast-routing global configuration command?​",
    opts: ["All router interfaces will be automatically activated.", "IPv6 enabled router interfaces begin sending ICMPv6 Router Advertisement messages.", "Each router interface will generate an IPv6 link-local address.", "It statically creates a global unicast address on this router."],
    a: ["IPv6 enabled router interfaces begin sending ICMPv6 Router Advertisement messages."],
    explain: "This command enables IPv6 routing. IPv6-enabled interfaces then send ICMPv6 Router Advertisements to help hosts with SLAAC."
  },
  {
    num: 46,
    topic: "OSI Model",
    q: "Which three layers of the OSI model map to the application layer of the TCP/IP model? (Choose three.)",
    opts: ["application", "network", "data link", "session", "presentation", "transport"],
    a: ["application", "session", "presentation"],
    explain: "OSI layers 5 (session), 6 (presentation), and 7 (application) all map to the TCP/IP application layer."
  },
  {
    num: 47,
    topic: "Routing",
    q: "Refer to the exhibit. If PC1 is sending a packet to PC2 and routing has been configured between the two routers, what will R1 do with the Ethernet frame header attached by PC1?",
    opts: ["nothing, because the router has a route", "open the header and determine if data goes out S0/0/0", "open the header and replace destination MAC address", "remove the Ethernet header and configure a new Layer 2 header before sending it out S0/0/0"],
    a: ["remove the Ethernet header and configure a new Layer 2 header before sending it out S0/0/0"],
    explain: "Routers strip the Layer 2 frame header and create a new one appropriate for the next network segment (e.g., serial link).",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i271152v1n1_271152.png"
  },
  {
    num: 48,
    topic: "Routing",
    q: "What will happen if the default gateway address is incorrectly configured on a host?",
    opts: ["The host cannot communicate with other hosts in the local network.", "The host cannot communicate with hosts in other networks.", "A ping to 127.0.0.1 would not be successful.", "The host will use ARP to determine the correct gateway address.", "The switch will not forward packets initiated by the host."],
    a: ["The host cannot communicate with hosts in other networks."],
    explain: "With an incorrect default gateway, the host can still communicate locally but cannot reach remote networks."
  },
  {
    num: 49,
    topic: "ARP",
    q: "What are two features of ARP? (Choose two.)",
    opts: ["Hosts refer to the MAC address table to map IP to MAC.", "ARP request contains the IP and multicast MAC of the destination.", "If a host needs to send to a local device and has IP but no MAC, it generates an ARP broadcast.", "If no device responds to ARP request, originating node broadcasts the data packet.", "If a device receiving ARP request has the destination IPv4 address, it responds with an ARP reply."],
    a: ["If a host needs to send to a local device and has IP but no MAC, it generates an ARP broadcast.", "If a device receiving ARP request has the destination IPv4 address, it responds with an ARP reply."],
    explain: "ARP works by: (1) host broadcasts when MAC is unknown; (2) the target device replies with its MAC address."
  },
  {
    num: 50,
    topic: "Subnetting",
    q: "A network administrator is adding a new LAN to a branch office. The new LAN must support 90 connected devices. What is the smallest network mask that the network administrator can use for the new network?",
    opts: ["255.255.255.128", "255.255.255.240", "255.255.255.248", "255.255.255.224"],
    a: ["255.255.255.128"],
    explain: "/25 (255.255.255.128) provides 2⁷−2 = 126 usable host addresses — sufficient for 90 devices."
  },
  {
    num: 51,
    topic: "IPv6",
    q: "What are two ICMPv6 messages that are not present in ICMP for IPv4? (Choose two.)",
    opts: ["Neighbor Solicitation", "Destination Unreachable", "Host Confirmation", "Time Exceeded", "Router Advertisement", "Route Redirection"],
    a: ["Neighbor Solicitation", "Router Advertisement"],
    explain: "ICMPv6 introduces Neighbor Solicitation and Router Advertisement as part of the Neighbor Discovery Protocol (NDP) — not present in ICMPv4."
  },
  {
    num: 52,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 80. What service is the client requesting?",
    opts: ["DHCP", "SMTP", "DNS", "HTTP"],
    a: ["HTTP"],
    explain: "Port 80 is reserved for HTTP — the protocol used to transfer web pages from servers to browsers."
  },
  {
    num: 53,
    topic: "Services",
    q: "What is an advantage for small organizations of adopting IMAP instead of POP?",
    opts: ["POP only allows centralized storage while IMAP allows distributed storage.", "Messages are kept in the mail servers until they are manually deleted from the email client.", "IMAP keeps messages on the server for a long time while POP only keeps copies briefly.", "IMAP sends and retrieves email, but POP only retrieves email."],
    a: ["Messages are kept in the mail servers until they are manually deleted from the email client."],
    explain: "IMAP keeps messages on the server until manually deleted, allowing access from multiple devices. POP typically downloads and removes messages from the server."
  },
  {
    num: 54,
    topic: "Troubleshooting",
    q: "A technician can ping the IP address of the web server of a remote company but cannot successfully ping the URL address of the same web server. Which software utility can the technician use to diagnose the problem?",
    opts: ["tracert", "ipconfig", "netstat", "nslookup"],
    a: ["nslookup"],
    explain: "nslookup manually queries DNS name servers to resolve hostnames — useful for diagnosing DNS resolution issues."
  },
  {
    num: 55,
    topic: "Data Link Layer",
    q: "Which two functions are performed at the LLC sublayer of the OSI Data Link Layer to facilitate Ethernet communication? (Choose two.)",
    opts: ["implements CSMA/CD over legacy shared half-duplex media", "enables IPv4 and IPv6 to utilize the same physical medium", "implements a process to delimit fields within an Ethernet 2 frame", "places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame", "applies source and destination MAC addresses to Ethernet frame"],
    a: ["enables IPv4 and IPv6 to utilize the same physical medium", "places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame"],
    explain: "LLC identifies the Layer 3 protocol being encapsulated (allowing IPv4 and IPv6 to share the same medium). MAC handles addressing, delimiting, and error detection."
  },
  {
    num: 56,
    topic: "Switching",
    q: "The global configuration command ip default-gateway 172.16.100.1 is applied to a switch. What is the effect of this command?",
    opts: ["The switch can communicate with other hosts on the 172.16.100.0 network.", "The switch can be remotely managed from a host on another network.", "The switch is limited to sending and receiving frames to/from the gateway.", "The switch will have a management interface with the address 172.16.100.1."],
    a: ["The switch can be remotely managed from a host on another network."],
    explain: "Configuring a default gateway on a switch allows it to communicate beyond its local network, enabling remote management."
  },
  {
    num: 57,
    topic: "Security",
    q: "What happens when the transport input ssh command is entered on the switch vty lines?",
    opts: ["The SSH client on the switch is enabled.", "The switch requires a username/password for remote access.", "Communication between the switch and remote users is encrypted.", "The switch requires remote connections via proprietary client software."],
    a: ["Communication between the switch and remote users is encrypted."],
    explain: "'transport input ssh' encrypts all inbound connections to the VTY lines, replacing insecure Telnet with SSH."
  },
  {
    num: 58,
    topic: "Security",
    q: "Match the type of threat with the cause. (Not all options are used.)",
    opts: [],
    a: ["electrical → voltage spikes, brownouts, noise, total power loss", "hardware → physical damage to devices/cabling", "environmental → temperature/humidity extremes", "maintenance → poor handling (ESD), lack of spares, poor cabling/labeling"],
    explain: "Cisco classifies physical threats into four categories: hardware, environmental, electrical, and maintenance.",
    img: "https://itexamanswers.net/wp-content/uploads/2019/12/2024-11-19_103653.jpg"
  },
  {
    num: 59,
    topic: "Security",
    q: "A disgruntled employee is using some free wireless networking tools to determine information about the enterprise wireless networks. This person is planning on using this information to hack the wireless network. What type of attack is this?",
    opts: ["DoS", "access", "reconnaissance", "Trojan horse"],
    a: ["reconnaissance"],
    explain: "Reconnaissance attacks involve unauthorized discovery and documentation of networks, systems, and vulnerabilities before launching an actual attack."
  },
  {
    num: 60,
    topic: "Services",
    q: "What service is provided by HTTP?",
    opts: ["Uses encryption to secure the exchange of web content.", "Allows for data transfers between a client and a file server.", "An application for real-time chatting among remote users.", "A basic set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the web."],
    a: ["A basic set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the web."],
    explain: "HTTP defines rules for how web browsers and servers exchange content including text, images, sound, and video."
  },
  {
    num: 61,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 67. What service is the client requesting?",
    opts: ["FTP", "DHCP", "Telnet", "SSH"],
    a: ["DHCP"],
    explain: "Port 67 is the DHCP server port. Clients send requests to port 67 to obtain automatic IP configuration."
  },
  {
    num: 62,
    topic: "ARP",
    q: "What are two problems that can be caused by a large number of ARP request and reply messages? (Choose two.)",
    opts: ["Switches become overloaded because they concentrate all traffic from subnets.", "The ARP request is sent as a broadcast, and will flood the entire subnet.", "The network may become overloaded due to large ARP reply payloads.", "A large number of ARPs may slow down switching, causing the switch to change its MAC table.", "All ARP request messages must be processed by all nodes on the local network."],
    a: ["The ARP request is sent as a broadcast, and will flood the entire subnet.", "All ARP request messages must be processed by all nodes on the local network."],
    explain: "ARP requests flood the subnet as broadcasts, and all nodes must process them (CPU interrupt) — both create overhead."
  },
  {
    num: 63,
    topic: "Troubleshooting",
    q: "A group of Windows PCs in a new subnet has been added to an Ethernet network. When testing the connectivity, a technician finds that these PCs can access local network resources but not the Internet resources. To troubleshoot the problem, the technician wants to initially confirm the IP address and DNS configurations on the PCs, and also verify connectivity to the local router. Which three Windows CLI commands and utilities will provide the necessary information? (Choose three.)",
    opts: ["netsh interface ipv6 show neighbor", "arp -a", "tracert", "ping", "ipconfig", "nslookup", "telnet"],
    a: ["ping", "ipconfig", "nslookup"],
    explain: "ipconfig verifies IP/DNS config; nslookup checks DNS resolution; ping verifies connectivity to the default gateway."
  },
  {
    num: 64,
    topic: "Routing",
    q: "During the process of forwarding traffic, what will the router do immediately after matching the destination IP address to a network on a directly connected routing table entry?",
    opts: ["analyze the destination IP address", "switch the packet to the directly connected interface", "look up the next-hop address for the packet", "discard the traffic after consulting the route table"],
    a: ["switch the packet to the directly connected interface"],
    explain: "When the destination is on a directly connected interface, the packet is immediately switched to that interface."
  },
  {
    num: 65,
    topic: "Security",
    q: "What characteristic describes antispyware?",
    opts: ["applications that protect end devices from becoming infected with malicious software", "a network device that filters access and traffic coming into a network", "software on a router that filters traffic based on IP addresses or applications", "a tunneling protocol that provides remote users with secure access"],
    a: ["applications that protect end devices from becoming infected with malicious software"],
    explain: "Antispyware protects end devices (PCs, smartphones) from malicious software that secretly collects user information."
  },
  {
    num: 66,
    topic: "Security",
    q: "A network administrator needs to keep the user ID, password, and session contents private when establishing remote CLI connectivity with a switch to manage it. Which access method should be chosen?",
    opts: ["Telnet", "AUX", "SSH", "Console"],
    a: ["SSH"],
    explain: "SSH (Secure Shell) encrypts all session data including credentials, unlike Telnet which sends everything in plaintext."
  },
  {
    num: 67,
    topic: "Security",
    q: "What are the two most effective ways to defend against malware? (Choose two.)",
    opts: ["Implement a VPN.", "Implement network firewalls.", "Implement RAID.", "Implement strong passwords.", "Update the operating system and other application software.", "Install and update antivirus software."],
    a: ["Update the operating system and other application software.", "Install and update antivirus software."],
    explain: "Keeping software updated patches vulnerabilities; antivirus software detects and removes malware — the two most direct defenses."
  },
  {
    num: 68,
    topic: "Security",
    q: "Which type of security threat would be responsible if a spreadsheet add-on disables the local software firewall?",
    opts: ["brute-force attack", "Trojan horse", "DoS", "buffer overflow"],
    a: ["Trojan horse"],
    explain: "A Trojan horse hides harmful functionality inside legitimate-looking software (like a spreadsheet add-on)."
  },
  {
    num: 69,
    topic: "Data Link Layer",
    q: "Which frame field is created by a source node and used by a destination node to ensure that a transmitted data signal has not been altered by interference, distortion, or signal loss?",
    opts: ["User Datagram Protocol field", "transport layer error check field", "flow control field", "frame check sequence field", "error correction process field"],
    a: ["frame check sequence field"],
    explain: "The FCS (Frame Check Sequence) field uses a CRC calculation. If the destination's calculation doesn't match, the frame is discarded."
  },
  {
    num: 70,
    topic: "Subnetting",
    q: "A network administrator is adding a new LAN to a branch office. The new LAN must support 4 connected devices. What is the smallest network mask that the network administrator can use for the new network?",
    opts: ["255.255.255.248", "255.255.255.0", "255.255.255.128", "255.255.255.192"],
    a: ["255.255.255.248"],
    explain: "/29 (255.255.255.248) provides 2³−2 = 6 usable host addresses — the smallest that fits 4 devices."
  },
  {
    num: 71,
    topic: "Services",
    q: "What service is provided by POP3?",
    opts: ["Retrieves email from the server by downloading it to the local mail application of the client.", "An application that allows real-time chatting among remote users.", "Allows remote access to network devices and servers.", "Uses encryption to provide secure remote access."],
    a: ["Retrieves email from the server by downloading it to the local mail application of the client."],
    explain: "POP3 (port 110) downloads email from the server to the client's local application, typically deleting it from the server."
  },
  {
    num: 72,
    topic: "Security",
    q: "What two security solutions are most likely to be used only in a corporate environment? (Choose two.)",
    opts: ["antispyware", "virtual private networks", "intrusion prevention systems", "strong passwords", "antivirus software"],
    a: ["virtual private networks", "intrusion prevention systems"],
    explain: "IPS monitors and stops network threats in real time; VPNs provide encrypted remote access — both are corporate-level solutions."
  },
  {
    num: 73,
    topic: "Security",
    q: "What characteristic describes antivirus software?",
    opts: ["applications that protect end devices from becoming infected with malicious software", "a network device that filters access and traffic coming into a network", "a tunneling protocol that provides remote users with secure access", "software on a router that filters traffic based on IP addresses or applications"],
    a: ["applications that protect end devices from becoming infected with malicious software"],
    explain: "Antivirus software protects end devices (PCs, smartphones) from viruses, worms, and other malware."
  },
  {
    num: 74,
    topic: "IPv4",
    q: "What mechanism is used by a router to prevent a received IPv4 packet from traveling endlessly on a network?",
    opts: ["Checks TTL field; if 0, discards and sends Destination Unreachable.", "Checks TTL field; if 100, discards and sends Destination Unreachable.", "Decrements TTL by 1; if result is 0, discards and sends Time Exceeded message.", "Increments TTL by 1; if result is 100, discards and sends Parameter Problem."],
    a: ["Decrements TTL by 1; if result is 0, discards and sends Time Exceeded message."],
    explain: "Routers decrement the TTL field by 1. When it reaches 0, the packet is discarded and an ICMP Time Exceeded message is sent to the source."
  },
  {
    num: 75,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 69. What service is the client requesting?",
    opts: ["DNS", "DHCP", "SMTP", "TFTP"],
    a: ["TFTP"],
    explain: "Port 69 is reserved for TFTP (Trivial File Transfer Protocol) — a simple, connectionless UDP-based file transfer protocol."
  },
  {
    num: 76,
    topic: "Security",
    q: "An administrator defined a local user account with a secret password on router R1 for use with SSH. Which three additional steps are required to configure R1 to accept only encrypted SSH connections? (Choose three.)",
    opts: ["Configure DNS on the router.", "Generate two-way pre-shared keys.", "Configure the IP domain name on the router.", "Generate the SSH keys.", "Enable inbound vty SSH sessions.", "Enable inbound vty Telnet sessions."],
    a: ["Configure the IP domain name on the router.", "Generate the SSH keys.", "Enable inbound vty SSH sessions."],
    explain: "After creating the user: (1) set ip domain-name, (2) generate RSA keys with 'crypto key generate rsa', (3) set 'transport input ssh' on vty lines."
  },
  {
    num: 77,
    topic: "Automation",
    q: "Which two functions are performed at the MAC sublayer of the OSI Data Link Layer to facilitate Ethernet communication? (Choose two.)",
    opts: ["handles communication between upper layer networking software and Ethernet NIC hardware", "places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame", "adds Ethernet control information to network protocol data", "enables IPv4 and IPv6 to utilize the same physical medium", "implements trailer with frame check sequence for error detection", "implements a process to delimit fields within an Ethernet 2 frame"],
    a: ["implements trailer with frame check sequence for error detection", "implements a process to delimit fields within an Ethernet 2 frame"],
    explain: "MAC sublayer = data encapsulation (frame delimiting) + error detection (FCS trailer) + media access control. LLC handles upper-layer/protocol identification."
  },
  {
    num: 78,
    topic: "IPv6",
    q: "An IPv6 enabled device sends a data packet with the destination address of FF02::2. What is the target of this packet?​",
    opts: ["all IPv6 enabled devices on the local link", "all IPv6 DHCP servers", "all IPv6 enabled devices across the network", "all IPv6 configured routers on the local link"],
    a: ["all IPv6 configured routers on the local link"],
    explain: "FF02::2 is the all-routers multicast address. FF02::1 targets all IPv6-enabled nodes on the local link."
  },
  {
    num: 79,
    topic: "IPv6",
    q: "What are the three parts of an IPv6 global unicast address? (Choose three.)",
    opts: ["subnet ID", "subnet mask", "broadcast address", "global routing prefix", "interface ID"],
    a: ["subnet ID", "global routing prefix", "interface ID"],
    explain: "IPv6 global unicast: global routing prefix (from ISP) + subnet ID (organization-defined) + interface ID (host portion, 64-bit)."
  },
  {
    num: 80,
    topic: "Wireless",
    q: "A network administrator is designing the layout of a new wireless network. Which three areas of concern should be accounted for when building a wireless network? (Choose three.)",
    opts: ["extensive cabling", "mobility options", "packet collision", "interference", "security", "coverage area"],
    a: ["interference", "security", "coverage area"],
    explain: "Wireless network design concerns: coverage area (signal reach), interference (from other devices), and security (unauthorized access)."
  },
  {
    num: 81,
    topic: "IOS CLI",
    q: "A new network administrator has been asked to enter a banner message on a Cisco device. What is the fastest way a network administrator could test whether the banner is properly configured?",
    opts: ["Enter CTRL-Z at the privileged mode prompt.", "Exit global configuration mode.", "Power cycle the device.", "Reboot the device.", "Exit privileged EXEC mode and press Enter."],
    a: ["Exit privileged EXEC mode and press Enter."],
    explain: "Type 'exit' at the Router# prompt and press Enter — the banner appears immediately without rebooting."
  },
  {
    num: 82,
    topic: "Wireless",
    q: "What method is used to manage contention-based access on a wireless network?",
    opts: ["token passing", "CSMA/CA", "priority ordering", "CSMA/CD"],
    a: ["CSMA/CA"],
    explain: "CSMA/CA (Collision Avoidance) is used in wireless networks. CSMA/CD (Collision Detection) is used in wired Ethernet."
  },
  {
    num: 83,
    topic: "Data Link Layer",
    q: "What is a function of the data link layer?",
    opts: ["provides the formatting of data", "provides end-to-end delivery of data between hosts", "provides delivery of data between two applications", "provides for the exchange of frames over a common local media"],
    a: ["provides for the exchange of frames over a common local media"],
    explain: "The data link layer (Layer 2) encapsulates Layer 3 packets into frames and manages delivery between NICs on the same local network."
  },
  {
    num: 84,
    topic: "Transport Layer",
    q: "What is the purpose of the TCP sliding window?",
    opts: ["to ensure that segments arrive in order at the destination", "to end communication when data transmission is complete", "to inform a source to retransmit data from a specific point forward", "to request that a source decrease the rate at which it transmits data"],
    a: ["to request that a source decrease the rate at which it transmits data"],
    explain: "The TCP sliding window allows the destination to tell the source to slow down by reducing the window size value in segments."
  },
  {
    num: 85,
    topic: "Security",
    q: "What characteristic describes spyware?",
    opts: ["a network device that filters access and traffic coming into a network", "software that is installed on a user device and collects information about the user", "an attack that slows or crashes a device or network service", "the use of stolen credentials to access private data"],
    a: ["software that is installed on a user device and collects information about the user"],
    explain: "Spyware secretly installs on end devices and collects user information (browsing habits, credentials) without the user's knowledge."
  },
  {
    num: 86,
    topic: "Switching",
    q: "Which switching method drops frames that fail the FCS check?",
    opts: ["store-and-forward switching", "borderless switching", "ingress port buffering", "cut-through switching"],
    a: ["store-and-forward switching"],
    explain: "Store-and-forward stores the entire frame and checks the FCS before forwarding. Corrupt frames are dropped. Cut-through does no error checking."
  },
  {
    num: 87,
    topic: "IPv6",
    q: "Which range of link-local addresses can be assigned to an IPv6-enabled interface?",
    opts: ["FEC0::/10", "FDEE::/7", "FE80::/10", "FF00::/8"],
    a: ["FE80::/10"],
    explain: "IPv6 link-local addresses are in the range FE80::/10 to FEBF::/10. FF00::/8 is multicast; FDEE::/7 is unique local; FEC0::/10 is deprecated."
  },
  {
    num: 88,
    topic: "Services",
    q: "What service is provided by FTP?",
    opts: ["A basic set of rules for exchanging multimedia files on the web.", "An application for real-time chatting among remote users.", "Allows for data transfers between a client and a file server.", "Uses encryption to secure the exchange of web content."],
    a: ["Allows for data transfers between a client and a file server."],
    explain: "FTP (File Transfer Protocol) enables uploading and downloading of files between a client and server using TCP ports 20 (data) and 21 (control)."
  },
  {
    num: 89,
    topic: "Configuration",
    q: "A user is attempting to access http://www.cisco.com/ without success. Which two configuration values must be set on the host to allow this access? (Choose two.)",
    opts: ["DNS server", "source port number", "HTTP server", "source MAC address", "default gateway"],
    a: ["DNS server", "default gateway"],
    explain: "A DNS server is needed to resolve the domain name; a default gateway is needed to route packets to the remote internet."
  },
  {
    num: 90,
    topic: "NAT",
    q: "Which two statements accurately describe an advantage or a disadvantage when deploying NAT for IPv4 in a network? (Choose two.)",
    opts: ["NAT adds authentication capability to IPv4.", "NAT introduces problems for some applications that require end-to-end connectivity.", "NAT will impact negatively on switch performance.", "NAT provides a solution to slow down the IPv4 address depletion.", "NAT improves packet handling.", "NAT causes routing tables to include more information."],
    a: ["NAT introduces problems for some applications that require end-to-end connectivity.", "NAT provides a solution to slow down the IPv4 address depletion."],
    explain: "NAT conserves public IPs (slowing IPv4 depletion) but breaks end-to-end connectivity for some applications by hiding real IP addresses."
  },
  {
    num: 91,
    topic: "IPv6",
    q: "What would be the interface ID of an IPv6 enabled interface with a MAC address of 1C-6F-65-C2-BD-F8 when the interface ID is generated by using the EUI-64 process?",
    opts: ["0C6F:65FF:FEC2:BDF8", "1E6F:65FF:FEC2:BDF8", "C16F:65FF:FEC2:BDF8", "106F:65FF:FEC2:BDF8"],
    a: ["1E6F:65FF:FEC2:BDF8"],
    explain: "EUI-64: (1) flip 7th bit of first byte (1C → 1E), (2) insert FFFE in the middle → 1E6F:65FF:FEC2:BDF8."
  },
  {
    num: 92,
    topic: "IPv6",
    q: "Refer to the exhibit. PC1 issues an ARP request because it needs to send a packet to PC2. In this scenario, what will happen next?",
    opts: ["SW1 will send an ARP reply with the SW1 Fa0/1 MAC address.​", "SW1 will send an ARP reply with the PC2 MAC address.​", "PC2 will send an ARP reply with the PC2 MAC address.", "RT1 will send an ARP reply with the RT1 Fa0/0 MAC address.​", "RT1 will send an ARP reply with the PC2 MAC address.​"],
    a: ["PC2 will send an ARP reply with the PC2 MAC address."],
    explain: "When a network device wants to communicate with another device on the same network, it sends a broadcast ARP request. In this case, the request will contain the IP address of PC2. The destination device (PC2) sends an ARP reply with its MAC address.",
    img: "https://itexamanswers.net/wp-content/uploads/2016/03/i209824v1n1_209824.jpg"
  },
  {
    num: 93,
    topic: "Services",
    q: "What service is provided by BOOTP?",
    opts: ["Uses encryption to secure web content exchange.", "Allows for data transfers between a client and a file server.", "Legacy application that enables a diskless workstation to discover its own IP address and find a BOOTP server.", "A basic set of rules for exchanging multimedia files on the web."],
    a: ["Legacy application that enables a diskless workstation to discover its own IP address and find a BOOTP server."],
    explain: "BOOTP (Bootstrap Protocol) is a predecessor to DHCP that allowed diskless workstations to auto-discover their IP address and boot files."
  },
  {
    num: 94,
    topic: "Security",
    q: "What characteristic describes adware?",
    opts: ["a network device that filters access and traffic coming into a network", "software that is installed on a user device and collects information about the user", "the use of stolen credentials to access private data", "an attack that slows or crashes a device or network service"],
    a: ["software that is installed on a user device and collects information about the user"],
    explain: "Adware (like spyware) is software installed on a user's device that secretly collects information, often to display targeted advertising."
  },
  {
    num: 95,
    topic: "Switching",
    q: "When a switch configuration includes a user-defined error threshold on a per-port basis, to which switching method will the switch revert when the error threshold is reached?",
    opts: ["cut-through", "store-and-forward", "fast-forward", "fragment-free"],
    a: ["store-and-forward"],
    explain: "When error rates exceed the threshold, the port automatically reverts to store-and-forward switching to check all frames for errors."
  },
  {
    num: 96,
    topic: "Application Layer",
    q: "Match a statement to the related network model. (Not all options are used.)",
    opts: [],
    a: ["peer-to-peer network → no dedicated server required; client and server roles are set on a per-request basis", "peer-to-peer application → requires a specific user interface; a background service is required"],
    explain: "P2P networks share resources without dedicated servers. A P2P application needs both a UI and a background service so the device can act as client and server.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/2022-05-12_101113.jpg"
  },
  {
    num: 97,
    topic: "Data Link Layer",
    q: "What are two primary responsibilities of the Ethernet MAC sublayer? (Choose two.)",
    opts: ["error detection", "frame delimiting", "accessing the media", "data encapsulation", "logical addressing"],
    a: ["accessing the media", "data encapsulation"],
    explain: "MAC sublayer is responsible for: data encapsulation (framing, addressing, error detection) and media access control (managing NIC for sending/receiving)."
  },
  {
    num: 98,
    topic: "IOS CLI",
    q: "Refer to the exhibit. What three facts can be determined from the viewable output of the show ip interface brief command? (Choose three.)",
    opts: ["Two physical interfaces have been configured.", "The switch can be remotely managed.", "One device is attached to a physical interface.", "Passwords have been configured on the switch.", "Two devices are attached to the switch.", "The default SVI has been configured."],
    a: ["The switch can be remotely managed.", "One device is attached to a physical interface.", "The default SVI has been configured."],
    explain: "VLAN1 (default SVI) with IP = remotely manageable. Fa0/0 showing up/up = one device connected. VLAN1 present = default SVI configured.",
    img: "https://itexamanswers.net/wp-content/uploads/2016/03/i274067v1n1_274067.jpg"
  },
  {
    num: 99,
    topic: "Automation",
    q: "Match each type of frame field to its function. (Not all options are used.)",
    opts: [],
    a: ["addressing → directs frame toward destination", "error detection → checks if frame was damaged", "type → identifies Layer 3 protocol (used by LLC)", "frame start → marks beginning of frame"],
    explain: "Standard Ethernet frame fields and their purposes.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/11.jpg"
  },
  {
    num: 100,
    topic: "IPv6",
    q: "What is the subnet ID associated with the IPv6 address 2001:DA48:FC5:A4:3D1B::1/64?",
    opts: ["2001:DA48::/64", "2001:DA48:FC5::A4:/64", "2001:DA48:FC5:A4::/64", "2001::/64"],
    a: ["2001:DA48:FC5:A4::/64"],
    explain: "With /64, the first 64 bits are the network+subnet. The subnet ID is 2001:DA48:FC5:A4::/64 (the first four 16-bit groups)."
  },
  {
    num: 101,
    topic: "Security",
    q: "Match the firewall function to the type of threat protection it provides to the network. (Not all options are used.)",
    opts: [],
    a: ["application filtering → prevents access by port number", "packet filtering → prevents access based on IP or MAC address", "stateful packet inspection → prevents unsolicited incoming sessions", "URL filtering → prevents access to specific websites"],
    explain: "Firewall techniques: packet filter (L3/L4), stateful (session aware), application filter (port/app), URL filter.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/18.jpg"
  },
  {
    num: 102,
    topic: "Subnetting",
    q: "Users are reporting longer delays in authentication and in accessing network resources during certain time periods of the week. What kind of information should network engineers check to find out if this situation is part of a normal network behavior?",
    opts: ["syslog records and messages", "the network performance baseline", "debug output and packet captures", "network configuration files"],
    a: ["the network performance baseline"],
    explain: "The network engineers should first establish that the reported performance of the network is in fact abnormal. This is done by referring to the documented network performance baseline.Once it has been verified that the network is not having a proper performance, then specific troubleshooting processes can be applied."
  },
  {
    num: 103,
    topic: "Security",
    q: "How does the service password-encryption command enhance password security on Cisco routers and switches?",
    opts: ["It requires encrypted passwords to be used when connecting remotely via Telnet.", "It encrypts passwords that are stored in router or switch configuration files.", "It requires that a user type encrypted passwords to gain console access.", "It encrypts passwords as they are sent across the network."],
    a: ["It encrypts passwords that are stored in router or switch configuration files."],
    explain: "'service password-encryption' encrypts plaintext passwords in the running/startup config files so they can't be read in plain text."
  },
  {
    num: 104,
    topic: "IPv4 vs IPv6",
    q: "Which two statements are correct in a comparison of IPv4 and IPv6 packet headers? (Choose two.)",
    opts: ["The Source Address field name from IPv4 is kept in IPv6.", "The Version field from IPv4 is not kept in IPv6.", "The Destination Address field is new in IPv6.", "The Header Checksum field from IPv4 is kept in IPv6.", "The Time-to-Live field from IPv4 has been replaced by Hop Limit in IPv6."],
    a: ["The Source Address field name from IPv4 is kept in IPv6.", "The Time-to-Live field from IPv4 has been replaced by Hop Limit in IPv6."],
    explain: "Both IPv4 and IPv6 have Source Address and Version fields. TTL (IPv4) → Hop Limit (IPv6). IPv6 removed the Header Checksum field."
  },
  {
    num: 105,
    topic: "Subnetting",
    q: "A network administrator wants to have the same network mask for all networks at a particular small site. The site has the following networks and number of devices: IP phones – 22 addresses PCs – 20 addresses needed Printers – 2 addresses needed Scanners – 2 addresses needed",
    opts: ["255.255.255.192", "255.255.255.252", "255.255.255.240", "255.255.255.248", "255.255.255.0", "255.255.255.224"],
    a: ["255.255.255.224"],
    explain: "If the same mask is to be used, then the network with the most hosts must be examined for the number of hosts, which in this case is 22 hosts. Thus, 5 host bits are needed. The /27 or 255.255.255.224 subnet mask would be appropriate to use for these networks."
  },
  {
    num: 106,
    topic: "Network Fundamentals",
    q: "What characteristic describes identity theft?",
    opts: ["the use of stolen credentials to access private data", "software on a router that filters traffic based on IP addresses or applications", "software that identifies fast-spreading threats", "a tunneling protocol that provides remote users with secure access into the network of an organization"],
    a: ["the use of stolen credentials to access private data"],
    explain: "Identity theft is a specific type of information theft where a threat actor steals login credentials or personal information to access private data . Once accessed, this information is often used to take over a person’s identity to make unauthorized purchases or obtain legal documents."
  },
  {
    num: 107,
    topic: "Subnetting",
    q: "A network administrator is adding a new LAN to a branch office. The new LAN must support 200 connected devices. What is the smallest network mask that the network administrator can use for the new network?",
    opts: ["255.255.255.240", "255.255.255.0", "255.255.255.248", "255.255.255.224"],
    a: ["255.255.255.0"],
    explain: "/24 (255.255.255.0) provides 2⁸−2 = 254 usable host addresses. 7 host bits only gives 126 — not enough for 200 devices."
  },
  {
    num: 108,
    topic: "IP Services",
    q: "What are three commonly followed standards for constructing and installing cabling? (Choose three.)",
    opts: ["cost per meter (foot)", "cable lengths", "connector color", "pinouts", "connector types", "tensile strength of plastic insulator"],
    a: ["cable lengths", "pinouts", "connector types"],
    explain: "According to the TIA/EIA-568 standard, which governs commercial cabling for LAN environments, several physical elements are defined to ensure consistency and performance. These include cable lengths , the specific connector types used (such as RJ-45), and the wire color-coded pin assignments, commonly referred to as pinouts ."
  },
  {
    num: 109,
    topic: "IP Services",
    q: "Refer to the exhibit. What is wrong with the displayed termination?",
    opts: ["The woven copper braid should not have been removed.", "The wrong type of connector is being used.", "The untwisted length of each wire is too long.", "The wires are too thick for the connector that is used."],
    a: ["The untwisted length of each wire is too long."],
    explain: "When a cable to an RJ-45 connector is terminated, it is important to ensure that the untwisted wires are not too long and that the flexible plastic sheath surrounding the wires is crimped down and not the bare wires. None of the colored wires should be visible from the bottom of the jack.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i274300v1n1_209630-300x221-1.png"
  },
  {
    num: 110,
    topic: "Addressing",
    q: "Match the characteristic to the category. (Not all options are used.)",
    opts: ["32 bits", "48 bits", "64 bits", "128 bits"],
    a: ["48 bits"],
    explain: "MAC addresses are 48 bits (Layer 2), divided into a 24-bit OUI and a 24-bit unique identifier. IP addresses are 32 (IPv4) or 128 (IPv6) bits.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/2020-03-26_165542.png"
  },
  {
    num: 111,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 143. What service is the client requesting?",
    opts: ["IMAP", "FTP", "SSH", "Telnet"],
    a: ["IMAP"],
    explain: "Port 143 is reserved for IMAP (Internet Message Access Protocol) — allows clients to access and manage email stored on a mail server."
  },
  {
    num: 112,
    topic: "Transport Layer",
    q: "What are two characteristics shared by TCP and UDP? (Choose two.)",
    opts: ["default window size", "connectionless communication", "port numbering", "3-way handshake", "ability to carry digitized voice", "use of checksum"],
    a: ["port numbering", "use of checksum"],
    explain: "Both TCP and UDP use port numbers to identify applications, and both use checksums for error detection."
  },
  {
    num: 113,
    topic: "Subnetting",
    q: "Refer to the exhibit. Which two network addresses can be assigned to the network containing 10 hosts? Your answers should waste the fewest addresses, not reuse addresses that are already assigned, and stay within the 10.18.10.0/24 range of addresses. (Choose two.)",
    opts: ["10.18.10.200/28", "10.18.10.208/28", "10.18.10.240/27", "10.18.10.200/27", "10.18.10.224/27", "10.18.10.224/28"],
    a: ["10.18.10.208/28", "10.18.10.224/28"],
    explain: "Addresses 10.18.10.0 through 10.18.10.63 are taken for the leftmost network. Addresses 192 through 199 are used by the center network. Because 4 host bits are needed to accommodate 10 hosts, a /28 mask is needed. 10.18.10.200/28 is not a valid network number. Two subnets that can be used are 10.18.10.208/28 and 10.18.10.224/28.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i274518v1n1_247518.png"
  },
  {
    num: 114,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 21. What service is the client requesting?",
    opts: ["FTP", "LDAP", "SLP", "SNMP"],
    a: ["FTP"],
    explain: "Port 21 is FTP's control connection port, used for sending commands and receiving replies during file transfers."
  },
  {
    num: 115,
    topic: "OSI Model",
    q: "What attribute of a NIC would place it at the data link layer of the OSI model?",
    opts: ["attached Ethernet cable", "IP address", "MAC address", "RJ-45 port", "TCP/IP protocol stack"],
    a: ["MAC address"],
    explain: "The MAC address is a Layer 2 (data link layer) physical address burned into the NIC, used for local network communication."
  },
  {
    num: 116,
    topic: "Subnetting",
    q: "A network administrator is adding a new LAN to a branch office. The new LAN must support 10 connected devices. What is the smallest network mask that the network administrator can use for the new network?",
    opts: ["255.255.255.192", "255.255.255.248", "255.255.255.224", "255.255.255.240"],
    a: ["255.255.255.240"],
    explain: "/28 (255.255.255.240) provides 2⁴−2 = 14 usable host addresses — the smallest that fits 10 devices."
  },
  {
    num: 117,
    topic: "Cabling",
    q: "What technique is used with UTP cable to help protect against signal interference from crosstalk?",
    opts: ["wrapping a foil shield around the wire pairs", "twisting the wires together into pairs", "terminating the cable with special grounded connectors", "encasing the cables within a flexible plastic sheath"],
    a: ["twisting the wires together into pairs"],
    explain: "Twisting wire pairs together causes their magnetic fields to cancel each other out, reducing crosstalk interference."
  },
  {
    num: 118,
    topic: "Subnetting",
    q: "Refer to the exhibit. The network administrator has assigned the LAN of LBMISS an address range of 192.168.10.0. This address range has been subnetted using a /29 prefix. In order to accommodate a new building, the technician has decided to use the fifth subnet for configuring the new network (subnet zero is the first subnet). By company policies, the router interface is always assigned the first usable host address and the workgroup server is given the last usable host address. Which configuration should be entered into the properties of the workgroup server to allow connectivity to the Internet?",
    opts: ["IP address: 192.168.10.65 subnet mask: 255.255.255.240, default gateway: 192.168.10.76", "IP address: 192.168.10.38 subnet mask: 255.255.255.240, default gateway: 192.168.10.33", "IP address: 192.168.10.38 subnet mask: 255.255.255.248, default gateway: 192.168.10.33", "IP address: 192.168.10.41 subnet mask: 255.255.255.248, default gateway: 192.168.10.46", "IP address: 192.168.10.254 subnet mask: 255.255.255.0, default gateway: 192.168.10.1"],
    a: ["IP address: 192.168.10.38 subnet mask: 255.255.255.248, default gateway: 192.168.10.33"],
    explain: "Using a /29 prefix to subnet 192.168.10.0 results in subnets that increment by 8: 192.168.10.0 (1) 192.168.10.8 (2) 192.168.10.16 (3) 192.168.10.24 (4) 192.168.10.32 (5)",
    img: "https://itexamanswers.net/wp-content/uploads/2017/05/i209840v1n3_209840.jpg"
  },
  {
    num: 119,
    topic: "Subnetting",
    q: "Refer to the exhibit. The switches are in their default configuration. Host A needs to communicate with host D, but host A does not have the MAC address for its default gateway. Which network hosts will receive the ARP request sent by host A?",
    opts: ["only host D", "only router R1", "only hosts A, B, and C", "only hosts A, B, C, and D", "only hosts B and C", "only hosts B, C, and router R1"],
    a: ["only hosts B, C, and router R1"],
    explain: "Since host A does not have the MAC address of the default gateway in its ARP table, host A sends an ARP broadcast. The ARP broadcast would be sent to every device on the local network. Hosts B, C, and router R1 would receive the broadcast. Router R1 would not forward the message.",
    img: "https://itexamanswers.net/wp-content/uploads/2016/03/i275353v1n1_275353.jpg"
  },
  {
    num: 120,
    topic: "Security",
    q: "Match a statement to the related network model. (Not all options are used.)",
    opts: [],
    a: ["peer-to-peer network → no dedicated server required; client/server roles per request", "peer-to-peer application → requires a specific user interface; background service required"],
    explain: "Same as Q96 — P2P network vs P2P application.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/2020-03-26_160912.png"
  },
  {
    num: 121,
    topic: "Subnetting",
    q: "Refer to the exhibit. A network engineer has been given the network address of 192.168.99.0 and a subnet mask of 255.255.255.192 to subnet across the four networks shown. How many total host addresses are unused across all four subnets?",
    opts: ["88", "200", "72", "224", "158"],
    a: ["200"],
    explain: "Usable hosts per subnet : A subnet mask of 255.255.255.192 (/26) leaves 6 host bits (32−26=6). Using the formula 2 n −2, each subnet provides 62 usable host addresses (2 6 −2=62). Used addresses : Across the four subnets, the total used addresses are 48 (30 for Network A, 14 for Network D, and 2 each for the router-to-router links in Networks B and C). Total unused : With four subnets, there are 248 total usable addresses (62×4=248). Subtracting the 48 used addresses results in 200",
    img: "https://itexamanswers.net/wp-content/uploads/2020/06/i304957v1n1_209418-1591171569.7915.png"
  },
  {
    num: 122,
    topic: "Network Access",
    q: "Which connector is used with twisted-pair cabling in an Ethernet LAN?",
    opts: ["RJ-45", "RJ-11", "LC", "SC", "BNC"],
    a: ["RJ-45"],
    explain: "RJ-45 is the standard connector for twisted-pair Ethernet (Cat5/5e/6). RJ-11 is used for telephone lines; LC and SC are fiber optic connectors; BNC is used with coaxial cable."
  },
  {
    num: 123,
    topic: "Ports",
    q: "A client packet is received by a server. The packet has a destination port number of 22. What service is the client requesting?",
    opts: ["SSH", "SMB/CIFS", "HTTPS", "SLP"],
    a: ["SSH"],
    explain: "Port 22 is reserved for SSH (Secure Shell) — provides encrypted remote access to network devices and servers."
  },
  {
    num: 124,
    topic: "Security",
    q: "What characteristic describes an IPS?",
    opts: ["a tunneling protocol that provides remote users with secure access", "a network device that filters access and traffic coming into a network", "software that identifies fast-spreading threats", "software on a router that filters traffic based on IP addresses or applications"],
    a: ["software that identifies fast-spreading threats"],
    explain: "An IPS (Intrusion Prevention System) monitors traffic for malware and attack signatures and can stop threats immediately."
  },
  {
    num: 125,
    topic: "Services",
    q: "What service is provided by DHCP?",
    opts: ["An application that allows real-time chatting among remote users.", "Allows remote access to network devices and servers.", "Dynamically assigns IP addresses to end and intermediary devices.", "Uses encryption to provide secure remote access."],
    a: ["Dynamically assigns IP addresses to end and intermediary devices."],
    explain: "DHCP automates assignment of IP addresses, subnet masks, default gateways, and DNS server addresses to network devices."
  },
  {
    num: 126,
    topic: "IP Connectivity",
    q: "Match the header field with the appropriate layer of the OSI model. (Not all options are used.)",
    opts: [],
    a: ["Layer 2 → 802.2 header, FCS, destination MAC address", "Layer 3 → source IP address, TTL", "Layer 4 → destination port number, Acknowledgement number"],
    explain: "L2 = MAC/LLC fields (802.2, FCS, MAC). L3 = IP fields (IP addr, TTL). L4 = TCP/UDP fields (port, ACK).",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/2020-03-26_173114.png"
  },
  {
    num: 127,
    topic: "Subnetting",
    q: "Refer to the exhibit. The switches have a default configuration. Host A needs to communicate with host D, but host A does not have the MAC address for the default gateway. Which network devices will receive the ARP request sent by host A?",
    opts: ["only host D", "only hosts A, B, C, and D", "only hosts B and C", "only hosts B, C, and router R1", "only hosts A, B, and C", "only router R1"],
    a: ["only hosts B, C, and router R1"],
    explain: "Because host A does not have the MAC address of the default gateway in the ARP table, host A sends an ARP broadcast. The ARP broadcast would be sent to every device on the local network. Hosts B, C, and router R1 would receive the broadcast. Router R1 would not forward the message.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/12/i360201v3n1_275353-1608482322.6606.png"
  },
  {
    num: 128,
    topic: "Wireless",
    q: "Which wireless technology has low-power and low-data rate requirements making it popular in IoT environments?",
    opts: ["Bluetooth", "Zigbee", "WiMAX", "Wi-Fi"],
    a: ["Zigbee"],
    explain: "Zigbee is designed for low data rate, low power IoT applications like wireless light switches and medical devices with long battery life."
  },
  {
    num: 129,
    topic: "IPv6",
    q: "What two ICMPv6 message types must be permitted through IPv6 access control lists to allow resolution of Layer 3 addresses to Layer 2 MAC addresses? (Choose two.)",
    opts: ["neighbor solicitations", "echo requests", "neighbor advertisements", "echo replies", "router solicitations", "router advertisements"],
    a: ["neighbor solicitations", "neighbor advertisements"],
    explain: "IPv6 uses NDP instead of ARP. Neighbor Solicitation (request) and Neighbor Advertisement (reply) resolve IPv6 addresses to MAC addresses."
  },
  {
    num: 130,
    topic: "IPv6",
    q: "A client is using SLAAC to obtain an IPv6 address for its interface. After an address has been generated and applied to the interface, what must the client do before it can begin to use this IPv6 address?",
    opts: ["Send a DHCPv6 INFORMATION-REQUEST message to request the DNS server address.", "Send a DHCPv6 REQUEST message to the DHCPv6 server to request permission.", "Send an ICMPv6 Router Solicitation message to determine the default gateway.", "Send an ICMPv6 Neighbor Solicitation message to ensure the address is not already in use."],
    a: ["Send an ICMPv6 Neighbor Solicitation message to ensure the address is not already in use."],
    explain: "Duplicate Address Detection (DAD): the client sends a Neighbor Solicitation to check if the address is already in use before assigning it."
  },
  {
    num: 131,
    topic: "Troubleshooting",
    q: "Two pings were issued from a host on a local network. The first ping was issued to the IP address of the default gateway of the host and it failed. The second ping was issued to the IP address of a host outside the local network and it was successful. What is a possible cause for the failed ping?",
    opts: ["The default gateway is not operational.", "The default gateway device is configured with the wrong IP address.", "Security rules are applied to the default gateway device, preventing it from processing ping requests.", "The TCP/IP stack on the default gateway is not working properly."],
    a: ["Security rules are applied to the default gateway device, preventing it from processing ping requests."],
    explain: "If the ping from one host to another host on a remote network is successful, this indicates that the default gateway is operational. In this scenario, if a ping from one host to the default gateway failed, it is possible that some security features are applied to the router interface, preventing it from responding to ping requests."
  },
  {
    num: 132,
    topic: "IPv6",
    q: "An organization is assigned an IPv6 address block of 2001:db8:0:ca00::/56. How many subnets can be created without using bits in the interface ID space?",
    opts: ["256", "512", "1024", "4096"],
    a: ["256"],
    explain: "Standard subnets use /64. With a /56 prefix, 8 bits remain for subnetting (64−56=8). 2⁸ = 256 subnets."
  },
  {
    num: 133,
    topic: "Subnetting",
    q: "What subnet mask is needed if an IPv4 network has 40 devices that need IP addresses and address space is not to be wasted?",
    opts: ["255.255.255.0", "255.255.255.240", "255.255.255.128", "255.255.255.192", "255.255.255.224"],
    a: ["255.255.255.192"],
    explain: "/26 (255.255.255.192) provides 2⁶−2 = 62 usable host addresses — enough for 40 devices with minimal waste."
  },
  {
    num: 134,
    topic: "IPv6",
    q: "Refer to the exhibit. If host A sends an IP packet to host B, what will the destination address be in the frame when it leaves host A?",
    opts: ["DD:DD:DD:DD:DD:DD", "172.168.10.99", "CC:CC:CC:CC:CC:CC", "172.168.10.65", "BB:BB:BB:BB:BB:BB", "AA:AA:AA:AA:AA:AA"],
    a: ["BB:BB:BB:BB:BB:BB"],
    explain: "When a host sends information to a distant network, the Layer 2 frame header will contain a source and destination MAC address. The source address will be the originating host device. The destination address will be the router interface that connects to the same network. In the case of host A sending information to host B, the source address is AA:AA:AA:AA:AA:AA and the destination address is the MAC address assigned to the R2 Ethernet interface, BB:BB:BB:BB:BB:BB.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i274590v1n1_274590.png"
  },
  {
    num: 135,
    topic: "Networking Trends",
    q: "What is a benefit of using cloud computing in networking?",
    opts: ["Technology is integrated into appliances allowing them to interconnect.", "Network capabilities are extended without requiring investment in new infrastructure, personnel, or software.", "End users have freedom to use personal tools to access business network resources.", "Home networking uses existing electrical wiring to connect devices."],
    a: ["Network capabilities are extended without requiring investment in new infrastructure, personnel, or software."],
    explain: "Cloud computing extends IT capabilities on-demand without requiring new hardware investment, training, or software licensing."
  },
  {
    num: 136,
    topic: "Addressing",
    q: "Which two statements are correct about MAC and IP addresses during data transmission if NAT is not involved? (Choose two.)",
    opts: ["Destination IP addresses in a packet remain constant along the entire path.", "Destination MAC addresses will never change in a frame crossing seven routers.", "Every time a frame has a new destination MAC, a new destination IP is needed.", "Destination and source MAC addresses change every time a frame goes from one LAN to another.", "A packet crossing four routers changes the destination IP address four times."],
    a: ["Destination IP addresses in a packet remain constant along the entire path.", "Destination and source MAC addresses change every time a frame goes from one LAN to another."],
    explain: "IP addresses (Layer 3) stay constant end-to-end. MAC addresses (Layer 2) are local and change at every router hop."
  },
  {
    num: 137,
    topic: "Data Link Layer",
    q: "What is one main characteristic of the data link layer?",
    opts: ["It generates the electrical or optical signals representing 1s and 0s on the media.", "It converts a stream of data bits into a predefined code.", "It shields the upper layer protocol from being aware of the physical medium.", "It accepts Layer 3 packets and decides the path to forward the packet to a remote network."],
    a: ["It shields the upper layer protocol from being aware of the physical medium."],
    explain: "The data link layer abstracts the physical medium from upper layers, so higher-layer protocols don't need to know what type of media is being used."
  },
  {
    num: 138,
    topic: "Ethernet",
    q: "What are three characteristics of the CSMA/CD process? (Choose three.)",
    opts: ["The device with the electronic token is the only one that can transmit after a collision.", "A device listens and waits until the media is not busy before transmitting.", "After detecting a collision, hosts can attempt retransmission after a random time delay.", "All devices on a segment see data that passes on the network medium.", "A jam signal indicates that the collision has cleared and the media is not busy.", "Devices can be configured with a higher transmission priority."],
    a: ["A device listens and waits until the media is not busy before transmitting.", "After detecting a collision, hosts can attempt retransmission after a random time delay.", "All devices on a segment see data that passes on the network medium."],
    explain: "CSMA/CD: listen before transmit, detect collision → send jam signal → wait random time → retry. All devices on the segment see the transmitted data."
  },
  {
    num: 139,
    topic: "IOS CLI",
    q: "Which information does the show startup-config command display?",
    opts: ["the IOS image copied into RAM", "the bootstrap program in the ROM", "the contents of the current running configuration file in the RAM", "the contents of the saved configuration file in the NVRAM"],
    a: ["the contents of the saved configuration file in the NVRAM"],
    explain: "'show startup-config' displays the saved configuration in NVRAM. 'show running-config' shows the current active config in RAM."
  },
  {
    num: 140,
    topic: "Windows CLI",
    q: "Which two commands can be used on a Windows host to display the routing table? (Choose two.)",
    opts: ["netstat -s", "route print", "show ip route", "netstat -r", "tracert"],
    a: ["route print", "netstat -r"],
    explain: "'route print' and 'netstat -r' both display the routing table on a Windows host. 'show ip route' is a Cisco IOS command."
  },
  {
    num: 141,
    topic: "Network Layer",
    q: "What are two functions that are provided by the network layer? (Choose two.)",
    opts: ["directing data packets to destination hosts on other networks", "placing data on the network medium", "carrying data between processes on source and destination hosts", "providing dedicated end-to-end connections", "providing end devices with a unique network identifier"],
    a: ["directing data packets to destination hosts on other networks", "providing end devices with a unique network identifier"],
    explain: "The network layer (Layer 3) routes packets to remote networks and provides unique IP addresses to identify source and destination devices."
  },
  {
    num: 142,
    topic: "Subnetting",
    q: "Which two statements describe features of an IPv4 routing table on a router? (Choose two.)​",
    opts: ["Directly connected interfaces will have two route source codes in the routing table: C and S .", "If there are two or more possible routes to the same destination, the route associated with the higher metric value is included in the routing table.", "The netstat -r command can be used to display the routing table of a router.​", "The routing table lists the MAC addresses of each active interface.", "It stores information about routes derived from the active router interfaces.", "If a default static route is configured in the router, an entry will be included in the routing table with source code S .", "The netstat -r command can be used to display the routing table of a router.​", "The routing table lists the MAC addresses of each active interface.", "Directly connected interfaces will have two route source codes in the routing table: C and S ."],
    a: ["It stores information about routes derived from the active router interfaces.", "If a default static route is configured in the router, an entry will be included in the routing table with source code S ."],
    explain: "The show ip route command is used to display the routing table of the router. In IPv4, directly connected interfaces will have one source code: C . The routing table stores information about directly connected routes and remote routes. An entry in the routing table with a source code of S is included if a default static route is configured on the router."
  },
  {
    num: 143,
    topic: "Security",
    q: "What characteristic describes a VPN?",
    opts: ["software on a router that filters traffic based on IP addresses or applications", "software that identifies fast-spreading threats", "a tunneling protocol that provides remote users with secure access into the network of an organization", "a network device that filters access and traffic coming into a network"],
    a: ["a tunneling protocol that provides remote users with secure access into the network of an organization"],
    explain: "A VPN creates encrypted tunnels to provide remote workers with secure access to an organization's internal network resources."
  },
  {
    num: 144,
    topic: "Switching",
    q: "Why would a Layer 2 switch need an IP address?",
    opts: ["to enable the switch to send broadcast frames to attached PCs", "to enable the switch to function as a default gateway", "to enable the switch to be managed remotely", "to enable the switch to receive frames from attached PCs"],
    a: ["to enable the switch to be managed remotely"],
    explain: "A switch needs an IP address (on its SVI/VLAN interface) only for remote management. Switches don't need IPs to forward frames."
  },
  {
    num: 145,
    topic: "IP Connectivity",
    q: "Match each description to its corresponding term. (Not all options are used.)",
    opts: [],
    a: ["message encapsulation → placing one message format inside another", "message sizing → breaking a long message into pieces before sending", "message encoding → converting information into a transmittable format"],
    explain: "Three fundamentals of message formatting: encoding, encapsulation, sizing."
  },
  {
    num: 146,
    topic: "IPv6",
    q: "A user sends an HTTP request to a web server on a remote network. During encapsulation for this request, what information is added to the address field of a frame to indicate the destination?",
    opts: ["the network domain of the destination host", "the IP address of the default gateway", "the MAC address of the destination host", "the MAC address of the default gateway"],
    a: ["the MAC address of the default gateway"],
    explain: "A frame is encapsulated with source and destination MAC addresses. The source device will not know the MAC address of the remote host. An ARP request will be sent by the source and will be responded to by the router. The router will respond with the MAC address of its interface, the one which is connected to the same network as the source."
  },
  {
    num: 147,
    topic: "Protocols",
    q: "What is an advantage to using a protocol that is defined by an open standard?",
    opts: ["A company can monopolize the market.", "The protocol can only be run on equipment from a specific vendor.", "An open standard protocol is not controlled by standards organizations.", "It encourages competition and promotes choices."],
    a: ["It encourages competition and promotes choices."],
    explain: "Open standards allow any vendor to implement the protocol, fostering competition, interoperability, and consumer choice."
  },
  {
    num: 148,
    topic: "Transport Layer",
    q: "Data is being sent from a source PC to a destination server. Which three statements correctly describe the function of TCP or UDP in this situation? (Choose three.)",
    opts: ["The source port field identifies the running application that will handle returning data to the PC.", "The TCP process randomly selects the destination port when establishing a session.", "UDP segments are encapsulated within IP packets for transport.", "The UDP destination port identifies the application on the server that will handle the data.", "TCP is preferred when a function requires lower network overhead.", "The TCP source port number identifies the sending host on the network."],
    a: ["The source port field identifies the running application that will handle returning data to the PC.", "UDP segments are encapsulated within IP packets for transport.", "The UDP destination port identifies the application on the server that will handle the data."],
    explain: "Source port tracks return traffic to the right app. Layer 4 segments are encapsulated in IP. Destination port identifies the server application."
  },
  {
    num: 149,
    topic: "Transport Layer",
    q: "Match each description with the corresponding TCP mechanism. (Not all options are used.)",
    opts: ["used to detect out-of-order delivery", "prevents sender from overwhelming the receiver"],
    a: [
      "number of bytes a destination device can accept and process at one time → window size",
      "used to identify missing segments of data → sequence numbers",
      "method of managing segments of data loss → retransmission",
      "received by a sender before transmitting more segments in a session → acknowledgment"
    ],
    explain: "TCP window size controls flow (bytes accepted at once). Sequence numbers identify and order segments. Retransmission handles lost segments. Acknowledgments confirm receipt and signal the sender to continue.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/20.jpg"
  },
  {
    num: 150,
    topic: "Subnetting",
    q: "Refer to the exhibit. A company uses the address block of 128.107.0.0/16 for its network. What subnet mask would provide the maximum number of equal size subnets while providing enough host addresses for each subnet in the exhibit?",
    opts: ["255.255.255.192", "255.255.255.0", "255.255.255.128", "255.255.255.240", "255.255.255.224"],
    a: ["255.255.255.128"],
    explain: "The largest subnet in the topology has 100 hosts in it so the subnet mask must have at least 7 host bits in it (27-2=126). 255.255.255.0 has 8 hosts bits, but this does not meet the requirement of providing the maximum number of subnets.",
    img: "https://itexamanswers.net/wp-content/uploads/2020/01/i207609v1n1_207609.png"
  },
  {
    num: 151,
    topic: "Subnetting",
    q: "A network administrator wants to have the same subnet mask for three subnetworks at a small site. The site has the following networks and numbers of devices: Subnetwork A: IP phones – 10 addresses; Subnetwork B: PCs – 8 addresses; Subnetwork C: Printers – 2 addresses. What single subnet mask would be appropriate to use for the three subnetworks?",
    opts: ["255.255.255.0", "255.255.255.240", "255.255.255.248", "255.255.255.252"],
    a: ["255.255.255.240"],
    explain: "If the same mask is to be used, then the network with the most hosts must be examined for number of hosts. Because this is 10 hosts, 4 host bits are needed. The /28 or 255.255.255.240 subnet mask would be appropriate to use for these networks. ​"
  },
  {
    num: 152,
    topic: "Network Fundamentals",
    q: "Match each item to the type of topology diagram on which it is typically identified. (Not all options are used.)",
    opts: [],
    a: ["physical topology → location of a desktop PC in a classroom; path of cables that connect rooms to wiring closets", "logical topology → IP address of a server"],
    explain: "Physical topology = where devices/cabling physically are. Logical topology = addressing scheme and how data flows."
  },
  {
    num: 153,
    topic: "IOS CLI",
    q: "What two pieces of information are displayed in the output of the show ip interface brief command? (Choose two.)",
    opts: ["IP addresses", "interface descriptions", "MAC addresses", "next-hop addresses", "Layer 1 statuses", "speed and duplex settings"],
    a: ["IP addresses", "Layer 1 statuses"],
    explain: "'show ip interface brief' shows IP addresses assigned to interfaces and their Layer 1 (physical) and Layer 2 (protocol) operational status."
  },
  {
    num: 154,
    topic: "Troubleshooting",
    q: "A user is complaining that an external web page is taking longer than normal to load.The web page does eventually load on the user machine. Which tool should the technician use with administrator privileges in order to locate where the issue is in the network?",
    opts: ["ping", "nslookup", "tracert", "ipconfig /displaydns"],
    a: ["tracert"],
    explain: "Tracert (traceroute) maps the path to the destination and measures transit delays at each hop, helping identify where slowdowns occur."
  },
  {
    num: 155,
    topic: "IPv4",
    q: "Which value, that is contained in an IPv4 header field, is decremented by each router that receives a packet?",
    opts: ["Header Length", "Differentiated Services", "Time-to-Live", "Fragment Offset"],
    a: ["Time-to-Live"],
    explain: "Each router decrements the TTL field by 1. When it reaches 0, the router discards the packet and sends an ICMP Time Exceeded message to the sender."
  },
  {
    num: 156,
    topic: "Cabling",
    q: "A network technician is researching the use of fiber optic cabling in a new technology center. Which two issues should be considered before implementing fiber optic media? (Choose two.)",
    opts: ["Fiber optic cabling requires different termination and splicing expertise than copper.", "Fiber optic cabling requires specific grounding to be immune to EMI.", "Fiber optic cabling is susceptible to loss of signal due to RFI.", "Fiber optic cable is able to withstand rough handling.", "Fiber optic provides higher data capacity but is more expensive than copper cabling."],
    a: ["Fiber optic cabling requires different termination and splicing expertise than copper.", "Fiber optic provides higher data capacity but is more expensive than copper cabling."],
    explain: "Fiber optic requires specialized termination/splicing skills, and has higher cost than copper — though it's immune to EMI/RFI."
  },
  {
    num: 157,
    topic: "Subnetting",
    q: "Match each description with an appropriate IP address. (Not all options are used.)",
    opts: [],
    a: ["experimental → 240.2.6.255", "link-local → 169.254.1.5", "public → 198.133.219.2", "loopback → 127.0.0.1"],
    explain: "240.0.0.0/4 reserved (experimental). 169.254.0.0/16 link-local. 127.0.0.0/8 loopback. The rest are public unless in private ranges.",
    img: "https://itexamanswers.net/wp-content/uploads/2022/09/2022-09-22_165322.jpg"
  },
  {
    num: 158,
    topic: "Troubleshooting",
    q: "A user is executing a tracert to a remote device. At what point would a router, which is in the path to the destination device, stop forwarding the packet?",
    opts: ["when the router receives an ICMP Time Exceeded message", "when the RTT value reaches zero", "when the host responds with an ICMP Echo Reply message", "when the value in the TTL field reaches zero", "when the values of both Echo Request and Echo Reply messages reach zero"],
    a: ["when the value in the TTL field reaches zero"],
    explain: "When a router decrements the TTL to 0, it drops the packet and sends an ICMP Time Exceeded message back to the source."
  },
  {
    num: 159,
    topic: "Security",
    q: "Users report that the network access is slow. After questioning the employees, the network administrator learned that one employee downloaded a third-party scanning program for the printer. What type of malware might be introduced that causes slow performance of the network?",
    opts: ["virus", "worm", "phishing", "spam"],
    a: ["worm"],
    explain: "A worm is self-propagating malware that spreads across networks (often through downloaded programs) and consumes bandwidth, causing slow network performance."
  }
];

const topics = [...new Set(questions.map(q => q.topic))].sort();
let flippedCards = new Set();
let activeFilter = "All";
const LS_KEY = 'networking-flashcards-learned-v1';
let learnedCards = new Set(
  (() => { try { return JSON.parse(localStorage.getItem(LS_KEY) || '[]'); } catch { return []; } })()
);

function saveLearned() {
  try { localStorage.setItem(LS_KEY, JSON.stringify([...learnedCards])); } catch {}
}

function init() {
  const filterContainer = document.getElementById('filters');
  const allBtn = document.createElement('button');
  allBtn.className = 'filter-btn active';
  allBtn.textContent = 'All';
  allBtn.onclick = () => setFilter('All', allBtn);
  filterContainer.appendChild(allBtn);

  topics.forEach(topic => {
    const btn = document.createElement('button');
    btn.className = 'filter-btn';
    btn.textContent = topic;
    btn.onclick = () => setFilter(topic, btn);
    filterContainer.appendChild(btn);
  });

  document.getElementById('total-count').textContent = questions.length;
  setupDropZone(document.getElementById('learned-zone'), 'learned');
  setupDropZone(document.getElementById('grid'), 'todo');
  renderAll();
}

function setFilter(topic, btn) {
  activeFilter = topic;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  filterCards();
}

function filterCards() { renderAll(); }

function getFiltered() {
  const search = document.getElementById('search').value.toLowerCase();
  return questions.filter(q => {
    const matchesTopic = activeFilter === 'All' || q.topic === activeFilter;
    const matchesSearch = !search ||
      q.q.toLowerCase().includes(search) ||
      q.a.some(a => a.toLowerCase().includes(search)) ||
      q.topic.toLowerCase().includes(search);
    return matchesTopic && matchesSearch;
  });
}

function renderAll() {
  const filtered = getFiltered();
  const todo = filtered.filter(q => !learnedCards.has(q.num));
  const learned = filtered.filter(q => learnedCards.has(q.num));

  renderInto(document.getElementById('grid'), todo, '// no matching questions');
  renderInto(document.getElementById('learned-zone'), learned, '// drop cards here once you\'ve learned them', true);

  document.getElementById('shown-count').textContent = filtered.length;
  document.getElementById('flipped-count').textContent = flippedCards.size;
  document.getElementById('learned-count').textContent = learnedCards.size;
  document.getElementById('learned-count-inline').textContent = learnedCards.size;
}

function renderInto(container, data, emptyMsg, isLearnedZone = false) {
  container.innerHTML = '';
  if (data.length === 0) {
    const cls = isLearnedZone ? 'empty-hint' : 'no-results';
    container.innerHTML = `<div class="${cls}">${emptyMsg}</div>`;
    return;
  }
  data.forEach(q => container.appendChild(buildCard(q)));
}

function buildCard(q) {
  const card = document.createElement('div');
  card.className = 'flip-card' + (flippedCards.has(q.num) ? ' flipped' : '');
  card.dataset.num = q.num;
  card.draggable = true;

  const correctAnswers = q.a.map(a => `<span class="correct-answer">✓ ${a}</span>`).join('<br>');

  card.innerHTML = `
    <div class="drag-handle" title="Drag to move">⋮⋮</div>
    <div class="flip-card-inner">
      <div class="flip-card-front">
        <div class="q-num">Q${q.num} · ${q.topic}</div>
        <div class="q-text">${q.q}</div>${q.img ? `<img src="${q.img}" alt="Exhibit" style="max-width:100%;margin-top:0.6rem;border:1px solid var(--border);border-radius:4px;" />` : ""}
        <div class="flip-hint">click to reveal →</div>
      </div>
      <div class="flip-card-back">
        <div class="a-label">ANSWER${q.a.length > 1 ? 'S' : ''}</div>
        <div class="a-text">
          ${correctAnswers}
          <br><br>
          <span style="color:#6b7280;font-size:0.78rem;">${q.explain}</span>
        </div>
      </div>
    </div>
  `;

  card.addEventListener('click', (e) => {
    if (e.target.classList.contains('drag-handle')) return;
    card.classList.toggle('flipped');
    if (card.classList.contains('flipped')) flippedCards.add(q.num);
    else flippedCards.delete(q.num);
    document.getElementById('flipped-count').textContent = flippedCards.size;
  });

  card.addEventListener('dragstart', (e) => {
    card.classList.add('dragging');
    e.dataTransfer.effectAllowed = 'move';
    e.dataTransfer.setData('text/plain', String(q.num));
  });
  card.addEventListener('dragend', () => card.classList.remove('dragging'));

  return card;
}

function setupDropZone(el, kind) {
  el.addEventListener('dragover', (e) => {
    e.preventDefault();
    e.dataTransfer.dropEffect = 'move';
    el.classList.add('drag-over');
  });
  el.addEventListener('dragleave', (e) => {
    if (e.target === el) el.classList.remove('drag-over');
  });
  el.addEventListener('drop', (e) => {
    e.preventDefault();
    el.classList.remove('drag-over');
    const num = Number(e.dataTransfer.getData('text/plain'));
    if (!num) return;
    if (kind === 'learned') learnedCards.add(num);
    else learnedCards.delete(num);
    saveLearned();
    renderAll();
  });
}

/* ============ STUDY MODE (spaced repetition) ============ */
const SR_KEY = 'networking-flashcards-sr-v1';
const HIST_KEY = 'networking-flashcards-history-v1';
// intervals in minutes: 1 = again (1 min), 2 = good (15 min), 3 = easy (4 hours, then doubles)
const INTERVALS = { 1: 1, 2: 15, 3: 240 };

let srState = (() => { try { return JSON.parse(localStorage.getItem(SR_KEY) || '{}'); } catch { return {}; } })();
let history = (() => { try { return JSON.parse(localStorage.getItem(HIST_KEY) || '[]'); } catch { return []; } })();
let currentStudyQ = null;
let studyRevealed = false;
let studySelected = new Set();
let autoCheck = (() => { try { return localStorage.getItem('networking-flashcards-autocheck-v1') === '1'; } catch { return false; } })();

function toggleAutoCheck() {
  autoCheck = !autoCheck;
  try { localStorage.setItem('networking-flashcards-autocheck-v1', autoCheck ? '1' : '0'); } catch {}
  updateAutoCheckUI();
}
function updateAutoCheckUI() {
  const el = document.getElementById('auto-check-toggle');
  if (el) el.classList.toggle('on', autoCheck);
}

function toggleStudyOpt(opt) {
  if (studyRevealed) return;
  const q = currentStudyQ; if (!q) return;
  const multi = q.a.length > 1;
  if (multi) {
    if (studySelected.has(opt)) studySelected.delete(opt);
    else studySelected.add(opt);
  } else {
    studySelected.clear();
    studySelected.add(opt);
  }
  // Auto-check when selection count matches expected answer count
  if (autoCheck && studySelected.size === q.a.length) {
    studyRevealed = true;
  }
  renderStudy();
}

function checkStudy() {
  if (!studySelected.size) return;
  studyRevealed = true;
  renderStudy();
}

function saveSR() { try { localStorage.setItem(SR_KEY, JSON.stringify(srState)); } catch {} }
function saveHist() { try { localStorage.setItem(HIST_KEY, JSON.stringify(history)); } catch {} }

function getDueQuestions() {
  const now = Date.now();
  return questions.filter(q => {
    const s = srState[q.num];
    return !s || s.dueAt <= now;
  });
}

function pickNextStudyQ() {
  const now = Date.now();
  const due = getDueQuestions();
  if (due.length > 0) {
    // weight: brand new + most overdue first, with randomness
    const ranked = due.map(q => {
      const s = srState[q.num];
      const overdue = s ? (now - s.dueAt) : 1e12; // new = highest priority
      return { q, score: overdue + Math.random() * 60000 };
    }).sort((a, b) => b.score - a.score);
    return ranked[0].q;
  }
  // Nothing due — show soonest
  const sorted = [...questions].sort((a, b) => {
    const da = srState[a.num] ? srState[a.num].dueAt : 0;
    const db = srState[b.num] ? srState[b.num].dueAt : 0;
    return da - db;
  });
  return sorted[0];
}

function rateQuestion(rating) {
  if (!currentStudyQ) return;
  // Reset matching state for next question
  matchingPairs = []; matchingLeft = []; matchingRight = [];
  matchingSelectedLeft = null; matchingSelectedRight = null;
  matchingDone = {}; matchingRevealed = false;
  const num = currentStudyQ.num;
  const prev = srState[num];
  let intervalMin;
  if (rating === 1) intervalMin = INTERVALS[1];
  else if (rating === 2) intervalMin = INTERVALS[2];
  else {
    // rating 3 — easy, doubles each consecutive easy review
    intervalMin = prev && prev.lastRating === 3 ? Math.min(prev.intervalMin * 2, 60 * 24 * 30) : INTERVALS[3];
  }
  srState[num] = {
    dueAt: Date.now() + intervalMin * 60 * 1000,
    intervalMin,
    lastRating: rating,
    reps: (prev?.reps || 0) + 1
  };
  saveSR();
  history.push({ t: Date.now(), num, rating });
  saveHist();
  studyRevealed = false;
  studySelected = new Set();
  currentStudyQ = pickNextStudyQ();
  renderStudy();
}

// Auto-rate based on selection correctness — used when revealing answer.
function autoRatingFromSelection() {
  const q = currentStudyQ; if (!q) return 1;
  const correct = new Set(q.a);
  const sel = studySelected;
  if (sel.size === 0) return 1;
  let missed = 0, wrong = 0;
  correct.forEach(c => { if (!sel.has(c)) missed++; });
  sel.forEach(s => { if (!correct.has(s)) wrong++; });
  if (missed === 0 && wrong === 0) return 3; // perfect
  if (wrong === 0 && missed <= 1) return 2;  // close
  return 1;
}



function formatDue(ts) {
  const diff = ts - Date.now();
  if (diff <= 0) return 'due now';
  const m = Math.round(diff / 60000);
  if (m < 60) return `in ${m}m`;
  const h = Math.round(m / 60);
  if (h < 24) return `in ${h}h`;
  return `in ${Math.round(h / 24)}d`;
}


/* ============ MATCHING HELPERS ============ */

function isMatchingQuestion(q) {
  return q.a && q.a.some(a => a.includes('→'));
}

function parseMatchingPairs(answers) {
  return answers
    .filter(a => a.includes('→'))
    .map(a => {
      const idx = a.indexOf('→');
      return { left: a.slice(0, idx).trim(), right: a.slice(idx + 1).trim() };
    });
}

function shuffleArray(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function initMatchingState(q) {
  matchingPairs = parseMatchingPairs(q.a);
  matchingLeft = matchingPairs.map((p, i) => ({ text: p.left, pairIdx: i }));
  matchingRight = shuffleArray(matchingPairs.map((p, i) => ({ text: p.right, pairIdx: i })));
  matchingSelectedLeft = null;
  matchingSelectedRight = null;
  matchingDone = {};
  matchingRevealed = false;
}

function renderMatchingUI(q) {
  const total = matchingPairs.length;
  const doneCount = Object.keys(matchingDone).length;

  // Build left column
  const leftHTML = matchingLeft.map((item, li) => {
    const isDone = matchingDone[li] !== undefined;
    let cls = '';
    if (matchingRevealed) {
      cls = isDone && matchingDone[li] === item.pairIdx ? 'reveal-correct matched-done' : 'reveal-missed matched-done';
    } else if (isDone) {
      cls = 'matched-correct matched-done';
    } else if (matchingSelectedLeft === li) {
      cls = 'selected';
    }
    const clickAttr = (!isDone && !matchingRevealed) ? `onclick="selectMatchLeft(${li})"` : '';
    return `<div class="match-item ${cls}" ${clickAttr}>${item.text}</div>`;
  }).join('');

  // Build right column
  const rightHTML = matchingRight.map((item, ri) => {
    const matchedLeftIdx = Object.entries(matchingDone).find(([li, pi]) => matchingRight[ri].pairIdx === pi)?.[0];
    const isDone = matchedLeftIdx !== undefined;
    let cls = '';
    if (matchingRevealed) {
      cls = isDone && matchingLeft[Number(matchedLeftIdx)].pairIdx === item.pairIdx ? 'reveal-correct matched-done' : 'reveal-missed matched-done';
    } else if (isDone) {
      cls = 'matched-correct matched-done';
    } else if (matchingSelectedRight === ri) {
      cls = 'selected';
    }
    const clickAttr = (!isDone && !matchingRevealed) ? `onclick="selectMatchRight(${ri})"` : '';
    return `<div class="match-item ${cls}" ${clickAttr}>${item.text}</div>`;
  }).join('');

  // Score / reveal result
  let scoreHTML = '';
  if (matchingRevealed) {
    const correct = Object.entries(matchingDone).filter(([li, pi]) => matchingLeft[Number(li)].pairIdx === pi).length;
    const pct = Math.round((correct / total) * 100);
    const cls = pct === 100 ? 'perfect' : pct >= 50 ? 'partial' : 'wrong';
    const icon = pct === 100 ? '✓' : '✗';
    scoreHTML = `<div class="matching-score ${cls}">${icon} ${correct}/${total} correct (${pct}%)</div>`;
    
    // Show correct pairs
    scoreHTML += `<div class="matching-pairs">`;
    matchingPairs.forEach(p => {
      scoreHTML += `<div class="matching-pair-row">
        <div class="matching-pair-left">${p.left}</div>
        <div class="matching-pair-arrow">→</div>
        <div class="matching-pair-right" style="color:var(--accent)">${p.right}</div>
      </div>`;
    });
    scoreHTML += `</div>`;
    scoreHTML += `<span class="explain">${q.explain}</span>`;
  } else if (doneCount === total) {
    scoreHTML = `<div class="matching-score perfect">✓ All matched! Check your answers below.</div>`;
  } else {
    scoreHTML = `<div class="study-hint-line">// Click a left item, then the matching right item</div>`;
  }

  const actionsHTML = matchingRevealed ? `
    <div class="study-actions">
      <button class="study-btn rate-1" onclick="rateQuestion(1)">Again<span class="hint">~1 min</span></button>
      <button class="study-btn rate-2" onclick="rateQuestion(2)">Good<span class="hint">~15 min</span></button>
      <button class="study-btn rate-3" onclick="rateQuestion(3)">Easy<span class="hint">hours+</span></button>
    </div>` : `
    <div class="study-actions">
      <button class="study-btn reveal" onclick="revealMatching()">Reveal answers →</button>
    </div>`;

  return `
    <div class="matching-container">
      <div class="matching-col">
        <div class="matching-col-label">// Terms</div>
        ${leftHTML}
      </div>
      <div class="matching-col">
        <div class="matching-col-label">// Definitions</div>
        ${rightHTML}
      </div>
    </div>
    ${scoreHTML}
    ${actionsHTML}
  `;
}

function selectMatchLeft(li) {
  if (matchingDone[li] !== undefined) return;
  if (matchingSelectedLeft === li) {
    matchingSelectedLeft = null;
  } else {
    matchingSelectedLeft = li;
    if (matchingSelectedRight !== null) {
      attemptMatch();
      return;
    }
  }
  renderStudy();
}

function selectMatchRight(ri) {
  if (Object.values(matchingDone).includes(matchingRight[ri].pairIdx)) return;
  if (matchingSelectedRight === ri) {
    matchingSelectedRight = null;
  } else {
    matchingSelectedRight = ri;
    if (matchingSelectedLeft !== null) {
      attemptMatch();
      return;
    }
  }
  renderStudy();
}

function attemptMatch() {
  const li = matchingSelectedLeft;
  const ri = matchingSelectedRight;
  const leftPairIdx = matchingLeft[li].pairIdx;
  const rightPairIdx = matchingRight[ri].pairIdx;
  
  if (leftPairIdx === rightPairIdx) {
    // Correct!
    matchingDone[li] = rightPairIdx;
  }
  // Whether right or wrong, deselect both (user gets visual feedback via no-match)
  matchingSelectedLeft = null;
  matchingSelectedRight = null;
  renderStudy();
}

function revealMatching() {
  matchingRevealed = true;
  renderStudy();
}

function renderStudy() {
  const container = document.getElementById('study-container');
  if (!questions.length) { container.innerHTML = '<div class="study-empty">No questions loaded.</div>'; return; }
  if (!currentStudyQ) currentStudyQ = pickNextStudyQ();
  const q = currentStudyQ;
  const s = srState[q.num];
  const dueLabel = s ? `next review: ${formatDue(s.dueAt)} · reps: ${s.reps}` : 'new card';

  // Handle matching questions separately
  if (isMatchingQuestion(q)) {
    if (matchingPairs.length === 0) {
      initMatchingState(q);
    }
    const metaHTML = `<div class="study-meta"><span>Q${q.num} · ${q.topic}</span><span class="due-tag">${dueLabel}</span></div>`;
    const qHTML = `<div class="study-q">${q.q}</div>`;
    const imgHTML = q.img ? `<img src="${q.img}" alt="Exhibit" style="max-width:100%;margin:0.6rem 0;border:1px solid var(--border);border-radius:4px;" />` : '';
    container.innerHTML = `<div class="study-card">${metaHTML}${qHTML}${imgHTML}${renderMatchingUI(q)}</div>`;
    updateStudyStats();
    return;
  }

  const correctSet = new Set(q.a);
  const multi = q.a.length > 1;
  const noOpts = !q.opts || q.opts.length === 0;

  const optsHTML = (q.opts || []).map(o => {
    const isCorrect = correctSet.has(o);
    const isSelected = studySelected.has(o);
    let cls = '';
    if (studyRevealed) {
      if (isSelected && isCorrect) cls = 'correct locked';
      else if (isSelected && !isCorrect) cls = 'incorrect locked';
      else if (!isSelected && isCorrect) cls = 'missed locked';
      else cls = 'locked';
    } else {
      cls = isSelected ? 'selected' : '';
    }
    const safe = o.replace(/"/g, '&quot;');
    return `<li class="${cls}" ${studyRevealed ? '' : `onclick="toggleStudyOpt('${safe.replace(/'/g, "\\'")}')"`}><span class="box"></span><span>${o}</span></li>`;
  }).join('');

  // Evaluate correctness if revealed
  let allCorrect = false;
  if (studyRevealed && !noOpts) {
    allCorrect = studySelected.size === correctSet.size &&
      [...studySelected].every(s => correctSet.has(s));
  }

  const answerHTML = studyRevealed ? (noOpts ? `
    <div class="study-answer">
      <strong style="color:var(--accent)">Answer${q.a.length>1?'s':''}:</strong>
      <ul style="margin:0.4rem 0 0 0; padding-left:1.1rem;">${q.a.map(a=>`<li>${a}</li>`).join('')}</ul>
      <span class="explain">${q.explain}</span>
    </div>` : `
    <div class="study-answer ${allCorrect ? '' : 'wrong'}">
      <strong style="color:${allCorrect ? 'var(--accent)' : '#ff8a96'}">
        ${allCorrect ? '✓ Correct!' : '✗ Not quite.'}
      </strong>
      Answer: ${q.a.join(', ')}
      <span class="explain">${q.explain}</span>
    </div>`) : '';

  const hintLine = !studyRevealed ? `<div class="study-hint-line">// ${noOpts ? 'think it through, then reveal' : (multi ? `select ${q.a.length} answers` : 'select one answer')}${autoCheck && !noOpts ? ' · auto-checks when complete' : ''}</div>` : '';

  const actionsHTML = studyRevealed ? `
    <div class="study-actions">
      <button class="study-btn rate-1" onclick="rateQuestion(1)">Again<span class="hint">~1 min</span></button>
      <button class="study-btn rate-2" onclick="rateQuestion(2)">Good<span class="hint">~15 min</span></button>
      <button class="study-btn rate-3" onclick="rateQuestion(3)">Easy<span class="hint">hours+</span></button>
    </div>` : (noOpts ? `
    <div class="study-actions">
      <button class="study-btn reveal" onclick="revealStudy()">Reveal answer →</button>
    </div>` : (autoCheck ? '' : `
    <div class="study-actions">
      <button class="study-btn check" onclick="checkStudy()" ${studySelected.size === 0 ? 'disabled' : ''}>Check answer →</button>
    </div>`));

  container.innerHTML = `
    <div class="study-card">
      <div class="study-meta">
        <span>Q${q.num} · ${q.topic}</span>
        <span class="due-tag">${dueLabel}</span>
      </div>
      <div class="study-q">${q.q}</div>${q.img ? `<img src="${q.img}" alt="Exhibit" style="max-width:100%;margin:0.6rem 0;border:1px solid var(--border);border-radius:4px;" />` : ""}
      ${hintLine}
      ${noOpts ? '' : `<ul class="study-opts">${optsHTML}</ul>`}
      ${answerHTML}
      ${actionsHTML}
    </div>
  `;

  updateAutoCheckUI();
  updateStudyStats();
}

function revealStudy() { studyRevealed = true; renderStudy(); }

function updateStudyStats() {
  document.getElementById('study-due').textContent = getDueQuestions().length;
  const today = new Date(); today.setHours(0,0,0,0);
  const startToday = today.getTime();
  document.getElementById('study-today').textContent = history.filter(h => h.t >= startToday).length;
  document.getElementById('study-streak').textContent = computeStreak();
}

function computeStreak() {
  if (!history.length) return 0;
  const days = new Set(history.map(h => {
    const d = new Date(h.t); d.setHours(0,0,0,0); return d.getTime();
  }));
  let streak = 0;
  const day = new Date(); day.setHours(0,0,0,0);
  // if no review today, start from yesterday
  if (!days.has(day.getTime())) day.setDate(day.getDate() - 1);
  while (days.has(day.getTime())) { streak++; day.setDate(day.getDate() - 1); }
  return streak;
}

/* ============ PROGRESS VIEW ============ */
function renderProgress() {
  document.getElementById('p-total').textContent = history.length;
  document.getElementById('p-unique').textContent = new Set(history.map(h => h.num)).size;
  document.getElementById('p-good').textContent = Object.values(srState).filter(s => s.lastRating === 3).length;
  document.getElementById('p-bad').textContent = Object.values(srState).filter(s => s.lastRating === 1).length;
  document.getElementById('p-streak').textContent = computeStreak() + 'd';

  renderDailyChart();
  renderTopicChart();
}

function renderDailyChart() {
  const target = document.getElementById('chart-daily');
  const days = 14;
  const buckets = [];
  const today = new Date(); today.setHours(0,0,0,0);
  for (let i = days - 1; i >= 0; i--) {
    const d = new Date(today); d.setDate(d.getDate() - i);
    const start = d.getTime();
    const end = start + 86400000;
    const items = history.filter(h => h.t >= start && h.t < end);
    buckets.push({
      date: d,
      total: items.length,
      good: items.filter(i => i.rating === 3).length,
      mid: items.filter(i => i.rating === 2).length,
      bad: items.filter(i => i.rating === 1).length,
    });
  }
  const max = Math.max(1, ...buckets.map(b => b.total));
  if (buckets.every(b => b.total === 0)) {
    target.innerHTML = '<div class="empty">// no reviews yet — start studying to see progress</div>';
    return;
  }

  const W = 700, H = 220, P = 30;
  const barW = (W - P * 2) / buckets.length - 6;
  const bars = buckets.map((b, i) => {
    const x = P + i * ((W - P * 2) / buckets.length) + 3;
    let yCursor = H - P;
    const segs = [
      { v: b.good, color: 'var(--accent)' },
      { v: b.mid, color: '#ffb84f' },
      { v: b.bad, color: '#ff5a6e' },
    ];
    const rects = segs.map(seg => {
      if (!seg.v) return '';
      const h = (seg.v / max) * (H - P * 2);
      yCursor -= h;
      return `<rect x="${x}" y="${yCursor}" width="${barW}" height="${h}" fill="${seg.color}" rx="2"/>`;
    }).join('');
    const label = `${b.date.getMonth()+1}/${b.date.getDate()}`;
    const showLabel = i === 0 || i === buckets.length - 1 || i === Math.floor(buckets.length/2);
    return `${rects}${showLabel ? `<text x="${x + barW/2}" y="${H - 10}" text-anchor="middle" fill="var(--text-muted)" font-family="JetBrains Mono" font-size="10">${label}</text>` : ''}`;
  }).join('');

  const gridLines = [0.25, 0.5, 0.75, 1].map(f => {
    const y = H - P - f * (H - P * 2);
    return `<line x1="${P}" y1="${y}" x2="${W - P}" y2="${y}" stroke="var(--border)" stroke-dasharray="2 4"/>
            <text x="${P - 6}" y="${y + 3}" text-anchor="end" fill="var(--text-muted)" font-family="JetBrains Mono" font-size="9">${Math.round(f * max)}</text>`;
  }).join('');

  target.innerHTML = `
    <svg viewBox="0 0 ${W} ${H}" preserveAspectRatio="xMidYMid meet">
      ${gridLines}
      ${bars}
    </svg>
    <div style="display:flex;gap:1rem;justify-content:center;margin-top:0.8rem;font-family:'JetBrains Mono',monospace;font-size:0.68rem;color:var(--text-muted);">
      <span><span style="color:var(--accent)">■</span> Easy</span>
      <span><span style="color:#ffb84f">■</span> Good</span>
      <span><span style="color:#ff5a6e">■</span> Again</span>
    </div>
  `;
}

function renderTopicChart() {
  const target = document.getElementById('chart-topics');
  const byTopic = {};
  questions.forEach(q => {
    if (!byTopic[q.topic]) byTopic[q.topic] = { total: 0, good: 0 };
    byTopic[q.topic].total++;
    const s = srState[q.num];
    if (s && s.lastRating >= 2) byTopic[q.topic].good++;
  });
  const rows = Object.entries(byTopic)
    .map(([name, v]) => ({ name, pct: v.total ? v.good / v.total : 0, total: v.total, good: v.good }))
    .sort((a, b) => b.pct - a.pct);

  if (rows.every(r => r.good === 0)) {
    target.innerHTML = '<div class="empty">// rate some questions to see topic mastery</div>';
    return;
  }

  target.innerHTML = rows.map(r => `
    <div class="topic-row">
      <span class="name" title="${r.name}">${r.name}</span>
      <span class="bar-bg"><span class="bar-fill" style="width:${(r.pct*100).toFixed(0)}%"></span></span>
      <span class="pct">${r.good}/${r.total}</span>
    </div>
  `).join('');
}

function resetProgress() {
  if (!confirm('Reset all study progress and history? This cannot be undone.')) return;
  srState = {}; history = [];
  saveSR(); saveHist();
  currentStudyQ = null; studyRevealed = false; studySelected = new Set();
  renderStudy(); renderProgress();
}

/* ============ EXAM MODE ============ */
let examQuestions = [];
let examAnswers = {}; // { qIndex: Set of selected option strings }
let examCurrentIdx = 0;
let examTimerInterval = null;
let examSecondsLeft = 0;
const EXAM_TOTAL_SECONDS = 70 * 60; // 1h 10min
const EXAM_Q_COUNT = 60;

function startExam() {
  // Pick 60 random questions
  const pool = [...questions].sort(() => Math.random() - 0.5).slice(0, EXAM_Q_COUNT);
  examQuestions = pool;
  examAnswers = {};
  examMatchState = {};
  examCurrentIdx = 0;
  examSecondsLeft = EXAM_TOTAL_SECONDS;

  document.getElementById('exam-start').style.display = 'none';
  document.getElementById('exam-active').style.display = 'block';
  document.getElementById('exam-results').style.display = 'none';

  renderExamDots();
  renderExamQuestion();
  startExamTimer();
}

function startExamTimer() {
  clearInterval(examTimerInterval);
  examTimerInterval = setInterval(() => {
    examSecondsLeft--;
    updateExamTimer();
    if (examSecondsLeft <= 0) {
      clearInterval(examTimerInterval);
      submitExam(true);
    }
  }, 1000);
}

function updateExamTimer() {
  const h = Math.floor(examSecondsLeft / 3600);
  const m = Math.floor((examSecondsLeft % 3600) / 60);
  const s = examSecondsLeft % 60;
  const str = h > 0
    ? `${h}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`
    : `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
  const el = document.getElementById('exam-timer');
  el.textContent = str;
  el.className = 'exam-timer';
  if (examSecondsLeft <= 120) el.classList.add('danger');
  else if (examSecondsLeft <= 600) el.classList.add('warn');
}

function renderExamDots() {
  const nav = document.getElementById('exam-dot-nav');
  nav.innerHTML = examQuestions.map((q, i) => {
    const answered = examIsAnswered(i);
    const isCurrent = i === examCurrentIdx;
    return `<button class="exam-dot${answered ? ' answered' : ''}${isCurrent ? ' current' : ''}"
      onclick="jumpExamQ(${i})" title="Q${i+1}">${i+1}</button>`;
  }).join('');
}

// Per-question matching state for exam (keyed by question index)
let examMatchState = {}; // { [idx]: { left, right, selectedLeft, selectedRight, done } }

function getExamMatchState(idx) {
  if (!examMatchState[idx]) {
    const q = examQuestions[idx];
    const pairs = parseMatchingPairs(q.a);
    examMatchState[idx] = {
      pairs,
      left: pairs.map((p, i) => ({ text: p.left, pairIdx: i })),
      right: shuffleArray(pairs.map((p, i) => ({ text: p.right, pairIdx: i }))),
      selectedLeft: null,
      selectedRight: null,
      done: {}
    };
  }
  return examMatchState[idx];
}

function examIsAnswered(idx) {
  const q = examQuestions[idx];
  if (isMatchingQuestion(q)) {
    const ms = examMatchState[idx];
    if (!ms) return false;
    return Object.keys(ms.done).length === ms.pairs.length;
  }
  return examAnswers[idx] && examAnswers[idx].size > 0;
}

function renderExamQuestion() {
  const q = examQuestions[examCurrentIdx];
  const total = examQuestions.length;

  document.getElementById('exam-qcount').textContent = `Q ${examCurrentIdx + 1} / ${total}`;
  const fillPct = ((examCurrentIdx + 1) / total * 100).toFixed(0);
  document.getElementById('exam-progress-fill').style.width = fillPct + '%';
  document.getElementById('exam-prev-btn').disabled = examCurrentIdx === 0;
  document.getElementById('exam-next-btn').textContent = examCurrentIdx === total - 1 ? 'Review →' : 'Next →';

  const imgHtml = q.img ? `<img src="${q.img}" class="exam-q-img" onerror="this.style.display='none'">` : '';

  let bodyHtml;
  if (isMatchingQuestion(q)) {
    bodyHtml = renderExamMatchingUI(examCurrentIdx);
  } else {
    const selected = examAnswers[examCurrentIdx] || new Set();
    const isMulti = q.a && q.a.length > 1;
    const letters = 'ABCDEFGHIJKLMNOP';
    const opts = (q.opts && q.opts.length > 0) ? q.opts : q.a;
    const multiHint = isMulti
      ? `<div style="font-family:'JetBrains Mono',monospace;font-size:0.68rem;color:var(--accent2);margin-bottom:0.8rem;">Choose ${q.a.length} answers</div>`
      : '';
    const optsHtml = opts.map((opt, i) => {
      const isSelected = selected.has(opt);
      return `<div class="exam-option${isSelected ? ' selected' : ''}" onclick="toggleExamOption(${JSON.stringify(opt)})">
        <span class="opt-letter">${letters[i]}.</span>
        <span>${opt}</span>
      </div>`;
    }).join('');
    bodyHtml = multiHint + optsHtml;
  }

  document.getElementById('exam-q-area').innerHTML = `
    <div class="exam-q-card">
      <div class="exam-q-num">Question ${examCurrentIdx + 1} of ${total} · ${q.topic}</div>
      ${imgHtml}
      <div class="exam-q-text">${q.q}</div>
      ${bodyHtml}
    </div>`;
}

function renderExamMatchingUI(idx) {
  const ms = getExamMatchState(idx);
  const { pairs, left, right, selectedLeft, selectedRight, done } = ms;
  const total = pairs.length;
  const doneCount = Object.keys(done).length;

  const leftHTML = left.map((item, li) => {
    const isDone = done[li] !== undefined;
    let cls = isDone ? 'matched-correct matched-done' : (selectedLeft === li ? 'selected' : '');
    const clickAttr = !isDone ? `onclick="examMatchSelectLeft(${idx},${li})"` : '';
    return `<div class="match-item ${cls}" ${clickAttr}>${item.text}</div>`;
  }).join('');

  const rightHTML = right.map((item, ri) => {
    const matchedLeftIdx = Object.entries(done).find(([li, pi]) => right[ri].pairIdx === Number(pi))?.[0];
    const isDone = matchedLeftIdx !== undefined;
    let cls = isDone ? 'matched-correct matched-done' : (selectedRight === ri ? 'selected' : '');
    const clickAttr = !isDone ? `onclick="examMatchSelectRight(${idx},${ri})"` : '';
    return `<div class="match-item ${cls}" ${clickAttr}>${item.text}</div>`;
  }).join('');

  const hint = doneCount === total
    ? `<div class="matching-score perfect">✓ All matched!</div>`
    : `<div class="study-hint-line">// Click a term, then its matching definition</div>`;

  return `
    <div class="matching-container">
      <div class="matching-col">
        <div class="matching-col-label">// Terms</div>
        ${leftHTML}
      </div>
      <div class="matching-col">
        <div class="matching-col-label">// Definitions</div>
        ${rightHTML}
      </div>
    </div>
    ${hint}`;
}

function examMatchSelectLeft(idx, li) {
  const ms = getExamMatchState(idx);
  if (ms.done[li] !== undefined) return;
  if (ms.selectedLeft === li) { ms.selectedLeft = null; }
  else {
    ms.selectedLeft = li;
    if (ms.selectedRight !== null) { examAttemptMatch(idx); return; }
  }
  renderExamQuestion();
}

function examMatchSelectRight(idx, ri) {
  const ms = getExamMatchState(idx);
  if (Object.values(ms.done).includes(ms.right[ri].pairIdx)) return;
  if (ms.selectedRight === ri) { ms.selectedRight = null; }
  else {
    ms.selectedRight = ri;
    if (ms.selectedLeft !== null) { examAttemptMatch(idx); return; }
  }
  renderExamQuestion();
}

function examAttemptMatch(idx) {
  const ms = getExamMatchState(idx);
  const li = ms.selectedLeft;
  const ri = ms.selectedRight;
  if (ms.left[li].pairIdx === ms.right[ri].pairIdx) {
    ms.done[li] = ms.right[ri].pairIdx;
  }
  ms.selectedLeft = null;
  ms.selectedRight = null;
  renderExamQuestion();
  renderExamDots();
}

function toggleExamOption(opt) {
  const q = examQuestions[examCurrentIdx];
  if (!examAnswers[examCurrentIdx]) examAnswers[examCurrentIdx] = new Set();
  const sel = examAnswers[examCurrentIdx];
  const isMulti = q.a && q.a.length > 1;
  if (isMulti) { if (sel.has(opt)) sel.delete(opt); else sel.add(opt); }
  else { sel.clear(); sel.add(opt); }
  renderExamQuestion();
  renderExamDots();
}

function examNav(dir) {
  const next = examCurrentIdx + dir;
  if (next < 0) return;
  if (next >= examQuestions.length) {
    const unanswered = examQuestions.filter((q, i) => !examIsAnswered(i)).length;
    if (unanswered > 0) {
      if (!confirm(`You have ${unanswered} unanswered question(s). Submit anyway?`)) return;
    }
    submitExam();
    return;
  }
  examCurrentIdx = next;
  renderExamQuestion();
  renderExamDots();
}

function jumpExamQ(idx) {
  examCurrentIdx = idx;
  renderExamQuestion();
  renderExamDots();
}

function submitExam(timeUp = false) {
  clearInterval(examTimerInterval);
  if (!timeUp) {
    const unanswered = examQuestions.filter((q, i) => !examIsAnswered(i)).length;
    if (unanswered > 0 && !confirm(`You have ${unanswered} unanswered question(s). Submit anyway?`)) {
      startExamTimer(); // resume
      return;
    }
  }

  // Grade
  let correct = 0;
  const results = examQuestions.map((q, i) => {
    let isCorrect = false;
    let sel = [];

    if (isMatchingQuestion(q)) {
      const ms = examMatchState[i];
      if (ms) {
        // Build submitted pairs as "left → right" strings
        const submittedPairs = Object.entries(ms.done).map(([li, pi]) => {
          const leftText = ms.left[Number(li)].text;
          const rightText = ms.pairs[pi].right;
          return `${leftText} → ${rightText}`;
        });
        sel = submittedPairs;
        // All pairs correct = full score for this question
        const allCorrect = Object.keys(ms.done).length === ms.pairs.length &&
          Object.entries(ms.done).every(([li, pi]) => ms.left[Number(li)].pairIdx === pi);
        isCorrect = allCorrect;
      }
    } else {
      sel = examAnswers[i] ? [...examAnswers[i]] : [];
      const normalize = s => s.toLowerCase().replace(/\s+/g,' ').trim();
      const correctSet = new Set(q.a.map(normalize));
      const selSet = new Set(sel.map(normalize));
      isCorrect = correctSet.size === selSet.size && [...correctSet].every(c => selSet.has(c));
    }

    if (isCorrect) correct++;
    return { q, sel, isCorrect, correctAnswers: q.a };
  });

  const pct = Math.round(correct / examQuestions.length * 100);
  const passed = pct >= 70;
  const timeTaken = EXAM_TOTAL_SECONDS - examSecondsLeft;
  const mm = Math.floor(timeTaken / 60);
  const ss = timeTaken % 60;

  showExamResults(results, correct, pct, passed, mm, ss);
}

function showExamResults(results, correct, pct, passed, mm, ss) {
  document.getElementById('exam-active').style.display = 'none';
  const resEl = document.getElementById('exam-results');
  resEl.style.display = 'block';

  const wrong = results.filter(r => !r.isCorrect);
  const circumference = 2 * Math.PI * 54;
  const dashOffset = circumference * (1 - pct / 100);
  const ringColor = passed ? '#22c55e' : '#ff5a6e';

  resEl.innerHTML = `
    <div class="exam-results">
      <div class="exam-score-ring">
        <svg width="130" height="130" viewBox="0 0 130 130">
          <circle cx="65" cy="65" r="54" fill="none" stroke="var(--border)" stroke-width="10"/>
          <circle cx="65" cy="65" r="54" fill="none" stroke="${ringColor}" stroke-width="10"
            stroke-dasharray="${circumference}" stroke-dashoffset="${dashOffset}"
            stroke-linecap="round" style="transition:stroke-dashoffset 1s ease;"/>
        </svg>
        <div class="exam-score-inner">
          <div class="exam-score-pct" style="color:${ringColor}">${pct}%</div>
          <div class="exam-score-label">${correct}/60</div>
        </div>
      </div>
      <div class="exam-verdict ${passed ? 'pass' : 'fail'}">${passed ? '✓ PASSED' : '✗ FAILED'}</div>
      <div style="font-family:'JetBrains Mono',monospace;font-size:0.75rem;color:var(--text-muted);margin-bottom:1rem;">
        ${passed ? 'Congratulations! You passed the exam.' : `You need 70% (42/60) to pass. Keep studying!`}
      </div>
      <div class="exam-result-stats">
        <div class="exam-result-stat"><div class="ers-label">Correct</div><div class="ers-val" style="color:#22c55e">${correct}</div></div>
        <div class="exam-result-stat"><div class="ers-label">Wrong</div><div class="ers-val" style="color:#ff5a6e">${60 - correct}</div></div>
        <div class="exam-result-stat"><div class="ers-label">Score</div><div class="ers-val" style="color:${ringColor}">${pct}%</div></div>
        <div class="exam-result-stat"><div class="ers-label">Time</div><div class="ers-val" style="color:var(--text-muted);font-size:1rem;">${mm}m ${ss}s</div></div>
      </div>
      <div style="margin-bottom:2rem;">
        <button class="exam-retry-btn" onclick="startExam()">Retry Exam</button>
        <button class="exam-review-btn" onclick="showExamReview()">Review Wrong Answers (${wrong.length})</button>
        <button class="exam-review-btn" onclick="document.getElementById('exam-start').style.display='';document.getElementById('exam-results').style.display='none';">← Back</button>
      </div>
      <div id="exam-review-area"></div>
    </div>`;

  // Store results for review
  window._examResults = results;
}

function showExamReview() {
  const results = window._examResults || [];
  const wrong = results.filter(r => !r.isCorrect);
  if (wrong.length === 0) {
    document.getElementById('exam-review-area').innerHTML = '<p style="color:var(--accent);font-family:JetBrains Mono,monospace;font-size:0.8rem;text-align:center;">🎉 All answers correct!</p>';
    return;
  }
  document.getElementById('exam-review-area').innerHTML = `
    <div style="text-align:left;">
      <h3 style="font-family:'JetBrains Mono',monospace;font-size:0.78rem;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.08em;margin-bottom:1rem;text-align:center;">Wrong Answers — ${wrong.length} question(s)</h3>
      ${wrong.map(r => {
        const isMatch = isMatchingQuestion(r.q);
        const yourAnswer = isMatch
          ? (r.sel.length > 0 ? r.sel.map(s => `<div style="margin:1px 0">• ${s}</div>`).join('') : '(no answer)')
          : (r.sel.length > 0 ? r.sel.join(', ') : '(no answer)');
        const correctAnswer = isMatch
          ? r.correctAnswers.map(s => `<div style="margin:1px 0">• ${s}</div>`).join('')
          : r.correctAnswers.join(', ');
        return `
        <div class="exam-review-card wrong">
          <div class="exam-review-q">${r.q.q}</div>
          ${r.q.img ? `<img src="${r.q.img}" style="max-width:100%;border-radius:4px;margin-bottom:0.5rem;" onerror="this.style.display='none'">` : ''}
          <div class="exam-review-your">✗ Your answer: ${yourAnswer}</div>
          <div class="exam-review-correct">✓ Correct: ${correctAnswer}</div>
          ${r.q.explain ? `<div class="exam-review-explain">${r.q.explain}</div>` : ''}
        </div>`;
      }).join('')}
    </div>`;
}

/* ============ VIEW SWITCHING ============ */
function switchView(name) {
  // Reset matching when switching to study
  if (name === 'study') { matchingPairs = []; matchingLeft = []; matchingRight = []; matchingSelectedLeft = null; matchingSelectedRight = null; matchingDone = {}; matchingRevealed = false; }
  // Pause exam timer when leaving exam view
  if (name !== 'exam' && examTimerInterval) { clearInterval(examTimerInterval); examTimerInterval = null; }
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.toggle('active', t.dataset.view === name));
  document.querySelectorAll('.view').forEach(v => v.classList.toggle('active', v.id === 'view-' + name));
  if (name === 'study') renderStudy();
  if (name === 'progress') renderProgress();
}

document.querySelectorAll('.nav-tab').forEach(t => {
  t.addEventListener('click', () => switchView(t.dataset.view));
});

init();


</script>
</body>
</html>
