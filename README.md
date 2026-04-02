<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LoanFlow — Repayment Portal</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --cyan:#0369a1;
  --lime:#15803d;
  --pink:#be185d;
  --orange:#c2410c;
  --purple:#6d28d9;
  --yellow:#b45309;
  --bg:#eef2ff;
  --surface:#ffffff;
  --border:#c7d2fe;
  --text:#1e1b4b;
  --muted:#4338ca;
}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{font-family:'Outfit',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;font-size:15px}

/* colourful bg blobs */
body::before{content:'';position:fixed;top:-15%;left:-5%;width:55vw;height:55vw;
  background:radial-gradient(circle,rgba(99,102,241,0.22) 0%,rgba(59,130,246,0.12) 45%,transparent 70%);
  pointer-events:none;animation:drift 20s ease-in-out infinite alternate;z-index:0}
body::after{content:'';position:fixed;bottom:-15%;right:-5%;width:50vw;height:50vw;
  background:radial-gradient(circle,rgba(236,72,153,0.18) 0%,rgba(245,158,11,0.1) 50%,transparent 70%);
  pointer-events:none;animation:drift 24s ease-in-out infinite alternate-reverse;z-index:0}
.orb3{position:fixed;top:35%;left:55%;width:35vw;height:35vw;
  background:radial-gradient(circle,rgba(16,185,129,0.12) 0%,transparent 65%);
  pointer-events:none;animation:drift 28s ease-in-out infinite alternate;z-index:0}

@keyframes drift{0%{transform:translate(0,0) scale(1)}100%{transform:translate(4%,5%) scale(1.07)}}
@keyframes fadeUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.3;transform:scale(.6)}}
@keyframes shimmer{0%{background-position:0% 50%}100%{background-position:200% 50%}}

.wrap{max-width:980px;margin:0 auto;padding:2rem 1.5rem;position:relative;z-index:1}

