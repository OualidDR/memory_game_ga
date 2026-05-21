# 🚀 GUIDE DE DÉMARRAGE RAPIDE

## Qu'est-ce que ce projet?

**"Jeu de Mémoire Évolutif avec Algorithme Génétique"**

- 🎮 Un jeu où l'agent reproduit des séquences de couleurs de plus en plus longues
- 🧬 Un algorithme génétique qui entraîne une population d'agents à devenir meilleur
- 📊 Des graphiques montrant comment les agents s'améliorent génération après génération

---

## ⚡ 5 Minutes de Démarrage

### Étape 1: Créer la structure du projet
```bash
bash setup_project.sh
cd memory_game_ga
```

### Étape 2: Installer les dépendances
```bash
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### Étape 3: Vérifier que tout est en place
```bash
ls src/
# Doit afficher: game, ga, utils, __init__.py
```

---

## 📚 Architecture Simple

```
main.py (orchestration)
    ↓
    ├→ GAME (src/game/)
    │   ├── config.py        ← Paramètres du jeu
    │   └── memory_game.py   ← Classe MemoryGame
    │
    ├→ GA (src/ga/)
    │   ├── genome.py        ← Classe Genome (agent)
    │   ├── fitness.py       ← Évaluation des agents
    │   └── genetic_algorithm.py ← Boucle GA
    │
    └→ UTILS (src/utils/)
        ├── visualization.py ← Graphiques
        └── logger.py        ← Logs & statistiques
```

---

## 📋 Phases de Développement

### ✅ Phase 1: GAME (1-1.5h)
- Implémenter `config.py` et `memory_game.py`
- Tester le jeu → **Les tests doivent passer**
- Objectif: Un agent peut jouer une partie

### ✅ Phase 2: GÉNOME (45 min)
- Implémenter `genome.py`
- Tester mutations et crossover
- Objectif: Les genomes peuvent se reproduire et muter

### ✅ Phase 3: FITNESS (45 min)
- Implémenter `fitness.py`
- Tester évaluation d'un agent
- Objectif: On peut scorer un agent

### ✅ Phase 4: GA (1h)
- Implémenter `genetic_algorithm.py`
- Tester la boucle complète
- Objectif: Population évolue, fitness augmente

### ✅ Phase 5: VISUALISATION (30 min)
- Implémenter `visualization.py`
- Créer graphiques
- Objectif: Un PNG avec courbe d'évolution

### ✅ Phase 6: MAIN (30 min)
- Remplir `main.py`
- Exécuter le projet entièrement
- Objectif: `python main.py` génère les résultats

---

## 🎯 Checklist de Succès

Après chaque phase, vérifier:

**Phase 1 - GAME:**
- [ ] Jeu initialise correctement
- [ ] `game.play([])` ajoute une couleur
- [ ] Bonne réponse → score augmente
- [ ] Mauvaise réponse → jeu terminé
- [ ] Tests passent: `pytest tests/test_game.py -v`

**Phase 2 - GÉNOME:**
- [ ] Genome crée 100+ gènes
- [ ] Mutation change les gènes
- [ ] Crossover crée enfant valide
- [ ] Tests passent: `pytest tests/test_genome.py -v`

**Phase 3 - FITNESS:**
- [ ] `evaluate_genome()` retourne un score
- [ ] Score fait sens (nombre de niveaux)
- [ ] Peut tester un agent plusieurs fois

**Phase 4 - GA:**
- [ ] Population crée avec N genomes
- [ ] `evaluate_population()` évalue tous
- [ ] `selection()` garde les meilleurs
- [ ] `reproduce()` crée génération suivante
- [ ] `run(10)` exécute 10 générations
- [ ] Fitness moyen augmente

**Phase 5 - VISUALISATION:**
- [ ] `plot_fitness_evolution()` crée PNG
- [ ] Graphique montre courbe montante
- [ ] PNG sauvegardé dans `results/`

**Phase 6 - MAIN:**
- [ ] `python main.py` s'exécute sans erreur
- [ ] `results/evolution.png` créé
- [ ] Graphique montre évolution claire

---

## 📊 Résultats Attendus

### Avant optimisation (générations 0-20)
```
Score moyen: 2-4 niveaux
```

### Après 100 générations
```
Score moyen: 8-15 niveaux (ou plus!)
Meilleur agent: 20+ niveaux
```

### Graphique
```
Courbe montante en J
Score augmente progressivement
Pas de plateau avant gen 50
```

---

## 🔧 Commandes Utiles

```bash
# Exécuter le projet complet
python main.py

# Lancer les tests
pytest tests/ -v

# Lancer un test spécifique
pytest tests/test_game.py::test_game_initialization -v

# Voir le coverage
pytest tests/ --cov=src

