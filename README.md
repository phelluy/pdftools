# PDF to Markdown Converters

Ce projet propose deux outils pour convertir des fichiers PDF en Markdown avec extraction d'images.

## Installation

### 1. Créer un environnement virtuel avec uv

```bash
uv venv
source .venv/bin/activate
```

Ou directement sans activer :
```bash
uv venv && source .venv/bin/activate
```

### 2. Installer les dépendances

```bash
uv pip install -r requirements.txt
```

**Dépendances :**
- `requests` - Pour les appels API
- `docling` - Convertisseur PDF avec VLM (Granite)
- `pillow` - Traitement d'images
- `huggingface-hub` - Pour télécharger les modèles
- `mlx` - Inférence sur Apple Silicon (recommandé pour Mac)
- `mlx-vlm` - Modèles VLM pour MLX

## Outils disponibles

### 1. Docling PDF to Markdown (`docling-pdf2md.py`)

Convertit un PDF en Markdown en utilisant le modèle Granite VLX de IBM via Docling. **Recommandé pour les visualisations complexes et les images.**

#### Utilisation

```bash
python docling-pdf2md.py <chemin_pdf> -o <dossier_sortie>
```

#### Exemples

```bash
# Utilisation basique
python docling-pdf2md.py document.pdf -o output

# Avec timeout personnalisé (en secondes)
python docling-pdf2md.py document.pdf -o output --timeout-seconds 300

# Forcer l'utilisation de Transformers au lieu de MLX
python docling-pdf2md.py document.pdf -o output --force-transformers

# Logs verbeux pour le téléchargement des modèles
python docling-pdf2md.py document.pdf -o output --verbose-download
```

#### Résultat

Le dossier de sortie contient :
- `<nom>.md` - Markdown converti avec images locales
- `<nom>.json` - Structure Docling complète en JSON
- `images/` - Images extraites du PDF

#### Exemple : `test/docling_output/`

```
docling_output/
├── sample.md          # Markdown avec références d'images
├── sample.json        # Données structurées
└── images/
    └── image_001.png  # Image extraite
```

---

### 2. Mistral OCR to Markdown (`mistral-pdf2md.py`)

Convertit un PDF en Markdown en utilisant l'API Mistral OCR. **Robuste pour les PDFs scannés et les contenus OCR.**

#### Configuration

Avant de lancer le script, définir la clé API Mistral :

```bash
export MISTRAL_API_KEY="votre-clé-api"
```

#### Utilisation

```bash
python mistral-pdf2md.py <dossier>
```

Parcourt récursivement le dossier en cherchant tous les fichiers `.pdf` et crée un `.md` correspondant.

#### Exemple

```bash
# Convertir tous les PDFs du dossier 'documents'
python mistral-pdf2md.py documents

# Convertir les PDFs du dossier courant
python mistral-pdf2md.py .
```

#### Résultat

Pour chaque PDF, génère :
- `<nom>.md` - Markdown extrait
- `sample_images/` - Images extraites (si présentes)

---

## Test rapide

Une demo est disponible dans le dossier `test/` :

```bash
# Le PDF de test est déjà généré
ls test/sample.pdf

# Teste Docling
python docling-pdf2md.py test/sample.pdf -o test/docling_output

# Teste Mistral (nécessite MISTRAL_API_KEY)
python mistral-pdf2md.py test
```

---

## Dépannage

### ⚠️ Warning : `mx.metal.device_info is deprecated`

C'est un avertissement interne de MLX, pas d'erreur. Le script fonctionne correctement.

### ⚠️ Erreur : `Could not import Docling classes`

Docling n'est pas installé. Réinstaller :
```bash
uv pip install docling --force-reinstall
```

### ⚠️ Erreur : `No API key found for Mistral`

Configurer la clé API avant de lancer :
```bash
export MISTRAL_API_KEY="la-clé"
```

---

## Comparaison des deux outils

| Feature | Docling | Mistral |
|---------|---------|---------|
| Extraction images | ✅ Oui | ✅ Oui |
| OCR | ✅ Oui (VLM) | ✅ Oui |
| PDFs scannés | ✅ Bien | ✅ Excellent |
| Coût | ✅ Gratuit | ⚠️ API payante |
| Installation | ⚠️ Lourde | ✅ Légère |
| Vitesse | ✅ Rapide | ⚠️ Nécessite appels réseau |
| Apple Silicon | ✅ MLX natif | ℹ️ Réseau |

---

## Docs

- [Docling Documentation](https://github.com/DS4SD/docling)
- [Mistral API Documentation](https://docs.mistral.ai/)
