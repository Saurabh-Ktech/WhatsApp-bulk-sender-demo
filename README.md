# WhatsApp-bulk-sender-demo
Backend-driven WhatsApp Bulk Sender with automation using Spring Boot and Selenium
# WhatsApp Bulk Sender (Spring Boot + Selenium)

## Overview

This project is an automation tool built using Spring Boot and Selenium WebDriver to send bulk personalized messages via WhatsApp Web.
##  Demo UI
This repository contains a demo frontend UI for the WhatsApp Bulk Sender.

Backend (Spring Boot + Selenium automation) is kept private.

##  Features
Bulk message sending automation
Personalized messages using Excel/CSV data
Automated WhatsApp Web login handling
Dynamic message processing using backend APIs
Error handling and retry mechanismTech Stack

##  Tech Stack

* Java
* Spring Boot
* Selenium WebDriver

##  Manual vs Automation (Selenium)
## Manual Process
You open each WhatsApp link manually
WhatsApp Web opens in the browser
The message appears in the chat box
You click the Send button
You repeat this process multiple times (e.g., 63 times )

## Automated Process (Using Selenium)
The system automatically opens the browser (Chrome)
Navigates to WhatsApp Web
Locates the message input box and types the message
Automatically clicks the Send button
Moves to the next contact/store and repeats the process
##  Source Code Notice

 Full source code is kept private for security and automation compliance reasons. Access can be provided upon request.

## Author

Saurabh Kashyap
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KUSUM Industries — WhatsApp Bulk Sender</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@600;700&family=Nunito+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --navy:#08111f;
  --navy2:#0e1b30;
  --navy3:#152240;
  --navy4:#1b2a4f;
  --gold:#c8a84b;
  --gold2:#e2c97e;
  --gold3:#f0dba8;
  --gd:rgba(200,168,75,0.14);
  --gg:rgba(200,168,75,0.06);
  --gb:rgba(200,168,75,0.22);
  --white:#f5f0e8;
  --muted:#7a8fad;
  --border:rgba(200,168,75,0.16);
  --border2:rgba(200,168,75,0.38);
  --ok:#27c76f;
  --err:#e05252;
  --r:13px;
  --r2:10px;
}
*{box-sizing:border-box;margin:0;padding:0;}
html{scroll-behavior:smooth;}
body{
  font-family:'Nunito Sans',sans-serif;
  background:var(--navy);
  color:var(--white);
  min-height:100vh;
  background-image:
    radial-gradient(ellipse 60% 40% at 8% 0%,rgba(200,168,75,.06) 0%,transparent 100%),
    radial-gradient(ellipse 50% 35% at 92% 100%,rgba(200,168,75,.04) 0%,transparent 100%);
}

/* ═══ TOPBAR ═══════════════════════════════════ */
.topbar{
  position:sticky;top:0;z-index:200;
  height:62px;
  background:rgba(8,17,31,.94);
  border-bottom:1px solid var(--border);
  backdrop-filter:blur(16px);
  display:flex;align-items:center;
  padding:0 2rem;
  justify-content:space-between;
}
.brand{display:flex;align-items:center;gap:13px;}
.logo-box{
  width:42px;height:42px;border-radius:9px;
  background:var(--gd);
  border:1.5px solid var(--gold);
  display:flex;align-items:center;justify-content:center;
  overflow:hidden;flex-shrink:0;
}
.logo-box img{width:100%;height:100%;object-fit:contain;}
.logo-init{
  font-family:'Cormorant Garamond',serif;
  font-size:22px;font-weight:700;
  color:var(--gold);line-height:1;
}
.brand-info{}
.brand-name{
  font-family:'Cormorant Garamond',serif;
  font-size:17px;font-weight:700;
  color:var(--white);letter-spacing:.4px;
}
.brand-tag{font-size:10.5px;color:var(--muted);letter-spacing:.5px;margin-top:1px;}
.top-right{display:flex;align-items:center;gap:12px;}
.srv-badge{
  display:flex;align-items:center;gap:7px;
  padding:5px 13px;border-radius:100px;
  font-size:11.5px;font-weight:700;
  letter-spacing:.3px;transition:.3s;
}
.srv-ok{background:rgba(39,199,111,.1);color:var(--ok);border:1px solid rgba(39,199,111,.28);}
.srv-err{background:rgba(224,82,82,.1);color:var(--err);border:1px solid rgba(224,82,82,.25);}
.dot{width:6px;height:6px;border-radius:50%;background:currentColor;animation:blink 1.3s ease-in-out infinite;}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.15}}

