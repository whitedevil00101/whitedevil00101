<!--
████████████████████████████████████████████████████████████
  RAJAT DEY — GitHub Profile README
  AI/ML Engineer | Full Stack Engineer | Founder @ Nexero
  GitHub: Whitedevil00101
████████████████████████████████████████████████████████████
-->

<div align="center">

<!-- ═══════════════════════════════════════════════════════ -->
<!--              ANIMATED HEADER BANNER                    -->
<!-- ═══════════════════════════════════════════════════════ -->


<style>
  #banner-wrap {
    width: 100%;
    background: #0d1117;
    border-radius: 12px;
    overflow: hidden;
    position: relative;
    font-family: 'Courier New', monospace;
  }
  #neural-canvas { display: block; width: 100%; height: 320px; }
  .banner-text {
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    pointer-events: none;
    z-index: 2;
  }
  .banner-name {
    font-size: 52px;
    font-weight: 800;
    letter-spacing: 4px;
    color: #64ffda;
    text-shadow: 0 0 40px rgba(100,255,218,0.5);
    margin: 0;
    font-family: 'Courier New', monospace;
  }
  .banner-role {
    font-size: 14px;
    color: #8892b0;
    letter-spacing: 3px;
    margin-top: 6px;
    text-transform: uppercase;
  }
  .banner-tags {
    display: flex;
    gap: 8px;
    margin-top: 14px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .tag {
    background: rgba(100,255,218,0.08);
    border: 1px solid rgba(100,255,218,0.3);
    color: #64ffda;
    font-size: 11px;
    padding: 3px 10px;
    border-radius: 20px;
    letter-spacing: 1px;
  }
  .scan-line {
    position: absolute;
    left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(100,255,218,0.4), transparent);
    animation: scan 3s linear infinite;
    pointer-events: none;
    z-index: 3;
  }
  @keyframes scan {
    0% { top: 0%; }
    100% { top: 100%; }
  }
</style>

<div id="banner-wrap">
  <canvas id="neural-canvas"></canvas>
  <div class="scan-line"></div>
  <div class="banner-text">
    <p class="banner-name">RAJAT DEY</p>
    <p class="banner-role">AI/ML Engineer &nbsp;·&nbsp; Full Stack Engineer &nbsp;·&nbsp; Founder @ Nexero</p>
    <div class="banner-tags">
      <span class="tag">EfficientNet-B3</span>
      <span class="tag">BiLSTM/CTC</span>
      <span class="tag">Laravel</span>
      <span class="tag">GCP</span>
      <span class="tag">Anti-Drone</span>
      <span class="tag">ICR Systems</span>
    </div>
  </div>
</div>

<script>
const canvas = document.getElementById('neural-canvas');
const ctx = canvas.getContext('2d');

function resize() {
  canvas.width = canvas.offsetWidth;
  canvas.height = canvas.offsetHeight;
}
resize();

const nodes = [];
const edges = [];
const N = 55;

for (let i = 0; i < N; i++) {
  nodes.push({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    vx: (Math.random() - 0.5) * 0.4,
    vy: (Math.random() - 0.5) * 0.4,
    r: Math.random() * 2.5 + 1,
    pulse: Math.random() * Math.PI * 2,
    pulseSpeed: Math.random() * 0.03 + 0.01,
    layer: Math.floor(Math.random() * 4)
  });
}

const layerColors = ['#64ffda', '#57cbff', '#b488ff', '#ff6b9d'];
const layerGlow   = ['rgba(100,255,218,', 'rgba(87,203,255,', 'rgba(180,136,255,', 'rgba(255,107,157,'];

let frame = 0;
let pulseOrigin = null;
let pulseRadius = 0;
let pulseActive = false;

