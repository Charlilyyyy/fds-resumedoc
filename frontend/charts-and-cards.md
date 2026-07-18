# Charts & Cards

## Purpose

Define the rendering functions that turn parsed fields into visuals: a doughnut score chart, a skills bar chart, result cards (skills, suggestions, ATS, sections, roadmap), and a client-side insight band.

Implements the visualization part of requirement **F11**.

---

## Chart lifecycle guard

Charts must be destroyed before redraw, or a re-submit stacks canvases:

```js
let scoreChartRef = null;
let barChartRef = null;
```

Each draw function checks and destroys the previous instance.

---

## Score doughnut

```js
function drawScore(score) {
  const ctx = document.getElementById("scoreChart");
  if (scoreChartRef) scoreChartRef.destroy();
  scoreChartRef = new Chart(ctx, {
    type: "doughnut",
    data: {
      labels: ["Score", "Remaining"],
      datasets: [{ data: [score, 100 - score],
        backgroundColor: ["#38bdf8", "#1e293b"], borderWidth: 0 }]
    },
    options: { cutout: "70%", plugins: { legend: { display: false } } }
  });
  document.getElementById("scoreText").innerText = score + "/100";
}
```

---

## Skills bar

```js
function drawBar(skills) {
  const ctx = document.getElementById("barChart");
  if (barChartRef) barChartRef.destroy();
  barChartRef = new Chart(ctx, {
    type: "bar",
    data: {
      labels: skills.length ? skills : ["No skills"],
      datasets: [{ label: "Skills",
        data: skills.length ? skills.map(() => 1) : [0],
        backgroundColor: "#6366f1" }]
    },
    options: { plugins: { legend: { display: false } },
      scales: { y: { display: false } } }
  });
}
```

---

## Cards

```js
function fillSkills(skills) {
  document.getElementById("skills").innerHTML =
    skills.length ? skills.map(s => `<span>${s}</span>`).join("")
                  : "<span>No skills detected</span>";
}

function fillSuggestions(list) {
  document.getElementById("suggestions").innerHTML =
    list.length ? list.map(s => `<li>${s}</li>`).join("")
                : "<li>Looks great!</li>";
}

function fillATS(score, missing) {
  document.getElementById("atsScore").innerText = "ATS Score: " + score + "%";
  document.getElementById("atsMissing").innerText = "Missing: " + (missing || "none");
}

function fillSections(block) {
  document.getElementById("sections").innerText = block || "Not detected";
}

function fillRoadmap(block) {
  document.getElementById("roadmap").innerText = block || "No roadmap";
}
```

---

## Insight band (client-side)

A friendly summary derived from the score band — no server call:

```js
function drawInsight(score) {
  let msg;
  if (score >= 80)      msg = "Excellent resume — you're interview ready!";
  else if (score >= 50) msg = "Good start — close a few skill gaps to stand out.";
  else                  msg = "Needs work — focus on core skills and projects.";
  document.getElementById("ai").innerText = msg;
}
```

| Score | Insight |
|-------|---------|
| ≥ 80 | Interview ready |
| 50–79 | Good start, close gaps |
| < 50 | Needs work |

---

## Re-submit safety

| Risk | Guard |
|------|-------|
| Duplicate charts | `destroy()` before new `Chart()` |
| Stale cards | `innerHTML`/`innerText` overwrite each render |

---

## Checklist

- [ ] Doughnut score chart + score text
- [ ] Skills bar chart
- [ ] Skills / suggestions / ATS / sections / roadmap cards filled
- [ ] Insight band from score
- [ ] Charts destroyed before redraw

---

## Link to prior context

Parsing: [response-parsing.md](./response-parsing.md).  
Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — browser testing and close.
