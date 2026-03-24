<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>PriceRadar</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700&family=DM+Sans:wght@300;400;500;700&display=swap" rel="stylesheet"/>
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:       #f7f6f2;
  --surface:  #ffffff;
  --surface2: #f0efe9;
  --border:   #e2e0d8;
  --border2:  #d0cec4;
  --ink:      #1a1917;
  --ink2:     #5a5750;
  --ink3:     #9a9590;
  --teal:     #0d7a5f;
  --teal-bg:  #e6f5f0;
  --teal-mid: #1a9974;
  --amber:    #92600a;
  --amber-bg: #fef3e0;
  --blue:     #1a4d8f;
  --blue-bg:  #e8f0fb;
  --red:      #b53030;
  --red-bg:   #fdeaea;
  --mono:     'JetBrains Mono', monospace;
  --sans:     'DM Sans', sans-serif;
}

body {
  background: var(--bg);
  color: var(--ink);
  font-family: var(--sans);
  font-size: 15px;
  line-height: 1.65;
  max-width: 860px;
  margin: 0 auto;
  padding: 0 28px 80px;
}

/* ── HERO ── */
.hero {
  padding: 64px 0 56px;
  border-bottom: 1px solid var(--border);
  margin-bottom: 60px;
}

.hero-label {
  font-family: var(--mono);
  font-size: 11px;
  font-weight: 500;
  color: var(--teal);
  letter-spacing: 0.14em;
  text-transform: uppercase;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.hero-label::before {
  content: '';
  display: inline-block;
  width: 20px;
  height: 2px;
  background: var(--teal);
}

h1 {
  font-size: clamp(44px, 8vw, 72px);
  font-weight: 700;
  letter-spacing: -0.04em;
  line-height: 1;
  color: var(--ink);
  margin-bottom: 6px;
}
h1 em {
  font-style: normal;
  color: var(--teal);
}

.hero-sub {
  font-size: 19px;
  font-weight: 300;
  color: var(--ink2);
  margin: 18px 0 32px;
  max-width: 520px;
  line-height: 1.5;
}

.pill-row {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 44px;
}
.pill {
  font-family: var(--mono);
  font-size: 11px;
  font-weight: 500;
  padding: 4px 11px;
  border-radius: 100px;
  letter-spacing: 0.04em;
}
.pill-teal   { background: var(--teal-bg);  color: var(--teal);  border: 1px solid #b5dfd4; }
.pill-amber  { background: var(--amber-bg); color: var(--amber); border: 1px solid #f0d48a; }
.pill-blue   { background: var(--blue-bg);  color: var(--blue);  border: 1px solid #bed2f0; }
.pill-red    { background: var(--red-bg);   color: var(--red);   border: 1px solid #f0c0c0; }
.pill-gray   { background: var(--surface2); color: var(--ink2);  border: 1px solid var(--border); }

.metrics {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  border: 1px solid var(--border);
  border-radius: 12px;
  overflow: hidden;
  background: var(--surface);
}
.metric {
  padding: 22px 20px;
  border-right: 1px solid var(--border);
}
.metric:last-child { border-right: none; }
.metric-n {
  font-size: 32px;
  font-weight: 700;
  letter-spacing: -0.04em;
  color: var(--ink);
  line-height: 1;
  margin-bottom: 4px;
  font-family: var(--mono);
}
.metric-l {
  font-size: 12px;
  color: var(--ink3);
  font-weight: 400;
}

/* ── SECTION HEADERS ── */
section { margin-bottom: 60px; }

h2 {
  font-size: 11px;
  font-weight: 700;
  font-family: var(--mono);
  text-transform: uppercase;
  letter-spacing: 0.14em;
  color: var(--ink3);
  margin-bottom: 24px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border);
}

h3 {
  font-size: 17px;
  font-weight: 700;
  color: var(--ink);
  margin-bottom: 8px;
  letter-spacing: -0.01em;
}

p { color: var(--ink2); margin-bottom: 14px; font-size: 14px; }

/* ── ARCHITECTURE ── */
.arch-wrap {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 32px;
  font-family: var(--mono);
}

.arch-row {
  display: flex;
  align-items: stretch;
  gap: 0;
  margin-bottom: 20px;
}
.arch-node {
  flex: 1;
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 12px 14px;
  background: var(--surface2);
  position: relative;
}
.arch-node-name {
  font-size: 12px;
  font-weight: 700;
  color: var(--ink);
  margin-bottom: 2px;
}
.arch-node-sub {
  font-size: 10px;
  color: var(--ink3);
  font-weight: 400;
}
.arch-node.t { border-color: #b5dfd4; background: var(--teal-bg); }
.arch-node.t .arch-node-name { color: var(--teal); }
.arch-node.a { border-color: #f0d48a; background: var(--amber-bg); }
.arch-node.a .arch-node-name { color: var(--amber); }
.arch-node.b { border-color: #bed2f0; background: var(--blue-bg); }
.arch-node.b .arch-node-name { color: var(--blue); }
.arch-node.r { border-color: #f0c0c0; background: var(--red-bg); }
.arch-node.r .arch-node-name { color: var(--red); }

.arch-arrow {
  display: flex;
  align-items: center;
  padding: 0 8px;
  color: var(--ink3);
  font-size: 14px;
  flex-shrink: 0;
}

.arch-footer {
  font-size: 11px;
  color: var(--ink3);
  border-top: 1px solid var(--border);
  padding-top: 16px;
  font-weight: 400;
  line-height: 1.6;
}
.arch-footer strong { color: var(--ink2); font-weight: 600; }

/* ── TECH STACK ── */
.stack-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}
.stack-card {
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 18px 16px;
  background: var(--surface);
  transition: border-color 0.15s;
}
.stack-card:hover { border-color: var(--border2); }
.stack-name { font-weight: 700; font-size: 13px; color: var(--ink); margin-bottom: 3px; font-family: var(--mono); }
.stack-role { font-size: 11px; color: var(--ink3); }

/* ── WHAT IT DOES ── */
.features { display: flex; flex-direction: column; gap: 1px; }
.feature {
  display: grid;
  grid-template-columns: 36px 1fr;
  gap: 16px;
  align-items: start;
  padding: 16px 0;
  border-bottom: 1px solid var(--border);
}
.feature:last-child { border-bottom: none; }
.feat-num {
  font-family: var(--mono);
  font-size: 11px;
  font-weight: 700;
  color: var(--ink3);
  padding-top: 2px;
}
.feat-title { font-weight: 600; font-size: 14px; color: var(--ink); margin-bottom: 3px; }
.feat-desc { font-size: 13px; color: var(--ink2); line-height: 1.55; }

/* ── DB SCHEMA ── */
.schema-wrap {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}
.table-card {
  border: 1px solid var(--border);
  border-radius: 10px;
  overflow: hidden;
  font-family: var(--mono);
  background: var(--surface);
}
.table-head {
  padding: 10px 14px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  font-size: 12px;
  font-weight: 700;
  color: var(--ink);
}
.table-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 7px 14px;
  border-bottom: 1px solid var(--border);
  font-size: 11px;
}
.table-row:last-child { border-bottom: none; }
.col-name { color: var(--ink2); }
.col-type { color: var(--ink3); }
.tag {
  font-size: 9px;
  font-weight: 700;
  padding: 2px 5px;
  border-radius: 3px;
  margin-left: 4px;
  letter-spacing: 0.05em;
}
.tag-pk { background: var(--amber-bg); color: var(--amber); }
.tag-fk { background: var(--blue-bg);  color: var(--blue); }

/* ── API ENDPOINTS ── */
.endpoints { display: flex; flex-direction: column; gap: 8px; }
.endpoint {
  display: flex;
  align-items: baseline;
  gap: 12px;
  padding: 12px 16px;
  border: 1px solid var(--border);
  border-radius: 8px;
  background: var(--surface);
  font-family: var(--mono);
  font-size: 13px;
}
.method {
  font-size: 10px;
  font-weight: 700;
  padding: 3px 7px;
  border-radius: 4px;
  min-width: 46px;
  text-align: center;
  flex-shrink: 0;
  letter-spacing: 0.05em;
}
.method-get    { background: var(--teal-bg);  color: var(--teal); }
.method-post   { background: var(--amber-bg); color: var(--amber); }
.method-delete { background: var(--red-bg);   color: var(--red); }
.ep-path { color: var(--ink); font-weight: 500; }
.ep-desc { color: var(--ink3); font-size: 11px; margin-left: auto; }

/* ── CODE BLOCK ── */
.code-wrap {
  background: #1c1e22;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 16px;
}
.code-bar {
  background: #25282e;
  padding: 9px 18px;
  font-family: var(--mono);
  font-size: 11px;
  color: #666d7a;
  display: flex;
  gap: 7px;
  align-items: center;
}
.dot { width: 10px; height: 10px; border-radius: 50%; }
.dot-r { background: #ff5f57; }
.dot-y { background: #febc2e; }
.dot-g { background: #28c840; }
.code-bar span { margin-left: 4px; }
pre {
  padding: 20px 22px;
  font-family: var(--mono);
  font-size: 12.5px;
  line-height: 1.75;
  color: #c9d1d9;
  overflow-x: auto;
  white-space: pre;
}
.kw  { color: #ff7b72; }
.st  { color: #a5d6ff; }
.cm  { color: #484f58; }
.fn  { color: #d2a8ff; }
.nm  { color: #ffa657; }
.gr  { color: #7ee787; }

/* ── DESIGN DECISIONS ── */
.decisions { display: flex; flex-direction: column; }
.decision {
  padding: 22px 0;
  border-bottom: 1px solid var(--border);
  display: grid;
  grid-template-columns: 200px 1fr;
  gap: 32px;
}
.decision:last-child { border-bottom: none; }
.dec-q { font-size: 13px; font-weight: 600; color: var(--ink); line-height: 1.4; }
.dec-a { font-size: 13px; color: var(--ink2); line-height: 1.6; }
code { font-family: var(--mono); font-size: 12px; background: var(--surface2); padding: 1px 5px; border-radius: 3px; color: var(--ink2); border: 1px solid var(--border); }

/* ── QUICKSTART ── */
.qs-steps { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 20px; }
.qs-step {
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 18px;
  background: var(--surface);
}
.qs-num { font-family: var(--mono); font-size: 11px; color: var(--teal); font-weight: 700; margin-bottom: 6px; }
.qs-title { font-size: 13px; font-weight: 600; color: var(--ink); margin-bottom: 4px; }
.qs-body { font-size: 12px; color: var(--ink3); line-height: 1.5; }

/* ── RESPONSIVE ── */
@media (max-width: 640px) {
  h1 { font-size: 40px; }
  .metrics { grid-template-columns: repeat(2, 1fr); }
  .stack-grid { grid-template-columns: repeat(2, 1fr); }
  .schema-wrap { grid-template-columns: 1fr; }
  .decision { grid-template-columns: 1fr; gap: 8px; }
  .qs-steps { grid-template-columns: 1fr; }
  .arch-row { flex-wrap: wrap; }
  .ep-desc { display: none; }
  body { padding: 0 16px 60px; }
}
</style>
</head>
<body>

<!-- HERO -->
<div class="hero">
  <div class="hero-label">Data Engineering · Python · FastAPI</div>
  <h1>Price<em>Radar</em></h1>
  <p class="hero-sub">
    A fully-automated pipeline that tracks product prices across e-commerce platforms, detects anomalies, forecasts trends, and fires real-time alerts.
  </p>
  <div class="pill-row">
    <span class="pill pill-teal">Python 3.11</span>
    <span class="pill pill-gray">Scrapy</span>
    <span class="pill pill-blue">PostgreSQL</span>
    <span class="pill pill-amber">Apache Airflow</span>
    <span class="pill pill-teal">FastAPI</span>
    <span class="pill pill-gray">Prophet</span>
    <span class="pill pill-blue">Docker Compose</span>
    <span class="pill pill-gray">Pandas</span>
  </div>
  <div class="metrics">
    <div class="metric"><div class="metric-n">2</div><div class="metric-l">platforms tracked</div></div>
    <div class="metric"><div class="metric-n">7d</div><div class="metric-l">price forecast</div></div>
    <div class="metric"><div class="metric-n">9</div><div class="metric-l">API endpoints</div></div>
    <div class="metric"><div class="metric-n">1 cmd</div><div class="metric-l">full stack deploy</div></div>
  </div>
</div>

<!-- ARCHITECTURE -->
<section>
  <h2>Architecture</h2>
  <div class="arch-wrap">
    <div class="arch-row">
      <div class="arch-node t">
        <div class="arch-node-name">Scrapy spiders</div>
        <div class="arch-node-sub">Amazon · Flipkart</div>
      </div>
      <div class="arch-arrow">→</div>
      <div class="arch-node">
        <div class="arch-node-name">Staging table</div>
        <div class="arch-node-sub">raw JSON + HTML</div>
      </div>
      <div class="arch-arrow">→</div>
      <div class="arch-node a">
        <div class="arch-node-name">ETL pipeline</div>
        <div class="arch-node-sub">Pandas · dedup</div>
      </div>
      <div class="arch-arrow">→</div>
      <div class="arch-node b">
        <div class="arch-node-name">PostgreSQL</div>
        <div class="arch-node-sub">price_history</div>
      </div>
      <div class="arch-arrow">→</div>
      <div class="arch-node">
        <div class="arch-node-name">Analytics</div>
        <div class="arch-node-sub">MA · Z-score · Prophet</div>
      </div>
      <div class="arch-arrow">→</div>
      <div class="arch-node t">
        <div class="arch-node-name">FastAPI</div>
        <div class="arch-node-sub">REST + alerts</div>
      </div>
    </div>
    <div class="arch-footer">
      <strong>Orchestration:</strong> all stages run as Airflow DAGs on a configurable schedule —
      scrape → clean → forecast → alert check, each independently retryable. &nbsp;
      <strong>Deployment:</strong> single <code>docker-compose up</code> starts Airflow, PostgreSQL, and the API.
    </div>
  </div>
</section>

<!-- WHAT IT DOES -->
<section>
  <h2>What it does</h2>
  <div class="features">
    <div class="feature">
      <div class="feat-num">01</div>
      <div>
        <div class="feat-title">Scheduled web scraping</div>
        <div class="feat-desc">Scrapy spiders crawl Amazon and Flipkart product pages on a cron schedule. Rotating user-agents, randomized delays, and configurable retry logic make the scrapers resilient. Raw HTML is stored alongside parsed JSON so selectors can be re-run without re-fetching.</div>
      </div>
    </div>
    <div class="feature">
      <div class="feat-num">02</div>
      <div>
        <div class="feat-title">Cross-platform deduplication</div>
        <div class="feat-desc">The ETL layer normalises product names and matches the same item across platforms using fuzzy matching. Price history is stored once per logical product, not once per URL — keeping the dataset clean for analytics.</div>
      </div>
    </div>
    <div class="feature">
      <div class="feat-num">03</div>
      <div>
        <div class="feat-title">Anomaly detection on price drops</div>
        <div class="feat-desc">A Z-score model flags sudden deviations from a product's rolling baseline. 7-day and 30-day moving averages are stored per product, and a drop percentage is computed relative to the historical median — not just the previous day.</div>
      </div>
    </div>
    <div class="feature">
      <div class="feat-num">04</div>
      <div>
        <div class="feat-title">7-day price forecast via Prophet</div>
        <div class="feat-desc">A Facebook Prophet model is trained per product on its full price history. Predictions and 95% confidence intervals are stored in the <code>forecasts</code> table and exposed through the API. Models retrain automatically on each Airflow run.</div>
      </div>
    </div>
    <div class="feature">
      <div class="feat-num">05</div>
      <div>
        <div class="feat-title">Price alert system</div>
        <div class="feat-desc">Users register a target price per product via the API. An Airflow-scheduled alert job checks current prices on each scrape cycle and fires an email via SMTP when a threshold is crossed. Alerts self-deactivate after triggering.</div>
      </div>
    </div>
  </div>
</section>

<!-- TECH STACK -->
<section>
  <h2>Tech stack</h2>
  <div class="stack-grid">
    <div class="stack-card"><div class="stack-name">Scrapy</div><div class="stack-role">Web scraping framework</div></div>
    <div class="stack-card"><div class="stack-name">PostgreSQL</div><div class="stack-role">Relational time-series store</div></div>
    <div class="stack-card"><div class="stack-name">Airflow</div><div class="stack-role">Pipeline orchestration</div></div>
    <div class="stack-card"><div class="stack-name">FastAPI</div><div class="stack-role">REST API layer</div></div>
    <div class="stack-card"><div class="stack-name">Pandas</div><div class="stack-role">Data cleaning & ETL</div></div>
    <div class="stack-card"><div class="stack-name">Prophet</div><div class="stack-role">Time-series forecasting</div></div>
    <div class="stack-card"><div class="stack-name">Docker Compose</div><div class="stack-role">Local deployment</div></div>
    <div class="stack-card"><div class="stack-name">Alembic</div><div class="stack-role">DB migrations</div></div>
  </div>
</section>

<!-- DB SCHEMA -->
<section>
  <h2>Database schema</h2>
  <div class="schema-wrap">
    <div class="table-card">
      <div class="table-head">products</div>
      <div class="table-row"><span class="col-name">id <span class="tag tag-pk">PK</span></span><span class="col-type">uuid</span></div>
      <div class="table-row"><span class="col-name">name</span><span class="col-type">text</span></div>
      <div class="table-row"><span class="col-name">url</span><span class="col-type">text</span></div>
      <div class="table-row"><span class="col-name">platform</span><span class="col-type">varchar(20)</span></div>
      <div class="table-row"><span class="col-name">created_at</span><span class="col-type">timestamptz</span></div>
    </div>
    <div class="table-card">
      <div class="table-head">price_history</div>
      <div class="table-row"><span class="col-name">id <span class="tag tag-pk">PK</span></span><span class="col-type">bigserial</span></div>
      <div class="table-row"><span class="col-name">product_id <span class="tag tag-fk">FK</span></span><span class="col-type">uuid</span></div>
      <div class="table-row"><span class="col-name">price</span><span class="col-type">numeric(10,2)</span></div>
      <div class="table-row"><span class="col-name">availability</span><span class="col-type">boolean</span></div>
      <div class="table-row"><span class="col-name">scraped_at</span><span class="col-type">timestamptz</span></div>
    </div>
    <div class="table-card">
      <div class="table-head">alerts</div>
      <div class="table-row"><span class="col-name">id <span class="tag tag-pk">PK</span></span><span class="col-type">uuid</span></div>
      <div class="table-row"><span class="col-name">product_id <span class="tag tag-fk">FK</span></span><span class="col-type">uuid</span></div>
      <div class="table-row"><span class="col-name">user_email</span><span class="col-type">text</span></div>
      <div class="table-row"><span class="col-name">target_price</span><span class="col-type">numeric(10,2)</span></div>
      <div class="table-row"><span class="col-name">triggered_at</span><span class="col-type">timestamptz</span></div>
    </div>
    <div class="table-card">
      <div class="table-head">forecasts</div>
      <div class="table-row"><span class="col-name">id <span class="tag tag-pk">PK</span></span><span class="col-type">bigserial</span></div>
      <div class="table-row"><span class="col-name">product_id <span class="tag tag-fk">FK</span></span><span class="col-type">uuid</span></div>
      <div class="table-row"><span class="col-name">forecast_date</span><span class="col-type">date</span></div>
      <div class="table-row"><span class="col-name">predicted_price</span><span class="col-type">numeric(10,2)</span></div>
      <div class="table-row"><span class="col-name">conf_low / conf_high</span><span class="col-type">numeric(10,2)</span></div>
    </div>
  </div>
</section>

<!-- API ENDPOINTS -->
<section>
  <h2>API endpoints</h2>
  <div class="endpoints">
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/products</span><span class="ep-desc">search with ?q=&platform=</span></div>
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/products/{id}/history</span><span class="ep-desc">full price timeline</span></div>
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/products/{id}/forecast</span><span class="ep-desc">7-day Prophet prediction + CI</span></div>
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/products/{id}/analytics</span><span class="ep-desc">moving avg, Z-score, drop %</span></div>
    <div class="endpoint"><span class="method method-post">POST</span><span class="ep-path">/alerts</span><span class="ep-desc">register a price threshold alert</span></div>
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/alerts</span><span class="ep-desc">list active alerts</span></div>
    <div class="endpoint"><span class="method method-delete">DEL</span><span class="ep-path">/alerts/{id}</span><span class="ep-desc">remove an alert</span></div>
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/deals/today</span><span class="ep-desc">biggest drops vs 30d baseline</span></div>
    <div class="endpoint"><span class="method method-get">GET</span><span class="ep-path">/dashboard</span><span class="ep-desc">aggregate stats across all products</span></div>
  </div>
  <p style="margin-top:14px;font-size:12px;">Auto-generated Swagger docs available at <code>/docs</code> when running locally.</p>
</section>

<!-- QUICKSTART -->
<section>
  <h2>Quickstart</h2>
  <div class="qs-steps">
    <div class="qs-step">
      <div class="qs-num">Step 01</div>
      <div class="qs-title">Clone & configure</div>
      <div class="qs-body">Copy <code>.env.example</code> → <code>.env</code> and add SMTP credentials for alerts.</div>
    </div>
    <div class="qs-step">
      <div class="qs-num">Step 02</div>
      <div class="qs-title">Start the stack</div>
      <div class="qs-body">Run <code>docker-compose up -d</code>. Airflow, PostgreSQL and the API start together.</div>
    </div>
    <div class="qs-step">
      <div class="qs-num">Step 03</div>
      <div class="qs-title">Add a product</div>
      <div class="qs-body">POST a product URL to <code>/products</code>. The next Airflow run picks it up automatically.</div>
    </div>
  </div>
  <div class="code-wrap">
    <div class="code-bar"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div><span>bash</span></div>
    <pre>
<span class="cm"># 1. Start everything</span>
<span class="fn">git</span> clone https://github.com/you/priceradar && <span class="fn">cd</span> priceradar
<span class="fn">cp</span> .env.example .env
<span class="fn">docker-compose</span> up -d

<span class="cm"># 2. Add a product to track</span>
<span class="fn">curl</span> -X POST http://localhost:8000/products \
  -H <span class="st">"Content-Type: application/json"</span> \
  -d <span class="st">'{"url": "https://amazon.in/dp/B09XYZ", "platform": "amazon"}'</span>

<span class="cm"># 3. Set an alert</span>
<span class="fn">curl</span> -X POST http://localhost:8000/alerts \
  -d <span class="st">'{"product_id": "...", "target_price": 999, "email": "you@gmail.com"}'</span>

<span class="cm"># Services</span>
<span class="cm"># API docs  →  http://localhost:8000/docs</span>
<span class="cm"># Airflow   →  http://localhost:8080  (admin / admin)</span>
    </pre>
  </div>
</section>

<!-- DESIGN DECISIONS -->
<section>
  <h2>Design decisions</h2>
  <div class="decisions">
    <div class="decision">
      <div class="dec-q">Why store raw HTML alongside parsed data?</div>
      <div class="dec-a">When a site updates its DOM, selectors break but historical data doesn't need to be lost. Raw HTML in the staging table means you can re-parse the past without re-scraping. Selector configs live in a single JSON file, not inside the spiders.</div>
    </div>
    <div class="decision">
      <div class="dec-q">Why Prophet over ARIMA for forecasting?</div>
      <div class="dec-a">E-commerce prices spike around sale events (Diwali, Big Billion Days, Prime Day). Prophet handles these as regressors and manages missing data automatically — both are problems ARIMA requires manual handling for. It also produces confidence intervals out of the box.</div>
    </div>
    <div class="decision">
      <div class="dec-q">Why Z-score for anomaly detection instead of a threshold?</div>
      <div class="dec-a">Fixed thresholds don't generalise — a ₹200 drop means nothing on a ₹50,000 laptop but everything on a ₹300 cable. Z-score normalises by each product's own variance, so the anomaly signal is comparable across price ranges.</div>
    </div>
    <div class="decision">
      <div class="dec-q">Why Airflow for a single-machine setup?</div>
      <div class="dec-a">The DAG abstraction forces each stage (scrape → clean → forecast → alert) to be independently retryable and observable. Swapping to Celery workers or migrating to a managed service later requires no changes to pipeline logic.</div>
    </div>
  </div>
</section>

<!-- REPO STRUCTURE -->
<section>
  <h2>Repo structure</h2>
  <div class="code-wrap">
    <div class="code-bar"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div><span>tree</span></div>
    <pre>
priceradar/
<span class="gr">├── scrapers/</span>
<span class="cm">│   ├── spiders/amazon.py       # Amazon product spider</span>
<span class="cm">│   ├── spiders/flipkart.py     # Flipkart product spider</span>
<span class="cm">│   ├── middlewares.py          # UA rotation, retry logic</span>
<span class="cm">│   └── selectors.json          # CSS selectors (centralised)</span>
<span class="gr">├── pipeline/</span>
<span class="cm">│   ├── etl.py                  # Pandas cleaning & normalisation</span>
<span class="cm">│   ├── dedup.py                # Cross-platform product matching</span>
<span class="cm">│   └── analytics.py           # Moving avg, Z-score, drop %</span>
<span class="gr">├── ml/</span>
<span class="cm">│   └── forecast.py            # Prophet training & prediction</span>
<span class="gr">├── api/</span>
<span class="cm">│   ├── main.py                 # FastAPI application</span>
<span class="cm">│   ├── routers/               # products, alerts, deals</span>
<span class="cm">│   └── schemas.py             # Pydantic models</span>
<span class="gr">├── airflow/dags/</span>
<span class="cm">│   ├── scrape_dag.py</span>
<span class="cm">│   ├── etl_dag.py</span>
<span class="cm">│   └── forecast_dag.py</span>
<span class="gr">├── db/migrations/</span>             <span class="cm"># Alembic migration scripts</span>
<span class="gr">├── docker-compose.yml</span>
<span class="gr">└── .env.example</span>
    </pre>
  </div>
</section>

</body>
</html>