function firePulse() {
  const src = nodes[Math.floor(Math.random() * nodes.length)];
  pulseOrigin = { x: src.x, y: src.y };
  pulseRadius = 0;
  pulseActive = true;
}
firePulse();
setInterval(firePulse, 2800);

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = '#0d1117';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  for (let i = 0; i < N; i++) {
    for (let j = i + 1; j < N; j++) {
      const dx = nodes[i].x - nodes[j].x;
      const dy = nodes[i].y - nodes[j].y;
      const dist = Math.sqrt(dx * dx + dy * dy);
      const maxDist = 110;
      if (dist < maxDist) {
        const alpha = (1 - dist / maxDist) * 0.35;
        const midLayer = Math.floor((nodes[i].layer + nodes[j].layer) / 2);
        ctx.beginPath();
        ctx.moveTo(nodes[i].x, nodes[i].y);
        ctx.lineTo(nodes[j].x, nodes[j].y);
        ctx.strokeStyle = layerGlow[midLayer] + alpha + ')';
        ctx.lineWidth = 0.6;
        ctx.stroke();
      }
    }
  }

  if (pulseActive) {
    ctx.beginPath();
    ctx.arc(pulseOrigin.x, pulseOrigin.y, pulseRadius, 0, Math.PI * 2);
    const pAlpha = Math.max(0, 0.5 - pulseRadius / 300);
    ctx.strokeStyle = 'rgba(100,255,218,' + pAlpha + ')';
    ctx.lineWidth = 1.5;
    ctx.stroke();
    pulseRadius += 3;
    if (pulseRadius > 300) pulseActive = false;
  }

  for (let i = 0; i < N; i++) {
    const n = nodes[i];
    n.pulse += n.pulseSpeed;
    const glow = (Math.sin(n.pulse) + 1) * 0.5;
    const r = n.r + glow * 1.5;

    ctx.beginPath();
    ctx.arc(n.x, n.y, r + 3, 0, Math.PI * 2);
    ctx.fillStyle = layerGlow[n.layer] + (glow * 0.25) + ')';
    ctx.fill();

    ctx.beginPath();
    ctx.arc(n.x, n.y, r, 0, Math.PI * 2);
    ctx.fillStyle = layerColors[n.layer];
    ctx.fill();

    n.x += n.vx;
    n.y += n.vy;
    if (n.x < 0 || n.x > canvas.width) n.vx *= -1;
    if (n.y < 0 || n.y > canvas.height) n.vy *= -1;
  }

  frame++;
  requestAnimationFrame(draw);
}
draw();
</script>


<!-- Animated Typing Role Banner -->
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=20&pause=1200&color=64FFDA&center=true&vCenter=true&multiline=false&width=750&height=40&lines=Architecting+AI+Systems+at+Scale;EfficientNet-B3+%2B+BiLSTM%2FCTC+on+1.6M+Samples;Full-Stack+%7C+Laravel+%7C+PHP+%7C+Python+%7C+Go;Anti-Drone+%7C+ADAS+%7C+ICR+Systems+Builder;Published+Author+%7C+Hackathon+Top-5+Lead;Founder+%40+Nexero+%E2%80%94+90%25+Cost-Efficient+Delivery" alt="Typing SVG" />

<br/>

