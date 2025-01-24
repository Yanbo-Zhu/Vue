
- **_Datenvalidierung_** prüft Daten auf Einhaltung bestimmter zuvor aufgestellter Regeln, z.B. Einhaltung eines definierten Wertebereichs, einer vorgegebenen Syntax oder eines bestimmten Formats
- **_Datenverifizierung_** bezieht sich auf die Richtigkeit von Daten und prüft deren Korrektheit
- hier: Validierung von Benutzereingaben eines Formulars

![](image/Pasted%20image%2020250106155841.png)

# 1 Arten der Validierung

- _Clienseitige Validierung_: liefert schnelles Feedback an den Nutzer und entlastet den Server
- _Serverseitige Validierung_: Sicherstellung der Validität auch bei fremder Clientsoftware und Nutzung durch APIs
- _Beidseitige Validierung_: kombiniert die Vorteile client- und serverseitiger Validierung und ist hier bevorzugt

**Clientbasierte Validierungstechniken**
- Browserbasiert
- JavaScript Validation API für das DOM
- JavaScript Reguläre Ausdrücke (Regular Expressions)



# 2 Reguläre Ausdrücke

- **_Reguläre Ausdrücke_** (**_Regular Expressions_**): Beschreibung regulärer Sprachen, welche sich in der Chomsky Hierarchie auf der untersten Ebene befinden und damit die ausdrucksschwächsten Sprache darstellen
- Erzeugung durch reguläre Grammatiken
- Hier: Definition von Syntaxregeln zur Validierung von Nutzereingaben 
- Reguläre Ausdrücke können in Javascript auf zwei Arten erstellt werden
- Mit der Methode `**test**` können Zeichenketten auf die Gültigkeit des regulären Ausdrucks überprüft werden

![](image/Pasted%20image%2020250106160257.png)

![](image/Pasted%20image%2020250106160315.png)


---


![](image/Pasted%20image%2020250106160402.png)

```
[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$

(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}

`**^(http(s):\/\/.)[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)$**`
```


---


![](image/Pasted%20image%2020250106160414.png)

```

(0?[1-9]|[12][0-9]|3[01])[\/\-](0?[1-9]|1[012])[\/\-]\d{4}

(?:(?:31(\/|-|\.)(?:0?[13578]|1[02]))\1|(?:(?:29|30)(\/|-|\.)(?:0?[13-9]|1[0-2])\2))(?:(?:1[6-9]|[2-9]\d)?\d{2})$|^(?:29(\/|-|\.)0?2\3(?:(?:(?:1[6-9]|[2-9]\d)?(?:0[48]|[2468][048]|[13579][26])|(?:(?:16|[2468][048]|[3579][26])00))))$|^(?:0?[1-9]|1\d|2[0-8])(\/|-|\.)(?:(?:0?[1-9])|(?:1[0-2]))\4(?:(?:1[6-9]|[2-9]\d)?\d{2})

```

# 3 BROWSERBASIERTE VALIDIERUNG


```html
<form class="container">
  <br><br>
  <div class="mb-3">
    <label for="emailInput" class="form-label">Email address</label>
    <input type="email" class="form-control" id="emailInput">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

![](image/a3a5d123ff8262eafaf4a8fdbb3baa91.png)

- Moderne Browser führen eine automatische Validierung von Formulareingaben aus
- Validierungsregeln basieren auf dem `**type**`-Attribut des `**input**`-Elements (`**email**`, `**date**`, `**url**`, `**tel**`, `**number**`, `**password**`, ... ) und anderen Attributen, z.B. `**required**`, `**min**`, `**max**`
- Nachteile: Validierung ist in verschiedenen Browsern nicht einheitlich implementiert und Validierungsfeedback passt sich nicht an das Design der Webseite an


---
pattern 的使用 

- Regeln für die automatische Validierung durch den Browser können durch das `**pattern**`-Attribut angepasst werden
- `**pattern**` erlaubt die Hinterlegung eines regulären Ausdrucks (ohne die Anfangs- und Endbegrenzungen `**/**` und `**/**`), der bei der Validierung zum Einsatz kommt

```html
<form class="container">
  <br><br>
  <div class="mb-3">
    <label for="emailInput" class="form-label">Passwort muss mindestens
        einen Klein- , einen Großbuchstaben und eine Zahl haben.</label>
    <input type="text" pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}" 
           class="form-control" id="emailInput">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

