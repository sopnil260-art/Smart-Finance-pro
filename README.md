<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>SmartFinance Pro - Wealth Growth Dashboard</title>
  <meta name="description" content="Advanced finance dashboard: calculators, charts, and personal finance tips to grow your wealth.">
  <meta name="keywords" content="finance, dashboard, calculator, compound interest, retirement, loan, personal finance, chart, tips">
  <meta name="author" content="SmartFinance Pro Team">
  <link rel="icon" href="https://cdn-icons-png.flaticon.com/512/2921/2921822.png" />
  <!-- Chart.js for charts -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    /* ... [CSS unchanged, omitted for brevity] ... */
    :root {
      --primary: #283e4a;
      --accent: #f1c40f;
      --accent-hover: #d4ac0d;
      --bg: #f5f7fa;
      --card-bg: #fff;
      --text: #333;
      --text-light: #f5f7fa;
      --success: #27ae60;
      --danger: #e74c3c;
      --link: #2980b9;
      --shadow: 0 8px 24px rgba(40,62,74,0.10);
      --sidebar: #202d35;
    }
    [data-theme="dark"] {
      --primary: #23272f;
      --accent: #f1c40f;
      --accent-hover: #d4ac0d;
      --bg: #181c23;
      --card-bg: #23272f;
      --text: #e5e7eb;
      --text-light: #f5f7fa;
      --success: #27ae60;
      --danger: #f44336;
      --link: #82b1ff;
      --shadow: 0 8px 32px rgba(225,225,225,0.10);
      --sidebar: #181c23;
    }
    html, body { height: 100%; margin: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: var(--bg); color: var(--text); transition: background 0.2s, color 0.2s; }
    body { display: flex; min-height: 100vh; flex-direction: column; }
    header { background: var(--primary); color: var(--text-light); padding: 1.4rem 2rem 1rem 2rem; text-align: left; box-shadow: var(--shadow); position: sticky; top: 0; z-index: 100; display: flex; align-items: center; justify-content: space-between; }
    header h1 { margin: 0; font-weight: 700; font-size: 2rem; letter-spacing: 1px; text-shadow: 0 2px 8px rgba(40,62,74,0.09); }
    .dark-toggle { cursor: pointer; background: none; border: none; color: var(--accent); font-size: 1.5rem; margin-left: 1rem; transition: color 0.2s; }
    .dark-toggle:focus { outline: 2px solid var(--accent); outline-offset: 2px; }
    .dashboard-container { display: flex; flex: 1 1 auto; height: 100%; }
    .sidebar { background: var(--sidebar); color: var(--text-light); width: 225px; min-width: 200px; height: 100%; padding: 2rem 0 1rem 0; box-shadow: 2px 0 10px rgba(40,62,74,0.10); display: flex; flex-direction: column; position: sticky; top: 0; z-index: 99; }
    .sidebar nav { display: flex; flex-direction: column; gap: 1rem; margin-top: 2rem; }
    .sidebar nav a { color: var(--text-light); text-decoration: none; font-weight: 600; font-size: 1.12rem; padding: 0.6rem 2rem; border-radius: 6px 0 0 6px; transition: background 0.2s, color 0.2s; position: relative; }
    .sidebar nav a.active, .sidebar nav a:hover { background: var(--accent); color: var(--primary); }
    .sidebar .logo { text-align: center; font-size: 2.2rem; font-weight: 900; letter-spacing: 2px; color: var(--accent); }
    main { flex: 1 1 auto; padding: 2rem 2.5vw; max-width: 1200px; margin: auto; background: var(--bg); min-height: 100vh; }
    .card { background: var(--card-bg); border-radius: 14px; box-shadow: var(--shadow); padding: 2rem 1.5rem; margin-bottom: 2rem; animation: fadeIn 1.1s; }
    @keyframes fadeIn { from {opacity: 0; transform: translateY(30px);} to {opacity: 1; transform: translateY(0);} }
    .card h2 { margin-top: 0; color: var(--primary);}
    .calculator-form label { display: block; margin-bottom: 0.3rem; font-weight: 600; color: var(--primary); }
    .calculator-form input[type="number"], .calculator-form input[type="email"] { width: 98%; padding: 0.52rem; border-radius: 6px; border: 1px solid #ccc; font-size: 1.05rem; margin-bottom: 1rem; background: var(--card-bg); color: var(--text); transition: box-shadow 0.2s, border-color 0.2s; }
    .calculator-form input:focus { box-shadow: 0 0 0 2px var(--accent)44; border-color: var(--accent); outline: none; }
    .calculator-form button { background: linear-gradient(90deg,var(--accent) 60%, #f6e58d 100%); color: var(--primary); border: none; padding: 0.8rem 2rem; font-size: 1.13rem; border-radius: 6px; cursor: pointer; font-weight: 700; box-shadow: 0 2px 8px var(--accent)22; transition: background 0.2s, box-shadow 0.2s; margin-top: 0.5rem; margin-bottom: 1rem; display: block; margin-left: auto; margin-right: auto; }
    .calculator-form button:hover, .calculator-form button:focus { background: var(--accent-hover); box-shadow: 0 4px 16px var(--accent)44; }
    .result { font-size: 1.18rem; font-weight: 700; text-align: center; color: var(--success); min-height: 2.2em; margin-bottom: 1rem; }
    .chart-container { width: 100%; min-height: 270px; margin-top: 1.5rem; margin-bottom: 2rem; display: flex; justify-content: center; align-items: center; }
    canvas { max-width: 100%; background: var(--card-bg); border-radius: 12px; }
    .tips-carousel { width: 100%; max-width: 700px; margin: 2rem auto; text-align: center; background: var(--card-bg); border-radius: 14px; box-shadow: var(--shadow); padding: 1.5rem 1rem; position: relative; min-height: 110px; animation: fadeIn 1.1s; }
    .tips-carousel .tip-content { font-size: 1.13rem; color: var(--primary); min-height: 2.5em; margin-bottom: 0.7em; transition: opacity 0.3s; }
    .tips-carousel .tip-nav { margin-top: 0.5em; }
    .tips-carousel button { background: var(--accent); color: var(--primary); border: none; border-radius: 6px; padding: 0.45em 1em; font-size: 1rem; margin: 0 0.4em; cursor: pointer; font-weight: 700; transition: background 0.2s; }
    .tips-carousel button:focus { outline: 2px solid var(--accent); }
    .subscribe-form { max-width: 420px; margin: 2rem auto; background: var(--card-bg); border-radius: 14px; box-shadow: var(--shadow); padding: 1.5rem 1rem; animation: fadeIn 1.1s; }
    .subscribe-form h3 { color: var(--primary); }
    .subscribe-form input[type="email"] { width: 90%; margin: auto; display: block; }
    .subscribe-form button { background: var(--accent); color: var(--primary); border: none; padding: 0.7rem 2rem; font-size: 1rem; border-radius: 5px; cursor: pointer; font-weight: 700; margin-top: 0.7rem; margin-bottom: 0.7rem; box-shadow: 0 2px 8px var(--accent)22; transition: background 0.2s; display: block; margin-left: auto; margin-right: auto; }
    .subscribe-form button:hover, .subscribe-form button:focus { background: var(--accent-hover); box-shadow: 0 4px 16px var(--accent)44; }
    .subscribe-form .msg { min-height: 1.6em; text-align: center; font-weight: 600; color: var(--link); margin-top: 0.7em; }
    .subscribe-form .msg.error { color: var(--danger); }
    footer { text-align: center; padding: 1.2rem; color: #777; font-size: 0.93rem; background: #f9fafb; border-top: 1px solid #e5e7eb; margin-top: auto; }
    @media (max-width: 900px) { .dashboard-container {flex-direction: column;} .sidebar {width: 100%; min-width: unset; position: static; flex-direction: row; justify-content: space-between; padding: 1rem 0;} .sidebar nav {flex-direction: row; gap: 0.2rem; margin-top: 0;} .sidebar .logo {display:none;} main {padding: 1.2rem 2vw;} }
    @media (max-width: 600px) { header h1 { font-size: 1.3rem; } .sidebar nav a { font-size: 0.92rem; padding: 0.5rem 0.6rem;} .card, .tips-carousel, .subscribe-form { padding: 1rem 0.3rem;} .tips-carousel .tip-content { font-size: 0.98rem;} }
  </style>
</head>
<body>
  <header>
    <h1>SmartFinance Pro</h1>
    <button class="dark-toggle" id="darkToggle" title="Toggle dark mode" aria-label="Toggle dark mode">🌙</button>
  </header>
  <div class="dashboard-container">
    <aside class="sidebar">
      <div class="logo" aria-label="SmartFinance logo">💸</div>
      <nav>
        <a href="#dashboard" id="nav-dashboard" class="active">Dashboard</a>
        <a href="#compound" id="nav-compound">Compound Interest</a>
        <a href="#retirement" id="nav-retirement">Retirement</a>
        <a href="#loan" id="nav-loan">Loan Payoff</a>
        <a href="#tips" id="nav-tips">Tips</a>
        <a href="#subscribe" id="nav-subscribe">Subscribe</a>
      </nav>
    </aside>
    <main>
      <!-- Dashboard -->
      <section class="card" id="dashboard">
        <h2>Welcome to SmartFinance Pro</h2>
        <p>
          Your all-in-one, advanced personal finance dashboard.<br>
          Explore powerful calculators, visualize your investment growth, and get the best tips for financial success.
        </p>
        <div class="chart-container">
          <canvas id="dashboardChart" width="650" height="320" aria-label="Sample Wealth Growth Chart"></canvas>
        </div>
      </section>
      <!-- Compound Interest Calculator -->
      <section class="card" id="compound" style="display:none;">
        <h2>Compound Interest Calculator</h2>
        <form class="calculator-form" id="compoundForm" autocomplete="off" aria-label="Compound Interest Calculator">
          <label for="c-principal">Initial Investment ($):</label>
          <input type="number" id="c-principal" name="c-principal" min="0" placeholder="e.g. 2000" required />
          <label for="c-rate">Annual Interest Rate (%):</label>
          <input type="number" id="c-rate" name="c-rate" step="0.01" min="0" max="100" placeholder="e.g. 7.5" required />
          <label for="c-years">Years:</label>
          <input type="number" id="c-years" name="c-years" min="1" placeholder="e.g. 15" required />
          <button type="submit">Calculate</button>
          <div class="result" id="compoundResult"></div>
        </form>
        <div class="chart-container">
          <canvas id="compoundChart" width="650" height="320" aria-label="Compound Interest Chart"></canvas>
        </div>
      </section>
      <!-- Retirement Calculator -->
      <section class="card" id="retirement" style="display:none;">
        <h2>Retirement Growth Calculator</h2>
        <form class="calculator-form" id="retirementForm" autocomplete="off" aria-label="Retirement Calculator">
          <label for="r-principal">Current Savings ($):</label>
          <input type="number" id="r-principal" name="r-principal" min="0" placeholder="e.g. 5000" required />
          <label for="r-contribution">Annual Contribution ($):</label>
          <input type="number" id="r-contribution" name="r-contribution" min="0" placeholder="e.g. 3000" required />
          <label for="r-rate">Annual Growth Rate (%):</label>
          <input type="number" id="r-rate" name="r-rate" step="0.01" min="0" max="100" placeholder="e.g. 6" required />
          <label for="r-years">Years to Retirement:</label>
          <input type="number" id="r-years" name="r-years" min="1" placeholder="e.g. 30" required />
          <button type="submit">Calculate</button>
          <div class="result" id="retirementResult"></div>
        </form>
        <div class="chart-container">
          <canvas id="retirementChart" width="650" height="320" aria-label="Retirement Chart"></canvas>
        </div>
      </section>
      <!-- Loan Payoff Calculator -->
      <section class="card" id="loan" style="display:none;">
        <h2>Loan Payoff Calculator</h2>
        <form class="calculator-form" id="loanForm" autocomplete="off" aria-label="Loan Payoff Calculator">
          <label for="l-amount">Loan Amount ($):</label>
          <input type="number" id="l-amount" name="l-amount" min="0" placeholder="e.g. 10000" required />
          <label for="l-rate">Annual Interest Rate (%):</label>
          <input type="number" id="l-rate" name="l-rate" step="0.01" min="0" max="100" placeholder="e.g. 5" required />
          <label for="l-years">Years to Pay Off:</label>
          <input type="number" id="l-years" name="l-years" min="1" placeholder="e.g. 5" required />
          <button type="submit">Calculate</button>
          <div class="result" id="loanResult"></div>
        </form>
        <div class="chart-container">
          <canvas id="loanChart" width="650" height="320" aria-label="Loan Payoff Chart"></canvas>
        </div>
      </section>
      <!-- Financial Tips Carousel -->
      <section class="tips-carousel" id="tips" style="display:none;">
        <h2>Top Personal Finance Tips</h2>
        <div class="tip-content" id="tipContent"></div>
        <div class="tip-nav">
          <button id="prevTip" aria-label="Previous tip">◀</button>
          <span id="tipIndex"></span>
          <button id="nextTip" aria-label="Next tip">▶</button>
        </div>
      </section>
      <!-- Newsletter Subscribe -->
      <section class="subscribe-form" id="subscribe" style="display:none;">
        <h3>Subscribe for Exclusive Tips & Updates</h3>
        <form id="subscribeForm" autocomplete="off" aria-label="Subscribe for Tips & Updates">
          <input type="email" id="s-email" name="s-email" placeholder="Enter your email" required />
          <button type="submit">Subscribe</button>
          <div class="msg" id="subscribeMsg"></div>
        </form>
      </section>
    </main>
  </div>
  <footer>
    &copy; 2025 SmartFinance Pro. All rights reserved.
  </footer>
  <script>
    // --- Dark Mode ---
    function setTheme(theme) {
      document.documentElement.setAttribute('data-theme', theme);
      localStorage.setItem('sf-theme', theme);
      darkToggle.textContent = theme === 'dark' ? '🌞' : '🌙';
    }
    const darkToggle = document.getElementById('darkToggle');
    darkToggle.onclick = () => {
      setTheme(document.documentElement.getAttribute('data-theme') === 'dark' ? 'light' : 'dark');
    };
    (() => {
      const saved = localStorage.getItem('sf-theme');
      setTheme(saved === 'dark' ? 'dark' : 'light');
    })();

    // --- Navigation ---
    const navLinks = [
      {id:"nav-dashboard",section:"dashboard"},
      {id:"nav-compound",section:"compound"},
      {id:"nav-retirement",section:"retirement"},
      {id:"nav-loan",section:"loan"},
      {id:"nav-tips",section:"tips"},
      {id:"nav-subscribe",section:"subscribe"}
    ];
    navLinks.forEach(link => {
      document.getElementById(link.id).onclick = function(e){
        e.preventDefault();
        navLinks.forEach(l => {
          document.getElementById(l.id).classList.remove("active");
          document.getElementById(l.section).style.display = "none";
        });
        this.classList.add("active");
        document.getElementById(link.section).style.display = "";
        window.location.hash = link.section;
        setTimeout(() => document.getElementById(link.section).scrollIntoView({behavior:"smooth"}),100);
      }
    });
    window.addEventListener("load", ()=>{
      const hash = location.hash.replace("#","");
      if(hash && document.getElementById(hash)) {
        document.getElementById("nav-"+hash).click();
      }
    });

    // --- Dashboard Chart (Sample projection) ---
    const dashboardCtx = document.getElementById('dashboardChart').getContext('2d');
    new Chart(dashboardCtx, {
      type: 'line',
      data: {
        labels: Array.from({length:21},(_,i)=>`Year ${i}`),
        datasets: [{
          label: 'Sample Portfolio ($)',
          data: Array.from({length:21},(_,i)=>2000*Math.pow(1.07,i)+1000*i),
          borderColor: '#27ae60',
          backgroundColor: 'rgba(39,174,96,0.11)',
          pointBackgroundColor: '#f1c40f',
          pointRadius: 3,
          tension: 0.18,
          fill:true
        }]
      },
      options: {
        plugins:{legend:{display:true,position:'bottom'},tooltip:{enabled:true}},
        scales:{x:{title:{display:true,text:'Year'}},y:{title:{display:true,text:'Value ($)'},beginAtZero:true}}
      }
    });

    // --- Compound Interest Calculator ---
    let compoundChart;
    function calculateCompound(principal, rate, years) {
      let values = [principal];
      for(let i=1;i<=years;i++) {
        values.push(values[i-1]*(1+rate/100));
      }
      return values;
    }
    document.getElementById("compoundForm").onsubmit = function(e){
      e.preventDefault();
      const principal = parseFloat(document.getElementById("c-principal").value);
      const rate = parseFloat(document.getElementById("c-rate").value);
      const years = parseInt(document.getElementById("c-years").value);
      const resultDiv = document.getElementById("compoundResult");
      if(isNaN(principal)||principal<=0||isNaN(rate)||rate<=0||rate>100||isNaN(years)||years<1){
        resultDiv.textContent = "Please enter valid positive numbers.";
        updateChart(compoundChart,[0],0,"compoundChart");
        return;
      }
      const growth = calculateCompound(principal,rate,years);
      resultDiv.textContent = `After ${years} years, your investment will grow to $${growth[growth.length-1].toLocaleString(undefined,{minimumFractionDigits:2})}`;
      updateChart(compoundChart,growth,years,"compoundChart");
    }
    function updateChart(chart, data, years, canvasId){
      const labels = Array.from({length:years+1},(_,i)=>`Year ${i}`);
      const ctx = document.getElementById(canvasId).getContext('2d');
      if(chart) chart.destroy();
      chart = new Chart(ctx, {
        type:'line',
        data:{
          labels:labels,
          datasets:[{
            label:'Value ($)',
            data:data,
            borderColor:'#27ae60',
            backgroundColor:'rgba(39,174,96,0.11)',
            pointBackgroundColor:'#f1c40f',
            pointRadius:3,
            tension:0.18,
            fill:true
          }]
        },
        options:{
          plugins:{legend:{display:false},tooltip:{enabled:true}},
          scales:{x:{title:{display:true,text:'Year'}},y:{title:{display:true,text:'Value ($)'},beginAtZero:true}}
        }
      });
      if(canvasId==="compoundChart") compoundChart=chart;
      if(canvasId==="retirementChart") retirementChart=chart;
      if(canvasId==="loanChart") loanChart=chart;
    }
    updateChart(compoundChart,[0],0,"compoundChart");

    // --- Retirement Calculator ---
    let retirementChart;
    function calculateRetirement(principal, annual, rate, years) {
      let values = [principal];
      for(let i=1;i<=years;i++) {
        let last = values[i-1]*(1+rate/100)+annual;
        values.push(last);
      }
      return values;
    }
    document.getElementById("retirementForm").onsubmit = function(e){
      e.preventDefault();
      const principal = parseFloat(document.getElementById("r-principal").value);
      const annual = parseFloat(document.getElementById("r-contribution").value);
      const rate = parseFloat(document.getElementById("r-rate").value);
      const years = parseInt(document.getElementById("r-years").value);
      const resultDiv = document.getElementById("retirementResult");
      if(isNaN(principal)||principal<0||isNaN(annual)||annual<0||isNaN(rate)||rate<=0||rate>100||isNaN(years)||years<1){
        resultDiv.textContent = "Please enter valid numbers.";
        updateChart(retirementChart,[0],0,"retirementChart");
        return;
      }
      const growth = calculateRetirement(principal,annual,rate,years);
      resultDiv.textContent = `After ${years} years, you will have $${growth[growth.length-1].toLocaleString(undefined,{minimumFractionDigits:2})}`;
      updateChart(retirementChart,growth,years,"retirementChart");
    };
    updateChart(retirementChart,[0],0,"retirementChart");

    // --- Loan Payoff Calculator ---
    let loanChart;
    function calculateLoanPayoff(amount, rate, years) {
      const n = years * 12;
      const r = rate / 100 / 12;
      if(r === 0) {
        const monthly = amount / n;
        return {monthly, total: amount, values: Array.from({length:years+1},(_,i)=>amount - monthly*i*12)};
      }
      const monthly = amount * r * Math.pow(1+r,n) / (Math.pow(1+r,n)-1);
      const total = monthly * n;
      let values = [amount];
      for(let i=1;i<=years;i++) {
        let paid = monthly*i*12;
        let rem = amount * Math.pow(1+r,i*12) - monthly * (Math.pow(1+r,i*12)-1)/r;
        values.push(rem>0?rem:0);
      }
      return {monthly, total, values};
    }
    document.getElementById("loanForm").onsubmit = function(e){
      e.preventDefault();
      const amount = parseFloat(document.getElementById("l-amount").value);
      const rate = parseFloat(document.getElementById("l-rate").value);
      const years = parseInt(document.getElementById("l-years").value);
      const resultDiv = document.getElementById("loanResult");
      if(isNaN(amount)||amount<=0||isNaN(rate)||rate<0||rate>100||isNaN(years)||years<1){
        resultDiv.textContent = "Please enter valid numbers.";
        updateChart(loanChart,[0],0,"loanChart");
        return;
      }
      const payoff = calculateLoanPayoff(amount,rate,years);
      resultDiv.innerHTML = `
        <div>Monthly Payment: <b>$${payoff.monthly.toLocaleString(undefined,{minimumFractionDigits:2})}</b></div>
        <div>Total Repayment: <b>$${payoff.total.toLocaleString(undefined,{minimumFractionDigits:2})}</b></div>
      `;
      updateChart(loanChart,payoff.values,years,"loanChart");
    };
    updateChart(loanChart,[0],0,"loanChart");

    // --- Tips Carousel ---
    const tipsArr = [
      "💡 <b>Start Early:</b> The sooner you invest, the more compound interest can work for you.",
      "📈 <b>Diversify:</b> Spread your investments across assets to reduce risk.",
      "📊 <b>Track Expenses:</b> Use budgeting apps to control spending.",
      "⏳ <b>Long-Term Focus:</b> Don't panic with short-term market changes.",
      "💸 <b>Automate Savings:</b> Set up auto-transfers to savings/investment accounts.",
      "🏦 <b>Emergency Fund:</b> Always keep 3-6 months of expenses saved.",
      "🧾 <b>Review Regularly:</b> Rebalance your portfolio annually.",
      "💳 <b>Minimize Debt:</b> Pay off high-interest loans first.",
      "🔍 <b>Educate Yourself:</b> Stay up-to-date with financial news and resources.",
      "🔒 <b>Protect Assets:</b> Insure what you can't afford to lose."
    ];
    let tipIdx = 0;
    const tipContent = document.getElementById("tipContent");
    const tipIndex = document.getElementById("tipIndex");
    function showTip(idx){
      tipContent.innerHTML = tipsArr[idx];
      tipIndex.textContent = `Tip ${idx+1} of ${tipsArr.length}`;
    }
    document.getElementById("prevTip").onclick = ()=>{tipIdx=(tipIdx-1+tipsArr.length)%tipsArr.length;showTip(tipIdx);}
    document.getElementById("nextTip").onclick = ()=>{tipIdx=(tipIdx+1)%tipsArr.length;showTip(tipIdx);}
    showTip(0);

    // --- Subscribe Form ---
    document.getElementById("subscribeForm").onsubmit = function(e){
      e.preventDefault();
      const email = document.getElementById("s-email").value.trim();
      const msg = document.getElementById("subscribeMsg");
      if(!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)){
        msg.textContent = "Please enter a valid email address.";
        msg.classList.add("error");
        return;
      }
      msg.textContent = `Thanks for subscribing, ${email}! 🚀`;
      msg.classList.remove("error");
      document.getElementById("s-email").value = '';
    }
  </script>
</body>
</html>
