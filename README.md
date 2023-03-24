# I. Création d'un pipeline de build

1. Lancer jenkins a l'aide du fichier `docker-compose.yml`
2. Ouvrir un navigateur a l'url `localhost:8080`
3. Jenkins demande un mot de passe pour initialiser l'installation, il se trouve dans la console de docker
4. Compléter l'installation, créer un compte et installer les plugins recommandés.
5. Installer le plugin "NodeJs Plugin" et redémarrer jenkins
6. Dans "configuration globale des outils", ajouter une installation de NodeJs avec comme nom "node" et comme version, la dernière.
7. Créer un nouveau pipeline étant un "Github project" avec comme url, l'url du projet, cocher la case "Github hook trigger for GITScm polling" dans "Build Triggers", Dans "pipeline", sélectionner "Pipeline script from SCM", mettre le "SCM" comme étant "Git" et mettre l'url permettant de cloner le projet en tant que "Repository URL" (l'url doit finir par .git). Ensuite, mettre comme branch a build par défaut la branche principale du projet. Pour finir, il faut laisser "Jenkinsfile" comme "Script Path".

# II. Test du pipeline de build
1. Sélectionnez le pipeline et cliquez sur le bouton "Run".
2. Si tout se passe bien, vous devriez obtenir des logs similaires a ceci: 

```
Started by user admin
Obtained Jenkinsfile from git https://github.com/gabrielseron/lifecycle-tic-tac.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/lifecycle
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/lifecycle/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/gabrielseron/lifecycle-tic-tac.git # timeout=10
Fetching upstream changes from https://github.com/gabrielseron/lifecycle-tic-tac.git
 > git --version # timeout=10
 > git --version # 'git version 2.30.2'
 > git fetch --tags --force --progress -- https://github.com/gabrielseron/lifecycle-tic-tac.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 53440021a64babd6f53752917e8ddf19cc5602ef (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 53440021a64babd6f53752917e8ddf19cc5602ef # timeout=10
Commit message: "Merge pull request #8 from gabrielseron/Dev"
 > git rev-list --no-walk 9e7b0aae7caa7126372679716e25293cf65d3858 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Tool Install)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] sh
+ npm install

up to date, audited 273 packages in 1s

102 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
[Pipeline] sh
+ npm run build

> lifecycle-tic-tac-toe@0.1.0 build
> next build

warn  - You have enabled experimental feature (appDir) in next.config.js.
warn  - Experimental features are not covered by semver, and may cause unexpected or broken application behavior. Use at your own risk.
info  - Thank you for testing `appDir` please leave your feedback at https://nextjs.link/app-feedback

info  - Creating an optimized production build...
info  - Compiled successfully
info  - Linting and checking validity of types...
Pages directory cannot be found at /var/jenkins_home/workspace/lifecycle/pages or /var/jenkins_home/workspace/lifecycle/src/pages. If using a custom path, please configure with the `no-html-link-fors` rule in your eslint config file.
info  - Collecting page data...
info  - Generating static pages (0/4)
info  - Generating static pages (1/4)
info  - Generating static pages (2/4)
info  - Generating static pages (3/4)
info  - Generating static pages (4/4)
info  - Finalizing page optimization...

Route (app)                                Size     First Load JS
┌ ○ /                                      1.29 kB        69.9 kB
└ ○ /api/hello                             0 B                0 B
+ First Load JS shared by all              68.6 kB
  ├ chunks/455-e1284d5dbd4590a8.js         66.4 kB
  ├ chunks/main-app-1aa4cdaa59c22c60.js    203 B
  └ chunks/webpack-9cd8869e365fd24e.js     2.01 kB

Route (pages)                              Size     First Load JS
─ ○ /404                                   179 B          84.5 kB
+ First Load JS shared by all              84.3 kB
  ├ chunks/main-7d38e82290595e18.js        82.1 kB
  ├ chunks/pages/_app-907dedfd0e4177db.js  192 B
  └ chunks/webpack-9cd8869e365fd24e.js     2.01 kB

○  (Static)  automatically rendered as static HTML (uses no initial props)

[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] tool
[Pipeline] envVarsForTool
[Pipeline] withEnv
[Pipeline] {
[Pipeline] echo
Deploy Should Be Here
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```


3. Le dossier de build sera accessible dans "jenkins_home/workspace/{{NomDuPipeline}}/production"

# III. Configuratio du Webhook depuis Github.
1. Dans l'onglet settings du repository du projet, choisissez Webhooks puis add webhook.
2. Mettez l'url de jenkins en tant que Payload Url et définissez le content type en tant que "application/json"
3. Sélectionnez "Let me select individual events." et cochez les cases "Pull requests", "Push" et "Active"


# IV. Sécurisation de Github

1. Dans l'onglet settings du repository du projet, choisissez Branches puis add rule.
2. Utilisez un pattern permettant de séléctionner les branches que vous souhaitez protéger
3. Cochez les cases suivantes
    - Require a pull request before merging
        - Require approvals
    - Require approval of the most recent reviewable push
    - Require status checks to pass before merging
        - Require branches to be up to date before merging
    - Require deployments to succeed before merging