/* HERO */
.hero{margin-bottom:2rem;animation:fadeUp .7s ease both;display:flex;flex-direction:column;align-items:center;gap:1rem}
.badge{display:inline-flex;align-items:center;gap:8px;
  background:linear-gradient(135deg,#6d28d9,#0369a1,#15803d);
  background-size:200% 200%;animation:shimmer 4s linear infinite;
  color:#fff;font-size:17px;font-weight:700;letter-spacing:1px;
  text-transform:uppercase;padding:10px 28px;border-radius:100px;
  box-shadow:0 4px 20px rgba(109,40,217,0.35)}
.badge-dot{width:8px;height:8px;background:#fff;border-radius:50%;opacity:.85;animation:pulse 2s infinite}
h1{font-family:'Syne',sans-serif;font-size:clamp(32px,5.5vw,52px);font-weight:800;line-height:1.05;
  background:linear-gradient(120deg,#6d28d9,#0369a1,#0891b2,#15803d);
  background-size:300% 300%;-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
  animation:shimmer 6s linear infinite;text-align:center}
.hero p{font-size:16px;color:#3730a3;font-weight:400;text-align:center;max-width:500px}

/* TABS */
.tabs{display:flex;gap:6px;margin-bottom:2rem;background:#fff;border:2px solid #c7d2fe;border-radius:14px;padding:5px;animation:fadeUp .7s .1s ease both;box-shadow:0 2px 12px rgba(99,102,241,0.1)}
.tab{flex:1;padding:11px 14px;font-size:14px;font-weight:600;color:#6366f1;cursor:pointer;border:none;background:none;border-radius:10px;transition:all .2s;font-family:'Outfit',sans-serif}
.tab.active{background:linear-gradient(135deg,#6d28d9,#0369a1);color:#fff;box-shadow:0 3px 12px rgba(99,102,241,0.4)}
.tab:hover:not(.active){background:#eef2ff;color:#4338ca}
.panel{display:none}.panel.active{display:block}

/* CARDS */
.card{background:#fff;border:2px solid #e0e7ff;border-radius:20px;padding:1.5rem;transition:border-color .2s,box-shadow .2s;box-shadow:0 2px 16px rgba(99,102,241,0.08)}
.card:hover{border-color:#a5b4fc;box-shadow:0 6px 28px rgba(99,102,241,0.15)}

/* GRID */
.grid-2{display:grid;grid-template-columns:1fr 1fr;gap:1.25rem}

/* INPUTS */
.input-group{margin-bottom:1.1rem}
.input-group label{display:block;font-size:12px;font-weight:700;color:#4338ca;margin-bottom:7px;text-transform:uppercase;letter-spacing:.8px}
.input-row{display:flex;align-items:center;background:#fff;border:2px solid #c7d2fe;border-radius:10px;overflow:hidden;transition:border-color .2s,box-shadow .2s}
.input-row:focus-within{border-color:#6d28d9;box-shadow:0 0 0 3px rgba(109,40,217,0.15)}
.prefix,.suffix{padding:0 13px;font-size:14px;font-weight:700;color:#fff;background:linear-gradient(135deg,#6d28d9,#0369a1);height:44px;display:flex;align-items:center;flex-shrink:0}
.input-row input{border:none;outline:none;padding:0 13px;font-size:16px;height:44px;flex:1;background:transparent;color:#1e1b4b;font-family:'Outfit',sans-serif;font-weight:600;min-width:0}
input[type=number]::-webkit-inner-spin-button{opacity:.3}

/* BUTTON */
.btn{width:100%;padding:14px;background:linear-gradient(135deg,#6d28d9,#0369a1,#15803d);color:#fff;border:none;border-radius:12px;font-size:15px;font-weight:700;cursor:pointer;font-family:'Syne',sans-serif;letter-spacing:.5px;transition:opacity .15s,transform .1s,box-shadow .2s;margin-top:6px;box-shadow:0 4px 18px rgba(109,40,217,0.3)}
.btn:hover{opacity:.92;transform:translateY(-2px);box-shadow:0 8px 28px rgba(109,40,217,0.4)}.btn:active{transform:translateY(0)}
.btn-sm{width:auto;padding:10px 20px;margin:0;font-size:14px;border-radius:10px}

/* METRIC CARDS — each with its own vivid colour */
.metrics{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin:1.1rem 0}
.metric{border-radius:16px;padding:16px 18px;transition:transform .2s,box-shadow .2s;position:relative;overflow:hidden}
.metric:hover{transform:translateY(-3px)}
.metric:nth-child(1){background:linear-gradient(135deg,#6d28d9,#7c3aed);box-shadow:0 4px 20px rgba(109,40,217,0.3)}
.metric:nth-child(2){background:linear-gradient(135deg,#0369a1,#0891b2);box-shadow:0 4px 20px rgba(3,105,161,0.3)}
.metric:nth-child(3){background:linear-gradient(135deg,#be185d,#db2777);box-shadow:0 4px 20px rgba(190,24,93,0.3)}
.metric:nth-child(4){background:linear-gradient(135deg,#c2410c,#ea580c);box-shadow:0 4px 20px rgba(194,65,12,0.3)}
.metric-label{font-size:11px;font-weight:700;color:rgba(255,255,255,0.8);text-transform:uppercase;letter-spacing:.8px;margin-bottom:8px}
.metric-value{font-size:21px;font-weight:800;font-family:'Syne',sans-serif;color:#fff}
.metric-value.cyan,.metric-value.lime,.metric-value.pink,.metric-value.orange{color:#fff}

/* BREAKDOWN */
.breakdown-row{display:flex;align-items:center;gap:10px;margin-bottom:10px;font-size:14px}
.breakdown-dot{width:12px;height:12px;border-radius:3px;flex-shrink:0}
.breakdown-label{color:#3730a3;font-weight:500;flex:1}
.breakdown-val{font-weight:700;font-family:'Syne',sans-serif;font-size:15px}
.progress-bar{height:10px;background:#e0e7ff;border-radius:5px;overflow:hidden;margin-top:1rem}
.progress-fill{height:100%;background:linear-gradient(90deg,#6d28d9,#0369a1,#15803d);border-radius:5px;transition:width .6s cubic-bezier(.4,0,.2,1)}
.ratio-row{display:flex;justify-content:space-between;font-size:12px;color:#4338ca;font-weight:600;margin-top:5px}

/* CHARTS */
.chart-row{display:grid;grid-template-columns:1fr 1fr;gap:1.25rem;margin-top:1.5rem}
.section-title{font-size:11px;font-weight:800;color:#4338ca;text-transform:uppercase;letter-spacing:1.2px;margin-bottom:14px}

/* FILTER / TABLE */
.filter-row{display:flex;align-items:center;gap:10px;margin-bottom:1.2rem;flex-wrap:wrap}
.filter-label{font-size:13px;color:#3730a3;font-weight:600}
.filter-input{border:2px solid #c7d2fe;border-radius:8px;padding:8px 13px;font-size:14px;font-family:'Outfit',sans-serif;background:#fff;color:#1e1b4b;font-weight:600;outline:none;width:95px;transition:border-color .2s}
.filter-input:focus{border-color:#6d28d9}

.table-wrap{border:2px solid #c7d2fe;border-radius:16px;overflow:hidden;box-shadow:0 2px 16px rgba(99,102,241,0.08)}
.sched-table{width:100%;border-collapse:collapse;font-size:14px}
.sched-table th{background:linear-gradient(90deg,#6d28d9,#0369a1);padding:13px 16px;text-align:right;font-weight:700;font-size:12px;color:#fff;text-transform:uppercase;letter-spacing:.8px;border-bottom:none}
.sched-table th:first-child{text-align:center}
.sched-table td{padding:11px 16px;text-align:right;border-bottom:1px solid #e0e7ff;color:#1e1b4b;font-weight:600;font-size:14px}
.sched-table td:first-child{text-align:center;color:#6d28d9;font-weight:700;font-family:'Syne',sans-serif;font-size:15px}
.sched-table tr:last-child td{border-bottom:none}
.sched-table tr:nth-child(even) td{background:#f5f3ff}
.sched-table tr:hover td{background:#ede9fe}
.balance-cell{color:#0369a1 !important;font-weight:700;font-family:'Syne',sans-serif}

/* SUMMARY / ANALYSIS */
.summary-panel{background:#fff;border:2px solid #c7d2fe;border-radius:16px;padding:1.4rem;margin-top:1.5rem;box-shadow:0 2px 16px rgba(99,102,241,0.08)}
.loan-tag{display:inline-block;font-size:11px;font-weight:700;background:linear-gradient(135deg,#15803d,#0369a1);color:#fff;padding:4px 14px;border-radius:100px;margin-bottom:12px;text-transform:uppercase;letter-spacing:.8px;box-shadow:0 2px 10px rgba(21,128,61,0.3)}

.anim-1{animation:fadeUp .6s .05s ease both}
.anim-2{animation:fadeUp .6s .15s ease both}
.anim-3{animation:fadeUp .6s .25s ease both}

@media(max-width:640px){
  .grid-2{grid-template-columns:1fr}
  .chart-row{grid-template-columns:1fr}
  body{padding:1rem}
  .wrap{padding:1rem}
}
</style>
</head>
<body>
<div style="height:5px;background:linear-gradient(90deg,#6d28d9,#0369a1,#0891b2,#15803d,#c2410c,#be185d;width:100%;position:fixed;top:0;left:0;z-index:999"></div>
<div class="wrap" style="padding-top:3rem">
  <div class="hero" style="display:flex;flex-direction:column;align-items:center;text-align:center;gap:1.2rem">
    <div style="background:#fff;border-radius:18px;padding:16px 24px;display:inline-flex;align-items:center;justify-content:center;box-shadow:0 4px 24px rgba(99,102,241,0.2),0 0 0 2px #c7d2fe;">
      <img src="data:image/jpeg;base64,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" alt="ADB Bank" style="height:52px;width:auto;display:block">
    </div>
    <div style="width:100%">
      <div class="badge"><span class="badge-dot"></span>Loan Calculator</div>
      <h1>LoanFlow Portal</h1>
      <p>Visualise repayments, track amortisation, and plan ahead with precision.</p>
    </div>
  </div>

  <div class="tabs">
    <button class="tab active" onclick="switchTab('calculator')">Calculator</button>
    <button class="tab" onclick="switchTab('schedule')">Amortisation</button>
    <button class="tab" onclick="switchTab('analysis')">Analysis</button>
  </div>

  <!-- CALCULATOR -->
  <div id="tab-calculator" class="panel active">
    <div class="grid-2">
      <div class="card anim-1">
        <div class="section-title">Loan parameters</div>
        <div class="input-group">
          <label>Principal amount</label>
          <div class="input-row"><span class="prefix">GH₵</span><input type="number" id="principal" value="60000" step="1000"></div>
        </div>
        <div class="input-group">
          <label>Annual interest rate</label>
          <div class="input-row"><input type="number" id="rate" value="18" step="0.1"><span class="suffix">%</span></div>
        </div>
        <div class="input-group">
          <label>Loan term (months)</label>
          <div class="input-row"><input type="number" id="term" value="48" step="1"><span class="suffix">mo</span></div>
        </div>
        <button class="btn" onclick="calculate()">Calculate repayment →</button>
      </div>

      <div class="anim-2">
        <div class="metrics">
          <div class="metric">
            <div class="metric-label">Monthly payment</div>
            <div class="metric-value cyan" id="m-monthly">GH₵1,762.50</div>
          </div>
          <div class="metric">
            <div class="metric-label">Total repayment</div>
            <div class="metric-value lime" id="m-total">GH₵84,600</div>
          </div>
          <div class="metric">
            <div class="metric-label">Total interest</div>
            <div class="metric-value pink" id="m-interest">GH₵24,600</div>
          </div>
          <div class="metric">
            <div class="metric-label">Interest ratio</div>
            <div class="metric-value orange" id="m-ratio">29.1%</div>
          </div>
        </div>
        <div class="card" style="padding:1.1rem">
          <div class="breakdown-row">
            <div class="breakdown-dot" style="background:var(--cyan)"></div>
            <span class="breakdown-label">Principal</span>
            <span class="breakdown-val" style="color:var(--cyan)" id="b-principal">GH₵60,000</span>
          </div>
          <div class="breakdown-row">
            <div class="breakdown-dot" style="background:var(--pink)"></div>
            <span class="breakdown-label">Interest</span>
            <span class="breakdown-val" style="color:var(--pink)" id="b-interest">GH₵24,600</span>
          </div>
          <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:71%"></div></div>
          <div class="ratio-row" id="ratio-labels"><span>Principal 71%</span><span>Interest 29%</span></div>
        </div>
      </div>
    </div>

    <div class="chart-row anim-3">
      <div class="card">
        <div class="section-title">Balance over time</div>
        <div style="position:relative;height:210px"><canvas id="balanceChart"></canvas></div>
      </div>
      <div class="card">
        <div class="section-title">Principal vs interest split</div>
        <div style="position:relative;height:210px"><canvas id="splitChart"></canvas></div>
      </div>
    </div>
  </div>

  <!-- SCHEDULE -->
  <div id="tab-schedule" class="panel">
    <div class="filter-row">
      <span class="filter-label">Show months</span>
      <input type="number" class="filter-input" id="show-from" value="1" min="1">
      <span class="filter-label">to</span>
      <input type="number" class="filter-input" id="show-to" value="48" min="1">
      <button class="btn btn-sm" onclick="renderTable()">Apply</button>
    </div>
    <div class="table-wrap">
      <table class="sched-table">
        <thead><tr>
          <th>Month</th><th>Payment</th><th>Interest</th><th>Principal</th><th>Balance</th>
        </tr></thead>
        <tbody id="schedule-body"></tbody>
      </table>
    </div>
  </div>

  <!-- ANALYSIS -->
  <div id="tab-analysis" class="panel">
    <div class="grid-2" style="gap:1.25rem">
      <div>
        <div class="card">
          <div class="section-title">Balance after N months</div>
          <div class="input-group">
            <label>Check balance at month</label>
            <div class="input-row"><input type="number" id="check-n" value="12" min="1"><span class="suffix">mo</span></div>
          </div>
          <button class="btn" onclick="checkBalance()">Check balance →</button>
          <div style="margin-top:1.2rem;padding:1rem;background:rgba(0,120,160,0.08);border:1px solid rgba(0,229,255,0.2);border-radius:12px">
            <div style="font-size:11px;font-weight:800;color:#6d28d9;text-transform:uppercase;letter-spacing:.8px;margin-bottom:6px" id="bal-label">Balance at month 12</div>
            <div style="font-size:30px;font-weight:800;font-family:'Syne',sans-serif;color:#0369a1" id="bal-amount">GH₵48,752</div>
            <div style="font-size:13px;color:#3730a3;font-weight:600;margin-top:6px" id="bal-paid">Paid so far: GH₵21,150 · Principal paid: GH₵11,248</div>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="section-title">Yearly breakdown</div>
        <div style="position:relative;height:250px"><canvas id="yearlyChart"></canvas></div>
      </div>
    </div>

    <div class="summary-panel">
      <div class="loan-tag">Sample profile — John Aidoo</div>
      <div class="grid-2" style="margin-top:12px;gap:10px">
        <div class="metric" style="margin:0"><div class="metric-label">Principal</div><div class="metric-value">GH₵20,600</div></div>
        <div class="metric" style="margin:0"><div class="metric-label">Term</div><div class="metric-value">6 months</div></div>
        <div class="metric" style="margin:0"><div class="metric-label">Monthly payment</div><div class="metric-value cyan">GH₵3,615.82</div></div>
        <div class="metric" style="margin:0"><div class="metric-label">Total interest</div><div class="metric-value pink">GH₵1,094.92</div></div>
      </div>
    </div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script>
let schedule=[];
let balChart,splitChartInst,yrChart;
const CYAN='#0369a1',LIME='#b5ff47',PINK='#ff4fd8',ORANGE='#ff7c3a';
const GRID='rgba(99,102,241,0.1)',TICK='#3730a3';

function fmt(n){return'GH₵'+Math.round(n).toLocaleString('en-GH');}
function fmtD(n){return'GH₵'+n.toLocaleString('en-GH',{minimumFractionDigits:2,maximumFractionDigits:2});}

function buildSchedule(P,r,n){
  let M=P*r*Math.pow(1+r,n)/(Math.pow(1+r,n)-1);
  let rows=[],bal=P;
  for(let i=1;i<=n;i++){
    let interest=bal*r,principal=M-interest;
    bal-=principal;if(bal<0.01)bal=0;
    rows.push({month:i,payment:M,interest,principal,balance:bal});
  }
  return{M,rows};
}

function calculate(){
  let P=parseFloat(document.getElementById('principal').value)||60000;
  let annRate=parseFloat(document.getElementById('rate').value)||18;
  let n=parseInt(document.getElementById('term').value)||48;
  let r=annRate/100/12;
  let res=buildSchedule(P,r,n);schedule=res.rows;
  let M=res.M,totalPaid=M*n,totalInterest=totalPaid-P,ratio=(totalInterest/totalPaid*100);
  document.getElementById('m-monthly').textContent=fmtD(M);
  document.getElementById('m-total').textContent=fmt(totalPaid);
  document.getElementById('m-interest').textContent=fmt(totalInterest);
  document.getElementById('m-ratio').textContent=ratio.toFixed(1)+'%';
  document.getElementById('b-principal').textContent=fmt(P);
  document.getElementById('b-interest').textContent=fmt(totalInterest);
  let pRatio=(P/totalPaid*100).toFixed(1),iRatio=(100-parseFloat(pRatio)).toFixed(1);
  document.getElementById('progress-fill').style.width=pRatio+'%';
  document.getElementById('ratio-labels').innerHTML='<span>Principal '+pRatio+'%</span><span>Interest '+iRatio+'%</span>';
  document.getElementById('show-to').value=n;
  renderCharts(schedule);renderTable();checkBalance();renderYearlyChart(schedule);
}

const baseOpts={responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},animation:{duration:700}};

function renderCharts(rows){
  let skip=Math.max(1,Math.floor(rows.length/20));
  let f=rows.filter((_,i)=>i%skip===0||i===rows.length-1);
  let labels=f.map(r=>r.month);
  if(balChart)balChart.destroy();
  balChart=new Chart(document.getElementById('balanceChart'),{
    type:'line',
    data:{labels,datasets:[{data:f.map(r=>Math.round(r.balance)),borderColor:CYAN,backgroundColor:'rgba(0,229,255,0.07)',fill:true,tension:0.4,pointRadius:0,borderWidth:2.5}]},
    options:{...baseOpts,scales:{x:{ticks:{font:{size:12,weight:'600'},color:TICK},grid:{color:GRID}},y:{ticks:{font:{size:10},color:TICK,callback:v=>fmt(v)},grid:{color:GRID}}}}
  });
  if(splitChartInst)splitChartInst.destroy();
  splitChartInst=new Chart(document.getElementById('splitChart'),{
    type:'bar',
    data:{labels,datasets:[
      {data:f.map(r=>Math.round(r.principal)),backgroundColor:CYAN,borderRadius:3,label:'Principal'},
      {data:f.map(r=>Math.round(r.interest)),backgroundColor:PINK,borderRadius:3,label:'Interest'}
    ]},
    options:{...baseOpts,scales:{x:{stacked:true,ticks:{font:{size:12,weight:'600'},color:TICK},grid:{display:false}},y:{stacked:true,ticks:{font:{size:10},color:TICK,callback:v=>fmt(v)},grid:{color:GRID}}}}
  });
}

function renderYearlyChart(rows){
  let years={};
  rows.forEach(r=>{let yr='Y'+(Math.ceil(r.month/12));if(!years[yr])years[yr]={i:0,p:0};years[yr].i+=r.interest;years[yr].p+=r.principal;});
  let labels=Object.keys(years);
  if(yrChart)yrChart.destroy();
  yrChart=new Chart(document.getElementById('yearlyChart'),{
    type:'bar',
    data:{labels,datasets:[
      {data:labels.map(k=>Math.round(years[k].p)),backgroundColor:LIME,borderRadius:4,label:'Principal'},
      {data:labels.map(k=>Math.round(years[k].i)),backgroundColor:ORANGE,borderRadius:4,label:'Interest'}
    ]},
    options:{...baseOpts,scales:{x:{stacked:true,ticks:{font:{size:12,weight:'600'},color:TICK},grid:{display:false}},y:{stacked:true,ticks:{font:{size:11},color:TICK,callback:v=>fmt(v)},grid:{color:GRID}}}}
  });
}

function renderTable(){
  let from=parseInt(document.getElementById('show-from').value)||1;
  let to=parseInt(document.getElementById('show-to').value)||schedule.length;
  document.getElementById('schedule-body').innerHTML=
    schedule.filter(r=>r.month>=from&&r.month<=to)
    .map(r=>`<tr><td>${r.month}</td><td>${fmtD(r.payment)}</td><td>${fmtD(r.interest)}</td><td>${fmtD(r.principal)}</td><td class="balance-cell">${r.balance<0.01?fmt(0):fmtD(r.balance)}</td></tr>`).join('');
}

function checkBalance(){
  let N=parseInt(document.getElementById('check-n').value)||12;
  if(!schedule.length)return;
  let row=schedule[Math.min(N,schedule.length)-1];if(!row)return;
  let P=parseFloat(document.getElementById('principal').value)||60000;
  document.getElementById('bal-label').textContent='Balance at month '+N;
  document.getElementById('bal-amount').textContent=fmtD(row.balance);
  document.getElementById('bal-paid').textContent='Paid so far: '+fmt(row.payment*row.month)+' · Principal paid: '+fmt(P-row.balance);
}

function switchTab(name){
  document.querySelectorAll('.tab').forEach((t,i)=>t.classList.toggle('active',['calculator','schedule','analysis'][i]===name));
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.getElementById('tab-'+name).classList.add('active');
}

calculate();
</script>

<footer style="position:relative;z-index:1;max-width:960px;margin:2.5rem auto 0;padding:1.5rem 1.5rem 2.5rem;border-top:1px solid rgba(0,0,0,0.1);display:flex;flex-wrap:wrap;align-items:center;justify-content:space-between;gap:1rem">
  <div>
    <div style="font-size:11px;font-weight:700;color:#4338ca;font-weight:700;text-transform:uppercase;letter-spacing:1px;margin-bottom:5px">Developed by</div>
    <div style="font-family:'Syne',sans-serif;font-size:16px;font-weight:800;background:linear-gradient(90deg,#6d28d9,#0369a1,#15803d);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text">Emmanuel Aidoo</div>
  </div>
  <div style="display:flex;align-items:center;gap:10px">
    <div style="width:1px;height:36px;background:rgba(0,0,0,0.12)"></div>
    <div>
      <div style="font-size:11px;font-weight:700;color:#4338ca;font-weight:700;text-transform:uppercase;letter-spacing:1px;margin-bottom:5px">Donated to</div>
      <div style="font-family:'Syne',sans-serif;font-size:16px;font-weight:800;color:#6d28d9">Ellen Odame</div>
      <div style="font-size:13px;color:#0369a1;margin-top:2px;font-weight:700">ADB Bank</div>
    </div>
  </div>
  <div style="display:flex;align-items:center;gap:10px">
    <div style="width:1px;height:36px;background:rgba(0,0,0,0.12)"></div>
    <div style="background:#fff;border-radius:8px;padding:6px 10px;display:flex;align-items:center">
      <img src="data:image/jpeg;base64,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" alt="ADB Bank" style="height:40px;width:auto;display:block">
    </div>
  </div>
</footer>
</body>
</html>
