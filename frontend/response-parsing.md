# Response Parsing

## Purpose

Define how the client extracts fields from the plain-text labeled response using regular expressions, inside `renderDashboard(text)`.

Implements the client side of requirement **F12** (AC-PARSE).

---

## Why regex

The backend returns a labeled plain-text body (not JSON) — see [response-assembly.md](../sections-roadmap/response-assembly.md). The client uses small regexes to pull each field.

---

## Parsing helpers

```js
function renderDashboard(text) {
  const score      = parseInt((text.match(/Score: (\d+)/) || [])[1] || 0);
  const skillsRaw  = (text.match(/Skills: \[(.*?)\]/) || [])[1] || "";
  const suggRaw    = (text.match(/Suggestions: \[(.*?)\]/) || [])[1] || "";
  const atsScore   = (text.match(/ATS Score: (\d+)%/) || [])[1] || "0";
  const atsMissing = (text.match(/Missing Keywords: \[(.*?)\]/) || [])[1] || "";
  const sections   = (text.match(/Resume Sections:\n([\s\S]*?)\n\n/) || [])[1] || "";
  const roadmap    = (text.match(/Career Roadmap[\s\S]*/) || [])[0] || "";

  const skills = skillsRaw ? skillsRaw.split(",").map(s => s.trim()).filter(Boolean) : [];
  const suggestions = suggRaw ? suggRaw.split(",").map(s => s.trim()).filter(Boolean) : [];

  // pass to renderers — see charts-and-cards.md
  fillSkills(skills);
  fillSuggestions(suggestions);
  fillATS(atsScore, atsMissing);
  fillSections(sections);
  fillRoadmap(roadmap);
  drawScore(score);
  drawBar(skills);
  drawInsight(score);
}
```

---

## Field regex table

| Field | Regex | Result |
|-------|-------|--------|
| Score | `/Score: (\d+)/` | number |
| Skills | `/Skills: \[(.*?)\]/` | CSV in brackets |
| Suggestions | `/Suggestions: \[(.*?)\]/` | CSV in brackets |
| ATS score | `/ATS Score: (\d+)%/` | number |
| ATS missing | `/Missing Keywords: \[(.*?)\]/` | CSV in brackets |
| Sections | `/Resume Sections:\n([\s\S]*?)\n\n/` | multi-line block |
| Roadmap | `/Career Roadmap[\s\S]*/` | trailing block |

---

## Defensive defaults

Each match uses `(... || [])[1] || fallback` so a missing field does not throw. If the body is the error string (`Error while processing resume ❌`), all fields fall back to empty/zero and the dashboard renders blank rather than crashing.

---

## Known limitation — comma split

Skills and suggestions are split on `,`. Suggestion text itself contains commas (e.g. "HTML, CSS and responsive design"), so a single suggestion may split into fragments. This is an accepted v1 rendering simplification, noted in [suggestions.md](../scoring/suggestions.md). A JSON contract (future scope) removes this.

---

## Checklist

- [ ] One regex per field
- [ ] Safe fallbacks (no throw on missing field)
- [ ] Skills/suggestions split + trimmed
- [ ] Sections captured as block
- [ ] Roadmap captured to end

---

## Link to prior context

Fetch: [upload-and-fetch.md](./upload-and-fetch.md).  
Next: [charts-and-cards.md](./charts-and-cards.md) — rendering charts, cards, and the insight band.