# 4 JAVASCRIPT VALIDATION API

- Browserbasierte Validierung kann durch `**novalidate**` im `**form**`-Element unterbunden werden
- Autor der Webseite muss dann eine eigene JavaScript-Funktion für die Validierung schreiben
- Beispiel zeigt die Anwendung der Methode `**checkValidity()**` auf dem DOM-Element des Eingabefeldes, welche die Eingabe basierend auf dem im `**pattern**`-Attribut hinterlegten regulären Ausdruck prüft
- `**validate()**` wird bei Klick auf den Submit-Button aufgerufen und schreibt Validierungsfeedback in ein hinterlegtes `**div**`-Element

- Anstelle des `**pattern**`-Attributs kann der reguläre Ausdruck auch in JavaScript definiert werden
- Validierung erfolgt dann durch Übergabe der Zeichenkette an `**test**`-Methode des regulären Ausdrucks

```html
<form class="container" novalidate autocomplete="off"> 
  <br><br>
  <div class="mb-3">
    <label for="emailInput" class="form-label">Passwort muss mindestens
        einen Klein- , einen Großbuchstaben und eine Zahl haben.</label>
    <input type="text" pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}" 
           class="form-control" id="emailInput">
    <div id="validation-feedback">
    <!-- Hier wird der Feedback-Text reingeschrieben -->
    </div>
  </div>
  <button type="button" onclick="validate()" class="btn btn-primary">Submit</button>
</form>

<script>
  function validate() {
    const inputElement=document.getElementById("emailInput");
    if (!inputElement.checkValidity()) {
      document.getElementById("validation-feedback").innerHTML=
      "Falsches Format. Text muss vorgegebenes Format haben.";
    }
    return false;
  }
</script>
```

![](image/c0f018adf78674dee0d30fc7048ce652.png)


# 5 Validierungsergebnisse in Bootstrap

