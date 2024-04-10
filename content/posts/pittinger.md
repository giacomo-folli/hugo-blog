---
title: "Pittinger"
date: "2024-01-04T09:09:07+01:00"
tags: ['slovenia']
---

Una poesia dentro a Javascript

```js
function returnLines(lines) {
  return lines.join(" ");
}

let verse_1 = [
  "La birra Pittinger è una chiara leggera che sembra fanta,\n",
  "non rimane sullo stomaco e non la senti\n",
  "quando ti alzi dubbioso per andare al bagno.\n",
  "\n",
];

let verse_2 = [
  "È una birra slovena\n",
  "con la latta nera e grigia e scritte che non riconosco,\n",
  "4,2%, confezionata in Austria.\n",
  "Ne abbiamo bevute 5\n",
  "in cinque, condividendone ognuna.\n",
  "Le abbiamo aperte una alla volta\n",
  "e le abbiamo bevute.\n",
  "\n",
];

let verse_3 = [
  "La Pittinger è buona e leggera.\n",
  "Ho ascoltato i miei amici cantare\n",
  "e parlare sopra un film.\n",
  "Mi sono bevuto la Slovenia\n",
  "in qualche sorso e fino all'ultimo ho riso.\n",
];

for (let verse of [verse_1, verse_2, verse_3]) {
    console.log(returnLines(verse))
}
```
