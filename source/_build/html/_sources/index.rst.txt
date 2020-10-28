.. M214_Handbuch documentation master file, created by
   sphinx-quickstart on Fri Oct 23 14:12:55 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

======================
Deployment mit Heroku
======================

Dieses Handbuch bietet eine Übersicht, wie man einfach über Heroku eine Webseite veröffentlichen kann.

Benötigte Vorkenntnisse sind:

* Git

.. toctree::
   :caption: Inhalt:

   index


1. Grundlagen
==============

1.1. Definitionen
-----------------
Heroku ist eine Container-basierte Cloud-Plattform als Service (PaaS). Sie dient zur Veröffent-lichung von Webapplikationen, sowie deren Management. 
Heroku bietet Services, Tools, Workflows und Support an, welche die Produktivität von Entwicklern verbessern sollen. 
Ausserdem ist sie mit den Programmiersprachen Ruby, Java, Node.js, Scala, Clojure, Python, PHP und Go kompatibel. (Heroku, 2020) [#]_

1.1.1. Plattform as a Service (PaaS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Eine PaaS (Platform as a Service) bietet eine vollständige Entwicklungsumgebung und Tools in der Cloud an.
Eine PaaS stellt dabei das Bindeglied zwischen SaaS (Software as a Service) und IaaS (Infrastructure as a Service) dar. 
Die IaaS bietet lediglich die Infrastruktur und SaaS bietet die Software, doch die PaaS geht einen Schritt weiter und bietet alle Tools und die Infrastruktur in einer Plattform an. 
Der gesamte Software-Lebenszyklus läuft hierbei in der Plattform ab, das heisst von der Entwicklung über das Testing bis zum produktiven Betrieb der Applikation. (IONOS, 2019) [#]_

1.1.2. Containerbasierte Plattform
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Container in der Informatik funktionieren mit einem ähnlichen Prinzip wie Transportcontainer in der Logistik. 
Es werden Waren jeglicher Art in einem standardisierten Container verschickt und verteilt. Dabei kommt es nicht darauf an, mit welchem Transportmittel. 
Dasselbe gilt für Contai-ner in der IT. Eine Anwendung wird dabei in einem Container «verpackt» zusammen mit allen Dateien, die zu ihrer Ausführung notwendig sind. 
Dies dient zur Vereinfachung von Installati-on, Betrieb, Management und Verteilung von Anwendungen. (von der Howen, 2020) Anwendungen sind von verschiedensten Elementen in ihrer Umgebung abhängig. 
Das bedeutet, dass lokale Einstellungen, Bibliotheken oder Dateien zur Nutzung der Anwendung benötigt werden. Dabei unterscheiden sich die Einstellungen oft in den verschiedenen Entwicklungsum-gebungen (Entwicklung, Testumgebung, Produktion) und dabei kann es passieren, dass eine Anwendung in der Produktion nicht oder falsch funktioniert.
Um dieses Risiko zu minimieren werden Container genutzt. In den Containern werden alle Abhängigkeiten verpackt, sodass die Anwendung also nicht mehr direkt auf den Zielsystemen installiert werden, sondern nur im Container funktionieren. (von der Howen, 2020) [#]_

1.2. Heroku Plattform
----------------------
Die Heroku PaaS enthält integrierte Datenmanagement Services und ein mächtiges Ökosystem. Heroku lässt die Applikationen in sogenannten Dynos laufen, welche vollumfänglich verwaltete Runtime-Enviroments enthalten. 
Die Dynos können dank einem Dashboard überwacht und verwaltet werden. Es sind ausserdem Add-ons verfügbar, um Applikationen anzupassen.

1.2.1. Dynos
~~~~~~~~~~~~~~~~~~~~
Dynos sind das Herz der Heroku-Plattform. Sie sind Container, in welchen alle Umgebungs-spezifischen Daten und Einstellungen laufen. Die «Containerisierung» abstrahiert die Last der Verwaltung von Hardware oder virtuellen Maschinen. 
Das heisst anstatt die Hardware zu ver-walten stellt man die Webapplikation Heroku zur Verfügung, welche dieses und all dessen Ab-hängigkeiten in einem Dyno (Container) verpackt. (Heroku, 2020) [#]_

1.2.2. Runtime
~~~~~~~~~~~~~~~~~~~~
Die Runtime in Heroku ist dazu da die Applikation laufen zu lassen und zu verwalten. 
Sie orchestriert ausserdem die Dynos, verwaltet und überwacht deren Lebenszyklus, stellt eine passende Netzwerkverbindung, sowie http Protokolle bereit. (Heroku, 2020) [#]_

1.2.3. Procfile
~~~~~~~~~~~~~~~~~
Das Procfile enthält alle Befehle, die beim Start der Applikation ausgeführt werden müssen. Das Procfile besitzt keine Endung, das heisst Procfile.txt ist nicht gültig und wird von Heroku nicht erkannt.
Das Format, in welchem das Procfile beschrieben wird sieht wie folgt aus: 

.. code-block:: bash

   <process type>: <command>

Das Procfile wird am einfachsten über die Commandline erstellt, in dem :code:`$ touch Procfile`ausgeführt wird. Danach wird mit :code:`$ nano Procfile` in das Procfile geschrieben. [#]_

2. Erste Einstellungen
======================


2.1. Heroku Account erstellen 
-----------------------------
Um auf Heroku eine Webapplikation zu veröffentlichen benötigt man einen Account. Einen Heroku Account zu erstellen ist gratis und führt nicht zu einer wahnsinnigen Flut an Werbe-Mails, daher ist es als sicher zu betrachten. 
Unter http://heroku.com kann in der Ecke rechts oben auf Sign Up geklickt und dann ein Konto errichtet werden. Man wird dazu aufgefordert seine E-Mail-Adresse zu verifizieren.
Nach erfolgreicher Verifizierung seiner E-Mail-Adresse kann man sich anmelden.

2.2. Git installieren 
-----------------------------

2.2.1. unter Windows
~~~~~~~~~~~~~~~~~~~~~~~~~~
Um Git für Windows herunterzuladen klickt man auf den folgenden Link, wobei der Download direkt gestartet wird: https://git-scm.com/download/win

2.2.2. unter MacOS
~~~~~~~~~~~~~~~~~~~~~~
Ab Mac OS Version 10.9 sollte Git bereits installiert sein. 
Sollte dies nicht der Fall sein, gibt es mehrere Optionen, um Git zu installieren.

Mit dem Installer:
Unter http://git-scm.com/download/mac kann man sich den Git-Installer herunterladen.

Mit Homebrew:
Homebrew ist eine Paketverwaltung von OS X, durch die einfach Programme und Software installiert werden können.
Auch Git kann mit Homebrew installiert werden.

.. code-block:: bash

   $ brew install git

Mit MacPorts:
Auch MacPorts ist eine Paketverwaltung von OS X, mit der Git heruntergeladen werden kann.
Zunächst muss MacPorts aktualisiert werden:

.. code-block:: bash

   $ sudo port selfupdate

Danach wird nach den neusten Portionierungen und Varianten von Git gesucht.

.. code-block:: bash

   $ port search git
   $ port variants git

Git installieren

.. code-block:: bash

   $ sudo port install git +bash_completion+credential_osxkeychain+doc


2.2.3. unter Linux
~~~~~~~~~~~~~~~~~~~~~~
Für die Installation unter Debian-basierten Linuxsystemen kann folgender Befehl verwendet werden:

.. code-block:: bash

   $ sudo apt-get install git

Für Fedora Linux kann über yum oder dnf Git installiert werden. 

.. code-block:: bash

   $ sudo dnf install git

oder

.. code-block:: bash

   $ sudo yum install git


2.2.4. nach der Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Zunächst muss man mit ``$ git --version`` überprüfen, ob Git tatsächlich installiert wurde.
Bei erfolgreicher Installation sollte man Git seine Identität bestätigen, in dem man seinen Namen und E-Mail-Adresse angibt.

.. code-block:: bash

   $ git config --global user.name "Max Mustermann" 
   $ git config --global user.email "max.mustermann@email.com"

Die Angaben werden bei jedem zukünftigen Commit zugeordnet, sodass immer ersichtlich ist, wer Änderungen gemacht hat.

2.3. Heroku installieren
------------------------
2.3.1. unter Windows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Für Windows gibt es einen Heroku Installer für 32- oder 64bit, der unter https://devcenter.heroku.com/articles/heroku-cli heruntergeladen werden kann.

2.3.2. unter MacOS
~~~~~~~~~~~~~~~~~~~~~~
Für die Installation unter MacOS wird die Bash verwendet und folgender Befehl ausgelöst:

.. code-block:: bash

   $ brew tap heroku/brew && brew install heroku

Homebrew installiert dann Heroku automatisch.

2.3.3. unter Linux
~~~~~~~~~~~~~~~~~~~~~~
In der Bash unter Linux Ubuntu kann folgender Befehl ausgelöst werden:

.. code-block:: bash

   $ sudo snap install --classic heroku

Auch für andere Linux-Systeme ist Heroku unter https://snapcraft.io/heroku verfügbar.

2.3.4. nach der Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nach der Installation von Heroku sollte die Installation verifiziert werden, in dem man die Version vom Heroku-CLI überprüft. 
Dies wird überprüft, in dem man :code:`$ heroku --version` im Terminal eingbit.
Die Ausgabe sollte wie folgt aussehen: :code:`heroku/7.0.0 (darwin-x64) node-v8.0.0`

3. Erste Schritte mit Heroku
============================
3.1. Heroku login
-----------------
Zunächst muss man sich mit dem in Punkt 2.1. erstellten Account bei Heroku anmelden.
Dies gelingt ebenfalls über das Terminal mit dem Befehl  :code:`$ heroku login`

.. image:: _static/images/login.png

Möchte man im Terminal bleiben, ohne sich über den Browser anzumelden, kann man auch den Befehl :code:`$ heroku login -i` eingeben. 

.. image:: _static/images/login-i.png

3.2. Applikation erstellen
--------------------------
Nachdem man sich erfolgreich bei Heroku über die Commandline angemolden hat, kann eine Applikation erstellt werden. Zunächst wird mit :code:`$ cd path/to/my/appfolder` in den entsprechenden Ordner navigiert. 
Mit dem Befehl :code:`$ heroku create -a appname` wird dann die Applikation auf Heroku erstellt.

.. image:: _static/images/create.png


4. Veröffentlichen einer Applikation
=====================================
Mit Heroku können viele verschiedene Applikationen mit jeglichen Programmiersprachen veröffentlicht werden. 
Namentlich:

* Ruby
* Java
* Python
* Node.js
* PHP
* Go
* Scala
* Clojure

4.1. Ruby
------------
4.1.1. Ruby on Rails installieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Für das Entwickeln einer Webapplikation mit Ruby müssen zunächst Ruby und Rails installiert werden.

4.1.1.1. unter Windows 
^^^^^^^^^^^^^^^^^^^^^^^
Für Windows gibt es einen Installer, den man herunterladen kann, welcher Ruby installiert. 
Dieser ist unter folgendem Link verfügbar: https://rubyinstaller.org

4.1.1.2. unter MacOS
^^^^^^^^^^^^^^^^^^^^^^^
Für MacOS gibt es Drittanbieter-Werkzeuge wie `rbenv`, die genutzt werden können, um Ruby zu installieren. 
In rbenv ist es möglich die Ruby-Version direkt auszuwählen, sodass die Entwicklungsumgebung und die Produktion zusammen funktionieren. [#]_ 

Die Installation ist einfach über Homebrew zu bewältigen.

.. code-block:: bash

   $ brew install rbenv
   $ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
   $ source ~/.bash_profile

Mit rbenv-doctor kann dann die Installation überprüft werden.

.. code-block:: bash

   $ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash

Nach der erfolgreichen Installation von rbenv kann nun mit Hilfe dessen Ruby und Rails installiert werden.

.. code-block:: bash

   $ rbenv install X.X.X 

Bei X.X.X ist die gewollte Ruby-Version zu spezifizieren. 
Laut Ruby's offizieller Website ist die aktuell stabilste Version 2.7.2 [#]_

Es kann passieren, dass bei der Installation ein OpenSSL-Fehler auftritt. Dieser kann mit folgenden Mitteln behoben werden:[#]_

.. code-block:: bash

   $ brew install curl-ca-bundle
   $ cp /usr/local/opt/curl-ca-bundle/share/ca-bundle.crt `ruby -ropenssl -e 'puts OpenSSL::X509::DEFAULT_CERT_FILE'`

Wenn Ruby installiert ist, muss danach noch Rails installiert werden. Rails lässt sich einfach mit :code:`gem install rails --no-document` installieren.
Nach der vollständigen Installation von Ruby und Rails wird jetzt noch yarn benötigt:

.. code-block:: bash

   $ brew install yarn

4.1.1.3. unter Linux
^^^^^^^^^^^^^^^^^^^^^^^
In Linux muss zunächst yarn installiert werden, bevor Ruby und Rails installiert werden können. 
Für Ubuntu werden folgende Befehle ausgeführt:

.. code-block:: bash

   $ sudo apt-get install curl
   $ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   $ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   $ sudo apt update && sudo apt install yarn

Für Fedora:

.. code-block:: bash

   $ curl -sL https://rpm.nodesource.com/setup_12.x | bash -
   $ curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
   $ sudo yum install yarn

Danach wird mit Hilfe von yarn Rails installiert:

Ubuntu: 

.. code-block:: bash

   $ bash < <(curl -sL https://raw.github.com/railsgirls/installation-scripts/master/rails-install-ubuntu.sh)

Fedora:

.. code-block:: bash

   $ bash < <(curl -sL https://raw.github.com/railsgirls/installation-scripts/master/rails-install-fedora.sh)

4.1.2. Ruby-App generieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Um eine neue Ruby-App zu erstellen wird zunächst mit dem Befehl :code:`$ rails new appname --database=postgresql` eine Rails-App erstellt, welche mit postgresql läuft, da Heroku SQLite nicht unterstützt. Mit :command:`cd` wird in das neu erstellte Projekt navigiert.
Danach wird mit :code:`rails g controller home` einen Controller erstellt. 

.. image:: _static/images/controller.png

Der Controller der hier erstellt wurde heisst also: `home_controller.rb`
In den Controller schreiben wir folgendes:

.. code-block:: ruby

   class HomeController < ApplicationController
      def index; end
   end

Nun wird eine HTML-Seite erstellt, welche angezeigt werden soll. 
Unter `app/views/pages` wird das File 'home.html.erb' generiert in welchem :code:`<h1>Hello World!</h1>` steht.

Nun wird noch die Route gesetzt, sodass der Controller die richtige HTML-Seite anspricht.
Daher wird das File `config/routes.rb` geöffnet und :code:`root 'index#home'` hineingeschrieben.

4.1.3. mit Heroku veröffentlichen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Um die soeben generierte Webapp zu veröffentlichen wird die Heroku-App verwendet, welche unter Punkt 3.2. erstellt wurde. 
Dazu muss jedoch noch ein sogenanntes Procfile erstellt werden. 

Für das Ruby-App werden folgende Befehle in das Procfile geschrieben:

.. code-block:: bash

   web: bundle exec puma -t 5:5 -p ${PORT:-3000} -e ${RACK_ENV:-development}

Mit :code:`$ git remote -v` wird zuerst überprüft, ob die Remote-Connection zur Heroku-App noch läuft.

.. image:: _static/images/gitremote

Sollten mit diesem Befehl keine Remote-Connections angezeigt werden, muss mit :code:`$ heroku git:remote -a appname` die Verbindung hergestellt werden.

Nun werden alle Änderungen mit :code:`$ git add .` auf Git gestaged und mit :code:`$ git commit -m 'my commitmessage'` comitted.
Danach wird der Commit auf das Remote-Repository gepushed mit :code:`$ git push heroku master` und ist somit auf Heroku veröffentlicht.

Die veröffentlichte Webapplikation ist nun unter https://appname.herokuapp.com/ einsehbar, wobei der appname der Name der Heroku-App ist.

4.2. Python
------------
4.2.1. Python installieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
4.2.1.1. unter Windows
^^^^^^^^^^^^^^^^^^^^^^^
Unter https://www.python.org/downloads/windows/ kann Python für Windows installiert werden.

4.2.1.2. unter MacOS
^^^^^^^^^^^^^^^^^^^^
Unter https://www.python.org/downloads/mac-osx/ kann Python für MacOS installiert werden.

4.2.1.3. unter Linux
^^^^^^^^^^^^^^^^^^^^
Unter https://www.python.org/downloads/source/ kann Python für alle Linux-Distributionen installiert werden.

4.2.2. Pip installieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
PIP ist ein Paketverwaltungssystem, welches zur Verwaltung und Installation von Paketen, die in Python geschrieben wurden. [#]_
Zunächst muss überprüft werden, ob PIP nicht bereits installiert ist.

.. code-block:: bash

   pip --version

Wenn pip antwortet, ist es bereits installiert. Im Normalfall sollte Pip bei der Installation von Python auch installiert worden sein.
Sollte dies jedoch nicht der Fall sein, kann man es nachträglich installieren. 

Dazu muss jedoch zuerst noch überprüft werden, ob Python auch wirklich installiert ist. 
Mit dem Befehl :code:`python` sollte die Version angezeigt werden. 

Nun wird das File  unter https://bootstrap.pypa.io/get-pip.py heruntergeladen und als get-pip.py gespeichert.

Um nun Pip zu installieren muss folgender Befehl ausgeführt werden:

.. code-block:: bash

   python get-pip.py

Bei Python 3 muss :code:`python3 get-pip.py` verwendet werden.

Dies startet den Installationsprozess.
Nach der Installation kann mit :code:`pip --version` die Installation überprüft werden.
[#]_

4.2.3. Python Flask-App generieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Flask ist ein Framework, um mit Python Webapplikationen zu generieren. 

Um eine Flask-App zu erstellen wird zunächst ein Ordner erstellt, in welchem man die Commandline öffnet. 
Zunächst wird eine virtuelle Umgebung mit pipenv erstellt und Flask und Gunicorn heruntergeladen.
Um pipenv zu nutzen wird pipenv mit pip installiert mit :code:`pip install pipenv`

.. code-block:: bash

   $ pipenv install flask gunicorn

Nach der Installation von Flask und Gunicorn wird nun das Procfile erstellt.

.. code-block:: bash
   
   web: gunicorn wsgi:app

In der Datei 'runtime.txt' wird die Python-Version definiert, welche mit :code:`python --version` in Erfahrung gebracht werden kann.

.. code-block:: bash
   
   Python-3.7.4

Nun wird unser app.py File erstellt:

.. code-block:: python

   from flask import Flask 
   
   app = Flask(__name__) 
   
   @app.route("/") 
   def index():
      return "Hello World!"

Zusätlich wird noch das wsgi.py File erstellt, weches die Applikation startet.

.. code-block:: python

   from app import app

   if __name__ == "__main__":
      app.run()

Nun wird die Virtuelle Umgebung mit :code:`$ pipenv shell` zum Laufen gebracht und alle Änderungen auf Git gestaged mit :code:`$ git add .`.
Nach dem Commit (:code:`$ git commit -m 'commitmessage'`) wird die Flask-App nun auf Heroku veröffentlicht mit :code:`$ git push heroku master`.

Die veröffentlichte Webapplikation ist nun unter https://appname.herokuapp.com/ einsehbar, wobei der appname der Name der Heroku-App ist.


Bibliographie
=============

.. [#] Heroku. (2020). About Heroku. Von Heroku: https://www.heroku.com/about abgerufen
.. [#] IONOS. (2019). PaaS: Platform as a Service im Überblick. Von IONOS: https://www.ionos.de/digitalguide/server/knowhow/paas-platform-as-a-service/ abgerufen
.. [#] von der Howen, L. (2020). Was sind Container in der IT? Von cloudmag: https://www.cloud-mag.com/was-sind-container/ abgerufen
.. [#] Heroku. (2020). Heroku Dynos. Von Heroku: https://www.heroku.com/dynos abgerufen
.. [#] Heroku. (2020). Heroku Runtime. Von Heroku: https://www.heroku.com/platform/runtime abgerufen
.. [#] RBENV. (2020). RBENV. Von Github: https://github.com/rbenv/rbenv abgerufen
.. [#] Ruby. (2020). Ruby herunterladen. Von Ruby: https://www.ruby-lang.org/de/downloads/ abgerufen
.. [#] RailGirls. (2020). Setup recipe for Rail Girls. Von RailGirls: https://guides.railsgirls.com/install#setup-for-os-x
.. [#] Heroku. (2020). The Procfile. Von Heroku: https://devcenter.heroku.com/articles/procfile abgerufen
.. [#] PhoenixNap. (2019). How To Install PIP To Manage Python Packages On Windows. Von PhoenixNap: https://phoenixnap.com/kb/install-pip-windows abgerufen
.. [#] PyPa. (2020). Installation. von PyPa: https://pip.pypa.io/en/stable/installing/ abgerufen