<!-- Social Badges -->
[![GitHub](https://img.shields.io/badge/GitHub-Whitedevil00101-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Whitedevil00101)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rajat_Dey-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/rajatdey)
[![Gmail](https://img.shields.io/badge/Email-rajat.dey00101%40gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:rajat.dey00101@gmail.com)
[![Phone](https://img.shields.io/badge/Call-+91--8974814515-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](tel:+918974814515)
[![Location](https://img.shields.io/badge/Vadodara,_Gujarat-India-FF6B35?style=for-the-badge&logo=googlemaps&logoColor=white)](https://maps.google.com/?q=Vadodara,Gujarat)

<br/>

![Profile Views](https://komarev.com/ghpvc/?username=Whitedevil00101&style=for-the-badge&color=64ffda&label=PROFILE+VIEWS)
[![GitHub followers](https://img.shields.io/github/followers/Whitedevil00101?style=for-the-badge&color=64ffda&logo=github)](https://github.com/Whitedevil00101)

</div>

---

## 🧠 Professional Summary

<img align="right" width="340" src="https://github-readme-stats.vercel.app/api?username=Whitedevil00101&show_icons=true&theme=tokyonight&border_radius=12&hide_border=true&bg_color=0d1117&title_color=64ffda&icon_color=64ffda&text_color=ccd6f6" />

```yaml
Name       : Rajat Dey
Role       : AI/ML Engineer | Full Stack Engineer
Location   : Vadodara, Gujarat, India
Education  : BCA (Hons) AI/ML — Parul University (2024–2028)
Focus      : Cost-efficient, high-impact AI & software systems

Strengths  :
  - Architecting AI pipelines under constrained budgets
  - Hybrid Deep Learning: CNN + RNN + CTC architectures
  - Full-Stack delivery (Laravel, React, Go, PHP, Python)
  - 90%+ cost reduction vs market on every project delivered

Status     : Open to SDE | AI/ML | Research Roles
```

<br clear="right"/>

---

## 🏗️ System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                     🤖  AI / ML LAYER                               │
│  EfficientNet-B3 · BiLSTM/CTC · SVM · XGBoost · Isolation Forest   │
│  Computer Vision (ICR/OCR) · NLP · End-to-End ML Pipelines         │
│  Google Vertex AI · Semi-Supervised Model Development               │
├─────────────────────────────────────────────────────────────────────┤
│                  ⚙️  BACKEND & API LAYER                            │
│  Laravel · Flask · PHP · Go · Python · RESTful APIs                 │
│  Razorpay · Shiprocket · Attendance · Payment Modules               │
├─────────────────────────────────────────────────────────────────────┤
│                  ☁️  CLOUD & INFRASTRUCTURE                         │
│  GCP · Google Colab · Docker · Firebase · MySQL 8.4 · GitHub       │
│  Vault (Model Security) · CI/CD · SEO-Optimized Architectures       │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technical Skills

### 🤖 AI / ML
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Vertex AI](https://img.shields.io/badge/Vertex_AI-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)

**Classical ML:** XGBoost · Random Forest · SVM · Isolation Forest · K-Means · DBSCAN  
**Deep Learning:** CNN (EfficientNet-B3, ResNet-50) · RNN (BiLSTM) · CTC · Hybrid Models  
**Specializations:** Computer Vision (ICR/OCR, Object Detection) · NLP · End-to-End ML Pipelines

### 🖥️ Backend & Full Stack
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)
![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)

### ☁️ Cloud, Databases & DevOps
![GCP](https://img.shields.io/badge/GCP-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL_8.4-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Vault](https://img.shields.io/badge/Vault-000000?style=for-the-badge&logo=vault&logoColor=white)

---

## 🚀 Featured Projects

<table>
<tr>
<td width="50%" valign="top">

### 🔤 Gujarati ICR System *(Ongoing — 2026)*
![Status](https://img.shields.io/badge/Status-Active-64ffda?style=flat-square)
![Dataset](https://img.shields.io/badge/Dataset-1.6M_Samples-orange?style=flat-square)

Architecting a **hybrid deep learning ICR system** combining EfficientNet-B3 (visual features) with BiLSTM/CTC (sequence decoding) on a 1.6M character dataset.

- 🎯 Target: **>97% accuracy** baseline
- 🔁 End-to-end pipeline: preprocessing → training → inference
- 🧩 Hybrid architecture: CNN + RNN + CTC

**Stack:** Python · EfficientNet-B3 · BiLSTM · CTC · OpenCV

</td>
<td width="50%" valign="top">

### 🛡️ Anti-Drone Gun System *(NFSU/BSF — 2023-24)*
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)
![Cost Reduction](https://img.shields.io/badge/Cost_Reduction-90%25-red?style=flat-square)

Independently architected a **multi-frequency RF jamming system** targeting 2.4GHz, GPS, and Bluetooth for defense-grade drone neutralization.

- 💰 Designed at **<₹1L** (~90% cost reduction vs market)
- 🏆 **Vadodara Hackathon 5.0 — Top 5 Team Lead**
- 🎯 Scalable deployment architecture for field use

**Stack:** RF Systems · Computer Vision · Hardware Design

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🚗 ADAS Vehicle Planning *(NFU — 2023-24)*
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)
![Team](https://img.shields.io/badge/Team-4_Members-blue?style=flat-square)
![Cost Reduction](https://img.shields.io/badge/Cost_Reduction-70%25-red?style=flat-square)

Led system planning in a **4-member team** to architect a cost-optimized ADAS hardware stack for autonomous vehicle assistance.

- 💰 Reduced projected cost to **<₹10L** (70% below market)
- 🧠 Sensor fusion + real-time CV pipeline design
- 📐 Full hardware + software co-architecture

**Stack:** Computer Vision · Sensor Fusion · Python · Embedded

</td>
<td width="50%" valign="top">

### 🛒 E-commerce Platform *(Ommtonic Marketing — 2025-26)*
![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=flat-square)
![Delivered At](https://img.shields.io/badge/Delivered_At-₹19.5k-yellow?style=flat-square)

Custom Laravel e-commerce platform with full payment and logistics integrations, SEO-optimized architecture, production deployment.

- 💳 **Razorpay** payments + **Shiprocket** logistics
- 📈 SEO-optimized for organic growth
- 💰 **₹19.5k** delivered (~90.3% cost reduction)

**Stack:** Laravel · PHP · MySQL · Razorpay · Shiprocket

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🎓 LMS Platform *(Aaradhya Coaching — 2023-24)*
![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=flat-square)
![Delivered At](https://img.shields.io/badge/Delivered_At-₹26k-yellow?style=flat-square)

Laravel-based **multi-user LMS** with attendance, content delivery, video streaming, and payment modules enabling full digital operations.

- 🎥 Video + attendance + payment — one unified platform
- 💰 Delivered at **₹26k** (~90% cost reduction)
- 👥 Multi-role: Admin, Teacher, Student

**Stack:** Laravel · PHP · MySQL · Video Streaming

</td>
<td width="50%" valign="top">

### 🤖 SVM Spam Classifier *(PLASMID Internship)*
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)
![Accuracy](https://img.shields.io/badge/Baseline-64%25_Accuracy-blue?style=flat-square)

End-to-end **SVM-based spam classification pipeline** on ~5k dataset with complete data preprocessing, feature engineering, and model training.

- 🔄 Full ML pipeline from raw data to deployment
- 📊 64% accuracy — manual → automated filtering
- 🧪 Semi-supervised model development applied

**Stack:** Python · scikit-learn · SVM · NLP · Pandas

</td>
</tr>
</table>

---

## 💼 Entrepreneurship

<div align="center">

### 🚀 Founder — Nexero *(2025–Present)*

</div>

> Founding and operating an IT services firm delivering **Custom Software Development** — LMS platforms, e-commerce systems, and bespoke business applications.

| Metric | Value |
|--------|-------|
| 💰 Cost Efficiency | **90%+ below market** on every project |
| 🔧 Services | Custom Software: LMS, E-commerce, APIs |
| 🏗️ Delivery | End-to-end: Requirements → Architecture → Deployment |
| 📍 Base | Vadodara, Gujarat, India |

---

## 📚 Publication & Recognition

<table>
<tr>
<td width="50%">

### 📖 Published Author
**"Developing a Hacker's Mindset"**  
*BlueRose Publishers — 2023*

![Amazon](https://img.shields.io/badge/Amazon-FF9900?style=for-the-badge&logo=amazon&logoColor=white)
![Flipkart](https://img.shields.io/badge/Flipkart-2874F0?style=for-the-badge&logo=flipkart&logoColor=white)
![Google Books](https://img.shields.io/badge/Google_Books-4285F4?style=for-the-badge&logo=google&logoColor=white)

Distributed on Amazon, Flipkart, and Google Books — exploring the offensive security mindset for builders and defenders.

</td>
<td width="50%">

### 🏆 Achievements

🥇 **Vadodara Hackathon 5.0**  
→ **Top 5 Team · Team Lead**  
→ Domain: Computer Vision / Anti-Drone Technology

🎓 **BCA (Hons) AI/ML** — Sem 4  
→ Parul University, Vadodara (2024–2028)

🔬 **AI/ML Internship — PLASMID**  
→ Semi-supervised model development  
→ SVM spam classification pipeline

</td>
</tr>
</table>

---

## 📊 GitHub Analytics

<div align="center">

<img height="180em" src="https://github-readme-stats.vercel.app/api?username=Whitedevil00101&show_icons=true&theme=tokyonight&include_all_commits=true&count_private=true&border_radius=12&hide_border=true&bg_color=0d1117&title_color=64ffda&icon_color=64ffda&text_color=ccd6f6"/>
<img height="180em" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Whitedevil00101&layout=compact&langs_count=8&theme=tokyonight&border_radius=12&hide_border=true&bg_color=0d1117&title_color=64ffda&text_color=ccd6f6"/>

</div>

<div align="center">

[![GitHub Streak](https://streak-stats.demolab.com?user=Whitedevil00101&theme=tokyonight&border_radius=12&hide_border=true&background=0d1117&ring=64ffda&fire=ff6b35&currStreakLabel=64ffda)](https://git.io/streak-stats)

</div>

<div align="center">

[![Activity Graph](https://github-readme-activity-graph.vercel.app/graph?username=Whitedevil00101&theme=tokyo-night&bg_color=0d1117&color=64ffda&line=64ffda&point=ff6b35&area=true&hide_border=true)](https://github.com/ashutosh00710/github-readme-activity-graph)

</div>

---

## 🏆 GitHub Trophies

<div align="center">

[![trophy](https://github-profile-trophy.vercel.app/?username=Whitedevil00101&theme=tokyonight&column=7&margin-w=8&margin-h=8&no-bg=true&no-frame=true)](https://github.com/ryo-ma/github-profile-trophy)

</div>

---

## 🎯 What I Bring to Your Team

```python
rajat = {
    "core_value"    : "90%+ cost efficiency without sacrificing quality or scale",

    "proven_impact" : [
        "Architected ₹1L anti-drone RF jamming system (vs ₹10L+ market alternatives)",
        "Delivered production LMS at ₹26k and e-commerce at ₹19.5k, end-to-end",
        "Building 1.6M sample Gujarati ICR system targeting >97% accuracy",
        "Hackathon Top-5 Team Lead — Computer Vision / Defense tech domain",
        "Published author — 'Developing a Hacker's Mindset' (BlueRose, 2023)",
    ],

    "ai_ml_depth"   : [
        "Hybrid architectures: EfficientNet-B3 + BiLSTM/CTC",
        "Classical ML: XGBoost, Random Forest, SVM, Isolation Forest",
        "Semi-supervised learning, model security via Vault",
        "Google Vertex AI, Prompt Engineering, GCP deployment",
    ],

    "engineering"   : "Build fast, ship lean, scale smart",

    "open_to"       : [
        "AI/ML Engineer — Research or Applied",
        "Full-Stack Engineer / Backend Engineer",
        "ML Internships — Remote or Hybrid, India",
        "Research Collaborations in CV / NLP / ICR",
    ],
}
```

---

## 📬 Let's Build Something

<div align="center">

> *"I don't just write code — I architect systems that solve real problems at a fraction of the cost."*  
> *— Rajat Dey*

<br/>

[![Email](https://img.shields.io/badge/📧_Email_Me-rajat.dey00101@gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:rajat.dey00101@gmail.com)
[![LinkedIn](https://img.shields.io/badge/💼_Connect-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/rajatdey)
[![GitHub](https://img.shields.io/badge/👾_Follow-Whitedevil00101-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Whitedevil00101)

<br/>

</div>

---

<div align="center">

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:112240,50:0a192f,100:000000&height=120&section=footer&animation=fadeIn" />

*⭐ From [Whitedevil00101](https://github.com/Whitedevil00101) — Built with precision, shipped with purpose.*

</div>
