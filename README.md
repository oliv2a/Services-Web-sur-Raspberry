# TP Raspberry Pi - Installation des services WEB

## Description
Ce TP guide l'installation et la configuration d'un serveur web complet sur Raspberry Pi avec Apache2, PHP, MySQL (MariaDB) et PHPMyAdmin.

## Prérequis
- Raspberry Pi fonctionnel avec Raspbian/Raspberry Pi OS
- Connexion internet
- Accès SSH ou terminal direct

---

## 1. Installation d'Apache2

Installation du serveur web Apache :

```bash
sudo apt install apache2
```

---

## 2. Configuration des droits d'accès

Donner les droits d'accès du répertoire web à l'utilisateur `pi` :

```bash
sudo chown -R pi:www-data /var/www/html/
sudo chmod -R 770 /var/www/html/
```

---

## 3. Vérification du fonctionnement d'Apache

**Sur un PC du réseau :**
- Ouvrir un navigateur et taper l'adresse IP de votre Raspberry Pi

**Sur le Raspberry Pi directement :**
- Ouvrir un navigateur et taper `localhost`

Vous devriez voir la page par défaut d'Apache.

---

## 4. Installation de PHP

```bash
sudo apt install php php-mbstring
```

---

## 5. Vérification du fonctionnement de PHP

### Supprimer la page par défaut d'Apache

```bash
sudo rm /var/www/html/index.html
```

### Créer un fichier de test PHP

```bash
echo "<?php phpinfo(); ?>" > /var/www/html/index.php
```

### Tester PHP

**Sur un PC du réseau :**
- Ouvrir un navigateur et taper l'adresse IP de votre Raspberry Pi

**Sur le Raspberry Pi :**
- Ouvrir un navigateur et taper `localhost`

Vous devriez voir la page d'information PHP.

---

## 6. Installation de MySQL (MariaDB)

```bash
sudo apt install mariadb-server php-mysql
```

---

## 7. Configuration de l'utilisateur MySQL

### Se connecter à MySQL

```bash
sudo mysql --user=root
```

### Configuration (sans se déconnecter)

**⚠️ ATTENTION : Les 3 commandes suivantes doivent être exécutées sans se déconnecter !**

1. **Supprimer l'utilisateur root par défaut :**
```sql
DROP USER 'root'@'localhost';
```

2. **Créer un nouvel utilisateur root avec mot de passe :**
```sql
CREATE USER 'root'@'localhost' IDENTIFIED BY 'raspberry';
```

3. **Donner tous les privilèges :**
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
```

### Se déconnecter

```sql
exit;
```

### Reconnexion avec le nouvel utilisateur

```bash
sudo mysql --user=root --password=raspberry
```

---

## 8. Installation de PHPMyAdmin

```bash
sudo apt install phpmyadmin
```

### Configuration durant l'installation

1. **Choix du serveur web :**
   - Sélectionner `apache2` avec la barre d'espace
   - Valider avec `Ok`

2. **Configuration de la base de données :**
   - Comme la BDD est déjà installée et configurée, choisir `<Non>`

### Vérification du fonctionnement

Ouvrir un navigateur et accéder à :
```
http://[ADRESSE_IP_RASPBERRY]/phpmyadmin
```

Exemple :
```
http://192.168.1.182/phpmyadmin
```

### Connexion

- **Utilisateur :** `root`
- **Mot de passe :** `raspberry`

**Note :** Une erreur peut apparaître lors de la première connexion. Cliquer sur le lien de l'erreur et suivre les indications pour la corriger.

---

## Résumé des identifiants

| Service | Utilisateur | Mot de passe |
|---------|-------------|--------------|
| MySQL   | root        | raspberry    |
| PHPMyAdmin | root     | raspberry    |

---

## Dépannage

- Si Apache ne démarre pas : vérifier les logs avec `sudo systemctl status apache2`
- Si PHP ne fonctionne pas : redémarrer Apache avec `sudo systemctl restart apache2`
- Pour les problèmes MySQL : vérifier le service avec `sudo systemctl status mariadb`

---

## Auteur
TP réalisé dans le cadre de l'apprentissage du Raspberry Pi
O. Wailly
