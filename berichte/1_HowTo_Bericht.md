---
title: "Berichte mit Github Pages"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "ITSE"
date: "2025-10-01"
---

# 1. Aufgabenstellung
Aufsetzen und Einrichten der Laborumgebung.


# Arbeitsbericht: Stylischer Arbeitsbericht mit GitHub Pages

## 1. Zielsetzung
Dieser Arbeitsbericht beschreibt, wie mit **GitHub Pages** eine Webseite erstellt werden kann, auf der Arbeitsberichte oder Dokumentationen im **Markdown-Format (md)** modern und übersichtlich dargestellt werden.  
Das Ziel ist eine klar strukturierte und optisch ansprechende Lösung, die ohne zusätzliche Tools nutzbar ist.

---

## 2. Voraussetzungen
- GitHub-Account [https://github.com](https://github.com)  
- Git installiert (optional, wenn lokal gearbeitet wird)  
- Grundkenntnisse in Markdown (`.md` Dateien)

---

## 3. Repository erstellen
1. Auf GitHub ein neues Repository erstellen, z. B. **arbeitsberichte** oder **Klasse_Fach_Name**.  
2. In den Einstellungen (`Settings`) unter **Pages** aktivieren:
   - Branch auswählen: `main`  
   - Ordner: `/docs`  
3. Nach dem Speichern ist die Seite unter  
   `https://<username>.github.io/arbeitsberichte/` erreichbar.

---

## 4. Ordnerstruktur anlegen
Empfohlene Struktur:

```none
/_layouts
└── default.html → Grundlayout für Seiten
/assets
└── style.css → eigenes Design
/docs/ → Inhalte für GitHub Pages
├── index.md → Startseite
└── faecher/ → Unterordner für Arbeitsberichte
```
oder
```none
/_layouts
└── default.html → Grundlayout für Seiten
/assets
└── style.css → eigenes Design
/docs/ → Inhalte für GitHub Pages
├── index.md → Startseite
└── berichte/ → Unterordner für Arbeitsberichte
```


---

## 5. Markdown-Dateien schreiben

Beispiel `index.md`:

```markdown
# Willkommen zu den Arbeitsberichten

Dokumentationen zu den einzelnen Fächern:

- [SYTI](faecher/syti.md)
- [MEDT](faecher/medt.md)
- [NWT](faecher/nwt.md)
- [SYTD](faecher/sytd.md)
```

oder für ein einzelnes Fach:

```markdown
# Arbeitsberichte ITSE

- [LAB Umgebung](berichte/2509022.md)
- [NMAP](berichte/251009.md)
```

### 5.1 Beispiel eines Arbeitsberichtes

```markdown
---
title: "ITSE Starten mit der Umgebung"
author: "Name"
klasse: "4AHITS"
subject: "ITSE"
date: "2025-09-19"
---

# 1. Aufgabenstellung
Aufsetzen und Einrichten der Laborumgebung.
```

---

## 6. Styling hinzufügen

Damit die GitHub Page ein modernes Erscheinungsbild erhält, kann ein eigenes CSS-Theme verwendet werden.

Datei: `assets/style.css`


```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

:root {
  --bg: #f5f7fa;
  --text: #333;
  --header-gradient: linear-gradient(135deg, #4A90E2, #7B4397);
  --meta-bg: rgba(255, 255, 255, 0.18);
  --meta-text: #fff;
  --box-bg: #fff;
  --accent: #4A90E2;
}

body.dark {
  --bg: #1e1e2f;
  --text: #e0e0e0;
  --header-gradient: linear-gradient(135deg, #7B4397, #4A90E2);
  --meta-bg: rgba(255, 255, 255, 0.1);
  --meta-text: #f0f0f0;
  --box-bg: #2a2a3b;
  --accent: #7dafff;
}

body {
  font-family: 'Poppins', sans-serif;
  background: var(--bg);
  color: var(--text);
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  line-height: 1.7;
  transition: background 0.3s, color 0.3s;
}

```

Button + Script einfügen (z. B. in index.md oder Layout)
```html
<button class="theme-toggle" onclick="toggleTheme()">🌙 / ☀️</button>

<script>
  function toggleTheme() {
    const current = document.documentElement.getAttribute("data-theme");
    const next = current === "dark" ? "light" : "dark";
    document.documentElement.setAttribute("data-theme", next);
    localStorage.setItem("theme", next);
  }

  // Beim Laden gespeicherte Einstellung setzen
  (function() {
    const saved = localStorage.getItem("theme") || "light";
    document.documentElement.setAttribute("data-theme", saved);
  })();
</script>
```

[Erweiterte Version](https://felix-mackinger.github.io/4AHITS_ITSE_Mackinger/berichte/HowTo/style.html)

Damit können Besucher jederzeit zwischen hellem und dunklem Modus wechseln.
Die Einstellung bleibt im Browser gespeichert.

---

## 7. YAML-Konfiguration

Damit Jekyll die Seiten korrekt rendert, empfiehlt sich eine `_config.yml`:


```markdown
title: "Arbeitsberichte"
description: "Markdown-basierte Dokumentation"
theme: null   # kein GitHub-Theme, es wird ein eigenes Layout genutzt
markdown: kramdown
```

---


## 8. HTML-Layout

Datei: `_layouts/default.html`

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{ page.title }} – {{ site.title }}</title>
  <link rel="stylesheet" href="{{ '/assets/style.css' | relative_url }}">
</head>
<body>
  <header>
    <button class="theme-toggle" onclick="toggleTheme()">🌙/☀️</button>
    <h1>{{ page.title }}</h1>
    <p class="meta">
      <strong>Fach:</strong> {{ page.subject }} |
      <strong>Klasse:</strong> {{ page.klasse }} |
      <strong>Autor:</strong> {{ page.author }} |
      <strong>Datum:</strong> {{ page.date }}
      {% if page.partner %}
        | <strong>Partner:</strong> {{ page.partner }}
      {% endif %}
    </p>
  </header>

  <main>
    {{ content }}
  </main>

  <footer>
    © {{ site.time | date: "%Y" }} – {{ site.title }}
  </footer>

  <script>
    function toggleTheme() {
      document.body.classList.toggle('dark');
      localStorage.setItem('theme',
        document.body.classList.contains('dark') ? 'dark' : 'light');
    }
    // gespeicherte Einstellung laden
    if(localStorage.getItem('theme') === 'dark'){
      document.body.classList.add('dark');
    }
  </script>
</body>
</html>
```
--- 

## 9. Arbeitsbericht abgabe

Um eine einfache abgabe zu gewährleisten, sucht man sich gleich am Anfang von seinem GitHub Pages die Webseite wo dann die HTML's angezeigt werden.

Beispiel:
[https://nutzername.github.io/repo/](https://felix-mackinger.github.io/4AHITS_ITSE_Mackinger/)

## 10. Zusatz

Markdown codespace highlighting

| *Sprache* | *Label für Markdown* |
|---------|-----------------|
| Allgemeiner Text / kein Highlight | `none` oder `text` |
| Bash / Shell | `bash`, `sh`, `shell` |
| Python | `python` |
| JavaScript | `js`, `javascript` |
| HTML | `html` |
| CSS | `css` |
| C# | `csharp` |
| C / C++ | `c`, `cpp` |
| Java | `java` |
| JSON | `json` |
| YAML | `yaml` |
| Markdown | `markdown` |
| SQL | `sql` |
| Ruby | `ruby` |
| PHP | `php` |

*PDF einfügen*
```code
[Alternativer Text der Beschreibt](https://repo-name.github.io/repo-name/weg/zum/pdf/DasPDF.pdf)
```

## 11. Ergebnis

GitHub Pages zeigt die Arbeitsberichte online an.
Das Layout ist modern, klar strukturiert und mit Light-/Dark-Mode nutzbar.
Alle Inhalte können mit Markdown einfach gepflegt und erweitert werden.


