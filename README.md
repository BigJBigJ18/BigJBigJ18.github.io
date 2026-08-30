
<html lang="de">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Lernplattform</title>
<style>
/* ============================= DESIGN TOKENS ============================= */
:root{
  --bg:#0f1218;
  --bg-2:#151a22;
  --card:#1a2029;
  --card-2:#20272f;
  --border:#2a323d;
  --text:#e6e9ee;
  --muted:#98a2b3;
  --accent:#6c8cff;
  --accent-2:#8b7cf6;
  --success:#4ea87a;
  --warning:#d3a04a;
  --error:#c96a6a;
  --radius:14px;
  --shadow:0 8px 24px rgba(0,0,0,.35);
  --font:'Inter',system-ui,-apple-system,'Segoe UI',Roboto,'Helvetica Neue',Arial,sans-serif;
}
*{box-sizing:border-box}
html,body{margin:0;padding:0}
body{
  background:var(--bg);color:var(--text);font-family:var(--font);
  font-size:16px;line-height:1.6;-webkit-font-smoothing:antialiased;
  min-height:100vh;overflow-x:hidden;
}
h1,h2,h3{line-height:1.25;margin:0 0 .4em}
h1{font-size:1.9rem;font-weight:650;letter-spacing:-.02em}
h2{font-size:1.3rem;font-weight:600;letter-spacing:-.01em}
h3{font-size:1.05rem;font-weight:600}
p{margin:0 0 1em}
button{font-family:inherit;font-size:1rem}
:focus-visible{outline:2px solid var(--accent);outline-offset:2px;border-radius:8px}

.wrap{max-width:880px;margin:0 auto;padding:28px 20px 80px}
.muted{color:var(--muted)}
.small{font-size:.875rem}
.row{display:flex;gap:12px;align-items:center;flex-wrap:wrap}
.spread{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap}

