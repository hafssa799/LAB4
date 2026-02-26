<img width="951" height="85" alt="image" src="https://github.com/user-attachments/assets/7a63d224-b9d5-43f1-b61b-cb8b1b84f6d3" /># LAB 4 — Analyse Statique d’un APK avec JADX

## Objectif du Lab

-Préparer un environnement d’analyse

-Vérifier l’intégrité d’un fichier APK

-Explorer sa structure interne

-Analyser son manifeste avec l’outil JADX GUI

  # Task 1 — Préparer le workspace et vérifier l’APK

 # Objectif

-Créer un environnement de travail organisé et vérifier l’intégrité de l’APK.

1️⃣ Création du dossier de travail

Sous Windows :

 # mkdir C:\APK-Analysis
 
<img width="277" height="28" alt="image" src="https://github.com/user-attachments/assets/1dab7e1d-b78f-4bc5-a912-5e065149d4c9" />

<img width="190" height="188" alt="image" src="https://github.com/user-attachments/assets/b9cb9a0d-11b1-45d1-b2bc-4ade75b8f5f8" />

2️⃣ Copie de l’APK dans le dossier

L’APK app-debug.apk a été copié dans le dossier : C:\APK-Analysis

<img width="617" height="101" alt="image" src="https://github.com/user-attachments/assets/f108fc33-bb14-46c0-870e-1e62a5486532" />

3️⃣ Vérification que l’APK est une archive ZIP

Commande utilisée :

<img width="574" height="54" alt="image" src="https://github.com/user-attachments/assets/36b3c0ab-5fc0-4b83-b9b4-4124da695815" />

Résultat attendu :

<img width="574" height="458" alt="image" src="https://github.com/user-attachments/assets/db1aef3e-2b91-4aec-8c37-75419d950d07" />

Explication :
Les bytes 50 4B correspondent à PK, signature d’un fichier ZIP.
Un APK étant une archive ZIP, cela confirme que le fichier est valide.

4️⃣ Liste du contenu interne de l’APK

Commande utilisée :

<img width="942" height="56" alt="image" src="https://github.com/user-attachments/assets/1cb21a0c-221b-41c4-9dd4-56caa68da3fe" />

Résultat observé :

<img width="942" height="409" alt="image" src="https://github.com/user-attachments/assets/dd562dbe-318b-4f85-9c0f-958079764080" />

5️⃣ Calcul du hash SHA-256

Commande :

<img width="455" height="38" alt="image" src="https://github.com/user-attachments/assets/700473b1-6175-4ca6-9277-379d3250c1fb" />

Exemple de résultat :

<img width="456" height="110" alt="image" src="https://github.com/user-attachments/assets/62ee9fa8-f77b-4d8a-9b63-4ca7d1f50e7b" />

# Explication :
Le hash SHA-256 permet d’assurer la traçabilité et l’intégrité du fichier.
Si le fichier est modifié, le hash changera.


✅ Task 2 — Obtenir l’APK

-S’assurer de disposer d’un APK valide pour l’analyse.

# Méthode utilisée : Génération depuis Android Studio

 - L’APK a été généré à partir d’un projet Android personnel via :

Build > Build Bundle(s) / APK(s) > Build APK(s)

Chemin du fichier généré :

# C:\Users\HP\AndroidStudioProjects\M\app\build\outputs\apk\debug\app-debug.apk

Copie vers le dossier de travail avec la Vérification du fichier copié :

# Commande utilisée 

<img width="600" height="147" alt="image" src="https://github.com/user-attachments/assets/f73c1005-8a9d-4118-aeca-eab4835d13d4" />

<img width="461" height="185" alt="image" src="https://github.com/user-attachments/assets/c93958e5-0491-4a4a-98de-5ce4cb319bd0" />

✅ Task 3 — Analyse avec JADX GUI

1️⃣ Lancement de JADX GUI

Commande utilisée :

<img width="447" height="37" alt="image" src="https://github.com/user-attachments/assets/6ca4746a-d2aa-4f6f-9197-41c9ec61ce3d" />

<img width="656" height="347" alt="image" src="https://github.com/user-attachments/assets/caf4080d-ecf9-43df-a3e5-2942034de6de" />

2️⃣ Structure observée

<img width="545" height="188" alt="image" src="https://github.com/user-attachments/assets/0bf22cb2-b48d-4b34-83d6-805a212bb9dd" />

<img width="320" height="469" alt="image" src="https://github.com/user-attachments/assets/a5170f25-4dd2-4871-b0e3-ab737831ebde" />


3️⃣ Analyse du AndroidManifest.xml
🔹 Package principal

Exemple :

package="com.example.myapplication"
🔹 Version
versionName="1.0"
🔹 SDK
minSdkVersion="24"
targetSdkVersion="33"
🔹 Permissions demandées

Exemples possibles :

android.permission.INTERNET

android.permission.ACCESS_NETWORK_STATE

Ces permissions déterminent les capacités de l’application.
<img width="612" height="52" alt="image" src="https://github.com/user-attachments/assets/3545b701-3789-4326-b494-13406f2eefd4" />

📸 Capture 10 : Permissions dans le manifeste

🔹 Composants identifiés

Activities

Services

Broadcast Receivers

Content Providers

🔹 Composants exportés

Exemple :

android:exported="true"

⚠ Explication :

Un composant avec android:exported="true" peut être lancé par d’autres applications.
Cela augmente la surface d’attaque.

Depuis Android 12, cet attribut doit être explicitement défini pour les composants avec intent-filter.
<img width="376" height="52" alt="image" src="https://github.com/user-attachments/assets/2c552669-ae75-477c-adb0-d5c1abdf95ca" />
<img width="413" height="50" alt="image" src="https://github.com/user-attachments/assets/67f28908-4167-4e7a-a2b4-6969f5adc396" />

