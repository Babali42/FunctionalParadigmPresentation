---
transition: fade-out
hideInToc: true
layout: image-right
image: /images/Colibri-white.svg
backgroundSize: 20em 20%
---

# Le paradigme fonctionnel
## On a des langages multi-paradigmes qui font du fonctionnel. En quoi ces langages font du fonctionnel ?

Gwennan et Baptiste 20/03/2025

---
hideInToc: true
---

# Qu'a-t-on hérité du fonctionnel dans nos langages plutôt orientés impératif ?

<v-click>

* Fonctions sur les itérateurs/listes (`for_each`, `map`, `fold`, `reduce`, ...)

* Immutabilité

* Pattern matching

* Optional

* ...

</v-click>

---
hideInToc: true
---

# Exemple de `map`

<div v-click="2">
Version impérative:
</div>

<div v-click="1">

```javascript
function map(list, mapper):
    var result = new List();
    for x in list:
        result.push(mapper(x));
    return result;
```

</div>

<div v-click="3">

Et si c'était fonctionnel ?

```javascript
function map(list, mapper):
    ...
    map(/*TODO METTRE QQCHOSE ICI*/);
    ...
    return qqchose;
```

</div>

---
hideInToc: true
---

# Plan

<Toc />

---

# Qu'est ce qu'un type dans le monde du fonctionnel ?

<div>

C'est un ensemble de valeurs.

</div>

<v-click>
Exemple:

```ocaml
type int8 := {-128, ... , 127}
```
</v-click>

<v-click>

```ocaml
val departement: int8 = 38
```

```ocaml
val ville: string = "Grenoble"
```

```ocaml
val temp: float = 22.8
```

</v-click>

<v-click>
En fait le fonctionnel part d'une approche inspirée des mathématiques, on va avoir des éléments qui appartiennent à des ensembles.

💡 cf _Théorie des types_
</v-click>

<v-click>

unit: une seule valeur possible `()`

never: aucune valeur possible (ensemble vide)

</v-click>

---
hideInToc: true
---

## Types somme

Union entre deux ensembles

```ocaml
type unsigned_int2 := {0, 1, 2, 3}
type first_chars := {'a', 'b', 'c'}

unsigned_int2 + first_chars -> {0, 1, 2, 3, 'a', 'b', 'c'}
```

## Types produit

Une combinaison entre deux ensembles

```ocaml
type unsigned_int2 := {0, 1, 2, 3}
type first_chars := {'a', 'b', 'c'}

unsigned_int2 * first_chars -> {
  (0, 'a'), (1, 'a'), (2, 'a'), (3, 'a'),
  (0, 'b'), (1, 'b'), (2, 'b'), (3, 'b'),
  (0, 'c'), (1, 'c'), (2, 'c'), (3, 'c')
}
```

---

## Utiliser un type somme

<div v-click>

```ocaml
val x: int + char;
match x with
    | {0} -> ...
    | {1, 2} -> ...
    | {'a'} -> ...
    | {'b', ..., 'f'} -> ...
    | _ -> ...
```

</div>

<v-click>

***C'est de là que vient l'idée du matching pattern !***

</v-click>

---

# Les fonctions

<div>

Type d'une fonction: `type_départ -> type_arrivée`

</div>

```ocaml
val char_to_int: char -> int
```

<v-click>

```ocaml
val mult2: int -> int = x -> x * 2
val mult2 = (x: int) -> x * 2

mult2(4)
(* res = 8 *)
```
</v-click>

---

⚠️ Pas de notion de temps / déroulement

=> la mutabilité n'a pas de sens ici d'un point de vue théorique

<v-click>

Imaginons qu'on crée un langage fonctionnel qui permet de redéfinir des variables. <br/>
Alors il devrait fonctionner comme ça :

```ocaml
val x = mult2(char_to_int('4'))
(* x = 8 *)
val y = x + 1
(* y = x + 1 = 9 *)

val x = 4 (* ça n'est plus le même x, l'ancien existe toujours et n'a pas changé *)
(* x = 4 *)
y
(* y = x + 1 = 9, ce qui compte c'est l'ancien x *)
```
</v-click>

<div v-click>

💡 _NB: Scala utilise le mot clé val (valeur) et non pas var (variable)_
</div>

---

# Les fonctions à argument multiples

<v-click>

```ocaml
val mult: int -> int -> int
```
</v-click>
<v-click>

```ocaml
val mult2 = mult(2)
(* mult2: int -> int *)

mult2(4)
(* res = 8 *)

mult2(4) == mult(2)(4)
```
</v-click>

<v-click>
💡 _Currification_
</v-click>

---

# Faire des opérations sur les listes

* Définir les listes

* Opérations sur les listes

---
transition: none
hideInToc: true
---

## C'est quoi une liste ?

<div v-click>