# Nettoyer les résultats
rm -rf results/*
```

---

## ⚠️ Problèmes Courants

### "ModuleNotFoundError: No module named 'src'"
**Solution:** Vérifier que `sys.path.insert(0, "src")` est dans `main.py`

### "Le jeu ne progresse pas au-delà du niveau 3"
**Solution:** Vérifier que `genome_decide()` crée vraiment des séquences

### "Le graphique est vide"
**Solution:** Vérifier que `history` est rempli (pas vide)

### "Les mutations ne fonctionnent pas"
**Solution:** Vérifier que `mutation_rate > 0` et que `mutation_strength > 0`

---

## 🎓 Concepts Clés

| Terme | Explication |
|-------|-------------|
| **Génome** | Un agent = un vecteur de poids (100+ nombres) |
| **Fitness** | Score au jeu (combien de niveaux réussis) |
| **Mutation** | Modifier aléatoirement les poids |
| **Crossover** | Créer enfant à partir de 2 parents |
| **Sélection** | Garder les agents avec meilleur score |
| **Génération** | Une itération de la boucle GA |

---

## 📈 Comment Lire les Résultats

### Graphique: Score moyen vs Générations

```
Score
  │     ╱╱╱╱
  │   ╱╱╱
  │ ╱╱
  │╱
  └─────────────── Générations
```

- **Axe Y:** Score moyen de la population (0-50)
- **Axe X:** Numéro de génération (0-100)
- **Courbe montante:** L'algorithme apprend! ✅
- **Plateau:** Population convergée (pas assez de diversité)

### CSV: `results/history.csv`

```
Generation,Avg Fitness,Best Fitness,Worst Fitness
0,1.50,2.00,1.00
1,2.30,3.50,1.20
...
100,12.50,18.00,8.50
```

- Avg = Score moyen
- Best = Meilleur agent
- Worst = Pire agent

---

## 💡 Conseils pour Optimiser

### 1. Augmenter la population
```python
POPULATION_SIZE = 100  # Au lieu de 50
```
→ Plus d'exploration, mais plus lent

### 2. Augmenter les générations
```python
NUM_GENERATIONS = 200  # Au lieu de 100
```
→ Plus de temps pour convergence

### 3. Ajuster la mutation
```python
child.mutate(mutation_rate=0.2, mutation_strength=0.2)
```
→ Plus agressif = plus d'exploration

### 4. Changer la taille du génome
```python
GENE_SIZE = 200  # Au lieu de 100
```
→ Plus de capacité à apprendre

---

## 📖 Documentation Complète

- **Architecture détaillée:** Lire `docs/ARCHITECTURE.md`
- **Plan pas à pas:** Lire `docs/PLAN_CONSTRUCTION.md` (ce document)
- **Théorie GA:** Lire `docs/GA_THEORY.md` (à remplir)

---

## 🎬 Exemple d'Exécution

```bash
$ python main.py

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
Gen  10 | Avg:   3.40 | Best:   5.50
Gen  20 | Avg:   5.10 | Best:   8.20
Gen  30 | Avg:   6.80 | Best:  10.50
...
Gen  90 | Avg:  12.40 | Best:  18.00
Gen  99 | Avg:  12.80 | Best:  19.50

📊 Traitement des résultats...
✅ Historique sauvegardé: results/history.csv
✅ Graphique sauvegardé: results/evolution.png
✅ Statistiques sauvegardées: results/stats.png

============================================================
RÉSUMÉ FINAL - ALGORITHME GÉNÉTIQUE
============================================================
Générations exécutées: 100
Taille population: 50
Taille génome: 100

Meilleur fitness obtenu: 19.50
Fitness moyen initial: 1.50
Fitness moyen final: 12.80
============================================================

✅ Tout est terminé!
📁 Résultats sauvegardés dans: ./results/
```

---

## 🚀 Prochaines Étapes Après le MVP

1. **Visualiser le jeu**
   - Afficher graphiquement la séquence
   - Animer la réponse de l'agent

2. **Analyser la diversité**
   - Tracer la diversité génétique au fil du temps
   - Détecter la convergence prématurée

3. **Comparer stratégies**
   - Tester différents `mutation_rate`
   - Tester différentes `selection_pressure`

4. **Intégrer Gymnasium**
   - Convertir en environnement Gym
   - Tester avec d'autres algorithmes (RL, CMA-ES, etc.)

5. **Visualisation interactive**
   - Web dashboard avec graphiques temps réel
   - Boutons pour pause/resume/reset

---

## 📞 Besoin d'Aide?

1. **Relire `ARCHITECTURE.md`** → explique la structure
2. **Relire `PLAN_CONSTRUCTION.md`** → explique étape par étape
3. **Vérifier les tests** → `pytest tests/ -v`
4. **Print de debug** → ajouter `print()` dans le code
5. **Git history** → `git log` pour voir ce qui a changé

---

## ✨ Objectif Final

Après avoir complété ce projet, vous aurez:

✅ **Compris comment marche la GA**
✅ **Implémenté un projet ML complet**
✅ **Créé des visualisations significatives**
✅ **Écrit du code testé et modulaire**
✅ **Expérimenté avec hyperparamètres**

---

**Bon courage! 🎉**