/* ═══ LAYOUT ════════════════════════════════════ */
.wrap{max-width:800px;margin:0 auto;padding:2.25rem 1.5rem 5rem;}

/* ═══ HERO ══════════════════════════════════════ */
.hero{text-align:center;padding:2.75rem 1rem 2rem;position:relative;}
.hero::before{
  content:'';position:absolute;
  inset:0;pointer-events:none;
  background:radial-gradient(ellipse 55% 60% at 50% 50%,rgba(200,168,75,.07) 0%,transparent 70%);
}
.eyebrow{
  display:inline-flex;align-items:center;gap:9px;
  background:var(--gd);border:1px solid var(--border2);
  border-radius:100px;padding:5px 18px;
  font-size:10px;font-weight:800;letter-spacing:2px;
  color:var(--gold);text-transform:uppercase;
  margin-bottom:1.1rem;
}
.eyebrow-gem{font-size:8px;color:var(--gold2);}
.hero h1{
  font-family:'Cormorant Garamond',serif;
  font-size:clamp(26px,5.5vw,44px);font-weight:700;
  line-height:1.15;color:var(--white);
  margin-bottom:.7rem;letter-spacing:.3px;
}
.hero h1 em{color:var(--gold);font-style:normal;}
.hero-sub{font-size:14px;color:var(--muted);max-width:460px;margin:0 auto;line-height:1.7;}

/* ═══ SECTION DIVIDER ═══════════════════════════ */
.sdiv{
  display:flex;align-items:center;gap:12px;
  margin:1.75rem 0 1.25rem;
  font-size:10px;font-weight:800;
  color:var(--muted);letter-spacing:1.8px;text-transform:uppercase;
}
.sdiv::before,.sdiv::after{content:'';flex:1;height:1px;background:var(--border);}

/* ═══ CARD ══════════════════════════════════════ */
.card{
  background:var(--navy2);
  border:1px solid var(--border);
  border-radius:var(--r);
  padding:1.6rem;
  margin-bottom:1.2rem;
  position:relative;overflow:hidden;
  transition:border-color .25s,transform .2s;
  animation:fadeUp .35s ease both;
}
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}
.card:hover{border-color:var(--border2);}
.card::after{
  content:'';position:absolute;
  top:0;left:20%;right:20%;height:1px;
  background:linear-gradient(90deg,transparent,var(--gold),transparent);
  opacity:0;transition:.3s;
}
.card:hover::after{opacity:.6;}
.card-hd{display:flex;align-items:center;gap:12px;margin-bottom:1.4rem;}
.step-num{
  width:30px;height:30px;border-radius:7px;
  background:var(--gd);border:1px solid var(--gold);
  display:flex;align-items:center;justify-content:center;
  font-family:'Cormorant Garamond',serif;
  font-size:15px;font-weight:700;color:var(--gold);
  flex-shrink:0;
}
.card-title{
  font-family:'Cormorant Garamond',serif;
  font-size:17px;font-weight:700;color:var(--white);
}
.card-sub{font-size:12px;color:var(--muted);margin-top:2px;}

/* ═══ UPLOAD ════════════════════════════════════ */
.drop{
  border:2px dashed rgba(200,168,75,.25);
  border-radius:10px;padding:2.25rem 1.5rem;
  text-align:center;cursor:pointer;
  transition:.22s;background:var(--gg);
}
.drop:hover,.drop.over{border-color:var(--gold);background:var(--gd);}
.drop.done{border-style:solid;border-color:var(--gold);background:var(--gd);}
.d-icon{font-size:40px;margin-bottom:.8rem;display:block;}
.d-main{font-size:14px;font-weight:700;color:var(--white);margin-bottom:4px;}
.d-hint{font-size:12px;color:var(--muted);}
#fi{display:none;}

