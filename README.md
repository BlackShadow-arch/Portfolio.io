<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ali Ahsan | Red Team Security</title>
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@400;600;700&family=Inter:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:        #050a05;
      --bg2:       #0a120a;
      --bg3:       #0d180d;
      --green:     #00ff41;
      --green-dim: #00c732;
      --green-muted: #00ff4122;
      --amber:     #ffb300;
      --red:       #ff3b3b;
      --text:      #c8d8c8;
      --text-dim:  #5a7a5a;
      --border:    #1a3a1a;
      --mono:      'Share Tech Mono', monospace;
      --display:   'Rajdhani', sans-serif;
      --body:      'Inter', sans-serif;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--body);
      font-size: 15px;
      line-height: 1.7;
      overflow-x: hidden;
    }

    /* ── SCANLINE OVERLAY ── */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,255,65,0.015) 2px, rgba(0,255,65,0.015) 4px);
      pointer-events: none;
      z-index: 9999;
    }

    /* ── SCROLLBAR ── */
    ::-webkit-scrollbar { width: 4px; }
    ::-webkit-scrollbar-track { background: var(--bg); }
    ::-webkit-scrollbar-thumb { background: var(--green-dim); border-radius: 2px; }

    /* ── NAV ── */
    nav {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 100;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 2.5rem;
      height: 56px;
      background: rgba(5,10,5,0.92);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
    }

    .nav-logo {
      font-family: var(--mono);
      font-size: 0.9rem;
      color: var(--green);
      letter-spacing: 0.05em;
    }

    .nav-logo span { color: var(--text-dim); }

    .nav-links { display: flex; gap: 2rem; list-style: none; }

    .nav-links a {
      font-family: var(--mono);
      font-size: 0.78rem;
      color: var(--text-dim);
      text-decoration: none;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      transition: color 0.2s;
    }

    .nav-links a:hover { color: var(--green); }

    /* ── HERO ── */
    #hero {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 5rem 2rem 3rem;
      position: relative;
      overflow: hidden;
    }

    /* grid bg */
    #hero::before {
      content: '';
      position: absolute;
      inset: 0;
      background-image:
        linear-gradient(var(--border) 1px, transparent 1px),
        linear-gradient(90deg, var(--border) 1px, transparent 1px);
      background-size: 40px 40px;
      opacity: 0.4;
    }

    /* glow */
    #hero::after {
      content: '';
      position: absolute;
      top: 30%; left: 50%;
      transform: translate(-50%, -50%);
      width: 600px; height: 600px;
      background: radial-gradient(circle, rgba(0,255,65,0.07) 0%, transparent 70%);
      pointer-events: none;
    }

    .hero-inner {
      position: relative;
      z-index: 1;
      max-width: 780px;
      width: 100%;
    }

    .hero-tag {
      font-family: var(--mono);
      font-size: 0.78rem;
      color: var(--green);
      letter-spacing: 0.15em;
      text-transform: uppercase;
      margin-bottom: 1.2rem;
      display: flex;
      align-items: center;
      gap: 0.6rem;
    }

    .hero-tag::before {
      content: '';
      width: 24px; height: 1px;
      background: var(--green);
    }

    h1 {
      font-family: var(--display);
      font-size: clamp(2.8rem, 7vw, 5.5rem);
      font-weight: 700;
      line-height: 1.05;
      letter-spacing: -0.01em;
      color: #fff;
      margin-bottom: 0.3rem;
    }

    h1 .accent { color: var(--green); }

    .hero-sub {
      font-family: var(--mono);
      font-size: 1rem;
      color: var(--text-dim);
      margin-bottom: 1.8rem;
      letter-spacing: 0.05em;
    }

    .hero-sub .cursor {
      display: inline-block;
      width: 10px; height: 1.1em;
      background: var(--green);
      vertical-align: text-bottom;
      animation: blink 1s step-end infinite;
    }

    @keyframes blink { 50% { opacity: 0; } }

    .hero-desc {
      font-size: 1rem;
      color: var(--text);
      max-width: 520px;
      margin-bottom: 2.2rem;
      line-height: 1.8;
    }

    .hero-btns { display: flex; gap: 1rem; flex-wrap: wrap; }

    .btn {
      font-family: var(--mono);
      font-size: 0.8rem;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      text-decoration: none;
      padding: 0.7rem 1.6rem;
      border-radius: 2px;
      transition: all 0.2s;
      display: inline-flex;
      align-items: center;
      gap: 0.5rem;
    }

    .btn-primary {
      background: var(--green);
      color: #000;
      font-weight: 700;
    }

    .btn-primary:hover { background: #00c732; transform: translateY(-1px); }

    .btn-ghost {
      border: 1px solid var(--border);
      color: var(--text-dim);
    }

    .btn-ghost:hover { border-color: var(--green); color: var(--green); }

    /* status bar */
    .status-bar {
      margin-top: 3rem;
      display: flex;
      gap: 2rem;
      flex-wrap: wrap;
    }

    .status-item {
      font-family: var(--mono);
      font-size: 0.75rem;
      color: var(--text-dim);
      display: flex;
      align-items: center;
      gap: 0.4rem;
    }

    .status-dot {
      width: 7px; height: 7px;
      border-radius: 50%;
      background: var(--green);
      box-shadow: 0 0 6px var(--green);
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: 0.4; } }

    /* ── SECTION COMMON ── */
    section { padding: 5rem 2rem; }

    .section-wrap { max-width: 1000px; margin: 0 auto; }

    .section-header { margin-bottom: 3rem; }

    .section-eyebrow {
      font-family: var(--mono);
      font-size: 0.72rem;
      color: var(--green);
      letter-spacing: 0.2em;
      text-transform: uppercase;
      margin-bottom: 0.6rem;
    }

    h2 {
      font-family: var(--display);
      font-size: clamp(1.8rem, 4vw, 2.6rem);
      font-weight: 700;
      color: #fff;
      letter-spacing: -0.01em;
    }

    /* ── ABOUT ── */
    #about { background: var(--bg2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }

    .about-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 3rem;
      align-items: start;
    }

    .terminal {
      background: #030803;
      border: 1px solid var(--border);
      border-radius: 4px;
      overflow: hidden;
    }

    .terminal-bar {
      background: #0d180d;
      padding: 0.5rem 1rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      border-bottom: 1px solid var(--border);
    }

    .t-dot { width: 10px; height: 10px; border-radius: 50%; }
    .t-red { background: #ff5f57; }
    .t-yellow { background: #febc2e; }
    .t-green { background: #28c840; }

    .terminal-title {
      font-family: var(--mono);
      font-size: 0.72rem;
      color: var(--text-dim);
      margin-left: 0.5rem;
    }

    .terminal-body {
      padding: 1.4rem 1.6rem;
      font-family: var(--mono);
      font-size: 0.82rem;
      line-height: 1.9;
    }

    .t-prompt { color: var(--green); }
    .t-cmd { color: #fff; }
    .t-key { color: var(--amber); }
    .t-val { color: var(--text); }
    .t-comment { color: var(--text-dim); }
    .t-string { color: #7ec8e3; }

    .about-text p {
      color: var(--text);
      margin-bottom: 1.2rem;
      line-height: 1.9;
    }

    .about-text p:last-child { margin-bottom: 0; }

    .highlight { color: var(--green); font-weight: 500; }

    /* ── SKILLS ── */
    #skills { background: var(--bg); }

    .skills-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1.2rem;
    }

    .skill-card {
      background: var(--bg2);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 1.4rem 1.6rem;
      transition: border-color 0.2s, transform 0.2s;
      position: relative;
      overflow: hidden;
    }

    .skill-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0;
      width: 3px; height: 100%;
      background: var(--green);
      opacity: 0;
      transition: opacity 0.2s;
    }

    .skill-card:hover { border-color: var(--green); transform: translateY(-2px); }
    .skill-card:hover::before { opacity: 1; }

    .skill-card-head {
      display: flex;
      align-items: center;
      gap: 0.8rem;
      margin-bottom: 1rem;
    }

    .skill-icon {
      width: 36px; height: 36px;
      background: var(--green-muted);
      border-radius: 4px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.1rem;
    }

    .skill-card-title {
      font-family: var(--display);
      font-size: 1rem;
      font-weight: 600;
      color: #fff;
      letter-spacing: 0.03em;
    }

    .skill-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 0.4rem;
    }

    .tag {
      font-family: var(--mono);
      font-size: 0.7rem;
      padding: 0.2rem 0.6rem;
      background: rgba(0,255,65,0.07);
      border: 1px solid rgba(0,255,65,0.15);
      color: var(--green-dim);
      border-radius: 2px;
      letter-spacing: 0.04em;
    }

    /* ── PROJECTS ── */
    #projects { background: var(--bg2); border-top: 1px solid var(--border); }

    .projects-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 1.2rem;
    }

    .project-card {
      background: var(--bg3);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 1.6rem;
      display: flex;
      flex-direction: column;
      gap: 0.8rem;
      transition: border-color 0.2s, transform 0.2s;
      text-decoration: none;
    }

    .project-card:hover { border-color: var(--green); transform: translateY(-2px); }

    .project-card-top {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
    }

    .project-week {
      font-family: var(--mono);
      font-size: 0.7rem;
      color: var(--green);
      letter-spacing: 0.1em;
    }

    .severity {
      font-family: var(--mono);
      font-size: 0.65rem;
      padding: 0.15rem 0.5rem;
      border-radius: 2px;
      letter-spacing: 0.06em;
    }

    .sev-critical { background: rgba(255,59,59,0.12); color: var(--red); border: 1px solid rgba(255,59,59,0.25); }
    .sev-high { background: rgba(255,179,0,0.1); color: var(--amber); border: 1px solid rgba(255,179,0,0.25); }
    .sev-medium { background: rgba(0,255,65,0.07); color: var(--green-dim); border: 1px solid rgba(0,255,65,0.15); }

    .project-title {
      font-family: var(--display);
      font-size: 1.05rem;
      font-weight: 600;
      color: #fff;
      line-height: 1.3;
    }

    .project-desc {
      font-size: 0.85rem;
      color: var(--text-dim);
      line-height: 1.7;
      flex: 1;
    }

    .project-tools {
      display: flex;
      flex-wrap: wrap;
      gap: 0.35rem;
    }

    .project-link {
      font-family: var(--mono);
      font-size: 0.72rem;
      color: var(--green);
      text-decoration: none;
      display: flex;
      align-items: center;
      gap: 0.3rem;
      margin-top: 0.3rem;
      transition: gap 0.2s;
    }

    .project-card:hover .project-link { gap: 0.6rem; }

    /* capstone card */
    .project-card.capstone {
      grid-column: 1 / -1;
      background: linear-gradient(135deg, #0a180a, #0d1f0d);
      border-color: rgba(0,255,65,0.2);
      flex-direction: row;
      align-items: center;
      gap: 2rem;
    }

    .capstone-icon {
      font-size: 3rem;
      flex-shrink: 0;
    }

    .capstone-body { flex: 1; }

    /* ── FOOTER ── */
    footer {
      background: var(--bg);
      border-top: 1px solid var(--border);
      padding: 2.5rem 2rem;
      text-align: center;
    }

    .footer-inner { max-width: 1000px; margin: 0 auto; }

    .footer-logo {
      font-family: var(--mono);
      font-size: 1rem;
      color: var(--green);
      margin-bottom: 0.5rem;
    }

    .footer-sub {
      font-family: var(--mono);
      font-size: 0.72rem;
      color: var(--text-dim);
      letter-spacing: 0.08em;
    }

    .footer-disclaimer {
      margin-top: 1.5rem;
      font-size: 0.75rem;
      color: var(--text-dim);
      max-width: 540px;
      margin-left: auto;
      margin-right: auto;
      line-height: 1.6;
    }

    /* ── RESPONSIVE ── */
    @media (max-width: 680px) {
      .about-grid { grid-template-columns: 1fr; }
      .project-card.capstone { flex-direction: column; }
      .nav-links { display: none; }
    }
  </style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="nav-logo">&gt; BlackShadow<span>.sh</span></div>
  <ul class="nav-links">
    <li><a href="#about">About</a></li>
    <li><a href="#skills">Skills</a></li>
    <li><a href="#projects">Projects</a></li>
  </ul>
</nav>

<!-- HERO -->
<section id="hero">
  <div class="hero-inner">
    <div class="hero-tag">Offensive Security Researcher</div>
    <h1>Ali <span class="accent">Ahsan</span></h1>
    <p class="hero-sub">root@kali:~$ <span class="cursor"></span></p>
    <p class="hero-desc">
      Cybersecurity enthusiast specializing in penetration testing, 
      Active Directory attacks, and cloud security. 
      Passionate about breaking things ethically and understanding how systems fail.
    </p>
    <div class="hero-btns">
      <a href="#projects" class="btn btn-primary">&#x25B6; View Projects</a>
      <a href="https://github.com/BlackShadow-arch" target="_blank" class="btn btn-ghost">&#x25A1; GitHub Profile</a>
    </div>
    <div class="status-bar">
      <div class="status-item"><span class="status-dot"></span> Available for opportunities</div>
      
      <div class="status-item">&#x1F512; Ethical Hacker</div>
    </div>
  </div>
</section>

<!-- ABOUT -->
<section id="about">
  <div class="section-wrap">
    <div class="section-header">
      <p class="section-eyebrow">// whoami</p>
      <h2>About Me</h2>
    </div>
    <div class="about-grid">
      <div class="terminal">
        <div class="terminal-bar">
          <span class="t-dot t-red"></span>
          <span class="t-dot t-yellow"></span>
          <span class="t-dot t-green"></span>
          <span class="terminal-title">whoami.sh</span>
        </div>
        <div class="terminal-body">
<span class="t-comment"># Operator Profile</span><br>
<br>
<span class="t-key">name</span>     = <span class="t-string">"Ali Ahsan"</span><br>

<span class="t-key">focus</span>    = <span class="t-string">"Offensive Security"</span><br>
<br>
<span class="t-key">skills</span> = [<br>
&nbsp;&nbsp;<span class="t-string">"Penetration Testing"</span>,<br>
&nbsp;&nbsp;<span class="t-string">"Active Directory Attacks"</span>,<br>
&nbsp;&nbsp;<span class="t-string">"Cloud Security"</span>,<br>
&nbsp;&nbsp;<span class="t-string">"Red Teaming"</span>,<br>
&nbsp;&nbsp;<span class="t-string">"AV / EDR Evasion"</span><br>
]<br>
<br>
<span class="t-key">goal</span>   = <span class="t-string">"Breaking things ethically"</span><br>
<span class="t-key">status</span> = <span class="t-string">"Open to opportunities"</span><br>
        </div>
      </div>

      <div class="about-text">
        <p>
          I'm a cybersecurity enthusiast with hands-on experience across the 
          full <span class="highlight">offensive security kill chain</span> — from passive OSINT 
          all the way to cloud infrastructure attacks and full domain compromise.
        </p>
        <p>
          My work covers <span class="highlight">network scanning</span>, 
          <span class="highlight">web application exploitation</span>, 
          <span class="highlight">Active Directory attacks</span>, 
          <span class="highlight">AV/EDR evasion</span>, 
          <span class="highlight">C2 infrastructure</span>, and 
          <span class="highlight">AWS cloud security</span>.
        </p>
        <p>
          I believe real-world security is broken by misconfigurations, not zero-days. 
          My goal is to think like an attacker to help defenders 
          build systems that are actually <span class="highlight">resilient</span>.
        </p>
      </div>
    </div>
  </div>
</section>

<!-- SKILLS -->
<section id="skills">
  <div class="section-wrap">
    <div class="section-header">
      <p class="section-eyebrow">// capabilities</p>
      <h2>Skills & Tools</h2>
    </div>
    <div class="skills-grid">

      <div class="skill-card">
        <div class="skill-card-head">
          <div class="skill-icon">&#x1F50D;</div>
          <div class="skill-card-title">Reconnaissance</div>
        </div>
        <div class="skill-tags">
          <span class="tag">Subfinder</span><span class="tag">Amass</span>
          <span class="tag">Nmap</span><span class="tag">crt.sh</span>
          <span class="tag">Gowitness</span><span class="tag">Google Dorking</span>
          <span class="tag">Enum4linux</span><span class="tag">WhatWeb</span>
        </div>
      </div>

      <div class="skill-card">
        <div class="skill-card-head">
          <div class="skill-icon">&#x1F4A5;</div>
          <div class="skill-card-title">Exploitation</div>
        </div>
        <div class="skill-tags">
          <span class="tag">Metasploit</span><span class="tag">Burp Suite</span>
          <span class="tag">SQLMap</span><span class="tag">FFUF</span>
          <span class="tag">Gobuster</span><span class="tag">WPScan</span>
          <span class="tag">Searchsploit</span><span class="tag">msfvenom</span>
        </div>
      </div>

      <div class="skill-card">
        <div class="skill-card-head">
          <div class="skill-icon">&#x2B06;</div>
          <div class="skill-card-title">Privilege Escalation</div>
        </div>
        <div class="skill-tags">
          <span class="tag">LinPEAS</span><span class="tag">WinPEAS</span>
          <span class="tag">PowerUp</span><span class="tag">LSE</span>
          <span class="tag">PrintSpoofer</span><span class="tag">SUID Abuse</span>
          <span class="tag">EternalBlue</span>
        </div>
      </div>

      <div class="skill-card">
        <div class="skill-card-head">
          <div class="skill-icon">&#x1F3DB;</div>
          <div class="skill-card-title">Active Directory</div>
        </div>
        <div class="skill-tags">
          <span class="tag">BloodHound</span><span class="tag">PowerView</span>
          <span class="tag">Mimikatz</span><span class="tag">Impacket</span>
          <span class="tag">Responder</span><span class="tag">Kerberoasting</span>
          <span class="tag">AS-REP Roasting</span><span class="tag">DCSync</span>
        </div>
      </div>

      <div class="skill-card">
        <div class="skill-card-head">
          <div class="skill-icon">&#x1F47B;</div>
          <div class="skill-card-title">Evasion & C2</div>
        </div>
        <div class="skill-tags">
          <span class="tag">Sliver C2</span><span class="tag">AMSI Bypass</span>
          <span class="tag">LOLBAS</span><span class="tag">GoPhish</span>
          <span class="tag">Invoke-Obfuscation</span><span class="tag">Process Injection</span>
          <span class="tag">Shikata-ga-nai</span>
        </div>
      </div>

      <div class="skill-card">
        <div class="skill-card-head">
          <div class="skill-icon">&#x2601;</div>
          <div class="skill-card-title">Cloud & Containers</div>
        </div>
        <div class="skill-tags">
          <span class="tag">AWS CLI</span><span class="tag">Pacu</span>
          <span class="tag">S3Scanner</span><span class="tag">CloudEnum</span>
          <span class="tag">Docker Escape</span><span class="tag">SSRF</span>
          <span class="tag">IAM Escalation</span>
        </div>
      </div>

    </div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <div class="section-wrap">
    <div class="section-header">
      <p class="section-eyebrow">// operations</p>
      <h2>Security Projects</h2>
    </div>
    <div class="projects-grid">

      <a href="https://github.com/BlackShadow-arch/Passive_OSINT_Subdomain_Enumeration" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 01</span>
          <span class="severity sev-medium">RECON</span>
        </div>
        <div class="project-title">Passive OSINT & Subdomain Enumeration</div>
        <div class="project-desc">Attack surface mapping using passive sources — subdomain discovery, technology profiling, GitHub dorking, and live asset identification.</div>
        <div class="project-tools">
          <span class="tag">Subfinder</span><span class="tag">Amass</span><span class="tag">Gowitness</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/Network_Scanning_Enumeration" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 02</span>
          <span class="severity sev-medium">SCANNING</span>
        </div>
        <div class="project-title">Network Scanning & Enumeration</div>
        <div class="project-desc">Advanced Nmap scanning, service fingerprinting, SMB enumeration, NSE vulnerability scripts, and firewall evasion via packet fragmentation.</div>
        <div class="project-tools">
          <span class="tag">Nmap</span><span class="tag">Enum4linux</span><span class="tag">NSE</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/Web_Vulnerability_Assessment" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 03</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">Web Vulnerability Assessment</div>
        <div class="project-desc">Directory brute-forcing, hidden parameter discovery, CVE mapping via Searchsploit, CMS scanning with WPScan and JoomScan, initial shell via Metasploit.</div>
        <div class="project-tools">
          <span class="tag">FFUF</span><span class="tag">WPScan</span><span class="tag">Metasploit</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/OWASP_Top10_Web_Pentest" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 04</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">OWASP Top 10 Web Pentest</div>
        <div class="project-desc">Hands-on exploitation of SQL Injection, Command Injection, IDOR, Broken Access Control, XSS, and File Inclusion against Mutillidae.</div>
        <div class="project-tools">
          <span class="tag">Burp Suite</span><span class="tag">SQLMap</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/Post_Exploitation_Privilege_Escalation" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 05</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">Post-Exploitation & Privilege Escalation</div>
        <div class="project-desc">Root on Linux via SUID nmap, SYSTEM on Windows 7 via EternalBlue and PrintSpoofer. SSH key and registry persistence. Netcat exfiltration.</div>
        <div class="project-tools">
          <span class="tag">LinPEAS</span><span class="tag">WinPEAS</span><span class="tag">PrintSpoofer</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/Lateral_Movement_Network_Pivoting" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 06</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">Lateral Movement & Network Pivoting</div>
        <div class="project-desc">SSH SOCKS5 tunneling, internal subnet scanning via Proxychains, /etc/shadow dumping, offline hash cracking, and Pass-the-Hash movement.</div>
        <div class="project-tools">
          <span class="tag">Proxychains</span><span class="tag">John the Ripper</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/Active_Directory_Enumeration_Kerberos" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 07</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">Active Directory Enumeration & Kerberos</div>
        <div class="project-desc">Domain mapping with PowerView, BloodHound attack path visualization, AS-REP Roasting, and Kerberoasting against SPN-registered service accounts.</div>
        <div class="project-tools">
          <span class="tag">BloodHound</span><span class="tag">Impacket</span><span class="tag">Hashcat</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/AD_Domain_Dominance" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 08</span>
          <span class="severity sev-critical">CRITICAL</span>
        </div>
        <div class="project-title">AD Domain Dominance</div>
        <div class="project-desc">LLMNR poisoning with Responder, DCSync attack via Mimikatz, KRBTGT hash extraction, and Golden Ticket forgery for 10-year persistent domain access.</div>
        <div class="project-tools">
          <span class="tag">Mimikatz</span><span class="tag">Responder</span><span class="tag">Hashcat</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/AV_EDR_Evasion" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 09</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">AV / EDR Evasion</div>
        <div class="project-desc">AMSI bypass via .NET reflection, fileless in-memory execution, LOLBAS abuse with regsvr32 Squiblydoo, and process injection into notepad.exe.</div>
        <div class="project-tools">
          <span class="tag">msfvenom</span><span class="tag">Invoke-Obfuscation</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/C2_Frameworks_Phishing_Infrastructure" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 10</span>
          <span class="severity sev-high">HIGH</span>
        </div>
        <div class="project-title">C2 Frameworks & Phishing Infrastructure</div>
        <div class="project-desc">Sliver C2 deployment, VBA macro weaponization, LNK shortcut payloads, GoPhish phishing campaigns, and SPF/DKIM/DMARC email security analysis.</div>
        <div class="project-tools">
          <span class="tag">Sliver C2</span><span class="tag">GoPhish</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <a href="https://github.com/BlackShadow-arch/Cloud_Security_Modern_Infrastructure" target="_blank" class="project-card">
        <div class="project-card-top">
          <span class="project-week">LAB 11</span>
          <span class="severity sev-critical">CRITICAL</span>
        </div>
        <div class="project-title">Cloud Security & Modern Infrastructure</div>
        <div class="project-desc">S3 bucket credential harvesting, SSRF to AWS IMDS for IAM theft, Pacu privilege escalation, Docker privileged escape, and socket abuse.</div>
        <div class="project-tools">
          <span class="tag">Pacu</span><span class="tag">S3Scanner</span><span class="tag">AWS CLI</span>
        </div>
        <span class="project-link">View Repo &#x2192;</span>
      </a>

      <!-- CAPSTONE -->
      <a href="https://github.com/BlackShadow-arch/The-Full-Attack-Chain" target="_blank" class="project-card capstone">
        <div class="capstone-icon">&#x2694;</div>
        <div class="capstone-body">
          <div class="project-card-top" style="margin-bottom:0.6rem">
            <span class="project-week">CAPSTONE PROJECT</span>
            <span class="severity sev-critical">DOMAIN ADMIN</span>
          </div>
          <div class="project-title">The Full Attack Chain</div>
          <div class="project-desc" style="margin:0.5rem 0">Complete end-to-end red team engagement — 23 open ports mapped, vsftpd backdoor exploitation, EternalBlue, Pass-the-Hash, DCSync, Golden Ticket forgery, and full domain compromise. All 15 findings mapped to MITRE ATT&CK. No zero-days required.</div>
          <div class="project-tools">
            <span class="tag">Nmap</span><span class="tag">Metasploit</span><span class="tag">Mimikatz</span>
            <span class="tag">BloodHound</span><span class="tag">Impacket</span><span class="tag">MITRE ATT&CK</span>
          </div>
          <span class="project-link" style="margin-top:0.6rem">View Capstone Repo &#x2192;</span>
        </div>
      </a>

    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <div class="footer-logo">&gt; BlackShadow.sh</div>
    <div class="footer-sub">OFFENSIVE SECURITY &nbsp;|&nbsp; 2026</div>
    <p class="footer-disclaimer">
      All techniques documented were performed exclusively in authorized, controlled lab environments 
      using intentionally vulnerable machines. For educational purposes only.
    </p>
  </div>
</footer>

</body>
</html>