```html
<form class="container" novalidate>
  <br><br>
  <div class="mb-3">
    <label for="emailInput" class="form-label">Email address</label>
    <input type="email" class="form-control is-valid" id="emailInput">
    <div class="invalid-feedback">
      Die Emailadresse muss ein @-Zeichen enthalten.
    </div>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

![](image/Pasted%20image%2020250106161137.png)


- In Bootstrap kann das Ergebnis der Validierung durch die Klassen `**.is-valid**` (grüne Umrandung) und `**.is-invalid**` (rote Umrandung) auf dem Eingabefeld gegeben werden
- Zusätzlicher Feedbacktext kann durch ein `**div**` der Klasse `**.invalid-feedback**` und `**.valid-feedback**` eingeblendet werden
- Validierungslogik muss selber programmiert werden, Bootstrap validiert nicht


# 6 RegEx beispiel 


## 6.1 Flightnumber
Betrachten Sie in ComplaintForm.vue die Methode flightNumberValid(), die einen String übergeben bekommt. Eine Flugnummer besteht aus einer Kennung der Fluggesellschaft (z.B. LH für Lufthansa) und einer Zahl mit bis zu vier Stellen (z.B. 1934). Die Kennung, der sogenannte IATA-Airline-Code, besteht aus exakt zwei Zeichen (Großbuchstagen oder Ziffern).
Beachten Sie: Ein IATA-Airline-Code kann nicht aus zwei Ziffern bestehen.

Gefolgt wird die Kennung der Fluggesellschaft von einer Zahl mit bis zu vier Stellen. Führende Nullen können weggelassen oder angegeben werden. Außerdem kann die Flugnummer mit einem Leerzeichen von der Kennung der Fluggesellschaft getrennt werden.

Die folgenden Fälle sind valide Flugnummern:
’LH0001’, ’LH 0001’, ’LH001’, ’LH01’, ’LH1’, ’3X1’, ’X31’

Die folgenden Fälle sind keine gültigen Flugnummern:
’LHA 0001’, ’LH 00001’, ’111’, ’LH 1A’, ’lh1’.

```js
function flightNumberValid(value) {
  return (/^(?!\d{2})([A-Z\d]{2})(\s?)\d{1,4}$/g).test(value);
};
```

- `^`: Start der Zeichenkette. Das Muster muss ganz am Anfang der Zeichenkette beginnen.
- `(?!\d{2})`: negative Lookahead. Stellt sicher, dass die Zeichenkette nicht mit zwei Ziffern beginnt (\d{2}).
- `([A-Z\d]{2})`: eine Gruppe, die genau zwei Zeichen erfasst, die entweder Großbuchstaben (A-Z) oder Ziffern (\d) sind.
- `(\s?)`: eine optionale Gruppe (? bedeutet 0 oder 1 Mal), die ein Leerzeichen (\s) erfasst, falls vorhanden.
- `\d{1,4}`: eine Gruppe von 1 bis 4 Ziffern
- `$`: Ende der Zeichenkette. Das Muster muss die gesamte Zeichenkette abdecken.
- `g`: Der Modifikator ist hier nicht notwendig, da .test() immer nur den ersten Treffer prüft. (steht aber in der Musterlösung ![🤷🏼‍♂️](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==))


---

Die Methode .test() wird auf ein RegEx-Objekt angewendet und prüft, ob ein bestimmtes Muster in einer Zeichenkette enthalten ist. Sie gibt einen Boolean-Wert zurück:
- true: wenn das Muster mit der Zeichenkette übereinstimmt.
- false: wenn das Muster nicht übereinstimmt.

## 6.2 Date of flight
Betrachten Sie nun die Methode dateValid() die auch einen String übergeben bekommt. Ein valides Datum wird in der Regel im Format JJJJ-MM-TT angegeben (z.B. 0001-01-01). Ihre Funktion soll jedoch das Weglassen von führenden Nullen erlauben (also: 1-1-1). Darüber hinaus endet die Zeitrechnung (zumindest vorerst) mit dem Jahr 9999.

Die folgenden Fälle sind valide Datumsangaben:
’2022-01-01’, ’2022-1-1’, ’222-1-1’, ’2-1-1’, ’0002-01-1’
Die folgenden Fälle sind keine gültigen Datumsangaben:
’20222-01-01’, ’2022-13-01’, ’2022-01-32’

Beachten Sie: Sie können davon ausgehen, dass alle Monate 31 Tage haben, von demher sind auch Schaltjahre irrelevant. ’2024-02-31’ ist also auch ein valides Datum, während ’2024-02-32 nicht valide ist.


```js
function dateValid(value) {
  return (/^(\d{1,4})(-)((0?[1-9])|1[0-2])(-)((0?[1-9])|[12]\d|3[0-1])$/g).test(value);
};
```

- ^: Start der Zeichenkette.
- (\d{1,4}): sucht nach einer Jahreszahl mit 1 bis 4 Ziffern.
- (-): stellt sicher, dass ein Bindestrich - zwischen den Abschnitten steht.
- `((0?[1-9])|1[0-2])`: validiert den Monat.
    - `0?[1-9]`: Monatswerte von 01 bis 09 (oder 1 bis 9 ohne führende Null)
    - `1[0-2]`: Monatswerte 10, 11 oder 12
- `((0?[1-9])|[12]\d|3[0-1])`: validiert den Tag.
    - ` 0?[1-9]`: Tageswerte von 01 bis 09 (oder 1 bis 9 ohne führende Null)
    - `[12]\d`: Tageswerte von 10 bis 29
    - `3[0-1]`: Tageswerte von 30 bis 31
- `$`: Ende der Zeichenkette. Die gesamte Eingabe muss mit dem Muster übereinstimmen.


## 6.3 IBAN

Betrachten Sie abschließend auch die Methode ibanValid(), die auch einen String übergeben bekommt. Diese soll überprüfen, ob eine valide IBAN (kurz für: International Bank Account Number) eingegeben wurde. In der Realität folgt diese einem relativ komplexen Format, das von Land zu Land ein wenig variiert. In Deutschland besteht die IBAN aus 22 Zeichen. Sie beginnt mit dem Ländercode DE gefolgt von einer zwei-stelligen Prüfsumme (eine Zweistellige Zahl).

Die restlichen 18 Zeichen sind ausschließlich Ziffern. Ein Leerzeichen zwischen Ländercode und Prüfsumme ist nicht zulässig, jedoch ein optionales Leerzeichen zwischen Prüfsumme und den restlichen 18 Zeichen, sowie ein optionales Leerzeichen nach jeweils 4 Ziffern.

Darüber hinaus sollen Sie anhand der Prüfsumme die IBAN validieren. Dazu müssen die 18 Ziffern aufsummiert und der Modulo von 100 bestimmt werden. Ist dieser identisch mit der Prüfsumme ist die IBAN gültig. Die Methode ibanValid() gibt also true zurück, wenn die IBAN im korrekten FOrmat angegeben wurde und die IBAN anhand der Prüfsumme auf Gültigkeit überprüft wurde. Sonst gibt diese false zurück. Hinweis: Das tatsächliche Verfahren zur Überprüfung der Gültigkeit von IBAN ist ähnlich, aber technisch etwas aufwendiger.

Die folgenden Fälle sind valide IBAN:
’DE18111111111111111111’, ’DE18 1111 1111 1111 1111 11’,
’DE18 11111111 11111111 11’, ’DE18 1111111111111111 11’

Die folgenden Fälle sind keine gültigen IBAN:
’DE11 1111 1111 1111 1111 11’, ’BE18 1111 1111 1111 1111 11’,
’DE11 1111 1111 1111 1111 1111’, ’DE11 1111 1111’



```js
function hasValidChecksum(iban) {
  iban = iban.replace(/\s/g,'');
  let checksum = Number.parseInt(iban.slice(2, 4));
  let sum = iban
    .slice(4)
    .split('')
    .reduce((acc, val) => acc += Number.parseInt(val), 0);
  return checksum === sum % 100;
};

