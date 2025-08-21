# Mini-cours Python (débutant) — Dessiner pixels & lignes qui rebondissent 🎨

> Objectif (2–4 h) : installer Python, créer un environnement, ouvrir une fenêtre, **dessiner des pixels et des lignes**, puis **animer une ligne qui rebondit** sur les bords (style économiseur d’écran).
> Codes simples, variables en français, commentaires en français, **prêts à lancer**.

---

## 1) Installation rapide

### 1.1 Installer Python 3

- **macOS / Windows / Linux** → télécharge Python 3 depuis python.org et installe.
- Vérifie dans un terminal :

  - macOS / Linux :

    ```bash
    python3 --version
    pip3 --version
    ```

  - Windows (PowerShell) :

    ```powershell
    py --version
    py -m pip --version
    ```

### 1.2 Créer un dossier de projet

```bash
# macOS / Linux
mkdir ~/python-dessin && cd ~/python-dessin

# Windows (PowerShell)
mkdir $HOME\python-dessin
cd $HOME\python-dessin
```

### 1.3 Créer et activer un **environnement** (venv)

- macOS / Linux :

  ```bash
  python3 -m venv .venv
  source .venv/bin/activate
  python -m pip install --upgrade pip wheel setuptools
  ```

- Windows (PowerShell) :

  ```powershell
  py -m venv .venv
  .\.venv\Scripts\Activate.ps1
  py -m pip install --upgrade pip wheel setuptools
  ```

  > Si PowerShell refuse d’exécuter :
  > `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

### 1.4 Installer **Pygame**

```bash
pip install pygame
```

Test rapide :

```bash
python -c "import pygame; print('pygame', pygame.version.ver)"
```

---

## 2) Première fenêtre (test)

Crée **`00_hello_fenetre.py`** :

```python
import pygame, sys

pygame.init()
LARGEUR, HAUTEUR = 640, 400
ecran = pygame.display.set_mode((LARGEUR, HAUTEUR))
pygame.display.set_caption("Fenêtre OK")
horloge = pygame.time.Clock()

while True:
    for ev in pygame.event.get():
        if ev.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    ecran.fill((20, 20, 30))  # fond
    pygame.display.flip()
    horloge.tick(60)  # 60 images/seconde
```

Lance :

```bash
python 00_hello_fenetre.py
```

---

## 3) Dessiner des **pixels**

Crée **`01_pixels.py`** :

```python
import pygame, sys, random

pygame.init()
LARGEUR, HAUTEUR = 640, 400
ecran = pygame.display.set_mode((LARGEUR, HAUTEUR))
pygame.display.set_caption("Pixels")
horloge = pygame.time.Clock()

def poser_pixel(x, y, couleur):
    # méthode rapide : dessiner un petit rectangle 1x1
    ecran.fill(couleur, rect=pygame.Rect(x, y, 1, 1))

while True:
    for ev in pygame.event.get():
        if ev.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    # fond sombre
    ecran.fill((10, 10, 15))

    # poser 500 pixels aléatoires à chaque frame
    for _ in range(500):
        x = random.randrange(LARGEUR)
        y = random.randrange(HAUTEUR)
        c = (random.randrange(256), random.randrange(256), random.randrange(256))
        poser_pixel(x, y, c)

    pygame.display.flip()
    horloge.tick(30)
```

> Variante : au lieu d’aléatoire, poser des pixels selon une formule (ex : diagonale, vagues, etc.).

---

## 4) Dessiner une **ligne** statique

Crée **`02_ligne_simple.py`** :

```python
import pygame, sys

pygame.init()
LARGEUR, HAUTEUR = 640, 400
ecran = pygame.display.set_mode((LARGEUR, HAUTEUR))
pygame.display.set_caption("Ligne statique")
horloge = pygame.time.Clock()

# deux points (x, y)
p1 = (50, 50)
p2 = (500, 300)
BLANC = (240, 240, 240)

while True:
    for ev in pygame.event.get():
        if ev.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    ecran.fill((0, 0, 0))
    # ligne de p1 à p2, épaisseur 2
    pygame.draw.line(ecran, BLANC, p1, p2, width=2)

    pygame.display.flip()
    horloge.tick(60)
```

---

## 5) **Animer** une ligne qui se déplace (rebond de tout le segment)

Crée **`03_ligne_mobile.py`** :

```python
import pygame, sys

pygame.init()
LARGEUR, HAUTEUR = 640, 400
ecran = pygame.display.set_mode((LARGEUR, HAUTEUR))
pygame.display.set_caption("Ligne mobile")
horloge = pygame.time.Clock()

# Ligne définie par un point de départ + un vecteur (dx, dy) commun
p1 = [100.0, 120.0]
p2 = [300.0, 200.0]
v = [3.0, 2.0]  # vitesse (déplacement par frame, en pixels)
COULEUR = (0, 200, 255)

def deplace_point(p, v):
    p[0] += v[0]
    p[1] += v[1]

def rebond_si_bord(p, v):
    # rebond si un des deux points sort de l'écran
    if p[0] < 0 or p[0] > LARGEUR:
        v[0] = -v[0]
    if p[1] < 0 or p[1] > HAUTEUR:
        v[1] = -v[1]

while True:
    for ev in pygame.event.get():
        if ev.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    # déplacer la ligne
    deplace_point(p1, v)
    deplace_point(p2, v)

    # rebond si un point touche un bord
    rebond_si_bord(p1, v)
    rebond_si_bord(p2, v)

    # dessiner
    ecran.fill((12, 12, 18))
    pygame.draw.line(ecran, COULEUR, p1, p2, width=3)
    pygame.display.flip()
    horloge.tick(60)
