# Nemours.org AEM Cloud Local Install

For the full readme, visit https://github.com/THE-NEMOURS-FOUNDATION-WEBSOLUTIONS/nemoursorg-cloud.

## GitHub

In order to view the `README.md` that contains the local setup instructions as well as necessary dependencies, you will need GitHub access. If you do not have a GitHub account, you will need to [create one](https://github.com/signup). Also, it may be required to add [two-factor autentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) added to your account.

Once you're logged into your GitHub account, navigate to the repo by going to https://github.com/THE-NEMOURS-FOUNDATION-WEBSOLUTIONS/nemoursorg-cloud.

## Clone the GitHub Repo

Prerequisites: In order to clone the repo, you must have GIT installed for your machine. Visit https://git-scm.com/book/en/v2/Getting-Started-Installing-Git for steps to follow based on your OS.

1. In your local projects folder, run the following command: `git clone git@github.com:THE-NEMOURS-FOUNDATION-WEBSOLUTIONS/nemoursorg-cloud.git -c core.symlinks=true`. Make sure to clone this repo with `-c core.symlinks=true` parameter. If you didn't, just delete your local repository and clone again. For Windows user it may be required to run git clone as an administrator.

2. Run `cd nemoursorg-cloud` to navigate into the folder that was created that contains the repo.

## Installing Dependencies

You will now install the necesarry dependencies in order to run the local setup. For more information on this, see the [README.md](https://github.com/THE-NEMOURS-FOUNDATION-WEBSOLUTIONS/nemoursorg-cloud#prerequisites) in the root directory of the repo. If you are a Mac user, you may want to also install [Homebrew]() as this makes installing some software below easier.

* Install Docker - [Download](https://www.docker.com/products/docker-desktop/)
* Install NodeJS - [Download](https://nodejs.org/en/download/)
    * Check if it is installed correctly by running `node -v`. If so, it will list the Node version that was installed.
* Install Gradle - [Download](https://gradle.org/install/)
    * Mac Users can run `brew install gradle` or install manually using the [installation guide](https://docs.gradle.org/current/userguide/installation.html#macos_installation)
    * Windows Users can refer to the [installation guide](https://docs.gradle.org/current/userguide/installation.html#windows_installation)
    * Check if it is installed correctly by running `gradle -v`. If so, it will list the Gradle version that was installed.
* Install Apache Maven - [Download](https://maven.apache.org/download.cgi)
    * Installation instructions are located at https://maven.apache.org/install.html. Alternatively, this tutorial can be used for macOS installation: https://www.digitalocean.com/community/tutorials/install-maven-mac-os
    * Check if it is installed correctly by running `mvn -v`. If so, it will list the Maven version that was installed.
* Install Java 11 - [Download](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html)
    * In order to install Java, you will need an Oracle account
    * If you use another version of Java for a separate application, it is recommended to install jEnv to easily switch between versions - [Download for macOS](https://www.jenv.be/) | [Download for Windows](https://github.com/FelixSelter/JEnv-for-Windows)
        * You can switch versions by running `jenv version ` followed by the version number
    * Check if it is installed correctly by running `java -version`. If so, it will list the Java version that it is currently using. In this case, it should be running Java 11.

### Setup Configs with Secrets
Download config files from [Nemours Sharepoint](https://nemoursonline.sharepoint.com/:f:/r/sites/NemoursAEMCloudRe-PlatformPrjTeam/Shared%20Documents/General/AEM%20Cloud/Phase-1/Implementation%20Phase/Dev%20Localhost%20Setup/local%20configs?csf=1&web=1&e=5kEyyf) and put respectively into your local repository into `ui.config/src/main/content/jcr_root/apps/nemours/osgiconfig/(CONFIG_FOLDER)`

> If in the future you should have any config which contains secret data
> - **for dev, stage, prod**  
>  use Cloud Manager Environment Configuration Secrets and refer to it in the OSGi config
> - **for local**  
> put OSGi configs in the sharepoint "local configs" folder

## Local Setup

1. Download latest AEM SDK from [AEM download page](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)
    * Move the AEM SDK to under the `env` directory
2. Obtain a dev license (`license.properties` file)
    * You can access this in the [Nemours AEM Cloud Re-Platform Project Teams channel](https://nemoursonline.sharepoint.com/:f:/r/sites/NemoursAEMCloudRe-PlatformPrjTeam/Shared%20Documents/General/AEM%20Cloud/Phase-1/Implementation%20Phase/Dev%20Localhost%20Setup?csf=1&web=1&e=1icsYD) under Documents > General > AEM Cloud > Phase-1 > Implementation Phase > Dev Localhost Setup
3. Make sure Docker is running
4. The setup process below would prompt for location of the installation source files - SDK and license.properties file paths. Full path for eg. like below should be entered.
    * `{full-directory-path}/aem-sdk-quickstart-xyz.jar`
    * `{full-directory-path}/license.properties`
5. Use gradle to setup your instance
    * Run command `sh gradlew props` and specify AEM instance source files,
    * Run command `sh gradlew :env:setup` to set up complete AEM environment with building & deploying AEM application incrementally

Other helpful commands:
* Start the instance `sh gradlew :env:up`
* Stop the instance `sh gradlew :env:down`

### Installing Content Packages Locally
Download packages from stage, then install via CRX Package Manager on your local instance `{localhost-path-and-port}/crx/packmgr/index.jsp`:

Pages: `{path-to-author-instance-stage-env}/crx/packmgr/index.jsp#/etc/packages/my_packages/go-live-sites-and-xf-content-1.0.0.zip`

DAM Assets: `{path-to-author-instance-stage-env}/crx/packmgr/index.jsp#/etc/packages/my_packages/go-live-assets-1.0.0.zip`

### Tags
Download tags zip from [nemours sharepoint](https://nemoursonline.sharepoint.com/:u:/r/sites/NemoursAEMCloudRe-PlatformPrjTeam/Shared%20Documents/General/AEM%20Cloud/Phase-1/Implementation%20Phase/Dev%20Localhost%20Setup/tags.zip?csf=1&web=1&e=AOWGxD) or [wtt sharepoint](https://wppcloud.sharepoint.com/:u:/r/sites/NemoursLocal/Shared%20Documents/Nemours%20Local/tags.zip?csf=1&web=1&e=zTMLwp)
Upload tags.zip into your AEM local environment.

Steps:
* go to `http://localhost:4502/crx/packmgr/index.jsp`
* click "Upload Package"
* browse for tags.zip
* click "Install" on "tags.zip"
* click More -> Replicate

## How to build

To build all the modules run in the project root directory the following command with Maven 3:
```
mvn clean install
```

To build all the modules and deploy the `all` package to a local instance of AEM, run in the project root directory the following command:
```
mvn clean install -PautoInstallSinglePackage
```

Or to deploy it to a publish instance, run
```
mvn clean install -PautoInstallSinglePackagePublish
```

Or alternatively
```
mvn clean install -PautoInstallSinglePackage -Daem.port=4503
```

# Frontend Local Setup

### Build and run frontend using the local environment

1. Go to  `/ui.theme` and initialize the project with the following command executed at the theme root:

```
npm install
```

2. Build the project (production mode) with the following command executed at the theme root.

```
npm run build
```

3. Run the frontend local proxy server with the following command executed at the theme root:

For localhost use (Author only):
```
npm run live
```
Opens proxy author server on localhost:7002 with browser sync - content edit does not work. For editing use localhost:4502.

For Stage with theme 1 use (Dispatcher only):
```
npm run live:stage1
```

For Stage with theme 2 use (Dispatcher only):
```
npm run live:stage2
```

For Production use (Dispatcher only):
```
npm run live:prod
```
