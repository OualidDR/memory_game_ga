# 🏗️ ARCHITECTURE - Jeu de Mémoire Évolutif

## Vue d'ensemble

```
┌─────────────────────────────────────────────────────────────┐
│                     main.py                                 │
│            (Point d'entrée - coordonne tout)               │
└──────────────┬──────────────────────────────────────────────┘
               │
      ┌────────┴─────────┬──────────────────┐
      ▼                  ▼                  ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   GAME       │  │     GA       │  │   UTILS      │
│  (src/game/) │  │   (src/ga/)  │  │ (src/utils/) │
└──────────────┘  └──────────────┘  └──────────────┘
```

---

## 1️⃣ MODULE GAME (src/game/)

### Fichier: `config.py`
**Responsabilité:** Centraliser tous les paramètres du jeu
```python
# Constantes du jeu
GAME_CONFIG = {
    "colors": ["🔴", "🟢", "🔵", "🟡"],  # ou ["R", "G", "B", "Y"]
    "max_sequence_length": 50,             # Limite de la séquence
    "initial_level": 1,                    # Départ au niveau 1
    "error_penalty": -10,                  # Pénalité pour erreur
    "level_reward": 1,                     # Score par niveau réussi
}
```

### Fichier: `memory_game.py`
**Responsabilité:** Implémenter le jeu

**Classe:** `MemoryGame`
- **Attributs:**
  - `sequence`: liste des couleurs actuelles
  - `current_level`: niveau actuel
  - `score`: score du joueur
  
- **Méthodes principales:**
  - `__init__()`: Initialiser le jeu
  - `generate_next_color()`: Ajouter une couleur aléatoire à la séquence
  - `play(user_sequence)`: L'agent propose sa réponse
  - `check_sequence(user_seq)`: Valider si c'est correct
  - `reset()`: Réinitialiser le jeu
  - `get_state()`: Retourner l'état actuel pour l'agent

**Flux du jeu:**
```
1. Initialiser avec sequence = []
2. À chaque niveau :
   - Ajouter une couleur aléatoire
   - sequence = [C1, C2, C3, ... Cn]
   - Demander à l'agent de reproduire
   - Si correct → niveau+1, score+1
   - Si erreur → jeu terminé, appliquer pénalité
```

---

## 2️⃣ MODULE GA (src/ga/)

### Fichier: `genome.py`
**Responsabilité:** Représenter un agent/solution

**Classe:** `Genome`
- **Attributs:**
  - `genes`: np.array de poids/paramètres (ex: 100-200 valeurs)
  - `fitness`: Score de cet génome
  
- **Méthodes:**
  - `__init__(gene_size)`: Initialiser aléatoirement
  - `mutate(mutation_rate)`: Appliquer des mutations
  - `crossover(other)`: Créer enfant à partir de 2 parents
  - `set_fitness(score)`: Enregistrer le fitness

