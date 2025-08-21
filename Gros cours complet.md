Gros cours complet
Super 👌 tu vas avoir un **gros cours progressif (6h environ)**, en **Markdown**, bien structuré, clair pour un ado de 13–14 ans, et surtout **original & créatif**.
L’idée : on commence par **jouer avec le texte dans le terminal**, on apprend pas à pas les **bases**, et à chaque étape il y a un **mini-défi** ou une **mini-histoire**.
À la fin, on a **2 mini-projets** :

- 🎯 _Jeu de devinette_ (numérique, fun)
- 🎓 _Calcul de moyenne avec gestion d’erreurs_

---

````markdown
# 🐍 Cours d’introduction à Python (Terminal)

> Public : débutants (13–14 ans)  
> Durée : ~6 heures (2 sessions possibles)  
> Objectif : apprendre les bases de la programmation avec Python en s’amusant

---

# 0) Avant de commencer : Installer Python

- Télécharge Python depuis [python.org](https://www.python.org/downloads/)
- Vérifie dans le terminal :
  ```bash
  python3 --version   # mac/Linux
  py --version        # Windows
  ```
````

→ Doit afficher `Python 3.x.x`

Crée un dossier de travail, par exemple `python-cours/`, et ouvre-le dans ton éditeur ou terminal.

---

# 1) Premier programme — Hello World (20 min)

Créer `bonjour.py` :

```python
print("Bonjour !")
print("Bienvenue dans ton premier programme Python :)")
```

Exécuter :

```bash
python3 bonjour.py   # mac/Linux
py bonjour.py        # Windows
```

👉 **Défi 1 :** change le message pour qu’il dise “Salut \[ton prénom] !”

---

# 2) Variables et types de données (40 min)

Créer `variables.py` :

```python
prenom = "Alice"     # str (texte)
age = 14             # int (entier)
pi = 3.14            # float (nombre décimal)
adolescent = True    # bool (vrai/faux)

print("Prénom:", prenom)
print("Âge:", age)
print("Nombre pi:", pi)
print("Es-tu adolescent ?", adolescent)
```

👉 Types de base :

- `str` → texte
- `int` → entier
- `float` → nombre à virgule
- `bool` → vrai ou faux

👉 **Défi 2 :** invente une variable `animal`, `couleur`, `poids` et affiche une phrase du style :
`Mon chat est noir et pèse 3 kg.`

---

# 3) Les opérateurs (40 min)

Créer `operateurs.py` :

```python
a = 10
b = 3

print("a + b =", a + b)
print("a - b =", a - b)
print("a * b =", a * b)
print("a / b =", a / b)    # division flottante
print("a // b =", a // b)  # division entière
print("a % b =", a % b)    # reste de division
print("a ** b =", a ** b)  # puissance

print("Comparaisons :")
print("a == b ?", a == b)
print("a > b ?", a > b)
print("a != b ?", a != b)
```

👉 **Défi 3 :** calcule le périmètre d’un rectangle de largeur 7 et hauteur 5.

---

# 4) Interaction avec l’utilisateur (30 min)

Créer `input.py` :

```python
nom = input("Comment t'appelles-tu ? ")
print("Enchanté,", nom, "!")

age = int(input("Quel âge as-tu ? "))
print("Dans 5 ans tu auras", age + 5, "ans.")
```

👉 **Défi 4 :** demande la ville de l’utilisateur et affiche `Tu habites à [ville]`.

---

# 5) Conditions `if` (30 min)

Créer `conditions.py` :

```python
age = int(input("Quel âge as-tu ? "))

if age >= 18:
    print("Tu es majeur.")
elif age >= 13:
    print("Tu es adolescent.")
else:
    print("Tu es enfant.")
```

👉 **Défi 5 :** crée un petit “quizz” qui demande :
`Quelle est la capitale de la France ?`

- Si la réponse est “Paris” → Bravo !
- Sinon → Raté !

---

# 6) Boucles (50 min)

Créer `boucles.py` :

```python
# Compter de 1 à 5
for i in range(1, 6):
    print("Compteur:", i)

# Boucle while (répète jusqu'à une condition)
mot = ""
while mot != "python":
    mot = input("Tape 'python' pour continuer : ")
print("Bravo !")
```

👉 **Défi 6 :**

- Affiche les nombres pairs de 0 à 20.
- Affiche la table de multiplication choisie par l’utilisateur.

---

# 7) Mini-projet 1 : Jeu de devinette 🎯 (40 min)

Créer `devinette.py` :

```python
import random

secret = random.randint(1, 20)
essais = 0
trouve = False

print("J'ai choisi un nombre entre 1 et 20...")

while not trouve:
    essai = int(input("Devine : "))
    essais += 1

    if essai < secret:
        print("Trop petit.")
    elif essai > secret:
        print("Trop grand.")
    else:
        print("Bravo ! Tu as trouvé en", essais, "essais.")
        trouve = True
```

👉 **Défi 7 :** limite à 5 essais maximum. Si perdu, affiche “Dommage ! Le nombre était X.”

---

# 8) Les listes (40 min)

Créer `listes.py` :

```python
fruits = ["pomme", "banane", "cerise"]

print(fruits[0])      # premier élément
fruits.append("orange")
print(fruits)
print("Il y a", len(fruits), "fruits")

for f in fruits:
    print("J'aime les", f)
```

👉 **Défi 8 :** demande 3 prénoms à l’utilisateur, mets-les dans une liste et affiche-les un par un.

---

# 9) Fonctions (40 min)

Créer `fonctions.py` :

```python
def dire_bonjour(nom):
    print("Salut", nom)

dire_bonjour("Alice")
dire_bonjour("Bob")

def carre(x):
    return x * x

print("Carré de 7 =", carre(7))
```

👉 **Défi 9 :** écris une fonction `table(n)` qui affiche la table de multiplication de `n`.

---

# 10) Chaînes de caractères (30 min)

Créer `chaines.py` :

```python
texte = "Python est génial"

print(texte.upper())     # majuscules
print(texte.lower())     # minuscules
print(texte.replace("génial", "cool"))
print("Longueur :", len(texte))

mot = "ha"
print(mot * 3)  # hahaha
```

👉 **Défi 10 :** demande le prénom et affiche :

- en majuscules
- en minuscules
- le nombre de lettres

---

# 11) Gestion d’erreurs (30 min)

Créer `erreurs.py` :

```python
try:
    age = int(input("Ton âge ? "))
    print("Dans 10 ans tu auras", age + 10)
except ValueError:
    print("Erreur : tu dois taper un nombre !")
```

👉 **Défi 11 :** écris un programme qui continue à demander un nombre tant que l’utilisateur ne tape pas un entier valide.

---

# 12) Mini-projet 2 : Moyenne de notes 🎓 (50 min)

Créer `moyenne.py` :

```python
def moyenne(notes):
    return sum(notes) / len(notes)

notes = []
for i in range(3):
    while True:
        try:
            n = float(input(f"Note {i+1} : "))
            notes.append(n)
            break
        except ValueError:
            print("Entre un nombre valide.")

print("Tes notes :", notes)
print("Moyenne :", moyenne(notes))
```

👉 **Défi 12 :** ajoute un message si la moyenne est ≥ 10 → “Bravo, tu as la moyenne !”

---

# ✅ Ce que tu sais maintenant

- Afficher (`print`) et demander (`input`).
- Variables et types (`int`, `float`, `str`, `bool`).
- Opérateurs (+ - \* / // % \*\*).
- Conditions `if/elif/else`.
- Boucles `for` et `while`.
- Listes (`append`, `len`, boucle).
- Fonctions (`def`, `return`).
- Chaînes (`upper`, `lower`, `replace`, `len`).
- Gestion d’erreurs (`try/except`).

Tu es officiellement un **jeune programmeur Python** 🥳

---

```

---

👉 Ce gros cours couvre **tout l’essentiel** pour être autonome en Python débutant, avec une progression claire, des mini-défis ludiques, et 2 mini-projets.

Veux-tu que je t’en fasse aussi une **version “fiche ultra-condensée” (1 page)** avec juste la syntaxe et exemples clés, comme aide-mémoire rapide pour l’enfant ?
```
