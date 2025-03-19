---
transition: fade-out
---

# Paradigme fonctionnel
## On a des langages multi-paradigmes qui font du fonctionnel. En quoi ces langages font du fonctionnel ?

Gwennan et Baptiste 20/03/2025

---

# Plan

- Type
  - Mutation
- Fonction
- Récursif
- Types somme et type produit
- Listes
- Pattern matching
- Réponse à la problématique


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
transition: none
---

# Et comment on définit des listes ?

<div v-click>
```ocaml
type List<T> := Empty | Elem T List<T>;
```
</div>

<div v-click>

Par exemple, la liste `[3, 5, 12]` devient:

```ocaml
Elem 3 (Elem 5 (Elem 12 empty));
```
</div>

---


# Et comment on définit des listes ?

```ocaml
type List<T> := [] | T::List<T>;
```

Par exemple, la liste `[3, 5, 12]` devient:

```ocaml
3::5::12::[];
```
