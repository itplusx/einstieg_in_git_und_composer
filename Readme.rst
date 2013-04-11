==========================
Einstieg in Git & Composer
==========================
+----------------------------+-----------------------------------+------------+
| TYPO3 User Group Stuttgart | Vaclav Janoch <janoch@itplusx.de> | 11.04.2013 |
+----------------------------+-----------------------------------+------------+

.. contents::

***
Git
***
Basics
======
Git ist eine Software zur verteilten Versionswerwaltung von Dateien:

- **Was heist verteilt?**
    - Es gibt keinen zentralen Server.
    - Jedes Repository ist "eigenständig".
    - Datentransfer zwischen Repositories.

- **Was heist Versionsverwaltung?**
    - ``commit``: Ein Satz von Änderungen wird in einer "Version" zusammengefasst.
    - ``branch``: Möglichkeit zur Entwicklung mehrer Entwicklungszweige.
        - z.B. master
        - z.B. TYPO3_6-0
        - z.B. TYPO3_4-7
    - ``tag``: Markierung eines Entwicklungsstandes.
        - z.B. TYPO3_6-0-1

- **Weitere Begriffe**
    - ``HEAD``: Zeigt (i.d.R) auf den letzen Commit des aktuellen Branch
    - ``master``: Standard-Branch
    - ``origin``: Standard-Remote-Repository

Git benutzen
============
Sehr guter und schlanker Einstieg:
http://rogerdudler.github.io/git-guide/index.de.html

Git im Team benutzen
====================
Grundsätzlich reicht es, dass die Repositories für alle Teammitglieder erreichbar sind.

**GitHub**
----------
Besonders für Open-Source-Projekte interessant:
http://github.com

**Git auf einem eigenen Server**
--------------------------------

Für kleine Teams (bzw. ohne ACL)
********************************

Bei dieser Variante haben alle Teammitglieder Zugriff auf alle Projekte.

Punkte 1.-5. werden auf dem Server ausgeführt, ab 6. auf den Clients.

____________________________________________
1. Git auf dem Server installieren. (Server)
____________________________________________

::

    yum install git

_____________________________________________________________________________________
2. Einen User namens ``git`` erstellen (der Name kann natürlich frei gewählt werden).
_____________________________________________________________________________________

::

    useradd git

______________________________________________________________________
3. Die Shell des ``git``-User's auf die ``git-shell`` ändern. (Server)
______________________________________________________________________

::

    usermod -s /usr/bin/git-shell git

_____________________________________
4. Public Keys registrieren. (Server)
_____________________________________

Die Public Keys der Teammitglieder in ``<git_user's_home>/.ssh/authorized_keys`` eintragen.

__________________________________________________
5. Repositories auf dem Server erstellen. (Server)
__________________________________________________

::

     git init --bare <path_to_repositories>/<repository_name>.git
     chown -R git:git <path_to_repositories>/<repository_name>.git

Hier eine kleine Hilfe (ggf. Pfad zu den Repositories anpassen):

::

    #!/bin/bash
    
    if [ $# = 1 ]
    then
     git init --bare /git/$1.git
     chown -R git:git /git/$1.git
    else
     echo "Bitte (nur) einen Namen fuer das zu erstellende Git-Repository angeben!"
     echo "Usage: ./create_git_bare_repository.sh <repository_name>"
     exit 1
    fi

_______________________________________
6. User und E-Mail einstellen. (Client)
_______________________________________
::

    git config --global user.mail "user@company.tld"
    git config --global user.name "username or name"

________________________________
7. Repositories clonen. (Client)
________________________________

::

    # ssh
    git clone user@server:project.git
    git clone ssh://user@server:project.git

    # local
    git clone /opt/git/project.git
    git clone file:///opt/git/project.git
    
    # git
    git://user@server:project.git
    
    # http
    git clone http://example.com/gitproject.git

______________________________
8. Dateien verändern. (Client)
______________________________

Einfach an den Dateien arbeiten.

_______________________________________________
9. Dateien dem Index/Stage hinzufügen. (Client)
_______________________________________________

Alle Dateien:
::

    git add .

Bestimmte Datei:    
::

    git add <file>

______________________________
10. Dateien commiten. (Client)
______________________________

::

    git commit -m "Some message."

.. tip:: Siehe TYPO3 Flow Coding Guidelines für Commit Messages:

    http://docs.typo3.org/flow/TYPO3FlowDocumentation/TheDefinitiveGuide/PartV/CodingGuideLines/PHP.html#commit-messages

____________________________
11. Dateien pushen. (Client)
____________________________

Ggf. weiterarbeiten und wieder commiten. Es muss natürlich nicht nach jedem ``commit`` ein ``push`` erfolgen. ``push`` eigentlich erst, wenn Task abgeschlossen ist.

::

    git push origin master

    
Für größere Teams (bzw. mit ACL)
********************************
Nur bestimmte Teammitglieder haben Zugriff auf bestimmte Projekte. Einfachere Verwaltung der User.

- **Gitosis**
    - Keys werden in einem git-Repository gepflegt
    - Zugriffskontrolle über ACL
    - http://git-scm.com/book/ch4-7.html
- **Gitolite**
    - https://github.com/sitaramc/gitolite
    - http://git-scm.com/book/ch4-8.html
- **GitLab**
    - http://gitlab.org

Git-Repositories im Browser
***************************
Eine Möglichkeit ist **Gitweb**: https://git.wiki.kernel.org/index.php/Gitweb

Übersichtlicher und vor allem einfacher zu installieren ist **GitList**: http://gitlist.org

Weiterführende Links
====================
================================= ===========================
http://git-scm.com                Offizielle Git Webiste.
http://gitref.org                 Übersichtliche Befehlsreferenz. Ist nicht ganz vollständig, reicht aber für den Alltag.
http://wiki.weinimo.de/Git-Hilfen Diverse Lösungen für Alltagsfragen.
================================= ===========================

********
Composer
********
Alles was man über Composer wissen muss:
http://getcomposer.org

Composer am Beispiel von TYPO3 Flow
===================================

Vorbereitung
------------
- Datenbank erstellen
- Datenbankuser erstellen
- Virtual Host erstellen

TYPO3 Flow installieren
-----------------------
Siehe: http://docs.typo3.org/flow/TYPO3FlowDocumentation/Quickstart/
::

    composer create-project -s dev typo3/flow-base-distribution htdocs
    chown -R apache:apache .
    cd htdocs
    composer install --dev

Weitere Pakete installieren
---------------------------
In die composer.json folgendes hinzufügen um TYPO3.Surf zu installieren.
::

    {
        ...
        "require": {
            ...
            "typo3/surf": "dev-master"
        },
        ...
    }
    