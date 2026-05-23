<!DOCTYPE html>
<html lang="en" id="html-root">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Spyridon Tsilimpokos</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Space+Grotesk:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap"
    rel="stylesheet" />
  <style>
    *,
    *::before,
    *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    /*  DESIGN TOKENS ─ */
    :root {
      --bg: #ffffff;
      --bg2: #f7f8fa;
      --bg3: #eef0f4;
      --border: #e2e7ef;
      --border2: #c9d3df;
      --text: #0f1623;
      --text2: #2d3a4a;
      --muted: #60728a;
      --faint: #96a5b8;
      --accent: #2563eb;
      --accent-bg: #eff6ff;
      --accent-bdr: #bfdbfe;
      --green: #059669;
      --amber: #c57c14;
      --dark-bg: #0c111c;
      --dark-bdr: #1e2d42;
      --dark-text: #c5d3e8;
      --dark-muted: #4a607a;
      --pad: max(1.5rem, 4vw);
      --max: 1600px;
      --r: 6px;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Inter', sans-serif;
      line-height: 1.6;
      overflow-x: hidden;
    }

    /*  BACKGROUND CANVAS ─ */
    #binary-canvas {
      position: fixed;
      inset: 0;
      z-index: 0;
      pointer-events: none;
      transition: transform 0.1s ease-out;
    }

    /*  HUD ─ */
    #horizon-hud {
      position: fixed;
      bottom: 1.8rem;
      right: 1.8rem;
      z-index: 800;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 5px;
      opacity: 0;
      transition: opacity 0.4s;
    }

    #horizon-hud.visible {
      opacity: 1;
    }

    #horizon-canvas {
      border-radius: 50%;
    }

    #horizon-label {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.6rem;
      color: var(--faint);
      letter-spacing: 0.08em;
    }

    @media(max-width:600px) {
      #horizon-hud {
        display: none;
      }
    }

    #progress-bar {
      position: fixed;
      top: 0;
      left: 0;
      height: 2px;
      width: 0%;
      background: var(--accent);
      z-index: 9999;
      transition: width 0.08s linear;
    }

    /*  NAV ─ */
    nav {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      z-index: 900;
      height: 58px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 var(--pad);
      background: rgba(255, 255, 255, 0);
      border-bottom: 1px solid transparent;
      transition: background 0.25s, border-color 0.25s;
    }

    nav.scrolled {
      background: rgba(255, 255, 255, 0.92);
      backdrop-filter: blur(12px);
      border-color: var(--border);
    }

    .nav-logo {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1rem;
      font-weight: 600;
      color: var(--text);
      text-decoration: none;
      letter-spacing: -0.01em;
    }

    .nav-right {
      display: flex;
      align-items: center;
      gap: 2rem;
    }

    .nav-links {
      display: flex;
      gap: 2.2rem;
      list-style: none;
    }

    .nav-links a {
      font-size: 0.84rem;
      font-weight: 500;
      color: var(--muted);
      text-decoration: none;
      transition: color 0.15s;
    }

    .nav-links a:hover,
    .nav-links a.active {
      color: var(--text);
    }

    .lang-btn {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.08em;
      padding: 0.22rem 0.65rem;
      border: 1px solid var(--border2);
      border-radius: 3px;
      background: transparent;
      color: var(--muted);
      cursor: pointer;
      transition: all 0.2s;
    }

    .lang-btn:hover {
      border-color: var(--text);
      color: var(--text);
    }

    .hamburger {
      display: none;
      flex-direction: column;
      gap: 5px;
      background: none;
      border: none;
      cursor: pointer;
      padding: 4px;
    }

    .hamburger span {
      display: block;
      width: 22px;
      height: 1.5px;
      background: var(--text);
      transition: transform 0.25s, opacity 0.25s;
    }

    .hamburger.open span:nth-child(1) {
      transform: translateY(6.5px) rotate(45deg);
    }

    .hamburger.open span:nth-child(2) {
      opacity: 0;
    }

    .hamburger.open span:nth-child(3) {
      transform: translateY(-6.5px) rotate(-45deg);
    }

    .mobile-menu {
      display: none;
      position: fixed;
      inset: 58px 0 0 0;
      background: rgba(255, 255, 255, 0.97);
      backdrop-filter: blur(12px);
      z-index: 850;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 2.5rem;
    }

    .mobile-menu.open {
      display: flex;
    }

    .mobile-menu a {
      color: var(--text);
      text-decoration: none;
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.5rem;
      font-weight: 600;
      transition: color 0.2s;
    }

    .mobile-menu a:hover {
      color: var(--accent);
    }

    @media(max-width:768px) {
      .nav-links {
        display: none;
      }

      .hamburger {
        display: flex;
      }
    }

    /*  LAYOUT  */
    .container {
      max-width: var(--max);
      margin: 0 auto;
      padding: 0 var(--pad);
    }

    section {
      padding: 100px 0;
      position: relative;
      z-index: 1;
    }

    .sh {
      display: flex;
      align-items: center;
      gap: 1.2rem;
      margin-bottom: 3.5rem;
    }

    .sh-label {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      color: var(--faint);
      letter-spacing: 0.1em;
    }

    .sh-title {
      font-family: 'Space Grotesk', sans-serif;
      font-size: clamp(1.5rem, 2.6vw, 2rem);
      font-weight: 700;
      color: var(--text);
      letter-spacing: -0.02em;
    }

    .sh-rule {
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    .reveal {
      opacity: 0;
      transform: translateY(18px);
      transition: opacity 0.55s ease, transform 0.55s ease;
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .btn {
      display: inline-flex;
      align-items: center;
      gap: 0.45rem;
      padding: 0.6rem 1.4rem;
      border-radius: var(--r);
      font-size: 0.88rem;
      font-weight: 500;
      text-decoration: none;
      border: 1px solid;
      transition: all 0.18s;
      cursor: pointer;
    }

    .btn-primary {
      background: var(--text);
      border-color: var(--text);
      color: #fff;
    }

    .btn-primary:hover {
      background: var(--text2);
      border-color: var(--text2);
    }

    .btn-ghost {
      background: transparent;
      border-color: var(--border2);
      color: var(--muted);
    }

    .btn-ghost:hover {
      border-color: var(--text);
      color: var(--text);
    }

    /*  HERO  */
    #hero {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding-top: 58px;
    }

    .hero-eyebrow {
      display: flex;
      align-items: center;
      gap: 0.7rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.78rem;
      color: var(--faint);
      letter-spacing: 0.1em;
      margin-bottom: 2rem;
    }

    .location-pin {
      display: inline-block;
      font-size: 0.85rem;
      animation: pin-bob 3s ease-in-out infinite;
    }

    @keyframes pin-bob {

      0%,
      100% {
        transform: translateY(0);
      }

      50% {
        transform: translateY(-3px);
      }
    }


    .hero-name {
      font-family: 'Space Grotesk', sans-serif;
      font-size: clamp(3.2rem, 8.5vw, 7.2rem);
      font-weight: 700;
      color: var(--text);
      letter-spacing: -0.04em;
      line-height: 1.0;
      margin-bottom: 1.2rem;
    }

    .hero-role {
      font-size: clamp(1rem, 2.2vw, 1.4rem);
      color: var(--muted);
      min-height: 1.8em;
      margin-bottom: 1.8rem;
    }

    .hero-role .typed {
      color: var(--accent);
      font-weight: 500;
    }

    .cursor {
      display: inline-block;
      width: 2px;
      height: 0.9em;
      background: var(--accent);
      margin-left: 2px;
      vertical-align: middle;
      animation: blink 0.85s step-end infinite;
    }

    @keyframes blink {
      50% {
        opacity: 0;
      }
    }

    .hero-body {
      display: grid;
      grid-template-columns: 1fr 1fr;
      align-items: end;
      gap: 4rem;
      margin-top: 2.5rem;
    }

    @media(max-width:768px) {
      .hero-body {
        grid-template-columns: 1fr;
        gap: 2rem;
      }
    }

    .hero-desc {
      font-size: 1rem;
      color: var(--muted);
      line-height: 1.75;
    }

    .hero-desc strong {
      color: var(--text2);
      font-weight: 500;
    }

    .hero-right {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 1.2rem;
    }

    .hero-pills {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
    }

    .pill {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      padding: 0.22rem 0.65rem;
      border-radius: 3px;
      border: 1px solid var(--border2);
      color: var(--muted);
      letter-spacing: 0.06em;
      background: var(--bg2);
    }

    .hero-cta {
      display: flex;
      gap: 0.75rem;
      flex-wrap: wrap;
    }

    .hero-divider {
      width: 100%;
      height: 1px;
      background: var(--border);
      margin-top: 5rem;
    }

    /*  ABOUT ─ */
    #about {
      background: var(--bg2);
    }

    .about-grid {
      display: grid;
      grid-template-columns: 1.6fr 1fr;
      gap: 6rem;
      align-items: start;
    }

    @media(max-width:900px) {
      .about-grid {
        grid-template-columns: 1fr;
        gap: 3rem;
      }
    }

    .about-text p {
      font-size: 0.97rem;
      color: var(--muted);
      line-height: 1.8;
      margin-bottom: 1rem;
    }

    .about-text p:last-child {
      margin-bottom: 0;
    }

    .about-text strong {
      color: var(--text2);
      font-weight: 500;
    }

    .stat-stack {
      display: flex;
      flex-direction: column;
      gap: 1px;
    }

    .stat-item {
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: var(--r);
      padding: 1.2rem 1.5rem;
      display: flex;
      align-items: baseline;
      gap: 1rem;
      transition: border-color 0.2s;
    }

    .stat-item:not(:first-child) {
      margin-top: -1px;
      border-radius: 0;
    }

    .stat-item:first-child {
      border-radius: var(--r) var(--r) 0 0;
    }

    .stat-item:last-child {
      border-radius: 0 0 var(--r) var(--r);
    }

    .stat-item:hover {
      border-color: var(--border2);
      z-index: 1;
      position: relative;
    }

    .stat-val {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.6rem;
      font-weight: 700;
      color: var(--text);
      flex-shrink: 0;
    }

    .stat-unit {
      font-size: 1rem;
      color: var(--muted);
      font-weight: 400;
    }

    .stat-lbl {
      font-size: 0.82rem;
      color: var(--muted);
    }

    /*  PROJECTS  */
    #projects {
      background: var(--bg);
    }

    .proj-featured {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }

    @media(max-width:900px) {
      .proj-featured {
        grid-template-columns: 1fr;
      }
    }

    .proj-row4 {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }

    @media(max-width:1100px) {
      .proj-row4 {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media(max-width:560px) {
      .proj-row4 {
        grid-template-columns: 1fr;
      }
    }

    .proj-row3 {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }

    @media(max-width:900px) {
      .proj-row3 {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media(max-width:560px) {
      .proj-row3 {
        grid-template-columns: 1fr;
      }
    }

    .proj-card {
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: var(--r);
      padding: 1.8rem;
      display: flex;
      flex-direction: column;
      gap: 0.9rem;
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    .proj-card:hover {
      border-color: var(--border2);
      box-shadow: 0 8px 28px rgba(0, 0, 0, 0.07);
    }

    .proj-card.featured {
      padding: 2.2rem;
    }

    .proj-type {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.67rem;
      color: var(--faint);
      letter-spacing: 0.1em;
      padding: 0.18rem 0.5rem;
      border: 1px solid var(--border);
      border-radius: 3px;
      width: fit-content;
      background: var(--bg2);
    }

    .proj-icon {
      font-family: 'JetBrains Mono', monospace;
      font-size: 1rem;
      color: var(--faint);
    }

    .proj-title {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.05rem;
      font-weight: 600;
      color: var(--text);
      line-height: 1.25;
    }

    .proj-title.lg {
      font-size: 1.2rem;
    }

    .proj-sub {
      font-size: 0.78rem;
      color: var(--accent);
      font-family: 'JetBrains Mono', monospace;
      letter-spacing: 0.02em;
    }

    .proj-desc {
      font-size: 0.875rem;
      color: var(--muted);
      line-height: 1.7;
      flex: 1;
    }

    .proj-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 0.35rem;
      margin-top: auto;
    }

    .tag {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.68rem;
      padding: 0.15rem 0.5rem;
      border-radius: 3px;
      border: 1px solid var(--border);
      color: var(--muted);
      background: var(--bg2);
    }

    .proj-mini-strip {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      border: 1px solid var(--border);
      border-radius: var(--r);
      overflow: hidden;
    }

    @media(max-width:900px) {
      .proj-mini-strip {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media(max-width:560px) {
      .proj-mini-strip {
        grid-template-columns: 1fr;
      }
    }

    .proj-mini {
      padding: 1.1rem 1.3rem;
      border-right: 1px solid var(--border);
      background: var(--bg2);
      transition: background 0.2s;
    }

    .proj-mini:last-child {
      border-right: none;
    }

    .proj-mini:hover {
      background: var(--bg3);
    }

    .proj-mini-title {
      font-size: 0.84rem;
      font-weight: 500;
      color: var(--text2);
      margin-bottom: 0.3rem;
      display: flex;
      align-items: center;
      gap: 0.4rem;
    }

    .proj-mini-icon {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.75rem;
      color: var(--faint);
    }

    .proj-mini-desc {
      font-size: 0.77rem;
      color: var(--muted);
      line-height: 1.55;
    }

    @media(max-width:900px) {
      .proj-mini:nth-child(2n) {
        border-right: none;
      }

      .proj-mini:nth-child(n+3) {
        border-top: 1px solid var(--border);
      }
    }

    @media(max-width:560px) {
      .proj-mini {
        border-right: none;
        border-top: 1px solid var(--border);
      }

      .proj-mini:first-child {
        border-top: none;
      }
    }

    .proj-note {
      margin-top: 2rem;
      padding: 1rem 1.4rem;
      border: 1px dashed var(--border2);
      border-radius: var(--r);
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      color: var(--faint);
      letter-spacing: 0.05em;
      line-height: 1.6;
    }

    /*  SKILLS  */
    #skills {
      background: var(--bg2);
    }

    .skills-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1.5rem;
    }

    @media(max-width:900px) {
      .skills-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media(max-width:560px) {
      .skills-grid {
        grid-template-columns: 1fr;
      }
    }

    .skill-group {
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: var(--r);
      padding: 1.4rem;
      transition: border-color 0.2s;
    }

    .skill-group:hover {
      border-color: var(--border2);
    }

    .skill-group-label {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.68rem;
      color: var(--faint);
      letter-spacing: 0.1em;
      margin-bottom: 1rem;
    }

    .skill-chips {
      display: flex;
      flex-wrap: wrap;
      gap: 0.4rem;
    }

    .chip {
      font-size: 0.8rem;
      padding: 0.22rem 0.65rem;
      border-radius: 4px;
      border: 1px solid var(--border);
      color: var(--text2);
      background: var(--bg2);
      transition: border-color 0.2s, background 0.2s;
    }

    .chip:hover {
      border-color: var(--accent);
      background: var(--accent-bg);
      color: var(--accent);
    }

    /*  EDUCATION ─ */
    #education {
      background: var(--bg);
    }

    .edu-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.5rem;
      margin-bottom: 3rem;
    }

    @media(max-width:768px) {
      .edu-grid {
        grid-template-columns: 1fr;
      }
    }

    .edu-card {
      border: 1px solid var(--border);
      border-radius: var(--r);
      padding: 2rem;
      background: var(--bg);
      display: flex;
      flex-direction: column;
      gap: 0.9rem;
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    .edu-card:hover {
      border-color: var(--border2);
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
    }

    .edu-status {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.68rem;
      color: var(--faint);
      letter-spacing: 0.08em;
    }

    .dot-live {
      width: 14px;
      height: 14px;
      border-radius: 50%;
      border: 1.5px solid rgba(0, 238, 255, 0.22);
      border-top-color: var(--green);
      flex-shrink: 0;
      animation: spin-live 0.9s linear infinite;
    }

    @keyframes spin-live {
      to {
        transform: rotate(360deg);
      }
    }

    .dot-done {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: var(--faint);
    }

    .edu-degree {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.15rem;
      font-weight: 600;
      color: var(--text);
      line-height: 1.2;
    }

    .edu-school {
      font-size: 0.84rem;
      color: var(--accent);
    }

    .edu-detail {
      font-size: 0.83rem;
      color: var(--muted);
      line-height: 1.7;
    }

    .edu-detail strong {
      color: var(--text2);
      font-weight: 500;
    }

    .achieve-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      border: 1px solid var(--border);
      border-radius: var(--r);
      overflow: hidden;
    }

    @media(max-width:900px) {
      .achieve-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    @media(max-width:560px) {
      .achieve-grid {
        grid-template-columns: 1fr;
      }
    }

    .achieve-item {
      padding: 1.4rem 1.6rem;
      border-right: 1px solid var(--border);
      background: var(--bg2);
      transition: background 0.2s;
    }

    .achieve-item:last-child {
      border-right: none;
    }

    .achieve-item:hover {
      background: var(--bg3);
    }

    .achieve-icon {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.8rem;
      color: var(--faint);
      margin-bottom: 0.5rem;
    }

    .achieve-title {
      font-size: 0.87rem;
      font-weight: 500;
      color: var(--text2);
    }

    .achieve-sub {
      font-size: 0.74rem;
      color: var(--muted);
      margin-top: 0.2rem;
      line-height: 1.4;
    }

    @media(max-width:900px) {
      .achieve-item:nth-child(2n) {
        border-right: none;
      }

      .achieve-item:nth-child(n+3) {
        border-top: 1px solid var(--border);
      }
    }

    @media(max-width:560px) {
      .achieve-item {
        border-right: none;
        border-top: 1px solid var(--border);
      }

      .achieve-item:first-child {
        border-top: none;
      }
    }

    /*  CONTACT ─ */
    #contact {
      background: var(--dark-bg);
      padding: 120px 0;
    }

    #contact .sh-label {
      color: var(--dark-muted);
    }

    #contact .sh-title {
      color: var(--dark-text);
    }

    #contact .sh-rule {
      background: var(--dark-bdr);
    }

    .contact-layout {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 6rem;
      align-items: start;
    }

    @media(max-width:900px) {
      .contact-layout {
        grid-template-columns: 1fr;
        gap: 3rem;
      }
    }

    .contact-desc {
      font-size: 0.97rem;
      color: var(--dark-muted);
      line-height: 1.75;
    }

    .contact-desc strong {
      color: var(--dark-text);
      font-weight: 500;
    }

    .contact-heading {
      font-size: clamp(1.8rem, 3.5vw, 2.8rem);
      font-family: 'Space Grotesk', sans-serif;
      font-weight: 700;
      color: var(--dark-text);
      line-height: 1.15;
      letter-spacing: -0.02em;
      margin-bottom: 1.5rem;
    }

    .contact-links {
      display: flex;
      flex-direction: column;
      gap: 0.75rem;
    }

    .contact-link {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 1rem 1.3rem;
      border: 1px solid var(--dark-bdr);
      border-radius: var(--r);
      text-decoration: none;
      color: var(--dark-text);
      font-size: 0.88rem;
      font-weight: 500;
      transition: all 0.2s;
      background: rgba(255, 255, 255, 0.02);
    }

    .contact-link:hover {
      border-color: rgba(255, 255, 255, 0.18);
      background: rgba(255, 255, 255, 0.05);
    }

    .contact-link-left {
      display: flex;
      align-items: center;
      gap: 0.7rem;
    }

    .contact-link svg {
      opacity: 0.5;
    }

    .contact-arrow {
      font-size: 0.75rem;
      color: var(--dark-muted);
    }

    footer {
      background: var(--dark-bg);
      border-top: 1px solid var(--dark-bdr);
      padding: 1.5rem var(--pad);
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 0.5rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      color: var(--dark-muted);
    }

    footer a {
      color: var(--dark-muted);
      text-decoration: none;
      transition: color 0.2s;
    }

    footer a:hover {
      color: var(--dark-text);
    }

    ::-webkit-scrollbar {
      width: 5px;
    }

    ::-webkit-scrollbar-track {
      background: var(--bg2);
    }

    ::-webkit-scrollbar-thumb {
      background: var(--border2);
      border-radius: 3px;
    }

    ::-webkit-scrollbar-thumb:hover {
      background: var(--muted);
    }
  </style>
