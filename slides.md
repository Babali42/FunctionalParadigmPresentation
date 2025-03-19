---
transition: fade-out
hideInToc: true
---

# Paradigme fonctionnel
## On a des langages multi-paradigmes qui font du fonctionnel. En quoi ces langages font du fonctionnel ?

Gwennan et Baptiste 20/03/2025

---
hideInToc: true
---

# Plan

<Toc />


---

# Qu'est ce qu'un type ?

Lors de la déclaration d'un élément on lui attribut un type int, string, float.

```ocaml
val dep: int = 38
```

```ocaml
val ville: string = "Grenoble"
```

```ocaml
val temp: float = 22.8
```

En fait le fonctionnel part d'une approche mathématique, on va avoir des éléments qui ont des types.

💡 cf _Théorie des types_

---

# Qu'est-ce que la mutation ?

```javascript
var a = 5;
a = 10;
```

<div v-click>
En mathématiques la mutation n'a pas de sens.

Il en est de même en programmation fonctionnelle.

</div>

<div v-click>

💡 _Par exemple Scala utilise le mot clé val (valeur) et non pas var (variable)_
</div>

---

# Faire des opérations sur les listes

* Définir les listes

* Opérations sur les listes

---
transition: none
hideInToc: true
---


## Définissons et utilisons les listes

<div v-click>

```ocaml
type list<T> := Empty | Elem T list<T>;
```

</div>

<div v-click>

Par exemple, la liste `[3, 5, 12]` devient:

```ocaml
val ma_liste = Elem 3 (Elem 5 (Elem 12 empty))
```
</div>


---
hideInToc: true
---


## Définissons et utilisons les listes

```ocaml
type list<T> := [] | T::list<T>;
```

Par exemple, la liste `[3, 5, 12]` devient:

```ocaml
val ma_liste = 3::5::12::[]
```

<div v-click>

```ocaml
val l = fun l -> match l with
        | [] -> []
        | v::reste -> (v+1)::reste
```

</div>

---
transition: fade
hideInToc: true
---


## Et donc, nos fâmeuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

<v-click>

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[] in
  map ma_liste (fun x -> x + 2);;

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


## Et donc, nos fâmeuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[] in
  map ma_liste (fun x -> x + 2);;

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


## Et donc, nos fâmeuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[] in
  map ma_liste (fun x -> x + 2);;

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


## Et donc, nos fâmeuses opérations sur les listes ?

```ocaml
type list<T> := [] | T::list<T>
```

Exemple de `map`:

```ocaml
val ma_liste = 1::4::3::12::[] in
  map ma_liste (fun x -> x + 2);;

(* res: 3::6::5::14::[] *)

```

```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
          | v::reste -> (f v)::(map reste f)
```

---
hideInToc: true
---


## Et donc, nos fâmeuses opérations sur les listes ?

```ocaml
val map = fun (l: list<T>) -> (f: T -> U) -> match l with
          | [] -> []
          | v::reste -> (f v)::(map reste f)
```

<v-click>

```javascript
function map(list, mapper):
    var result = new List();
    for x in list:
        result.push(mapper(x));
    return result;
```

</v-click>

<div v-click="[+2, +3]">

```javascript
function map(list, mapper):
    ...
    map(wahla jsp quoi mettre ici);
    ...
    return qqchose;
```

</div>

<div v-click="+3">

```javascript
function map(list, mapper):
    if list.isEmpty():
      return [];
    else:
      return [mapper(list[0])] + map(list[1:], mapper);
```

</div>