**Stratégie de gènes:**
Plusieurs approches possibles (à choisir):
- **Approche 1:** Mémoire = tableau codant les associations "séquence → réponse"
- **Approche 2:** Réseau de neurones minimaliste (poids d'une mini-NN)
- **Approche 3:** Règles d'apprentissage (taux de rétention par position)

### Fichier: `fitness.py`
**Responsabilité:** Évaluer la performance d'un agent

**Fonction:** `evaluate_fitness(genome, game, num_trials)`
```python
def evaluate_fitness(genome, game, num_trials=3):
    """
    Paramètres:
      - genome: l'agent à tester
      - game: l'instance du jeu
      - num_trials: nombre de fois qu'on teste cet agent
    
    Retour:
      - score moyen sur les essais
    """
    # Exécuter le jeu num_trials fois
    # Accumuler les scores
    # Retourner la moyenne
```

### Fichier: `genetic_algorithm.py`
**Responsabilité:** Implémenter la boucle GA

**Classe:** `GeneticAlgorithm`
- **Attributs:**
  - `population`: Liste de genomes
  - `generation`: Numéro de génération actuelle
  - `history`: Historique des scores (pour graphiques)
  
- **Méthodes principales:**
  - `__init__(pop_size, gene_size, game)`: Initialiser
  - `evaluate_population()`: Évaluer tous les agents
  - `selection()`: Sélectionner les meilleurs (ex: top 50%)
  - `reproduction()`: Créer nouvelle génération
  - `run(num_generations)`: Boucle principale GA
  - `get_best_genome()`: Retourner le meilleur agent

**Boucle GA (pseudocode):**
```
Pour chaque génération:
  1. Évaluer fitness de tous les genomes
  2. Trier par fitness
  3. Sélectionner les N meilleurs
  4. Créer nouvelle pop par:
     - Copier les meilleurs (élitisme)
     - Croiser les meilleurs
     - Muter les copies
  5. Enregistrer stats (score moyen, max, etc.)
```

---

## 3️⃣ MODULE UTILS (src/utils/)

### Fichier: `visualization.py`
**Responsabilité:** Créer les graphiques

**Fonctions principales:**
```python
def plot_fitness_evolution(history, save_path):
    """
    Graphique : Score moyen vs Générations
    Axes:
      - X: Numéro de génération
      - Y: Score moyen de la population
    Ajouter aussi:
      - Meilleur score de la génération
      - Ligne de tendance
    """

def plot_comparison(best_genomes, save_path):
    """
    Comparer les meilleurs agents de différentes générations
    """
```

### Fichier: `logger.py`
**Responsabilité:** Logs et affichage

**Fonctions:**
```python
def log_generation(gen_num, stats):
    """Afficher : Génération X | Score moyen: X.XX | Best: XX"""

def save_stats_to_csv(history, filename):
    """Sauvegarder l'historique en CSV"""
```

---

## 4️⃣ MAIN (main.py)

**Responsabilité:** Orchestrer l'exécution complète

**Flux:**
```python
if __name__ == "__main__":
    # 1. Charger configuration
    config = load_config()
    
    # 2. Créer le jeu
    game = MemoryGame(config)
    
    # 3. Créer l'AG
    ga = GeneticAlgorithm(
        pop_size=50,
        gene_size=100,
        game=game
    )
    
    # 4. Exécuter GA
    ga.run(num_generations=100)
    
    # 5. Visualiser résultats
    plot_fitness_evolution(ga.history, "results/evolution.png")
    
    # 6. Tester le meilleur agent
    best = ga.get_best_genome()
    test_agent(best, game)
```

---

## 5️⃣ TESTS (tests/)

### `test_game.py`
```python
# Tester les méthodes du jeu
- test_initialization()
- test_sequence_generation()
- test_correct_play()
- test_incorrect_play()
```

### `test_ga.py`
```python
# Tester l'algorithme génétique
- test_genome_creation()
- test_mutation()
- test_crossover()
- test_fitness_evaluation()
```

---

## 6️⃣ RÉSULTATS (results/)

Les fichiers générés seront:
```
results/
├── evolution.png           # Graphique principal
├── best_agents.png         # Comparaison des meilleurs
├── stats.csv               # Données brutes
└── final_report.txt        # Résumé
```

---

## 📋 Plan d'Implémentation Pas à Pas

### **Phase 1: Fondations** (1-2h)
- [x] Créer structure du projet
- [ ] Implémenter `config.py`
- [ ] Implémenter `memory_game.py`
- [ ] Tester le jeu manuellement

### **Phase 2: Génome & Évaluation** (2-3h)
- [ ] Implémenter `genome.py`
- [ ] Implémenter `fitness.py`
- [ ] Écrire tests pour fitness

### **Phase 3: Algorithme Génétique** (2-3h)
- [ ] Implémenter `genetic_algorithm.py`
- [ ] Boucle GA fonctionnelle
- [ ] Tests GA

### **Phase 4: Visualisation & Résultats** (1-2h)
- [ ] Implémenter visualisations
- [ ] Logger et statistiques
- [ ] Générer rapports

### **Phase 5: Amélioration & Documentation** (1h)
- [ ] Optimiser paramètres
- [ ] Écrire documentation complète
- [ ] Faire graphiques finaux

---

## 🔑 Points Clés

| Aspect | Description |
|--------|-------------|
| **Représentation** | Genomes = vecteurs de poids (100-200 dimensions) |
| **Fitness** | Score au jeu (niveaux réussis) |
| **Mutation** | Petits changements aléatoires dans les poids |
| **Crossover** | Mélanger poids de 2 parents |
| **Sélection** | Garder top 50% par fitness |
| **Générations** | 50-200 générations pour voir une évolution claire |

---

## 🎯 Métriques de Succès

- ✅ Score moyen augmente génération après génération
- ✅ Meilleur agent atteint niveau 10+ (ou plus)
- ✅ Graphique montre évolution claire
- ✅ Code modulaire et testable

---

## 💡 Extensions Possibles

1. **Gymnasium Integration** - Convertir en env Gym standard
2. **Visualisation du jeu** - Affichage graphique des séquences
3. **Hyperparamètres** - Tester différentes mutation_rates
4. **Comparaison** - GA vs autre algo (RL simple, etc.)
5. **Statistiques avancées** - Diversité génétique, convergence prématurée