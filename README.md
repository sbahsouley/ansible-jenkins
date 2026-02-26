# Projet Ansible - Installation Nginx

Ce projet Ansible permet d'installer et de configurer Nginx sur un ou plusieurs serveurs à l'aide d'un rôle Ansible.

## Structure du projet

```
ansible-nginx-project/
├── inventory              # Fichier d'inventaire des serveurs
├── ansible.cfg           # Configuration Ansible
├── playbook.yml          # Playbook principal
├── roles/
│   └── nginx/
│       ├── tasks/
│       │   └── main.yml           # Tâches d'installation
│       ├── handlers/
│       │   └── main.yml           # Gestionnaires d'événements
│       ├── defaults/
│       │   └── main.yml           # Variables par défaut
│       ├── files/
│       │   └── index.html         # Fichier index.html
│       └── templates/
│           └── default.j2         # Template de configuration Nginx
└── README.md              # Ce fichier
```

## Prérequis

- Ansible 2.9+
- Accès SSH aux serveurs cibles
- Les serveurs cibles doivent être basés sur Debian/Ubuntu

## Configuration

### 1. Éditer le fichier d'inventaire

Modifiez `inventory` pour ajouter vos serveurs :

```ini
[webservers]
web1 ansible_host=192.168.1.10 ansible_user=ubuntu
web2 ansible_host=192.168.1.11 ansible_user=ubuntu
```

### 2. Configuration Ansible (optionnel)

Le fichier `ansible.cfg` contient la configuration de base. Vous pouvez l'adapter selon vos besoins.

## Utilisation

### Exécuter le playbook

```bash
ansible-playbook playbook.yml
```

### Exécuter sur des hôtes spécifiques

```bash
ansible-playbook playbook.yml -l web1
```

### Mode dry-run (sans apporter de changements)

```bash
ansible-playbook playbook.yml --check
```

### Avec verbosité (pour le débogage)

```bash
ansible-playbook playbook.yml -v
```

## Rôle Nginx

Le rôle `nginx` effectue les tâches suivantes :

1. **Installation** : Installe le package Nginx
2. **Création du répertoire** : Crée le document root à `/var/www/nginx`
3. **Copie du fichier index.html** : Déploie une page d'accueil personnalisée
4. **Configuration** : Configure Nginx avec un template Jinja2
5. **Vérification** : Valide la syntaxe de la configuration
6. **Activation** : Démarre et active Nginx au démarrage

## Variables

Les variables par défaut sont définies dans `roles/nginx/defaults/main.yml` :

- `nginx_port`: Port d'écoute (par défaut 80)
- `nginx_user`: Utilisateur Nginx (par défaut www-data)
- `nginx_worker_processes`: Nombre de workers (par défaut auto)
- `nginx_document_root`: Chemin du document root (par défaut /var/www/nginx)

## Personnalisation

### Modifier le port

```bash
ansible-playbook playbook.yml -e "nginx_port=8080"
```

### Modifier le fichier index.html

Éditez `roles/nginx/files/index.html` et réexécutez le playbook.

### Ajouter une configuration personnalisée

Modifiez le template `roles/nginx/templates/default.j2`.

## Dépannage

### Test de connexion aux serveurs

```bash
ansible all -i inventory -m ping
```

### Vérifier les variables appliquées

```bash
ansible-playbook playbook.yml -e "ansible_verbosity=4"
```

### Afficher les tâches sans les exécuter

```bash
ansible-playbook playbook.yml --list-tasks
```

## Compatibilité

- ✅ Ubuntu 18.04+
- ✅ Ubuntu 20.04+
- ✅ Debian 10+
- ✅ Debian 11+

## License

Ce projet est fourni à titre d'exemple.
