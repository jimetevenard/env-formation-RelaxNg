# Image Docker de formation

basée sur [codercom/code-server](https://github.com/cdr/code-server)

le [TP RNG](https://github.com/jimetevenard/TP-RelaxNg.git) et son environnement d'éxécution (Java + Jing) sont préinstallés.

## Build et lancement

````
# build de l'image de ce dépot
docker build -t formation-relaxng .

# lancement d'un container et exposition du port 8080 en local
docker run -d -p 127.0.0.1:8080:8080 formation-relaxng --auth none
````

## Utilisation et test

Accéder à <http://localhost:8080/?folder=/home/coder/project>

L'éditeur s'ouvre sur le projet du TP

L'utilitaire *Jing* est disponible dans le *path*.  
Exemple : ouvrir le terminal intégré et taper : `jing livres-init.rng livres.xml`

### Notes

* le fait de spécifier l'hôte local dans le mapping de port `-p 127.0.0.1:8080:8080` restreint l'exposition du serveur à la machine hôte.

* On aurait pu exploser le port `8080` à l'exterieur du *container* avec `-p 8080:8080`.  
Cela n'est toutefois pas recommandé, on préferera utiliser un *reverse proxy* (avec chiffrement TLS pour servir en HTTPS) pour exposer notre container. L'usage d'un serveur en *reverse proxy* permet également de mapper un (sous-)domaine différent pour chaqun des conteneurs.

* On spécifie l'option `--auth none` pour désactiver l'authentification par mot de passe de code-server (de façon à générer nous même l'authentification)

* Il peut être utile de faire un *volume* sur le répoertoire du projet (pour le conserver à l'exterieur du container : `-v "/somwhere/on/host:/home/coder/project"`



## Liste complète des args de code-server

````
coder@b5aca4af2f36:~/project$ /usr/local/bin/code-server --help
code-server 2.1698-vsc1.41.1

Usage: code-server [options][paths...]

Options
  --locale <locale>              The locale to use (e.g. en-US or zh-TW).
  --user-data-dir <dir>          Specifies the directory that user data is kept in. Can be used to open multiple distinct instances of Code.
  -v --version                   Print version.
  -h --help                      Print usage.
  --telemetry                    Shows all telemetry events which VS code collects.
  --extra-builtin-extensions-dir Path to an extra builtin extension directory.
  --extra-extensions-dir         Path to an extra user extension directory.
  --base-path                    Base path of the URL at which code-server is hosted (used for login redirects).
  --cert                         Path to certificate. If the path is omitted, both this and --cert-key will be generated.
  --cert-key                     Path to the certificate's key if one was provided.
  --format                       Format for the version. Allowed value is 'json'.
  --host                         Host for the server.
  --auth                         The type of authentication to use. Allowed values are 'password', 'none'.
  --open                         Open in the browser on startup.
  --port                         Port for the main server.
  --socket                       Listen on a socket instead of host:port.

Extensions Management
  --extensions-dir <dir>                            Set the root path for extensions.
  --list-extensions                                 List the installed extensions.
  --show-versions                                   Show versions of installed extensions, when using --list-extension.
  --category                                        Filters installed extensions by provided category, when using --list-extension.
  --install-extension <extension-id | path-to-vsix> Installs or updates the extension. Use `--force` argument to avoid prompts.
  --uninstall-extension <extension-id>              Uninstalls an extension.
  --enable-proposed-api <extension-id>              Enables proposed API features for extensions. Can receive one or more extension IDs to enable individually.

Troubleshooting
  --verbose                          Print verbose output (implies --wait).
  --log <level>                      Log level to use. Default is 'info'. Allowed values are 'critical', 'error', 'warn', 'info', 'debug', 'trace', 'off'.
  -s --status                        Print process usage and diagnostics information.
  --disable-extensions               Disable all installed extensions.
  --disable-extension <extension-id> Disable an extension.
  --max-memory                       Max memory size for a window (in Mbytes).
````

