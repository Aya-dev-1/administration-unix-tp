# TP4 : Gestion des paquetages sous Linux


> Les mentions **À COMPLÉTER** sont des valeurs à recopier depuis la capture correspondante.

## Atelier 1 : Gestion de paquetages avec rpm

`rpm` manipule directement les fichiers `.rpm` et la base des paquetages installés, mais il ne résout pas les dépendances.

### 1.1 Liste des paquetages installés
Commandes : `rpm -qa` et `rpm -qa | wc -l`

`-q` interroge la base RPM et `-a` désigne tous les paquetages. Nombre de paquetages installés : **À COMPLÉTER**.

![liste des paquetages](01-liste-paquetages.png)
![nombre de paquetages](02-liste-paquetages.png)

### 1.2 et 1.3 Paquetage binutils : informations et fichiers
Commandes : `rpm -q binutils`, `rpm -qi binutils` et `rpm -ql binutils`

`-i` affiche les informations (version, éditeur, date d'installation, description) et `-l` liste les fichiers installés par le paquetage. binutils est installé : **À COMPLÉTER** (version).

![informations et fichiers de binutils](03-informations-fichier-binutils.png)

### 1.4 Paquetage d'origine de /etc/services
Commande : `rpm -qf /etc/services`

`-f` retrouve le paquetage auquel appartient un fichier. Réponse : **À COMPLÉTER**.

![paquetage de /etc/services](04-paquetage-services.png)

### 1.5 Fichiers modifiés de nfs-utils et pam
Commande : `rpm -V nfs-utils pam`

`-V` compare les fichiers installés aux métadonnées d'origine. Chaque ligne indique le type de différence : `S` (taille), `5` (somme MD5, donc contenu modifié), `T` (date), `U`/`G` (propriétaire/groupe), `M` (permissions), `c` (fichier de configuration). Fichiers modifiés et nature des différences : **À COMPLÉTER**.

![vérification nfs-utils et pam](05-verification-nfs-pam.png)

### 1.6 Téléchargement de zsh et fichiers de configuration
Commandes : `wget https://rpmfind.net/linux/centos-stream/10-stream/BaseOS/x86_64/os/Packages/zsh-5.9-14.el10.x86_64.rpm` puis `rpm -qpc zsh-5.9-14.el10.x86_64.rpm`

`-p` interroge le fichier `.rpm` sans l'installer et `-c` liste ses fichiers de configuration. Fichiers de configuration : **À COMPLÉTER**.

![fichiers de configuration de zsh](06-fichiers-configuration-zsh.png)

### 1.7 Signature du paquetage
Commande : `rpm -K zsh-5.9-14.el10.x86_64.rpm`

Vérifie l'intégrité (condensats) et la signature électronique de l'archive. Résultat : **À COMPLÉTER** (signature valide ou clé publique absente).

![signature de zsh](07-signature-zsh.png)

### 1.8 Installation de zsh
Commande : `sudo rpm -ivh --nodeps zsh-5.9-14.el10.x86_64.rpm`

`-i` installe, `-v` détaille et `-h` affiche une barre de progression. `--nodeps` ignore les dépendances, à utiliser avec prudence car le paquetage peut ne pas fonctionner.

![installation de zsh](08-installation-zsh.png)

### 1.9 et 1.10 Vérification et fichiers fournis
Commandes : `rpm -q zsh` et `rpm -ql zsh`

La première confirme que zsh est installé, la seconde liste les fichiers qu'il fournit.

![vérification de zsh](09-verification-installation-zsh.png)
![fichiers de zsh](10-liste-fichiers-zsh.png)

### 1.11 Désinstallation de zsh
Commande : `sudo rpm -e zsh`

`-e` (erase) supprime le paquetage. `rpm -q zsh` répond ensuite `package zsh is not installed`.

![désinstallation de zsh](11-desinstallation-zsh.png)

### 1.12 Paquetage fournissant /bin/ls
Commande : `rpm -qf /bin/ls`

Réponse : **À COMPLÉTER** (attendu : `coreutils`).

![paquetage de /bin/ls](12-paquetage-bin-ls.png)

### 1.13 Installation de wireshark avec rpm
Commandes : `wget https://fr2.rpmfind.net/linux/centos-stream/9-stream/AppStream/x86_64/os/Packages/wireshark-3.4.10-14.el9.x86_64.rpm` puis `sudo rpm -ivh wireshark-3.4.10-14.el9.x86_64.rpm`

L'installation échoue : wireshark dépend d'autres paquetages (bibliothèques, `wireshark-cli`, etc.) que `rpm` ne cherche pas dans les dépôts, il signale seulement les dépendances manquantes (**À COMPLÉTER** : liste affichée).

![échec de l'installation de wireshark avec rpm](13-installation-wireshark-rpm.png)

## Atelier 2 : Installation et gestion avec dnf

`dnf` s'appuie sur des dépôts : il résout les dépendances et télécharge les paquetages nécessaires.

### 2.1 Vérifier que dnf est installé
Commande : `rpm -q dnf`

![vérification de dnf](14-verification-dnf.png)

### 2.2 Installation de wireshark avec dnf
Commande : `sudo dnf install wireshark`

Cette fois, dnf télécharge wireshark et toutes ses dépendances depuis BaseOS et AppStream, puis demande confirmation avant de procéder.

![installation de wireshark avec dnf](14-installation-wireshark-dnf.png)
![suite de l'installation](suite-14.png)

### 2.3 Vérification de wireshark
Commande : `rpm -q wireshark`

![vérification de wireshark](15-verification-wireshark.png)

### 2.4 Recherche de mises à jour
Commande : `dnf check-update`

Liste les paquetages pour lesquels une version plus récente existe dans les dépôts actifs.

![vérification des mises à jour](16-verification-mises-a-jour.png)

### 2.5 Mise à jour du système
Commande : `sudo dnf update`

![mise à jour du système](17-mise-a-jour-systeme.png)
![suite de la mise à jour](suite-17-mise-a-jour-systeme.png)

### 2.6 Historique et annulation d'une transaction
Commandes : `dnf history`, `dnf history info <ID>` et `sudo dnf history undo <ID>`

`<ID>` est le numéro de la transaction qui a installé wireshark (**À COMPLÉTER**). `info` détaille la transaction (date, paquetages ajoutés) et `undo` l'annule, ce qui désinstalle wireshark et ses dépendances.

![historique dnf](18-historique-annulation-wireshark.png)
![annulation de la transaction](18-annulation-wireshark.png)

### 2.7 Informations sur zsh
Commande : `dnf info zsh`

zsh est disponible mais pas installé : version 5.8, release 9.el9, architecture x86_64, taille 3,2 Mo, dépôt `rhel-9-for-x86_64-baseos-rpms`. C'est un interpréteur de commandes interactif proche de ksh, avec complétion programmable et correction orthographique.

![informations sur zsh](19-informations-paquetage-zsh.png)

### 2.8 Paquetage fournissant /usr/share/magic
Commande : `dnf provides /usr/share/magic`

Recherche dans les dépôts le paquetage qui fournit un fichier donné. Réponse : **À COMPLÉTER**.

![paquetage de /usr/share/magic](20-paquet-fournissant-usr-share-magic.png)
![suite](suite-20-paquet-fournissant-usr-share-magic.png)

### 2.9 et 2.10 Groupes de paquets
Commandes : `dnf group list` et `dnf group info "System Tools"`

Un groupe rassemble des paquetages pour une fonction donnée. `info` détaille les paquetages obligatoires, par défaut et optionnels du groupe.

![liste des groupes](21-liste-groupes-paquets.png)
![informations du groupe System Tools](22-informations-groupe-system-tools.png)

### 2.11 et 2.12 Installation et désinstallation du groupe System Tools
Commandes : `sudo dnf group install "System Tools"` et `sudo dnf group remove "System Tools"`

![installation du groupe](22-installation-groupe-system-tools.png)
![suite de l'installation](suite-22-installation-groupe-system-tools.png)
![désinstallation du groupe](23-desinstallation-groupe-system-tools.png)

### 2.13 Dépôts actifs
Commande : `dnf repolist`

Les dépôts actifs sont `rhel-9-for-x86_64-baseos-rpms` et `rhel-9-for-x86_64-appstream-rpms`. `dnf repolist --all` affiche aussi les nombreux dépôts désactivés (`Enabled: 0`) : OpenShift, Ansible, SAP, etc.

![dépôts actifs](24-liste-depots-actifs.png)

### 2.14 à 2.16 Désactiver AppStream dans le fichier de configuration
Commandes : `sudo vi /etc/yum.repos.d/redhat.repo`, puis `dnf repolist`

Sous RHEL 9, les dépôts officiels sont décrits dans `redhat.repo` (le sujet mentionne RHEL 8, le principe est le même). Dans la section `[rhel-9-for-x86_64-appstream-rpms]`, `enabled = 1` devient `enabled = 0`. AppStream disparaît alors de `dnf repolist`.

![fichiers de configuration des dépôts](25-fichiers-configuration-depots.png)
![vérification du dépôt AppStream](25-verification-depot-appstream.png)
![désactivation d'AppStream](26-14-15-16-configuration-depots-desactivation.png)

### 2.17 Réactivation avec dnf config-manager
Commandes : `sudo dnf config-manager --set-enabled rhel-9-for-x86_64-appstream-rpms` puis `dnf repolist`

BaseOS et AppStream sont de nouveau actifs.

![réactivation d'AppStream](27-17-reactivation-appstream.png)

## Atelier 3 : Création d'un dépôt de logiciels local

### 3.1 Installation d'Apache
Commande : `sudo dnf install httpd`

![installation de httpd](28-installation-httpd.png)

### 3.2 Répertoire du dépôt
Commande : `sudo mkdir /var/www/html/mon_depot`

![création du répertoire](29-creation-repertoire-depot.png)

### 3.3 Alimentation du dépôt
Commande : `sudo dnf install --downloadonly --downloaddir=/var/www/html/mon_depot wireshark`

`--downloadonly` télécharge les RPM de wireshark et de ses dépendances sans les installer.

![téléchargement de wireshark](30-telechargement-wireshark.png)
![suite du téléchargement](suite30-telechargement-wireshark.png)

### 3.4 Création du dépôt
Commandes : `sudo dnf install createrepo_c` puis `sudo createrepo /var/www/html/mon_depot`

`createrepo` a parcouru le répertoire (13 paquetages) et créé le dossier `repodata/` contenant les métadonnées (bases sqlite) qui permettent à dnf de l'utiliser comme dépôt.

![installation de createrepo](31-installation-createrepo.png)
![création du dépôt local](32-creation-depot-local.png)

### 3.5 Configuration du dépôt
Commandes : `sudo vi /etc/yum.repos.d/mon_depot.repo` et `cat /etc/yum.repos.d/mon_depot.repo`

```ini
[mon_depot]
name=Mon Depot Local
baseurl=file:///var/www/html/mon_depot
gpgcheck=0
enabled=1
```

`baseurl` pointe vers le chemin local (`file://`), `gpgcheck=0` désactive la vérification de signature et `enabled=1` active le dépôt.

![configuration du dépôt local](33-configuration-depot-local.png)

### 3.6 Désactivation des dépôts officiels
Commandes : `sudo dnf config-manager --set-disabled rhel-9-for-x86_64-baseos-rpms`, `sudo dnf config-manager --set-disabled rhel-9-for-x86_64-appstream-rpms` puis `dnf repolist --enabled`

Seul `mon_depot` reste actif, ce qui force dnf à utiliser uniquement le dépôt local.

![désactivation des dépôts officiels](34-desactivation-depots-officiels.png)

### 3.7 Installation de wireshark depuis le dépôt local
Commandes : `sudo dnf install wireshark` et `dnf info wireshark`

Le champ `Repository` indique `mon_depot` : l'installation s'est faite depuis le dépôt local, sans accès aux dépôts Red Hat. Les dépôts officiels doivent ensuite être réactivés avec `--set-enabled`.

![informations sur wireshark](35-informations-paquet-wireshark.png)
![vérification de wireshark](35-verification-wireshark.png)

## Conclusion
`rpm` gère directement les fichiers `.rpm` (installation, interrogation, vérification) mais ne résout pas les dépendances, ce qui explique l'échec de wireshark. `dnf` s'appuie sur des dépôts : il résout les dépendances, gère les mises à jour, l'historique avec annulation et les groupes de paquets. Un dépôt local (`httpd`, `createrepo` et un fichier `.repo`) permet de distribuer des paquetages sans passer par les dépôts officiels.
