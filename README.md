# Projet : Infrastructure as Code (IaC) avec Ansible

## 📌 Objectif

Automatiser le déploiement d'une infrastructure système complète (DNS + Web + LDAP + Sécurité + Backup) sur une machine Ubuntu, en utilisant **Ansible** comme outil d'Infrastructure as Code.

Au lieu de configurer manuellement chaque service, l'ensemble du déploiement est décrit dans du code YAML, versionnable et reproductible.

## 🛠️ Compétences mises en œuvre

- **Ansible** : Playbooks, rôles, handlers, templates Jinja2, variables
- **DNS** : BIND9 (zone `l2eni.mg`)
- **Web** : Apache2 + PHP
- **Annuaire** : OpenLDAP + phpLDAPadmin
- **Sécurité** : UFW (pare-feu) + Fail2ban (anti-brute-force)
- **Backup** : Sauvegarde automatique des configurations
- **Linux** : Ubuntu 24.04, administration système

## 🏗️ Structure du projet
projet_iac_l2eni/
├── README.md # Ce fichier
├── playbook.yml # Le playbook principal
├── inventory/
│ └── inventory.ini # Liste des machines cibles
├── group_vars/
│ └── all.yml # Variables globales (domaine, mots de passe)
└── roles/
├── dns/ # BIND9
├── web/ # Apache + LDAP
├── security/ # UFW + Fail2ban
└── backup/ # Sauvegarde des configs




## 🚀 Comment lancer le projet

### Prérequis
- Ubuntu 24.04
- Ansible installé : `sudo apt install ansible -y`
- Collection `community.general` : `ansible-galaxy collection install community.general`

### Lancement

```bash
cd ~/projet_iac_l2eni
ansible-playbook -i inventory/inventory.ini playbook.yml -K

L'option -K demande le mot de passe sudo pour l'élévation de privilèges.





Vérification
# Vérifier le DNS
dig appli.l2eni.mg @localhost

# Vérifier les utilisateurs LDAP
ldapsearch -x -D "cn=admin,dc=l2eni,dc=mg" -w admin123 -b "dc=l2eni,dc=mg"

# Ouvrir le site dans un navigateur
# http://appli.l2eni.mg (login : testuser / test123)




🔄 Reproductibilité
Le projet est idempotent : relancer le playbook ne casse rien, il applique uniquement les changements nécessaires.

Pour tester la reproductibilité :
# Tout effacer
sudo apt purge -y bind9 apache2 slapd ufw fail2ban
rm -rf /etc/bind /etc/apache2 /etc/ldap

# Relancer
ansible-playbook -i inventory/inventory.ini playbook.yml -K




📊 Résultat
À la fin du playbook :

Le serveur DNS résout l2eni.mg et ses sous-domaines

Le site appli.l2eni.mg est protégé par authentification LDAP

Les utilisateurs testuser et alice sont créés dans LDAP

Le pare-feu UFW et Fail2ban protègent la machine

Les configurations sont sauvegardées automatiquement

👤 Auteur
[Johnny]
Étudiant en L2 Informatique


Sauvegarde (`Ctrl+O`, `Entrée`, `Ctrl+X`).

---

## ✅ Vérifie

```bash
cat ~/projet_iac_l2eni/README.md
