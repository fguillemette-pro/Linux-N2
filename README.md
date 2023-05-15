---
marp: true
class:
  - invert
paginate: true
background: img/linux.png
---

![bg](img/linux.png)

# Formation Linux N2
## 
*2H - Florent Guillemette - Session 1/3*

---

# Qu'attendez-vous de cette formation ?

---

# Sommaire 1/2

- Exécuter des commandes de manière plus efficace en utilisant les fonctionnalités avancées du shell Bash, des scripts shell et divers utilitaires fournis par Red Hat Enterprise Linux.
- Configurer un service de ligne de commande sécurisé sur des systèmes distants en utilisant OpenSSH.
- Configurer les interfaces réseau et les paramètres des serveurs Red Hat Enterprise Linux.
- Archiver et copier des fichiers d'un système à un autre.
- Contrôler les connexions réseau vers les services en utilisant le pare-feu système et les règles SELinux.

---

# Sommaire 2/2

- Accéder, inspecter et utiliser des systèmes de fichiers existants sur le stockage attaché à un serveur Linux.
- Programmer l'exécution de commandes dans le futur, soit une fois ou de manière récurrente.
- Améliorer les performances du système en définissant des paramètres de réglage et en ajustant la priorité de planification des processus.

---

# OpenSSH > Présentation

sshd est le serveur (daemon)

ssh est le client

D'autres utilitaires sont présents aussi.

---

# OpenSSH > Fonctions

Permet la connexion à distance sur une machine (similaire à RDP)

Permet l'envoi et le téléchargement de fichiers.

---

# OpenSSH > Installation 1/2

Téléchargement et installation
```
dnf install openssh-server
```

Démarrage du service
```
systemctl start sshd
systemctl enable sshd
```

Controle du service
```
systemctl status sshd
```
---

# OpenSSH > Installation 2/2

Configuration du parfeux

```
# firewall-cmd --zone=public --permanent --add-service=ssh
# firewall-cmd --reload
```

---

# OpenSSH > Configuration 1/2

Le fichier de configuration

```
/etc/ssh/sshd_config
```

---

# OpenSSH > Configuration 2/2

Par défaut, l'utilisateur root n'est pas autorisé à se connecter

Pour autoriser la connexion de root:

```
PermitRootLogin yes
```

---

# OpenSSH > Authentification par clé 1/3

Vous devez posséder une paire de clés.

Cette paire est constituée d'une clé privée et d'une clé publique.

La clé privée doit rester secrète.

La clé publique est "ajoutée" sur les machines cibles

Pour générer une clé sur la machine client;

```
ssh-keygen -t rsa
```

---

# OpenSSH > Authentification par clé 2/3

Pour copier la clé publique sur le serveur

```
ssh-copy-id user@remote_host
```

---

# OpenSSH > Authentification par clé 3/3

Pour tester la connexion

```
ssh user@remote_host
```

---

# SCP > Présentation

SCP (Secure Copy) est une commande utilisée pour transférer des fichiers entre des systèmes Linux distants de manière sécurisée.

```
scp [OPTIONS] SOURCE DESTINATION
```

---

## SCP > Exemples d'utilisation

- Copier un fichier local vers un système distant :

```
scp fichier.txt utilisateur@serveur distant:/chemin/destination
```

- Copier un fichier distant vers un système local :

```
scp utilisateur@serveur distant:/chemin/source/fichier.txt /chemin/destination
```

---

## SCP > Options couramment utilisées

- `-P port` : Spécifier un port personnalisé pour la connexion SSH.
- `-r` : Copier récursivement un répertoire et son contenu.
- `-v` : Afficher la sortie détaillée pour le débogage.
- `-p` : Préserver les attributs d'origine du fichier (horodatage, permissions, etc.).
- `-C` : Activer la compression lors de la transmission des données.

---

## SCP > Authentification

SCP utilise SSH pour l'authentification et le transfert sécurisé des fichiers.

- Vous pouvez utiliser des clés SSH pour éviter de saisir le mot de passe à chaque transfert.
- Exemple : `scp -i chemin/vers/cle_privee fichier.txt utilisateur@serveur:/chemin/destination`

---

# SELinux - Security-Enhanced Linux > Présentation

SELinux (Security-Enhanced Linux) est un module de sécurité intégré dans le noyau Linux qui renforce les politiques de contrôle d'accès pour renforcer la sécurité du système.

---

## SELinux > Buts

- Contrôle fin des autorisations d'accès aux fichiers, processus, ports réseau, etc.
- Isolation des processus pour limiter les effets des failles de sécurité.
- Protection contre les attaques de type "escalade de privilèges".
- Audits et logs pour une meilleure visibilité des activités système.

---

## SELinux > Modes de fonctionnement

- **Enforcing** : Mode par défaut. SELinux applique strictement les politiques de sécurité et bloque les actions non autorisées.
- **Permissive** : SELinux génère des avertissements sans bloquer les actions non autorisées. Utile pour le débogage des politiques de sécurité.
- **Disabled** : SELinux est désactivé. Les politiques de sécurité ne sont pas appliquées.

---

## SELinux > Contextes 

- Chaque fichier, processus, port réseau, etc. a un contexte SELinux.
- Les contextes sont composés de :
  - **User** : Identifie l'utilisateur ou le rôle.
  - **Role** : Indique le rôle associé à l'utilisateur.
  - **Type** : Définit le type d'objet.
  - **MLS/MCS Level** : Niveau de sécurité multilabel (optionnel).

---

## SELinux > Commandes utiles

- `sestatus` : Affiche l'état actuel de SELinux et le mode d'application.
- `getenforce` : Vérifie si SELinux est en mode "Enforcing" ou "Permissive".
- `setenforce` : Change le mode d'application SELinux.
- `semanage` : Gère les politiques SELinux, notamment les ports, les modules, les utilisateurs, etc.
- `ls -Z` : Affiche les contextes SELinux des fichiers et répertoires.

---

## SELinux > Politiques SELinux

- Les politiques SELinux définissent les règles de contrôle d'accès.
- Les modules de politique sont utilisés pour personnaliser les politiques SELinux.
- Exemple : `semodule -i mon_module.pp` pour installer un module de politique personnalisé.

---
