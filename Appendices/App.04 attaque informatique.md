# Appendice 4: une attaque informatique
Il n'est pas ici sujet de couvrir l'intégralité des attaques informatiques existantes, c'est le sujet de cursus complets de formations.

Je vais simplement décrire le schéma classique d'une attaque et ce sur quoi chaque étape s'appuie.

#### Reconnaissance
C'est une étape ou l'attaquant cherche à vous connaître _personnellement_, ainsi que votre matériel. Il veut savoir quels logiciels et matériels vous utilisez, et quels sont vos habitude de travail. Tout y passe **linkedin**, réseaux sociaux, **banner grabbing**, **OSINT**, **scans de ports**, **google dorks**, etc.

#### Weaponisation
Maintenant que l'attaquant sait que vous utilisez telle ou telle version d'**Apache** il va chercher les exploits qui y sont liés, comme par exemple [ici](https://www.cve.org/CVERecord?id=CVE-2025-54090). Bien entendu ce sera aussi le cas de **SSH**, **mariadb**, **php**, ou n'importe quel **logiciel présent** sur votre système.

Livraison — s’appuie sur phishing, pièces jointes, liens, supply-chain, accès distant exposé.

Exploitation — s’appuie sur vulnérabilités non corrigées, mauvaise configuration, code d’exécution à distance.

Accès initial — s’appuie sur credentiels volés, backdoors, configurations faibles, comptes tiers compromis.

Installation — s’appuie sur dropper, persistence via services, tâches planifiées, drivers malveillants.

Commande & Contrôle (C2) — s’appuie sur canaux chiffrés, domaines compromis, tunnels, serveurs bastion.

Mouvement latéral — s’appuie sur standing creds, pass-the-hash, RDP/SMB ouverts, trust relationships.

Escalade de privilèges — s’appuie sur failles locales, mauvaises ACL, services avec privilèges.

Collecte de données — s’appuie sur recherche de fichiers sensibles, bases de données, caches, journaux.

Exfiltration — s'appuie sur canaux chiffrés, DNS tunnelling, cloud tiers, volumes compressés.

Maintien & Persistance — s’appuie sur comptes backdoor, scheduled tasks, rootkits, implants.

Effacement / anti-forensic — s’appuie sur suppression de logs, chiffrement, timestomping, obfuscation.

Monétisation / impact — s’appuie sur ransomware, extorsion, vente de données, sabotage.