/* ═══ FORM ══════════════════════════════════════ */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:1rem;}
@media(max-width:500px){.g2{grid-template-columns:1fr;}}
.fl{
  display:block;font-size:10.5px;font-weight:800;
  color:var(--gold);letter-spacing:.9px;
  text-transform:uppercase;margin-bottom:6px;
}
select,input[type=number]{
  width:100%;background:var(--navy3);
  border:1px solid var(--border);color:var(--white);
  padding:10px 13px;border-radius:8px;
  font-size:13.5px;font-family:'Nunito Sans',sans-serif;
  outline:none;cursor:pointer;transition:.2s;
}
select:focus,input[type=number]:focus{border-color:var(--gold);background:var(--navy4);}
select option{background:var(--navy2);}
textarea{
  width:100%;background:var(--navy3);
  border:1px solid var(--border);color:var(--white);
  padding:13px;border-radius:8px;
  font-size:13px;font-family:'Nunito Sans',sans-serif;
  resize:vertical;min-height:140px;outline:none;line-height:1.7;transition:.2s;
}
textarea:focus{border-color:var(--gold);background:var(--navy4);}
.hint{font-size:12px;color:var(--muted);margin-top:8px;line-height:1.65;}
code{
  background:var(--navy);border:1px solid var(--border);
  padding:2px 7px;border-radius:4px;
  color:var(--gold2);font-size:11px;
}
.preview-box{
  margin-top:12px;padding:10px 14px;
  background:var(--navy);border:1px solid var(--border);
  border-radius:8px;font-size:12px;color:var(--muted);line-height:1.7;
}
.preview-box strong{color:var(--gold2);}
.settings-row{display:flex;gap:1rem;flex-wrap:wrap;}
.settings-row .field{flex:1;min-width:150px;}
.info-box{
  margin-top:1rem;padding:13px 15px;
  background:var(--navy);border:1px solid var(--border);
  border-radius:9px;font-size:13px;color:var(--muted);line-height:1.7;
}
.info-box b{color:var(--gold2);}

/* ═══ BUTTONS ═══════════════════════════════════ */
.btn-row{display:flex;gap:9px;flex-wrap:wrap;margin-top:1.25rem;}
.btn{
  display:inline-flex;align-items:center;gap:8px;
  padding:11px 22px;border-radius:9px;
  font-size:13.5px;font-weight:800;
  font-family:'Nunito Sans',sans-serif;
  cursor:pointer;border:none;transition:.16s;
  letter-spacing:.4px;
}
.btn:active{transform:scale(.97);}
.btn:disabled{opacity:.38;cursor:not-allowed;transform:none;}
.btn-gold{
  background:linear-gradient(135deg,var(--gold) 0%,var(--gold2) 100%);
  color:var(--navy);
  box-shadow:0 4px 18px rgba(200,168,75,.28);
}
.btn-gold:hover:not(:disabled){
  background:linear-gradient(135deg,var(--gold2),var(--gold3));
  box-shadow:0 6px 26px rgba(200,168,75,.4);
  transform:translateY(-1px);
}
.btn-danger{
  background:rgba(224,82,82,.12);color:var(--err);
  border:1px solid rgba(224,82,82,.28);
}
.btn-danger:hover{background:rgba(224,82,82,.2);}
.btn-ghost{
  background:var(--navy3);color:var(--muted);
  border:1px solid var(--border);
}
.btn-ghost:hover{color:var(--white);border-color:var(--border2);}

/* ═══ PROGRESS SECTION ══════════════════════════ */
.banner{
  display:flex;align-items:center;gap:10px;
  padding:12px 16px;border-radius:10px;
  font-size:13px;font-weight:600;
  margin-bottom:1.2rem;transition:.3s;
}
.bn-idle{background:var(--navy3);color:var(--muted);border:1px solid var(--border);}
.bn-run{background:rgba(39,199,111,.07);color:var(--ok);border:1px solid rgba(39,199,111,.22);}
.bn-wait{background:var(--gd);color:var(--gold2);border:1px solid var(--border2);}
.bn-done{background:rgba(39,199,111,.12);color:var(--ok);border:1px solid rgba(39,199,111,.35);}
.bn-err{background:rgba(224,82,82,.08);color:var(--err);border:1px solid rgba(224,82,82,.22);}

.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:1.2rem;}
.stat{
  background:var(--navy2);border:1px solid var(--border);
  border-radius:10px;padding:15px 10px;
  text-align:center;position:relative;overflow:hidden;
}
.stat::after{
  content:'';position:absolute;
  bottom:0;left:0;right:0;height:2px;
}
.s-ok::after{background:linear-gradient(90deg,transparent,var(--ok),transparent);}
.s-err::after{background:linear-gradient(90deg,transparent,var(--err),transparent);}
.s-gold::after{background:linear-gradient(90deg,transparent,var(--gold),transparent);}
.stat-n{
  font-family:'Cormorant Garamond',serif;
  font-size:30px;font-weight:700;line-height:1;margin-bottom:4px;
}
.s-ok .stat-n{color:var(--ok);}
.s-err .stat-n{color:var(--err);}
.s-gold .stat-n{color:var(--gold);}
.stat-l{font-size:10px;font-weight:800;color:var(--muted);text-transform:uppercase;letter-spacing:.9px;}

