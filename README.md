# VR Gaze Agent

**Interactions naturelles par le regard avec un agent virtuel en réalité virtuelle**

Projet d'études pratiques, 3e année d'informatique à l'INSA Rennes (2025-2026, semestres 5 et 6), supervisé par Katja Zibrek.

> Ce dépôt est une vitrine : il présente le projet à travers la soutenance finale. Le projet Unreal Engine lui-même n'est pas publié.

📄 **[Voir les slides de la soutenance (PDF)](soutenance.pdf)** · 🎬 **[Voir la démo du prototype](https://youtu.be/vwSNiX_6F8w)**

[![Démo du prototype](https://img.youtube.com/vi/vwSNiX_6F8w/maxresdefault.jpg)](https://youtu.be/vwSNiX_6F8w)

---

## Le projet

Un humain virtuel (MetaHuman) avec lequel on converse à voix haute en VR, et dont **le regard exprime une personnalité**. L'agent suit le regard de l'utilisateur grâce à l'eye-tracking du casque, et adapte ses mouvements d'yeux et de tête selon un trait de personnalité : extraverti ou introverti.

**Semestre 5 : un agent conversationnel en VR.** Conversation vocale complète avec le personnage (reconnaissance vocale, réponse générée par un LLM, synthèse vocale), avec un regard encore aléatoire.

**Semestre 6 : un regard qui transmet une personnalité.** Objectifs :
- transmettre des émotions et des traits de personnalité par le comportement non verbal ;
- converser de façon crédible, avec synchronisation labiale (lip sync) ;
- faire réagir l'agent au regard réel de l'utilisateur.

## Fonctionnement

| Brique | Rôle |
|---|---|
| **Eye-tracking** | Le casque HTC Vive mesure où regarde l'utilisateur ; l'agent peut ainsi soutenir ou éviter son regard |
| **Personality Gaze (yeux)** | Rotation et interpolation des yeux du MetaHuman, paramétrées selon le profil extraverti ou introverti |
| **Personality Gaze (tête)** | Mouvements de tête cohérents avec le regard et la personnalité |
| **Émotions** | Expressions faciales pilotées par les Animation Blueprints |
| **Conversation** | Pipeline voix → texte → LLM → voix, d'abord via l'API OpenAI, puis via le plugin ConvAI pour bénéficier du lip sync et d'animations de conversation |

Les comportements de regard s'appuient sur des travaux de recherche en perception, notamment sur la façon dont le regard d'un personnage virtuel influence la personnalité perçue (Ruhland, Zibrek et McDonnell, 2015), et sur le modèle des traits de personnalité de Lewis Goldberg (Big Five).

## Choix techniques

**API OpenAI ou plugin ConvAI ?** Nous avons comparé les deux approches pour la conversation :

| | API OpenAI | Plugin ConvAI |
|---|---|---|
| Lip sync | ❌ | ✅ |
| Animations de conversation | ❌ | ✅ (quelques-unes) |
| Latence question → réponse | Faible | Élevée |
| Coût | Gratuit | Gratuit (avec limites) |

ConvAI a été retenu pour le lip sync et les animations, indispensables à la crédibilité du personnage, au prix d'une latence plus forte.

## Difficultés rencontrées

- **Plugin limité** : 100 requêtes par mois dans l'offre gratuite, et des fonctionnalités gratuites modifiées en cours de projet.
- **Animations corrompues** : environ la moitié des animations fournies par ConvAI étaient inutilisables.
- **Calibration** de l'eye-tracking du HTC Vive, délicate à stabiliser.
- **Poids du projet** : Unreal Engine et les MetaHumans demandent une machine puissante et génèrent des fichiers volumineux.

## Stack

`Unreal Engine 5.7` · `Blueprints` · `MetaHuman` · `HTC Vive (eye-tracking)` · `SteamVR` · `API OpenAI` · `ConvAI`

## Équipe

Projet réalisé à cinq, en deux sous-groupes : l'un sur l'intégration et le réglage de ConvAI, l'autre sur les émotions et les interactions avec le MetaHuman.

Amadou B. · Tom B. · Maël J. · Thomas L. · **Alexandre R.**

**Ma contribution :** [À COMPLÉTER : ton sous-groupe et ce que tu as développé concrètement]

## Références

1. K. Ruhland, K. Zibrek, R. McDonnell. *Perception of personality through eye gaze of realistic and cartoon models.* ACM Symposium on Applied Perception, 2015.
2. I. Goudé, A. Bruckert, A. H. Olivier, L. Hoyet. *Real-time multi-map saliency-driven gaze behavior for non-conversational characters.* IEEE TVCG, 2023.
3. M. Chollet, M. Ochs, C. Pelachaud. *A methodology for the automatic extraction and generation of non-verbal signals sequences conveying interpersonal attitudes.* IEEE Transactions on Affective Computing, 2017.
4. B. Biancardi, C. Wang, M. Mancini, C. Pelachaud. *A computational model for managing impressions of an embodied conversational agent in real-time.* ACII, 2019.
5. B. David-John, C. Peacock, T. R. Jonker. *Towards gaze-based prediction of the intent to interact in virtual reality.* ETRA, 2021.
6. S. Sonlu, B. Bendiksen, F. Durupinar, U. Güdükbay. *The effects of embodiment and personality expression on learning in LLM-based educational agents.* arXiv:2407.10993, 2024.
