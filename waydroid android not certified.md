---
title: Waydroid android not Certified
---
## Waydroid affiche appareil non certifié :

Recupérer l'id Android
`sudo grep android_id ~/.local/share/waydroid/data/data/com.google.android.gms/shared_prefs/Checkin.xml | cut -f2 -d">" | cut -f1 -d"<"`

Puis enregistrer l'id sur [https://www.google.com/android/uncertified/](https://www.google.com/android/uncertified/)