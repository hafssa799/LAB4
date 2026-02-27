
📄 Rapport d'analyse statique - MyApplication
Informations générales

Date d'analyse : 27/02/2026

Analyste : Hafssa Azarg

APK analysé : app-debug.apk

Version : 1.0 (versionCode 1)

Package : com.example.myApplication

Provenance : APK généré depuis Android Studio (mode debug)

Outils utilisés : JADX GUI, dex2jar v2.4, JD-GUI

Résumé exécutif

Cette analyse statique a révélé 3 vulnérabilités potentielles dans l'application PizzaRecipes.

Les principales préoccupations concernent :

L'application est compilée en mode debug

Une activité exportée accessible depuis l’extérieur

L’option allowBackup activée

Le niveau de risque global est évalué comme : Moyen

Actions prioritaires recommandées :

Désactiver le mode debug en production

Vérifier et restreindre les composants exportés

Désactiver allowBackup si non nécessaire

Constats détaillés
Constat #1 : Application en mode debug activé

Sévérité : Élevée

Description :
Le fichier AndroidManifest.xml contient l’attribut :

android:debuggable="true"

Localisation :
AndroidManifest.xml → Balise <application>

Impact potentiel :

L’application peut être déboguée

Risque d’ingénierie inverse facilité

Accès facilité aux données internes

Remédiation recommandée :
Désactiver le mode debug en production :

android:debuggable="false"
Constat #2 : Activité exportée

Sévérité : Moyenne

Description :
L’activité suivante est exportée :

android:exported="true"

SplashActivity est accessible depuis d'autres applications.

Localisation :
AndroidManifest.xml → SplashActivity

Impact potentiel :

Une autre application peut lancer cette activité

Augmentation de la surface d’attaque

Remédiation recommandée :
Limiter l’export uniquement si nécessaire ou ajouter des contrôles de sécurité.

Constat #3 : allowBackup activé

Sévérité : Moyenne

Description :
L’attribut suivant est activé :

android:allowBackup="true"

Localisation :
AndroidManifest.xml → Balise <application>

Impact potentiel :

Les données de l'application peuvent être sauvegardées

Extraction possible via ADB backup

Remédiation recommandée :

android:allowBackup="false"
Annexes
Permissions demandées

com.example.pizzarecipes.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION

(Aucune permission sensible comme INTERNET ou ACCESS_FINE_LOCATION n’a été détectée.)

Composants exportés

SplashActivity (exported = true)

ProfileInstallReceiver (exported = true)

Conclusion finale

L'application analysée est une application simple sans permissions sensibles majeures.

Cependant, la présence du mode debug activé et allowBackup augmente les risques en environnement de production.

Avant publication sur le Play Store, il est recommandé de :

Compiler en mode release

Désactiver debug

Restreindre les composants exportés

Vérifier les paramètres de sécurité Android
