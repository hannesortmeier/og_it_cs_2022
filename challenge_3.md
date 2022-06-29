# IT-CS 2022 - Bei 100 Mrd. Clicksteam-Events & Backend-Daten den Überblick wahren? Mit SQL geht’s - Eine Coding-Challenge
## Challenge 3

Jedesmal, wenn ein User ein oder mehrere Produkte in einem der Otto Group online Shops kauft, wird ein
Eintrag in der Tabelle `oghub-analytics.it_cs_2022.purchase` erzeugt.

Berechne für jeden User die wöchentliche Anzahl der Käufe, sowie die wöchentliche Anzahl der Käufe in 
den jeweils vorherigen vier Wochen.


So könnte der Output einer Lösung aussehen:

| user_id                                                          | cw  | count_week_1 | count_week_2 | count_week_3 | count_week_4 | count_week_5 |
| ---------------------------------------------------------------- | --- | :----------: | :----------: | :----------: | :----------: | :----------: |
| 07be35e7c3c9b8aa828526a5eecdde50b36083195ac71d23b795941d2aa41385 | 8   |      1       |      0       |      0       |      1       |      0       |
| 0fda16d3332ec867d4c1d92351ff046b3be296abffb3ac9866d3b2fbfc0b21f6 | 8   |      0       |      4       |      1       |      1       |      1       |
| 1ee5adcce2da977539e03814b462d0e37cfa0c2acf240224de71220e53f3137e | 8   |      0       |      0       |      0       |      1       |      0       |
| 3dfcb0be70a89abeb201d4fcc4cfed15d32660b9cf8fcd905c4cefb1d84bef97 | 8   |      7       |      1       |      1       |      1       |      8       |
| a84cf32a55af25dafcb2de19c89c22ac9f9f419112cf6adcf24c652cc3baa1fa | 42  |      1       |      10      |      0       |      11      |      0       |
| 9a1c52eada3e02f333ce55ca94577befc74db8bc89ba7998140ba91e1e060421 | 37  |      0       |      0       |      0       |      2       |      9       |