# Appendice 4: une attaque informatique
Il n'est pas ici sujet de couvrir l'intégralité des attaques informatiques existantes, c'est le sujet de cursus complets de formations. Je vais simplement résumé _en gros_ quelques étapes classiques d'une attaque et détailler ce qui nous concerne le plus

#### Reconnaissance
C'est une étape ou l'attaquant cherche à vous connaître _personnellement_, ainsi que votre matériel. Il veut savoir quels logiciels et matériels vous utilisez, et quels sont vos habitude de travail. Tout y passe **linkedin**, réseaux sociaux, **banner grabbing**, **OSINT**, **scans de ports**, **google dorks**, etc.

#### Analyse des vulnérabilités
Maintenant que l'attaquant sait que vous utilisez telle ou telle version d'**Apache** il va chercher les exploits qui y sont liés, comme par exemple [ici](https://www.cve.org/CVERecord?id=CVE-2025-54090). Bien entendu ce sera aussi le cas de **SSH**, **mariadb**, **php**, ou n'importe quel **logiciel présent** sur votre système. Il va ensuite copier le plus possible votre infra et s'entraîner dessus.

#### Exploitation
C'est l'attaque à proprement parler. Une fois entré, l'attaquant va chercher à atteindre son objectif, soit directement, soit après un ou plusieurs **pivot** (_fait de changer de machine ou de logiciel pendant l'attaque_). Il réalisera aussi dans doute une **élévation de privilèges** (_fait d’obtenir plus de droits que ce que l'on avait déjà dans le système_).

#### Post-exploitation
Une fois son attaque menée, l'attaquant peut se réserver une **back-door** pour revenir, **vandaliser** si tel était son objectif, **modifier des données**, en **voler**, etc. On va aussi parfois **modifier les logs** pour effacer ses traces.

### Final
Oui, **OUI** je **SAIS** c'est très vulgarisé et simplifié. Mais c'est pour que vous ayez une idée du comportement d'en face, et de fait, comprenez en quoi notre consolidation est pertinente. L'idée est aussi de vous montrer l'étendue de ce que vous ne savez pas, ce qui est vital pour avoir envie de s'améliorer.

Allez me cassez pas les pieds et [retour au cours](./CoursApache/Chapitres/06-Consolidation.md)