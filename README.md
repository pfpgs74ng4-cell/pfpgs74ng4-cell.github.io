<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Home</title>
  <style>
    :root {
      --bg: #0d1117;
      --border: #30363d;
      --text: #f0f6fc;
      --muted: #8b949e;
      --link: #58a6ff;
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
      width: min(900px, calc(100% - 40px));
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

    nav a:hover { color: var(--text); }

    main { padding: 64px 0 100px; }

    section { margin-bottom: 64px; }

    h2 {
      margin: 0 0 18px;
      padding-bottom: 12px;
      border-bottom: 1px solid var(--border);
      font-size: 22px;
    }

    ul {
      margin: 0;
      padding-left: 22px;
    }

    li {
      padding: 10px 0;
      color: var(--muted);
      line-height: 1.6;
    }

    li a {
      color: var(--link);
      text-decoration: none;
      font-weight: 500;
    }

    li a:hover { text-decoration: underline; }

    .repo-description {
      display: block;
      margin-top: 4px;
      max-width: 760px;
      color: var(--muted);
      font-size: 14px;
    }

    .repo-meta {
      display: inline-block;
      margin-top: 5px;
      font-size: 12px;
      color: #6e7681;
    }

    .status { color: var(--muted); }

    footer {
      padding: 28px 0;
      border-top: 1px solid var(--border);
      color: var(--muted);
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
          <a href="#repos">Repos</a>
          <a href="#tools">Tools</a>
          <a href="https://github.com/pfpgs74ng4-cell">GitHub</a>
        </div>
      </nav>
    </div>
  </header>

  <main class="container">
    <section id="repos">
      <h2>Repos</h2>
      <ul id="repo-list">
        <li class="status">Loading public repositories...</li>
      </ul>
    </section>

    <section id="tools">
      <h2>Tools</h2>
      <ul>
        <li>
          <a href="/raw-bayer-viewer/">RAW Bayer Viewer</a>
          — Preview Bayer RAW images in the browser
        </li>
      </ul>
    </section>
  </main>

  <footer>
    <div class="container">GitHub Pages</div>
  </footer>

  <script>
    const githubUser = "pfpgs74ng4-cell";
    const homepageRepo = `${githubUser}.github.io`;
    const repoList = document.getElementById("repo-list");

    function escapeHtml(value) {
      return String(value ?? "")
        .replaceAll("&", "&amp;")
        .replaceAll("<", "&lt;")
        .replaceAll(">", "&gt;")
        .replaceAll('"', "&quot;")
        .replaceAll("'", "&#039;");
    }

    function cleanMarkdown(markdown) {
      if (!markdown) return "";

      let text = markdown
        .replace(/```[\s\S]*?```/g, " ")
        .replace(/`([^`]+)`/g, "$1")
        .replace(/!\[[^\]]*\]\([^)]*\)/g, " ")
        .replace(/\[([^\]]+)\]\([^)]*\)/g, "$1")
        .replace(/<img[^>]*>/gi, " ")
        .replace(/<[^>]+>/g, " ")
        .replace(/^#{1,6}\s+.*$/gm, " ")
        .replace(/^\s*[-*_]{3,}\s*$/gm, " ")
        .replace(/^\s*>\s?/gm, "")
        .replace(/^\s*[-*+]\s+/gm, "")
        .replace(/^\s*\d+\.\s+/gm, "")
        .replace(/\*\*([^*]+)\*\*/g, "$1")
        .replace(/__([^_]+)__/g, "$1")
        .replace(/\*([^*]+)\*/g, "$1")
        .replace(/_([^_]+)_/g, "$1")
        .replace(/\r/g, "")
        .replace(/\n{2,}/g, "\n")
        .trim();

      const lines = text
        .split("\n")
        .map(line => line.trim())
        .filter(line => {
          if (!line) return false;
          if (/^https?:\/\//i.test(line)) return false;
          if (/^\[!?\[.*badge/i.test(line)) return false;
          return true;
        });

      return lines.join(" ").replace(/\s+/g, " ").trim();
    }

    function summarize(text, maxLength = 220) {
      if (!text) return "";
      if (text.length <= maxLength) return text;
      return text.slice(0, maxLength).replace(/\s+\S*$/, "") + "…";
    }

    async function fetchReadmeSummary(repoName) {
      try {
        const response = await fetch(
          `https://api.github.com/repos/${githubUser}/${repoName}/readme`,
          { headers: { "Accept": "application/vnd.github.raw+json" } }
        );

        if (!response.ok) return "";

        const markdown = await response.text();
        return summarize(cleanMarkdown(markdown));
      } catch {
        return "";
      }
    }

    async function loadRepos() {
      try {
        const response = await fetch(
          `https://api.github.com/users/${githubUser}/repos?type=public&sort=updated&per_page=100`,
          { headers: { "Accept": "application/vnd.github+json" } }
        );

        if (!response.ok) {
          throw new Error(`GitHub API returned ${response.status}`);
        }

        const repos = await response.json();

        const visibleRepos = repos
          .filter(repo => !repo.fork)
          .filter(repo => repo.name !== homepageRepo)
          .sort((a, b) => new Date(b.updated_at) - new Date(a.updated_at));

        repoList.innerHTML = "";

        if (visibleRepos.length === 0) {
          repoList.innerHTML = '<li class="status">No public repositories yet.</li>';
          return;
        }

        for (const repo of visibleRepos) {
          const li = document.createElement("li");
          const summary = await fetchReadmeSummary(repo.name);
          const displayText = summary || repo.description || "";

          li.innerHTML = `
            <a href="${repo.html_url}" target="_blank" rel="noreferrer">
              ${escapeHtml(repo.name)}
            </a>
            ${displayText ? `<span class="repo-description">${escapeHtml(displayText)}</span>` : ""}
            ${repo.language ? `<span class="repo-meta">${escapeHtml(repo.language)}</span>` : ""}
          `;

          repoList.appendChild(li);
        }
      } catch (error) {
        console.error(error);
        repoList.innerHTML =
          '<li class="status">Unable to load repositories from GitHub.</li>';
      }
    }

    loadRepos();
  </script>
</body>
</html>
