<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HackTheBox Writeups | Muzec</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/marked/9.1.6/marked.min.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Inter', sans-serif;
            background: #0a0e17;
            color: #e4e4e7;
            line-height: 1.6;
        }

        header {
            padding: 1.5rem 5%;
            background: rgba(10, 14, 23, 0.95);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(34, 197, 94, 0.2);
            position: sticky;
            top: 0;
            z-index: 100;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-size: 1.3rem;
            font-weight: 700;
            color: #22c55e;
            text-decoration: none;
        }

        .back-btn {
            color: #a1a1aa;
            text-decoration: none;
            padding: 0.5rem 1rem;
            border: 1px solid rgba(34, 197, 94, 0.3);
            border-radius: 6px;
            transition: all 0.3s;
        }

        .back-btn:hover {
            background: rgba(34, 197, 94, 0.1);
            border-color: #22c55e;
            color: #22c55e;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 3rem 5%;
        }

        h1 {
            font-size: 2.5rem;
            color: #22c55e;
            margin-bottom: 1rem;
        }

        .subtitle {
            color: #71717a;
            margin-bottom: 3rem;
            font-size: 1.1rem;
        }

        .writeups-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 1.5rem;
        }

        .writeup-card {
            background: rgba(20, 29, 43, 0.5);
            border: 1px solid rgba(34, 197, 94, 0.2);
            border-radius: 12px;
            padding: 1.5rem;
            cursor: pointer;
            transition: all 0.3s;
        }

        .writeup-card:hover {
            transform: translateY(-5px);
            border-color: #22c55e;
            box-shadow: 0 10px 30px rgba(34, 197, 94, 0.2);
        }

        .writeup-card h3 {
            color: #22c55e;
            margin-bottom: 0.5rem;
            font-size: 1.3rem;
        }

        .difficulty {
            display: inline-block;
            padding: 0.3rem 0.8rem;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: 600;
            margin-bottom: 0.8rem;
        }

        .easy { background: rgba(34, 197, 94, 0.2); color: #22c55e; }
        .medium { background: rgba(251, 191, 36, 0.2); color: #fbbf24; }
        .hard { background: rgba(239, 68, 68, 0.2); color: #ef4444; }
        .insane { background: rgba(139, 92, 246, 0.2); color: #8b5cf6; }

        .writeup-card p {
            color: #a1a1aa;
            font-size: 0.95rem;
            margin-bottom: 0.8rem;
        }

        .date {
            color: #71717a;
            font-size: 0.85rem;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            z-index: 1000;
            overflow-y: auto;
        }

        .modal-content {
            max-width: 900px;
            margin: 2rem auto;
            background: #0f1419;
            border: 1px solid rgba(34, 197, 94, 0.2);
            border-radius: 12px;
            padding: 3rem;
        }

        .close-btn {
            float: right;
            font-size: 2rem;
            color: #71717a;
            cursor: pointer;
            line-height: 1;
        }

        .close-btn:hover { color: #22c55e; }

        .markdown-content {
            color: #e4e4e7;
        }

        .markdown-content h1, .markdown-content h2 {
            color: #22c55e;
            margin-top: 2rem;
            margin-bottom: 1rem;
        }

        .markdown-content h3 { color: #10b981; margin-top: 1.5rem; }
        .markdown-content code {
            background: rgba(34, 197, 94, 0.1);
            padding: 0.2rem 0.4rem;
            border-radius: 4px;
            color: #22c55e;
        }

        .markdown-content pre {
            background: #141d2b;
            padding: 1rem;
            border-radius: 8px;
            overflow-x: auto;
            border: 1px solid rgba(34, 197, 94, 0.2);
        }

        .markdown-content pre code {
            background: none;
            color: #e4e4e7;
        }

        @media (max-width: 768px) {
            h1 { font-size: 2rem; }
            .writeups-grid { grid-template-columns: 1fr; }
            .modal-content { padding: 2rem 1.5rem; }
        }
    </style>
</head>
<body>
    <header>
        <a href="../index.html" class="logo">muzec@hackwell:~$</a>
        <a href="../index.html" class="back-btn">← Back to Home</a>
    </header>

    <div class="container">
        <h1>HackTheBox Writeups</h1>
        <p class="subtitle">Detailed walkthroughs and exploitation techniques</p>

        <div class="writeups-grid" id="writeups"></div>
    </div>

    <div class="modal" id="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <div class="markdown-content" id="content"></div>
        </div>
    </div>

    <script>
        // Sample writeups data - replace with your actual data
        const writeups = [
            { title: "Sau", difficulty: "easy", date: "2024-01-15", file: "sau.md", preview: "Command injection via SSRF leading to CVE exploitation" },
            { title: "Keeper", difficulty: "easy", date: "2024-01-10", file: "keeper.md", preview: "KeePass exploitation with CVE-2023-32784" },
            { title: "BoardLight", difficulty: "medium", date: "2023-12-20", file: "boardlight.md", preview: "Dolibarr CMS exploitation and privilege escalation" },
            { title: "Hospital", difficulty: "medium", date: "2023-12-15", file: "hospital.md", preview: "File upload bypass and RoundCube exploitation" },
            { title: "Analytics", difficulty: "hard", date: "2023-11-30", file: "analytics.md", preview: "Metabase pre-auth RCE and kernel exploitation" },
            { title: "Manager", difficulty: "hard", date: "2023-11-25", file: "manager.md", preview: "Active Directory certificate services abuse" }
        ];

        function renderWriteups() {
            const grid = document.getElementById('writeups');
            grid.innerHTML = writeups.map(w => `
                <div class="writeup-card" onclick="loadWriteup('${w.file}')">
                    <h3>${w.title}</h3>
                    <span class="difficulty ${w.difficulty}">${w.difficulty.toUpperCase()}</span>
                    <p>${w.preview}</p>
                    <span class="date">${w.date}</span>
                </div>
            `).join('');
        }

        function loadWriteup(file) {
            const modal = document.getElementById('modal');
            const content = document.getElementById('content');
            
            // In production, fetch from: `/posts/hackthebox/${file}`
            // For demo, showing sample markdown
            const sampleMarkdown = `# ${file.replace('.md', '').toUpperCase()} - HackTheBox

## Overview
This is a sample writeup. Replace this content by fetching your actual markdown files.

## Reconnaissance
\`\`\`bash
nmap -sC -sV -oN nmap.txt 10.10.11.224
\`\`\`

## Initial Access
Description of exploitation steps...

## Privilege Escalation
Steps to gain root access...

---
*Add your actual writeup content here*`;

            content.innerHTML = marked.parse(sampleMarkdown);
            modal.style.display = 'block';
        }

        function closeModal() {
            document.getElementById('modal').style.display = 'none';
        }

        window.onclick = (e) => {
            if (e.target.id === 'modal') closeModal();
        };

        renderWriteups();
    </script>
</body>
</html>