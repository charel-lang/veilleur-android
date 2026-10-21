# Veilleur — compiler l'application Android

J'ai déjà exécuté moi-même, dans mon environnement, tout ce qui ne demande pas le SDK
Android : `npm install`, `npx cap add android` et `npx cap sync android` ont tous les
trois réussi — le dossier `android/` dans cette archive est donc déjà un vrai projet
Gradle/Android complet et à jour avec votre code.

J'ai aussi testé la compilation réelle (`./gradlew`) pour vérifier la limite exacte :
elle échoue dans mon environnement parce qu'il ne peut pas joindre
`services.gradle.org` (téléchargement de Gradle) ni les serveurs du SDK Android — ce
sont des domaines bloqués ici, pas une estimation de ma part. Android Studio, lui,
pourra les joindre normalement depuis votre ordinateur.

**Il ne vous reste donc qu'une seule chose à faire** : installer Android Studio et
ouvrir ce dossier.

## Ce qu'il faut installer (une seule fois)

**Android Studio** (il installe automatiquement le SDK Android, le JDK et Gradle) —
https://developer.android.com/studio

(Node.js n'est nécessaire que si vous voulez relancer `npx cap sync` vous-même plus
tard, après une modification du code — voir la dernière section.)

## Ouvrir et construire

1. Lancez **Android Studio**.
2. **Open** → sélectionnez le dossier `veilleur-android/android` (celui-là précisément,
   pas le dossier parent).
3. Laissez Android Studio synchroniser Gradle au premier lancement (il télécharge ce
   dont il a besoin — cela peut prendre quelques minutes la première fois).

## Dans Android Studio

- Pour tester sur votre téléphone : branchez-le en USB (mode développeur + débogage USB
  activés), puis cliquez sur ▶ Run.
- Pour obtenir un fichier `.apk` à installer manuellement : menu **Build → Build Bundle(s) /
  APK(s) → Build APK(s)**. Le fichier apparaît dans
  `android/app/build/outputs/apk/debug/`.

## Sur le téléphone, après l'installation

1. Au premier lancement, autorisez les **notifications** quand l'app le demande — sans
   cette autorisation, aucune alarme ne sonnera.
2. Allez dans **Réglages → Applications → Veilleur → Batterie**, et choisissez
   **Non optimisée** (ou équivalent selon votre marque de téléphone). Sinon, Android peut
   couper les alarmes en arrière-plan pour économiser la batterie.
3. Sur Android 13 ou plus récent, si le système le demande, autorisez aussi
   **« Alarmes et rappels »** pour que l'heure soit respectée précisément même téléphone
   verrouillé.

## Mettre à jour l'app plus tard

Quand je vous donnerai une nouvelle version de `www/index.html` (nouvelles fonctions,
corrections) :

```bash
npm install
# remplacez www/index.html par le nouveau fichier, puis :
npx cap sync android
```
Puis rouvrez/reconstruisez dans Android Studio comme ci-dessus.

Vos données (tâches, habitudes, alarmes) restent sur l'appareil d'une mise à jour à
l'autre — seul le code de l'app change.

## Vérifié dans mon environnement (pas une supposition)

- ✅ `npm install` — réussi (99 paquets)
- ✅ `npx cap add android` — réussi, projet Android créé
- ✅ `npx cap sync android` — réussi, plugin `@capacitor/local-notifications@8.3.1`
  bien détecté et lié
- ❌ `./gradlew` (compilation réelle) — échoue ici précisément parce que mon
  environnement ne peut pas atteindre `services.gradle.org` ni les serveurs Google du
  SDK Android. C'est la seule étape qui doit se faire chez vous.

## Ce qui est déjà fait dans le code

- Un onglet **Alarmes** complet (ajout, modification, suppression, activation, jours de la
  semaine) est déjà dans `www/index.html`.
- Dès que l'app détecte qu'elle tourne dans cette enveloppe Android (et non dans un simple
  navigateur), elle demande la permission de notifications et programme automatiquement une
  vraie alarme locale pour chacune de vos alarmes activées — sans action supplémentaire de
  votre part, à chaque ajout ou modification.
- Dans un navigateur normal (comme l'aperçu que je publie), l'onglet Alarmes reste visible
  et utilisable comme liste de référence, mais l'app l'indique clairement : aucun son ne
  peut se déclencher hors de l'application Android compilée.