/* ============================= BUTTONS ============================= */
.btn{
  display:inline-flex;align-items:center;justify-content:center;gap:8px;
  padding:11px 18px;border-radius:10px;border:1px solid var(--border);
  background:var(--card-2);color:var(--text);cursor:pointer;
  transition:background .15s ease,border-color .15s ease,transform .12s ease;
  min-height:44px;
}
.btn:hover{background:#28313c}
.btn:active{transform:translateY(1px)}
.btn-primary{background:var(--accent);border-color:var(--accent);color:#0d1017;font-weight:600}
.btn-primary:hover{background:#7d99ff}
.btn-ghost{background:transparent}
.btn-ghost:hover{background:var(--card-2)}
.btn-danger{border-color:#4a2b2b;color:#e2a1a1;background:transparent}
.btn-danger:hover{background:#2a1c1c}
.btn-block{width:100%}
.btn:disabled{opacity:.5;cursor:not-allowed}

.rate{flex:1 1 30%;min-width:100px;font-weight:600;padding:16px 12px;font-size:1rem;flex-direction:column;gap:2px}
.rate small{font-weight:400;opacity:.75;font-size:.72rem}
.rate-bad{background:rgba(201,106,106,.14);border-color:rgba(201,106,106,.45);color:#e59b9b}
.rate-bad:hover{background:rgba(201,106,106,.24)}
.rate-mid{background:rgba(211,160,74,.14);border-color:rgba(211,160,74,.45);color:#e0bb7c}
.rate-mid:hover{background:rgba(211,160,74,.24)}
.rate-good{background:rgba(78,168,122,.14);border-color:rgba(78,168,122,.45);color:#82c8a3}
.rate-good:hover{background:rgba(78,168,122,.24)}

/* ============================= CARDS ============================= */
.card{
  background:var(--card);border:1px solid var(--border);border-radius:var(--radius);
  padding:22px;box-shadow:var(--shadow);
}
.course-card{cursor:pointer;transition:border-color .15s ease,transform .15s ease}
.course-card:hover{border-color:#3b4757;transform:translateY(-2px)}
.grid{display:grid;gap:16px;grid-template-columns:repeat(auto-fill,minmax(280px,1fr))}

.bar{height:8px;border-radius:99px;background:#232b35;overflow:hidden}
.bar > i{display:block;height:100%;border-radius:99px;
  background:linear-gradient(90deg,var(--accent),var(--accent-2));transition:width .5s ease}

.badge{display:inline-block;padding:3px 10px;border-radius:99px;background:var(--card-2);
  border:1px solid var(--border);font-size:.78rem;color:var(--muted)}
.badge-done{background:rgba(78,168,122,.12);border-color:rgba(78,168,122,.4);color:#82c8a3}

/* ============================= TABS ============================= */
.tabs{display:flex;gap:6px;background:var(--bg-2);border:1px solid var(--border);
  padding:5px;border-radius:12px;margin:18px 0 24px;overflow-x:auto}
.tab{flex:1;min-width:92px;padding:10px 12px;border-radius:9px;border:none;background:transparent;
  color:var(--muted);cursor:pointer;font-weight:500;transition:background .15s,color .15s;min-height:42px}
.tab:hover{color:var(--text)}
.tab[aria-selected="true"]{background:var(--card);color:var(--text);box-shadow:0 1px 3px rgba(0,0,0,.4)}

/* ============================= FLASHCARD ============================= */
.flash{
  background:var(--card);border:1px solid var(--border);border-radius:18px;
  padding:32px 26px;min-height:230px;display:flex;flex-direction:column;justify-content:center;
  animation:fade .22s ease;
}
.q{font-size:1.28rem;font-weight:600;line-height:1.45}
.a{font-size:1.06rem;color:#cdd4de;border-top:1px solid var(--border);margin-top:22px;padding-top:20px;
  animation:reveal .25s ease;white-space:pre-wrap}
@keyframes fade{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}
@keyframes reveal{from{opacity:0;transform:translateY(-4px)}to{opacity:1;transform:none}}

/* ============================= BROWSE GRID + WINDOW ============================= */
.browse-grid{display:grid;gap:12px;grid-template-columns:repeat(auto-fill,minmax(230px,1fr))}
.browse-item{
  background:var(--card);border:1px solid var(--border);border-radius:12px;
  padding:16px;cursor:pointer;text-align:left;color:var(--text);
  transition:border-color .15s ease,transform .15s ease;
  display:flex;flex-direction:column;gap:10px;min-height:110px;
}
.browse-item:hover{border-color:#3b4757;transform:translateY(-2px)}
.browse-item .num{font-size:.72rem;color:var(--muted);font-family:ui-monospace,monospace}
.browse-item .qt{font-size:.95rem;font-weight:550;line-height:1.4;flex:1}

.overlay{
  position:fixed;inset:0;background:rgba(8,10,14,.72);backdrop-filter:blur(4px);
  display:flex;align-items:center;justify-content:center;padding:20px;z-index:50;
  animation:fadeIn .18s ease;
}
.window{
  background:var(--bg-2);border:1px solid var(--border);border-radius:18px;
  box-shadow:0 24px 64px rgba(0,0,0,.55);
  width:100%;max-width:640px;max-height:calc(100vh - 40px);overflow-y:auto;
  padding:22px 22px 26px;animation:popIn .2s ease;
}
.window-bar{display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:16px}
.window-dots{display:flex;gap:6px}
.window-dots i{width:10px;height:10px;border-radius:50%;background:var(--border);display:block}
.window-dots i:first-child{background:#c96a6a}
.window-dots i:nth-child(2){background:#d3a04a}
.window-dots i:nth-child(3){background:#4ea87a}
.window-close{
  border:1px solid var(--border);background:var(--card-2);color:var(--muted);
  width:34px;height:34px;border-radius:9px;cursor:pointer;font-size:1rem;line-height:1;
  display:inline-flex;align-items:center;justify-content:center;transition:background .15s,color .15s;
}
.window-close:hover{background:#28313c;color:var(--text)}
.window .flash{min-height:200px}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes popIn{from{opacity:0;transform:scale(.96) translateY(8px)}to{opacity:1;transform:none}}

/* ============================= STATS / CHART ============================= */
.stats{display:grid;gap:12px;grid-template-columns:repeat(auto-fill,minmax(150px,1fr))}
.stat{background:var(--bg-2);border:1px solid var(--border);border-radius:12px;padding:14px 16px}
.stat b{display:block;font-size:1.5rem;font-weight:650;letter-spacing:-.02em}
.chart{display:flex;flex-direction:column;gap:10px}
.chart-row{display:grid;grid-template-columns:44px 1fr 42px;align-items:center;gap:10px}
.chart-bar{height:20px;border-radius:6px;background:#232b35;overflow:hidden}
.chart-bar > i{display:block;height:100%;background:linear-gradient(90deg,var(--accent),var(--accent-2));
  transition:width .5s ease;min-width:2px}
.ring{display:block;margin:0 auto}

input[type=text],textarea,select{
  width:100%;padding:13px 15px;border-radius:10px;border:1px solid var(--border);
  background:var(--bg-2);color:var(--text);font-size:1rem;font-family:inherit;min-height:46px}
textarea{min-height:96px;resize:vertical;line-height:1.5}
select{cursor:pointer;appearance:none;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%2398a2b3' stroke-width='2' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
  background-repeat:no-repeat;background-position:right 14px center}
input[type=text]::placeholder,textarea::placeholder{color:#66707e}
label{display:block;margin-bottom:8px;font-weight:500}

.empty{text-align:center;padding:44px 20px;color:var(--muted)}
.empty .icon{font-size:2rem;display:block;margin-bottom:10px}
.center{text-align:center}
.kbd{display:inline-block;padding:1px 7px;border:1px solid var(--border);border-radius:6px;
  background:var(--bg-2);font-size:.75rem;color:var(--muted);font-family:ui-monospace,monospace}

@media (max-width:640px){
  .wrap{padding:20px 14px 70px}
  h1{font-size:1.55rem}
  .flash{padding:24px 18px;min-height:190px}
  .q{font-size:1.12rem}
  .rate{flex:1 1 100%}
  .card{padding:18px}
  .window{padding:16px 16px 20px}
  .browse-grid{grid-template-columns:1fr 1fr}
}
@media (prefers-reduced-motion:reduce){
  *{animation:none!important;transition:none!important}
}
</style>
</head>
<body>
<div id="app" class="wrap"></div>

<script>
"use strict";
/* =====================================================================
   DATA — Kursinhalte. NUR HIER etwas hinzufügen, um Kurse/Fragen zu ergänzen.
   ===================================================================== */
const COURSES = [
  {
    id: "ccna",
    title: "1 CCNA Exam",
    description: "Cisco CCNA Prüfungsvorbereitung",
    questions: [
      { id:"ccna-001", question:"What is the purpose of a default gateway?",
        answer:"The default gateway is the router interface a host sends packets to when the destination IP is outside its own subnet. It forwards traffic between networks." },
      { id:"ccna-002", question:"Which protocol automatically assigns IP addresses to devices?",
        answer:"DHCP (Dynamic Host Configuration Protocol). It leases IP address, subnet mask, default gateway and DNS servers via DISCOVER / OFFER / REQUEST / ACK." },
      { id:"ccna-003", question:"What is the difference between a collision domain and a broadcast domain?",
        answer:"A switch port = one collision domain (each port separates collisions).\nA router interface / VLAN = one broadcast domain (routers do not forward broadcasts)." },
      { id:"ccna-004", question:"How many usable host addresses are in a /26 network?",
        answer:"62.\n/26 → 64 addresses total (2^6), minus network address and broadcast address = 62 usable hosts." },
      { id:"ccna-005", question:"What are the layers of the OSI model (1–7)?",
        answer:"1 Physical, 2 Data Link, 3 Network, 4 Transport, 5 Session, 6 Presentation, 7 Application." },
      { id:"ccna-006", question:"What does a switch do when it receives a frame with an unknown destination MAC?",
        answer:"It floods the frame out of all ports in the same VLAN except the port it arrived on (unknown unicast flooding)." },
      { id:"ccna-007", question:"What is the difference between TCP and UDP?",
        answer:"TCP: connection-oriented, 3-way handshake, reliable, ordered, flow & congestion control (HTTP, SSH).\nUDP: connectionless, no guarantees, low overhead (DNS, VoIP, DHCP)." },
      { id:"ccna-008", question:"What is a VLAN and why is it used?",
        answer:"A VLAN is a logical Layer-2 segmentation of a switch. It creates separate broadcast domains, improves security and organisation without extra hardware. Inter-VLAN traffic requires a router or L3 switch." },
      { id:"ccna-009", question:"What is the administrative distance of OSPF and of static routes?",
        answer:"Static route: 1. OSPF: 110. (Connected: 0, EIGRP internal: 90, RIP: 120.) Lower AD = more trusted." },
      { id:"ccna-010", question:"What does NAT do?",
        answer:"Network Address Translation rewrites IP addresses in packet headers, typically translating private RFC1918 addresses to a public address (PAT/overload uses port numbers to share one public IP)." }
      /* Neue Frage einfach hier anhängen:
         { id:"ccna-011", question:"...", answer:"..." } */
    ]
  },
  {
    id: "pos-herbstpruefung",
    title: "POS-Herbstprüfung",
    description: "Vorbereitung auf die POS-Herbstprüfung",
    questions: [
      { id:"pos-001", question:"Was ist ein package?",
        answer:"Ein Directory, in dem sich eine Klasse befindet." },
      { id:"pos-002", question:"Wo/Wie gebe ich ein package an?",
        answer:"In jeder Klasse ganz oben: \"package (name);\"" },
      { id:"pos-003", question:"Wie kann ich alle Subklassen eines Package mit einem import einholen?",
        answer:"\"import (package).*;\"" },
      { id:"pos-004", question:"Was sind static Variablen?",
        answer:"Variablen, die, wenn sie von einer Instanz der Klasse geändert werden, auch bei allen anderen Klassen geändert werden." },
      { id:"pos-005", question:"Wie kann man statische Variablen abrufen?",
        answer:"Entweder mit einer Instanz der Klasse oder mit \"(Klasse).(variable)\"" },
      { id:"pos-006", question:"Was ist das Besondere an statischen Methoden?",
        answer:"Man kann sie ohne Instanz der Klasse aufrufen." },
      { id:"pos-007", question:"Was ist eine Instanz einer Klasse?",
        answer:"Wenn man new (Klasse) macht, ist das Ergebnis eine Instanz." },
      { id:"pos-008", question:"Was ist der Garbage-Collector?",
        answer:"Managed den Speicherplatz, indem er ungenutzte Objekte entfernt. Ist automatisch in der JVM integriert." },
      { id:"pos-009", question:"Was ist der Vorteil eines Garbage-Collectors?",
        answer:"Man muss nicht an den Speicherplatz denken → Alles Unnötige ist weg." },
      { id:"pos-010", question:"Was ist der Nachteil eines Garbage-Collectors?",
        answer:"Speicherplatz für andere Anwendungen kann nicht erzwungen werden." },
      { id:"pos-011", question:"Was sind Konstanten?",
        answer:"Variablen, die durch final nicht mehr geändert werden können, d.h. immer gleich sein werden." },
      { id:"pos-012", question:"Wie ist die Struktur der Namen von 1) Klassen, 2) Konstanten und dem Rest?",
        answer:"1) Klassen beginnen mit einem Großbuchstaben, 2) Konstanten bestehen nur aus Großbuchstaben und der Rest nur aus Kleinbuchstaben." },
      { id:"pos-013", question:"Was sind die 2 Arten von Datentypen?",
        answer:"Primitive Datentypen und Objekt-Typen." },
      { id:"pos-014", question:"Was sind Primitive Datentypen?",
        answer:"Alle Typen, die man klein schreibt (float, double, byte, int, etc.). Wenn sie Klassenvariablen sind, werden sie automatisch auf den Default (meist 0) gesetzt. Bei lokalen Variablen ist das nicht der Fall (=Fehler)." },
      { id:"pos-015", question:"Was sind Objekt-Typen?",
        answer:"Alle Typen, die man groß schreibt (also Klassen sind), z.B.: String, (Enums), alle Klassen." },
      { id:"pos-016", question:"Was ist der Unterschied zwischen den beiden Datentypen?",
        answer:"Bei Objekt-Typen wird eine Referenz auf das Objekt im Speicher genommen [by reference]. Bei primitiven Datentypen wird der Wert immer als Wert gespeichert [by value]." },
      { id:"pos-017", question:"Was sind Membervariablen?",
        answer:"Variablen, die in einer Klasse oben angegeben sind (Klassenvariablen)." },
      { id:"pos-018", question:"Was sind lokale Variablen?",
        answer:"Variablen, die nur in einer Methode sind." },
      { id:"pos-019", question:"Was sind alle MATH-Funktionen?",
        answer:"Max, Min, Ceil, Floor, Round, Sqrt, Abs, Pow, Random.\n• max(a,b) / min(a,b): größerer / kleinerer Wert\n• ceil(x): aufrunden · floor(x): abrunden\n• round(x): kaufmännisch runden\n• sqrt(x): Wurzel · abs(x): Betrag\n• pow(a,b): a hoch b · random(): Zufallszahl 0.0–<1.0" },
      { id:"pos-020", question:"Wie kann man beim Start eines Programms Argumente mitgeben?",
        answer:"Mit dem args[] bei main(). In IntelliJ kann man oben bei Konfigurationen \"Program Arguments\" angeben → Dort Satz eingeben → Jedes Leerzeichen fungiert wie ein split() → Jedes Wort einzeln in den Argumenten." },
      { id:"pos-021", question:"Wie funktioniert der Binäre Baum?",
        answer:"Es gibt eine root-Node, die auf bis zu 2 weitere zeigt → Wenn der nächste Wert <= als der jetzige ist, wird eine neue Node links erstellt, sonst rechts → sollte es dort schon eine geben, wird es an diese übergeben." },
      { id:"pos-022", question:"Was ist eine rekursive Methode?",
        answer:"Eine Methode, die sich in sich selbst wieder aufruft." },
      { id:"pos-023", question:"Was ist eine iterative Methode?",
        answer:"Eine Methode, die mit einer Schleife Aufgaben erledigt." },
      { id:"pos-024", question:"Was sind gute Anwendungen von rekursiven Methoden?",
        answer:"Fakultät, Summe von Arrayelementen, Array von hinten ausgeben." }
    ]
  }
];

/* =====================================================================
   STORAGE — zentrale Persistenz (localStorage)
   ===================================================================== */
const STORAGE_KEY = "learningAppData";
const DATA_VERSION = 1;

function defaultData(){
  return { version: DATA_VERSION, name: null, progress: {}, dailyStats: {}, customQuestions: [] };
}

let data = defaultData();

function loadData(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw) return defaultData();
    const parsed = JSON.parse(raw);
    if(!parsed || typeof parsed !== "object") return defaultData();
    const migrated = migrate(parsed);
    return {
      version: DATA_VERSION,
      name: typeof migrated.name === "string" && migrated.name.trim() ? migrated.name : null,
      progress: (migrated.progress && typeof migrated.progress === "object") ? migrated.progress : {},
      dailyStats: (migrated.dailyStats && typeof migrated.dailyStats === "object") ? migrated.dailyStats : {},
      customQuestions: Array.isArray(migrated.customQuestions) ? migrated.customQuestions : []
    };
  }catch(e){
    console.warn("Beschädigte Daten – Fallback auf Standardwerte.", e);
    return defaultData();
  }
}

/** Platz für spätere Migrationen (v1 -> v2 ...) */
function migrate(obj){
  if(!obj.version) obj.version = DATA_VERSION;
  return obj;
}

function saveData(){
  try{ localStorage.setItem(STORAGE_KEY, JSON.stringify(data)); }
  catch(e){ console.warn("Speichern fehlgeschlagen", e); }
}

/* Eigene Fragen in die Kursdaten einhängen */
function applyCustomQuestions(){
  COURSES.forEach(c => {
    c.questions = c.questions.filter(q => !q.id.startsWith("custom-"));
  });
  data.customQuestions.forEach(q => {
    const course = COURSES.find(c => c.id === q.courseId);
    if(course) course.questions.push({ id:q.id, question:q.question, answer:q.answer });
  });
}

const isAdmin = () => (data.name || "").trim().toLowerCase() === "jojo";

/* =====================================================================
   STATE
   ===================================================================== */
const state = {
  view: "home",          // onboarding | home | course | settings
  courseId: null,
  tab: "browse",         // browse | study | progress
  browseIndex: null,     // null = Fenster geschlossen, sonst Kartenindex
  browseRevealed: false,
  study: null            // { courseId, currentCardId, revealed, cardsStudied, lastCardId, allowNotDue }
};

/* =====================================================================
   UTILITIES
   ===================================================================== */
const el = (tag, props = {}, children = []) => {
  const n = document.createElement(tag);
  for(const [k,v] of Object.entries(props)){
    if(k === "class") n.className = v;
    else if(k === "text") n.textContent = v;       // sicheres Einfügen (kein HTML)
    else if(k === "html") n.innerHTML = v;         // nur für statisches Markup
    else if(k.startsWith("on") && typeof v === "function") n.addEventListener(k.slice(2), v);
    else if(v !== null && v !== undefined && v !== false) n.setAttribute(k, v);
  }
  (Array.isArray(children) ? children : [children]).forEach(c => {
    if(c === null || c === undefined || c === false) return;
    n.appendChild(typeof c === "string" ? document.createTextNode(c) : c);
  });
  return n;
};
const $ = sel => document.querySelector(sel);
const shuffleArray = arr => { const a = arr.slice();
  for(let i=a.length-1;i>0;i--){ const j = Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; } return a; };
const todayKey = (d = new Date()) => {
  const p = n => String(n).padStart(2,"0");
  return `${d.getFullYear()}-${p(d.getMonth()+1)}-${p(d.getDate())}`;
};
const WEEKDAYS = ["So","Mo","Di","Mi","Do","Fr","Sa"];
function formatRelativeTime(ts){
  const diff = ts - Date.now();
  if(diff <= 0) return "jetzt fällig";
  const m = Math.round(diff/60000);
  if(m < 60) return `in ${m} Min`;
  const h = Math.round(m/60);
  if(h < 24) return `in ${h} Std`;
  return `in ${Math.round(h/24)} Tagen`;
}

/* =====================================================================
   DATA ACCESS / SPACED REPETITION
   ===================================================================== */
const INTERVALS = { bad: 60*1000, mid: 30*60*1000, good: 3*60*60*1000 };
const RATING_LABEL = { bad:"Schlecht", mid:"Mittel", good:"Gut" };

const getCourse = id => COURSES.find(c => c.id === id) || null;
const getQuestion = (course, qid) => course ? course.questions.find(q => q.id === qid) || null : null;
const getCardProgress = qid => data.progress[qid] || null;
const calculateNextReview = rating => Date.now() + (INTERVALS[rating] ?? INTERVALS.mid);

/** Karte gilt als "gelernt", sobald sie mindestens einmal bewertet wurde. */
const isStudied = qid => !!getCardProgress(qid);

function calculateCourseProgress(course){
  if(!course || course.questions.length === 0) return { studied:0, total:0, percent:0 };
  const total = course.questions.length;
  const studied = course.questions.filter(q => isStudied(q.id)).length;
  return { studied, total, percent: Math.round(studied/total*100) };
}

function getDueCards(course){
  const now = Date.now();
  return course.questions.filter(q => {
    const p = getCardProgress(q.id);
    return p && typeof p.nextReview === "number" && p.nextReview <= now;
  });
}
const getNewCards = course => course.questions.filter(q => !getCardProgress(q.id));

/**
 * Kartenauswahl: 1) fällige, 2) neue, 3) (optional) restliche Karten.
 * Gleiche Priorität → zufällige Reihenfolge. Direkte Wiederholung wird vermieden.
 */
function getStudyCards(course, { includeNotDue = false } = {}){
  const due = shuffleArray(getDueCards(course));
  const fresh = shuffleArray(getNewCards(course));
  let pool = [...due, ...fresh];
  if(includeNotDue){
    const rest = shuffleArray(course.questions.filter(q => !pool.includes(q)));
    pool = [...pool, ...rest];
  }
  return pool;
}

function pickNextCard(course, lastCardId, includeNotDue){
  const pool = getStudyCards(course, { includeNotDue });
  if(pool.length === 0) return null;
  const filtered = pool.filter(q => q.id !== lastCardId);
  return (filtered.length ? filtered : pool)[0];
}

function recordReview(courseId, questionId, rating){
  const prev = getCardProgress(questionId) || { reviewCount:0, correctCount:0, mediumCount:0, wrongCount:0 };
  data.progress[questionId] = {
    courseId,
    lastReviewed: Date.now(),
    nextReview: calculateNextReview(rating),
    reviewCount: (prev.reviewCount||0) + 1,
    correctCount: (prev.correctCount||0) + (rating === "good" ? 1 : 0),
    mediumCount: (prev.mediumCount||0) + (rating === "mid" ? 1 : 0),
    wrongCount:  (prev.wrongCount||0)  + (rating === "bad" ? 1 : 0),
    lastRating: rating
  };
  const key = todayKey();
  data.dailyStats[key] = (data.dailyStats[key] || 0) + 1;
  saveData();
}

const getDailyStats = (days = 7) => {
  const out = [];
  for(let i = days-1; i >= 0; i--){
    const d = new Date(); d.setDate(d.getDate() - i);
    const k = todayKey(d);
    out.push({ key:k, label:WEEKDAYS[d.getDay()], count:data.dailyStats[k] || 0 });
  }
  return out;
};
const todayCount = () => data.dailyStats[todayKey()] || 0;

/* =====================================================================
   NAVIGATION (History API, kein Reload)
   ===================================================================== */
function navigate(next, push = true){
  Object.assign(state, next);
  if(push){
    const hash = state.view === "course"
      ? `#/course/${state.courseId}/${state.tab}`
      : state.view === "settings" ? "#/settings" : "#/";
    if(location.hash !== hash) history.pushState({}, "", hash);
  }
  renderApp();
}

function applyHash(){
  const h = location.hash.replace(/^#\/?/, "");
  const parts = h.split("/").filter(Boolean);
  if(parts[0] === "course" && getCourse(parts[1])){
    const tab = ["browse","study","progress"].includes(parts[2]) ? parts[2] : "browse";
    if(state.courseId !== parts[1]){ state.browseIndex = null; state.study = null; }
    Object.assign(state, { view:"course", courseId:parts[1], tab, browseRevealed:false });
  } else if(parts[0] === "settings"){
    state.view = "settings";
  } else {
    state.view = "home";
  }
}
window.addEventListener("popstate", () => { if(data.name){ applyHash(); renderApp(); } });

/* =====================================================================
   SHARED UI PARTS
   ===================================================================== */
function renderEmptyState(icon, title, text, action){
  return el("div",{class:"empty"},[
    el("span",{class:"icon",text:icon}),
    el("h3",{text:title}),
    text ? el("p",{class:"small",text:text}) : null,
    action || null
  ]);
}
function progressBar(percent){
  return el("div",{class:"bar",role:"progressbar","aria-valuenow":percent,"aria-valuemin":0,"aria-valuemax":100},[
    el("i",{style:`width:${percent}%`})
  ]);
}
function topBar(){
  return el("div",{class:"spread",style:"margin-bottom:22px"},[
    el("button",{class:"btn btn-ghost", "aria-label":"Zur Kursübersicht",
      onclick:() => navigate({view:"home"})},"← Alle Kurse"),
    el("button",{class:"btn btn-ghost", onclick:() => navigate({view:"settings"})},"⚙ Einstellungen")
  ]);
}

/* =====================================================================
   ONBOARDING
   ===================================================================== */
function renderOnboarding(){
  const input = el("input",{type:"text",id:"nameInput",placeholder:"Dein Name",autocomplete:"off",maxlength:"40"});
  const err = el("p",{class:"small",style:"color:var(--error);min-height:20px;margin:8px 0 0"});
  const submit = () => {
    const v = input.value.trim();
    if(!v){ err.textContent = "Bitte gib einen Namen ein."; input.focus(); return; }
    data.name = v; saveData();
    navigate({view:"home"});
  };
  input.addEventListener("keydown", e => { if(e.key === "Enter") submit(); });

  const box = el("div",{class:"card",style:"max-width:440px;margin:12vh auto 0"},[
    el("h1",{text:"Willkommen 👋"}),
    el("p",{class:"muted",text:"Deine persönliche Lernplattform mit Karteikarten und Spaced Repetition."}),
    el("label",{for:"nameInput",text:"Wie heißt du?"}),
    input, err,
    el("div",{style:"height:16px"}),
    el("button",{class:"btn btn-primary btn-block",onclick:submit},"Los geht's")
  ]);
  setTimeout(() => input.focus(), 30);
  return box;
}

/* =====================================================================
   HOME
   ===================================================================== */
function renderCourseCard(course){
  const { studied, total, percent } = calculateCourseProgress(course);
  const open = () => navigate({view:"course",courseId:course.id,tab:"browse",browseIndex:null,browseRevealed:false,study:null});
  return el("article",{class:"card course-card",tabindex:"0",role:"button",
    "aria-label":`Kurs öffnen: ${course.title}`,
    onclick:open,
    onkeydown:e => { if(e.key === "Enter" || e.key === " "){ e.preventDefault(); open(); } }
  },[
    el("h3",{text:course.title}),
    el("p",{class:"muted small",text:course.description}),
    el("div",{class:"spread",style:"margin-bottom:8px"},[
      el("span",{class:"badge",text:`${total} Karten`}),
      el("span",{class:"small muted",text:`${percent} % Fortschritt`})
    ]),
    progressBar(percent),
    el("p",{class:"small muted",style:"margin:8px 0 16px",text:`${studied} / ${total} Karten gelernt`}),
    el("button",{class:"btn btn-primary btn-block",tabindex:"-1"},"Kurs öffnen")
  ]);
}

function renderHome(){
  const frag = document.createDocumentFragment();
  frag.appendChild(el("div",{class:"spread",style:"margin-bottom:26px"},[
    el("h1",{},[document.createTextNode(`Hallo, ${data.name} `), el("span",{text:"👋"})]),
    el("button",{class:"btn btn-ghost",onclick:() => navigate({view:"settings"})},"⚙ Einstellungen")
  ]));
  frag.appendChild(el("h2",{text:"Deine Kurse"}));
  if(COURSES.length === 0){
    frag.appendChild(el("div",{class:"card"},[renderEmptyState("📚","Noch keine Kurse verfügbar.","")]));
    return frag;
  }
  const grid = el("div",{class:"grid",style:"margin-top:14px"});
  COURSES.forEach(c => grid.appendChild(renderCourseCard(c)));
  frag.appendChild(grid);

  const today = todayCount();
  frag.appendChild(el("p",{class:"small muted",style:"margin-top:26px",
    text: today > 0 ? `Heute bereits ${today} Karte${today===1?"":"n"} gelernt. Weiter so!`
                    : "Heute noch keine Karten gelernt – starte eine Study-Session."}));
  return frag;
}

/* =====================================================================
   COURSE SHELL + TABS
   ===================================================================== */
function renderCourse(){
  const course = getCourse(state.courseId);
  if(!course){ state.view = "home"; return renderHome(); }

  const frag = document.createDocumentFragment();
  frag.appendChild(topBar());
  frag.appendChild(el("h1",{text:course.title}));
  frag.appendChild(el("p",{class:"muted",text:course.description}));

  const tabs = el("div",{class:"tabs",role:"tablist","aria-label":"Kursbereiche"});
  [["browse","Browse"],["study","Study"],["progress","Progress"]].forEach(([id,label]) => {
    tabs.appendChild(el("button",{
      class:"tab", role:"tab", "aria-selected": String(state.tab === id),
      onclick:() => navigate({tab:id, browseIndex:null, browseRevealed:false})
    }, label));
  });
  frag.appendChild(tabs);

  const panel = el("div",{role:"tabpanel"});
  if(course.questions.length === 0 && !(isAdmin() && state.tab === "browse")){
    panel.appendChild(el("div",{class:"card"},[renderEmptyState("🗂","Dieser Kurs enthält noch keine Karten.","")]));
  } else if(state.tab === "browse") panel.appendChild(renderBrowse(course));
  else if(state.tab === "study")    panel.appendChild(renderStudy(course));
  else                              panel.appendChild(renderProgress(course));
  frag.appendChild(panel);
  return frag;
}

/* =====================================================================
   BROWSE — Kartenübersicht; Karte öffnet sich als Fenster
   ===================================================================== */
function renderFlashcard(q, revealed){
  return el("div",{class:"flash"},[
    el("p",{class:"q",text:q.question}),
    revealed ? el("div",{class:"a",text:q.answer}) : null
  ]);
}

function closeBrowseWindow(){
  state.browseIndex = null;
  state.browseRevealed = false;
  document.body.style.overflow = "";
  renderApp();
}

function renderBrowseWindow(course){
  const total = course.questions.length;
  if(state.browseIndex >= total) state.browseIndex = 0;
  if(state.browseIndex < 0) state.browseIndex = total - 1;
  const q = course.questions[state.browseIndex];

  const win = el("div",{class:"window",role:"dialog","aria-modal":"true","aria-label":`Karte ${state.browseIndex+1} von ${total}`},[
    el("div",{class:"window-bar"},[
      el("span",{class:"window-dots"},[el("i"),el("i"),el("i")]),
      el("span",{class:"small muted",text:`${state.browseIndex+1} / ${total}`}),
      el("button",{class:"window-close","aria-label":"Fenster schließen",onclick:closeBrowseWindow},"✕")
    ]),
    renderFlashcard(q, state.browseRevealed),
    el("div",{style:"margin-top:16px"},[
      el("button",{class:"btn btn-primary btn-block",
        onclick:() => { state.browseRevealed = !state.browseRevealed; renderApp(); }},
        state.browseRevealed ? "Antwort verbergen" : "Antwort anzeigen")
    ]),
    el("div",{class:"spread",style:"margin-top:14px"},[
      el("button",{class:"btn","aria-label":"Vorherige Karte",
        onclick:() => { state.browseIndex--; state.browseRevealed = false; renderApp(); }},"← Zurück"),
      el("button",{class:"btn","aria-label":"Nächste Karte",
        onclick:() => { state.browseIndex++; state.browseRevealed = false; renderApp(); }},"Weiter →")
    ]),
    (() => {
      const p = getCardProgress(q.id);
      return el("p",{class:"small muted",style:"margin-top:16px;margin-bottom:0",
        text: p ? `Zuletzt bewertet: ${RATING_LABEL[p.lastRating] || "–"} · nächste Wiederholung ${formatRelativeTime(p.nextReview)} · ${p.reviewCount}× wiederholt`
                : "Diese Karte wurde noch nicht gelernt."});
    })(),
    el("p",{class:"small muted",style:"margin-top:8px;margin-bottom:0"},[
      el("span",{class:"kbd",text:"←"})," / ",el("span",{class:"kbd",text:"→"})," blättern · ",
      el("span",{class:"kbd",text:"Leertaste"})," Antwort · ",
      el("span",{class:"kbd",text:"Esc"})," schließen"
    ])
  ]);

  const overlay = el("div",{class:"overlay",onclick:e => { if(e.target === overlay) closeBrowseWindow(); }},[win]);
  document.body.style.overflow = "hidden";
  return overlay;
}

/* =====================================================================
   ADMIN — eigene Fragen hinzufügen (nur für "Jojo")
   ===================================================================== */
function addCustomQuestion(courseId, question, answer){
  const q = {
    id: "custom-" + Date.now() + "-" + Math.random().toString(36).slice(2,7),
    courseId, question, answer
  };
  data.customQuestions.push(q);
  saveData();
  applyCustomQuestions();
  return q;
}

function deleteCustomQuestion(id){
  data.customQuestions = data.customQuestions.filter(q => q.id !== id);
  delete data.progress[id];
  saveData();
  applyCustomQuestions();
}

function renderAdminPanel(course){
  const qInput = el("textarea",{rows:"2",placeholder:"Frage",style:"width:100%"});
  const aInput = el("textarea",{rows:"4",placeholder:"Antwort",style:"width:100%"});
  const note = el("p",{class:"small",style:"min-height:20px;margin:8px 0 0"});

  const submit = () => {
    const qv = qInput.value.trim(), av = aInput.value.trim();
    if(!qv || !av){
      note.style.color = "var(--error)";
      note.textContent = "Bitte Frage und Antwort ausfüllen.";
      return;
    }
    addCustomQuestion(course.id, qv, av);
    qInput.value = ""; aInput.value = "";
    renderApp();
  };

  const own = data.customQuestions.filter(q => q.courseId === course.id);
  const list = el("div",{style:"margin-top:14px"});
  own.forEach(q => {
    list.appendChild(el("div",{class:"spread",style:"gap:8px;border-top:1px solid var(--border);padding:8px 0"},[
      el("span",{class:"small",text:q.question}),
      el("button",{class:"btn btn-danger small",onclick:() => {
        if(confirm("Diese Frage wirklich löschen?")){ deleteCustomQuestion(q.id); renderApp(); }
      }},"Löschen")
    ]));
  });

  return el("div",{class:"card",style:"margin:0 0 18px"},[
    el("h3",{text:"➕ Neue Frage hinzufügen"}),
    el("p",{class:"small muted",text:`Als "${data.name}" kannst du diesem Kurs eigene Karten hinzufügen.`}),
    el("label",{text:"Frage"}), qInput,
    el("div",{style:"height:10px"}),
    el("label",{text:"Antwort"}), aInput,
    note,
    el("div",{style:"height:12px"}),
    el("button",{class:"btn btn-primary",onclick:submit},"Frage hinzufügen"),
    own.length ? el("p",{class:"small muted",style:"margin:18px 0 0",text:`Eigene Fragen (${own.length})`}) : null,
    own.length ? list : null
  ]);
}

function renderBrowse(course){
  const frag = document.createDocumentFragment();

  if(isAdmin()) frag.appendChild(renderAdminPanel(course));

  frag.appendChild(el("p",{class:"small muted",style:"margin:0 0 14px",
    text:"Klicke auf eine Karte, um sie in einem Fenster zu öffnen."}));

  const grid = el("div",{class:"browse-grid"});
  course.questions.forEach((q, i) => {
    const learned = isStudied(q.id);
    grid.appendChild(el("button",{class:"browse-item",
      "aria-label":`Karte ${i+1} öffnen: ${q.question}`,
      onclick:() => { state.browseIndex = i; state.browseRevealed = false; renderApp(); }
    },[
      el("span",{class:"num",text:`#${String(i+1).padStart(2,"0")}`}),
      el("span",{class:"qt",text:q.question}),
      el("span",{class:`badge${learned ? " badge-done" : ""}`,text: learned ? "✓ Gelernt" : "Neu"})
    ]));
  });
  frag.appendChild(grid);

  if(state.browseIndex !== null){
    frag.appendChild(renderBrowseWindow(course));
  } else {
    document.body.style.overflow = "";
  }
  return frag;
}

/* =====================================================================
   STUDY
   ===================================================================== */
function ensureSession(course){
  if(!state.study || state.study.courseId !== course.id){
    state.study = { courseId:course.id, currentCardId:null, revealed:false, cardsStudied:0, lastCardId:null, allowNotDue:false };
  }
  const s = state.study;
  if(s.currentCardId && !getQuestion(course, s.currentCardId)) s.currentCardId = null;
  if(!s.currentCardId){
    const next = pickNextCard(course, s.lastCardId, s.allowNotDue);
    s.currentCardId = next ? next.id : null;
    s.revealed = false;
  }
  return s;
}

function renderStudy(course){
  const s = ensureSession(course);
  const frag = document.createDocumentFragment();
  const due = getDueCards(course).length;
  const fresh = getNewCards(course).length;

  frag.appendChild(el("div",{class:"spread",style:"margin-bottom:18px"},[
    el("div",{},[
      el("div",{class:"small muted",text:"Heute gelernt"}),
      el("strong",{style:"font-size:1.3rem",text:`${todayCount()} Karten`})
    ]),
    el("div",{class:"row"},[
      el("span",{class:"badge",text:`Fällig: ${due}`}),
      el("span",{class:"badge",text:`Neu: ${fresh}`}),
      el("span",{class:"badge",text:`Session: ${s.cardsStudied}`})
    ])
  ]));

  if(!s.currentCardId){
    const allDone = fresh === 0 && due === 0;
    frag.appendChild(el("div",{class:"card"},[
      renderEmptyState(
        allDone ? "🎉" : "✅",
        allDone ? "Sehr gut!" : "Du hast aktuell keine fälligen Karten.",
        allDone ? "Du hast alle verfügbaren Karten für heute bearbeitet."
                : "Alle Karten wurden bereits gelernt und sind noch nicht wieder fällig.",
        el("button",{class:"btn btn-primary",style:"margin-top:8px",
          onclick:() => { s.allowNotDue = true; s.currentCardId = null; renderApp(); }},
          allDone ? "Weiterlernen" : "Trotzdem lernen")
      )
    ]));
    return frag;
  }

  const q = getQuestion(course, s.currentCardId);
  frag.appendChild(renderFlashcard(q, s.revealed));

  if(!s.revealed){
    frag.appendChild(el("div",{style:"margin-top:18px"},[
      el("button",{class:"btn btn-primary btn-block",
        onclick:() => { s.revealed = true; renderApp(); }},"Antwort anzeigen")
    ]));
    frag.appendChild(el("p",{class:"small muted center",style:"margin-top:12px"},[
      el("span",{class:"kbd",text:"Leertaste"})," zum Aufdecken"
    ]));
  } else {
    const rate = rating => {
      recordReview(course.id, q.id, rating);
      s.cardsStudied++;
      s.lastCardId = q.id;
      s.currentCardId = null;
      s.revealed = false;
      renderApp();
    };
    frag.appendChild(el("div",{class:"row",style:"margin-top:18px;gap:10px"},[
      el("button",{class:"btn rate rate-bad",onclick:() => rate("bad")},[
        document.createTextNode("Schlecht"), el("small",{text:"in 1 Min"})]),
      el("button",{class:"btn rate rate-mid",onclick:() => rate("mid")},[
        document.createTextNode("Mittel"), el("small",{text:"in 30 Min"})]),
      el("button",{class:"btn rate rate-good",onclick:() => rate("good")},[
        document.createTextNode("Gut"), el("small",{text:"in 3 Std"})])
    ]));
    frag.appendChild(el("p",{class:"small muted center",style:"margin-top:12px"},[
      el("span",{class:"kbd",text:"1"})," Schlecht · ",
      el("span",{class:"kbd",text:"2"})," Mittel · ",
      el("span",{class:"kbd",text:"3"})," Gut"
    ]));
  }
  return frag;
}

/* =====================================================================
   PROGRESS
   ===================================================================== */
function ringChart(percent){
  const r = 62, c = 2*Math.PI*r;
  const svg = document.createElementNS("http://www.w3.org/2000/svg","svg");
  svg.setAttribute("viewBox","0 0 150 150");
  svg.setAttribute("width","150"); svg.setAttribute("height","150");
  svg.setAttribute("class","ring"); svg.setAttribute("role","img");
  svg.setAttribute("aria-label",`Fortschritt ${percent} Prozent`);
  svg.innerHTML =
    `<defs><linearGradient id="g1" x1="0" y1="0" x2="1" y2="1">
       <stop offset="0%" stop-color="#6c8cff"/><stop offset="100%" stop-color="#8b7cf6"/>
     </linearGradient></defs>
     <circle cx="75" cy="75" r="${r}" fill="none" stroke="#232b35" stroke-width="12"/>
     <circle cx="75" cy="75" r="${r}" fill="none" stroke="url(#g1)" stroke-width="12"
       stroke-linecap="round" stroke-dasharray="${c}"
       stroke-dashoffset="${c - c*percent/100}" transform="rotate(-90 75 75)"/>
     <text x="75" y="82" text-anchor="middle" fill="#e6e9ee" font-size="30"
       font-family="Inter,system-ui,sans-serif" font-weight="600">${percent}%</text>`;
  return svg;
}

function renderProgressChart(){
  const days = getDailyStats(7);
  const max = Math.max(...days.map(d => d.count), 1);
  const hasData = days.some(d => d.count > 0);
  const box = el("div",{class:"card",style:"margin-top:16px"},[el("h3",{text:"Lernaktivität (7 Tage)"})]);
  if(!hasData){
    box.appendChild(renderEmptyState("📈","Noch keine Lernaktivität vorhanden.",
      "Starte eine Study-Session, um hier deinen Fortschritt zu sehen."));
    return box;
  }
  const chart = el("div",{class:"chart"});
  days.forEach(d => chart.appendChild(el("div",{class:"chart-row"},[
    el("span",{class:"small muted",text:d.label}),
    el("div",{class:"chart-bar"},[el("i",{style:`width:${Math.round(d.count/max*100)}%`})]),
    el("span",{class:"small",style:"text-align:right",text:String(d.count)})
  ])));
  box.appendChild(chart);
  return box;
}

function renderProgress(course){
  const { studied, total, percent } = calculateCourseProgress(course);
  let reviews = 0, good = 0, mid = 0, bad = 0;
  course.questions.forEach(q => {
    const p = getCardProgress(q.id); if(!p) return;
    reviews += p.reviewCount||0; good += p.correctCount||0; mid += p.mediumCount||0; bad += p.wrongCount||0;
  });

  const frag = document.createDocumentFragment();
  frag.appendChild(el("div",{class:"card center"},[
    el("h2",{text:"Lernfortschritt"}),
    ringChart(percent),
    el("p",{class:"muted",style:"margin-top:14px",text:`${studied} / ${total} Karten gelernt`}),
    el("div",{style:"max-width:420px;margin:0 auto"},[progressBar(percent)])
  ]));

  const stats = el("div",{class:"stats",style:"margin-top:16px"});
  [["Karten gesamt",total],["Gelernt",studied],["Offen",total-studied],
   ["Wiederholungen",reviews],["Gut",good],["Mittel",mid],["Schlecht",bad],
   ["Fällig jetzt",getDueCards(course).length]]
   .forEach(([label,val]) => stats.appendChild(
     el("div",{class:"stat"},[el("b",{text:String(val)}), el("span",{class:"small muted",text:label})])));
  frag.appendChild(stats);

  frag.appendChild(renderProgressChart());
  return frag;
}

/* =====================================================================
   SETTINGS
   ===================================================================== */
function renderSettings(){
  const frag = document.createDocumentFragment();
  frag.appendChild(el("div",{style:"margin-bottom:20px"},[
    el("button",{class:"btn btn-ghost",onclick:() => navigate({view:"home"})},"← Zurück")
  ]));
  frag.appendChild(el("h1",{text:"Einstellungen"}));

  const input = el("input",{type:"text",id:"settingsName",value:data.name || "",maxlength:"40"});
  const note = el("p",{class:"small",style:"min-height:20px;margin:8px 0 0"});
  const save = () => {
    const v = input.value.trim();
    if(!v){ note.style.color = "var(--error)"; note.textContent = "Der Name darf nicht leer sein."; return; }
    data.name = v; saveData();
    note.style.color = "var(--success)"; note.textContent = "Name gespeichert.";
  };
  input.addEventListener("keydown", e => { if(e.key === "Enter") save(); });

  frag.appendChild(el("div",{class:"card",style:"margin-top:10px"},[
    el("h3",{text:"Profil"}),
    el("label",{for:"settingsName",text:"Dein Name"}),
    input, note,
    el("div",{style:"height:14px"}),
    el("button",{class:"btn btn-primary",onclick:save},"Speichern")
  ]));

  const resetNote = el("p",{class:"small",style:"min-height:20px;margin:8px 0 0;color:var(--success)"});
  frag.appendChild(el("div",{class:"card",style:"margin-top:16px"},[
    el("h3",{text:"Lernfortschritt zurücksetzen"}),
    el("p",{class:"small muted",text:"Löscht alle Kartenbewertungen und Statistiken. Kurse und Fragen bleiben erhalten."}),
    el("button",{class:"btn btn-danger",onclick:() => {
      const ok = confirm("Möchtest du wirklich deinen gesamten Lernfortschritt löschen?\n\nDiese Aktion kann nicht rückgängig gemacht werden.");
      if(!ok) return;
      data.progress = {}; data.dailyStats = {}; state.study = null;
      saveData();
      renderApp();
      alert("Dein Lernfortschritt wurde zurückgesetzt.");
    }},"Fortschritt löschen"),
    resetNote
  ]));
  return frag;
}

/* =====================================================================
   KEYBOARD SHORTCUTS
   ===================================================================== */
document.addEventListener("keydown", e => {
  const t = e.target;
  if(t && (t.tagName === "INPUT" || t.tagName === "TEXTAREA" || t.isContentEditable)) return;
  if(state.view !== "course") return;
  const course = getCourse(state.courseId);
  if(!course || course.questions.length === 0) return;

  if(state.tab === "browse"){
    if(state.browseIndex === null) return;   // Fenster geschlossen → keine Shortcuts
    if(e.key === "Escape"){ e.preventDefault(); closeBrowseWindow(); }
    else if(e.key === "ArrowLeft"){ e.preventDefault(); state.browseIndex--; state.browseRevealed = false; renderApp(); }
    else if(e.key === "ArrowRight"){ e.preventDefault(); state.browseIndex++; state.browseRevealed = false; renderApp(); }
    else if(e.code === "Space"){ e.preventDefault(); state.browseRevealed = !state.browseRevealed; renderApp(); }
  } else if(state.tab === "study"){
    const s = state.study;
    if(!s || !s.currentCardId) return;
    if(e.code === "Space" && !s.revealed){ e.preventDefault(); s.revealed = true; renderApp(); return; }
    if(!s.revealed) return;               // Ratings erst nach Aufdecken
    const map = { "1":"bad", "2":"mid", "3":"good" };
    if(map[e.key]){
      e.preventDefault();
      recordReview(course.id, s.currentCardId, map[e.key]);
      s.cardsStudied++; s.lastCardId = s.currentCardId; s.currentCardId = null; s.revealed = false;
      renderApp();
    }
  }
});

/* =====================================================================
   APP RENDER
   ===================================================================== */
function renderApp(){
  const root = $("#app");
  root.textContent = "";
  try{
    if(!data.name) root.appendChild(renderOnboarding());
    else if(state.view === "settings") root.appendChild(renderSettings());
    else if(state.view === "course")   root.appendChild(renderCourse());
    else                               root.appendChild(renderHome());
  }catch(err){
    console.error(err);
    root.appendChild(el("div",{class:"card"},[
      renderEmptyState("⚠️","Etwas ist schiefgelaufen.","Die Ansicht konnte nicht geladen werden.",
        el("button",{class:"btn btn-primary",style:"margin-top:8px",
          onclick:() => navigate({view:"home"})},"Zur Startseite"))
    ]));
  }
}

/* ============================= INIT ============================= */
data = loadData();
applyCustomQuestions();
if(data.name) applyHash();
renderApp();
</script>
</body>
</html>
