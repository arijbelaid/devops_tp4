<div align="center">
  <h1>🤖 TP4 : Bases d'Ansible (Inventaire, Playbooks, Clés SSH, Rôles)</h1>
  <p><strong>Automatisation de l'infrastructure avec Ansible</strong></p>
  
  <img src="https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white" alt="Ansible" />
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white" alt="Jenkins" />
</div>

<br>

> **Objectif :** Maîtriser les bases d'Ansible en déployant une infrastructure complète (serveur web, Docker, Jenkins, Portainer) sur deux nœuds gérés, en passant d'une authentification par mot de passe à une authentification par clé SSH, et en réorganisant les playbooks en rôles réutilisables.

<div align="center">
  <br>
  📄 <strong><a href="./TP4.pdf">Télécharger le rapport complet du TP4 (PDF)</a></strong>
  <br><br>
</div>

---

## 🚀 Partie 0 & 1 : Préparation et Configuration

Installation des outils sur le nœud de contrôle et configuration de l'inventaire pour cibler les nœuds `.22` et `.23`. Ajout des empreintes SSH pour sécuriser les connexions.

### Installation d'Ansible et sshpass
![Setup Ansible](captures/01-setup-ansible.png)
![Setup Ansible](captures/01-setup-ansible1.png)


### Ajout des empreintes SSH (Known Hosts)
![SSH Keyscan](captures/02-ssh-keyscan.png)

### Test de connexion avec le module Ping
Vérification de la communication avec les deux nœuds gérés.
![Ping Ansible](captures/03-ping.png)

---

## 📜 Partie 2 : Premier Playbook (Installation d'Apache)

Création du playbook `webserver_install.yml` (disponible dans ce dépôt) pour installer Apache et déployer une page web statique.

### Vérification et Exécution
Test de la syntaxe, exécution en mode "Dry-run", puis exécution réelle suivie d'un test `curl`.
![Syntax et Dry-run](captures/04-syntax-dryrun.png)
![Playbook Run](captures/05-playbook-run.png)

### Test de l'idempotence
Deuxième exécution du playbook montrant `changed=0`.
![Idempotence](captures/06-idempotence.png)

---

## 🔑 Partie 3 : Authentification par Clé SSH

Génération d'une paire de clés ED25519 et copie de la clé publique sur les nœuds gérés avec `ssh-copy-id`.

### Génération et Copie de la clé
![SSH Keygen](captures/07-ssh-keygen.png)
![SSH Copy ID](captures/08-ssh-copy-id.png)

### Exécution sans `-k`
Le playbook tourne désormais sans demander le mot de passe SSH.
![Playbook sans -k](captures/09-playbook-no-k.png)

---

## 🐳 Partie 4 : Déploiement de Conteneurs

Création du `Dockerfile` et du `docker-compose.yml` pour le site web. Le playbook `docker_install.yml` (disponible dans ce dépôt) installe Docker, Portainer, Jenkins et lance le conteneur web.

### Interfaces Web
Les services sont accessibles depuis le navigateur :
*   **Portainer** (Gestion Docker) : `http://192.168.159.22:9000`
*   **Jenkins** (CI/CD) : `http://192.168.159.22:8081`

![Portainer](captures/10-portainer.png)
![Jenkins](captures/11-jenkins.png)

---

## 🧩 Partie 5 : Réorganisation en Rôles

Découpage du playbook monolithique en trois rôles distincts (`docker_portainer_install`, `jenkins_install`, `container_deployment`). Le code de ces rôles est disponible dans le dossier `roles/` de ce dépôt.

### Création des rôles
![Init Rôles](captures/12-roles-init.png)

### Exécution finale et Idempotence
Exécution du playbook `site.yml` avec `changed=0` au deuxième passage, prouvant une gestion d'état parfaite.
![Rôles Run](captures/13-roles-run.png)

---
*Réalisé par [Arij Belaid](https://github.com/arijbelaid) dans le cadre du cours de DevOps.*
