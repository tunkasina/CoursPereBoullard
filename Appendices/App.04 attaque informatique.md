# Appendice 4: une attaque informatique
Il n'est pas ici sujet de couvrir l'intégralité des attaques informatiques existantes, c'est le sujet de cursus complets de formations.

Je vais simplement décrire le schéma classique d'une attaque et ce sur quoi chaque étape s'appuie.


D’accord — toutes les étapes, une ligne chacune, très court.

Reconnaissance — s’appuie sur OSINT, scans (ports/services), fuites humaines, certificats, moteurs de recherche.

Weaponisation — s’appuie sur exploit kits, payloads sur-mesure, choix d’outils et de malware.

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