</head>

<body>

  <canvas id="binary-canvas"></canvas>
  <div id="horizon-hud">
    <canvas id="horizon-canvas" width="68" height="68"></canvas>
    <div id="horizon-label">ALT 000</div>
  </div>
  <div id="progress-bar"></div>

  <nav id="navbar">
    <a href="#hero" class="nav-logo">Spyridon T.</a>
    <div class="nav-right">
      <ul class="nav-links">
        <li><a href="#about" data-i18n="nav_about">About</a></li>
        <li><a href="#projects" data-i18n="nav_projects">Projects</a></li>
        <li><a href="#skills" data-i18n="nav_skills">Skills</a></li>
        <li><a href="#education" data-i18n="nav_education">Education</a></li>
        <li><a href="#contact" data-i18n="nav_contact">Contact</a></li>
      </ul>
      <button class="lang-btn" id="langBtn">EL</button>
      <button class="hamburger" id="hamburger" aria-label="Menu"><span></span><span></span><span></span></button>
    </div>
  </nav>

  <div class="mobile-menu" id="mobileMenu">
    <a href="#about" class="mob-link" data-i18n="nav_about">About</a>
    <a href="#projects" class="mob-link" data-i18n="nav_projects">Projects</a>
    <a href="#skills" class="mob-link" data-i18n="nav_skills">Skills</a>
    <a href="#education" class="mob-link" data-i18n="nav_education">Education</a>
    <a href="#contact" class="mob-link" data-i18n="nav_contact">Contact</a>
  </div>

  <!-- HERO -->
  <section id="hero">
    <div class="container">
      <div class="hero-eyebrow reveal">
        <span class="location-pin">📍</span>
        <span data-i18n="hero_eyebrow">Athens, Greece &nbsp;&middot;&nbsp; MSc Candidate &nbsp;&middot;&nbsp; Open to
          opportunities</span>
      </div>
      <h1 class="hero-name reveal">Spyridon<br>Tsilimpokos</h1>
      <p class="hero-role reveal"><span class="typed" id="typed"></span><span class="cursor"></span></p>
      <div class="hero-body reveal">
        <p class="hero-desc" data-i18n="hero_desc">I build autonomous systems end-to-end, from <strong>bare-metal drone
            firmware in C++</strong> to <strong>self-supervised LSTM models for GPS-denied UAV swarm recovery</strong>.
          Fresh out of university, looking for the right team to build something meaningful.</p>
        <div class="hero-right">
          <div class="hero-pills">
            <span class="pill" data-i18n="pill_msc">MSC CANDIDATE</span>
            <span class="pill" data-i18n="pill_drone">DRONE BUILDER</span>
            <span class="pill" data-i18n="pill_ml">ML RESEARCH</span>
          </div>
          <div class="hero-cta">
            <a href="#projects" class="btn btn-primary" data-i18n="btn_projects">View Projects</a>
            <a href="https://github.com/Tsilispyr" target="_blank" rel="noopener" class="btn btn-ghost">
              <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor">
                <path
                  d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
              </svg>
              GitHub
            </a>
          </div>
        </div>
      </div>
      <div class="hero-divider reveal"></div>
    </div>
  </section>

  <!-- ABOUT -->
  <section id="about">
    <div class="container">
      <div class="sh reveal">
        <span class="sh-label">01</span>
        <h2 class="sh-title" data-i18n="sec_about">About</h2>
        <div class="sh-rule"></div>
      </div>
      <div class="about-grid">
        <div class="about-text reveal">
          <p data-i18n="about_p1">I'm a Computer Science graduate from IHU with a <strong>BSc grade of 7.05/10</strong>,
            currently finishing my MSc at Harokopeio University. My thesis tackles connectivity prediction and path
            recovery in GPS-denied UAV swarms, combining self-supervised LSTM dead reckoning with PPO reinforcement
            learning. Submission is roughly 4 months out.</p>
          <p data-i18n="about_p2">The drone work goes all the way down: I assembled a quadcopter from bare components,
            wrote its flight controller firmware in C++ from scratch (Madgwick AHRS, cascade PID, DShot600 via DMA),
            then built the Android telemetry app for it in Godot 4 with a full MAVLink v2 FSM parser.</p>
          <p data-i18n="about_p3">On the software side I've delivered full-stack web platforms, agentic RAG pipelines,
            Kubernetes-backed CI/CD infrastructure, real-time audio DSP tools, and graph-based multi-agent SAR
            simulations. I work most naturally at the intersection of <strong>embedded systems, applied ML, and backend
              engineering</strong>.</p>
          <p data-i18n="about_p4">I'm strongly AI-driven in how I work, using it as a force multiplier for ideation,
            analysis, and implementation, while grounding decisions in the underlying theory. My instinct is to find
            practical improvements, orchestrate systems that hold together, and make things accessible even to less
            experienced users. In a workplace, I aim to put real skills to the test, learn the team's workflow deeply,
            and grow from there.</p>
        </div>
        <div class="stat-stack reveal">
          <div class="stat-item">
            <div class="stat-val">7.05<span class="stat-unit">/10</span></div>
            <div class="stat-lbl" data-i18n="stat_bsc_lbl">BSc Final Grade</div>
          </div>
          <div class="stat-item">
            <div class="stat-val">10</div>
            <div class="stat-lbl" data-i18n="stat_msc_lbl">MSc Courses Completed</div>
          </div>
          <div class="stat-item">
            <div class="stat-val">12<span class="stat-unit">+</span></div>
            <div class="stat-lbl" data-i18n="stat_proj_lbl">Projects Built</div>
          </div>
          <div class="stat-item">
            <div class="stat-val">~4<span class="stat-unit">mo</span></div>
            <div class="stat-lbl" data-i18n="stat_sub_lbl">to MSc Submission</div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- PROJECTS -->
  <section id="projects">
    <div class="container">
      <div class="sh reveal">
        <span class="sh-label">02</span>
        <h2 class="sh-title" data-i18n="sec_projects">Projects</h2>
        <div class="sh-rule"></div>
      </div>

      <div class="proj-featured reveal">
        <div class="proj-card featured">
          <div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:0.5rem">
            <span class="proj-type" data-i18n="ptype_thesis">RESEARCH, ONGOING</span>
            <span class="proj-icon">[*]</span>
          </div>
          <div class="proj-title lg" data-i18n="ptitle_thesis">MSc Thesis - AI_Recovery</div>
          <div class="proj-sub" data-i18n="psub_thesis">Data-driven Connectivity Prediction and Path Recovery in SD-UAV
            Networks</div>
          <p class="proj-desc" data-i18n="pdesc_thesis">Self-supervised LSTM dead reckoning (2-layer, 64 hidden units,
            14-feature 9-axis IMU input) trained on 544K sequences at 240Hz, val loss 0.2264. PPO reinforcement learning
            agent for swarm recovery in a Godot 4 3-drone simulation, bridged via UDP at 10Hz. 4-run domain-shift
            experiment quantifies 47% degradation when mixing GPS-degree and metre-scale targets.</p>
          <div class="proj-tags"><span class="tag">PyTorch</span><span class="tag">LSTM</span><span class="tag">PPO /
              RL</span><span class="tag">Godot 4</span><span class="tag">MAVLink</span><span
              class="tag">Self-supervised</span></div>
        </div>
        <div class="proj-card featured">
          <div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:0.5rem">
            <span class="proj-type" data-i18n="ptype_atlas">HARDWARE - PERSONAL</span>
            <span class="proj-icon">[#]</span>
          </div>
          <div class="proj-title lg" data-i18n="ptitle_atlas">Project Atlas - Drone From Scratch</div>
          <div class="proj-sub" data-i18n="psub_atlas">Full hardware build, custom C++ firmware, flight testing</div>
          <p class="proj-desc" data-i18n="pdesc_atlas">FlyfishRC Atlas 4 LR deadcat frame (173mm), Teensy 4.1 (600MHz
            Cortex-M7), dual IMU (MPU6050 + ICM-20948), BMP280, QMC5883L, GM10 GPS, GEPRC Taker E55 ESC, ELRS EP1 Dual @
            420kbaud. Custom firmware: Madgwick6DOF AHRS (&beta;=0.04), cascade PID (100Hz angle / 1kHz rate loop via
            IntervalTimer ISR), DShot600 over DMA.</p>
          <div class="proj-tags"><span class="tag">C++</span><span class="tag">Teensy 4.1</span><span
              class="tag">DShot600 / DMA</span><span class="tag">AHRS</span><span class="tag">Cascade PID</span><span
              class="tag">CRSF</span></div>
        </div>
      </div>

      <div class="proj-row4 reveal">
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_telemetry">ANDROID APP</span>
          <div class="proj-icon">[&gt;]</div>
          <div class="proj-title" data-i18n="ptitle_telemetry">Atlas Telemetry</div>
          <div class="proj-sub" data-i18n="psub_telemetry">Companion app for Project Atlas</div>
          <p class="proj-desc" data-i18n="pdesc_telemetry">Godot 4 APK (v1.1, 116 MB). MAVLink v2 FSM parser with
            CRC-16/MCRF4XX. Three modes: USB-C VCP, WiFi UDP 14550, Bluetooth SPP. Retro VFD aesthetic, vector
            artificial horizon, 300-sample voltage graph (30s at 10Hz), 20-segment current bar (0&ndash;40A).</p>
          <div class="proj-tags"><span class="tag">Godot 4</span><span class="tag">GDScript</span><span
              class="tag">MAVLink v2</span><span class="tag">Android</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_paperpilot">AI / RAG, MSC COURSE</span>
          <div class="proj-icon">[*]</div>
          <div class="proj-title" data-i18n="ptitle_paperpilot">PaperPilot</div>
          <div class="proj-sub" data-i18n="psub_paperpilot">Agentic RAG over research papers</div>
          <p class="proj-desc" data-i18n="pdesc_paperpilot">LangGraph ReAct agent over 25 ArXiv papers (360K tokens,
            Qdrant). Section-aware chunking, BAAI/bge-reranker-base reranking, arxiv_search fallback tool. RAGAS: Tool
            Call Accuracy +10% (v1&rarr;v2), Context Precision +23%. Docker stack with Langfuse, Chainlit UI.</p>
          <div class="proj-tags"><span class="tag">LangGraph</span><span class="tag">Qdrant</span><span
              class="tag">RAGAS</span><span class="tag">GPT-4.1-mini</span><span class="tag">Docker</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_sar">FULL-STACK SIMULATION</span>
          <div class="proj-icon">[~]</div>
          <div class="proj-title" data-i18n="ptitle_sar">SAR-UGV Platform</div>
          <div class="proj-sub" data-i18n="psub_sar">Multi-agent SAR on real maps</div>
          <p class="proj-desc" data-i18n="pdesc_sar">FastAPI backend, osmnx graph routing, risk-weighted A* with tiered
            enemy penalties, cKDTree O(log n). Four simultaneous agents, 18 REST endpoints, 0.25&times;&ndash;100&times;
            playback speed, WGS-84 to UTM projection.</p>
          <div class="proj-tags"><span class="tag">FastAPI</span><span class="tag">osmnx</span><span
              class="tag">A*</span><span class="tag">NetworkX</span><span class="tag">Python</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_devops">CLOUD / DEVOPS, MSC COURSE</span>
          <div class="proj-icon">[^]</div>
          <div class="proj-title" data-i18n="ptitle_devops">DevOps-Pets</div>
          <div class="proj-sub" data-i18n="psub_devops">K8s CI/CD with Azure deployment</div>
          <p class="proj-desc" data-i18n="pdesc_devops">Kubernetes (Kind) provisioned via Ansible, automated Jenkins
            CI/CD, PostgreSQL, MinIO, MailHog, HTTPS via cert-manager + Let's Encrypt. Azure VM and AKS cloud
            deployment. Single-command setup for local, VM, or cloud environments.</p>
          <div class="proj-tags"><span class="tag">Kubernetes</span><span class="tag">Ansible</span><span
              class="tag">Jenkins</span><span class="tag">Azure</span><span class="tag">Docker</span></div>
        </div>
      </div>

      <div class="proj-row3 reveal">
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_audioweb">FULL-STACK, SELF-HOSTED</span>
          <div class="proj-icon">[@]</div>
          <div class="proj-title" data-i18n="ptitle_audioweb">AudioWeb</div>
          <div class="proj-sub" data-i18n="psub_audioweb">Self-hosted music streaming</div>
          <p class="proj-desc" data-i18n="pdesc_audioweb">Flask 3.0, PostgreSQL, MinIO, Docker Compose. yt-dlp with
            bgutil headless Chromium PO-token auth, 320kbps MP3. React 18/Vite frontend, 10-band EQ (&plusmn;12dB, Q=1.2
            biquad), 10 Canvas visualisation modes, 2048-sample FFT.</p>
          <div class="proj-tags"><span class="tag">Flask</span><span class="tag">React 18</span><span
              class="tag">PostgreSQL</span><span class="tag">Docker</span><span class="tag">Web Audio API</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_netflix">ML, MSC COURSE</span>
          <div class="proj-icon">[*]</div>
          <div class="proj-title" data-i18n="ptitle_netflix">Netflix Recommendation System</div>
          <div class="proj-sub" data-i18n="psub_netflix">Content-based filtering with Explainable AI</div>
          <p class="proj-desc" data-i18n="pdesc_netflix">TF-IDF over director, cast, genre, and description on 8000+
            Netflix titles. Cosine similarity with 3x genre weighting, sanitised cast names to resolve metadata overlap.
            K-Means (15 clusters) + PCA 2D visualisation maps the full catalogue. XAI layer surfaces why each
            recommendation fired (shared director, cast, or genre). Age-rating classification explored to 61% ceiling,
            explained as a data quality ceiling rather than model failure.</p>
          <div class="proj-tags"><span class="tag">scikit-learn</span><span class="tag">TF-IDF</span><span
              class="tag">K-Means</span><span class="tag">PCA</span><span class="tag">XAI</span><span
              class="tag">Python</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_bigdata">DATA ANALYSIS, GITHUB</span>
          <div class="proj-icon">[=]</div>
          <div class="proj-title" data-i18n="ptitle_bigdata">Big Data Analysis</div>
          <div class="proj-sub" data-i18n="psub_bigdata">Graph network analysis and large-scale EDA</div>
          <p class="proj-desc" data-i18n="pdesc_bigdata">Multi-layer graph network analysis with hub detection,
            community discovery, and degree distribution profiling. Hub nodes (blue), intermediate nodes (pink), and
            leaf nodes (green) visualised with force-directed layout. Includes large-scale dataset EDA, aggregation
            pipelines, and statistical summaries across heterogeneous data sources.</p>
          <div class="proj-tags"><span class="tag">NetworkX</span><span class="tag">Python</span><span class="tag">Graph
              Analysis</span><span class="tag">EDA</span><span class="tag">Data Mining</span></div>
        </div>
      </div>

      <div class="proj-row3 reveal">
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_datamine">DATA MINING, MSC COURSE</span>
          <div class="proj-icon">[=]</div>
          <div class="proj-title" data-i18n="ptitle_datamine">Passenger Traffic Prediction</div>
          <div class="proj-sub" data-i18n="psub_datamine">SF Airport, 38&thinsp;893 records, 2000-2022</div>
          <p class="proj-desc" data-i18n="pdesc_datamine">5-class classification on historical airport data. KNIME
            pipeline: IQR outlier removal, stratified 70/30 split, seven algorithms benchmarked. Best ensemble (Random
            Forest + GBT voting): <strong>78.9% accuracy</strong>.</p>
          <div class="proj-tags"><span class="tag">KNIME</span><span class="tag">Random Forest</span><span
              class="tag">GBT</span><span class="tag">Ensemble</span><span class="tag">Classification</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_attica">STATISTICS, MSC COURSE</span>
          <div class="proj-icon">[-]</div>
          <div class="proj-title" data-i18n="ptitle_attica">Attica Health Study Analysis</div>
          <div class="proj-sub" data-i18n="psub_attica">20-year epidemiological cohort, CVD risk modelling</div>
          <p class="proj-desc" data-i18n="pdesc_attica">R-based statistical analysis of the ATTICA Study cohort.
            Logistic regression for 10-year and 20-year CVD outcomes, Pearson correlation heatmap, boxplots stratified
            by outcome, skewness and VIF multicollinearity diagnostics, pROC for ROC/AUC evaluation. Auto-generated
            multi-page PDF report via ggplot2, with safe-mode package bootstrapping for reproducibility.</p>
          <div class="proj-tags"><span class="tag">R</span><span class="tag">ggplot2</span><span
              class="tag">pROC</span><span class="tag">Logistic Regression</span><span class="tag">VIF</span></div>
        </div>
        <div class="proj-card">
          <span class="proj-type" data-i18n="ptype_bscthesis">ACADEMIC, BSC THESIS</span>
          <div class="proj-icon">[~]</div>
          <div class="proj-title" data-i18n="ptitle_bscthesis">BSc Thesis - SAR Simulation</div>
          <div class="proj-sub" data-i18n="psub_bscthesis">Grade 10/10, multi-vehicle cooperative SAR</div>
          <p class="proj-desc" data-i18n="pdesc_bscthesis">Python/Pygame cooperative simulation (~1&thinsp;763 lines).
            25&times;20 grid, three vehicle types, six cooperative scenarios, A* with terrain costs, three search
            patterns (Expanding Square, Sector, Parallel Track), fog-of-war, JSON stats export.</p>
          <div class="proj-tags"><span class="tag">Python</span><span class="tag">Pygame</span><span
              class="tag">A*</span><span class="tag">Multi-agent</span><span class="tag">Simulation</span></div>
        </div>
      </div>

      <div class="proj-mini-strip reveal">
        <div class="proj-mini">
          <div class="proj-mini-title"><span class="proj-mini-icon">[+]</span> <span data-i18n="mini_vis_title">Python
              Audio Visualiser</span></div>
          <p class="proj-mini-desc" data-i18n="mini_vis_desc">WASAPI loopback, NumPy rfft 2048-sample, 200 bars
            20Hz-22kHz, 60FPS Pygame.</p>
        </div>
        <div class="proj-mini">
          <div class="proj-mini-title"><span class="proj-mini-icon">[*]</span> <span data-i18n="mini_yolo_title">YOLOv8
              Live Detection</span></div>
          <p class="proj-mini-desc" data-i18n="mini_yolo_desc">YOLOv8l, OpenCV 1280&times;720, agnostic NMS, polygon
            zone monitoring via Supervision.</p>
        </div>
        <div class="proj-mini">
          <div class="proj-mini-title"><span class="proj-mini-icon">[@]</span> <span data-i18n="mini_yt_title">YouTube
              Downloader</span></div>
          <p class="proj-mini-desc" data-i18n="mini_yt_desc">HTML/Flask front-end for yt-dlp, YouTube &amp; YouTube
            Music, playlists, 320kbps MP3.</p>
        </div>
        <div class="proj-mini">
          <div class="proj-mini-title"><span class="proj-mini-icon">[...]</span> <span data-i18n="mini_more_title">More
              on the way</span></div>
          <p class="proj-mini-desc" data-i18n="mini_more_desc">NLP, deep learning, computer vision, and additional MSc
            coursework not yet published.</p>
        </div>
      </div>

      <p class="proj-note reveal" data-i18n="proj_note">[ Several projects are still in progress or not yet uploaded.
        NLP assignments, deep learning experiments, graph and network analysis coursework, and more are coming. ]</p>
    </div>
  </section>

  <!-- SKILLS -->
  <section id="skills">
    <div class="container">
      <div class="sh reveal">
        <span class="sh-label">03</span>
        <h2 class="sh-title" data-i18n="sec_skills">Skills</h2>
        <div class="sh-rule"></div>
      </div>
      <div class="skills-grid reveal">
        <div class="skill-group">
          <div class="skill-group-label" data-i18n="sg_lang">LANGUAGES</div>
          <div class="skill-chips"><span class="chip">Python</span><span class="chip">C++</span><span
              class="chip">GDScript</span><span class="chip">JavaScript</span><span class="chip">Java</span><span
              class="chip">R</span><span class="chip">SQL</span></div>
        </div>
        <div class="skill-group">
          <div class="skill-group-label" data-i18n="sg_ai">AI &amp; MACHINE LEARNING</div>
          <div class="skill-chips"><span class="chip">PyTorch</span><span class="chip">LSTM</span><span class="chip">PPO
              / RL</span><span class="chip">LangGraph / RAG</span><span class="chip">YOLOv8</span><span
              class="chip">scikit-learn</span><span class="chip">NumPy</span><span class="chip">OpenCV</span></div>
        </div>
        <div class="skill-group">
          <div class="skill-group-label" data-i18n="sg_sys">SYSTEMS &amp; EMBEDDED</div>
          <div class="skill-chips"><span class="chip">Teensy 4.1</span><span class="chip">DShot600 / DMA</span><span
              class="chip">IMU Fusion</span><span class="chip">MAVLink v2</span><span class="chip">CRSF /
              ELRS</span><span class="chip">I2C / SPI</span><span class="chip">Cascade PID</span></div>
        </div>
        <div class="skill-group">
          <div class="skill-group-label" data-i18n="sg_back">BACKEND &amp; DATA</div>
          <div class="skill-chips"><span class="chip">FastAPI</span><span class="chip">Flask</span><span
              class="chip">PostgreSQL</span><span class="chip">MinIO</span><span class="chip">NetworkX</span><span
              class="chip">osmnx</span><span class="chip">REST APIs</span></div>
        </div>
        <div class="skill-group">
          <div class="skill-group-label" data-i18n="sg_cloud">CLOUD &amp; DEVOPS</div>
          <div class="skill-chips"><span class="chip">Kubernetes</span><span class="chip">Docker</span><span
              class="chip">Ansible</span><span class="chip">Jenkins</span><span class="chip">Azure VM / AKS</span><span
              class="chip">Linux</span><span class="chip">Git</span></div>
        </div>
        <div class="skill-group">
          <div class="skill-group-label" data-i18n="sg_front">FRONTEND &amp; TOOLS</div>
          <div class="skill-chips"><span class="chip">React 18 / Vite</span><span class="chip">Web Audio API</span><span
              class="chip">Godot 4</span><span class="chip">Pygame</span><span class="chip">Canvas API</span><span
              class="chip">KNIME</span><span class="chip">Android APK</span></div>
        </div>
      </div>
    </div>
  </section>

  <!-- EDUCATION -->
  <section id="education">
    <div class="container">
      <div class="sh reveal">
        <span class="sh-label">04</span>
        <h2 class="sh-title" data-i18n="sec_edu">Education</h2>
        <div class="sh-rule"></div>
      </div>
      <div class="edu-grid reveal">
        <div class="edu-card">
          <div class="edu-status"><span class="dot-live"></span> <span data-i18n="edu_msc_status">ONGOING</span></div>
          <div class="edu-degree" data-i18n="edu_msc_degree">MSc in Computer Science</div>
          <div class="edu-school" data-i18n="edu_msc_school">Harokopeio University of Athens (HUA)</div>
          <div class="edu-detail" data-i18n="edu_msc_detail">Sep 2025 - present &nbsp;&middot;&nbsp; submission ~Sep
            2026<br><br>Thesis: <strong>Data-driven Connectivity Prediction and Path Recovery in SD-UAV Networks using
              Self-Supervised Learning</strong><br><br>Courses: Deep Learning, Computer Vision, NLP &amp; Info
            Retrieval, Graph &amp; Network Analysis, Machine Learning, Data Mining &amp; Recommender Systems,
            Applications of DS &amp; AI, Cloud Platforms, Big Data Analysis, Statistics, plus thesis.</div>
        </div>
        <div class="edu-card">
          <div class="edu-status"><span class="dot-done"></span> <span data-i18n="edu_bsc_status">COMPLETED</span></div>
          <div class="edu-degree" data-i18n="edu_bsc_degree">BSc in Computer Science</div>
          <div class="edu-school" data-i18n="edu_bsc_school">Harokopio University of Athens (HUA)</div>
          <div class="edu-detail" data-i18n="edu_bsc_detail">Sep 2021 - Sep 2025 &nbsp;&middot;&nbsp; <strong>Grade
              7.05/10</strong><br><br>Thesis: <strong>Σχεδιασμός αλγορίθμων διαχείρισης για μη επανδρωμένα οχήματα για
              επιχειρήσεις έρευνας και διάσωσης</strong><br>(Algorithm Design for UAV Management in SAR
            Operations)<br><br>Thesis grade: <strong>10/10</strong></div>
        </div>
      </div>
      <p style="font-family:'JetBrains Mono',monospace;font-size:0.7rem;color:var(--faint);letter-spacing:0.1em;margin-bottom:1rem;"
        class="reveal" data-i18n="sec_achieve">ACHIEVEMENTS</p>
      <div class="achieve-grid reveal">
        <div class="achieve-item">
          <div class="achieve-icon">[*]</div>
          <div class="achieve-title" data-i18n="ach1_title">G. Karabatzos Scholarship</div>
          <div class="achieve-sub" data-i18n="ach1_sub">Harokopeio University, awarded for MSc studies</div>
        </div>
        <div class="achieve-item">
          <div class="achieve-icon">[*]</div>
          <div class="achieve-title" data-i18n="ach2_title">CS Teaching Certificate</div>
          <div class="achieve-sub" data-i18n="ach2_sub">ASEP (Supreme Council for Civil Personnel Selection)</div>
        </div>
        <div class="achieve-item">
          <div class="achieve-icon">[*]</div>
          <div class="achieve-title" data-i18n="ach3_title">BSc Thesis 10/10</div>
          <div class="achieve-sub" data-i18n="ach3_sub">Multi-vehicle cooperative SAR algorithm design</div>
        </div>
        <div class="achieve-item">
          <div class="achieve-icon">[*]</div>
          <div class="achieve-title" data-i18n="ach4_title">BSc Grade 7.05/10</div>
          <div class="achieve-sub" data-i18n="ach4_sub">International Hellenic University, Class of 2025</div>
        </div>
      </div>
    </div>
  </section>

  <!-- CONTACT -->
  <section id="contact">
    <div class="container">
      <div class="sh reveal">
        <span class="sh-label">05</span>
        <h2 class="sh-title" data-i18n="sec_contact">Contact</h2>
        <div class="sh-rule"></div>
      </div>
      <div class="contact-layout">
        <div class="reveal">
          <p class="contact-heading" data-i18n="contact_heading">Let&rsquo;s build<br>something.</p>
          <p class="contact-desc" data-i18n="contact_desc">Finishing my MSc and actively looking for <strong>engineering
              roles</strong> in autonomous systems, ML infrastructure, or embedded software. Happy to talk research,
            drones, or backends, reach out through any of these channels.</p>
        </div>
        <div class="contact-links reveal">
          <a href="mailto:s.e.tsilimpokos@gmail.com" class="contact-link">
            <div class="contact-link-left"><svg width="15" height="15" viewBox="0 0 24 24" fill="none"
                stroke="currentColor" stroke-width="1.5">
                <rect x="2" y="4" width="20" height="16" rx="2" />
                <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7" />
              </svg>s.e.tsilimpokos@gmail.com</div>
            <span class="contact-arrow">&#8599;</span>
          </a>
          <a href="https://github.com/Tsilispyr" target="_blank" rel="noopener" class="contact-link">
            <div class="contact-link-left"><svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor">
                <path
                  d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
              </svg>github.com/Tsilispyr</div>
            <span class="contact-arrow">&#8599;</span>
          </a>
          <a href="https://linkedin.com/in/spyridon-tsilimpokos" target="_blank" rel="noopener" class="contact-link">
            <div class="contact-link-left"><svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor">
                <path
                  d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z" />
              </svg>linkedin.com/in/spyridon-tsilimpokos</div>
            <span class="contact-arrow">&#8599;</span>
          </a>
        </div>
      </div>
    </div>
  </section>

  <footer>
    <span>Spyridon Tsilimpokos - <span data-i18n="footer_loc">Athens, Greece</span></span>
    <a href="https://github.com/Tsilispyr" target="_blank" rel="noopener">Tsilispyr.github.io</a>
  </footer>

  <script>
    // ------------------------------------------------------------------
    // BACKGROUND CONFIG  - all visual tuning lives here
    // ------------------------------------------------------------------    
    const BG = {
      rain: {
        colSpacing: 10,       // px between columns / char cell height
        charSize: 8,       // font size px
        speed: 0.03,     // rows per frame (higher = faster cascade)
        speedVar: 0.28,     // per-column speed variation
        trailMin: 22,        // minimum stream length in characters
        trailMax: 90,       // maximum stream length in characters
        headOpacity: 0.55,    // head character opacity (most visible)
        midOpacity: 0.28,    // second character opacity
        tailOpacity: 0.03,    // tail-end opacity (fades to near-invisible)
        mutRate: 80,     // probability per frame to mutate a stream character
        parallaxX: 100,        // max px canvas shift on mouse X
        parallaxY: 100,       // max px canvas shift on mouse Y
      },
      nn: {
        enabled: true,
        layers: [5, 8, 6, 4],   // node counts per layer
        nodeRadius: 4,
        edgeOpacity: 0.50,           // edge line opacity
        nodeOpacity: 0.80,           // node fill base opacity
        nodeColors: [                // pastel fills (alpha appended at draw time)
          'rgba(147,197,253,',        // blue
          'rgba(196,181,253,',        // violet
          'rgba(167,243,208,',        // green
        ],
        pulseSpeed: 0.0008,          // node pulse animation speed
        marginX: 0.10,            // horizontal margin as fraction of canvas width
        marginY: 0.15,            // vertical margin as fraction of canvas height
      }
    };

    // ------------------------------------------------------------------
    // TRANSLATIONS
    // ------------------------------------------------------------------
    const T = {
      en: {
        nav_about: 'About', nav_projects: 'Projects', nav_skills: 'Skills', nav_education: 'Education', nav_contact: 'Contact',
        hero_eyebrow: 'Athens, Greece &nbsp;&middot;&nbsp; MSc Candidate &nbsp;&middot;&nbsp; Open to opportunities',
        hero_desc: 'I build autonomous systems end-to-end, from <strong>bare-metal drone firmware in C++</strong> to <strong>self-supervised LSTM models for GPS-denied UAV swarm recovery</strong>. Fresh out of university, looking for the right team to build something meaningful.',
        pill_msc: 'MSC CANDIDATE', pill_drone: 'DRONE BUILDER', pill_ml: 'ML RESEARCH', btn_projects: 'View Projects',
        sec_about: 'About',
        about_p1: 'I\'m a Computer Science graduate from IHU with a <strong>BSc grade of 7.05/10</strong>, currently finishing my MSc at Harokopeio University. My thesis tackles connectivity prediction and path recovery in GPS-denied UAV swarms, combining self-supervised LSTM dead reckoning with PPO reinforcement learning. Submission is roughly 4 months out.',
        about_p2: 'The drone work goes all the way down: I assembled a quadcopter from bare components, wrote its flight controller firmware in C++ from scratch (Madgwick AHRS, cascade PID, DShot600 via DMA), then built the Android telemetry app for it in Godot 4 with a full MAVLink v2 FSM parser.',
        about_p3: 'On the software side I\'ve delivered full-stack web platforms, agentic RAG pipelines, Kubernetes-backed CI/CD infrastructure, real-time audio DSP tools, and graph-based multi-agent SAR simulations. I work most naturally at the intersection of <strong>embedded systems, applied ML, and backend engineering</strong>.',
        about_p4: 'I\'m strongly AI-driven in how I work, using it as a force multiplier for ideation, analysis, and implementation, while grounding decisions in the underlying theory. My instinct is to find practical improvements, orchestrate systems that hold together, and make things accessible even to less experienced users. In a workplace, I aim to put real skills to the test, learn the team\'s workflow deeply, and grow from there.',
        stat_bsc_lbl: 'BSc Final Grade', stat_msc_lbl: 'MSc Courses Completed', stat_proj_lbl: 'Projects Built', stat_sub_lbl: 'to MSc Submission',
        sec_projects: 'Projects',
        ptype_thesis: 'RESEARCH, ONGOING', ptitle_thesis: 'MSc Thesis - AI_Recovery', psub_thesis: 'Data-driven Connectivity Prediction and Path Recovery in SD-UAV Networks',
        pdesc_thesis: 'Self-supervised LSTM dead reckoning (2-layer, 64 hidden units, 14-feature 9-axis IMU input) trained on 544K sequences at 240Hz, val loss 0.2264. PPO reinforcement learning agent for swarm recovery in a Godot 4 3-drone simulation, bridged via UDP at 10Hz. 4-run domain-shift experiment quantifies 47% degradation when mixing GPS-degree and metre-scale targets.',
        ptype_atlas: 'HARDWARE - PERSONAL', ptitle_atlas: 'Project Atlas - Drone From Scratch', psub_atlas: 'Full hardware build, custom C++ firmware, flight testing',
        pdesc_atlas: 'FlyfishRC Atlas 4 LR deadcat frame (173mm), Teensy 4.1 (600MHz Cortex-M7), dual IMU (MPU6050 + ICM-20948), BMP280, QMC5883L, GM10 GPS, GEPRC Taker E55 ESC, ELRS EP1 Dual @ 420kbaud. Custom firmware: Madgwick6DOF AHRS (&beta;=0.04), cascade PID (100Hz angle / 1kHz rate loop via IntervalTimer ISR), DShot600 over DMA.',
        ptype_telemetry: 'ANDROID APP', ptitle_telemetry: 'Atlas Telemetry', psub_telemetry: 'Companion app for Project Atlas',
        pdesc_telemetry: 'Godot 4 APK (v1.1, 116 MB). MAVLink v2 FSM parser with CRC-16/MCRF4XX. Three modes: USB-C VCP, WiFi UDP 14550, Bluetooth SPP. Retro VFD aesthetic, vector artificial horizon, 300-sample voltage graph (30s at 10Hz), 20-segment current bar (0&ndash;40A).',
        ptype_paperpilot: 'AI / RAG, MSC COURSE', ptitle_paperpilot: 'PaperPilot', psub_paperpilot: 'Agentic RAG over research papers',
        pdesc_paperpilot: 'LangGraph ReAct agent over 25 ArXiv papers (360K tokens, Qdrant). Section-aware chunking, BAAI/bge-reranker-base reranking, arxiv_search fallback tool. RAGAS: Tool Call Accuracy +10% (v1&rarr;v2), Context Precision +23%. Docker stack with Langfuse, Chainlit UI.',
        ptype_sar: 'FULL-STACK SIMULATION', ptitle_sar: 'SAR-UGV Platform', psub_sar: 'Multi-agent SAR on real maps',
        pdesc_sar: 'FastAPI backend, osmnx graph routing, risk-weighted A* with tiered enemy penalties, cKDTree O(log n). Four simultaneous agents, 18 REST endpoints, 0.25&times;&ndash;100&times; playback speed, WGS-84 to UTM projection.',
        ptype_devops: 'CLOUD / DEVOPS, MSC COURSE', ptitle_devops: 'DevOps-Pets', psub_devops: 'K8s CI/CD with Azure deployment',
        pdesc_devops: 'Kubernetes (Kind) provisioned via Ansible, automated Jenkins CI/CD, PostgreSQL, MinIO, MailHog, HTTPS via cert-manager + Let\'s Encrypt. Azure VM and AKS cloud deployment. Single-command setup for local, VM, or cloud environments.',
        ptype_audioweb: 'FULL-STACK, SELF-HOSTED', ptitle_audioweb: 'AudioWeb', psub_audioweb: 'Self-hosted music streaming',
        pdesc_audioweb: 'Flask 3.0, PostgreSQL, MinIO, Docker Compose. yt-dlp with bgutil headless Chromium PO-token auth, 320kbps MP3. React 18/Vite frontend, 10-band EQ (&plusmn;12dB, Q=1.2 biquad), 10 Canvas visualisation modes, 2048-sample FFT.',
        ptype_netflix: 'ML, MSC COURSE', ptitle_netflix: 'Netflix Recommendation System', psub_netflix: 'Content-based filtering with Explainable AI',
        pdesc_netflix: 'TF-IDF over director, cast, genre, and description on 8000+ Netflix titles. Cosine similarity with 3x genre weighting, sanitised cast names to resolve metadata overlap. K-Means (15 clusters) + PCA 2D visualisation maps the full catalogue. XAI layer surfaces why each recommendation fired (shared director, cast, or genre). Age-rating classification explored to 61% ceiling, explained as a data quality ceiling rather than model failure.',
        ptype_bigdata: 'DATA ANALYSIS, GITHUB', ptitle_bigdata: 'Big Data Analysis', psub_bigdata: 'Graph network analysis and large-scale EDA',
        pdesc_bigdata: 'Multi-layer graph network analysis with hub detection, community discovery, and degree distribution profiling. Hub nodes (blue), intermediate nodes (pink), and leaf nodes (green) visualised with force-directed layout. Includes large-scale dataset EDA, aggregation pipelines, and statistical summaries across heterogeneous data sources.',
        ptype_datamine: 'DATA MINING, MSC COURSE', ptitle_datamine: 'Passenger Traffic Prediction', psub_datamine: 'SF Airport, 38 893 records, 2000-2022',
        pdesc_datamine: '5-class classification on historical airport data. KNIME pipeline: IQR outlier removal, stratified 70/30 split, seven algorithms benchmarked. Best ensemble (Random Forest + GBT voting): <strong>78.9% accuracy</strong>.',
        ptype_attica: 'STATISTICS, MSC COURSE', ptitle_attica: 'Attica Health Study Analysis', psub_attica: '20-year epidemiological cohort, CVD risk modelling',
        pdesc_attica: 'R-based statistical analysis of the ATTICA Study cohort. Logistic regression for 10-year and 20-year CVD outcomes, Pearson correlation heatmap, boxplots stratified by outcome, skewness and VIF multicollinearity diagnostics, pROC for ROC/AUC evaluation. Auto-generated multi-page PDF report via ggplot2, with safe-mode package bootstrapping for reproducibility.',
        ptype_bscthesis: 'ACADEMIC, BSC THESIS', ptitle_bscthesis: 'BSc Thesis - SAR Simulation', psub_bscthesis: 'Grade 10/10, multi-vehicle cooperative SAR',
        pdesc_bscthesis: 'Python/Pygame cooperative simulation (~1 763 lines). 25&times;20 grid, three vehicle types, six cooperative scenarios, A* with terrain costs, three search patterns (Expanding Square, Sector, Parallel Track), fog-of-war, JSON stats export.',
        mini_vis_title: 'Python Audio Visualiser', mini_vis_desc: 'WASAPI loopback, NumPy rfft 2048-sample, 200 bars 20Hz-22kHz, 60FPS Pygame.',
        mini_yolo_title: 'YOLOv8 Live Detection', mini_yolo_desc: 'YOLOv8l, OpenCV 1280&times;720, agnostic NMS, polygon zone monitoring via Supervision.',
        mini_yt_title: 'YouTube Downloader', mini_yt_desc: 'HTML/Flask front-end for yt-dlp, YouTube &amp; YouTube Music, playlists, 320kbps MP3.',
        mini_more_title: 'More on the way', mini_more_desc: 'NLP, deep learning, computer vision, and additional MSc coursework not yet published.',
        proj_note: '[ Several projects are still in progress or not yet uploaded. NLP, deep learning experiments, graph and network analysis, and more are coming. ]',
        sec_skills: 'Skills', sg_lang: 'LANGUAGES', sg_ai: 'AI &amp; MACHINE LEARNING', sg_sys: 'SYSTEMS &amp; EMBEDDED', sg_back: 'BACKEND &amp; DATA', sg_cloud: 'CLOUD &amp; DEVOPS', sg_front: 'FRONTEND &amp; TOOLS',
        sec_edu: 'Education', edu_msc_status: 'ONGOING', edu_msc_degree: 'MSc in Computer Science', edu_msc_school: 'Harokopeio University of Athens (HUA)',
        edu_msc_detail: 'Sep 2025 - present &nbsp;&middot;&nbsp; submission ~Sep 2026<br><br>Thesis: <strong>Data-driven Connectivity Prediction and Path Recovery in SD-UAV Networks using Self-Supervised Learning</strong><br><br>Courses: Deep Learning, Computer Vision, NLP &amp; Info Retrieval, Graph &amp; Network Analysis, Machine Learning, Data Mining &amp; Recommender Systems, Applications of DS &amp; AI, Cloud Platforms, Statistics, plus thesis.',
        edu_bsc_status: 'COMPLETED', edu_bsc_degree: 'BSc in Computer Science', edu_bsc_school: 'Harokopio University of Athens (HUA)',
        edu_bsc_detail: 'Sep 2021 - Sep 2025 &nbsp;&middot;&nbsp; <strong>Grade 7.05/10</strong><br><br>Thesis: <strong>Σχεδιασμός αλγορίθμων διαχείρισης για μη επανδρωμένα οχήματα για επιχειρήσεις έρευνας και διάσωσης</strong><br>(Algorithm Design for UAV Management in SAR Operations)<br><br>Thesis grade: <strong>10/10</strong>',
        sec_achieve: 'ACHIEVEMENTS',
        ach1_title: 'G. Karabatzos Scholarship', ach1_sub: 'Harokopeio University, awarded for MSc studies',
        ach2_title: 'CS Teaching Certificate', ach2_sub: 'ASEP (Supreme Council for Civil Personnel Selection)',
        ach3_title: 'BSc Thesis 10/10', ach3_sub: 'Multi-vehicle cooperative SAR algorithm design',
        ach4_title: 'BSc Grade 7.05/10', ach4_sub: 'International Hellenic University, Class of 2025',
        sec_contact: 'Contact', contact_heading: 'Let\'s build<br>something.',
        contact_desc: 'Finishing my MSc and actively looking for <strong>engineering roles</strong> in autonomous systems, ML infrastructure, or embedded software. Happy to talk research, drones, or backends, reach out through any of these channels.',
        footer_loc: 'Athens, Greece',
        roles: ['Software Engineer', 'ML Researcher', 'Drone Builder', 'Embedded Systems Dev']
      },
      el: {
        nav_about: 'Σχετικά', nav_projects: 'Έργα', nav_skills: 'Δεξιότητες', nav_education: 'Εκπαίδευση', nav_contact: 'Επικοινωνία',
        hero_eyebrow: 'Αθήνα, Ελλάδα &nbsp;&middot;&nbsp; Υποψήφιος ΠΜΣ &nbsp;&middot;&nbsp; Αναζητώ ευκαιρίες',
        hero_desc: 'Αναπτύσσω αυτόνομα συστήματα εξαρχής, από <strong>firmware ελεγκτή πτήσης σε C++</strong> έως <strong>αυτο-εποπτευόμενα μοντέλα LSTM για ανάκτηση σμήνους UAV χωρίς GPS</strong>. Μόλις αποφοίτησα, αναζητώ την κατάλληλη ομάδα για να χτίσουμε κάτι σημαντικό.',
        pill_msc: 'ΥΠΟΨ. ΠΜΣ', pill_drone: 'ΚΑΤΑΣΚΕΥΗ DRONE', pill_ml: 'ΈΡΕΥΝΑ ML', btn_projects: 'Δες τα Έργα',
        sec_about: 'Σχετικά',
        about_p1: 'Είμαι πτυχιούχος Πληροφορικής από το ΔΙΠαΘ με <strong>βαθμό πτυχίου 7.05/10</strong>, και ολοκληρώνω το ΠΜΣ μου στο Χαροκόπειο Πανεπιστήμιο Αθηνών. Η διπλωματική μου ασχολείται με την πρόβλεψη συνδεσιμότητας και την ανάκτηση διαδρομής σε δίκτυα UAV χωρίς GPS, συνδυάζοντας αυτο-εποπτευόμενη εκμάθηση LSTM με ενισχυτική μάθηση PPO. Η κατάθεση αναμένεται σε περίπου 4 μήνες.',
        about_p2: 'Η δουλειά με τα drone φτάνει μέχρι τα βασικά: συναρμολόγησα τετρακόπτερο από μηδέν, έγραψα το firmware του ελεγκτή πτήσης εξαρχής σε C++ (Madgwick AHRS, cascade PID, DShot600 μέσω DMA), και ανέπτυξα την Android εφαρμογή τηλεμετρίας σε Godot 4 με πλήρη MAVLink v2 FSM parser.',
        about_p3: 'Στο λογισμικό, έχω παραδώσει full-stack πλατφόρμες, agentic RAG pipelines, CI/CD υποδομή με Kubernetes, εργαλεία audio DSP σε πραγματικό χρόνο, και γραφοθεωρητικές προσομοιώσεις SAR. Εργάζομαι πιο φυσικά στην τομή <strong>ενσωματωμένων συστημάτων, εφαρμοσμένης ML και backend engineering</strong>.',
        about_p4: 'Στον τρόπο που εργάζομαι χρησιμοποιώ έντονα την ΤΝ ως πολλαπλασιαστή δυνάμεων για ιδέες, ανάλυση και υλοποίηση, στηρίζοντας πάντα τις αποφάσεις στην υποκείμενη θεωρία. Η φύση μου είναι να βρίσκω πρακτικές βελτιώσεις, να οργανώνω συστήματα που λειτουργούν και να κάνω τα πράγματα προσβάσιμα ακόμα και σε λιγότερο έμπειρους χρήστες. Στον επαγγελματικό χώρο, στοχεύω να δοκιμάσω πραγματικές δεξιότητες, να μάθω σε βάθος τη ροή εργασίας της ομάδας και να εξελιχθώ.',
        stat_bsc_lbl: 'Βαθμός Πτυχίου', stat_msc_lbl: 'Μαθήματα ΠΜΣ', stat_proj_lbl: 'Υλοποιημένα Έργα', stat_sub_lbl: 'ως Κατάθεση ΠΜΣ',
        sec_projects: 'Έργα',
        ptype_thesis: 'ΈΡΕΥΝΑ, ΣΕ ΕΞΕΛΙΞΗ', ptitle_thesis: 'Διπλωματική ΠΜΣ - AI_Recovery', psub_thesis: 'Πρόβλεψη Συνδεσιμότητας και Ανάκτηση Διαδρομής σε Δίκτυα SD-UAV',
        pdesc_thesis: 'Αυτο-εποπτευόμενο LSTM (2 επίπεδα, 64 κρυφές μονάδες, 14 χαρακτηριστικά 9-αξόνων IMU) εκπαιδευμένο σε 544K ακολουθίες στα 240Hz, val loss 0.2264. PPO agent για ανάκτηση σμήνους σε προσομοίωση 3 drones (Godot 4) μέσω UDP bridge στα 10Hz. Πείραμα domain-shift 4 επαναλήψεων ποσοτικοποιεί 47% υποβάθμιση.',
        ptype_atlas: 'ΥΛΙΚΟ - ΠΡΟΣΩΠΙΚΟ', ptitle_atlas: 'Project Atlas - Drone από Μηδέν', psub_atlas: 'Πλήρης κατασκευή υλικού, custom firmware C++, δοκιμές πτήσης',
        pdesc_atlas: 'FlyfishRC Atlas 4 LR deadcat πλαίσιο (173mm), Teensy 4.1 (600MHz Cortex-M7), διπλό IMU (MPU6050 + ICM-20948), BMP280, QMC5883L, GM10 GPS, ESC GEPRC Taker E55, ELRS EP1 Dual @ 420kbaud. Custom firmware: Madgwick6DOF AHRS (&beta;=0.04), cascade PID (100Hz γωνία / 1kHz ρυθμός), DShot600 μέσω DMA.',
        ptype_telemetry: 'ΕΦΑΡΜΟΓΗ ANDROID', ptitle_telemetry: 'Atlas Τηλεμετρία', psub_telemetry: 'Companion app για το Project Atlas',
        pdesc_telemetry: 'Godot 4 APK (v1.1, 116 MB). MAVLink v2 FSM parser με CRC-16/MCRF4XX. Τρεις λειτουργίες: USB-C VCP, WiFi UDP 14550, Bluetooth SPP. Retro VFD αισθητική, διανυσματικός τεχνητός ορίζοντας, γράφημα τάσης 300 δειγμάτων (30s στα 10Hz), μπάρα ρεύματος 20 τμημάτων (0&ndash;40A).',
        ptype_paperpilot: 'AI / RAG, ΜΑΘΗΜΑ ΠΜΣ', ptitle_paperpilot: 'PaperPilot', psub_paperpilot: 'Agentic RAG σύστημα για ερευνητικά άρθρα',
        pdesc_paperpilot: 'LangGraph ReAct agent για 25 άρθρα ArXiv (360K tokens, Qdrant). Section-aware chunking, reranking BAAI/bge-reranker-base, fallback εργαλείο arxiv_search. RAGAS: Tool Call Accuracy +10% (v1→v2), Context Precision +23%. Docker stack με Langfuse, Chainlit UI.',
        ptype_sar: 'FULL-STACK ΠΡΟΣΟΜΟΙΩΣΗ', ptitle_sar: 'Πλατφόρμα SAR-UGV', psub_sar: 'Multi-agent SAR σε πραγματικούς χάρτες',
        pdesc_sar: 'FastAPI backend, δρομολόγηση osmnx, A* με βάρη κινδύνου και τιμωρίες εχθρικών ζωνών, cKDTree O(log n). Τέσσερις ταυτόχρονοι agents, 18 REST endpoints, αναπαραγωγή 0.25&times;&ndash;100&times;, προβολή WGS-84 σε UTM.',
        ptype_devops: 'CLOUD / DEVOPS, ΜΑΘΗΜΑ ΠΜΣ', ptitle_devops: 'DevOps-Pets', psub_devops: 'K8s CI/CD με ανάπτυξη Azure',
        pdesc_devops: 'Kubernetes (Kind) με Ansible playbooks, αυτόματο Jenkins CI/CD, PostgreSQL, MinIO, MailHog, HTTPS μέσω cert-manager + Let\'s Encrypt. Ανάπτυξη σε Azure VM και AKS. Ρύθμιση με μία εντολή για τοπικό, VM ή cloud περιβάλλον.',
        ptype_audioweb: 'FULL-STACK, ΑΥΤΟ-ΦΙΛΟΞΕΝΙΑ', ptitle_audioweb: 'AudioWeb', psub_audioweb: 'Πλατφόρμα streaming μουσικής',
        pdesc_audioweb: 'Flask 3.0, PostgreSQL, MinIO, Docker Compose. yt-dlp με bgutil headless Chromium για 320kbps MP3. Frontend React 18/Vite, 10-band EQ (&plusmn;12dB, Q=1.2 biquad), 10 λειτουργίες Canvas οπτικοποίησης, 2048-sample FFT.',
        ptype_netflix: 'ML, ΜΑΘΗΜΑ ΠΜΣ', ptitle_netflix: 'Σύστημα Προτάσεων Netflix', psub_netflix: 'Content-based filtering με Explainable AI',
        pdesc_netflix: 'TF-IDF πάνω σε σκηνοθέτη, cast, είδος και περιγραφή για 8000+ τίτλους Netflix. Cosine similarity με τριπλό βάρος στο είδος, εξυγίανση ονομάτων cast για αποφυγή overlap metadata. K-Means (15 clusters) + PCA 2D οπτικοποίηση ολόκληρου του καταλόγου. XAI επίπεδο που εξηγεί γιατί προτάθηκε κάθε τίτλος (κοινός σκηνοθέτης, cast ή είδος). Ταξινόμηση age-rating εξερευνήθηκε ως 61% οροφή, εξηγούμενη ως ποιοτικό πρόβλημα δεδομένων.',
        ptype_bigdata: 'ΑΝΑΛΥΣΗ ΔΕΔΟΜΕΝΩΝ, GITHUB', ptitle_bigdata: 'Big Data Analysis', psub_bigdata: 'Ανάλυση γράφου δικτύου και μεγάλης κλίμακας EDA',
        pdesc_bigdata: 'Ανάλυση γράφου δικτύου πολλαπλών επιπέδων με ανίχνευση κόμβων-κόμβων, ανακάλυψη κοινοτήτων και κατανομή βαθμού. Κόμβοι-κόμβοι (μπλε), ενδιάμεσοι (ροζ) και φύλλα (πράσινο) οπτικοποιημένα με force-directed layout. Περιλαμβάνει EDA μεγάλης κλίμακας, pipelines συγκέντρωσης και στατιστικές συνοψίσεις.',
        ptype_datamine: 'ΕΞΟΡΥΞΗ ΔΕΔΟΜΕΝΩΝ, ΜΑΘΗΜΑ ΠΜΣ', ptitle_datamine: 'Πρόβλεψη Κυκλοφορίας Επιβατών', psub_datamine: 'Αεροδρόμιο SF, 38 893 εγγραφές, 2000-2022',
        pdesc_datamine: 'Ταξινόμηση σε 5 κλάσεις σε ιστορικά δεδομένα αεροδρομίου. Pipeline KNIME: αφαίρεση outliers με IQR, στρωματοποιημένη κατανομή 70/30, επτά αλγόριθμοι. Καλύτερο ensemble (Random Forest + GBT voting): <strong>78,9% ακρίβεια</strong>.',
        ptype_attica: 'ΣΤΑΤΙΣΤΙΚΗ, ΜΑΘΗΜΑ ΠΜΣ', ptitle_attica: 'Ανάλυση Μελέτης Υγείας Αττικής', psub_attica: '20-ετής επιδημιολογική κοόρτη, μοντελοποίηση κινδύνου ΚΑΝ',
        pdesc_attica: 'Στατιστική ανάλυση σε R της κοόρτης ATTICA Study. Λογιστική παλινδρόμηση για 10-ετή και 20-ετή καρδιαγγειακά αποτελέσματα, χάρτης θερμότητας συσχετίσεων Pearson, boxplots ανά αποτέλεσμα, ασυμμετρία και VIF διαγνωστικά πολυσυγγραμμικότητας, pROC για καμπύλες ROC/AUC. Αυτόματη πολυσέλιδη αναφορά PDF μέσω ggplot2.',
        ptype_bscthesis: 'ΑΚΑΔΗΜΑΪΚΟ, ΠΤΥΧΙΑΚΗ', ptitle_bscthesis: 'Πτυχιακή - Προσομοίωση SAR', psub_bscthesis: 'Βαθμός 10/10, συνεργατικά πολυ-οχηματικά SAR',
        pdesc_bscthesis: 'Συνεργατική προσομοίωση Python/Pygame (~1 763 γραμμές). Πλέγμα 25&times;20, τρία είδη οχημάτων, έξι συνεργατικά σενάρια, A* με κόστη εδάφους, τρία μοτίβα αναζήτησης (Expanding Square, Sector, Parallel Track), ομίχλη πολέμου, εξαγωγή στατιστικών JSON.',
        mini_vis_title: 'Python Οπτικοποιητής Ήχου', mini_vis_desc: 'WASAPI loopback, NumPy rfft 2048 δείγματα, 200 μπάρες 20Hz-22kHz, 60FPS Pygame.',
        mini_yolo_title: 'YOLOv8 Live Ανίχνευση', mini_yolo_desc: 'YOLOv8l, OpenCV 1280&times;720, agnostic NMS, παρακολούθηση πολυγωνικής ζώνης.',
        mini_yt_title: 'YouTube Downloader', mini_yt_desc: 'HTML/Flask front-end για yt-dlp, YouTube &amp; YouTube Music, playlist, 320kbps MP3.',
        mini_more_title: 'Περισσότερα έρχονται', mini_more_desc: 'NLP, deep learning, computer vision και επιπλέον εργασίες ΠΜΣ που δεν έχουν δημοσιευτεί ακόμα.',
        proj_note: '[ Αρκετά έργα βρίσκονται σε εξέλιξη ή δεν έχουν ανέβει ακόμα. NLP, πειράματα deep learning, ανάλυση γράφων και δικτύων, και περισσότερα έρχονται. ]',
        sec_skills: 'Δεξιότητες', sg_lang: 'ΓΛΩΣΣΕΣ', sg_ai: 'ΤΕΧΝΗΤΗ ΝΟΗΜΟΣΥΝΗ &amp; ML', sg_sys: 'ΣΥΣΤΗΜΑΤΑ &amp; ΕΝΣΩΜΑΤΩΜΕΝΑ', sg_back: 'BACKEND &amp; ΔΕΔΟΜΕΝΑ', sg_cloud: 'CLOUD &amp; DEVOPS', sg_front: 'FRONTEND &amp; ΕΡΓΑΛΕΙΑ',
        sec_edu: 'Εκπαίδευση', edu_msc_status: 'ΣΕ ΕΞΕΛΙΞΗ', edu_msc_degree: 'ΠΜΣ Πληροφορική', edu_msc_school: 'Χαροκόπειο Πανεπιστήμιο Αθηνών (ΧΠΑ)',
        edu_msc_detail: 'Σεπ 2025 - παρόν &nbsp;&middot;&nbsp; κατάθεση ~Σεπ 2026<br><br>Διπλωματική: <strong>Data-driven Connectivity Prediction and Path Recovery in SD-UAV Networks using Self-Supervised Learning</strong><br><br>Μαθήματα: Deep Learning, Computer Vision, NLP &amp; Ανάκτηση Πληροφορίας, Ανάλυση Γράφων &amp; Δικτύων, Machine Learning, Εξόρυξη Δεδομένων &amp; Συστήματα Συστάσεων, Εφαρμογές Επιστήμης Δεδομένων &amp; ΤΝ, Cloud Πλατφόρμες, Στατιστική, συν διπλωματική.',
        edu_bsc_status: 'ΟΛΟΚΛΗΡΩΘΗΚΕ', edu_bsc_degree: 'Πτυχίο Πληροφορικής', edu_bsc_school: 'Χαροκόπειο Πανεπιστήμιο Αθηνών (ΧΠΑ)',
        edu_bsc_detail: 'Σεπ 2021 - Σεπ 2025 &nbsp;&middot;&nbsp; <strong>Βαθμός 7.05/10</strong><br><br>Πτυχιακή: <strong>Σχεδιασμός αλγορίθμων διαχείρισης για μη επανδρωμένα οχήματα για επιχειρήσεις έρευνας και διάσωσης</strong><br><br>Βαθμός πτυχιακής: <strong>10/10</strong>',
        sec_achieve: 'ΕΠΙΤΕΥΓΜΑΤΑ',
        ach1_title: 'Υποτροφία Γ. Καραμπατζός', ach1_sub: 'Χαροκόπειο Πανεπιστήμιο, βραβεύτηκε για σπουδές ΠΜΣ',
        ach2_title: 'Πιστοποιητικό Διδακτικής Επάρκειας', ach2_sub: 'ΑΣΕΠ (Ανώτατο Συμβούλιο Επιλογής Προσωπικού)',
        ach3_title: 'Πτυχιακή 10/10', ach3_sub: 'Σχεδιασμός αλγορίθμων για συνεργατικά UAV σε SAR επιχειρήσεις',
        ach4_title: 'Βαθμός Πτυχίου 7.05/10', ach4_sub: 'Διεθνές Πανεπιστήμιο Ελλάδος, τάξη 2025',
        sec_contact: 'Επικοινωνία', contact_heading: 'Ας χτίσουμε<br>κάτι μαζί.',
        contact_desc: 'Ολοκληρώνω το ΠΜΣ μου και αναζητώ ενεργά <strong>θέσεις μηχανικού</strong> σε αυτόνομα συστήματα, ML infrastructure ή ενσωματωμένο λογισμικό. Χαίρομαι να συζητήσω για έρευνα, drones ή backends.',
        footer_loc: 'Αθήνα, Ελλάδα',
        roles: ['Μηχανικός Λογισμικού', 'Ερευνητής ML', 'Κατασκευαστής Drone', 'Ενσωμ. Συστήματα']
      }
    };

    //  Typewriter 
    const typedEl = document.getElementById('typed');
    let ri = 0, ci = 0, del = false, typeTimer = null;
    function restartTypewriter() {
      clearTimeout(typeTimer); ri = 0; ci = 0; del = false;
      typedEl.textContent = ''; typeStep();
    }
    function typeStep() {
      const roles = T[lang].roles, w = roles[ri];
      typedEl.textContent = del ? w.slice(0, ci--) : w.slice(0, ci++);
      let t = del ? 42 : 82;
      if (!del && ci > w.length) { t = 1500; del = true; }
      if (del && ci < 0) { del = false; ri = (ri + 1) % roles.length; ci = 0; t = 260; }
      typeTimer = setTimeout(typeStep, t);
    }

    //  Language switch ─
    let lang = localStorage.getItem('lang') || 'en';
    const langBtn = document.getElementById('langBtn');
    function applyLang(l) {
      lang = l;
      localStorage.setItem('lang', l);
      document.getElementById('html-root').lang = l;
      langBtn.textContent = l === 'en' ? 'EL' : 'EN';
      document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.dataset.i18n;
        if (T[l][key] !== undefined) el.innerHTML = T[l][key];
      });
      restartTypewriter();
    }
    langBtn.addEventListener('click', () => applyLang(lang === 'en' ? 'el' : 'en'));
    applyLang(lang);

    //  Background canvas: neural network + Matrix-style binary rain 
    (function () {
      const c = document.getElementById('binary-canvas');
      const ctx = c.getContext('2d');
      const R = BG.rain, N = BG.nn;
      let cols = 0, drops = [], nodes = [], ts = 0;

      function makeStream(scatter) {
        const len = R.trailMin + Math.floor(Math.random() * (R.trailMax - R.trailMin + 1));
        return {
          row: scatter
            ? Math.random() * (c.height / R.colSpacing + len) - len
            : -(Math.random() * R.trailMax + 2),
          speed: R.speed + Math.random() * R.speedVar,
          len,
          chars: Array.from({ length: len }, () => Math.random() < .5 ? '0' : '1')
        };
      }

      function buildNodes() {
        nodes = [];
        if (!N.enabled) return;
        const W = c.width, H = c.height;
        const mx = W * N.marginX, my = H * N.marginY;
        const usableW = W - mx * 2, usableH = H - my * 2;
        const layerGap = usableW / (N.layers.length - 1);
        N.layers.forEach((count, li) => {
          const x = mx + li * layerGap;
          const nodeGap = usableH / (count + 1);
          for (let ni = 0; ni < count; ni++) {
            nodes.push({
              x, y: my + (ni + 1) * nodeGap, layer: li,
              color: N.nodeColors[li % N.nodeColors.length],
              phase: Math.random() * Math.PI * 2
            });
          }
        });
      }

      function resize() {
        c.width = window.innerWidth + 60;
        c.height = window.innerHeight + 60;
        cols = Math.ceil(c.width / R.colSpacing) + 1;
        while (drops.length < cols) drops.push(makeStream(true));
        drops.length = cols;
        buildNodes();
      }
      resize();
      window.addEventListener('resize', resize);

      document.addEventListener('mousemove', e => {
        c.style.transform = `translate(${(e.clientX / innerWidth - .5) * R.parallaxX}px,${(e.clientY / innerHeight - .5) * R.parallaxY}px)`;
      });

      function tick(now) {
        ts = now;

        // 1 - clear canvas (page background shows through transparent canvas)
        ctx.clearRect(0, 0, c.width, c.height);

        // 2 - neural network redrawn fresh each frame
        if (N.enabled) {
          for (let i = 0; i < nodes.length; i++) {
            for (let j = i + 1; j < nodes.length; j++) {
              if (nodes[j].layer !== nodes[i].layer + 1) continue;
              ctx.beginPath(); ctx.moveTo(nodes[i].x, nodes[i].y);
              ctx.lineTo(nodes[j].x, nodes[j].y);
              ctx.strokeStyle = 'rgba(100,140,220,' + N.edgeOpacity + ')';
              ctx.lineWidth = 0.7; ctx.stroke();
            }
          }
          nodes.forEach(n => {
            const pulse = 0.5 + 0.5 * Math.sin(ts * N.pulseSpeed + n.phase);
            ctx.beginPath(); ctx.arc(n.x, n.y, N.nodeRadius, 0, Math.PI * 2);
            ctx.fillStyle = n.color + (N.nodeOpacity * (0.55 + 0.45 * pulse)) + ')';
            ctx.fill();
          });
        }

        // 3 - Matrix streams: head brightest, trail fades, chars mutate each frame
        ctx.font = R.charSize + 'px JetBrains Mono,monospace';
        for (let i = 0; i < drops.length; i++) {
          const d = drops[i];
          const x = i * R.colSpacing;

          // Randomly mutate one char - creates the "constantly changing rows" effect
          if (Math.random() < R.mutRate) {
            d.chars[Math.floor(Math.random() * d.len)] = Math.random() < .5 ? '0' : '1';
          }

          // Draw head (j=0) down to tail (j=len-1) with decreasing opacity
          for (let j = 0; j < d.len; j++) {
            const py = (d.row - j) * R.colSpacing;
            if (py < -R.colSpacing || py > c.height + R.colSpacing) continue;
            const t = j / Math.max(d.len - 1, 1);
            const opacity = j === 0 ? R.headOpacity
              : R.midOpacity * (1 - t) + R.tailOpacity * t;
            if (opacity < 0.008) continue;
            ctx.fillStyle = `rgba(20,50,150,${opacity.toFixed(3)})`;
            ctx.fillText(d.chars[j], x, py);
          }

          d.row += d.speed;
          if ((d.row - d.len) * R.colSpacing > c.height) drops[i] = makeStream(false);
        }
        requestAnimationFrame(tick);
      }
      requestAnimationFrame(tick);
    })();

    //  Artificial horizon 
    (function () {
      const hud = document.getElementById('horizon-hud');
      const c = document.getElementById('horizon-canvas');
      const ctx = c.getContext('2d'), W = c.width, H = c.height, CX = W / 2, CY = H / 2, R = W / 2 - 2;
      function draw(p) {
        ctx.clearRect(0, 0, W, H); ctx.save();
        ctx.beginPath(); ctx.arc(CX, CY, R, 0, Math.PI * 2); ctx.clip();
        const hy = CY + (p - .5) * H * 1.6;
        ctx.fillStyle = 'rgba(196,218,245,0.7)'; ctx.fillRect(0, 0, W, hy);
        ctx.fillStyle = 'rgba(200,175,140,0.7)'; ctx.fillRect(0, hy, W, H);
        ctx.strokeStyle = 'rgba(40,80,160,0.5)'; ctx.lineWidth = 1;
        ctx.beginPath(); ctx.moveTo(0, hy); ctx.lineTo(W, hy); ctx.stroke();
        ctx.restore();
        ctx.strokeStyle = 'rgba(150,170,200,0.6)'; ctx.lineWidth = 1;
        ctx.beginPath(); ctx.arc(CX, CY, R, 0, Math.PI * 2); ctx.stroke();
        ctx.strokeStyle = 'rgba(30,60,130,0.7)'; ctx.lineWidth = 1;
        ctx.beginPath(); ctx.moveTo(CX - 13, CY); ctx.lineTo(CX - 5, CY); ctx.moveTo(CX + 5, CY); ctx.lineTo(CX + 13, CY); ctx.stroke();
        ctx.fillStyle = 'rgba(30,60,130,0.7)'; ctx.beginPath(); ctx.arc(CX, CY, 2, 0, Math.PI * 2); ctx.fill();
      }
      const lbl = document.getElementById('horizon-label');
      window.addEventListener('scroll', () => {
        const h = document.documentElement, p = h.scrollTop / (h.scrollHeight - h.clientHeight) || 0;
        hud.classList.toggle('visible', h.scrollTop > 60);
        lbl.textContent = 'ALT ' + String(Math.round(p * 100)).padStart(3, '0');
        draw(p);
      }, { passive: true });
      draw(0);
    })();

    //  Progress + nav 
    const pb = document.getElementById('progress-bar'), nav = document.getElementById('navbar');
    window.addEventListener('scroll', () => {
      const h = document.documentElement;
      pb.style.width = (h.scrollTop / (h.scrollHeight - h.clientHeight) * 100) + '%';
      nav.classList.toggle('scrolled', h.scrollTop > 8);
    }, { passive: true });

    //  Mobile nav 
    const hbg = document.getElementById('hamburger'), mob = document.getElementById('mobileMenu');
    hbg.addEventListener('click', () => { hbg.classList.toggle('open'); mob.classList.toggle('open'); });
    document.querySelectorAll('.mob-link').forEach(a => a.addEventListener('click', () => { hbg.classList.remove('open'); mob.classList.remove('open'); }));

    //  Active nav link ─
    const navAs = document.querySelectorAll('.nav-links a');
    document.querySelectorAll('section[id]').forEach(s =>
      new IntersectionObserver(entries => {
        entries.forEach(e => { if (e.isIntersecting) navAs.forEach(a => a.classList.toggle('active', a.getAttribute('href') === '#' + e.target.id)); });
      }, { threshold: 0.3 }).observe(s)
    );

    //  Reveal on scroll 
    const revIo = new IntersectionObserver((entries) => {
      entries.forEach((e, i) => {
        if (e.isIntersecting) { setTimeout(() => e.target.classList.add('visible'), i * 55); revIo.unobserve(e.target); }
      });
    }, { threshold: 0.07, rootMargin: '0px 0px -24px 0px' });
    document.querySelectorAll('.reveal').forEach(el => revIo.observe(el));
  </script>
</body>

</html>
