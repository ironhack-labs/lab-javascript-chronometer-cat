![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# LAB | JS IronChronometer

![giphy (1)](https://user-images.githubusercontent.com/76580/167427065-a674fb55-44ea-448a-a7ef-940b45eeb9df.gif)

## Introducció

En aquest laboratori, crearem un [cronòmetre](https://www.dictionary.com/browse/chronometer) . Els cronòmetres són molt utilitzats en els esports: curses de cotxes, atletisme, etc. Per què no practicar una mica els nostres coneixements de manipulació de JS i DOM i crear el nostre propi IronChronometer? I després, podem fer-lo servir per veure quants minuts i segons ens portarà completar qualsevol dels nostres laboratoris. Sona com un pla.

Anem allà.

Aquestes són les nostres fites:

1. El nostre cronòmetre tindrà una _pantalla LCD_ , on veurem els minuts i els segons avançant.
2. També tindrà dos botons diferents que canviaran el comportament depenent de l'estat del cronòmetre. Per exemple, el botó inicial es convertirà en un botó de parada quan el cronòmetre funcioni.
3. Com a extra, afegirem una [funcionalitat de divisió](https://www.google.com/search?q=chronometer+split\&oq=chronometer+split) que ens permetrà registrar el temps quan polsem el botó de divisió.

Ho farem!

Per comprovar com hauria de ser la versió final comprova aquesta **[demo](https://sandrabosk.github.io/demo-chrono/index.html)**.

## Requisits

- Feu un fork d'aquest repo.
- Clona aquest repo.

## La presentació

- En acabar, executa les ordres següents:

```shell
$ git add .
$ git commit -m "solve iteration x, y, z"
$ git push origin master
```

- Crea un Pull Request perquè les TAs puguin comprovar la teva feina.

## Instruccions

Per començar, se'ns proporcionen els següents arxius i carpetes:

```shell
    ├── README.md
    ├── index.html
    ├── javascript
    │   ├── chronometer.js
    │   └── index.js
    ├── styles
    │   ├── fonts
    │   │   ├── ds-digi.ttf
    │   └── style.css
    └── tests
        └── chronometer.spec.js
```

<br/>

La fulla d'estils ja té inserida la font `ds-digi` . Aquesta font ens ajuda a tenir una pantalla LCD clàssica per assolir els estils dels cronòmetres tradicionals.

També hem creat el rellotge perquè et centris a la part de JavaScript d'aquest exercici. Feu clic a baix per veure la imatge que hauríeu d'obtenir quan obriu el fitxer `index.html` :

<br/>

<details><summary> Feu clic aquí per veure la imatge </summary>

<br/>

![](https://education-team-2020.s3-eu-west-1.amazonaws.com/web-dev/labs/chronometer.png)

</details>

**Aquest laboratori està dividit en dues parts principals** :

- Part 1: la lògica (que afegiràs al fitxer `javascript/chronometer.js` ).
- Part 2: la manipulació del DOM, per poder representar visualment i mostrar la lògica prèviament escrita (el codi que afegiràs al `javascript/index.js` ).

La teva solució requerirà l'ús dels mètodes globals `setInterval` i `clearInterval` .

[`setInterval`](https://developer.mozilla.org/en-US/docs/Web/API/setInterval) pot ser cridat amb una funció com a primer argument i un nombre de mil·lisegons com a segon argument. Executarà aquesta funció cada nombre de mil·lisegons que li hagis passat.

En ser anomenat, `setInterval` retorna un nombre que pot ser usat per a identificar l' _interval_ que va ser inicialitzat. Aquest mateix interval pot ser detingut posteriorment executant `clearInterval` i passant-li l'id de l'interval que volem interrompre.

### Iteració 1: La lògica

A la primera part del LAB, es treballarà al fitxer `javascript/chronometer.js` .

#### La classe `Chronometer`

Crearem la nostra classe `Cronometer` . El mètode `constructor` no hauria d'esperar cap argument. Hauria d'inicialitzar dues propietats del cronòmetre:

- `currentTime` , que ha de començar com el número `0` .
- `intervalId` , que ha de començar com a `null` .

Procedim a la creació dels mètodes del `Cronometer` .

#### Mètode `start`

La classe `Cronometer` necessita tenir un mètode `start` . Quan sigui cridat, `start` començarà a portar el compte del temps, executant una funció en un interval d'1 segon, que incrementarà en `1` la quantitat de segons emmagatzemats a la propietat `currentTime` .

Haureu de confiar en el mètode `setInterval` per aconseguir això. L'id de l'interval que es torna en trucar a `setInterval` ha de ser assignat a la nostra propietat `intervalId` , d'aquesta manera podrem esborrar-lo més tard quan necessitem aturar el temporitzador.

A més, el mètode `start` ha d'acceptar una funció com a argument. Anomenem-la `callback` . L'argument `callback` és opcional. Si s'anomena a `start` i se li passa un `callback` , aquest `callback` s'ha d'executar dins de la funció que se li ha passat a `setInterval` . Si no es passa cap trucada de retorn, cal ignorar-la (pista: heu de comprovar _si_ s'ha passat la `callback` de retorn abans d'intentar executar-la).

:bulb: _Pista 1_ : Tingueu en compte que si passa una declaració de funció al mètode setInterval `()` (escrivint `setInterval(function () {/* */})` ), la paraula clau `this` no es referirà a l'objecte _cronòmetre_ , sinó a l'àmbit global. Per permetre la referència al cronòmetre mitjançant l'accés a `this` , passeu una expressió de funció (una anomenada funció de fletxa) al mètode `setInterval()` (escrivint al seu lloc `setInterval(() => {/* *` /})).

#### Mètode `getMinutes`

Estem emmagatzemant el nombre de segons que han passat a la propietat `currentTime` . Tot i això, podríem voler saber quants minuts han passat.

El mètode `getMinutes` no hauria de prendre arguments, i hauria de tornar el _nombre_ de minuts que han passat com un enter, com un nombre enter. Podríem utilitzar el mètode `Math.floor()` per obtenir un nombre arrodonit, usant l'hora actual i dividint-la per .

#### Mètode `getSeconds`

Ara podem obtenir el número de minuts que han passat. Però, què passa si volem obtenir el nombre de segons que han passat després de l'inici del minut actual?

El mètode `getSeconds` ha de tornar el nombre de segons que han passat després de l'inici del minut actual.

Per exemple, si la propietat `currentTime` té `75` , `getSeconds` hauria de tornar `15` . Si `currentTime` té `210` , `getSeconds` hauria de tornar `30` , i així successivament.

Podríem utilitzar l'operador de mòdul (% 60) per obtenir el número final de segons.

Consell: L'operador [matemàtic resta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Remainder) podria estar molt útil en aquesta situació.

#### Mètode `computeTwoDigitNumber`

El nostre cronòmetre té una pantalla súper xula que necessita un número de dos dígits per mostrar els minuts i els segons. De vegades, però, els mètodes `getMinutes` i `getSeconds` retornen un nombre d'un dígit. Crearem un algoritme súper senzill que convertirà en un nombre de dos dígits qualsevol valor rebut.

El mètode `computeTwoDigitNumber` ha de prendre un número, i tornar una cadena on el nombre rebut com a argument ha estat emplenat amb 0s per assegurar que el valor té almenys 2 caràcters (podríem fer servir el mètode `.slice()` per aconseguir el nostre objectiu).

Per exemple, si s'anomena `computeTwoDigitNumber` amb el número `7` , hauríeu de tornar una cadena amb el valor `"07"` . Si es truca amb el número `36` , hauríeu de tornar una cadena amb el valor de `"36` ".

Més endavant, utilitzarem el mètode `computeTwoDigitNumber` per formatar els valors retornats per `getMinutes` i `getSeconds` i mostrar-los al nostre cronòmetre.

#### Mètode `stop`

Ja podem engegar el nostre cronòmetre. Crearem un mètode que l'aturi.

En ser invocat, el mètode `stop` ha d'esborrar l'interval amb l'id que s'havia emmagatzemat a la propietat `intervalId` Id . És tan simple com això.

:bulb: _Suggeriment_: Utilitza `clearInterval` .

#### Mètode `reset`

El mètode `reset(` ) reiniciarà el nostre cronòmetre. Ja que el nostre codi és súper net, només hem de tornar a posar la nostra propietat `currentTime` a 0, i ja està!

També necessitem restablir els valors al nostre fitxer HTML, usant `.innerHTML` .

#### Mètode `split`

En certs moments, és possible que vulguem extreure una marca de temps formatada per al temps transcorregut des que es va iniciar el cronòmetre. D'això en diem "obtenir el temps de split".

El mètode `split` no ha d'esperar arguments, i retorna una cadena on el temps transcorregut des de l'inici està formatat com a\_"mm:ss\_". Internament, el mètode `split` pot fer ús de mètodes prèviament declarats com `getMinutes` , `getSeconds` i `computeTwoDigitNumber` .

### Iteració 2: Manipulació del DOM

La teva classe Cronòmetre és ara completa. Això vol dir que podem seguir endavant i crear una interfície visual que ens permeti utilitzar tota la lògica que acabem de codificar.

En aquest punt, hauríeu de començar a treballar al fitxer `javascript/index.js` . Tingues en compte que, per ara, no has de canviar res als fitxers HTML o CSS.

En aquesta iteració, el teu objectiu és crear un nou cronòmetre, i utilitzar els seus mètodes (que definim prèviament a `chronometer.js` ) mentre interactua amb el DOM. Exemple: en fer clic, el botó de `start` ha d'invocar el mètode de `start` del cronòmetre.

Com podeu veure, tenim dos botons diferents: `start` i `clear` . Aquests són els valors del botó quan el cronòmetre no funciona. Quan el cronòmetre està en marxa, el botó inicial canviarà el comportament per aturar el cronòmetre. Per contra, el botó de reinici canviarà per dividir.

Tots dos botons tindran un comportament diferent depenent de l'estat del cronòmetre. Aquests botons són `btnLeft` i `btnRight` al nostre HTML. Podem veure els diferents valors que tindran a la taula següent:

<br/>

| Estat del cronòmetre | ID del botó | Text  | Classe CSS  |
| -------------------- | ----------- | ----- | ----------- |
| Detingut             | `btnLeft`   | START | `btn start` |
| Detingut             | `btnRight`  | RESET | `btn reset` |
| En marxa             | `btnLeft`   | STOP  | `btn stop`  |
| En marxa             | `btnRight`  | SPLIT | `btn split` |

<br/>

Trobareu dos escoltadors d'esdeveniments de clic que ja estan vinculats amb els botons `btnLeft` i `btnRight` . Heu de crear el codi necessari per canviar l'estat dels botons.

:bulb : _Pista_: Per canviar l' _estat_ dels botons, hem d' _alternar_ les classes depenent de _si_ les classes inclouen 'start' o 'reset'.

Això vol dir que quan fem clic al `btnLeft` , si teniu la classe `start` , caldrà canviar els botons `btnLeft` i `btnRight` , configurant-los amb l'estat Running descrit a la taula anterior.

D'altra banda, si el `btnLeft` no té la classe `inicio` quan fem clic, haurem de canviar les propietats del `btnLeft` i del `btnRight` configurant-los amb l'estat Detingut descrit a la taula anterior.

Si el btnLeft té la classe `start` , haurem de trucar a l' `startmethod` de chronometer -Recorda els arguments! -.

#### Canviant els textos dels botons

Treballarem al fitxer `javascript/index.js` . Necessitem fer el següent (podríem fer servir .className i .innerHTML per fer-ho):

- Quan es prem el botó esquerre mentre el cronòmetre està aturat, necessitem:

  - Establir el botó `btnLeft` amb el text STOP, i la classe `btn stop`
  - Establir el botó `btnDerecho` amb el text SPLIT, i la classe `btn` split.

- Quan es pitja el botó esquerre mentre el cronòmetre està en marxa hem de:

  - Establir el botó `btnLeft` amb el text START i la classe `btn` start.
  - Establir el botó `btnRight` amb el text RESET, i la classe `btn` reset.

- Al fitxer `index.js` , crear una nova instància de l'objecte `Chronometer` (que ja tenim a la part superior del fitxer).

- Crea el codi necessari a l' `index.js` per trucar al mètode `start` del cronòmetre si el botó té la classe `start` , o al mètode `stop` si el botó té aplicada la classe `stop` .

#### Imprimir el nostre cronòmetre

Cada segon necessitem actualitzar la nostra pantalla. Així que segueix endavant i crea una funció que rebi el valor dels minuts i els segons, i imprimeix això al nostre HTML - recorda reutilitzar el nostre mètode twodigits que tenim al cronòmetre!

Podeu invocar el mètode start del vostre cronòmetre.

Suggeriment: si ho recordes, el mètode start espera un callback com a argument i executarà aquest callback cada segon (pots passar com a argument una funció per actualitzar la interfície d'usuari)

<details><summary> Feu clic aquí per veure la imatge </summary>

<br/>

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_1a87e0edfb6efea2ae0c77c490e8563b.png)

</details>

### Iteració 3: Temps de divisió

La funció següent que implementarem és el botó de divisió. Recordeu que el botó de divisió es troba al botó `btnRight` quan el cronòmetre està en marxa. En aquesta iteració, haurem de crear dues coses diferents: HTML\&CSS, i el JavaScript associat.

#### HTML & CSS

En primer lloc, hem de localitzar al nostre arxiu `index.html` una llista ordenada a la qual afegirem l'hora actual cada vegada que polsem el botó de divisió - té un id de `splits` , i el tenim apuntat al final del nostre arxiu index .js.

#### JavaScript

Un cop hem localitzat la llista ordenada al nostre HTML, hem de crear la funcionalitat del botó. Cada cop que fem clic al botó de divisió, haurem de crear un nou element `li` i afegir-lo a la llista ordenada. El text d'aquest element serà l'hora actual del cronòmetre (tenim un mètode al nostre cronòmetre que torna aquest :wink :).

Per tant, primer hem de crear un element de la llista cada vegada que fem clic al botó. Després, hem d'afegir un nom de classe a aquest element de la llista ('list-item'). Després hem d'actualitzar l'innerHTML amb el resultat del nostre mètode `splitin` chronometer. I finalment l'hem d'annexar a l'element pare a l'arxiu html (la llista ordenada amb `id` s de `splits` )

<details><summary> Feu clic aquí per veure la imatge </summary>

<br/>

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_a5c9687f25bd710b2e7658ee6d997174.png)

</details>

### Iteració 4: Reset

Per acabar amb aquesta lliçó, crearem la funció _clear_ . Recordeu que l'executarem quan el cronòmetre s'aturi, i l'usuari faci clic al botó dret - comprova l'oient d'esdeveniments a la part inferior del fitxer. El comportament aquí és senzill: hem d'esborrar totes les dades del rellotge.

Per això, haurem de posar a zero els minuts i els segons al nostre rellotge i eliminar tots els elements que `li` tenir a la llista que vam crear a la iteració anterior.

### BONUS Iteració 5: Mil·lisegons

Ara podem utilitzar el nostre cronòmetre per calcular el temps que fem servir en cada exercici d'Ironhack. Què passa si volem calcular el nostre temps en una cursa? Hem de ser més precisos amb el nostre cronòmetre. Com podem ser més precisos? Afegint mil·lisegons.

Si volem afegir mil·lisegons al cronòmetre, haurem de manipular l'HTML, el CSS i el JavaScript. A l'HTML, tindrem que un contenidor per mostrar els mil·lisegons, canviant l'estil d'aquest contenidor - de manera que hem de modificar el DOM amb el valor decimal i d'unitat dels mil·lisegons (estan dirigits al principi de l'arxiu index.js).Finalment , en JavaScript, haurem d'afegir tota la lògica per mostrar els mil·lisegons al rellotge. També hauràs d'afegir aquests mil·lisegons al comptador de la divisió - hem de trucar al mètode twoDigits també aquí.

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_82e9d1fd5976a3f98bb1382f2385f6a1.png)

El teu objectiu és crear la lògica de JavaScript per a:

- Ser capaç de comptar els mil·lisegons.
- Mostrar els mil·lisegons en avançar.
- Mostrar els mil·lisegons quan es captura un temps dividit.
- Esborrar els mil·lisegons quan es faci clic al botó de reinici.

Aquest laboratori és una mica complex, però us guiarà en el procés lògic de resolució del problema i, alhora, seguint les pautes, aprendràs a separar les preocupacions entre la lògica i la manipulació del DOM (que són les visuals).

## Proveu el vostre codi

## Proves, proves, proves!

Aquest LAB està equipat amb proves unitàries per proporcionar informació automatitzada sobre el progrés del laboratori. Començaràs treballant amb les proves i fent-les servir juntament amb les instruccions d'iteració.

Si us plau, obre el teu terminal, canvia els directoris a l'arrel del laboratori, i executa npm `install` per instal·lar l'executor de proves. A continuació, executeu l'ordre `npm run test:watch` per executar les proves automatitzades.

```shell
$ cd lab-javascript-chronometer
$ npm install
$ npm run test:watch
```

<br/>

Obriu el fitxer `lab-solution.html` resultant amb l'extensió [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) VSCode per veure els resultats de les proves.

**Nota:** L'entorn de proves i la pàgina `lab-solution` .html no permeten imprimir els _registres de la_ consola al navegador.

Per veure les sortides de console.log que s'escriuen a qualsevol dels fitxers JavaScript, obriu el fitxer `index.html` amb l'extensió VSCode del [Live](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) Server.

<br/>

**Feliç codificació!** :heart: