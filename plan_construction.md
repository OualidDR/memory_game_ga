# 🛠️ PLAN DE CONSTRUCTION - Pas à Pas

## 📍 Vue d'ensemble

Ce document détaille EXACTEMENT comment construire le projet du zéro jusqu'à l'obtention du graphique final.

---

# ÉTAPE 1️⃣ : INITIALISATION DU PROJET (15 min)

## 1.1 Créer la structure
```bash
bash setup_project.sh
cd memory_game_ga
```

## 1.2 Créer l'environnement virtuel
```bash
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## 1.3 Vérifier la structure
```bash
ls -R src/
```
Résultat attendu:
```
src/
├── __init__.py
├── game/
│   ├── __init__.py
│   ├── config.py
│   └── memory_game.py
├── ga/
│   ├── __init__.py
│   ├── genome.py
│   ├── fitness.py
│   └── genetic_algorithm.py
└── utils/
    ├── __init__.py
    ├── visualization.py
    └── logger.py
```

---

# ÉTAPE 2️⃣ : IMPLÉMENTER LE JEU (1-1.5h)

## 2.1 Remplir `src/game/config.py`

**Objectif:** Centraliser tous les paramètres

```python
# À écrire dans src/game/config.py

GAME_CONFIG = {
    "colors": ["R", "G", "B", "Y"],        # Rouge, Vert, Bleu, Jaune
    "num_colors": 4,
    "max_sequence_length": 50,             # Limite max
    "initial_level": 1,
    "error_penalty": -10,
    "level_reward": 1,
    "sequence_delay": 0.5,                 # Pour affichage (optionnel)
}

# Paramètres additionnels
DEBUG = True
```

## 2.2 Remplir `src/game/memory_game.py`

**Objectif:** Implémenter la logique du jeu Simon/Mémoire

**Structure de la classe:**

```python
import numpy as np
from .config import GAME_CONFIG

class MemoryGame:
    """
    Jeu de mémoire où l'agent reproduit une séquence grandissante.
    
    Flux:
    1. generate_next_color() → ajoute couleur à la séquence
    2. play(user_sequence) → agent propose sa réponse
    3. check_sequence() → validez si correct
    4. Si correct → niveau+1, score+1
       Si erreur → jeu terminé
    """
    
    def __init__(self):
        """Initialiser le jeu."""
        # Récupérer config
        # Initialiser: sequence vide, level 1, score 0
        
    def generate_next_color(self):
        """Ajouter une couleur aléatoire à la séquence."""
        # random.choice(GAME_CONFIG["colors"])
        
    def reset(self):
        """Réinitialiser le jeu pour un nouvel agent."""
        # Vider sequence, level=1, score=0
        
    def play(self, user_sequence):
        """
        L'agent essaye de reproduire la séquence.
        
        Args:
            user_sequence: list de couleurs proposées par l'agent
            
        Returns:
            (score, success): score obtenu, True si correct
        """
        # 1. Augmenter sequence d'une couleur
        # 2. Vérifier si user_sequence == self.sequence
        # 3. Si correct: level+1, score+1, retourner (score, True)
        # 4. Si erreur: appliquer pénalité, retourner (score-penalty, False)
        
    def get_state(self):
        """
        Retourner l'état du jeu pour l'agent.
        
        Returns:
            dict avec:
              - current_sequence: la séquence actuelle
              - current_level: le niveau
              - score: le score actuel
        """
        
    @property
    def is_game_over(self):
        """Le jeu est terminé si une erreur est faite."""
        
    def __repr__(self):
        return f"Level {self.current_level} | Sequence: {self.sequence} | Score: {self.score}"
```

## 2.3 Écrire des tests pour le jeu

**Fichier:** `tests/test_game.py`

```python
import sys
sys.path.insert(0, '../src')

from game.memory_game import MemoryGame

def test_game_initialization():
    """Le jeu démarre au niveau 1 avec séquence vide."""
    game = MemoryGame()
    assert game.current_level == 1
    assert len(game.sequence) == 0
    assert game.score == 0

def test_game_adds_color():
    """À chaque niveau, une couleur est ajoutée."""
    game = MemoryGame()
    game.play([])  # Premier tour
    assert len(game.sequence) == 1
    game.play(game.sequence)  # Reproduire correctement
    assert len(game.sequence) == 2

def test_game_punishes_error():
    """Une erreur applique la pénalité."""
    game = MemoryGame()
    game.play([])  # Séquence: [C1]
    initial_score = game.score
    game.play(["X"])  # Mauvaise réponse
    assert game.score < initial_score
    assert game.is_game_over

def test_game_rewards_success():
    """Une bonne réponse augmente le score."""
    game = MemoryGame()
    game.play([])  # Séquence: [C1]
    correct_answer = game.sequence.copy()
    old_score = game.score
    game.play(correct_answer)
    assert game.score > old_score
    assert not game.is_game_over
```

**Exécuter les tests:**
```bash
cd tests
python -m pytest test_game.py -v
```

✅ **Si tous les tests passent:** Passer à l'étape 3

---

# ÉTAPE 3️⃣ : IMPLÉMENTER LE GÉNOME (45 min)

## 3.1 Remplir `src/ga/genome.py`

**Objectif:** Représenter un agent évolutif

```python
import numpy as np

class Genome:
    """
    Un génome représente un agent capable d'apprendre une séquence.
    
    Attributs:
      - genes: np.array de poids/paramètres (100-200 valeurs)
      - fitness: score obtenu au jeu
    """
    
    def __init__(self, gene_size=100, seed=None):
        """
        Initialiser un génome aléatoire.
        
        Args:
            gene_size: nombre de gènes (poids)
            seed: pour reproductibilité
        """
        # self.genes = np.random.uniform(-1, 1, gene_size)
        # self.fitness = 0
        
    def mutate(self, mutation_rate=0.1, mutation_strength=0.1):
        """
        Appliquer des mutations.
        
        Args:
            mutation_rate: % de gènes à muter (ex: 0.1 = 10%)
            mutation_strength: amplitude des mutations (ex: 0.1 = ±0.1)
        """
        # Pour chaque gène:
        #   - Avec probabilité mutation_rate:
        #     - Ajouter bruit Gaussien N(0, mutation_strength)
        
    @staticmethod
    def crossover(parent1, parent2, seed=None):
        """
        Créer un enfant à partir de 2 parents (uniform crossover).
        
        Args:
            parent1, parent2: Genomes
            seed: reproductibilité
            
        Returns:
            Nouveau Genome (enfant)
        """
        # Pour chaque position i:
        #   - Enfant[i] = Parent1[i] ou Parent2[i] (50/50)
        
    def copy(self):
        """Créer une copie du génome."""
        # return Genome(gene_size=len(self.genes))
        # child.genes = self.genes.copy()
        # return child
        
    def set_fitness(self, fitness_value):
        """Enregistrer le fitness."""
        self.fitness = fitness_value
        
    def __repr__(self):
        return f"Genome(fitness={self.fitness:.2f}, gene_size={len(self.genes)})"
```

## 3.2 Écrire des tests pour le génome

**Fichier:** `tests/test_genome.py`

```python
import sys
sys.path.insert(0, '../src')
import numpy as np
from ga.genome import Genome

def test_genome_initialization():
    """Un génome a la bonne taille."""
    g = Genome(gene_size=100)
    assert len(g.genes) == 100
    assert -1 <= g.genes.min() <= 1
    assert -1 <= g.genes.max() <= 1

def test_genome_mutation():
    """La mutation modifie les gènes."""
    g = Genome(gene_size=100)
    original = g.genes.copy()
    g.mutate(mutation_rate=0.5)
    assert not np.allclose(g.genes, original)

def test_genome_crossover():
    """L'enfant hérite de ses deux parents."""
    parent1 = Genome(gene_size=50)
    parent2 = Genome(gene_size=50)
    child = Genome.crossover(parent1, parent2)
    
    assert len(child.genes) == 50
    # L'enfant devrait avoir des gènes de chaque parent
    assert np.any(child.genes == parent1.genes) or np.any(child.genes == parent2.genes)

def test_genome_copy():
    """La copie est indépendante."""
    g1 = Genome()
    g2 = g1.copy()
    g2.mutate(mutation_rate=1.0)
    assert not np.allclose(g1.genes, g2.genes)
```

---

# ÉTAPE 4️⃣ : IMPLÉMENTER L'ÉVALUATION (45 min)

## 4.1 Remplir `src/ga/fitness.py`

**Objectif:** Évaluer la performance d'un agent

```python
from src.game.memory_game import MemoryGame
from src.game.config import GAME_CONFIG

def evaluate_genome(genome, game_instance=None, num_trials=3):
    """
    Tester un génome au jeu et retourner son score moyen.
    
    Args:
        genome: l'agent à évaluer
        game_instance: une instance du jeu (créer si None)
        num_trials: nombre de fois qu'on teste cet agent
        
    Returns:
        float: score moyen sur tous les essais
    """
    # Si pas de jeu, créer
    if game_instance is None:
        game = MemoryGame()
    else:
        game = game_instance
    
    total_score = 0
    
    for trial in range(num_trials):
        game.reset()
        
        # Boucle de jeu
        while not game.is_game_over:
            # 1. Demander au génome quelle séquence produire
            #    (Utiliser les gènes du génome pour "prédire")
            user_sequence = genome_decide(genome, game.current_level)
            
            # 2. Jouer
            score, success = game.play(user_sequence)
            
            # Si erreur, quitter cette boucle
            if not success:
                break
        
        total_score += game.score
    
    # Retourner la moyenne
    average_score = total_score / num_trials
    return average_score


def genome_decide(genome, level):
    """
    Demander au génome quelle séquence reproduire.
    
    Idée simple:
    - Utiliser les gènes comme "poids de confiance" pour chaque position
    - Extraire les couleurs les plus "confidentes"
    
    C'est une heuristique simple!
    """
    # Pour cette implémentation simple:
    # On prend aleatoirement une séquence basée sur les gènes
    
    # Approche: diviser les gènes par position
    # genes[0:25] → confiance pour position 1
    # genes[25:50] → confiance pour position 2
    # etc.
    
    colors = GAME_CONFIG["colors"]
    sequence = []
    
    for pos in range(level):
        # Sélectionner le gène pour cette position
        if pos < len(genome.genes) // len(colors):
            gene_index = pos * len(colors)
            weights = genome.genes[gene_index:gene_index + len(colors)]
            # Choisir la couleur avec le poids max
            chosen_color = colors[weights.argmax()]
        else:
            # Si on dépasse, choisir aléatoire
            chosen_color = colors[np.random.randint(len(colors))]
        
        sequence.append(chosen_color)
    
    return sequence
```

---

# ÉTAPE 5️⃣ : IMPLÉMENTER L'ALGORITHME GÉNÉTIQUE (1h)

## 5.1 Remplir `src/ga/genetic_algorithm.py`

**Objectif:** Implémenter la boucle GA principale

```python
import numpy as np
from .genome import Genome
from .fitness import evaluate_genome
from src.game.memory_game import MemoryGame

class GeneticAlgorithm:
    """
    Algorithme génétique pour évoluer des stratégies de mémorisation.
    """
    
    def __init__(self, population_size=50, gene_size=100, game=None):
        """
        Initialiser la GA.
        
        Args:
            population_size: nombre d'agents par génération
            gene_size: taille des génomes
            game: instance du jeu (créer si None)
        """
        self.population_size = population_size
        self.gene_size = gene_size
        self.game = game or MemoryGame()
        
        # Initialiser population
        # self.population = [Genome(gene_size) for _ in range(population_size)]
        
        self.generation = 0
        
        # Historique pour graphiques
        self.history = {
            "generation": [],
            "avg_fitness": [],
            "best_fitness": [],
            "worst_fitness": [],
        }
    
    def evaluate_population(self):
        """
        Évaluer tous les genomes de la population.
        Remplir chaque genome.fitness.
        """
        for genome in self.population:
            fitness = evaluate_genome(genome, self.game, num_trials=2)
            genome.set_fitness(fitness)
    
    def selection(self, keep_ratio=0.5):
        """
        Sélectionner les meilleurs genomes.
        
        Args:
            keep_ratio: proportion à garder (ex: 0.5 = top 50%)
            
        Returns:
            list: les meilleurs genomes
        """
        # Trier par fitness
        sorted_pop = sorted(self.population, key=lambda g: g.fitness, reverse=True)
        
        # Garder les meilleurs
        num_to_keep = max(1, int(len(self.population) * keep_ratio))
        return sorted_pop[:num_to_keep]
    
    def reproduce(self, selected):
        """
        Créer une nouvelle génération à partir des sélectionnés.
        
        Args:
            selected: list de genomes à reproduire
            
        Returns:
            list: nouvelle population
        """
        new_population = []
        
        # Élitisme: copier les meilleurs
        for genome in selected:
            new_population.append(genome.copy())
        
        # Remplir le reste par crossover et mutation
        while len(new_population) < self.population_size:
            # Sélectionner 2 parents aléatoires
            parent1 = selected[np.random.randint(len(selected))]
            parent2 = selected[np.random.randint(len(selected))]
            
            # Crossover
            child = Genome.crossover(parent1, parent2)
            
            # Mutation
            child.mutate(mutation_rate=0.1, mutation_strength=0.15)
            
            new_population.append(child)
        
        # Garder seulement population_size
        return new_population[:self.population_size]
    
    def update_history(self):
        """Enregistrer les statistiques de cette génération."""
        fitness_values = [g.fitness for g in self.population]
        
        self.history["generation"].append(self.generation)
        self.history["avg_fitness"].append(np.mean(fitness_values))
        self.history["best_fitness"].append(np.max(fitness_values))
        self.history["worst_fitness"].append(np.min(fitness_values))
    
    def run(self, num_generations=100, verbose=True):
        """
        Exécuter l'algorithme génétique.
        
        Args:
            num_generations: nombre de générations
            verbose: afficher les stats à chaque génération
        """
        for gen in range(num_generations):
            self.generation = gen
            
            # 1. Évaluer population
            self.evaluate_population()
            
            # 2. Enregistrer histoire
            self.update_history()
            
            # 3. Afficher stats
            if verbose and gen % 10 == 0:
                best = max(self.population, key=lambda g: g.fitness)
                avg = np.mean([g.fitness for g in self.population])
                print(f"Gen {gen:3d} | Avg: {avg:.2f} | Best: {best.fitness:.2f}")
            
            # 4. Sélectionner
            selected = self.selection(keep_ratio=0.3)
            
            # 5. Reproduire
            self.population = self.reproduce(selected)
    
    def get_best_genome(self):
        """Retourner le meilleur génome de la population."""
        return max(self.population, key=lambda g: g.fitness)
    
    def get_history(self):
        """Retourner l'historique pour graphiques."""
        return self.history
```

---

# ÉTAPE 6️⃣ : IMPLÉMENTER LA VISUALISATION (30 min)

## 6.1 Remplir `src/utils/visualization.py`

```python
import matplotlib.pyplot as plt
import numpy as np

def plot_fitness_evolution(history, save_path="results/evolution.png"):
    """
    Créer le graphique principal: Score vs Générations.
    
    Args:
        history: dict avec clés 'generation', 'avg_fitness', 'best_fitness'
        save_path: où sauvegarder
    """
    plt.figure(figsize=(12, 6))
    
    # Tracer
    plt.plot(
        history["generation"],
        history["avg_fitness"],
        label="Score moyen",
        marker="o",
        linewidth=2,
        markersize=4,
    )
    
    plt.plot(
        history["generation"],
        history["best_fitness"],
        label="Meilleur score",
        marker="s",
        linewidth=2,
        markersize=4,
        linestyle="--",
    )
    
    plt.plot(
        history["generation"],
        history["worst_fitness"],
        label="Pire score",
        marker="^",
        alpha=0.5,
        linewidth=1,
        linestyle=":",
    )
    
    # Formatage
    plt.xlabel("Génération", fontsize=12, fontweight="bold")
    plt.ylabel("Score (Niveaux Réussis)", fontsize=12, fontweight="bold")
    plt.title("Évolution du Score - Algorithme Génétique", fontsize=14, fontweight="bold")
    plt.legend(fontsize=10, loc="upper left")
    plt.grid(True, alpha=0.3)
    plt.tight_layout()
    
    # Sauvegarder
    plt.savefig(save_path, dpi=300, bbox_inches="tight")
    print(f"✅ Graphique sauvegardé: {save_path}")
    
    plt.show()


def plot_summary_stats(history, save_path="results/stats.png"):
    """Créer un résumé avec sous-graphiques."""
    fig, axes = plt.subplots(2, 2, figsize=(14, 10))
    
    # (1) Évolution du score moyen
    axes[0, 0].plot(history["generation"], history["avg_fitness"], "b-o")
    axes[0, 0].set_title("Score Moyen par Génération")
    axes[0, 0].set_xlabel("Génération")
    axes[0, 0].set_ylabel("Score")
    axes[0, 0].grid(True, alpha=0.3)
    
    # (2) Distribution des meilleurs scores
    axes[0, 1].plot(history["generation"], history["best_fitness"], "g-s")
    axes[0, 1].set_title("Meilleur Score par Génération")
    axes[0, 1].set_xlabel("Génération")
    axes[0, 1].set_ylabel("Score")
    axes[0, 1].grid(True, alpha=0.3)
    
    # (3) Écart type / Diversité
    std_per_gen = []
    for i in range(len(history["generation"])):
        avg = history["avg_fitness"][i]
        best = history["best_fitness"][i]
        worst = history["worst_fitness"][i]
        diversity = best - worst
        std_per_gen.append(diversity)
    
    axes[1, 0].bar(history["generation"], std_per_gen, alpha=0.7, color="orange")
    axes[1, 0].set_title("Diversité de Population (Best - Worst)")
    axes[1, 0].set_xlabel("Génération")
    axes[1, 0].set_ylabel("Diversité")
    axes[1, 0].grid(True, alpha=0.3, axis="y")
    
    # (4) Statistiques finales (texte)
    axes[1, 1].axis("off")
    stats_text = f"""
    STATISTIQUES FINALES
    
    Nombre de générations: {len(history["generation"])}
    
    Score moyen initial: {history["avg_fitness"][0]:.2f}
    Score moyen final: {history["avg_fitness"][-1]:.2f}
    Amélioration: {history["avg_fitness"][-1] - history["avg_fitness"][0]:+.2f}
    
    Meilleur score global: {max(history["best_fitness"]):.2f}
    Pire score global: {min(history["worst_fitness"]):.2f}
    
    Progression moyenne par génération:
    {(history["avg_fitness"][-1] - history["avg_fitness"][0]) / len(history["generation"]):.4f}
    """
    
    axes[1, 1].text(0.1, 0.5, stats_text, fontsize=11, verticalalignment="center",
                     family="monospace",
                     bbox=dict(boxstyle="round", facecolor="lightblue", alpha=0.5))
    
    plt.tight_layout()
    plt.savefig(save_path, dpi=300, bbox_inches="tight")
    print(f"✅ Statistiques sauvegardées: {save_path}")
    plt.show()
```

## 6.2 Remplir `src/utils/logger.py`

```python
import csv
from datetime import datetime

def log_generation(gen_num, population_stats, verbose=True):
    """
    Afficher une ligne de log pour la génération.
    
    Args:
        gen_num: numéro de génération
        population_stats: dict avec avg_fitness, best_fitness, etc.
        verbose: afficher ou non
    """
    if not verbose:
        return
    
    avg = population_stats.get("avg_fitness", 0)
    best = population_stats.get("best_fitness", 0)
    worst = population_stats.get("worst_fitness", 0)
    
    print(f"Gen {gen_num:3d} | Avg: {avg:6.2f} | Best: {best:6.2f} | Worst: {worst:6.2f}")


def save_stats_to_csv(history, filename="results/history.csv"):
    """Sauvegarder l'historique en CSV."""
    with open(filename, "w", newline="") as f:
        writer = csv.writer(f)
        
        # Header
        writer.writerow(["Generation", "Avg Fitness", "Best Fitness", "Worst Fitness"])
        
        # Données
        for i in range(len(history["generation"])):
            writer.writerow([
                history["generation"][i],
                history["avg_fitness"][i],
                history["best_fitness"][i],
                history["worst_fitness"][i],
            ])
    
    print(f"✅ Historique sauvegardé: {filename}")


def log_final_summary(ga, best_genome):
    """Afficher un résumé final."""
    print("\n" + "="*60)
    print("RÉSUMÉ FINAL - ALGORITHME GÉNÉTIQUE")
    print("="*60)
    print(f"Générations exécutées: {ga.generation + 1}")
    print(f"Taille population: {ga.population_size}")
    print(f"Taille génome: {ga.gene_size}")
    print(f"\nMeilleur fitness obtenu: {best_genome.fitness:.2f}")
    print(f"Fitness moyen initial: {ga.history['avg_fitness'][0]:.2f}")
    print(f"Fitness moyen final: {ga.history['avg_fitness'][-1]:.2f}")
    print("="*60 + "\n")
```

---

# ÉTAPE 7️⃣ : CRÉER LE POINT D'ENTRÉE (30 min)

## 7.1 Remplir `main.py`

```python
#!/usr/bin/env python3
"""
Point d'entrée du projet: Jeu de Mémoire Évolutif avec GA
"""

import os
import sys
from pathlib import Path

# Ajouter src au path
sys.path.insert(0, str(Path(__file__).parent / "src"))

# Imports
from game.memory_game import MemoryGame
from game.config import GAME_CONFIG
from ga.genetic_algorithm import GeneticAlgorithm
from utils.visualization import plot_fitness_evolution, plot_summary_stats
from utils.logger import log_final_summary, save_stats_to_csv

def main():
    """Point d'entrée principal."""
    
    print("\n🧠 JEU DE MÉMOIRE ÉVOLUTIF - Algorithme Génétique")
    print("="*60 + "\n")
    
    # Configuration
    POPULATION_SIZE = 50
    GENE_SIZE = 100
    NUM_GENERATIONS = 100
    
    print(f"📋 Configuration:")
    print(f"   Population: {POPULATION_SIZE} agents")
    print(f"   Génome: {GENE_SIZE} gènes")
    print(f"   Générations: {NUM_GENERATIONS}")
    print(f"   Jeu: Séquence max {GAME_CONFIG['max_sequence_length']} couleurs")
    print()
    
    # Créer dossier résultats
    os.makedirs("results", exist_ok=True)
    
    # 1. Initialiser le jeu
    print("🎮 Initialisation du jeu...")
    game = MemoryGame()
    
    # 2. Créer GA
    print("🧬 Initialisation de l'algorithme génétique...")
    ga = GeneticAlgorithm(
        population_size=POPULATION_SIZE,
        gene_size=GENE_SIZE,
        game=game,
    )
    
    # 3. Exécuter GA
    print(f"▶️  Exécution de {NUM_GENERATIONS} générations...\n")
    ga.run(num_generations=NUM_GENERATIONS, verbose=True)
    
    # 4. Récupérer résultats
    print("\n📊 Traitement des résultats...")
    best_genome = ga.get_best_genome()
    history = ga.get_history()
    
    # 5. Sauvegarder stats
    save_stats_to_csv(history, "results/history.csv")
    
    # 6. Créer graphiques
    print("\n📈 Création des graphiques...")
    plot_fitness_evolution(history, "results/evolution.png")
    plot_summary_stats(history, "results/stats.png")
    
    # 7. Résumé final
    log_final_summary(ga, best_genome)
    
    print("✅ Tout est terminé!")
    print("📁 Résultats sauvegardés dans: ./results/\n")


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n⚠️  Interrompu par l'utilisateur.")
        sys.exit(0)
    except Exception as e:
        print(f"\n❌ Erreur: {e}")
        import traceback
        traceback.print_exc()
        sys.exit(1)
```

---

# ÉTAPE 8️⃣ : TESTER & DÉBOGUER (1h)

## 8.1 Lancer le projet

```bash
python main.py
```

Résultat attendu:
```
🧠 JEU DE MÉMOIRE ÉVOLUTIF - Algorithme Génétique
============================================================

📋 Configuration:
   Population: 50 agents
   Génome: 100 gènes
   Générations: 100
   Jeu: Séquence max 50 couleurs

🎮 Initialisation du jeu...
🧬 Initialisation de l'algorithme génétique...
▶️  Exécution de 100 générations...

Gen   0 | Avg:   1.50 | Best:   2.00
Gen  10 | Avg:   3.20 | Best:   5.00
Gen  20 | Avg:   5.10 | Best:   8.00
...
Gen  90 | Avg:  12.50 | Best:  18.00

📊 Traitement des résultats...
✅ Historique sauvegardé: results/history.csv
✅ Graphique sauvegardé: results/evolution.png
✅ Statistiques sauvegardées: results/stats.png

RÉSUMÉ FINAL
============================================================
...
```

## 8.2 Vérifier les résultats

```bash
ls -la results/
```

Doit contenir:
- `evolution.png` ✅
- `stats.png` ✅
- `history.csv` ✅

## 8.3 Si des erreurs

**Erreur:** `ModuleNotFoundError`
→ Vérifier `sys.path.insert(0, "src")`

**Erreur:** `GAME_CONFIG not found`
→ Vérifier que `config.py` existe et est importé

**Les graphiques ne montent pas**
→ Vérifier que la mutation/crossover marche bien

---

# ÉTAPE 9️⃣ : OPTIMISATION & TUNING (1-2h)

## 9.1 Expérimenter avec les hyperparamètres

```python
# Dans main.py, tester différentes valeurs:

CONFIGS_TO_TEST = [
    {"population_size": 30, "gene_size": 50, "generations": 100},
    {"population_size": 50, "gene_size": 100, "generations": 100},
    {"population_size": 100, "gene_size": 150, "generations": 150},
]
```

## 9.2 Améliorer la stratégie de `genome_decide`

La fonction actuelle est très basique. Essayer:
- Utiliser un mini-réseau de neurones
- Implémenter une mémoire associative
- Ajouter une pondération par position (les couleurs récentes plus importantes)

---

# ÉTAPE 🔟 : DOCUMENTATION FINALE (30 min)

## 10.1 Remplir `docs/GA_THEORY.md`

Expliquer:
- Comment marche la GA
- Pourquoi mutation/crossover
- Résultats théoriques attendus

## 10.2 Remplir README.md

Expliquer comment lancer le projet pour un nouveau venu

---

## 📋 CHECKLIST FINALE

- [ ] Structure créée (`setup_project.sh` exécuté)
- [ ] `src/game/config.py` ✅
- [ ] `src/game/memory_game.py` ✅
- [ ] Tests jeu passent ✅
- [ ] `src/ga/genome.py` ✅
- [ ] `src/ga/fitness.py` ✅
- [ ] `src/ga/genetic_algorithm.py` ✅
- [ ] `src/utils/visualization.py` ✅
- [ ] `src/utils/logger.py` ✅
- [ ] `main.py` ✅
- [ ] Exécution sans erreur ✅
- [ ] Graphiques générés ✅
- [ ] Score augmente génération après génération ✅
- [ ] Documentation complète ✅
- [ ] Repo Git initialisé et pushé ✅

---

## ⏱️ TEMPS ESTIMÉ TOTAL

| Phase | Temps |
|-------|-------|
| Setup | 15 min |
| Jeu | 1-1.5h |
| Génome | 45 min |
| Fitness | 45 min |
| GA | 1h |
| Vizualisation | 30 min |
| Main | 30 min |
| Tests | 1h |
| Optimisation | 1-2h |
| Documentation | 30 min |
| **TOTAL** | **~7-8 heures** |

---

## 🎯 Objectifs de Succès

✅ **Le score moyen augmente d'au moins 50% entre la première et la dernière génération**

✅ **Le meilleur agent atteint au moins 10-15 niveaux**

✅ **Les graphiques montrent une courbe de convergence claire**

✅ **Le code est modulaire, testable et bien documenté**

✅ **Un nouveau développeur peut suivre ce guide et reproduire le projet en 1 journée**

---

**Bonne chance! 🚀**