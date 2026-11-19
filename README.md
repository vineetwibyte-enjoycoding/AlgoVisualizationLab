# AlgoVisualizationLab
This repository contains code for visualizing algos. 
# ALGO-VISUALIZE

Interactive Python and Pygame algorithm visualizations running directly in the browser via **PyScript** and hosted on **GitHub Pages**.

---

## 📁 Folder Structure

Every visualization must live inside its own **lowercase, hyphenated folder** (no spaces). Each folder needs two files: `index.html` and `main.py`.

```text
ALGO-VISUALIZE/
├── README.md
├── two-pointers/
│   ├── index.html
│   └── main.py
└── binary-search/
    ├── index.html
    └── main.py
```

Once pushed to the `main` branch, GitHub Pages automatically publishes each folder within 1–2 minutes at:
`https://<your-github-username>.github.io/ALGO-VISUALIZE/<folder-name>/`

---

## 🚀 How to Add a New Visualization

1. **Create a new folder** in the root of this repository using a lowercase name (for example, `merge-sort`).
2. **Add `index.html`** inside that folder using the exact template below.
3. **Add `main.py`** containing your Pygame code (make sure it follows the browser rules below).
4. **Commit and push** directly to `main`. Wait ~2 minutes, then test your live link.

---

## 📄 Standard `index.html` Template

Copy this exact file into every new visualization folder without changing anything:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Algorithm Visualization</title>
    <link rel="stylesheet" href="https://pyscript.net">
    <script type="module" src="https://pyscript.net"></script>
    <style>
        body {
            margin: 0;
            background-color: #1e1e1e;
            color: #ffffff;
            font-family: system-ui, -apple-system, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
        }
        canvas {
            border: 1px solid #444;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
        }
        #loading {
            margin-bottom: 12px;
            font-size: 0.95rem;
            color: #aaa;
        }
    </style>
</head>
<body>
    <div id="loading">Loading Python & Pygame environment...</div>
    <canvas id="canvas"></canvas>

    <script type="py-game" src="./main.py"></script>

    <script>
        window.addEventListener('py:ready', () => {
            const loader = document.getElementById('loading');
            if (loader) loader.style.display = 'none';
        });
    </script>
</body>
</html>
```

---

## 🐍 Python & Pygame Rules for the Browser

Because your Python code runs inside a browser tab rather than a desktop window, follow these three rules in `main.py`:

### 1. Make `while` loops asynchronous
The browser will freeze if a `while` loop runs without yielding control. Import `asyncio` and add `await asyncio.sleep(1 / 60)` inside **every** animation loop:

```python
import asyncio
import pygame

pygame.init()
screen = pygame.display.set_mode((800, 600))

running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Your drawing and algorithm update code here
    screen.fill((30, 30, 30))
    pygame.display.flip()

    # Required so the browser updates the screen
    await asyncio.sleep(1 / 60)
```

### 2. Avoid `SysFont`
Browsers cannot access your computer's system fonts. Use Pygame's built-in font instead:
* **Use:** `pygame.font.Font(None, 24)`
* **Avoid:** `pygame.font.SysFont("arial", 24)`

### 3. Keep file names lowercase
GitHub Pages is case-sensitive. Always name your script `main.py` (all lowercase), matching `src="./main.py"` in `index.html`.

---

## 💻 Testing Locally Before Pushing

Double-clicking `index.html` will not work due to browser security rules. To test on your computer:
1. Open a terminal inside your visualization folder.
2. Run `python -m http.server`
3. Open `http://localhost:8000` in your browser.