.prog-wrap{
  background:var(--navy3);border-radius:100px;
  height:6px;overflow:hidden;
  margin-bottom:1.2rem;border:1px solid var(--border);
}
.prog-fill{
  height:100%;
  background:linear-gradient(90deg,var(--gold),var(--gold2));
  border-radius:100px;transition:width .4s ease;
}

.delay-bar{height:3px;background:var(--navy3);border-radius:2px;overflow:hidden;margin-bottom:6px;}
.delay-fill{height:100%;background:var(--gold);border-radius:2px;transition:width 1s linear;}
.delay-txt{font-size:12px;color:var(--muted);text-align:center;margin-bottom:12px;}

/* ═══ LOG LIST ══════════════════════════════════ */
.log-list{max-height:390px;overflow-y:auto;padding-right:4px;}
.log-list::-webkit-scrollbar{width:3px;}
.log-list::-webkit-scrollbar-thumb{background:var(--border2);border-radius:3px;}
.li{
  display:flex;align-items:center;gap:10px;
  padding:9px 11px;border-radius:8px;
  margin-bottom:3px;font-size:13px;
  transition:background .2s;
}
.li.pending{color:var(--muted);}
.li.sending{
  background:var(--gg);color:var(--white);
  border:1px solid var(--gb);
  animation:pulse 1.6s ease-in-out infinite;
}
@keyframes pulse{0%,100%{border-color:rgba(200,168,75,.18)}50%{border-color:rgba(200,168,75,.55)}}
.li.sent{background:rgba(39,199,111,.04);color:var(--muted);}
.li.failed{background:rgba(224,82,82,.06);color:var(--err);}
.li-num{min-width:28px;font-size:11px;color:var(--muted);font-weight:800;}
.li-name{flex:1;font-weight:700;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
.li-phone{font-size:11px;color:var(--muted);min-width:118px;}
.li-status{font-size:12px;font-weight:800;min-width:82px;text-align:right;}
.li.sent .li-status{color:var(--ok);}
.li.failed .li-status{color:var(--err);}
.li.sending .li-status{color:var(--gold);}

/* ═══ FOOTER ════════════════════════════════════ */
.footer{
  text-align:center;padding:2.5rem 1rem 1rem;
  color:var(--muted);font-size:12px;
  border-top:1px solid var(--border);margin-top:3rem;
}
.footer-brand{
  font-family:'Cormorant Garamond',serif;
  font-size:16px;font-weight:700;color:var(--gold2);
  margin-bottom:6px;
}
.footer-dividers{
  display:flex;align-items:center;justify-content:center;
  gap:8px;flex-wrap:wrap;margin-top:4px;
}
.footer-dividers span{color:var(--border2);}

.sec{display:none;}.sec.show{display:block;}
/* ═══ LIGHT MODE ══════════════════════════════ */
body.light{
  --navy:#f0ece3;
  --navy2:#ffffff;
  --navy3:#f5f1e8;
  --navy4:#ede8dc;
  --white:#1a1008;
  --muted:#7a6a50;
  --border:rgba(180,140,50,0.2);
  --border2:rgba(180,140,50,0.45);
  --gd:rgba(200,168,75,0.12);
  --gg:rgba(200,168,75,0.06);
  --gb:rgba(200,168,75,0.28);
  background-image:
    radial-gradient(ellipse 60% 40% at 8% 0%,rgba(200,168,75,.08) 0%,transparent 100%),
    radial-gradient(ellipse 50% 35% at 92% 100%,rgba(200,168,75,.05) 0%,transparent 100%);
}
body.light .topbar{background:rgba(240,236,227,.96);}
body.light .card{background:var(--navy2);}
body.light select,body.light input[type=number],body.light textarea{background:var(--navy3);color:var(--white);}
body.light select option{background:#fff;color:#1a1008;}
body.light .preview-box{background:var(--navy3);}
body.light .info-box{background:var(--navy3);}
body.light .drop{background:var(--gg);}
body.light .drop:hover,.light .drop.over,.light .drop.done{background:var(--gd);}
body.light .banner{background:var(--navy3);}
body.light .stat{background:var(--navy2);border-color:var(--border);}
body.light .prog-wrap{background:var(--navy3);}
body.light .delay-bar{background:var(--navy3);}
body.light .li.pending{color:var(--muted);}
body.light .li.sending{background:var(--gd);}
body.light .li.sent{background:rgba(39,199,111,.06);}
body.light .li.failed{background:rgba(224,82,82,.08);}
body.light .btn-ghost{background:var(--navy3);color:var(--muted);}
body.light .btn-ghost:hover{color:var(--white);}
body.light .hero::before{background:radial-gradient(ellipse 55% 60% at 50% 50%,rgba(200,168,75,.1) 0%,transparent 70%);}
body.light .footer{border-color:var(--border);}

/* Theme toggle button */
.theme-btn{
  width:36px;height:36px;border-radius:8px;
  background:var(--gd);
  border:1px solid var(--border2);
  display:flex;align-items:center;justify-content:center;
  cursor:pointer;font-size:16px;
  transition:.2s;flex-shrink:0;
}
.theme-btn:hover{background:var(--gb);}

</style>
</head>
<body>

<!-- ═══ TOPBAR ══════════════════════════════════ -->
<nav class="topbar">
  <div class="brand">
    <div class="logo-box" id="logoBox">
      <!-- To add your logo: replace the span below with: <img src="your_logo.png" alt="Kusum"> -->
      <span class="logo-init">K</span>
    </div>
    <div class="brand-info">
      <div class="brand-name">Kusum Industries</div>
      <div class="brand-tag">Premium Quality Products</div>
    </div>
  </div>
  <div class="top-right">
    <button class="theme-btn" id="themeBtn" onclick="toggleTheme()" title="Toggle Light/Dark Mode">🌙</button>
    <div class="srv-badge srv-err" id="srvBadge">
      <div class="dot"></div>
      <span id="srvTxt">Connecting...</span>
    </div>
  </div>
</nav>

<!-- ═══ MAIN ═════════════════════════════════════ -->
<div class="wrap">

  <!-- HERO -->
  <div class="hero">
    <div class="eyebrow">
      <span class="eyebrow-gem">◆</span>
      WhatsApp Bulk Sender
      <span class="eyebrow-gem">◆</span>
    </div>
    <h1>Reach Every <em>Kirana Store</em><br>Directly &amp; Instantly</h1>
    <p class="hero-sub">Upload your Excel file, compose your message — Selenium will automatically open Chrome, type, and send to every store. Scan QR once, rest is automatic.</p>
  </div>

  <div class="sdiv">Setup</div>

  <!-- STEP 1: UPLOAD -->
  <div class="card">
    <div class="card-hd">
      <div class="step-num">1</div>
      <div>
        <div class="card-title">Upload Excel File</div>
        <div class="card-sub">File containing store names and phone numbers</div>
      </div>
    </div>
    <div class="drop" id="dropZone" onclick="document.getElementById('fi').click()">
      <span class="d-icon" id="dIcon">📊</span>
      <div class="d-main" id="dText">Click here or drag &amp; drop your file</div>
      <div class="d-hint" id="dHint">.xlsx &nbsp;&nbsp; .xls &nbsp;&nbsp; .csv — all supported</div>
    </div>
    <input type="file" id="fi" accept=".xlsx,.xls,.csv" onchange="uploadFile(this.files[0])">
  </div>

  <!-- STEP 2: COLUMN MAPPING -->
  <div class="card sec" id="mapCard">
    <div class="card-hd">
      <div class="step-num">2</div>
      <div>
        <div class="card-title">Map Your Columns</div>
        <div class="card-sub">Select which column has store names and which has phone numbers</div>
      </div>
    </div>
    <div class="g2">
      <div>
        <label class="fl">Store Name Column</label>
        <select id="nameCol" onchange="updatePreview()"></select>
      </div>
      <div>
        <label class="fl">Phone Number Column</label>
        <select id="phoneCol" onchange="updatePreview()"></select>
      </div>
    </div>
    <div id="prevBox" class="preview-box" style="display:none;"></div>
  </div>

  <!-- STEP 3: MESSAGE -->
  <div class="card sec" id="msgCard">
    <div class="card-hd">
      <div class="step-num">3</div>
      <div>
        <div class="card-title">Compose Your Message</div>
        <div class="card-sub">Will be personalised for each store automatically</div>
      </div>
    </div>
    <textarea id="msgTpl">*KUSUM INDUSTRIES*
*Direct Company Supply for Kirana Stores*

Dear {name} ji 🙏

Kusum Industries is now offering Direct Company Supply to select kirana stores.

💰 Higher Retailer Margin than Market
📦 Direct Company Rate — No Distributor, No Wholesaler
🚚 Fast & Reliable Delivery

If you want a high-profit detergent powder for your store, contact us now!</textarea>
    <div class="hint">Use <code>{name}</code> — it will be automatically replaced with each store's name</div>
  </div>

  <!-- STEP 4: SETTINGS + START -->
  <div class="card sec" id="settCard">
    <div class="card-hd">
      <div class="step-num">4</div>
      <div>
        <div class="card-title">Configure &amp; Launch</div>
        <div class="card-sub">Set batch size, delay, then start sending</div>
      </div>
    </div>
    <div class="settings-row">
      <div class="field">
        <label class="fl">Batch Size (stores per group)</label>
        <input type="number" id="batchSz" value="5" min="1" max="20">
      </div>
      <div class="field">
        <label class="fl">Delay Between Batches (seconds)</label>
        <input type="number" id="delay" value="5" min="3" max="60">
      </div>
    </div>
    <div class="info-box">
      <b>How it works:</b> Selenium opens Chrome browser → navigates to WhatsApp Web →
      <b>scan QR code once</b> (first time only) → automatically types message → clicks Send →
      moves to next store. Chrome profile is saved, so you won't need to scan again.
    </div>
    <div class="btn-row">
      <button class="btn btn-gold" id="startBtn" onclick="startSending()">
        &#9654;&nbsp; Start Sending
      </button>
    </div>
  </div>

  <!-- PROGRESS -->
  <div class="sec" id="progSec">

    <div class="sdiv">Live Progress</div>

    <div class="banner bn-idle" id="banner">
      <div class="dot"></div>
      <span id="bannerTxt">Ready...</span>
    </div>

    <div class="stats">
      <div class="stat s-ok">
        <div class="stat-n" id="stSent">0</div>
        <div class="stat-l">Sent</div>
      </div>
      <div class="stat s-err">
        <div class="stat-n" id="stFail">0</div>
        <div class="stat-l">Failed</div>
      </div>
      <div class="stat s-gold">
        <div class="stat-n" id="stLeft">0</div>
        <div class="stat-l">Remaining</div>
      </div>
    </div>

    <div class="prog-wrap">
      <div class="prog-fill" id="progFill" style="width:0%"></div>
    </div>

    <div id="delaySec" style="display:none;">
      <div class="delay-bar"><div class="delay-fill" id="delayFill" style="width:100%"></div></div>
      <div class="delay-txt" id="delayTxt">Next batch incoming...</div>
    </div>

    <div class="btn-row" style="margin-bottom:1.25rem;">
      <button class="btn btn-danger" id="pauseBtn" onclick="togglePause()">&#9646;&#9646;&nbsp; Pause</button>
      <button class="btn btn-ghost" onclick="stopSending()">&#9632;&nbsp; Stop</button>
      <button class="btn btn-ghost" onclick="resetAll()">&#8635;&nbsp; Reset</button>
    </div>

    <!-- LOG -->
    <div class="card" style="padding:1.25rem;">
      <div class="card-hd" style="margin-bottom:1rem;">
        <div class="step-num" style="font-family:sans-serif;font-size:16px;">&#128203;</div>
        <div>
          <div class="card-title">Live Sending Log</div>
          <div class="card-sub">Real-time status for every store</div>
        </div>
      </div>
      <div class="log-list" id="logList"></div>
    </div>

  </div>
</div>

<!-- ═══ FOOTER ════════════════════════════════ -->
<footer class="footer">
  <div class="footer-brand">Kusum Industries</div>
  <div class="footer-dividers">
    <span>WhatsApp Bulk Sender</span>
    <span>&#9670;</span>
    <span>Powered by Spring Boot + Selenium</span>
    <span>&#9670;</span>
    <span style="color:var(--gold);">localhost:8081</span>
  </div>
</footer>

<script>
 alert("This is a demo UI. Backend automation is kept private.");
const API = ''; // Demo mode (no backend)
let isPaused = false, eventSource = null, previewRows = null;
let totalStores = 0, delayTotal = 5;

/* ── Server Check ─────────────────────────── */
async function checkServer() {
  try {
    const r = await fetch(API + '/status', { signal: AbortSignal.timeout(3000) });
    if (r.ok) { setServer(true, 'Server Online'); return true; }
  } catch(e) {}
  setServer(false, 'Server Offline');
  return false;
}
function setServer(ok, txt) {
  const b = document.getElementById('srvBadge');
  b.className = 'srv-badge ' + (ok ? 'srv-ok' : 'srv-err');
  document.getElementById('srvTxt').textContent = txt;
}

/* ── File Upload ──────────────────────────── */
async function uploadFile(file) {
  if (!file) return;
  const ok = await checkServer();
  if (!ok) {
    alert('Server is not running!\nIn Eclipse: Right-click WhatsAppSenderApplication.java → Run As → Java Application');
    return;
  }
  document.getElementById('dText').textContent = 'Uploading...';
  const fd = new FormData();
  fd.append('file', file);
  try {
    const r = await fetch(API + '/upload-excel', { method: 'POST', body: fd });
    const d = await r.json();
    if (d.error) { alert('Error: ' + d.error); return; }

    document.getElementById('dIcon').textContent = '✅';
    document.getElementById('dText').textContent = file.name;
    document.getElementById('dHint').textContent = d.totalRows + ' rows found';
    document.getElementById('dropZone').classList.add('done');

    const nc = document.getElementById('nameCol');
    const pc = document.getElementById('phoneCol');
    nc.innerHTML = pc.innerHTML = '';
    d.columns.forEach(c => {
      nc.innerHTML += `<option value="${c}">${c}</option>`;
      pc.innerHTML += `<option value="${c}">${c}</option>`;
    });
    if (d.detectedNameCol)  nc.value = d.detectedNameCol;
    if (d.detectedPhoneCol) pc.value = d.detectedPhoneCol;

    previewRows = d.preview;
    updatePreview();
    show('mapCard'); show('msgCard'); show('settCard');
  } catch(e) {
    alert('Upload failed: ' + e.message);
    document.getElementById('dText').textContent = 'Error — please try again';
  }
}

function updatePreview() {
  if (!previewRows) return;
  const nc = document.getElementById('nameCol').value;
  const pc = document.getElementById('phoneCol').value;
  const s = previewRows.slice(0,3).map((r,i) =>
    `${i+1}. <strong>${r[nc]||'?'}</strong> — ${r[pc]||'?'}`
  ).join('&nbsp;&nbsp;|&nbsp;&nbsp;');
  const el = document.getElementById('prevBox');
  el.innerHTML = '<strong>Preview:</strong>&nbsp; ' + s;
  el.style.display = 'block';
}

/* ── Start Sending ────────────────────────── */
async function startSending() {
  const ok = await checkServer();
  if (!ok) { alert('Server is not connected!'); return; }

  const nameCol     = encodeURIComponent(document.getElementById('nameCol').value);
  const phoneCol    = encodeURIComponent(document.getElementById('phoneCol').value);
  const msgTemplate = encodeURIComponent(document.getElementById('msgTpl').value.trim());
  const batchSize   = parseInt(document.getElementById('batchSz').value) || 5;
  delayTotal        = parseInt(document.getElementById('delay').value) || 5;

  if (!msgTemplate) { alert('Please write a message first!'); return; }

  show('progSec');
  isPaused = false;
  setPauseBtn(false);
  setBanner('run', 'Initialising — Chrome browser is opening...');

  const url = `${API}/send-messages?nameCol=${nameCol}&phoneCol=${phoneCol}&messageTemplate=${msgTemplate}&batchSize=${batchSize}&delaySeconds=${delayTotal}`;
  if (eventSource) eventSource.close();
  eventSource = new EventSource(url);
  eventSource.onmessage = e => handleEvent(JSON.parse(e.data));
  eventSource.onerror   = () => setBanner('err', 'Connection lost — check server');
}

/* ── SSE Event Handler ────────────────────── */
function handleEvent(ev) {
  const sent  = ev.sent   || 0;
  const fail  = ev.failed || 0;
  const total = ev.total  || totalStores || 1;
  totalStores = total;

  document.getElementById('stSent').textContent = sent;
  document.getElementById('stFail').textContent = fail;
  document.getElementById('stLeft').textContent = ev.remaining ?? Math.max(0, total - sent - fail);

  const pct = ((sent + fail) / total * 100).toFixed(1);
  document.getElementById('progFill').style.width = pct + '%';

  switch(ev.type) {
    case 'STARTED':
    case 'WAITING_QR':
    case 'READY':
      setBanner('run', ev.message);
      break;
    case 'BATCH_START':
      setBanner('run', ev.message);
      document.getElementById('delaySec').style.display = 'none';
      break;
    case 'STORE_SENDING':
      setBanner('run', ev.message);
      document.getElementById('delaySec').style.display = 'none';
      logRow(ev.storeIndex, ev.storeName, ev.storePhone, 'sending', '&#9654; Sending...');
      break;
    case 'STORE_SENT':
      logRow(ev.storeIndex, ev.storeName, ev.storePhone, 'sent', '&#10003; Sent');
      setBanner('run', ev.message);
      break;
    case 'STORE_FAILED':
      logRow(ev.storeIndex, ev.storeName, ev.storePhone, 'failed', '&#10007; Failed');
      setBanner('run', ev.message);
      break;
    case 'WAITING':
      setBanner('wait', ev.message);
      document.getElementById('delaySec').style.display = 'block';
      const pctD = ((ev.waitSeconds||0) / delayTotal * 100);
      document.getElementById('delayFill').style.width = Math.max(0,pctD) + '%';
      document.getElementById('delayTxt').textContent = ev.message;
      break;
    case 'COMPLETED':
      setBanner('done', '&#127881; ' + ev.message);
      document.getElementById('delaySec').style.display = 'none';
      if (eventSource) { eventSource.close(); eventSource = null; }
      break;
    case 'ERROR':
      setBanner('err', '&#10060; ' + ev.message);
      if (eventSource) { eventSource.close(); eventSource = null; }
      break;
  }
}

function logRow(idx, name, phone, status, statusTxt) {
  const list = document.getElementById('logList');
  let row = document.getElementById('li-' + idx);
  if (!row) {
    row = document.createElement('div');
    row.id = 'li-' + idx;
    list.appendChild(row);
  }
  row.className = 'li ' + status;
  row.innerHTML = `
    <span class="li-num">#${idx}</span>
    <span class="li-name">${name || ''}</span>
    <span class="li-phone">${phone || ''}</span>
    <span class="li-status">${statusTxt}</span>`;
  row.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
}

/* ── Controls ─────────────────────────────── */
async function togglePause() {
  if (isPaused) {
    await fetch(API + '/resume', { method: 'POST' });
    isPaused = false;
    setPauseBtn(false);
    setBanner('run', 'Resumed — continuing...');
  } else {
    await fetch(API + '/pause', { method: 'POST' });
    isPaused = true;
    setPauseBtn(true);
    setBanner('wait', 'Paused — press Resume when ready');
  }
}

function setPauseBtn(paused) {
  const btn = document.getElementById('pauseBtn');
  if (paused) {
    btn.innerHTML = '&#9654;&nbsp; Resume';
    btn.className = 'btn btn-gold';
  } else {
    btn.innerHTML = '&#9646;&#9646;&nbsp; Pause';
    btn.className = 'btn btn-danger';
  }
}

async function stopSending() {
  if (!confirm('Stop sending messages?')) return;
  await fetch(API + '/stop', { method: 'POST' });
  if (eventSource) { eventSource.close(); eventSource = null; }
  setBanner('err', 'Sending stopped');
}

function resetAll() {
  if (eventSource) { eventSource.close(); eventSource = null; }
  hide('progSec');
  document.getElementById('logList').innerHTML = '';
  document.getElementById('delaySec').style.display = 'none';
  isPaused = false;
  setPauseBtn(false);
}

function setBanner(type, txt) {
  document.getElementById('banner').className = 'banner bn-' + type;
  document.getElementById('bannerTxt').innerHTML = txt;
}
function show(id) { document.getElementById(id).classList.add('show'); }
function hide(id) { document.getElementById(id).classList.remove('show'); }

/* ── Drag & Drop ──────────────────────────── */
const dz = document.getElementById('dropZone');
dz.addEventListener('dragover',  e => { e.preventDefault(); dz.classList.add('over'); });
dz.addEventListener('dragleave', ()  => dz.classList.remove('over'));
dz.addEventListener('drop', e => {
  e.preventDefault(); dz.classList.remove('over');
  if (e.dataTransfer.files[0]) uploadFile(e.dataTransfer.files[0]);
});


document.getElementById('logoBox').innerHTML = '<img src="logo.jpeg" alt="Kusum Industries">';


/* ── Theme Toggle ─────────────────────────── */
function toggleTheme() {
  const isLight = document.body.classList.toggle('light');
  document.getElementById('themeBtn').textContent = isLight ? '☀️' : '🌙';
  localStorage.setItem('kusum_theme', isLight ? 'light' : 'dark');
}
// Restore saved theme
(function(){
  const saved = localStorage.getItem('kusum_theme');
  if (saved === 'light') {
    document.body.classList.add('light');
    document.getElementById('themeBtn').textContent = '☀️';
  }
})();

/* ── Init ─────────────────────────────────── */
checkServer();
setInterval(checkServer, 12000);
</script>
</body>
</html>
