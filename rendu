#  TP 4 Puissant-DEMASFRAND
 

## Exercice 1

### Gestion des utilisateurs et des groupes

#### Ajouter des deux groupes : groupe1 et groupe2

```
sudo addgroup groupe1
sudo addgroup groupe2
```

#### Création des utilisateurs

```
sudo useradd -m u1 --shell /bin/bash
sudo useradd -m u2 --shell /bin/bash
sudo useradd -m u3 --shell /bin/bash
sudo useradd -m u4 --shell /bin/bash
```

#### Ajout des utilisateurs dans les groupes

```
sudo usermod -g groupe1 u1
sudo usermod -g groupe1 u2
sudo usermod -g groupe1 u4
sudo usermod -g groupe2 u2
sudo usermod -g groupe2 u3
sudo usermod -g groupe2 u4
```

#### Affichage des membres d'un groupe

Méthode 1 : Cette méthode affiche uniquement les utilisateurs dont le groupe2 est secondaire. Les utilisateurs qui ont pour groupe primaire le groupe 2 ne sont pas affichés.
```
cat /etc/group | grep groupe2
```

Méthode 2 : Pour afficher tous les utilisateurs d'un groupe, il faut utiliser une commande externe : members
```
sudo apt install members
members groupe2
```

#### ### Faire de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

```
sudo chown :groupe1 /home/u1
sudo chown :groupe1 /home/u2
sudo chown :groupe2 /home/u3
sudo chown :groupe2 /home/u4
```

####  Remplacer le groupe primaire des utilisateurs

```
sudo usermod -g groupe1 u1
sudo usermod -g groupe1 u2
sudo usermod -g groupe2 u3
sudo usermod -g groupe2 u4
```

#### Création de deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettre en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.

```
cd /home
sudo mkdir groupe1
sudo chgrp groupe1 /home/groupe1

sudo mkdir groupe2
sudo chgrp groupe2 /home/groupe2
```

#### Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?

Lors de la création d'un fichier, ses permissions sont 666 - umask. 666 droit lecture et écriture pour tout le monde.
Pour un dossier c'est 777 - umask. 777 droit lecture, écriture et exécution du dossier.

```
sudo chmod 1770 /home/groupe1
```
Nous avons le 1 en premier correspond au sticky bit et indique que seul le propriétaire ou le root peut renommer ou supprimer le dossier. Nous laissons les droits en lecture, écriture et execution du dossier pour les utilisateurs dans le groupes. Pour les utilisateurs externes, nous enlevons tous les droits. 

#### Essayer de se connecter à u1

Pour l'instant, la connection à u1 est impossible car cet utilisateur est inactif. En effet, nous n'avons pas encore défini son mot de passe.

#### Définition du mot de passe

```
sudo passwd u1
```
Une fois le mot de passe défini, nous pouvons nous connecter à u1.

#### Quels sont l’uid et le gid de u1 ?

```
id u1
```
uid : 1001 ( u1)
gid : 1001 (groupe1)
groups : 1001 (groupe1)

#### Quel utilisateur a pour uid 1003 ?

```
getent passwd "1003" | cut -d: -f1
```
uid: 1003 (u3)

####  Quel est l’id du groupe groupe1 ?

```
getent group groupe1 | cut -d: -f1
```
L'id du groupe1 est sont gid : 1001

#### Quel groupe a pour gid 1002 ?

```
getent group "1002" | cut -d: -f1
```

Le groupe ayant pour gid 1002 est le groupe2.

#### Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez

```
sudo gpasswd -d u3 groupe2
```
Cette commande renvoie une erreur car groupe2 est l'unique groupe de u3 et un utilisateur doit nécessairement appartenir à au moins un groupe primaire.

#### Modification du compte u4 :
	
