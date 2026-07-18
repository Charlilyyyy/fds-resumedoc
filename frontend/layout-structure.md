# Layout Structure

## Purpose

Define the HTML skeleton of `index.html`: navbar, hero upload area with role dropdown, and the hidden dashboard grid populated after analysis.

Implements the structural part of requirement **F11**.

---

## Page regions

| Region | Element id | Visibility |
|--------|-----------|-----------|
| Navbar | `.nav` / `.logo` | Always |
| Hero + upload | `#hero` | Shown first, hidden after submit |
| Loader | `#loader` | Shown during request |
| Dashboard | `#dashboard` | Hidden until response |

---

## Head

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ResumeDoc</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>/* see styling.md */</style>
</head>
```

Chart.js is loaded from CDN before the dashboard script runs.

---

## Navbar

```html
<div class="nav">
  <div class="logo">ResumeDoc</div>
</div>
```

---

## Hero + upload form

```html
<div class="hero" id="hero">
  <h1>Analyze Your Resume <span>Instantly</span></h1>
  <p>Get ATS score, skills & improvements in seconds</p>

  <div class="upload">
    <form id="form">
      <input type="file" id="file" required><br>
      <select id="role" required>
        <option value="">Select Job Role</option>
        <option>Java Developer</option>
        <option>Full Stack Developer</option>
        <option>Data Analyst</option>
      </select>
      <button class="btn">Analyze Now</button>
    </form>
    <div class="loader" id="loader"></div>
  </div>
</div>
```

| Element | Purpose |
|---------|---------|
| `#file` (required) | PDF selection; blocks empty submit |
| `#role` (required) | Three roles + empty placeholder |
| `.btn` | Triggers form submit |
| `#loader` | Spinner during fetch |

The three `<option>` values must match backend role keys exactly.

---

## Dashboard grid

```html
<div class="dashboard" id="dashboard">
  <div class="grid">
    <div class="card">
      <h3>Score</h3>
      <canvas id="scoreChart"></canvas>
      <div class="scoreText" id="scoreText"></div>
    </div>
    <div class="card">
      <h3>Skills</h3>
      <div id="skills" class="skills"></div>
    </div>
    <div class="card">
      <h3>Skill Analysis</h3>
      <canvas id="barChart"></canvas>
    </div>
  </div>

  <div class="grid" style="margin-top:20px;">
    <div class="card">
      <h3>Suggestions</h3>
      <ul id="suggestions"></ul>
    </div>
    <div class="card">
      <h3>Insight</h3>
      <div id="ai" class="ai"></div>
    </div>
    <div class="card">
      <h3>ATS Analysis</h3>
      <p id="atsScore"></p>
      <p id="atsMissing"></p>
    </div>
    <div class="card">
      <h3>Resume Sections</h3>
      <p id="sections"></p>
    </div>
    <div class="card">
      <h3>Career Roadmap</h3>
      <p id="roadmap"></p>
    </div>
  </div>
</div>
```

---

## Element ↔ data map

| id | Filled from |
|----|-------------|
| `scoreChart`, `scoreText` | Score |
| `skills` | Skills list |
| `barChart` | Skills (bar) |
| `suggestions` | Suggestions |
| `ai` | Score band (client-side) |
| `atsScore`, `atsMissing` | ATS block |
| `sections` | Resume Sections block |
| `roadmap` | Career Roadmap block |

---

## Checklist

- [ ] Navbar, hero, dashboard regions present
- [ ] File + role inputs `required`
- [ ] Role options match backend keys
- [ ] Dashboard hidden initially
- [ ] All result element ids present

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [styling.md](./styling.md) — CSS theme and animations.
