# Cours d’introduction au développement (Python) — 4h

> Public cible : 13–14 ans • Objectif : **écrire ses premiers programmes** (texte + dessins), comprendre les bases, savoir **installer Python** et **travailler proprement avec un environnement virtuel**.

---

## 🗺️ Plan horaire (indicatif)

- **00:00–00:20** — Installation & vérifications (Python + éditeur)
- **00:20–01:10** — Bases : `print`, variables, `input`, conditions, boucles
- **01:10–01:30** — Mini-projet 1 : Jeu de devinettes (texte)
- **Pause 10 min**
- **01:40–02:10** — Environnements virtuels & librairies (`venv`, `pip`)
- **02:10–03:10** — Dessins avec `turtle` (carré, spirale, étoiles)
- **03:10–03:45** — Mini-projet 2 : Motifs visuels (libre)
- **03:45–04:00** — Récap & pistes pour continuer

---

## 0) Installer Python & choisir un environnement de dev

### Option A — Ultra simple (recommandée pour débuter) : **Thonny**

- Télécharge **Thonny** (Windows/macOS/Linux). Il **intègre Python** et évite les soucis de configuration.
- Ouvre Thonny → en bas à droite, vérifie la version de Python.

> Si tu utilises Thonny pour ce cours, tu peux **sauter** la création de l’environnement virtuel au début, et y revenir plus tard (section 3).

### Option B — Éditeur moderne : **VS Code** + Python

1. **Installer Python**

- **macOS** :

  - Télécharge Python 3 sur python.org (installeur officiel).
  - **Ouvre Terminal** et vérifie :

    ```bash
    python3 --version
    pip3 --version
    ```

- **Windows** :

  - Installe Python 3 (coche “Add Python to PATH” pendant l’installation).
  - **Ouvre PowerShell** et vérifie :

    ```powershell
    py --version
    py -m pip --version
    ```

- **Linux (Debian/Ubuntu)** :

  ```bash
  sudo apt update
  sudo apt install -y python3 python3-pip python3-venv
  python3 --version
  pip3 --version
  ```

> **Note Turtle (dessins)** : `turtle` fait partie de Python, mais nécessite **Tk**.
>
> - macOS (installeur python.org) & Windows : OK d’office.
> - Linux : installe Tk si besoin → `sudo apt install -y python3-tk`.

2. **Installer VS Code** et l’extension **Python** (éditeur → Extensions → rechercher “Python” par Microsoft).

3. **Créer un dossier de projet**

```bash
# macOS / Linux
mkdir ~/python-kid && cd ~/python-kid

# Windows (PowerShell)
mkdir $HOME\python-kid
cd $HOME\python-kid
```

---

## 1) Créer et utiliser un **environnement virtuel** (propre & pro)

> Même si Thonny le masque, il est bien d’apprendre “la bonne manière”.

### Création

- **macOS / Linux**

  ```bash
  python3 -m venv .venv
  source .venv/bin/activate
  python --version     # doit afficher Python 3.x dans l'environnement
  pip install --upgrade pip
  ```

- **Windows (PowerShell)**

  ```powershell
  py -m venv .venv
  .\.venv\Scripts\Activate.ps1
  python --version
  py -m pip install --upgrade pip
  ```

> Si l’activation échoue sous PowerShell, exécute **en admin** :
> `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` puis réessaye.

### Installer une librairie

```bash
pip install rich
```

### Geler les dépendances (facultatif, “pro”)

```bash
pip freeze > requirements.txt
```

Pour réinstaller ailleurs :

```bash
pip install -r requirements.txt
```

### Quitter l’environnement (plus tard)

```bash
deactivate
```

---

## 2) Premiers programmes (texte)

Crée un fichier **`hello.py`** :

```python
print("Bonjour !")
name = input("Comment t'appelles-tu ? ")
print("Enchanté,", name, "!")
```

Exécuter :

- **Thonny** : bouton “Run”.
- **Terminal** :

  - macOS/Linux : `python hello.py`
  - Windows : `python hello.py` (ou `py hello.py`)

### Variables & opérations — `bases.py`

```python
a = 7
b = 3
print("a + b =", a + b)
print("a * b =", a * b)
print("a ** b (puissance) =", a ** b)
```

### Conditions — `conditions.py`

```python
age = int(input("Ton âge ? "))
if age >= 18:
    print("Majeur.")
else:
    print("Mineur.")
```

### Boucles — `boucles.py`

```python
# for
for i in range(1, 6):
    print("Compteur:", i)

# while
compte = 3
while compte > 0:
    print("Encore", compte)
    compte -= 1
print("Go!")
```

---

## 3) Mini-projet 1 (texte) — **Jeu de devinettes** 🎯

Crée **`devinette.py`** (code final “qui marche”) :

```python
import random

SECRET_MIN = 1
SECRET_MAX = 100
secret = random.randint(SECRET_MIN, SECRET_MAX)
essais = 0

print(f"Je pense à un nombre entre {SECRET_MIN} et {SECRET_MAX}...")

while True:
    reponse = input("Devine : ")
    try:
        n = int(reponse)
    except ValueError:
        print("Tape un nombre entier 😉")
        continue

    essais += 1

    if n < secret:
        print("Trop petit !")
    elif n > secret:
        print("Trop grand !")
    else:
        print(f"Bravo ! Trouvé en {essais} essai(s).")
        break
```

**Idées d’amélioration** : limiter à 7 essais, afficher “chaud/froid”, enregistrer le meilleur score dans un fichier.

---

## 4) Dessiner avec `turtle` (visuel & fun)

> Si une fenêtre ne s’affiche pas : vérifie Tk (voir plus haut).

### Carré — `carre.py`

