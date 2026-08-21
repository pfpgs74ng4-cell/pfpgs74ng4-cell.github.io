<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Home</title>
  <style>
    :root {
      --bg: #0d1117;
      --panel: #161b22;
      --border: #30363d;
      --text: #f0f6fc;
      --muted: #8b949e;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: var(--bg);
      color: var(--text);
    }

    header {
      height: 64px;
      border-bottom: 1px solid var(--border);
      display: flex;
      align-items: center;
    }

    .container {
      width: min(1000px, calc(100% - 40px));
      margin: 0 auto;
    }

    nav {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .logo {
      font-size: 18px;
      font-weight: 600;
    }

    nav a {
      color: var(--muted);
      text-decoration: none;
      margin-left: 24px;
    }

    nav a:hover {
      color: var(--text);
    }

    main {
      padding: 80px 0;
    }

    .hero {
      min-height: 300px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
    }

    .hero h1 {
      margin: 0;
      font-size: clamp(40px, 8vw, 72px);
      font-weight: 600;
      letter-spacing: -2px;
    }

    .hero p {
      margin-top: 16px;
      color: var(--muted);
    }

    .tools {
      margin-top: 80px;
      padding-top: 32px;
      border-top: 1px solid var(--border);
    }

    .tools h2 {
      margin: 0 0 20px;
      font-size: 22px;
    }

    .tool-card {
      display: block;
      max-width: 320px;
      padding: 22px;
      border: 1px solid var(--border);
      border-radius: 12px;
      background: var(--panel);
      color: var(--text);
      text-decoration: none;
    }

    .tool-card:hover {
      border-color: var(--muted);
    }

    .tool-card strong {
      display: block;
      margin-bottom: 8px;
    }

    .tool-card span {
      color: var(--muted);
      font-size: 14px;
    }

    footer {
      padding: 32px 0;
      color: var(--muted);
      border-top: 1px solid var(--border);
      font-size: 13px;
    }
  </style>
</head>

<body>
  <header>
    <div class="container">
      <nav>
        <div class="logo">Home</div>
        <div>
          <a href="/">Home</a>
          <a href="#tools">Tools</a>
          <a href="https://github.com/pfpgs74ng4-cell">GitHub</a>
        </div>
      </nav>
    </div>
  </header>

  <main class="container">
    <section class="hero">
      <div>
        <h1>Hello.</h1>
        <p>More things will be added here.</p>
      </div>
    </section>

    <section class="tools" id="tools">
      <h2>Tools</h2>

      <a class="tool-card" href="/raw-bayer-viewer/">
        <strong>RAW Bayer Viewer</strong>
        <span>Preview Bayer RAW images in the browser →</span>
      </a>
    </section>
  </main>

  <footer>
    <div class="container">GitHub Pages</div>
  </footer>
</body>
</html>
