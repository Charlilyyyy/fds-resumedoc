# Upload & Fetch

## Purpose

Define the client logic that intercepts the form submit, builds `FormData`, calls `POST /resume/upload`, manages the loader, and hands the response text to the parser.

Implements the client side of requirements **F1/F2** and **F12**.

---

## Submit handler

```html
<script>
const form = document.getElementById("form");
const loader = document.getElementById("loader");
const hero = document.getElementById("hero");
const dashboard = document.getElementById("dashboard");

form.addEventListener("submit", async (e) => {
  e.preventDefault();

  const fileInput = document.getElementById("file");
  const role = document.getElementById("role").value;

  const formData = new FormData();
  formData.append("file", fileInput.files[0]);
  formData.append("role", role);

  loader.style.display = "block";

  try {
    const res = await fetch("/resume/upload", {
      method: "POST",
      body: formData
    });
    const text = await res.text();

    loader.style.display = "none";
    hero.style.display = "none";
    dashboard.style.display = "block";

    renderDashboard(text);   // see response-parsing.md
  } catch (err) {
    loader.style.display = "none";
    alert("Something went wrong. Please try again.");
  }
});
</script>
```

---

## Key points

| Concern | Handling |
|---------|----------|
| Field names | `file` and `role` — must match `@RequestParam` names in controller |
| Content type | Not set manually; browser sets `multipart/form-data` boundary for `FormData` |
| Method | `POST` |
| Response | Read as `text()` (plain string, not JSON) |
| Loader | Shown before fetch, hidden after |
| View switch | Hide hero, show dashboard on success |

---

## Why not set Content-Type

Setting `Content-Type` manually with `FormData` breaks the multipart boundary. Let the browser set it automatically.

---

## Field name contract

```
formData.append("file", ...)  ⟷  @RequestParam("file") MultipartFile file
formData.append("role", ...)  ⟷  @RequestParam("role") String role
```

Mismatched names cause a 400 (missing parameter). See [controller-design.md](../api-layer/controller-design.md).

---

## Validation

- `#file` and `#role` are `required` in HTML, so the browser blocks empty submits before JS runs.
- Non-PDF or corrupt files are handled server-side ([error-handling.md](../pdf-extraction/error-handling.md)); the response text will contain the error message string.

---

## Checklist

- [ ] `preventDefault()` on submit
- [ ] `FormData` with `file` and `role`
- [ ] `fetch` POST to `/resume/upload`
- [ ] Response read as text
- [ ] Loader show/hide
- [ ] Hero hidden, dashboard shown on success
- [ ] `try/catch` with user-facing error

---

## Link to prior context

Structure: [layout-structure.md](./layout-structure.md).  
Next: [response-parsing.md](./response-parsing.md) — extracting fields from the response text.
