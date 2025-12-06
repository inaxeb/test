# Mini crm golang
Gestionnaire de contacts via le cmd

Le projet contient l'utilisation de:

- Architecture découplée via interfaces
- Gestion des dépendances via Go Modules
- CLI professionnelle via **Cobra**
- Configuration externe via **Viper**
- Persistance via JSON ou SQLite/GORM
- Stockage en mémoire (mode test)

# Fonctionnalités principales

- Ajouter un contact
- Lister les contacts
- Supprimer un contact
- Modifier un contact
- Stockage interchangeable :
    - **In-memory**
    - **JSON**
    - **SQLite avec GORM**
- Configuration externe via `config.yaml`
- CLI structurée avec Cobra (`add`, `list`, `update`, `delete`)

## 1 - Programme initial (en mémoire)
Base du CRM utilisant uniquement une `map[int]Contact`.

## 2 – Structs & Interfaces
- Création du modèle `Contact`
- Création de l’interface `Storer`
- Passage à une architecture extensible et modulaire

## 3 – CLI & JSON

### 3.1 – CLI avec Cobra
- Commande principale `crm`
- Sous-commandes :
    - `crm add`
    - `crm list`
    - `crm update`
    - `crm delete`
- Mode interactif + mode flags

### 3.2 – Stockage JSON
- Implémentation du `JSONStore`
- Persistance dans `contacts.json`
- Lecture/écriture automatique

## 4 – Nouveau Storer : GORM + SQLite
(Étape réalisée ici)

- Ajout des dépendances GORM
- Mise à jour de `Contact` pour GORM (tags)
- Création de `GORMStore`
- Implémentation complète :
    - Ajouter
    - Lister
    - Supprimer
    - Modifier
    - Gestion auto des IDs
- Auto-migration de la base via GORM
- Production d’un fichier SQLite `crm.db`

## 5 – Configuration avec Viper
- Chargement de `config.yaml`
- Sélection dynamique du storer :
    - `"memory"`
    - `"json"`
    - `"gorm"`

---


# Exécution du projet

Dans le dossier racine du projet :

```bash
go run ./cmd/crm
```

---

## Les commandes:

## Ajouter un contact

```bash
go run ./cmd/crm add
```

Avec flags :

```bash
go run ./cmd/crm add --nom Kevin --email kevin@test.com
```

---

## Lister les contacts

```bash
go run ./cmd/crm list
```

---

## Modifier un contact

```bash
go run ./cmd/crm update --id 1 --nom Mathis
```

---

## Supprimer un contact

```bash
go run ./cmd/crm delete --id 1
```

---

# Visualiser la base SQLite (GORM)

Lorsque le storer est configuré en `gorm`, la base est créée automatiquement.

## Voir son contenu avec SQLiteStudio :

1. Installer SQLiteStudio
2. Fichier → Ajouter une base
3. Sélectionner `crm.db`
4. Ouvrir la table `contacts`
5. Exécuter :

```sql
SELECT * FROM contacts;
```

---