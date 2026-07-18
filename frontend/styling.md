# Styling

## Purpose

Define the CSS look and feel of the dashboard: dark theme, animated gradient background, glassmorphism cards, buttons, skill tags, and the loading spinner.

Implements the visual part of requirement **F11** (AC-UI10).

---

## Design tokens

| Token | Value |
|-------|-------|
| Background | `#020617` (near-black navy) |
| Accent 1 | `#38bdf8` (sky) |
| Accent 2 | `#6366f1` (indigo) |
| Text | `#ffffff` / muted `#94a3b8` |
| Card surface | `rgba(255,255,255,0.05)` + blur |
| Radius | 15–20px |
| Font | `Segoe UI`, sans-serif |

---

## Base + background

```css
* { margin:0; padding:0; box-sizing:border-box; font-family:'Segoe UI', sans-serif; }

body { background:#020617; color:white; overflow-x:hidden; }

body::before {
  content:"";
  position:fixed;
  width:200%; height:200%;
  background:
    radial-gradient(circle, #0ea5e9 0%, transparent 40%),
    radial-gradient(circle, #6366f1 0%, transparent 40%);
  animation:moveBg 10s linear infinite;
  z-index:-1;
}

@keyframes moveBg {
  0%   { transform:translate(0,0); }
  50%  { transform:translate(-25%,-25%); }
  100% { transform:translate(0,0); }
}
```

---

## Navbar & hero

```css
.nav { display:flex; justify-content:space-between; padding:20px 40px; backdrop-filter:blur(10px); }

.logo {
  font-size:20px; font-weight:bold;
  background:linear-gradient(45deg, #38bdf8, #6366f1);
  -webkit-background-clip:text; color:transparent;
}

.hero { height:90vh; display:flex; flex-direction:column; justify-content:center; align-items:center; text-align:center; }
.hero h1 { font-size:3.5rem; }
.hero span { color:#38bdf8; }
.hero p { margin-top:10px; color:#94a3b8; }
```

---

## Upload card & button

```css
.upload {
  margin-top:30px; padding:30px; border-radius:20px;
  background:rgba(255,255,255,0.05);
  backdrop-filter:blur(20px);
  box-shadow:0 10px 40px rgba(0,0,0,0.6);
  transition:0.3s;
}
.upload:hover { transform:scale(1.05); }

.btn {
  margin-top:15px; padding:12px 25px; border:none; border-radius:30px;
  background:linear-gradient(45deg, #38bdf8, #6366f1);
  color:white; cursor:pointer; transition:0.3s;
}
.btn:hover { box-shadow:0 0 30px #38bdf8; }
```

---

## Dashboard, grid & cards

```css
.dashboard { display:none; padding:40px; }

.grid { display:grid; grid-template-columns:repeat(3, 1fr); gap:20px; }

.card {
  padding:20px; border-radius:20px;
  background:rgba(255,255,255,0.05);
  backdrop-filter:blur(20px);
  box-shadow:0 10px 40px rgba(0,0,0,0.5);
  transition:0.3s;
}
.card:hover { transform:translateY(-10px); }
```

`.dashboard { display:none }` keeps results hidden until JS reveals them.

---

## Skill tags & score text

```css
.skills span {
  background:linear-gradient(45deg, #38bdf8, #6366f1);
  padding:6px 12px; margin:5px; border-radius:10px; display:inline-block;
}

.scoreText { font-size:28px; text-align:center; margin-top:10px; }
```

---

## Loader & insight

```css
.loader {
  display:none; margin:auto;
  border:6px solid #1e293b; border-top:6px solid #38bdf8;
  width:50px; height:50px; border-radius:50%;
  animation:spin 1s linear infinite;
}
@keyframes spin { 100% { transform:rotate(360deg); } }

.ai {
  margin-top:20px; padding:20px; border-radius:15px;
  background:rgba(56,189,248,0.1); font-size:18px;
}
```

---

## Responsiveness note

The base grid is 3 columns. For small screens, an optional media query can stack cards:

```css
@media (max-width:768px) {
  .grid { grid-template-columns:1fr; }
  .hero h1 { font-size:2.2rem; }
}
```

Optional for v1; core requirement is a working dark dashboard.

---

## Checklist

- [ ] Dark theme + animated gradient
- [ ] Glassmorphism cards with hover
- [ ] Gradient button + logo
- [ ] Skill tag styling
- [ ] Loader spinner keyframes
- [ ] Dashboard hidden by default

---

## Link to prior context

Structure: [layout-structure.md](./layout-structure.md).  
Next: [upload-and-fetch.md](./upload-and-fetch.md) — submit handling and fetch.
