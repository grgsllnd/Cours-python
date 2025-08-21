Parfait 👍 voici une **fiche récapitulatif ultra-simple** au format **Markdown (.md)**, claire et prête à imprimer/donner comme aide-mémoire :

---

````markdown
# 🐍 Fiche mémo Python (débutant)

## Afficher du texte

```python
print("Bonjour")
```
````

## Variables et types

```python
texte = "Hello"      # str (chaîne)
nombre = 42          # int (entier)
pi = 3.14            # float (décimal)
vrai_ou_faux = True  # bool (booléen)
```

## Demander à l’utilisateur

```python
nom = input("Ton nom ? ")
print("Salut", nom)
```

## Opérateurs de base

```python
a = 10
b = 3

print(a + b)   # addition → 13
print(a - b)   # soustraction → 7
print(a * b)   # multiplication → 30
print(a / b)   # division → 3.333...
print(a // b)  # division entière → 3
print(a % b)   # reste (modulo) → 1
print(a ** b)  # puissance → 1000
```

## Comparaisons (renvoient True ou False)

```python
print(a == b)   # égal ?
print(a != b)   # différent ?
print(a > b)    # plus grand ?
print(a <= b)   # plus petit ou égal ?
```

## Conditions

```python
age = int(input("Ton âge ? "))

if age >= 18:
    print("Majeur")
elif age >= 13:
    print("Adolescent")
else:
    print("Enfant")
```

## Boucles

```python
# Compter de 1 à 5
for i in range(1, 6):
    print(i)

# Répéter jusqu’à ce que la condition soit vraie
mot = ""
while mot != "python":
    mot = input("Tape 'python' : ")
```

## Mini-exemples

### Table de multiplication

```python
n = int(input("Choisis un nombre : "))
for i in range(1, 11):
    print(n, "x", i, "=", n * i)
```

### Jeu de devinette

```python
import random
secret = random.randint(1, 20)
trouve = False

while not trouve:
    essai = int(input("Devine : "))
    if essai == secret:
        print("Bravo !")
        trouve = True
    elif essai < secret:
        print("Trop petit")
    else:
        print("Trop grand")
```

---

```

---

👉 Veux-tu que je t’ajoute aussi une **section mini-dessin ASCII** (par ex. dessiner un carré ou un triangle avec des `print("*")`), histoire d’avoir un côté visuel même en mode texte ?
```
