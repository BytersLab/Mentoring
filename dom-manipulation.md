<br><br><br>

# 🌳 DOM-Grundlagen (Document Object Model)

Der **DOM** ist die Baum-Repräsentation deiner HTML-Seite im Speicher. Jeder Knoten ist ein **Element** (z. B. `<div>`), **Text** oder **Attribut**. Mit JavaScript kannst du diesen Baum lesen, verändern, neue Zweige einfügen oder alte entfernen.

Mini-Setup (ES-Module):

```html
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="utf-8" />
    <title>DOM Basics</title>
  </head>
  <body>
    <main id="app"></main>
    <script type="module" src="./main.js"></script>
  </body>
</html>
```

<br><br><br>

# 🔎 Elemente finden & im Baum navigieren

```js
// main.js
const app = document.querySelector("#app"); // erstes Match
const buttons = document.querySelectorAll("button.primary"); // NodeList

// Traversieren
const card = app.querySelector(".card");
const parent = card?.parentElement;
const firstChild = card?.firstElementChild;
const nearestForm = card?.closest("form"); // sucht nach oben

// Existenz prüfen
if (!card) {
  console.warn("Keine .card gefunden");
}
```

**Tipps:**

- `querySelector(All)` ist die moderne Wahl.
- `closest(selector)` ist Gold für Event-Delegation.

<br><br><br>

# ✍️ Inhalte & Attribute ändern

```js
const title = document.createElement("h1");
title.textContent = "Hallo DOM!"; // sicher gegen XSS
app.append(title);

// Attribute
title.setAttribute("data-testid", "page-title");
console.log(title.dataset.testid); // "page-title"

// HTML gezielt einfügen (nur mit vertrauenswürdigem HTML!)
title.insertAdjacentHTML("afterend", `<p class="lead">Willkommen 👋</p>`);
```

**Merke:**

- **`textContent`** für reinen Text (sicher).
- **`innerHTML`/`insertAdjacentHTML`** nur, wenn das HTML vertrauenswürdig oder **vorher sanitiziert** ist.

<br><br><br>

# 🎨 Klassen & Styles steuern

```js
title.classList.add("mb-24");
title.classList.toggle("highlight"); // an/aus
title.style.marginTop = "1rem";

// CSS-Variablen dynamisch setzen (z. B. Theme)
document.documentElement.style.setProperty("--brand-hue", "260");
```

**Best Practice:** Styles möglichst in CSS halten und im JS nur **Klassen toggeln**.

<br><br><br>

# 🧱 Knoten erstellen, einfügen, ersetzen, löschen

```js
// Erstellen
const cardEl = document.createElement("article");
cardEl.className = "card";

// Inhalt mit Fragment (performant)
const frag = document.createDocumentFragment();
for (const txt of ["Alpha", "Beta", "Gamma"]) {
  const p = document.createElement("p");
  p.textContent = txt;
  frag.append(p);
}
cardEl.append(frag);

// Einfügen
app.prepend(cardEl); // auch: append, before, after
// Ersetzen
// cardEl.replaceWith(neuesEl);
// Entfernen
// cardEl.remove();
```

<br><br><br>

# 📨 Formulare & FormData

```html
<form id="login">
  <input name="email" type="email" required />
  <input name="password" type="password" required minlength="8" />
  <button>Login</button>
</form>
```

```js
const form = document.querySelector("#login");

form.addEventListener("submit", async (e) => {
  e.preventDefault();
  if (!form.reportValidity()) return; // native Validierung

  const fd = new FormData(form);
  const payload = Object.fromEntries(fd.entries());

  try {
    const res = await fetch("/api/login", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(payload),
    });
    if (!res.ok) throw new Error("Login fehlgeschlagen");
    // Erfolg -> UI updaten
    form.replaceWith(document.createTextNode("Eingeloggt ✅"));
  } catch (err) {
    console.error(err);
    form.querySelector("button").textContent = "Nochmal versuchen";
  }
});
```
