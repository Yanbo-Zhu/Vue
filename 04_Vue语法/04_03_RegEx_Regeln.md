

- Zeichen
    - Normale Zeichen: Buchstaben, Ziffern und andere Zeichen (z. B. abc, 123) stehen für sich selbst.
    - Escape-Sequenzen: Ein Backslash \ wird verwendet, um Sonderzeichen (z. B. . oder *) als normale Zeichen zu behandeln.
- Zeichenklassen
    - `.`: beliebiges einzelnes Zeichen (außer Zeilenumbruch)
        - a.c findet „abc“, „a1c“, aber nicht „ac“.
    - `[ ]`: Zeichenbereich oder -gruppe
        - `[aeiou]` sucht nach einem beliebigen Vokal.
        - `[0-9]` sucht nach einer beliebigen Ziffer.
        - `[^]`: Negation (z. B. [^aeiou] findet alles außer Vokale)
- Vordefinierte Zeichenklassen
    - `\d`: Eine Ziffer (0–9)
    - \D: Alles außer Ziffern
    - \w: Ein alphanumerisches Zeichen (a–z, A–Z, 0–9, _)
    - \W: Alles außer alphanumerischen Zeichen
    - \s: Ein Leerraumzeichen (Leerzeichen, Tab, Zeilenumbruch)
    - \S: Alles außer Leerzeichen
- Wiederholungszeichen
    - `*`: 0 oder mehr Wiederholungen
        - ab* findet „a“, „ab“, „abb“, usw.
    - +: 1 oder mehr Wiederholungen
        - ab+ findet „ab“, „abb“, aber nicht „a“.
    - ?: 0 oder 1 Wiederholun
        - ab? findet „a“ oder „ab“.
    - {n}: Genau n Wiederholunge
        - a{3} findet „aaa“
    - {n,}: Mindestens n Wiederholungen
        - a{2,} findet „aa“, „aaa“, usw.
    - {n,m}: Zwischen n und m Wiederholungen
        - a{2,4} findet „aa“, „aaa“, „aaaa“
- Positionierungsanker
    - ^: Anfang einer Zeile
        - ^abc findet „abc“ am Zeilenanfang.
    - $: Ende einer Zeile
        - abc$ findet „abc“ am Zeilenende.
    - \b: Wortgrenze
        - \bcat\b findet „cat“, aber nicht „scatter“.
    - \B: keine Wortgrenze
- Gruppierung und Alternativen
    - ( ): Gruppierung
        - (ab)+ findet „ab“, „abab“, usw.
    - |: “oder”
        - cat|dog findet „cat“ oder „dog“.
- Lookahead und Lookbehind
    - (?=...): positiver Lookahead
        - \d(?=px) findet eine Ziffer vor „px“.
    - (?!...): negativer Lookahead.
        - \d(?!px) findet eine Ziffer, die nicht vor „px“ steht.
    - (?<=...): positiver Lookbehind.
    - (?<!...): negativer Lookbehind.
- Modifikatoren
    - i: Groß-/Kleinschreibung ignorieren
    - g: global (findet alle Treffer, nicht nur den ersten)
    - m: mehrzeilig (Anker wie ^ und $ gelten für jede Zeile)




