<br><br><br>

# 🧱 JavaScript-Objekte (Objects) – „Schlüssel → Wert“

Ein **Objekt** ist eine Sammlung von _Schlüssel–Wert_-Paaren. Schlüssel (Keys) sind Strings oder Symbols, Werte können alles sein: Zahlen, Strings, Funktionen, Arrays, andere Objekte.

```js
// Erstellen
const user = {
  id: 42,
  name: "Ada",
  active: true,
  roles: ["admin", "editor"],
  greet() {
    return `Hi, ich bin ${this.name}!`;
  },
};

// Zugriff
console.log(user.name); // "Ada"
console.log(user["roles"]); // ["admin", "editor"]
console.log(user.greet()); // "Hi, ich bin Ada!"

// Hinzufügen / Ändern / Löschen
user.email = "ada@example.com";
user.active = false;
delete user.roles;

// Existenz prüfen
console.log("email" in user); // true
```

**Wichtig:** Objekte sind **referenz-basiert**. Wenn du sie zuweist, teilst du eine Referenz – nicht eine Kopie.

<br><br><br>

# 📦 JavaScript-Arrays – geordnete Listen

Ein **Array** ist eine geordnete Liste von Werten (beliebiger Typ). Typische Operationen sind _hinzufügen, filtern, abbilden (map), reduzieren_.

```js
const nums = [3, 1, 4];

// Lesen & Schreiben
console.log(nums[0]); // 3
nums[1] = 99; // [3, 99, 4]

// Länge & push/pop/shift/unshift
nums.push(7); // [3, 99, 4, 7]
const last = nums.pop(); // last=7, nums=[3, 99, 4]
nums.unshift(0); // [0, 3, 99, 4]
nums.shift(); // [3, 99, 4]
```

<br><br><br>

# 🔁 Iteration über Arrays & Objekte

```js
const arr = ["a", "b", "c"];
for (const value of arr) {
  console.log(value); // a, b, c
}

const book = { title: "Clean Code", year: 2008 };
for (const key in book) {
  console.log(key, book[key]); // "title Clean Code", "year 2008"
}

// Moderne Objekt-Iteration
Object.keys(book).forEach((k) => console.log(k));
Object.values(book).forEach((v) => console.log(v));
Object.entries(book).forEach(([k, v]) => console.log(k, v));
```

<br><br><br>

# 🛠️ Häufige Array-Methoden (praktisch im Alltag)

```js
const users = [
  { id: 1, name: "Ada", score: 92 },
  { id: 2, name: "Alan", score: 67 },
  { id: 3, name: "Grace", score: 81 },
];

// map: Form ändern
const names = users.map((u) => u.name); // ["Ada","Alan","Grace"]

// filter: Auswahl treffen
const passed = users.filter((u) => u.score >= 80); // Ada & Grace

// reduce: auf einen Wert verdichten
const avg = users.reduce((sum, u, _, a) => sum + u.score / a.length, 0);

// find / some / every
const alan = users.find((u) => u.name === "Alan"); // Objekt oder undefined
const anyLow = users.some((u) => u.score < 70); // true
const allHigh = users.every((u) => u.score >= 60); // true

// sort (Achtung: mutiert das Array!)
const byScoreAsc = [...users].sort((a, b) => a.score - b.score);
```

<br><br><br>

# 🗝️ Nützliche Objekt-Utilities

```js
const config = { retries: 3, timeout: 2000 };

const keys = Object.keys(config); // ["retries","timeout"]
const vals = Object.values(config); // [3, 2000]
const entries = Object.entries(config); // [["retries",3],["timeout",2000]]

const obj = Object.fromEntries([
  ["lang", "de"],
  ["theme", "dark"],
]);
// { lang: "de", theme: "dark" }
```

<br><br><br>

# 🧩 Destructuring – elegant Werte „auspacken“

```js
// Array-Destructuring
const point = [10, 20];
const [x, y] = point; // x=10, y=20
const [first, , third] = ["a", "b", "c"]; // first="a", third="c"

// Objekt-Destructuring
const person = { name: "Ada", city: "London" };
const { name, city: hometown, age = 30 } = person;
// name="Ada", hometown="London", age=30 (Default)

// Funktion mit Destructuring
const connect = ({ host, port = 5432 }) => `Verbinde zu ${host}:${port}`;
console.log(connect({ host: "db.local" }));
```

<br><br><br>

# 🧵 Spread & Rest – kopieren, mergen, sammeln

```js
// Spread bei Arrays (flache Kopie)
const a = [1, 2];
const b = [...a, 3]; // [1,2,3]

// Spread bei Objekten (flache Kopie/Merge)
const base = { a: 1, b: 2 };
const merged = { ...base, b: 9, c: 3 }; // { a:1, b:9, c:3 }

// Rest-Parameter
const sum = (...nums) => nums.reduce((s, n) => s + n, 0);
console.log(sum(1, 2, 3)); // 6
```

<br><br><br>

# 🧊 Immutabilität & sichere Kopien

Viele Methoden (z. B. `sort`, `push`, `splice`) **mutieren** Arrays/Objekte. In modernen Apps (React/Redux etc.) arbeitet man oft **immutabel**:

```js
// Array ohne Mutation erweitern
const xs = [1, 2, 3];
const xs2 = [...xs, 4]; // statt xs.push(4)

// Objekt-Feld ändern (ohne Mutation)
const settings = { theme: "dark", lang: "de" };
const newSettings = { ...settings, lang: "en" };

// Tiefe Kopie (nested)
const nested = { a: { b: 1 } };
const deepCopy = structuredClone(nested); // Node 17+/Browser modern
deepCopy.a.b = 2; // nested bleibt unverändert
```

<br><br><br>

# 🧭 Verschachtelte Strukturen (Nested Objects/Arrays)

```js
const school = {
  classes: [
    { id: "js-101", students: [{ name: "Ada" }, { name: "Alan" }] },
    { id: "py-201", students: [{ name: "Guido" }] },
  ],
};

// sicher zugreifen
const firstStudentName = school.classes?.[0]?.students?.[0]?.name ?? "n/a";

// tief updaten (immutabel)
const updated = {
  ...school,
  classes: school.classes.map((c) =>
    c.id === "js-101"
      ? { ...c, students: [...c.students, { name: "Grace" }] }
      : c
  ),
};
```

<br><br><br>

# 🔄 Umwandeln: Objekt ⇄ Array

```js
const counts = { apples: 3, bananas: 5 };

// Objekt → Array von Paaren
const pairs = Object.entries(counts); // [["apples",3],["bananas",5]]

// Array transformieren …
const doubled = pairs.map(([k, v]) => [k, v * 2]); // [["apples",6],["bananas",10]]

// … und zurück in ein Objekt
const doubledObj = Object.fromEntries(doubled);
// { apples: 6, bananas: 10 }
```
