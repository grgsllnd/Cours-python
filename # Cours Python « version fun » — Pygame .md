# Cours Python « version fun » — Pygame (Snake + Jeu de la Vie) 🎮

> Objectif (4 h) : partir de zéro (installation + venv), afficher une fenêtre Pygame, puis coder **un Snake jouable** et (bonus) **le Jeu de la Vie**.
> Tout est **clé en main** : commandes, fichiers, et codes finaux.

---

## 1) Préparer l’environnement

### 1.1 Installer Python 3

* **macOS** (recommandé) : télécharge Python sur python.org (installateur “universal2”).
  Puis, dans **Terminal** :

  ```bash
  python3 --version
  pip3 --version
  ```
* **Windows** : installe Python 3 depuis python.org **en cochant** “Add Python to PATH”.
  Dans **PowerShell** :

  ```powershell
  py --version
  py -m pip --version
  ```
* **Linux (Debian/Ubuntu)** :

  ```bash
  sudo apt update
  sudo apt install -y python3 python3-pip python3-venv python3-tk
  python3 --version
  pip3 --version
  ```

> 💡 Si tu es sur macOS/Apple Silicon et que l’installation de Pygame échoue avec Python 3.13, installe Python 3.12 (via python.org) — Pygame suit vite mais parfois une version de Python toute récente arrive avant la wheel Pygame.

---

### 1.2 Créer un dossier de projet

```bash
# macOS / Linux
mkdir ~/python-games && cd ~/python-games

# Windows (PowerShell)
mkdir $HOME\python-games
cd $HOME\python-games
```

---

### 1.3 Créer et activer un environnement virtuel

* **macOS / Linux**

  ```bash
  python3 -m venv .venv
  source .venv/bin/activate
  python --version
  python -m pip install --upgrade pip setuptools wheel
  ```
* **Windows (PowerShell)**

  ```powershell
  py -m venv .venv
  .\.venv\Scripts\Activate.ps1
  python --version
  py -m pip install --upgrade pip setuptools wheel
  ```

  > Si PowerShell bloque l’activation :
  > lance **en administrateur** :
  > `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

---

### 1.4 Installer Pygame dans le venv

```bash
pip install pygame
```

Vérification rapide :

```bash
python -c "import pygame; print('pygame', pygame.version.ver)"
```

---

## 2) Première fenêtre Pygame (test rapide)

Crée **`hello_pygame.py`** :

```python
import pygame, sys

pygame.init()
screen = pygame.display.set_mode((640, 360))
pygame.display.set_caption("Test Pygame")
clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 36)

while True:
    for e in pygame.event.get():
        if e.type == pygame.QUIT:
            pygame.quit(); sys.exit()

    screen.fill((20, 20, 30))
    txt = font.render("Pygame OK 👍", True, (230, 230, 230))
    screen.blit(txt, (20, 20))
    pygame.display.flip()
    clock.tick(60)
```

Lancer :

```bash
python hello_pygame.py
```

Si la fenêtre s’affiche → on enchaîne.

---

## 3) Projet 1 — Snake 🐍 (code final prêt à jouer)

Crée **`snake.py`** (entier) :

```python
import pygame, sys, random

# --- Paramètres ---
CELL = 20                 # taille d'une case en pixels
GRID_W, GRID_H = 30, 20   # grille 30x20 => fenêtre 600x400

SPEED_START = 8           # FPS de départ
SPEED_MAX = 20

BG = (18, 18, 18)
GRID_COLOR = (30, 30, 30)
HEAD_COLOR = (0, 255, 0)
BODY_COLOR = (0, 200, 0)
FOOD_COLOR = (255, 80, 80)
TEXT_COLOR = (230, 230, 230)
HINT_COLOR = (255, 200, 0)

