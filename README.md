<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>cash-tupe26 — Demo</title>
<style>
  :root{ --bg:#fff; --pink:#ff5fa2; --muted:#666; --card:#fff0f6; --accent:#ff8fba; }
  body{margin:0;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, Arial;background:var(--bg);color:#222;}
  .container{max-width:920px;margin:18px auto;padding:18px;}
  .center{text-align:center;}
  h1,h2{margin:6px 0;color:var(--pink);}
  .card{background:var(--card); padding:16px;border-radius:10px;border:1px solid rgba(255,95,162,0.12); box-shadow:0 4px 18px rgba(0,0,0,0.04); margin-bottom:12px;}
  input,select{width:100%;padding:12px;border-radius:8px;border:1px solid #ffd7e8;margin-top:10px;box-sizing:border-box;font-size:15px}
  .btn{display:inline-block;padding:11px 14px;border-radius:10px;border:none;background:linear-gradient(90deg,var(--pink),var(--accent));color:#fff;font-weight:700;cursor:pointer}
  .btn.full{display:block;width:100%;margin-top:12px}
  .muted{color:var(--muted);font-size:14px}
  .menu-btn{display:block;width:100%;padding:10px;border-radius:10px;border:1px solid var(--pink);background:#fff;color:var(--pink);margin-top:10px;cursor:pointer}
  .hidden{display:none}
  .small{font-size:13px;color:var(--muted)}
  .loader { width:48px;height:48px;border-radius:50%;border:6px solid rgba(255,95,162,0.12);border-top-color:var(--pink); margin:12px auto; animation:spin 1s linear infinite; }
  @keyframes spin{ to { transform:rotate(360deg); } }
  .badge{display:inline-block;padding:6px 10px;border-radius:999px;background:#fff;color:var(--pink);border:1px solid rgba(255,95,162,0.12);font-weight:700}
  .top-row{display:flex;gap:12px;align-items:center;justify-content:space-between;flex-wrap:wrap}
  .video-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px;margin-top:12px}
  .video-card{background:#fff;padding:12px;border-radius:8px;border:1px solid #ffe6f1}
  .go-back{margin-top:12px;display:inline-block;padding:10px;border-radius:8px;background:#fff;border:1px solid #ddd;color:#333;cursor:pointer}
  footer{margin-top:18px;text-align:center;color:var(--muted);font-size:13px}
</style>
</head>
<body>

<div class="container">

  <!-- WELCOME -->
  <div id="welcomeScreen" class="center card">
    <h1>Welcome to <span style="color:#000">cash-tupe26</span></h1>
    <p class="small">Form appears in <span id="countdown">5</span> seconds</p>
  </div>

  <!-- LOGIN -->
  <div id="loginScreen" class="hidden card">
    <h2>cash-tupe26</h2>
    <p class="muted">Enter your phone number and full name</p>
    <input id="phone" type="tel" placeholder="Phone number (e.g. 08012345678)" inputmode="numeric" />
    <input id="fullname" type="text" placeholder="Full name" />
    <div id="loginError" class="small" style="color:#d9534f;display:none"></div>
    <button class="btn full" id="loginBtn">Login</button>
  </div>

  <!-- GENERIC LOADING (used for multiple flows) -->
  <div id="loadingScreen" class="hidden card center" aria-live="polite">
    <div class="loader" role="status" aria-hidden="true"></div>
    <div id="loadingMsg" class="small">Loading...</div>
  </div>

  <!-- DASHBOARD -->
  <div id="dashboard" class="hidden">
    <div class="top-row">
      <div>
        <h2>Dashboard</h2>
        <div class="small">Watch video and earn money</div>
      </div>
      <div class="card" style="min-width:200px;">
        <div class="small">Balance</div>
        <div style="font-size:20px;font-weight:800" id="balanceDisplay">₦0.00</div>
        <div class="small">Target: ₦59,000 in 20:00 minutes (demo)</div>
      </div>
    </div>

    <div style="margin-top:12px">
      <div class="video-grid" id="videosGrid"></div>
    </div>

    <div style="margin-top:14px;display:grid;grid-template-columns:1fr 1fr;gap:10px">
      <button class="btn full" id="withdrawBtn">Withdraw</button>
      <button class="menu-btn" id="buyCodeBtn">Buy Code</button>
    </div>

    <div style="display:flex;gap:10px;margin-top:10px">
      <button class="menu-btn" id="supportBtn">Support</button>
      <button class="menu-btn" id="faqBtn">FAQ</button>
    </div>

    <div style="margin-top:12px" class="small">Videos watched: <span id="watchedCount">0</span> / 6 • Progress: <span id="progressPct">0%</span></div>
  </div>

  <!-- WITHDRAW PAGE -->
  <div id="withdrawPage" class="hidden card">
    <h2>Withdraw Funds</h2>
    <p class="small">Fill details and provide your purchase code to enable withdrawal.</p>
    <input id="w_fullname" type="text" placeholder="Your Full Name">
    <input id="w_account" type="text" placeholder="Account Number">
    <select id="w_bank">
      <option value="">Select Bank</option>
      <option>Access Bank</option>
      <option>UBA</option>
      <option>First Bank</option>
      <option>FCMB</option>
      <option>GT Bank</option>
      <option>OPay</option>
    </select>
    <input id="w_code" type="password" placeholder="Enter your purchase code (required)">
    <div id="withdrawError" class="small" style="color:#d9534f;display:none"></div>
    <button class="btn full" id="doWithdrawBtn">Withdraw</button>
    <button class="go-back" onclick="goBack()">Go Back</button>
  </div>

  <!-- BUY CODE: ENTRY FORM -->
  <div id="buyForm" class="hidden card">
    <h2>Welcome to purchase code</h2>
    <p class="small">Enter details to purchase the code</p>
    <input id="buy_email" type="email" placeholder="Your email">
    <input id="buy_name" type="text" placeholder="Your full name">
    <div id="buyError" class="small" style="color:#d9534f;display:none"></div>
    <button class="btn full" id="payBtn">Pay</button>
    <button class="go-back" onclick="goBack()">Go Back</button>
  </div>

  <!-- BUY CODE: PAYMENT INSTRUCTIONS -->
  <div id="paymentInstructions" class="hidden card">
    <h2>Pay to this account details before your code</h2>
    <div style="margin-top:8px">
      <div class="small"><b>Acc:</b> yrufyf</div>
      <div class="small"><b>Bank:</b> opay</div>
      <div class="small"><b>Name:</b> frank</div>
      <div class="small"><b>Fee:</b> NGN5,500</div>
    </div>

    <div style="margin-top:12px">
      <button class="btn full" id="confirmPaymentBtn">Confirm Payment</button>
      <button class="go-back" onclick="goBack()">Go Back</button>
    </div>
  </div>

  <!-- PAYMENT PROCESS (20s spinner then failure) -->
  <div id="paymentProcessing" class="hidden card center">
    <div class="loader" aria-hidden="true"></div>
    <div class="small" id="paymentTimerText">Processing payment... 20s</div>
  </div>

  <!-- PAYMENT FAILURE RESULT -->
  <div id="paymentFailed" class="hidden card center">
    <h2 style="color:#d9534f">Sorry — no transaction</h2>
    <p class="small">Fund check the details and try again.</p>
    <button class="go-back" onclick="goBack()">Go Back</button>
  </div>

  <!-- WITHDRAW SUCCESS (not reachable unless correct code used) -->
  <div id="withdrawSuccess" class="hidden card center">
    <h2 style="color:green">Transaction Successful</h2>
    <p class="small">You will receive your alert shortly.</p>
    <button class="go-back" onclick="goBack()">Go Back</button>
  </div>

  <!-- FAQ -->
  <div id="faqPage" class="hidden card">
    <h2>FAQ</h2>
    <p><b>How do I earn?</b><br>Watch videos on the dashboard to increase your demo balance.</p>
    <p><b>How to withdraw?</b><br>Go to Withdraw, fill details and supply the purchase code.</p>
    <button class="go-back" onclick="goBack()">Go Back</button>
  </div>

  <footer>
    Demo site — front end only. No real payments.
  </footer>
</div>

<script>
/* ============================
   Configuration (kept secret)
   ============================ */
const SECRET_PURCHASE_CODE = "cash-tu@2"; // stored but never shown on UI

/* ============================
   Basic UI helpers
   ============================ */
function show(id){ document.getElementById(id).classList.remove('hidden'); }
function hide(id){ document.getElementById(id).classList.add('hidden'); }
function hideAllMain(){ 
  const ids = ["loginScreen","loadingScreen","dashboard","withdrawPage","buyForm","paymentInstructions","paymentProcessing","paymentFailed","withdrawSuccess","faqPage"];
  ids.forEach(hide);
}

/* ============================
   Welcome → show login after 5s
   ============================ */
let startT = 5;
const countdownEl = document.getElementById('countdown');
const intervalWelcome = setInterval(()=>{
  startT--;
  countdownEl.textContent = startT;
  if(startT <= 0){
    clearInterval(intervalWelcome);
    hide('welcomeScreen');
    show('loginScreen');
  }
},1000);

/* ============================
   Login flow
   ============================ */
document.getElementById('loginBtn').addEventListener('click', ()=>{
  const phone = document.getElementById('phone').value.trim();
  const name = document.getElementById('fullname').value.trim();
  const err = document.getElementById('loginError');
  err.style.display='none';
  if(!phone || !/^[0-9]{7,15}$/.test(phone)){
    err.textContent = "Please enter valid phone number (7-15 digits)."; err.style.display='block'; return;
  }
  if(!name){ err.textContent = "Please enter your full name."; err.style.display='block'; return; }
  // show loading spinner for 2s
  hideAllMain(); show('loadingScreen'); document.getElementById('loadingMsg').textContent = "Logging in...";
  setTimeout(()=>{ hide('loadingScreen'); openDashboard(); }, 2000);
});

/* ============================
   Dashboard & Videos
   ============================ */
const VIDEO_COUNT = 6;
const VIDEO_LENGTH = 60; // seconds each
let earningStarted = false;
let balance = 0;
let elapsed = 0;
const TARGET = 59000;
const TOTAL_SECONDS = 20 * 60;
let earningInterval = null;
let watchedCount = 0;

function openDashboard(){
  hideAllMain();
  show('dashboard');
  buildVideos();
  // reset balance display
  balance = 0; elapsed = 0; watchedCount = 0;
  updateBalanceDisplay();
  document.getElementById('watchedCount').textContent = watchedCount;
  document.getElementById('progressPct').textContent = '0%';
}

function buildVideos(){
  const grid = document.getElementById('videosGrid');
  grid.innerHTML = '';
  for(let i=1;i<=VIDEO_COUNT;i++){
    const card = document.createElement('div'); card.className='video-card';
    card.innerHTML = `
      <div style="font-weight:700">Video ${i}</div>
      <div class="small">Duration: 1 minute</div>
      <div style="margin-top:10px">
        <button class="btn full" onclick="playVideo(${i}, this)">Play</button>
      </div>
    `;
    grid.appendChild(card);
  }
}

function playVideo(id, btn){
  // start earning counter once first video started
  if(!earningStarted){
    startEarning();
  }
  // simulate playing for 60s in background (no need to block)
  btn.disabled = true; btn.textContent = 'Playing...';
  setTimeout(()=>{
    btn.textContent = 'Watched';
    btn.disabled = true;
    watchedCount++;
    document.getElementById('watchedCount').textContent = watchedCount;
  }, VIDEO_LENGTH * 1000);
}

/* Earning runs for TOTAL_SECONDS and updates every second */
function startEarning(){
  earningStarted = true;
  const perSecond = TARGET / TOTAL_SECONDS;
  earningInterval = setInterval(()=>{
    elapsed++;
    balance += perSecond;
    if(elapsed >= TOTAL_SECONDS){
      balance = TARGET;
      clearInterval(earningInterval);
    }
    updateBalanceDisplay();
  },1000);
}
function updateBalanceDisplay(){
  const val = balance;
  const formatted = '₦' + Number(val).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  document.getElementById('balanceDisplay').textContent = formatted;
  const pct = Math.min(100, Math.round((balance / TARGET) * 100));
  document.getElementById('progressPct').textContent = pct + '%';
}

/* ============================
   Support button -> open telegram link
   ============================ */
document.getElementById('supportBtn').addEventListener('click', ()=>{
  // open in new tab
  window.open('https://t.me/moniepoint123', '_blank', 'noopener');
});

/* ============================
   FAQ
   ============================ */
document.getElementById('faqBtn').addEventListener('click', ()=>{
  hideAllMain(); show('faqPage');
});

/* ============================
   Withdraw flow (requires SECRET_PURCHASE_CODE)
   ============================ */
document.getElementById('withdrawBtn').addEventListener('click', ()=>{
  hideAllMain(); show('withdrawPage');
});

document.getElementById('doWithdrawBtn').addEventListener('click', ()=>{
  const full = document.getElementById('w_fullname').value.trim();
  const acct = document.getElementById('w_account').value.trim();
  const bank = document.getElementById('w_bank').value.trim();
  const code = document.getElementById('w_code').value.trim();
  const err = document.getElementById('withdrawError'); err.style.display='none';
  if(!full || !acct || !bank || !code){ err.textContent = "Please fill all fields including your purchase code."; err.style.display='block'; return; }
  if(code !== SECRET_PURCHASE_CODE){
    err.textContent = "Invalid purchase code. Withdrawal blocked."; err.style.display='block'; return;
  }
  // correct code -> proceed to loading then success
  hideAllMain(); show('loadingScreen'); document.getElementById('loadingMsg').textContent = "Processing withdrawal...";
  setTimeout(()=>{
    hide('loadingScreen'); show('withdrawSuccess');
  }, 2000);
});

/* ============================
   Buy Code flow
   ============================ */
document.getElementById('buyCodeBtn').addEventListener('click', ()=>{
  hideAllMain(); show('buyForm');
});

document.getElementById('payBtn').addEventListener('click', ()=>{
  const email = document.getElementById('buy_email').value.trim();
  const name = document.getElementById('buy_name').value.trim();
  const be = document.getElementById('buyError'); be.style.display='none';
  if(!email || !name){ be.textContent = "Please enter your email and name."; be.style.display='block'; return; }
  // show 5s loading spinner then payment instructions
  hideAllMain(); show('loadingScreen'); document.getElementById('loadingMsg').textContent = "Redirecting to payment...";
  setTimeout(()=>{
    hide('loadingScreen'); hideAllMain(); show('paymentInstructions');
  }, 5000);
});

document.getElementById('confirmPaymentBtn').addEventListener('click', ()=>{
  // start the 20s spinner countdown
  hideAllMain(); show('paymentProcessing');
  let secondsLeft = 20;
  const timerText = document.getElementById('paymentTimerText');
  timerText.textContent = `Processing payment... ${secondsLeft}s`;
  const paymentTimer = setInterval(()=>{
    secondsLeft--;
    timerText.textContent = `Processing payment... ${secondsLeft}s`;
    if(secondsLeft <= 0){
      clearInterval(paymentTimer);
      hide('paymentProcessing'); show('paymentFailed');
    }
  }, 1000);
});

/* ============================
   Go Back helper (returns to dashboard)
   ============================ */
function goBack(){
  hideAllMain(); show('dashboard');
}

/* Ensure go back buttons exist on other pages too (already inline handlers) */

</script>
</body>
</html>![1000025131](https://github.com/user-attachments/assets/5fc16d04-8d32-47ae-82b6-e445947e42d8)


