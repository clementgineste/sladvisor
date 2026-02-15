# SLA Advisor

> Calculateur interactif de SLA — répondez à quelques questions sur votre service et obtenez une recommandation de niveau de SLA avec les implications techniques, financières et opérationnelles.

🔗 **[Essayer en ligne](https://clementgineste.github.io/sladvisor/)**

## Pourquoi ?

Quand un client ou un PM demande "on veut du 99.99% de disponibilité", il ne réalise pas toujours ce que ça implique :
- **99.9%** → ~8h45 d'indisponibilité par an — un seul serveur avec monitoring peut suffire
- **99.99%** → ~52 minutes par an — multi-AZ, auto-scaling, blue-green obligatoires
- **99.999%** → ~5 minutes par an — architecture active-active multi-région, coûts x10-50

Cet outil fait le pont entre le besoin business et la réalité technique.

## Fonctionnalités

- 🎯 Recommandation de tier SLA basée sur 6 critères (criticité, tolérance, support, budget, RTO, RPO)
- ⏱️ Calcul précis du temps d'indisponibilité (annuel, mensuel, hebdomadaire)
- 🏗️ Implications architecturales pour chaque niveau
- ⚠️ Alertes d'incohérence (budget faible + SLA élevé)
- 📊 Comparaison des tiers côte à côte
- 🌙 Mode sombre

## Stack

Volontairement minimaliste :
- **HTML** unique
- **[Alpine.js](https://alpinejs.dev/)** pour la réactivité (~15kb, via CDN)
- **[Tailwind CSS](https://tailwindcss.com/)** pour le styling (Play CDN)
- Zéro build, zéro dépendance, zéro backend

## Développement

```bash
# Cloner
git clone git@github.com:clementgineste/sladvisor.git
cd sladvisor

# Ouvrir directement dans le navigateur
open index.html
# ou
python3 -m http.server 8080
```

Pas de `npm install`, pas de build. Éditer `index.html`, rafraîchir le navigateur.

## Déploiement

GitHub Pages depuis la branche `main`, racine `/`.

## Licence

MIT