```

Funktionserklärung:
- iban.replace(/\s/g,'')
    - entfernt alle Leerzeichen aus der IBAN, um sicherzustellen, dass das Format keine ungewollten Abstände enthält
- iban.slice(2, 4)
    - extrahiert die Prüfziffer (2 Zeichen ab Index 2)
- iban.slice(4).split('').reduce(...)
    - nimmt den numerischen Teil der IBAN ab Index 4, teilt ihn in einzelne Ziffern und summiert sie
- checksum === sum % 100
    - prüft, ob die Prüfziffer gleich der Summe der restlichen Ziffern modulo 100 ist

---


```js
function ibanValid(value) {
	let isValidFormat = (/^(DE)(\d{2})((\s?)(\d{4})){4}(\s?)(\d{2})$/g).test(value);
  return isValidFormat && hasValidChecksum(value);
};
```


- `^`: Start der Zeichenkette
- `(DE)`: Länderkennung DE
- `(\d{2})`: zweistellige Prüfziffer
- `((\s?)(\d{4})){4}`: vier Blöcke von je 4 Ziffern (\d{4}), optional getrennt durch Leerzeichen (\s?).
- `(\s?)(\d{2})`: optionales Leerzeichen und zwei weitere Ziffern am Ende.
- `$`: Ende der Zeichenkette


