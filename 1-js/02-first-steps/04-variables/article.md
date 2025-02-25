# Փոփոխականներ

JavaScript ծրագրերը հաճախ աշխատում են տեղեկատվության հետ: Ահա երկու օրինակ.
1. Առցանց խանութ -- տեղեկատվության մաս են կազմում վաճառվող ապրանքները, զամբյուղը (shopping cart):
2. A chat application -- տեղեկատվությունթան մաս են կազմում օգտատերերը, հաղորդագրությունները և այլն:

Փոփոխականներն օգտագործվում են այս ամենը պահելու համար:

## Փոփոխական

[Փոփոխականը](https://en.wikipedia.org/wiki/Variable_(computer_science)) տվյալների «պահեստարան» է: Փոփոխականները կարելի է օգտագործել տարբեր տիպի տվյալներ պահելու համար:

JavaScript-ում փոփոխական ստեղծելու համար կիրառվում է `let` հիմնաբառը:

Ստորև բերված հայտարատությունը ստեղծում է (այլ կերպ ասած *հայտարարում է*) «message» անունով փոփոխական.

```js
let message;
```

Այդ փոփոխականի մեջ որևէ տվյալ տեղադրելու համար կիրառենք վերագրման օպերատորը՝ `=`.

```js
let message;

*!*
message = 'Hello'; // տեղադրել տողը «message» անունով փոփոխականի մեջ
*/!*
```

Այդ տողն այժմ պահվում է հիշողության այն հատվածում, որը վերագրված է մեր փոփոխականին: Փոփոխականի պարունակությունը կարելի է կարդալ փոփոխականի անվան միջոցով.

```js run
let message;
message = 'Hello!';

*!*
alert(message); // ցույց է տալիս փոփոխականի պարունակությունը
*/!*
```

Հակիրճ լինելու համար կհամատեղենք փոփոխականների հայտարարումն ու արժեքի վերագրումը մեկ տողում.

```js run
let message = 'Hello!'; // հայտատարում և արժեք ենք վերագրում փոփոխականին

alert(message); // Hello!
```

Նույն տողում կարելի է հայտարարել մի քանի փոփոխականներ.

```js no-beautify
let user = 'John', age = 25, message = 'Hello';
```

Չնայած սա կարող է հարմար թվալ, այնուամենայնիվ մենք խորհուրդ ենք տալիս յուրաքանչյուր փոփոխական հայտարարել նոր տողում:

Մի քանի տողում փոփոխականներ հայտարարելը ավելի երկար է, բայց և ավելի հեշտ է դրանք ընթեռնելը.

```js
let user = 'John';
let age = 25;
let message = 'Hello';
```

Երբեմն փոփոխականները հայտարարվում են հետևյալ կերպ.
```js no-beautify
let user = 'John',
  age = 25,
  message = 'Hello';
```

...կամ էլ այսպես:

```js no-beautify
let user = 'John'
  , age = 25
  , message = 'Hello';
```

Սրանք բոլորն էլ նույն արդյունքն ունեն և յուրաքանչյուր ոք ընտրում է ըստ ճաշակի և նախընտրության:

````smart header="`var`՝ `let`-ի փոխարեն"
`let`-ի փոխարեն կարող է հանդիպել նաև `var` հիմնաբառը.

```js
*!*var*/!* message = 'Hello';
```

`var` հիմնաբառը *գրեթե* նույն `let`-ն է: Այն նույնպես հայտարարում է փոփոխական, բայց կան որոշ տարբերություններ:

`let`-ի և `var`-ի մեջ կան փոքր տարբերություններ, որոնք մենք դեռ չեն դիտարկի: Այդ տարբերությունների մասին մանրամասն կխոսենք <info:var> բաժնում:
````

## A real-life analogy

Հեշտությամբ կարելի է հասկանալ «փոփոխական»-ի գաղափարը, եթե պատկերացնենք այն որպես տվյալների «արկղ», որն իրեն առանձնահատուկ պիտակ ունի:

Օրինակ՝ `message` փոփոխականը կարելի է պատկերացնել որպես `"message"` պիտակով արկղ, որի մեջ պահվում է `"Hello!"` արժեքը:

![](variable.svg)

Արկղի մեջ կարող ենք դնել ցանկացած արժեք:

Այդ արժեքները կարող ենք փոխել ըստ անհրաժեշտության.
```js run
let message;

message = 'Hello!';

message = 'World!'; // արժեքը փոխված է

alert(message);
```

Երբ արժեքը փոխվում է, հին տվյալները դառնում են անհասանելի.

![](variable-change.svg)

Մենք նաև կարող ենք սահմանել երկու փոփոխական և պատճենել դրանցից մեկի տվյալները մյուսի մեջ:

```js run
let hello = 'Hello world!';

let message;

*!*
// պատճենել 'Hello world'-ը hello-ից message
message = hello;
*/!*

// այժմ երկու փոփոխականները պարունակում են նույն տվյալները
alert(hello); // Hello world!
alert(message); // Hello world!
```

````warn header="Նույն փոփոխականը մեկից ավել անգամ սահմանելը սխալ է առաջացնում"
Փոփոխականը կարող է սահմանվել միայն մեկ անգամ:

Փոփոխականի հայտարարման կրկնությունը սխալ է.

```js run
let message = "This";

// կրկնվող 'let' հայտարարությունները առաջացնում են սխալներ
let message = "That"; // SyntaxError: 'message' has already been declared
```
Այսպիսով՝ մենք պետք է փոփոխականները հայտարարենք մեկ անգամ, այնուհետև օգտագործենք առանց `let` հիմնաբառի:
````

```smart header="Ֆունկցիոնալ լեզուներ"
Հետաքրքիր է իմանալ, որ գոյություն ունեն [ֆունկցիոնալ](https://hy.wikipedia.org/wiki/%D5%96%D5%B8%D6%82%D5%B6%D5%AF%D6%81%D5%AB%D5%B8%D5%B6%D5%A1%D5%AC_%D5%AE%D6%80%D5%A1%D5%A3%D6%80%D5%A1%D5%BE%D5%B8%D6%80%D5%B8%D6%82%D5%B4) ծրագրավորման լեզուներ, օրինակ՝ [Scala](http://www.scala-lang.org/), [Erlang](http://www.erlang.org/), որոնք արգելում են փոփոխականների արժեքների փոփոխությունները:

Նման լեզուներում, երբ արժեքները տեղադրվում են «արկղների» դրանք այլևս փոխել հնարավոր չէ: Եթե անհրաժեշտ է պահել այլ արժեք, լեզուն ստիպում է ստեղծել նոր արկղ (հայտարարել նոր փոփոխական):

Չնայած առաջին հայացքից այդ պահվածքը կարող է տարօրինակ թվալ, բայց այդ լեզուները իրոք օգտագործվում են լուրջ նախագծեր մշակելու համար: Ավելին, կան ոլորտներ (օր.՝ զուգահեռ հաշվարկներ), որտեղ այս սահմանափակումները շահավետ են: Խորհուրդ է տրվում ուսումնասիրել նման ծրագրավորման լեզու (նույնիսկ եթե չեք պատրաստվում օգտագործել) «աշխարհայացք» զարգացնելու համար:
```

## Փոփոխականների անվանումը [#variable-naming]

JavaScript-ում փոփոխականների անունների հետ կապված կա երկու սահմանափակում.

1. Անունը պետք է պարունակի միայն տառեր, թվեր, կամ `$` և `_` նշանները:
2. Փոփոխականի անվան առաջին նշանը չպետք է լինի թիվ:

Ստորև բերված են վավեր անունների օրինակներ.

```js
let userName;
let test123;
```

Երբ փոփոխականի անունը պարունակում է մի քանի բառեր, սովորաբար օգտագործվում է [camelCase](https://en.wikipedia.org/wiki/CamelCase): camelCase-ի օգինակ է հետևյալ անունը՝ `myVeryLongName`:

Ինչպես նշեցինք `'$'` և `'_'` նշանները ևս կարող են օգտագործվել փոփոխականների անուններում, սակայն դրանք որոշակի հատուկ իմաստ չունեն:

Հետևյալ անունները վավեր են.

```js run untrusted
let $ = 1; // հայտարարել փոփոխական "$" անունով
let _ = 2; // և "_" անունով

alert($ + _); // 3
```

Սրանք անվավեր անունների օրինակներ են.

```js no-beautify
let 1a; // փոփոխականի անունը չի կարող սկսել թվով

let my-name; // գծիկները թույլատրված չեն անուններում
```

```smart header="Case-ը կարևոր է"
`apple` և `AppLE` անուններով փոփոխականները երկու տարբեր փոփոխականներ են:
```

````smart header="Ոչ լատիներեն տառերը թույլատրված են, բայց խորհուրդ չի տրվում դրանք օգտագործել"
Հնարավոր է օգտագործել ցանկացած լեզվի տառեր, օրինակ՝

```js
let имя = '...';
let 我 = '...';
```

Նման անունները վավեր են, սակայն ամենուր ընդունված է օգտագործել անգլերենը: Նույնիսկ եթե փոքր սկրիպտ եք գրում, այն հնարավոր է կարդան նաև այլազգի ծրագրավորողները:
````

````warn header="Վերապահված անուններ (reserved names)"
Գոյություն ունի [վերապահված բառերի ցանկ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords), որոնք չի կարելի օգտագործել որպես փոփոխականի անուն, քանի որ դրանք օգտագործվում են հենց լեզվի կողմից:

Օրինակ՝ `let`, `class`, `return`, և `function` հիմնաբառերը վերապահված են:

Ստորև բերված կոդը սկալ է պարունակում.

```js run no-beautify
let let = 5; // փոփոխականի անունը չի կարող լինել "let"
let return = 5; // ինչպես նաև "return"
```
````

````warn header="Փոփոխականի վերագրումն առանց `use strict`-ի"

Սովորաբար փոփոխականներն օգտագործելուց առաջ պետք է դրանք հայտարարել: Նախկինում հնարավոր էր ստեղծել փոփոխական միայն վերագրման միջոցով՝ առանց օգտագործելու `let`-ը: Դա այժմ ևս հնարավոր է, եթե սկրիպտում չլինի `use strict`-ը:

```js run no-strict
// ուշադրություն դրաձրեք, որ այս սկրիպտում "use strict" չկա

num = 5; // "num" փոփոխականը ստեղծվում է, եթե այն գոյություն չուներ

alert(num); // 5
```

Սա սխալ է առաջացնում «strict mode»-ում.

```js
"use strict";

*!*
num = 5; // error: num is not defined
*/!*
```
````

## Հաստատուններ

Հաստատուն սահմանելու համար `let`-ի փոխարեն օգտագործեք `const`.

```js
const myBirthday = '18.04.1982';
```

`const`-ով սահմանված փոփոխականները կոչվում են «հաստատուններ». դրանց չի կարելի նոր արժեք վերագրել: Նման փորձը սխալի կհանգեցնի.

```js run
const myBirthday = '18.04.1982';

myBirthday = '01.01.2001'; // error, can't reassign the constant!
```

Եթե վստահ եք, որ փոփոխականի արժեքը երբեր չի փոխվի, սահմանեք այն որպես `const` դա ապահովելու և բոլորին այդ փաստն ակնհայտ դարձնելու համար:


### Մեծատառ հաստատուններ

Ընդունված է օգտագործել հաստատունները դժվար հիշվող արժեքների համար, որոնք հայտնի են նախօրոք:

Նման հաստատունները գրվում են մեծատառ և ընդգծիկներով (underscores):

Օրինակ՝ սահմանենք հաստատուններ որոշ գույների համար.

```js run
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
const COLOR_ORANGE = "#FF7F00";

// ...գույնն օգտագործելու համար
let color = COLOR_ORANGE;
alert(color); // #FF7F00
```

Առավելությունները.

- `COLOR_ORANGE`-ը շատ ավելի հեշտ է մտապահել, քան `"#FF7F00"`-ը,
- ավելի հեշտ է սխալ գրել `"#FF7F00"`-ը, քան `COLOR_ORANGE`-ը,
- կոդը կարդալիս `COLOR_ORANGE`-ն ավելի շատ իմաստ է արտահայտում, քան `#FF7F00`-ը:

Եկեք հասկանանք երբ է պետք հաստատունները գրել մեծատառերով, իսկ երբ ոչ:

«Հաստատունն» ուղղակի փոփոխական է, որի արժեքը չի փոխվում: Այունամենայնիվ կան հաստատուններ, որոնց արժեքը հայտնի է մինչև սկրիպը գործարկելը (ինչպես, օրինակ, վերոնշյալ գույները) և կան հաստատուններ, որոնց արժեքը հայտնի է դառնում միայն սկրիպտի աշխատանքի ընթացքում և այն չի փոխվում սկզբնական վերագրումից հետո:

Օրինակ՝
```js
const pageLoadTime = /* time taken by a webpage to load */;
```

`pageLoadTime`-ի արժեքը հայտնի չէ, քանի դեռ էջն ամբողջությամբ չի բեռնվել, սակայն այն հաստատուն է, քանի որ արժեքը չի փոխվում:

Այլ կերպ ասած՝ մեծատառերով գրված հաստատունները օգտագործվում են միայն որպես «կոշտ կոդավորված» արժեքների փոխանուններ:

## Ճիշտ անունների ընտրությունը կարևոր է

Երբ խոսում ենք փոփոխականների մասին մի կարևոր բան կա, որը պետք է միշտ հիշել:

Փոփոխականը պետք է ունենա պարզ, միարժեք հասկանալի, և պարունակող արժեքը բնութագրող անուն:

Փոփոխականները անվանելը կարևորագույն և դժվար ունակություններից է ծրագրավորման մեջ: Արագ նայելով փոփոխականնրի անուններին կարելի է կարծիք կազվել կոդը գրած ծրագրավորողի ունակությունների մասին:

Իրական նախագծերում հիմնական ժամանակը ծախսվում է գոյություն ունեցող կոդը փոփոխելու և զարգացնելու վրա: Երբ վերադառնում ենք մեր գրած կոդին որոշ ժամանակ հետո շատ ավելի հեշտ է վերհիշել այն, երբ փոփոխականներն իմաստավոր անուններ ունեն:

Հորդորում ենք ծախսել որոշ ժամանակ մտածելու ճիշտ անուններ փոփոխականների համար մինչև դրանք հայտարարելը: Հավատացեք, դա Ձեզ հետագայում միայն կօգնի:

Ահա որոշ կանոններ, որոնց արժի հետևել.

- օգտագործեք հեշտ ընթեռնելի անուններ, օրինակ՝ `userName`, կամ `shoppingCart`,
- մի օգտագործեք հապավումներ կամ նմանատիպ կարճ անուններ՝ `a`, `b`, `c`, եթե, իհարկե, գիտեք ինչ եք ունում,
- անուններն ընտրեք հնարավորինս նկարագրող և հակիրճ: `data`-ն և `value`-ն վատ անունների օրինակներ են, քանի որ նման անունները ոչինչ չեն ասում: Նման անունները կարելի է միայն օգտագործել այն դեպքերում, երբ կոդի համատեքստը (context) միանշանակ հասկանալի է դարձնում, թե ինչ արժեքներ են այդ փոփոխականները պարունակում,
- եկեք համաձայնության Ձեր թիմով, կամ ինքներդ Ձեզ հետ: Եթե Ձեր կայքի հաճախորդին անվանում եք «user», ապա նրա հետ կապված փոփոխականները պետք է ունենան նմանատիպ անուններ՝ `currentUser`, կամ `newUser` և ոչ թե `currentVisitor`, կամ `newManInTown`:

Այս ամենը պարզ է հնչում, սակայն գործնականում հասկանալի և հակիրճ փոփոխականների անուններ մտածելն այնքան էլ հեշտ չէ: Ինքներդ փորձեք և կհամոզվեք:

```smart header="Օգտագործե՞լ եղած փոփոխականը, թե՞ նորը մտածել"
Որոշ ծույլ ծրագրավորողներ փորձում են կրկին օգտագործել հայտարարած փոփոխականները՝ նորերը սահմանելու փոխարեն:

Արդյունքում նրանց փոփոխականները նմանվում են արկղերի, որոնց մեջ տարբեր տիպի իրեր են նետվում, առանց արկի պիտակը փոխելու: Այպես դժվարանում է հասկանալ, թե ինչ է պահվում այդ արկղում:

Նման դեպքերում ծրագրավորողները մի փոքր խնայում են փոփոխականները հայտարարելիս, սակայն ավելի շատ ժամանակ են ծախսում, երբ խնդիրներ են առաջանում:

Այսպիսով՝ ավել փոփոխականը վատ բան չէ:

Ժամանակակից JavaScript-ում կան գործիքներ, որոնք փոքրացնում և լավարկում (optimize) են կոդն այն աստիճան, որ ավել փոփոխականներ ունենալը խնդիրներ չի առաջացնի: Նույնիսկ հակառակը. տարբեր արժեքները տարբեր փոփոխականներում պահելը կարող է օգնել շարժիչին այս գործը կատարելիս:
```

## Ամփոփում

Տվյալները պահելու համար կարող ենք հայտարարել փոփոխականներ, օգտագործելով `var`, `let`, կամ `const` հիմնաբառերը:

- `let` -- ներկայումս փոփոխականները հայտարարվում են այս հիմնաբառով:
- `var` -- նախկինում օգտագործվում էր փոփոխականներ հայտարարելու համար: Այժմ այն չենք օգտագործում, իսկ սրա և `let`-ի տարբերությունները կքննարկենք <info:var> բաժնում:
- `const` -- նման է `let`-ին, սակայն այս հիմնաբառով հայտարարված փոփոխականի արժեքը չի փոխվում:

Փոփոխականների անունները պետք է պարզ հուշեն, թե ինչ արժեք է պարունակում տվյալ փոփոխականը։