📸 Capture 11 : Composant exporté

🔹 Configurations sensibles

Vérification de :

android:usesCleartextTraffic="true"
android:debuggable="true"

usesCleartextTraffic="true" → Autorise HTTP non sécurisé

debuggable="true" → Application débogable (risque en production)
<img width="268" height="61" alt="image" src="https://github.com/user-attachments/assets/76db35ca-26e8-4bdf-a7d6-54b39c20506a" />


📸 Capture 12 : Vérification cleartext / debuggable

4️⃣ Analyse des ressources
🔹 strings.xml

Contient :

Messages affichés

URLs

Clés API (parfois exposées)

<img width="706" height="422" alt="image" src="https://github.com/user-attachments/assets/6f641b7e-69fb-4938-af42-94627ebf554d" />
<img width="725" height="418" alt="image" src="https://github.com/user-attachments/assets/58aeef7e-21f4-418d-879f-dc1ca85cf371" />

📸 Capture 13 : strings.xml

🔹 network_security_config.xml (si présent)

Aucun fichier network_security_config.xml n’a été trouvé dans l’APK.
L’application utilise la configuration réseau par défaut d’Android (HTTP bloqué par défaut, HTTPS autorisé).

<img width="517" height="125" alt="image" src="https://github.com/user-attachments/assets/d9ae2467-cc33-4678-a4a2-af6a93551632" />

Task 4 — Recherche de chaînes sensibles

Objectif : Identifier les informations sensibles codées en dur dans l'application.

Méthodologie

L’application a été ouverte dans JADX GUI.

La recherche globale (Ctrl+F) a été utilisée pour vérifier les patterns critiques suivants :

Patterns recherchés :

URLs et endpoints : http://, https://, .com, .net, .org, .io, api, endpoint, url, server

Informations d’authentification : token, api_key, apikey, secret, password, pwd, auth, bearer, jwt, oauth

Indicateurs de mode de développement : DEBUG, debug, test, staging, dev, firebase, crashlytics

Observations

| # | Valeur trouvée                                                      | Fichier / Classe        | Risque | Description                                                                                |
| - | ------------------------------------------------------------------- | ----------------------- | ------ | ------------------------------------------------------------------------------------------ |
| 1 | `https://example.com/api/pizzas`                                    | Classes Java / Retrofit | Moyen  | URL d’API codée en dur. Exposition possible des endpoints si reverse-engineering.          |
| 2 | `http://test.example.net`                                           | Classes Java            | Moyen  | URL HTTP non sécurisée. Risque d’interception de données.                                  |
| 3 | `android:debuggable="true"`                                         | `AndroidManifest.xml`   | Élevé  | Application débogable activée. Permet une inspection et modification en debug.             |
| 4 | `com.example.pizzarecipes.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` | `AndroidManifest.xml`   | Faible | Permission interne exposée. Peu critique mais attention si combinée à d’autres failles.    |
| 5 | `SplashActivity` avec `android:exported="true"`                     | `AndroidManifest.xml`   | Moyen  | Activity exportée pouvant être lancée par d’autres applications. Surface d’attaque accrue. |

Captures d’écran

Observation des URLs et endpoints :

Capture 1 : Recherche http

<img width="953" height="239" alt="image" src="https://github.com/user-attachments/assets/730010fe-963c-4cc9-be5d-e47f951c9631" />
Capture 2 : Recherche https

<img width="954" height="43" alt="image" src="https://github.com/user-attachments/assets/55836886-22d1-4df5-9487-b2edc0bde25d" />
Capture 3 : Exemple de résultat global (endpoint dans le code)

<img width="290" height="87" alt="image" src="https://github.com/user-attachments/assets/b1e55c09-ca3b-4c73-aec2-dbec28d10c7f" />

# Task 5 — Conversion DEX → JAR avec dex2jar

Objectif : Transformer le bytecode Android (fichiers DEX) en fichiers JAR pour permettre une analyse alternative.

Méthodologie

Création du dossier pour extraire les DEX :

<img width="344" height="20" alt="image" src="https://github.com/user-attachments/assets/dc64cb82-26c5-4d88-b4bc-af1cc16c839d" />


Extraction des fichiers DEX depuis l’APK :

<img width="441" height="29" alt="image" src="https://github.com/user-attachments/assets/befc8702-65d6-40b1-9a64-651ef9d20ba9" />
<img width="455" height="28" alt="image" src="https://github.com/user-attachments/assets/f299dca2-33f3-465c-ba33-81cc702e597b" />
<img width="456" height="91" alt="image" src="https://github.com/user-attachments/assets/bb600eb2-99bb-4c9d-9b70-db43d19390e3" />

Vérification des fichiers DEX extraits :

<img width="403" height="23" alt="image" src="https://github.com/user-attachments/assets/f14b492b-9588-4811-8f0b-c7ed11d0b431" />

<img width="450" height="188" alt="image" src="https://github.com/user-attachments/assets/7c0dc7a8-54f2-4ebe-a4c4-3d695a989b39" />


cd C:\path\to\dex2jar
.\d2j-dex2jar.bat "C:\APK-Analysis\dex_out\classes.dex" -o "C:\APK-Analysis\classes.jar"

Capture 18 : montre la conversion réussie en classes.jar.

Conversion automatique pour tous les fichiers DEX (multi-dex) :

Get-ChildItem -Path C:\APK-Analysis\dex_out -Filter "classes*.dex" | ForEach-Object {
    $out = "C:\APK-Analysis\$($_.BaseName).jar"
    .\d2j-dex2jar.bat $_.FullName -o $out
}

Capture 19 : tous les fichiers DEX convertis en JAR.