```

> Ici **tout le segment** se déplace comme un objet rigide : on inverse la vitesse si un **des deux points** sort.

---

## 6) **Animer une “vraie” ligne élastique** (chaque extrémité rebondit)

Crée **`04_ligne_elastique.py`** :

```python
import pygame, sys

pygame.init()
LARGEUR, HAUTEUR = 800, 480
ecran = pygame.display.set_mode((LARGEUR, HAUTEUR))
pygame.display.set_caption("Ligne élastique (chaque bout rebondit)")
horloge = pygame.time.Clock()

# chaque extrémité a sa propre vitesse
p1 = [100.0, 100.0]; v1 = [3.0, 2.5]
p2 = [600.0, 300.0]; v2 = [-2.0, 3.5]

COULEUR = (255, 180, 0)
EPAISSEUR = 3
POINTS = (220, 220, 255)

def deplace(p, v):
    p[0] += v[0]
    p[1] += v[1]
    # rebond
    if p[0] < 0:  p[0] = 0;           v[0] *= -1
    if p[0] > LARGEUR:  p[0] = LARGEUR; v[0] *= -1
    if p[1] < 0:  p[1] = 0;           v[1] *= -1
    if p[1] > HAUTEUR: p[1] = HAUTEUR;  v[1] *= -1

while True:
    for ev in pygame.event.get():
        if ev.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    deplace(p1, v1)
    deplace(p2, v2)

    ecran.fill((8, 10, 14))
    pygame.draw.line(ecran, COULEUR, p1, p2, width=EPAISSEUR)
    # petits ronds sur les extrémités (joli et pédagogique)
    pygame.draw.circle(ecran, POINTS, (int(p1[0]), int(p1[1])), 5)
    pygame.draw.circle(ecran, POINTS, (int(p2[0]), int(p2[1])), 5)

    pygame.display.flip()
    horloge.tick(60)
```

---

## 7) Plusieurs lignes qui rebondissent (petit “screensaver”)

Crée **`05_lignes_screensaver.py`** :

```python
import pygame, sys, random

pygame.init()
LARGEUR, HAUTEUR = 900, 600
ecran = pygame.display.set_mode((LARGEUR, HAUTEUR))
pygame.display.set_caption("Lignes qui rebondissent")
horloge = pygame.time.Clock()

NB_LIGNES = 12
EPAISSEUR = 2

def nouvelle_extremite():
    return [random.uniform(0, LARGEUR), random.uniform(0, HAUTEUR)]

def nouvelle_vitesse():
    # vitesses aléatoires mais pas trop lentes
    vx = random.uniform(-3.5, 3.5)
    vy = random.uniform(-3.5, 3.5)
    if abs(vx) < 1.0: vx = 1.2 * (1 if vx >= 0 else -1)
    if abs(vy) < 1.0: vy = 1.2 * (1 if vy >= 0 else -1)
    return [vx, vy]

lignes = []
for _ in range(NB_LIGNES):
    p1, p2 = nouvelle_extremite(), nouvelle_extremite()
    v1, v2 = nouvelle_vitesse(), nouvelle_vitesse()
    couleur = tuple(random.randrange(80, 256) for _ in range(3))
    lignes.append([p1, v1, p2, v2, couleur])

def deplace(p, v):
    p[0] += v[0]; p[1] += v[1]
    if p[0] < 0: p[0] = 0; v[0] *= -1
    if p[0] > LARGEUR: p[0] = LARGEUR; v[0] *= -1
    if p[1] < 0: p[1] = 0; v[1] *= -1
    if p[1] > HAUTEUR: p[1] = HAUTEUR; v[1] *= -1

while True:
    for ev in pygame.event.get():
        if ev.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    ecran.fill((5, 7, 10))

    for p1, v1, p2, v2, couleur in lignes:
        deplace(p1, v1)
        deplace(p2, v2)
        pygame.draw.line(ecran, couleur, p1, p2, width=EPAISSEUR)

    pygame.display.flip()
    horloge.tick(60)
```

---

## 8) Petits défis (faciles et motivants)

- **Changer les couleurs** selon la vitesse (plus rapide → plus clair).
- **Épais vs fin** : faire varier l’épaisseur des lignes.
- **Trainées lumineuses** : au lieu de `ecran.fill(...)` en noir chaque frame, remplis avec un **noir semi-transparent** (technique : surface supplémentaire avec alpha) pour garder des traces.
- **Interaction** : clic de souris → ajoute une nouvelle ligne, touche **Espace** → pause, **R** → réinitialiser.
- **Compteur FPS** : afficher le nombre d’images/seconde dans le titre.

Ex. compteur FPS minimal :

```python
# dans la boucle principale, après horloge.tick(...)
fps = int(horloge.get_fps())
pygame.display.set_caption(f"Lignes — {fps} FPS")
```

---

## 9) Mémo des commandes

```bash
# activer l'environnement
# macOS / Linux :
source .venv/bin/activate
# Windows :
.\.venv\Scripts\Activate.ps1

# installer pygame
pip install pygame

# lancer un script
python 05_lignes_screensaver.py

# désactiver l'environnement
deactivate
```

---

Si tu veux, je peux te préparer une **petite variante “trainée lumineuse”** ou l’ajout **d’interactions (clic pour ajouter une ligne)** — dis-moi ce que tu préfères 👍