```python
import turtle

t = turtle.Turtle()
t.speed(3)

for _ in range(4):
    t.forward(120)
    t.right(90)

turtle.done()
```

### Polygone & paramètre — `polygone.py`

```python
import turtle

t = turtle.Turtle()
t.speed(0)

nb_cotes = 6          # essaie 3, 5, 8...
longueur = 100
angle = 360 / nb_cotes

for _ in range(nb_cotes):
    t.forward(longueur)
    t.right(angle)

turtle.done()
```

### Spirale simple — `spirale.py`

```python
import turtle

t = turtle.Turtle()
t.speed(0)

longueur = 5
for i in range(80):
    t.forward(longueur)
    t.right(20)
    longueur += 3

turtle.done()
```

### Étoile (style “ninja star”) — `etoile.py`

```python
import turtle

t = turtle.Turtle()
t.speed(0)

branches = 36
for _ in range(branches):
    t.forward(150)
    t.right(170)   # change cet angle pour varier le motif

turtle.done()
```

---

## 5) Mini-projet 2 (visuel) — **Rosace paramétrique** 🌸

Crée **`rosace.py`** :

```python
import turtle

t = turtle.Turtle()
t.speed(0)

petales = 24         # essaie 12, 36...
rayon = 140
rotation = 360 / petales

for _ in range(petales):
    # demi-cercle
    t.circle(rayon, 60)
    t.left(120)
    t.circle(rayon, 60)
    # se tourner pour le pétale suivant
    t.left(180 - 60)
    t.left(rotation)

turtle.done()
```

**Défis** :

- Demander les paramètres à l’utilisateur (`input()`), convertir en `int`.
- Changer la vitesse, la taille, le nombre de pétales.
- Ajouter un fond (voir `turtle.Screen().bgcolor(...)`).

---

## 6) Bonus (si motivé) — **Fractale de Koch** ❄️

> Introduit la **récursion** de façon ludique.

`koch.py` :

```python
import turtle

t = turtle.Turtle()
t.speed(0)

def segment_koch(longueur, ordre):
    if ordre == 0:
        t.forward(longueur)
    else:
        segment_koch(longueur/3, ordre-1)
        t.left(60)
        segment_koch(longueur/3, ordre-1)
        t.right(120)
        segment_koch(longueur/3, ordre-1)
        t.left(60)
        segment_koch(longueur/3, ordre-1)

def flocon(longueur, ordre):
    for _ in range(3):
        segment_koch(longueur, ordre)
        t.right(120)

# Essaie ordre 0, 1, 2, 3, 4 (commence petit !)
flocon(300, 3)
turtle.done()
```

---

## 7) Utiliser VS Code proprement (si tu choisis cette voie)

1. Ouvre ton dossier de projet.
2. Active l’environnement virtuel **dans le terminal intégré**.
3. Installe l’extension **Python** (Microsoft).
4. Sélectionne l’interpréteur : `Ctrl/Cmd + Shift + P` → “Python: Select Interpreter” → choisis **.venv**.
5. Crée un fichier `.py` et lance-le (triangle “Run” ou via terminal).

---

## 8) Installer des librairies (dans l’environnement)

- Exemple :

  ```bash
  pip install colorama
  ```

- Utilisation :

  ```python
  from colorama import init, Fore
  init()
  print(Fore.GREEN + "Texte en vert !")
  ```

- Lister ce que tu as installé :

  ```bash
  pip list
  ```

- Sauvegarder :

  ```bash
  pip freeze > requirements.txt
  ```

---

## 9) Dépannage rapide

- **`python3: command not found` (mac/Linux)** → installe Python (ou utilise `python`).
- **`py: command not found` (mac/Linux)** → `py` est spécifique Windows, utilise `python3`.
- **Fenêtre Turtle ne s’ouvre pas** → installe Tk (Linux : `sudo apt install -y python3-tk`).
- **Permission PowerShell (Windows)** →
  `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
- **Thonny** ne trouve pas un module que tu as installé ailleurs → Thonny utilise **son** Python intégré. Installe via **Outils → Gérer les paquets** dans Thonny, ou bascule sur ton venv dans VS Code.

---

## 10) Fiche récap (à imprimer)

- **Exécuter un script** : `python mon_script.py`
- **Créer venv** : `python -m venv .venv`
- **Activer venv** :

  - mac/Linux : `source .venv/bin/activate`
  - Windows : `.venv\Scripts\Activate.ps1`

- **Installer** : `pip install nom_du_package`
- **Geler** : `pip freeze > requirements.txt`
- **Désactiver venv** : `deactivate`
- **Modules utiles** : `random`, `math`, `turtle`, `datetime`

---

## 11) Idées de défis (selon l’humeur)

- **Jeu de devinettes** : limite d’essais, “chaud/froid”, meilleur score enregistré.
- **Turtle** : tracer un labyrinthe, un drapeau, un flocon animé (boucler plusieurs ordres).
- **Texte** : petit “chatbot” avec réponses conditionnelles.

---

## 12) Organisation pour la séance

1. **Installer/ouvrir** (Thonny _ou_ VS Code + Python).
2. **Hello World**, variables, `input`.
3. **Conditions/loops** → mini-exercices.
4. **Jeu de devinettes** (code final ci-dessus).
5. **Venv & pip** (montrer la bonne pratique).
6. **Turtle** → carré, spirale, étoile.
7. **Mini-projet visuel** (libre).
8. **Récap** & idées pour la suite.

---

Si tu veux, je peux aussi te **préparer un pack .zip** avec tous les fichiers (`hello.py`, `bases.py`, `devinette.py`, `carre.py`, etc.) et un petit **README** prêt à distribuer à l’élève.
