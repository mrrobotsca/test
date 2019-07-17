# Récuperation des donnés DJ

[![N|Solid](http://www.hydroquebec.com/images2016/logo-hydro-quebec-couleur.svg)](https://nodesource.com/products/nsolid)



[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

[![N|](https://raw.githubusercontent.com/mrrobotsca/test/master/ezgif.com-video-to-gif.gif)]()

# INTRODUCTION

Nous voulons effectuer des requêtes automatiques afin de capturer et déposer dans un répertoire centralisé les fichiers qui sont gardés en mémoire sur les relais des disjoncteurs télécommandés du réseau électrique de HQD.

Notre objectif est de donner accès à une centaine d’utilisateurs (ingénieurs et techniciens) de HQD afin d’analyser les événements des disjoncteurs.


# DESCRIPTION GÉNÉRALE

#### BESOIN D’AFFAIRES

Le besoin d’affaires est de diminuer la fréquence et les déplacements inutiles de patrouilleurs sur les lignes de HQD. Ceci nous aidera à diminuer, expliquer, prévenir les plaintes clients.


# PÉRIMÈTRE DU PROJET 

LE PROJET COUVRE LE PÉRIMÈTRE SUIVANT :


  - 1800 disjoncteurs en ligne
  - 1/3 sont télécommandés et en augmentation sur le réseau.
  - Il y a donc environ 600 disjoncteurs télécommandés que nous pouvons interroger.
  - Modèle des relais de disjoncteurs : SEL 351 et SEL 651
  - Fichiers à capturer
  - SEL 351 fichiers [*.cev ]
  - SEL 651 fichiers [ *.dat  *.hdr  *.cfg  ] comtrade
  - Nous pouvons déjà aller capturer ces fichiers en mode manuel.
  - Ce projet doit automatiser cette fonction.
  - Capturer et déposer dans un répertoire indexé tous les fichiers ci-dessus.
  - Rendre accessible ces fichiers à une centaine d’utilisateurs.

### Libraries

Libraries externs utilisés:

* [PySerial](https://pyserial.readthedocs.io/en/latest/pyserial_api.html) - pySerial API
* [Pandas](https://pandas.pydata.org/) - Python Data Analysis Library


### Installation

Tout d'abord, il faut créer notre environement virtuel à l'aide anaconda  [Anaconda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

```sh
$ conda create --name MonEnv --file spec-file.txt
$ conda activate MonEnv
```

# Compilation et Création .Exe

```sh
$ pyinstaller --onefile RecupDj.py
```

### Transferer le fichier .EXE dans les serveur de maintenance
Dans cet exemple , on transfère le fichier exécutable dans un répertoire accessible dans tous les serveurs de maintenance. 
```sh
$ copy /Y "Votre path du repertoire\RecupDj.exe" "Z:\Scripts(tous)\Lire_Info_du_SEL351R\Programmes MAT"
```

# Structure du programme


| Methode | Fonction |
| ------ | ------ |
|  urlify(s) | Remplacer tous les espaces blancs par un seul tiret |
| His_Analyse(lclcl,info_recup_path) | Analyse l'écart entre le nouveau fichier HIS extrait avec le fichier de Events.csv |
| timeout_handler(signum, frame) | Gestionnaire des timeouts  |
| end_subprocess_fct(p) | Met fin à notre subprocess |
| Lire_donnees_SEL351R(ced_lclcl, ser_comm_obj) | Fonctionne principal qui lis les donnés demandés au DJ , écrire dans des fichier désiré ( .CEV , .CSV ) |
| Lire_donnees_HIS(ced_lclcl, ser_comm_obj) | En mode auto, cette methode permet de extrait un nouveau Ficher HIS pour pouvoir comparer et detectrer  |
| get_info_sel351r(ced_lclcl, adr_ip_tunnel) | Focntion qui monte le passthrough et Ramasse les donnees du SEL351R. Lancement de SMP4 |
| Startup_validation_CED() | La fonctionne qui prend 2em argument entrée au moment de lancement du programme pour l'associer au bon CED (territoire: LAV, MAT, MTL, et etc) |
| GET_ini() |  La fonctionne qui prend 3em argument entrée au moment de lancement du programme pour l'associer au bon Fichier de .ini  |
| read_config_file() |  La fonctionne qui lit les parametre configuré dans le fichier .INI et l'associer au bon parametre du programme |
| init_logging() | La fonctionne qui initie le [Logging](https://docs.python.org/2/library/logging.html) et qui enregistre tous les logs dans le dossier désiré |
| init_resume() | La fonctionne qui initie le log file des Print() et qui enregistre le résumé tous les logs dans le dossier désiré |
| init_stop() | La fonctionne qui arrete le log file des Print()  |
| get_liste_lclcl() | La fonctionne qui lit tous les paramtres dans la liste des LCLCLs du territoire désiré  |
| send_command(command_to_send) | La fonction qui envoit les commandes désirés implementés dans le fichier ,INI . Exemple : HIS, EVE , EVE C .   |
| read_data() | La foction qui lis les donnés envoyé par SMP4. Lecture ligne par ligne et décode chaque ligne (bit-->string) |
| log_into_SEL351R() | La foction qui initie le [Serial](https://pythonhosted.org/pyserial/) et envois des commandes d'accés administrateur au SMP4 |
| detect_actual_level(actual_level, ser_comm_obj) | La fonction qui détécte le niveau d'administrateur ( Level, 1,2,3) |


License
----

MIT