- il expire au 1 er juin 2020 
-  il faut changer de mot de passe avant 90 jours 
- il faut attendre 5 jours pour modifier un mot de passe 
- l’utilisateur est averti 14 jours avant l’expiration de son mot de passe 
-  le compte sera bloqué 30 jours après expiration du mot de passe
```
sudo usermod --expiredate 2020-06-01 u4 sudo chage -m 90 u4 sudo chage -W 14 u4 sudo chage -I 30 u4
```

#### Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?

```
cat /etc/passwd | grep "root" | cut -d: -f7
```
L'interpréteur est /bin/bash.

#### à quoi correspond l’utilisateur nobody ?

L'utilisateur nobody est le nom d'un utilisateur qui n'as pas de compte utilisateur, qui n'est pas dans un groupe. Il correspond au dernier paramètre de la définition des permissions d'un fichier ou dossier.

#### Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?

La commande sudo garde le mot de passe en mémoire par défaut pendant 15 minutes.
La commande permettant de forcer l'oublie du mot de passe sudo est :

```
sudo -k
```

## Exercice 2

### Gestion des permissions

#### Création d'un fichier texte dans un dossier test dans le répertoire $HOME

Les permissions par défaut du fichier sont :

```
ls -l fichier 
-rw-r--r--1 root root
```

#### Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

```
sudo chmod 000 fichier
```

Nous ne pouvons pas lire, modifier, executer le fichier en tant que n'importe quel utilisateur. Cependant, l'utilisateur root a tous les droits sur tous les fichiers donc nous pouvons toujours faire ce que nous voulons en tant que root.


#### Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

Le contenu du fichier à bien été remplacé. On peut dire que l'utilisateur a bien les droits en écriture sur le fichier. Cependant, l'utilisateur ne peut pas lire le fichier.

#### Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

On ne peut pas exécuter le fichier avec l'utilisateur car il n'a que les droits en écriture. Mais avec sudo on peut car l'utilisateur root a tous les droits.

#### Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test

Si on enlève les droit en lecture sur le répertoire, même si le fichier a les droits de lectures, l'utilisateur n'as pas le droit de lire le fichier. . Si les permissions du répertoire sont plus restrictives que le fichier où il est, alors ce sont les permissions du répertoires qui sont prioritaires.

#### Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?

On ne peut pas modifier le fichier nouveau sans les permissions en écriture.
Même si nous redonnons la permission d'écrire dans le répertoire test, on ne peut pas écrire dans un fichier s'il n'a pas les droits en écriture. Les permissions sont donc prioritaires sur les fichiers si les permissions sont plus contraignantes par rapport au répertoire.

#### Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

Si on enlève les droits en exécution d'un répertoire, on peut toujours lister ce qu'il y a dans le répertoire mais on ne peut pas lire les fichier dedans. De plus, si ce répertoire contient lui-même des répertoires, on ne peut pas accéder à leur contenu puisqu'on ne peut pas "traverser" le répertoire. On ne peut pas modifier ni supprimer des fichiers dans un répertoire sans droit d'exécution.

#### Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?

Si nous nous plaçons dans le répertoire et que nous enlevons le droit d'exécution, nous ne pouvons plus rien faire, ni même lister les éléments du répertoire. Nous pouvons remonter dans le répertoire parent sans problème.

#### Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture

```
chmod 640 fichier
```

#### Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 077 test 
```

#### Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 055 test
```

#### Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 037 test
```

#### Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) : 
- chmod u=rx,g=wx,o=r fic 
```
chmod 534 fic
```
-  chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x--- 
```
chmod 602 fic
```
-  chmod 653 fic en sachant que les droits initiaux de fic sont 711 
```
 chmod 711 fic
```
-  chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
```
chmod 520 fic
```

#### Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

```
ls -l /etc/passwd
-rw-r--r-- 1 root root
```
Seulement root peut lire et modifier le fichier passwd, les autres utilisateurs peuvent uniquement le lire.
Ce qui est normal puisque le fichier passwd contient les informations relatives aux informations des utilisateur. 
