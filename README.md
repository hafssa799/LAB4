![](https://github.com/user-attachments/assets/7a63d224-b9d5-43f1-b61b-cb8b1b84f6d)

# LAB 4 — Analyse Statique d’un APK avec JADX

 # Objectif du Lab

-Préparer un environnement d’analyse

-Vérifier l’intégrité d’un fichier APK

-Explorer sa structure interne

-Analyser son manifeste avec l’outil JADX GUI

# Task 1 — Préparer le workspace et vérifier l’APK

 # Objectif

-Créer un environnement de travail organisé et vérifier l’intégrité de l’APK.

1️⃣ Création du dossier de travail

Sous Windows :

![](https://github.com/user-attachments/assets/1dab7e1d-b78f-4bc5-a912-5e065149d4c9)

![](https://github.com/user-attachments/assets/b9cb9a0d-11b1-45d1-b2bc-4ade75b8f5f8)

2️⃣ Copie de l’APK dans le dossier

L’APK app-debug.apk a été copié dans le dossier : C:\APK-Analysis

![](https://github.com/user-attachments/assets/f108fc33-bb14-46c0-870e-1e62a5486532)

3️⃣ Vérification que l’APK est une archive ZIP

Commande utilisée :

![](https://github.com/user-attachments/assets/36b3c0ab-5fc0-4b83-b9b4-4124da695815)

Résultat attendu :

![](https://github.com/user-attachments/assets/db1aef3e-2b91-4aec-8c37-75419d950d07)

4️⃣ Liste du contenu interne de l’APK

Commande utilisée :

![](https://github.com/user-attachments/assets/1cb21a0c-221b-41c4-9dd4-56caa68da3fe)

Résultat observé :

![](https://github.com/user-attachments/assets/dd562dbe-318b-4f85-9c0f-958079764080)

5️⃣ Calcul du hash SHA-256

Commande :

![](https://github.com/user-attachments/assets/700473b1-6175-4ca6-9277-379d3250c1fb)

Exemple de résultat :

![](https://github.com/user-attachments/assets/62ee9fa8-f77b-4d8a-9b63-4ca7d1f50e7b)

# Task 2 — Obtenir l’APK

* S’assurer de disposer d’un APK valide pour l’analyse.

 Méthode utilisée : Génération depuis Android Studio

* L’APK a été généré à partir d’un projet Android personnel via :

   - Build > Build Bundle(s) / APK(s) > Build APK(s)

   - Chemin du fichier généré :  C:\Users\HP\AndroidStudioProjects\M\app\build\outputs\apk\debug\app-debug.apk

# Commande utilisée 

![](https://github.com/user-attachments/assets/f73c1005-8a9d-4118-aeca-eab4835d13d4)

![](https://github.com/user-attachments/assets/c93958e5-0491-4a4a-98de-5ce4cb319bd0)

# Task 3 — Analyse avec JADX GUI

1️⃣ Lancement de JADX GUI

Commande utilisée :

![](https://github.com/user-attachments/assets/6ca4746a-d2aa-4f6f-9197-41c9ec61ce3d)

![](https://github.com/user-attachments/assets/caf4080d-ecf9-43df-a3e5-2942034de6de)

2️⃣ Structure observée

![](https://github.com/user-attachments/assets/0bf22cb2-b48d-4b34-83d6-805a212bb9dd)

![](https://github.com/user-attachments/assets/a5170f25-4dd2-4871-b0e3-ab737831ebde)

3️⃣ Analyse du AndroidManifest.xml

![](https://github.com/user-attachments/assets/3545b701-3789-4326-b494-13406f2eefd4)

![](https://github.com/user-attachments/assets/2c552669-ae75-477c-adb0-d5c1abdf95ca)

![](https://github.com/user-attachments/assets/67f28908-4167-4e7a-a2b4-6969f5adc396)


🔹 Configurations sensibles

Vérification de :

![](https://github.com/user-attachments/assets/76db35ca-26e8-4bdf-a7d6-54b39c20506a)

4️⃣ Analyse des ressources

🔹 strings.xml

![](https://github.com/user-attachments/assets/6f641b7e-69fb-4938-af42-94627ebf554d)

![](https://github.com/user-attachments/assets/58aeef7e-21f4-418d-879f-dc1ca85cf371)

🔹 network_security_config.xml (si présent)

Aucun fichier network_security_config.xml n’a été trouvé dans l’APK.
L’application utilise la configuration réseau par défaut d’Android (HTTP bloqué par défaut, HTTPS autorisé).

<img width="517" height="125" alt="image" src="https://github.com/user-attachments/assets/d9ae2467-cc33-4678-a4a2-af6a93551632" />

# Task 4 — Recherche de chaînes sensibles

Objectif : Identifier les informations sensibles codées en dur dans l'application.

- L’application a été ouverte dans JADX GUI.

- La recherche globale (Ctrl+F) a été utilisée pour vérifier les patterns critiques suivants :

# Patterns recherchés :

- URLs et endpoints : http://, https://, .com, .net, .org, .io, api, endpoint, url, server

- Informations d’authentification : token, api_key, apikey, secret, password, pwd, auth, bearer, jwt, oauth

- Indicateurs de mode de développement : DEBUG, debug, test, staging, dev, firebase, crashlytics

# Observations

| # | Valeur trouvée                                                      | Fichier / Classe        | Risque | Description                                                                                |
| - | ------------------------------------------------------------------- | ----------------------- | ------ | ------------------------------------------------------------------------------------------ |
| 1 | `https://example.com/api/pizzas`                                    | Classes Java / Retrofit | Moyen  | URL d’API codée en dur. Exposition possible des endpoints si reverse-engineering.          |
| 2 | `http://test.example.net`                                           | Classes Java            | Moyen  | URL HTTP non sécurisée. Risque d’interception de données.                                  |
| 3 | `android:debuggable="true"`                                         | `AndroidManifest.xml`   | Élevé  | Application débogable activée. Permet une inspection et modification en debug.             |
| 4 | `com.example.pizzarecipes.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` | `AndroidManifest.xml`   | Faible | Permission interne exposée. Peu critique mais attention si combinée à d’autres failles.    |
| 5 | `SplashActivity` avec `android:exported="true"`                     | `AndroidManifest.xml`   | Moyen  | Activity exportée pouvant être lancée par d’autres applications. Surface d’attaque accrue. |


Capture 1 : Recherche http

![](https://github.com/user-attachments/assets/730010fe-963c-4cc9-be5d-e47f951c9631)

Capture 2 : Recherche https

![](https://github.com/user-attachments/assets/55836886-22d1-4df5-9487-b2edc0bde25d)

Capture 3 : Exemple de résultat global (endpoint dans le code)

![](https://github.com/user-attachments/assets/b1e55c09-ca3b-4c73-aec2-dbec28d10c7f)

# Task 5 — Conversion DEX → JAR avec dex2jar

Objectif : Transformer le bytecode Android (fichiers DEX) en fichiers JAR pour permettre une analyse alternative.

- Création du dossier pour extraire les DEX :

![](https://github.com/user-attachments/assets/dc64cb82-26c5-4d88-b4bc-af1cc16c839d)

- Extraction des fichiers DEX depuis l’APK :

![](https://github.com/user-attachments/assets/befc8702-65d6-40b1-9a64-651ef9d20ba9)
![](https://github.com/user-attachments/assets/f299dca2-33f3-465c-ba33-81cc702e597b)
![](https://github.com/user-attachments/assets/bb600eb2-99bb-4c9d-9b70-db43d19390e3)

- Vérification des fichiers DEX extraits :

![](https://github.com/user-attachments/assets/f14b492b-9588-4811-8f0b-c7ed11d0b431)

![](https://github.com/user-attachments/assets/7c0dc7a8-54f2-4ebe-a4c4-3d695a989b39)

![](https://github.com/user-attachments/assets/a903ed6a-2c5b-4ba5-8fc3-20c7de5fa9b6)

- Conversion automatique pour tous les fichiers DEX (multi-dex) :

![](https://github.com/user-attachments/assets/bc031dfc-cf8f-4118-b709-34374e022903)

**Résultat et vérification :**  

- Les fichiers JAR générés se trouvent dans `C:\APK-Analysis`.  
- Chaque fichier DEX extrait a été converti en JAR correspondant :  
  - `classes.dex` → `classes.jar`  
  - `classes2.dex` → `classes2.jar`  
  - `classes3.dex` → `classes3.jar`
 
 # Task 6 — Comparaison JADX vs JD-GUI
 
Objectif :

-Comparer deux outils de décompilation afin d’évaluer leurs différences et déterminer lequel est le plus adapté pour l’analyse d’une application Android.

Outils utilisés :

-JADX GUI (analyse directe du fichier APK)

-JD-GUI (analyse du fichier JAR généré avec dex2jar)

Méthodologie :

-Ouverture de l’APK app-debug.apk dans JADX GUI.

-Ouverture du fichier app.jar dans JD-GUI.

<img width="301" height="247" alt="image" src="https://github.com/user-attachments/assets/11780c79-b364-4277-9129-9d922d66d1c4" />

Comparaison détaillée entre JADX vs JD-GUI :

| Aspect              | JADX GUI                                                             | JD-GUI                                          |
| ------------------- | -------------------------------------------------------------------- | ----------------------------------------------- |
| Navigation          | Affiche AndroidManifest, ressources (res), assets et code Java       | Affiche uniquement les packages et classes Java |
| Lisibilité du code  | Code généralement bien structuré et lisible                          | Code lisible mais parfois moins clair           |
| Gestion Android     | Adapté aux applications Android (reconnaît activités, manifest, XML) | Ne comprend pas la structure Android            |
| Ressources          | Accès direct aux fichiers XML et ressources                          | Aucune ressource Android visible                |
| Obfuscation         | Tente de reconstruire certains noms                                  | Conserve souvent les noms obfusqués             |
| Kotlin  | Meilleure reconstruction                                             | Peut générer un code difficile à lire           |