```ocaml
type list<T> := {empty} + {(T, list<T>)};
```
</div>

<v-click>

```ocaml
type list<bool> := {empty, (true, empty), (false, empty), (true, (true, empty)), (true, (false, empty)), ...};
```
</v-click>


<div v-click>

Par exemple, la liste `[3, 5, 12]` devient:

```ocaml
val ma_liste = (3, (5, (12, empty)));
```
</div>


---
hideInToc: true
---


## C'est quoi une liste ?

```ocaml
type list<T> := [] | T::list<T>;
```

```ocaml
type list<bool> := {[], true::[], false::[], true::true::[], true::false::[], ...};
```

Par exemple, la liste `[3, 5, 12]` devient:

```ocaml
val ma_liste = 3::5::12::[];
```


---
hideInToc: true
---

## Et comment on l'utilise ?

```ocaml
type list<T> := [] | T::list<T>;
```

Exemple: fonction `first`

<div v-click>

```ocaml
val first = (l: list<T>) -> match l
                         | x::reste -> x
                         | [] -> unit
```
```ocaml
val first: list<T> -> T + unit
```

</div>

---
transition: fade
hideInToc: true
---


## Et donc, nos fameuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

<v-click>

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[];

map (ma_liste) (fun x -> x + 2);
(* res: 3::6::5::14::[] *)
```

</v-click>

<v-click>

```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with

...
```

</v-click>

---
transition: fade
hideInToc: true
---


## Et donc, nos fameuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[];

map (ma_liste) (fun x -> x + 2);
(* res: 3::6::5::14::[] *)
```

```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
...
```

---
transition: fade
hideInToc: true
---


## Et donc, nos fameuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[];

map (ma_liste) (fun x -> x + 2);
(* res: 3::6::5::14::[] *)
```

```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
          | v::reste -> ...
```

---
hideInToc: true
---


## Et donc, nos fameuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[];

map (ma_liste) (fun x -> x + 2);
(* res: 3::6::5::14::[] *)
```

```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
          | v::reste -> (f(v))::(map(reste)(f))
```

---
hideInToc: true
transition: fade
---

Fonctionnel:
```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
          | v::reste -> (f v)::(map(reste)(f))
```

<v-click>

Impératif:
```javascript
function map(l, mapper):
    var result = new List();
    for x in l:
        result.push(mapper(x));
    return result;
```

</v-click>

<div v-click>

Ce qu'on avait deviné:
```javascript
function map(l, mapper):
    ...
    map(wahla jsp quoi mettre ici);
    ...
    return qqchose;
```

</div>


---
hideInToc: true
---

Fonctionnel:
```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
          | v::reste -> (f v)::(map(reste)(f))
```

Impératif:
```javascript
function map(l, mapper):
    var result = new List();
    for x in l:
        result.push(mapper(x));
    return result;
```

<div v-click>

Functionnal-style en impératif:
```javascript
function map(l, mapper):
    if l.isEmpty():
      return [];
    else:
      return [mapper(l[0])] + map(l[1:], mapper);
```

</div>

---
hideInToc: true
---

# En quoi nos langages font du fonctionnel ?

- Pas du fonctionnel pur

- Pour réussir à créer des programmes, le fonctionnel a dû créer des outils

- Ces outils ont été repris par les langages impératifs

```javascript
function map(list, mapper):
    var result = new List();
    for x in list:
        result.push(mapper(x));
    return result;
```

```javascript
var x = /* some int*/
switch x:
    0 -> ...
    (> 1) && (< 10) -> ...
    _ -> ...
```

---
hideInToc: true
---

# Option / Optionnal

```ocaml
type option<T> = {None} + T
```

<v-click>

```ocaml
type nullable<T> = {null} + T
```
</v-click>


<v-click>
  
Sauf qu'en fonctionnel:

```ocaml
option<T> != T
val x: option<string>;
val length: string -> int
```
</v-click>
<v-click>

```ocaml
length(x) (* non!!!! On ne peut pas faire ça *)

match x
  | None -> 0
  | s: string -> length(s)
(* Oui, car tous les cas sont gérés :D *)
```
</v-click>

<v-click>

Ce que les langages OO recopient en utilisant `optionnal`,<br/>
**c'est la rigueur du système de typage fonctionnel**
</v-click>

---

# En résumé

* Les langages multi-paradigmes n'adhèrent pas au fonctionnel pur.  
Exemples: implémentation de map, les types nullables

* Il s'inspirent des fonctionnalités du fonctionnel et les adaptent.  
Exemples: optionnal, les opérations sur les listes/itérateurs, le pattern matching

**Il ne sont pas vraiment fonctionnels, mais ont appris du fonctionnel.**

---
layout: image-right

image: /Images/bob.jpg
backgroundSize: 20em 80%
---

# Des questions ?