# Hanoi Tower

## 1. Première Méthode : Résolution Itérative

- Cette version simule l’algorithme récursif de manière itérative.
- À chaque étape, on effectue le seul mouvement légal possible entre deux tiges, selon un schéma cyclique.
- La logique diffère selon que le nombre de disques est pair ou impair.
- **Avantage** : Pas de récursion, donc pas de risque de dépassement de pile pour de grands n.

### Description du Code

#### a. Initialisation

```python
NUMBER_OF_DISKS = 4
number_of_moves = 2 ** NUMBER_OF_DISKS - 1
rods = {
    'A': list(range(NUMBER_OF_DISKS, 0, -1)),
    'B': [],
    'C': []
}
```
- **NUMBER_OF_DISKS** : Nombre de disques à déplacer.
- **number_of_moves** : Nombre minimal de mouvements nécessaires (formule : 2ⁿ - 1).
- **rods** : Dictionnaire représentant les trois tiges (A, B, C), chaque tige contenant une pile de disques (plus grand en bas).

#### b. Fonction `make_allowed_move`

```python
def make_allowed_move(rod1, rod2):    
    forward = False
    if not rods[rod2]:
        forward = True
    elif rods[rod1] and rods[rod1][-1] < rods[rod2][-1]:
        forward = True              
    if forward:
        print(f'Moving disk {rods[rod1][-1]} from {rod1} to {rod2}')
        rods[rod2].append(rods[rod1].pop())
    else:
        print(f'Moving disk {rods[rod2][-1]} from {rod2} to {rod1}')
        rods[rod1].append(rods[rod2].pop())
    
    # display our progress
    print(rods, '\n')
```
- **But** : Effectuer un mouvement légal entre deux tiges.
- **Logique** :
  - Si la tige cible est vide, ou si le disque du dessus de la tige source est plus petit que celui de la tige cible, on déplace de la source vers la cible.
  - Sinon, on fait l’inverse.
- **Affichage** : Affiche le mouvement effectué et l’état des tiges.

#### c. Fonction `move` (itérative)

```python
def move(n, source, auxiliary, target):
    print(rods, '\n')
    for i in range(number_of_moves):
        remainder = (i + 1) % 3
        if remainder == 1:
            if n % 2 != 0:
                print(f'Move {i + 1} allowed between {source} and {target}')
                make_allowed_move(source, target)
            else:
                print(f'Move {i + 1} allowed between {source} and {auxiliary}')
                make_allowed_move(source, auxiliary)
        elif remainder == 2:
            if n % 2 != 0:
                print(f'Move {i + 1} allowed between {source} and {auxiliary}')
                make_allowed_move(source, auxiliary)
            else:
                print(f'Move {i + 1} allowed between {source} and {target}')
                make_allowed_move(source, target)
        elif remainder == 0:
            print(f'Move {i + 1} allowed between {auxiliary} and {target}')
            make_allowed_move(auxiliary, target)
```
- **But** : Effectuer tous les mouvements nécessaires pour résoudre le problème.
- **Logique** :
  - Pour chaque mouvement, selon le numéro du mouvement et la parité du nombre de disques, on détermine quelles tiges sont impliquées.
  - On utilise la fonction `make_allowed_move` pour déplacer les disques.
- **Affichage** : Affiche chaque mouvement et l’état des tiges.

#### d. Lancement

```python
if __name__ == "__main__":
    move(NUMBER_OF_DISKS, 'A', 'B', 'C')
```
- Lance la résolution pour 4 disques de A vers C avec B comme auxiliaire.

---

## 2. Deuxième Méthode : Résolution Récursive des Tours de Hanoï

- **Principe** :
  - Divise le problème en sous-problèmes plus petits.
  - Appelle la fonction elle-même pour gérer les mouvements des piles de taille n-1.
- **Avantage** : Simple, élégant, reflète la structure mathématique du problème.
- **Limite** : Peut dépasser la pile d’appels pour de très grands n.

### Description du Code

#### a. Initialisation

```python
NUMBER_OF_DISKS = 5
A = list(range(NUMBER_OF_DISKS, 0, -1))
B = []
C = []
```
- **A, B, C** : Trois listes représentant les tiges, A contenant tous les disques au départ.

#### b. Fonction `move` (récursive)

```python
def move(n, source, auxiliary, target):
    if n <= 0:
        return
        # move n - 1 disks from source to auxiliary, so they are out of the way
        move(n - 1, source, target, auxiliary)
        
        # move the nth disk from source to target
        target.append(source.pop())
        
        # display our progress
        print(A, B, C, '\n')
        
        # move the n - 1 disks that we left on auxiliary onto target
        move(n - 1,  auxiliary, source, target)
```
- **But** : Déplacer une pile de n disques d’une tige source à une tige cible, en utilisant une tige auxiliaire.
- **Logique** :
  1. Déplacer les n-1 disques du haut de la source vers l’auxiliaire.
  2. Déplacer le plus gros disque (en bas) de la source vers la cible.
  3. Déplacer les n-1 disques de l’auxiliaire vers la cible.
- **Affichage** : Affiche l’état des tiges après chaque mouvement.

#### c. Lancement

```python
if __name__=="__main__":
    move(NUMBER_OF_DISKS, A, B, C)
```
- Lance la résolution pour 5 disques de A vers C avec B comme auxiliaire.

---

## Résumé Comparatif

| Méthode                | Type d’algorithme | Avantage principal         | Limite principale        |
|------------------------|-------------------|---------------------------|--------------------------|
| Première (itérative)   | Itératif          | Pas de récursion, efficace pour grands n | Plus complexe à comprendre |
| Deuxième (récursive)   | Récursif          | Simple, élégant, naturel  | Risque de dépassement de pile |

**Les deux méthodes résolvent le même problème, mais utilisent des approches différentes (itérative vs récursive) pour déplacer tous les disques d’une tige à une autre, en respectant les règles des Tours de Hanoï.**
