# QRadtran — Guide d'installation et d'utilisation

**Version** 0.3.0　　**Date** 2026-09-19　　**Éditeur** Wuhan WaveFront Co., Ltd.

Ce guide existe aussi en 简体中文, English, Русский, 日本語, 한국어, Deutsch et Español ; après l'installation, toutes les versions se trouvent dans le dossier `doc\` du répertoire d'installation.

QRadtran est un logiciel de calcul de transmittance et de luminance atmosphériques destiné aux ingénieurs en systèmes infrarouges et optroniques. Sous Windows, il calcule via une interface graphique la transmittance spectrale sur trajets obliques et horizontaux, la luminance du trajet et du ciel ainsi que la luminance apparente d'une cible. Il permet la comparaison de plusieurs courbes, des balayages de paramètres produisant des tables de correspondance (CSV / HDF5), un outil en ligne de commande et une interface Python. Le noyau de calcul est le logiciel libre libRadtran, installé avec le programme ; aucune configuration supplémentaire n'est nécessaire.

---

## Sommaire

1. [Configuration requise](#1-configuration-requise)
2. [Installation](#2-installation)
3. [Premier démarrage](#3-premier-démarrage)
4. [Vue d'ensemble de l'interface](#4-vue-densemble-de-linterface)
5. [Panneau de paramètres en détail](#5-panneau-de-paramètres-en-détail)
6. [Procédures types](#6-procédures-types)
7. [Balayage de paramètres et tables](#7-balayage-de-paramètres-et-tables)
8. [Outil en ligne de commande](#8-outil-en-ligne-de-commande)
9. [Interface Python](#9-interface-python)
10. [Paramètres](#10-paramètres)
11. [Méthode de calcul et précision](#11-méthode-de-calcul-et-précision)
12. [Questions fréquentes](#12-questions-fréquentes)
13. [Désinstallation](#13-désinstallation)
14. [Fichiers et dossiers](#14-fichiers-et-dossiers)

---

## 1. Configuration requise

| Élément | Exigence |
|---|---|
| Système d'exploitation | Windows 10 / 11, 64 bits |
| Processeur | 4 cœurs ou plus. Le noyau exécute plusieurs processus en parallèle ; plus il y a de cœurs, plus les calculs par lots sont rapides |
| Mémoire | 8 Go ou plus |
| Disque | Environ 1,2 Go après installation (tables spectrales des trois résolutions incluses). SSD recommandé : les calculs en résolution fine lisent beaucoup de données |
| Écran | 1600 × 900 ou plus |
| Autre | Ni Python, ni MSYS2, ni WSL requis ; le runtime VC++ est inclus |

L'interface Python est facultative et nécessite un Python 3.10 64 bits avec numpy installé sur la machine.

---

## 2. Installation

1. Double-cliquez sur `QRadtran-0.3.0-Setup-x64.exe`.
2. Choisissez la langue de l'installateur (简体中文, English, Русский, 日本語, 한국어, Français, Deutsch, Español). L'installateur enregistre la langue choisie comme langue de l'interface du logiciel ; elle peut être modifiée ensuite dans Outils → Paramètres.
3. Lisez les informations sur les licences tierces et cliquez sur Suivant.
4. Choisissez le dossier d'installation, par défaut `C:\Program Files\QRadtran`. Le chemin peut contenir des caractères accentués, mais un lecteur réseau est déconseillé.
5. Tâches supplémentaires :
   - **Créer un raccourci sur le bureau** : coché par défaut.
   - **Ajouter le dossier d'installation au PATH** : décoché par défaut. Une fois coché, `qradtran-cli` peut être tapé dans n'importe quelle invite de commandes.
6. Cliquez sur Installer et attendez la copie des fichiers (1 à 3 minutes).
7. La dernière page permet de lancer le logiciel ou d'ouvrir ce guide.

L'installateur requiert les droits d'administrateur. Pour installer uniquement pour l'utilisateur courant, choisissez « Installer pour moi seulement » dans l'invite de privilèges ; le dossier par défaut devient alors `%LOCALAPPDATA%\Programs\QRadtran`.

### Utilisation portable

L'installateur est facultatif : copiez le dossier de distribution complet `QRadtran\` où vous voulez et lancez `QRadtran.exe` depuis ce dossier. Toutes les dépendances y sont ; supprimer le dossier suffit à désinstaller.

---

## 3. Premier démarrage

Après le démarrage, regardez la barre d'état en bas à droite de la fenêtre :

```
Noyau libradtran 2.0.6 | 7 processus
```

Cette ligne indique que le noyau de calcul est prêt ; « processus » est le nombre de processus du noyau exécutés simultanément. Si « Noyau indisponible » s'affiche, ouvrez Outils → Paramètres et vérifiez le chemin du noyau, voir section 10.

**Langue de l'interface** : par défaut le logiciel suit la langue d'affichage de Windows et prend en charge le chinois simplifié, l'anglais, le russe, le japonais, le coréen, le français, l'allemand et l'espagnol. Choisissez une langue dans Outils → Paramètres → Langue de l'interface et redémarrez ; la langue peut aussi être imposée en ligne de commande, par exemple `QRadtran.exe --lang fr`.

Pour le premier essai, partez d'un scénario prédéfini :

1. Menu Fichier → Charger un scénario prédéfini…, choisissez `mls_ground_to_air_3_5um.json` (atmosphère latitudes moyennes été, observation depuis le sol d'une cible à 10 km d'altitude, 3–5 µm).
2. Cliquez sur Exécuter dans la barre d'outils ou appuyez sur F5.
3. Une tâche apparaît dans le panneau Tâches et journal en bas ; lorsque la barre de progression est terminée, les courbes s'affichent au centre : bleu pour la transmittance (axe gauche), couleurs chaudes pour les grandeurs radiométriques (axe droit). L'ensemble prend environ 7 secondes.

---

## 4. Vue d'ensemble de l'interface

```
┌─────────────────────────────────────────────────────────────────────┐
│ Fichier  Calcul  Affichage  Outils  Aide                            │
│ [Nouveau][Ouvrir][Enregistrer] │ [▶Exécuter][■Arrêter][Balayage] │ [Exporter][Zoom] │ Axe X  Densité │
├──────────────┬───────────────────────────────────────┬──────────────┤
│  Paramètres  │  Spectres / Table des résultats       │  Courbes     │
│  ▸ Atmosphère│                                       │  ☑ Transmitt.│
│  ▸ Géométrie │        (graphique à deux axes)        │  ☑ Luminance │
│  ▸ Aérosol   │                                       │  couleur     │
│  ▸ Sol       │                                       │              │
│  ▸ Spectral  │                                       │              │
│  ▸ Sorties   │                                       │              │
│  [Calculer]  │  x = 4,35 µm  Transmittance 0,812 …   │              │
├──────────────┴───────────────────────────────────────┴──────────────┤
│  Tâches et journal : liste (état / progression / durée) │ journal   │
├─────────────────────────────────────────────────────────────────────┤
│ Barre d'état : lecture du curseur               Noyau libradtran 2.0.6 │
└─────────────────────────────────────────────────────────────────────┘
```

Les trois panneaux ancrables peuvent être déplacés, fermés et réorganisés ; un panneau fermé se restaure via le menu Affichage ou en tirant le bord.

### Barre d'outils

| Bouton | Fonction |
|---|---|
| Nouveau / Ouvrir / Enregistrer le projet | Un projet est un fichier JSON contenant tous les paramètres et les résultats calculés |
| Charger un scénario prédéfini | Trois scénarios d'exemple fournis avec le logiciel ; leurs noms suivent la langue de l'interface |
| Exécuter (F5) | Calcule avec les paramètres courants ; le résultat est ajouté au graphique sans remplacer les précédents |
| Tout arrêter | Annule toutes les tâches en cours |
| Balayage de paramètres / table | Ouvre la boîte de dialogue de calcul par lots, voir section 7 |
| Exporter l'image | Enregistre le graphique courant en PNG / PDF / JPEG |
| Réinitialiser le zoom (Début) | Ramène le graphique à la plage complète |
| Axe X | Unité de l'axe horizontal : µm / nm / cm⁻¹ ; effet immédiat |
| Densité spectrale | Unité de densité spectrale des grandeurs radiométriques : par µm / par nm / par cm⁻¹ |

### Manipulation du graphique

| Action | Effet |
|---|---|
| Molette | Zoom centré sur le curseur |
| Glisser | Déplacement |
| Double-clic | Réinitialisation |
| Déplacer la souris | La ligne du bas et la barre d'état affichent la valeur de chaque courbe sous le curseur |
| Case à cocher du panneau Courbes | Afficher / masquer une courbe |
| Double-clic sur la pastille de couleur | Changer la couleur de la courbe |
| Clic droit sur un nom de résultat | Exporter ce résultat en CSV ou le supprimer |

La transmittance est tracée sur l'axe gauche (0 à 1) ; luminance et éclairement sur l'axe droit, ce qui permet de les superposer.

---

## 5. Panneau de paramètres en détail

### 5.1 Atmosphère

| Élément | Description |
|---|---|
| Atmosphère standard | Tropicale, latitudes moyennes été, latitudes moyennes hiver, subarctique été, subarctique hiver, standard US 1976 (les six atmosphères AFGL), ou profil personnalisé |
| Importer un profil (CSV) | Importe votre propre sondage pression/température/humidité, voir ci-dessous |
| Colonne de vapeur d'eau | Si coché, l'eau précipitable totale est ramenée aux millimètres indiqués ; sinon la valeur de l'atmosphère standard est utilisée |
| Colonne d'ozone | Idem, en DU |
| Rapport de mélange CO₂ | ppm, 420 par défaut |

**Format du fichier de profil personnalisé** : CSV ou texte séparé par des espaces avec une ligne d'en-tête. Colonnes obligatoires : altitude, pression, température et une colonne d'humidité. Noms de colonnes reconnus (insensibles à la casse) :

| Grandeur | Noms de colonne | Unité |
|---|---|---|
| Altitude | z, alt, altitude, height | km (`z_m` pour des mètres) |
| Pression | p, pres, pressure | hPa (`p_pa` pour des Pa) |
| Température | t, temp, temperature | K (`t_c` pour des °C) |
| Humidité (au choix) | rh ; q ; h2o / h2o_vmr | humidité relative % ; humidité spécifique g/kg ; rapport de mélange volumique ppmv |
| Ozone (facultatif) | o3, o3_vmr | ppmv |

Exemple :

```
z_km,p_hpa,t_k,rh
0.0,1013.0,295.0,70
1.0,900.0,289.0,65
2.0,795.0,283.0,55
5.0,540.0,262.0,40
10.0,265.0,223.0,20
```

Au-dessus du profil importé, les niveaux sont complétés par l'atmosphère standard sélectionnée.

### 5.2 Géométrie

| Type de trajet | Signification | Paramètres |
|---|---|---|
| Trajet oblique (observateur → cible) | Observateur et cible à des altitudes différentes | Altitude de l'observateur, altitude de la cible, angle zénithal de visée |
| Trajet horizontal | Observateur et cible à la même altitude | Altitude de l'observateur, longueur du trajet horizontal |
| Colonne entière, verticale | Du sol au sommet de l'atmosphère | Aucun |
| Luminance du ciel seule | Sans cible ; luminance du ciel dans une direction donnée | Altitude de l'observateur, angle zénithal, azimut |

Conventions angulaires :

- **Angle zénithal de visée** : 0° vers le haut, 90° horizontal, 180° vers le bas. Il doit être inférieur à 90° si la cible est au-dessus de l'observateur et supérieur à 90° sinon ; le panneau signale une erreur dans le cas contraire.
- **Azimut de visée**, **azimut solaire** : 0° au nord, sens horaire.
- **Angle zénithal solaire** : n'agit que sur les grandeurs radiométriques de la bande solaire, pas sur la transmittance.
- **Altitude du sol** : altitude de la base du profil atmosphérique ; les altitudes de l'observateur et de la cible sont au-dessus du niveau de la mer.

Pour des angles zénithaux entre 85° et 95°, le logiciel passe à une intégration de l'extinction par couches pour les trajets quasi horizontaux et émet un avertissement de précision.

### 5.3 Aérosol et nuage

| Élément | Description |
|---|---|
| Type d'aérosol | Aucun, rural, urbain, maritime, troposphérique (modèles de Shettle) ou mélange OPAC |
| Visibilité | Visibilité horizontale au sol en km ; fixe la charge en aérosol de la couche limite |
| Saison | Profil printemps/été ou automne/hiver |
| Aérosol stratosphérique | Fond / volcanique modéré / volcanique élevé / volcanique extrême |
| Nom du mélange OPAC | Utilisé avec OPAC, p. ex. continental_average, maritime_clean, urban, desert |
| Couche nuageuse unique | Nuage d'eau ou de glace avec base, sommet, contenu en eau et rayon effectif |

### 5.4 Sol

| Élément | Description |
|---|---|
| Albédo du sol | Bande solaire, 0 à 1 |
| Température du sol | Température d'émission du sol dans l'infrarouge thermique |
| Émissivité du sol | Infrarouge thermique, 0 à 1 |

### 5.5 Spectral

| Élément | Description |
|---|---|
| Unité spectrale | µm / nm / cm⁻¹. Les bornes sont converties lors du changement d'unité |
| De / à | Plage spectrale. Le logiciel confie séparément au noyau la bande solaire (≤ 5 µm) et la bande thermique (≥ 2,5 µm) puis raccorde les résultats |
| Pas de sortie | Pas de la grille de sortie ; 0 utilise directement la grille de longueurs d'onde représentatives du noyau |
| Résolution du modèle de bande | Grossier 15 cm⁻¹ / moyen 5 cm⁻¹ / fin 1 cm⁻¹ / LOWTRAN 20 cm⁻¹. Plus fin = plus précis et plus lent |
| Solveur | DISORT (diffusion multiple, par défaut) ou deux flux. Lorsqu'une luminance est demandée, le logiciel bascule automatiquement sur DISORT |
| Flux DISORT | 16 par défaut ; passez à 32 pour les cas fortement diffusants (aérosol dense, nuage) |

Durées indicatives (PC de bureau 4 cœurs) : 3–5 µm sol-air, grossier ≈ 7 s, moyen ≈ 30 s ; 8–12 µm horizontal 1 km ≈ 2 s.

### 5.6 Sorties

| Élément | Signification | Unité |
|---|---|---|
| Transmittance du trajet | Transmittance spectrale de l'observateur à la cible | sans dimension |
| Luminance du trajet | Luminance émise et diffusée dans la ligne de visée par l'atmosphère le long du trajet | W/(m²·sr·cm⁻¹), commutable |
| Luminance du ciel | Luminance du ciel vue de l'observateur le long de la ligne de visée | idem |
| Éclairement direct / diffus descendant / ascendant | Éclairements à l'altitude de l'observateur | W/(m²·cm⁻¹) |
| Luminance apparente de la cible | Pour la température et l'émissivité de cible données, luminance vue par l'observateur après atténuation atmosphérique plus luminance du trajet ; la luminance incidente à la cible est également produite | W/(m²·sr·cm⁻¹) |

---

## 6. Procédures types

### 6.1 Calcul simple et comparaison

1. Réglez les paramètres et cliquez sur Exécuter.
2. Modifiez un ou deux paramètres (par exemple la visibilité de 23 km à 5 km) et relancez. Le nouveau résultat s'ajoute au même graphique dans une autre couleur, avec le nom du scénario dans la légende.
3. Cochez ou décochez les courbes à comparer dans le panneau Courbes à droite.
4. Passez à l'onglet Table des résultats pour lire les valeurs ; les unités d'axe et de densité spectrale suivent la barre d'outils.

### 6.2 Enregistrement et restauration

Fichier → Enregistrer le projet stocke les paramètres courants et tous les résultats dans un fichier JSON. À la réouverture, les courbes sont restaurées sans recalcul.

### 6.3 Export

- Graphique : Fichier → Exporter l'image ou le bouton de la barre d'outils, PNG / PDF / JPEG.
- Données : clic droit sur un résultat dans le panneau Courbes → Exporter ce résultat en CSV. Le CSV utilise les unités d'axe et de densité spectrale sélectionnées dans la barre d'outils ; l'en-tête précise les unités.

### 6.4 Scénario de détection de cible

Pour obtenir la luminance apparente d'une cible au détecteur :

1. Choisissez Trajet oblique dans Géométrie, entrez les altitudes de l'observateur et de la cible et l'angle zénithal.
2. Cochez Luminance apparente de la cible dans Sorties et entrez la température et l'émissivité de la cible.
3. L'exécution produit quatre courbes : transmittance, luminance du trajet, luminance apparente de la cible et luminance incidente à la cible.
4. Soustrayez le résultat Luminance du ciel seule obtenu pour la même géométrie de la luminance apparente de la cible pour obtenir le contraste de luminance cible/fond.

---

## 7. Balayage de paramètres et tables

Calcul → Balayage de paramètres / table… part du panneau de paramètres courant, calcule chaque combinaison (produit cartésien) des valeurs choisies et écrit une table de correspondance.

### Éléments de la boîte de dialogue

| Élément | Description |
|---|---|
| Dimensions du balayage | Un paramètre par ligne. Les paramètres s'écrivent en pointeurs JSON ; la liste déroulante propose les plus courants. Les valeurs sont une liste `1, 2, 5, 10` ou une plage `0:10:2` (début:fin:pas) ; les paramètres textuels s'écrivent par leur nom, p. ex. `Rural, Urban` |
| Colonne | Nom de la coordonnée de cette dimension dans le fichier de sortie, par défaut le nom du paramètre |
| Sorties | Si rien n'est coché, toutes les grandeurs du scénario sont écrites |
| Moyennes par bande | Facultatif ; moyenne et intégrale par bande en µm, écrites en complément |
| Fichier de sortie | L'extension détermine le format : `.h5` pour HDF5, `.csv` pour une table longue |
| Reprendre | Ignore les combinaisons déjà calculées lors d'une relance après interruption |

La boîte de dialogue affiche le nombre total de combinaisons et le nombre d'exécutions du noyau par combinaison. Le calcul peut être annulé ; la partie terminée est conservée.

### Structure du fichier HDF5

```
/<Grandeur>                   [dim1, …, dimN, spectral]   données, unité dans l'attribut "units"
/bands/<Grandeur>_average     [dim1, …, dimN, bande]      moyenne par bande
/bands/<Grandeur>_integral    [dim1, …, dimN, bande]      intégrale par bande
/coords/wavenumber            [spectral]                  cm⁻¹
/coords/wavelength_um         [spectral]                  µm
/coords/<colonne>             [valeurs]                   coordonnée de chaque dimension (nombre ou chaîne)
/coords/band                  [bande]                     noms des bandes
/filled                       [dim1, …, dimN]             1 = combinaison calculée
Attributs racine : kernel, kernel_version, created_utc, base_scenario (JSON du scénario de base), plan (JSON du plan de balayage)
```

Lecture en Python (nécessite h5py) :

```python
import h5py
with h5py.File("lut.h5") as f:
    T = f["Transmittance"][...]            # forme [dim1, …, spectral]
    nu = f["coords/wavenumber"][...]
    tgt = f["coords/target_km"][...]
```

`pyqradtran\read_lut_h5.py` dans le dossier d'installation est un script de visualisation prêt à l'emploi.

### Table longue CSV

Une ligne par (valeurs des dimensions…, nombre d'onde, grandeur, valeur) ; les moyennes par bande vont dans `<fichier>.bands.csv`. Convient aux tableaux croisés Excel et à pandas.

---

## 8. Outil en ligne de commande

`qradtran-cli.exe` se trouve dans le dossier d'installation ; avec « Ajouter au PATH » coché, il fonctionne depuis n'importe où.

```
qradtran-cli example --out case.json         écrire un fichier de scénario modèle
qradtran-cli validate case.json              vérifier les paramètres
qradtran-cli plan case.json                  lister les exécutions du noyau prévues, sans calcul
qradtran-cli run case.json --out out.csv     calculer et écrire un CSV (--unit um|nm|cm-1  --density cm-1|nm|um)
qradtran-cli run case.json --out out.json    écrire un JSON (avec métadonnées)
qradtran-cli batch-example --out plan.json   écrire un modèle de plan de balayage
qradtran-cli batch plan.json --out lut.h5    calcul par lots (.h5 ou .csv ; --resume pour reprendre)
qradtran-cli kernel-info                     afficher l'état du noyau et les résolutions disponibles
```

Codes de sortie : 0 succès, 1 erreur d'argument, 2 échec de validation, 3 échec du noyau. Les fichiers de scénario et les fichiers de projet de l'interface sont interchangeables.

---

## 9. Interface Python

`pyqradtran\` dans le dossier d'installation est un paquet Python compilé pour Python 3.10 64 bits. Ajoutez le dossier d'installation au `PYTHONPATH` avant utilisation :

```bat
set PYTHONPATH=C:\Program Files\QRadtran
python
```

```python
import pyqradtran as qr

s = qr.Scenario.standard("MidlatitudeSummer")
s.geometry.mode = "SlantPath"
s.geometry.observer_alt_km = 0.0
s.geometry.target_alt_km = 10.0
s.geometry.view_zenith_deg = 30.0
s.aerosol.type = "Rural"
s.aerosol.visibility_km = 23.0
s.spectral.set_range(3, 5, unit="Micrometer", step=0.01, resolution="Coarse")
s.outputs.transmittance = True
s.outputs.path_radiance = True

r = qr.run(s)
nu = r.wavenumber                   # tableau numpy, cm⁻¹
T = r["Transmittance"]
L = r.series("PathRadiance", per="Micrometer")     # converti en par µm
print(r.band_average("Transmittance", 3.0, 5.0))  # {'average':…, 'integral':…}
r.to_csv("out.csv", unit="Micrometer")

# balayage de paramètres
plan = qr.BatchPlan()
plan.base = s
plan.add_dimension("/geometry/target_alt_km", [1, 2, 5, 10], "tgt_km")
plan.add_band("MWIR", 3.0, 5.0)
qr.batch(plan, "lut.h5")
```

L'exemple complet est `pyqradtran\python_quickstart.py`. Les liaisons partagent la bibliothèque centrale et le noyau avec l'interface graphique : les résultats sont identiques.

---

## 10. Paramètres

Outils → Paramètres :

| Élément | Description |
|---|---|
| uvspec.exe / répertoire de données libRadtran | Emplacement du noyau, par défaut `kernels\libradtran` sous le dossier d'installation. Normalement à ne pas modifier |
| Détecter le noyau | Affiche la version du noyau et les résolutions installées |
| Processus noyau simultanés max. | 0 = automatique (cœurs CPU moins 1). Réduire si la mémoire manque |
| Délai d'exécution du noyau | Secondes. Résolution fine, bandes larges ou nombreux flux peuvent nécessiter une valeur plus grande |
| Conserver le répertoire de travail en cas d'échec | Conserve les fichiers d'entrée et de sortie du noyau pour diagnostic ; l'emplacement figure dans le journal |
| Conserver aussi le répertoire de travail en cas de succès | Diagnostic uniquement, normalement désactivé |
| Langue de l'interface | Suivre le système ou fixer l'une des huit langues ; prend effet après redémarrage |

Les répertoires de travail sont créés dans `%LOCALAPPDATA%\WaveFront\QRadtran\runs\`.

---

## 11. Méthode de calcul et précision

- **Noyau** : uvspec de libRadtran 2.0.6, exécuté dans un processus séparé ; absorption moléculaire par le modèle de bande à longueurs d'onde représentatives REPTRAN (fondé sur HITRAN 2004 et suivants), diffusion multiple par DISORT.
- **Transmittance sur trajet oblique** : en bande solaire, rapport des éclairements directs aux deux altitudes ; en bande thermique, rapport des différences de luminance de deux exécutions avec température de sol perturbée.
- **Trajets horizontaux et quasi horizontaux** : le coefficient d'extinction est déduit de la transmittance verticale de la couche puis intégré le long du trajet sphérique.
- **Luminance du trajet** : luminance à l'observateur moins la luminance à la cible multipliée par la transmittance.
- **Précision** : comparaison avec MODTRAN4 dans les fenêtres atmosphériques en bande III et bande II ; dans 6 cas sur 8, la transmittance moyenne dans la fenêtre s'écarte de moins de 0,04, le meilleur cas de 0,003. Dans les bandes d'absorption fortes, les deux générations de données spectroscopiques diffèrent systématiquement. Voir le rapport de validation fourni avec le logiciel.
- **Résolutions REPTRAN** : le fin 1 cm⁻¹ est de la même classe que le modèle de bande 1 cm⁻¹ de MODTRAN ; le grossier 15 cm⁻¹ convient aux estimations rapides.

---

## 12. Questions fréquentes

**La barre d'état affiche « Noyau indisponible » au démarrage.**
Le dossier d'installation a été déplacé ou `kernels\libradtran` a été supprimé par un antivirus. Vérifiez les chemins dans les paramètres ; réinstallez si nécessaire.

**Le bouton Exécuter est grisé.**
Un message de validation rouge s'affiche sous le panneau de paramètres ; corrigez ce qu'il indique. Causes fréquentes : trajet oblique choisi avec la cible à la même altitude que l'observateur ; angle zénithal en contradiction avec la position haute/basse de la cible ; bornes spectrales identiques.

**Le calcul échoue et le journal indique « Netcdf error … reptran_…_medium ».**
Les tables de données de la résolution choisie manquent. L'installateur complet contient les trois ; avec un paquet réduit, utilisez Grossier ou ajoutez les données.

**La transmittance infrarouge thermique est nulle partout.**
Se produisait dans les versions antérieures à 0.2.0 avec le solveur deux flux ; corrigé — DISORT est utilisé automatiquement lorsqu'une luminance est requise.

**Le calcul est lent.**
Résolution fine + bande large + 32 flux est le cas le plus lent. Réglez les paramètres en résolution grossière et produisez le graphique final en résolution fine ; pour les lots, utilisez tous les processus disponibles.

**Les valeurs de l'axe droit sont difficiles à interpréter.**
L'axe droit porte les grandeurs radiométriques ; l'unité suit le réglage Densité spectrale de la barre d'outils : par cm⁻¹, par nm ou par µm. Tables et CSV utilisent le même réglage.

**L'interface est en anglais.**
Lorsque la langue d'affichage de Windows n'est pas prise en charge, le logiciel revient à l'anglais. Choisissez la langue dans Outils → Paramètres → Langue de l'interface et redémarrez.

**Python signale « DLL load failed ».**
`PYTHONPATH` doit pointer sur le dossier d'installation lui-même (le niveau contenant `qradtran_core.dll` et `Qt6Core.dll`), pas sur le sous-dossier `pyqradtran` ; Python doit être en 3.10 64 bits.

**Puis-je utiliser mon propre libRadtran ?**
Oui. Dans les paramètres, faites pointer uvspec.exe et le répertoire de données vers votre version ; elle doit être en 2.0.x.

---

## 13. Désinstallation

Désinstallez QRadtran depuis Paramètres → Applications, ou lancez Désinstaller QRadtran depuis le menu Démarrer. La désinstallation conserve les fichiers de projet de l'utilisateur et les enregistrements d'exécution sous `%LOCALAPPDATA%\WaveFront\QRadtran` ; supprimez-les manuellement si nécessaire.

Pour la variante portable, supprimez simplement le dossier.

---

## 14. Fichiers et dossiers

```
QRadtran\
├── QRadtran.exe              interface graphique
├── qradtran-cli.exe          ligne de commande
├── qradtran_core.dll         bibliothèque centrale
├── Qt6*.dll, platforms\, styles\, translations\ …   runtime Qt
├── hdf5.dll, msvcp140.dll, vcruntime140*.dll        HDF5 et runtime VC++
├── kernels\libradtran\
│   ├── bin\uvspec.exe        noyau de calcul (compilation MinGW) et son runtime
│   ├── data\                 profils atmosphériques, aérosols, nuages, spectres solaires et tables REPTRAN
│   ├── source\               archive des sources libRadtran, correctifs et script de compilation (exigence GPL)
│   └── VERSION
├── data\presets\             scénarios d'exemple
├── pyqradtran\               liaisons Python et exemples
├── licenses\                 textes des licences tierces
└── doc\                      ce guide (md et html en huit langues)
```

Les fichiers de projet (`.json`) et les CSV / HDF5 / images exportés sont enregistrés à l'emplacement choisi par l'utilisateur.

---

## Assistance

Wuhan WaveFront Co., Ltd.　　WeChat : angaio
