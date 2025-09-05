<br><br><br>

# ⚙️ Was sind asynchrone Funktionen? (Grundlagen)

In JavaScript laufen viele Dinge **asynchron** ab – also **zeitversetzt**. Zum Beispiel:

- Warten auf eine Antwort vom Server (`fetch`)
- Timer (`setTimeout`)
- Lesen von Dateien (z. B. in Node.js)

Damit dein Code nicht „stehen bleibt“, benutzt JavaScript sogenannte **Promises**. Diese versprechen: „Ich liefere dir später ein Ergebnis“.
Und genau hier kommen **`async` & `await`** ins Spiel – damit du mit diesen Promises **einfacher und lesbarer** arbeiten kannst.

<br><br><br>

# 🔁 Promise kurz erklärt

Ein **Promise** ist ein Platzhalter für ein zukünftiges Ergebnis. Es hat 3 Zustände:

1. `pending` → läuft noch
2. `fulfilled` → erfolgreich abgeschlossen
3. `rejected` → Fehler passiert

Ein einfaches Beispiel:

```js
const warte = (ms) =>
  new Promise((resolve) => {
    setTimeout(() => resolve(`Fertig nach ${ms}ms`), ms);
  });
```

Verwendung ohne `async/await` (klassisch mit `.then()`):

```js
warte(1000).then((result) => console.log(result));
```

Mit `async/await` viel lesbarer:

```js
const start = async () => {
  const result = await warte(1000);
  console.log(result);
};
start(); // => Fertig nach 1000ms
```

<br><br><br>

# ✨ `async` – macht eine Funktion „asynchron“

Sobald du vor einer Funktion `async` schreibst, passiert Folgendes:

- Die Funktion **gibt immer ein Promise zurück**
- Du kannst darin `await` verwenden

```js
async function sagHallo() {
  return "Hallo!";
}
sagHallo().then(console.log); // → "Hallo!"
```

Auch als Pfeilfunktion:

```js
const sagHallo = async () => "Hallo!";
```

<br><br><br>

# 🕰️ `await` – auf ein Ergebnis warten

Mit `await` wartest du auf das Ergebnis eines Promises – ohne `.then()` und ohne Callback-Hölle.

```js
const holDaten = async () => {
  const res = await fetch("https://jsonplaceholder.typicode.com/users");
  const data = await res.json();
  console.log(data);
};
holDaten();
```

**Wichtig: `await` funktioniert nur innerhalb von `async`-Funktionen.**

<br><br><br>

# 🔐 Fehlerbehandlung mit `try/catch`

Da `await` Fehler „auslöst“, wenn ein Promise fehlschlägt (z. B. Netzwerkfehler), musst du es mit `try/catch` abfangen:

```js
const ladeDaten = async () => {
  try {
    const res = await fetch("https://api.example.com/data");
    if (!res.ok) throw new Error(`HTTP Fehler: ${res.status}`);
    const data = await res.json();
    console.log("Daten geladen:", data);
  } catch (err) {
    console.error("Fehler beim Laden:", err.message);
  }
};
```

<br><br><br>

# 🧪 Beispiel: Wartefunktion und parallele Aufrufe

```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

const testAsync = async () => {
  console.log("Start");
  await delay(1000);
  console.log("Nach 1 Sekunde");
  await delay(500);
  console.log("Noch 0.5 Sekunde später");
};

testAsync();
```

Oder mehrere Promises gleichzeitig ausführen:

```js
const ladeAlle = async () => {
  const [users, posts] = await Promise.all([
    fetch("https://jsonplaceholder.typicode.com/users").then((res) =>
      res.json()
    ),
    fetch("https://jsonplaceholder.typicode.com/posts").then((res) =>
      res.json()
    ),
  ]);
  console.log("Users:", users);
  console.log("Posts:", posts);
};
ladeAlle();
```

<br><br><br>

# 🔁 Warten in Schleifen: `for...of` + `await`

Wenn du viele Items **nacheinander** verarbeiten willst:

```js
const urls = [
  "https://jsonplaceholder.typicode.com/users/1",
  "https://jsonplaceholder.typicode.com/users/2",
];

const ladeEinzeln = async () => {
  for (const url of urls) {
    const res = await fetch(url);
    const user = await res.json();
    console.log("User:", user.name);
  }
};
ladeEinzeln();
```
