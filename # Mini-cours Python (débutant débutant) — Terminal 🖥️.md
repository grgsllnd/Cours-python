Parfait 👍 revenons **au tout tout début**, sans fenêtre graphique, juste **texte dans le terminal**.
C’est souvent la meilleure façon de commencer pour un·e ado qui découvre, car tout est simple et visible tout de suite.

---

# Mini-cours Python (débutant débutant) — Terminal 🖥️

## 1) Vérifier que Python est installé

Dans le **terminal** (mac/Linux) ou **PowerShell** (Windows) :

```bash
python3 --version   # mac / Linux
py --version        # Windows
```

Si ça affiche quelque chose comme `Python 3.12.3`, c’est bon ✅.

---

## 2) Premier programme

Crée un fichier `bonjour.py` avec ce contenu :

```python
print("Bonjour !")
print("Bienvenue dans ton premier programme Python :)")
```

Lance-le :

```bash
python3 bonjour.py   # mac/Linux
py bonjour.py        # Windows
```

---

## 3) Variables et calculs

Crée `calculs.py` :

```python
a = 5
b = 3
print("a =", a)
print("b =", b)
print("a + b =", a + b)
print("a * b =", a * b)
print("a puissance b =", a ** b)
```

---

## 4) Interaction avec l’utilisateur

Crée `salut.py` :

```python
nom = input("Comment t'appelles-tu ? ")
print("Enchanté,", nom, "!")
```

---

## 5) Conditions (`if`)

Crée `age.py` :

```python
age = int(input("Quel âge as-tu ? "))

if age >= 18:
    print("Tu es majeur.")
else:
    print("Tu es mineur.")
```

---

## 6) Boucles (`for` et `while`)

Crée `boucles.py` :

```python
# Compter de 1 à 5
for i in range(1, 6):
    print("Compteur:", i)

# Répéter jusqu'à trouver la bonne réponse
mot = ""
while mot != "python":
    mot = input("Tape 'python' pour continuer : ")
print("Bravo !")
```

---

## 7) Mini-projet : jeu de devinette 🎯

Crée `devinette.py` :

```python
import random

nombre_secret = random.randint(1, 20)
trouve = False

print("J'ai choisi un nombre entre 1 et 20...")

while not trouve:
    essai = int(input("Devine : "))
    if essai < nombre_secret:
        print("Trop petit.")
    elif essai > nombre_secret:
        print("Trop grand.")
    else:
        print("Bravo, tu as trouvé !")
        trouve = True
```

---

## 8) Défis simples (à proposer)

- Modifier le jeu de devinette pour limiter à **5 essais**.
- Afficher le **score** (1 point si trouvé, 0 sinon).
- Faire un petit “quiz” avec 3 questions et un compteur de bonnes réponses.
- Faire répéter un message 10 fois (ex: `"Python c'est cool !"`).
- Faire afficher la **table de multiplication** choisie par l’utilisateur.

Exemple table de multiplication :

```python
n = int(input("Choisis un nombre : "))
for i in range(1, 11):
    print(n, "x", i, "=", n * i)
```

---

💡 Ici, chaque programme tient en **10 lignes max**, donc facile à taper et à comprendre.
On apprend :

- `print` (affichage),
- `input` (interaction),
- variables, opérations,
- conditions (`if`),
- boucles (`for`, `while`),
- et un mini-projet amusant.

---

👉 Veux-tu que je t’organise ça en **séance de 4h progressive** (avec pauses, petits défis, mini-projet final) au format `.md`, pour que tu aies un vrai plan de cours prêt à dérouler ?