class SnakeGame:
    def __init__(self):
        pygame.init()
        self.screen = pygame.display.set_mode((GRID_W * CELL, GRID_H * CELL))
        pygame.display.set_caption("Snake — Python")
        self.clock = pygame.time.Clock()
        self.font = pygame.font.SysFont(None, 32)
        self.reset()

    def reset(self):
        mid = (GRID_W // 2, GRID_H // 2)
        self.snake = [mid, (mid[0] - 1, mid[1]), (mid[0] - 2, mid[1])]
        self.dir = (1, 0)          # direction courante (dx, dy)
        self.next_dir = self.dir   # direction à appliquer au prochain tick
        self.score = 0
        self.speed = SPEED_START
        self.game_over = False
        self.spawn_food()

    def spawn_food(self):
        empty = [(x, y) for x in range(GRID_W) for y in range(GRID_H)
                 if (x, y) not in self.snake]
        self.food = random.choice(empty) if empty else None

    def handle_input(self):
        for e in pygame.event.get():
            if e.type == pygame.QUIT:
                pygame.quit(); sys.exit()
            if e.type == pygame.KEYDOWN:
                if self.game_over:
                    if e.key == pygame.K_r:
                        self.reset()
                else:
                    nd = None
                    if e.key in (pygame.K_UP, pygame.K_w): nd = (0, -1)
                    elif e.key in (pygame.K_DOWN, pygame.K_s): nd = (0, 1)
                    elif e.key in (pygame.K_LEFT, pygame.K_a): nd = (-1, 0)
                    elif e.key in (pygame.K_RIGHT, pygame.K_d): nd = (1, 0)
                    if nd and not (nd[0] == -self.dir[0] and nd[1] == -self.dir[1]):
                        self.next_dir = nd

    def update(self):
        if self.game_over:
            return
        # appliquer la nouvelle direction
        self.dir = self.next_dir
        hx, hy = self.snake[0]
        nh = (hx + self.dir[0], hy + self.dir[1])

        # collisions murs
        if not (0 <= nh[0] < GRID_W and 0 <= nh[1] < GRID_H):
            self.game_over = True
            return
        # collision sur soi
        if nh in self.snake:
            self.game_over = True
            return

        # avancer
        self.snake.insert(0, nh)

        # manger ?
        if self.food and nh == self.food:
            self.score += 1
            self.speed = min(SPEED_MAX, self.speed + 0.5)
            self.spawn_food()
        else:
            self.snake.pop()

    def draw_cell(self, pos, color):
        rect = pygame.Rect(pos[0] * CELL, pos[1] * CELL, CELL, CELL)
        pygame.draw.rect(self.screen, color, rect)

    def draw(self):
        self.screen.fill(BG)

        # grille optionnelle (repères visuels)
        for x in range(GRID_W):
            pygame.draw.line(self.screen, GRID_COLOR, (x * CELL, 0), (x * CELL, GRID_H * CELL))
        for y in range(GRID_H):
            pygame.draw.line(self.screen, GRID_COLOR, (0, y * CELL), (GRID_W * CELL, y * CELL))

        # nourriture
        if self.food:
            self.draw_cell(self.food, FOOD_COLOR)

        # serpent
        for i, seg in enumerate(self.snake):
            self.draw_cell(seg, HEAD_COLOR if i == 0 else BODY_COLOR)

        # HUD
        score_txt = self.font.render(f"Score : {self.score}", True, TEXT_COLOR)
        self.screen.blit(score_txt, (10, 10))

        if self.game_over:
            over = self.font.render("Game Over — Appuie sur R pour recommencer", True, HINT_COLOR)
            rect = over.get_rect(center=(GRID_W * CELL // 2, GRID_H * CELL // 2))
            self.screen.blit(over, rect)

        pygame.display.flip()

    def run(self):
        while True:
            self.handle_input()
            self.update()
            self.draw()
            self.clock.tick(int(self.speed if not self.game_over else 12))

if __name__ == "__main__":
    SnakeGame().run()
```

Lancer :

```bash
python snake.py
```

**Contrôles** : Flèches (ou WASD) pour bouger — **R** pour recommencer après Game Over.

**Améliorations faciles** :

* Ajouter des **murs** (listes de cases interdites).
* Mettre 2 pommes à la fois.
* Ajout d’un **meilleur score** sauvegardé dans un fichier (`json`).

---

## 4) Projet 2 — Jeu de la Vie (Conway) 🧬 (bonus)

Crée **`life.py`** (entier) :

```python
import pygame, sys, random

# --- Paramètres ---
CELL = 10
GRID_W, GRID_H = 80, 60        # 800x600
BG = (15, 15, 20)
GRID_COLOR = (35, 35, 45)
ALIVE = (0, 220, 180)
TEXT = (230, 230, 230)
HINT = (200, 200, 100)

class Life:
    def __init__(self):
        pygame.init()
        self.w, self.h = GRID_W * CELL, GRID_H * CELL
        self.screen = pygame.display.set_mode((self.w, self.h))
        pygame.display.set_caption("Jeu de la Vie — Conway")
        self.clock = pygame.time.Clock()
        self.font = pygame.font.SysFont(None, 24)
        self.running = False
        self.speed = 10  # itérations par seconde
        self.grid = [[0] * GRID_W for _ in range(GRID_H)]
        self.mouse_drawing = None  # None / 0 / 1

    def randomize(self, density=0.2):
        for y in range(GRID_H):
            for x in range(GRID_W):
                self.grid[y][x] = 1 if random.random() < density else 0

    def clear(self):
        for y in range(GRID_H):
            for x in range(GRID_W):
                self.grid[y][x] = 0

    def toggle_cell_at_pixel(self, px, py, value=None):
        x = px // CELL
        y = py // CELL
        if 0 <= x < GRID_W and 0 <= y < GRID_H:
            if value is None:
                self.grid[y][x] = 1 - self.grid[y][x]
            else:
                self.grid[y][x] = value

    def neighbors(self, x, y):
        s = 0
        for dy in (-1, 0, 1):
            for dx in (-1, 0, 1):
                if dx == 0 and dy == 0:
                    continue
                nx, ny = x + dx, y + dy
                if 0 <= nx < GRID_W and 0 <= ny < GRID_H:
                    s += self.grid[ny][nx]
        return s

    def step(self):
        new = [[0] * GRID_W for _ in range(GRID_H)]
        for y in range(GRID_H):
            for x in range(GRID_W):
                n = self.neighbors(x, y)
                if self.grid[y][x] == 1:
                    new[y][x] = 1 if n in (2, 3) else 0
                else:
                    new[y][x] = 1 if n == 3 else 0
        self.grid = new

    def handle_input(self):
        for e in pygame.event.get():
            if e.type == pygame.QUIT:
                pygame.quit(); sys.exit()
            elif e.type == pygame.KEYDOWN:
                if e.key == pygame.K_SPACE:      # pause/play
                    self.running = not self.running
                elif e.key == pygame.K_c:         # clear
                    self.running = False
                    self.clear()
                elif e.key == pygame.K_r:         # random
                    self.running = False
                    self.randomize(0.22)
                elif e.key == pygame.K_n:         # next step
                    if not self.running:
                        self.step()
                elif e.key == pygame.K_EQUALS or e.key == pygame.K_PLUS:
                    self.speed = min(60, self.speed + 1)
                elif e.key == pygame.K_MINUS or e.key == pygame.K_KP_MINUS:
                    self.speed = max(1, self.speed - 1)
            elif e.type == pygame.MOUSEBUTTONDOWN:
                if e.button in (1, 3):  # gauche=vivant, droit=vide
                    self.mouse_drawing = 1 if e.button == 1 else 0
                    self.toggle_cell_at_pixel(*e.pos, value=self.mouse_drawing)
            elif e.type == pygame.MOUSEBUTTONUP:
                self.mouse_drawing = None
            elif e.type == pygame.MOUSEMOTION and self.mouse_drawing is not None:
                self.toggle_cell_at_pixel(*e.pos, value=self.mouse_drawing)

    def draw(self):
        self.screen.fill(BG)
        # grille
        for x in range(GRID_W):
            pygame.draw.line(self.screen, GRID_COLOR, (x * CELL, 0), (x * CELL, self.h))
        for y in range(GRID_H):
            pygame.draw.line(self.screen, GRID_COLOR, (0, y * CELL), (self.w, y * CELL))
        # cellules vivantes
        for y in range(GRID_H):
            for x in range(GRID_W):
                if self.grid[y][x]:
                    rect = pygame.Rect(x * CELL + 1, y * CELL + 1, CELL - 1, CELL - 1)
                    pygame.draw.rect(self.screen, ALIVE, rect)

        # HUD
        state = "▶️" if self.running else "⏸️"
        hud = self.font.render(
            f"{state}  SPACE=Pause/Play  Click=on/off  R=Random  C=Clear  N=Step  +/-=Vitesse({self.speed})",
            True, TEXT
        )
        self.screen.blit(hud, (10, 10))
        hint = self.font.render("Astuce : dessine un planeur, lance ▶️ et observe.", True, HINT)
        self.screen.blit(hint, (10, 34))

        pygame.display.flip()

    def run(self):
        while True:
            self.handle_input()
            if self.running:
                self.step()
            self.draw()
            self.clock.tick(self.speed if self.running else 30)

if __name__ == "__main__":
    Life().run()
```

Lancer :

```bash
python life.py
```

**Contrôles** :

* **Space** = Play/Pause • **C** = effacer • **R** = aléatoire • **N** = avancer d’un pas
* **Clic gauche** = cellule vivante • **Clic droit** = cellule morte • **+/-** = vitesse

---

## 5) Organisation d’un cours de 4 h (suggestion)

1. **Installation & venv (30 min)**
   Suivre les étapes 1.2 à 1.4. Tester `hello_pygame.py`.
2. **Bases orientées jeu (20 min)**
   Montrer une boucle de jeu (*event → update → draw*).
3. **Snake (1 h 30)**
   Recopier `snake.py`, jouer, puis modifier : taille, vitesse, 2 pommes, murs, meilleur score.
4. **Pause (10 min)**
5. **Jeu de la Vie (1 h)**
   Recopier `life.py`, jouer : dessiner un **planeur**/oscillateur, régler la vitesse.
6. **Récap & défis (30 min)**
   Idées : son sur Snake (`pygame.mixer`), écran titre, niveaux, sauvegarde JSON du meilleur score, thème clair/sombre.

---

## 6) Dépannage rapide

* **`ModuleNotFoundError: No module named 'pygame'`**
  → Active bien ton venv (`source .venv/bin/activate` ou `.venv\Scripts\Activate.ps1`), puis `pip install pygame`.
* **Pygame échoue à s’installer**
  → `pip install --upgrade pip setuptools wheel`
  → Si Python 3.13 sur macOS ARM pose souci, installe Python 3.12 via python.org et recrée le venv.
* **Fenêtre freeze ou clignote**
  → Ne jamais faire de `print` géant dans la boucle ; conserve `clock.tick(...)` pour limiter les FPS.

---

## 7) Commandes utiles (mémo)

```bash
# Créer venv
python3 -m venv .venv

# Activer venv
source .venv/bin/activate          # macOS/Linux
.\.venv\Scripts\Activate.ps1       # Windows PowerShell

# Installer dépendances
pip install pygame

# Lancer un jeu
python snake.py
python life.py

# Désactiver venv
deactivate
```

---

Si tu veux, on peut encore **pimper Snake** (sons, menu, skins, obstacles, score persistant) ou ajouter une **version mobile** (contrôles WASD + ZQSD, toucher/souris).
