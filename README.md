# 🎛️ Jour 8 / 10 — Dashboard Interactif Excel

> **Série : 10 Days of Excel** · Jour 8/10  
> Concepts : Validation de données · Contrôles formulaire · SOMME.SI.ENS dynamique · Graphique lié

---

## 📁 Fichiers du projet

```
Jour8-DashboardInteractif/
│
├── ventes_dashboard.csv          ← 30 ventes · 6 mois · 5 vendeurs · 2 trimestres
└── dashboard_interactif_jour8.xlsx
    ├── DONNÉES                    ← Source : 30 lignes, 8 colonnes
    ├── DASHBOARD                  ← KPIs + tableau + graphique filtrés
    └── GUIDE CONTRÔLES            ← 5 étapes pour rendre le dashboard interactif
```

---

## 🔑 Comment ça fonctionne

L'idée est simple : **deux cellules de filtre** (`C5` = Vendeur, `E5` = Trimestre) pilotent tout le dashboard via `SOMME.SI.ENS`.

Quand tu changes `C5` de `Alice` à `Karim` → toutes les formules recalculent instantanément.

---

## 🔑 La formule centrale

```excel
=SOMME.SI.ENS(
    DONNÉES!H4:H33,      ← plage des montants
    DONNÉES!D4:D33, $C$5, ← critère 1 : Vendeur = cellule filtre
    DONNÉES!B4:B33, $E$5  ← critère 2 : Trimestre = cellule filtre
)
```
**Pourquoi `$C$5` avec les `$` ?**  
Le `$` fige la référence — quand on copie la formule sur 6 lignes (un par mois), elle continue de pointer sur C5 et pas sur C6, C7...

---

## 🎛️ Rendre le dashboard interactif

### Option 1 — Liste déroulante (la plus simple)
```
Cliquer sur C5 → Données → Validation des données
Autoriser : Liste
Source : Alice,Karim,Lucie,Thomas,Nadia
```
Une flèche apparaît → cliquer change la valeur → tout se recalcule.

### Option 2 — Liste depuis une plage
```
Source : =$J$1:$J$5   (plage contenant les noms)
```
Plus flexible : ajouter un vendeur dans la plage met la liste à jour.

### Option 3 — Case à cocher (Contrôle formulaire)
```
Onglet Développeur → Insérer → Case à cocher
Clic droit → Format de contrôle → Cellule liée : $K$1
```
Cochée → K1 = VRAI → utiliser dans une formule `=SI($K$1, ..., ...)`

### Option 4 — Barre de défilement
```
Développeur → Insérer → Barre de défilement
Cellule liée : $L$1  |  Min : 1  |  Max : 6
```
Glisser → L1 change → `=INDEX({"Janv";"Févr";"Mars";"Avr";"Mai";"Juin"}, L1)` renvoie le mois.

---

## 💡 Activer l'onglet Développeur

```
Fichier → Options → Personnaliser le ruban
Cocher "Développeur" → OK
```

---

## 📐 Structure des formules du dashboard

| Formule | Rôle |
|---------|------|
| `SOMME.SI.ENS(..., $C$5, $E$5)` | CA filtré par vendeur ET trimestre |
| `SI(D9>0, C9/D9, 0)` | Pourcentage d'atteinte (évite la division par zéro) |
| `SI(C13>=D13,"✓ Atteint","✗ En dessous")` | Statut texte par mois |
| `NB.SI.ENS(..., $C$5, $E$5)` | Nombre de ventes filtrées |

---

## 💡 Astuces pro

- **Nommer les cellules** : CTRL+F3 → nommer C5 "Vendeur" → les formules lisent `=SOMME.SI.ENS(..., Vendeur, ...)` — bien plus lisible
- **Protéger le dashboard** : Révision → Protéger la feuille → décocher uniquement les cellules de filtre
- **Graphique dynamique** : le graphique lié au tableau se met à jour automatiquement avec les filtres
