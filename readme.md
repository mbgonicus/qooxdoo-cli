# qooxdoo command line interface

This command line utility allows you create, manage and build qooxdoo applications.

## Develoment status
Alpha/Proof-of-concept. Everything can and will change.

## Prerequisites
- **Node** Currently requires NodeJS v8. The released version will be transpiled to support earlier node versions, but whichever
version you choose to use we recommend you consider `nvm` to ease installing and switching between node versions - you
can find the Linux version at http://nvm.sh and there is a version for Windows at 
https://github.com/coreybutler/nvm-windows

Install `nvm` and then:

```bash
nvm install 8
nvm use 8
```

- **Qooxdoo** - you need a clone of the Qooxdoo repository - automatic installation is coming (and will be part of the
6.0 release) but for now you need to make sure you clone the repo yourself:

```bash
git clone https://github.com/qooxdoo/qooxdoo.git
```
  
- **Python v2** - you still need Python v2, again this will go soon but for now please install a recent version 2 (not v3) of
    python from https://www.python.org/downloads/.  
    
    **NOTE** If you're on Windows, make sure you add Python to your PATH
     
(Note that there is no longer a dependency on ImageMagick)

## Installation
- Install qx-cli, create a sample application and compile it
```bash
npm install -g qx-cli
qx create myapp
cd myapp
qx compile
```

Note that `qx-cli` needs to be able to find the `qooxdoo` repo that you cloned from github - in the example above,
it finds it because it discovers a `qooxdoo` directory, but if you place your `qooxdoo` directory elsewhere you
should use this syntax to create an application:
```
qx create myapp --qxpath /path/to/qooxdoo/repo
```

## Installation for Development
- Install qx-cli 
```bash
git clone https://github.com/qooxdoo/qx-cli
cd qx-cli
npm install
```

- In order to have a globally callable executable, do the following:
```bash
npm link
```

- If you want to use an unreleased version of qxcompiler, download it and 
  `npm link path/to/qxcompiler` from the `qx-cli` directory.


## Example command line usage
```bash
qx create myapp -I # creates the foo application skeleton non-interactively
cd myapp

# (optional) install contrib libraries
qx contrib update # updates the local cache with information on available contribs 
qx contrib list # lists contribs compatible with myapp's qooxdoo version, determine installation candidate
qx contrib install johnspackman/UploadMgr # install UploadMgr contrib library 

# compile the application, using the compile.json default configuration values 
qx compile
```

See below ("Contribution compatibility management)  if you don't get any contribs listed or if the ones you are looking for 
are missing.

## TODO
- [x] make it work, i.e., compile :-) 
- [ ] create an npm installable package
- [x] install qx as global executable

## Documemtation

### Commands

```
Typical usage:
  qx <commands> [options]

Type qx <command> --help for options and subcommands.

Commands:
  compile [options] [configFile]       compiles the current application, using
                                       compile.json
  contrib <command> [options]          manages qooxdoo contrib libraries
  create <application name> [options]  creates a qooxdoo application skeleton
  upgrade [options]                    upgrades a qooxdoo application


qx compile [options] [configFile]

Options:
  --all-targets             Compile all targets in config file         [boolean]
  --target                  Set the target type: source, build, hybrid or class
                            name                    [string] [default: "source"]
  --output-path             Base path for output                        [string]
  --locale                  Compile for a given locale                   [array]
  --write-all-translations  enables output of all translations, not just those
                            that are explicitly referenced             [boolean]
  --set                     sets an environment value                    [array]
  --app-class               sets the application class                  [string]
  --app-theme               sets the theme class for the current application
                                                                        [string]
  --app-name                sets the name of the current application    [string]
  --library                 adds a library                               [array]
  --watch                   enables continuous compilation             [boolean]
  --verbose                 enables additional progress output to console
                                                                       [boolean]

qx create <application name> [options]

Options:
  -t, --type       Type of the application to create, one of: ['desktop',
                   'inline', 'mobile', 'native', 'server', 'website'].'desktop'
                   -- is a standard qooxdoo GUI application; 'inline' -- is an
                   inline qooxdoo GUI application; 'mobile' -- is a qooxdoo
                   mobile application with full OO support and mobile GUI
                   classes; 'native' -- is a qooxdoo application with full OO
                   support but no GUI classes; 'server' -- for non-browser run
                   times like Rhino, node.js; 'website' -- can be used to build
                   low-level qooxdoo applications. (Default: desktop)
                                                            [default: "desktop"]
  -o, --out        Output directory for the application folder    [default: "."]
  -s, --namespace  Applications's top-level namespace.
  -q, --qxpath     Path to the folder containing the qooxdoo framework.
                                                          [default: "./qooxdoo"]
  -v, --verbose    verbose logging
  
qx contrib <command> [options]

Commands:
  install  installs the latest compatible release of a contrib library (as per
           Manifest.json). Use "-r <release tag>" to install this particular
           release.
  list     if no repository name is given, lists all available contribs that are
           compatible with the project's qooxdoo version ("--all" lists
           incompatible ones as well). Otherwise, list all releases of this
           contrib library.
  remove   removes a contrib library from the configuration.
  update   updates information on contrib libraries from github. Has to be called
           before the other commands. 

Options:
  -a, --all      disable filters (for example, also show incompatible versions)
  -r, --release  use a specific release tag instead of the tag of the latest
                 compatible release
  -v, --verbose  verbose logging

qx upgrade [options]

Options:
  -v, --verbose  verbose logging
```
### Create a new project

You can create new project skeletons by using the `qx create command`. It has the following options:
```
  -t, --type            Type of the application to create
          [string] [choices: "desktop", "contrib", "mobile", "native", "server",
                                                 "website"] [default: "desktop"]
  -o, --out             Output directory for the application content.
  -s, --namespace       Top-level namespace.
  -n, --name            Name of application/library (defaults to namespace).
  -q, --qxpath          Path to the folder containing the qooxdoo framework.
        [default: "./qooxdoo/framework"]
  --theme               The name of the theme to be used.    [default: "indigo"]
  --icontheme           The name of the icon theme to be used.
                                                             [default: "Oxygen"]
  -I, --noninteractive  Do not prompt for missing values
  -V, --verbose         Verbose logging
```

Currently, only the "desktop" and "contrib" skeleton types are implemented. The fastest way to
create a new project is to execute `qx create foo -I`. This will create a new application
with the namespace "foo", using default values. However, in most cases you wamt to customize the
generated application skeleton. `qx create foo` will interactively ask you all information it
needs, providing default values where possible. It is recommended to pass the absolute path to 
the qooxdoo framework folder via the `--qxpath` flag in each case.

### How to list get your contrib repository listed with `qx contrib list`

- The repository **must** have a [GitHub topic](https://help.github.com/articles/about-topics/)
  `qooxdoo-contrib` in order to be found and listed.
- The tool will only show **[releases](https://help.github.com/articles/about-releases/)**
  not branches. The releases (tags) **should** be named in
  [semver-compatible format](http://semver.org/) (X.Y.Z). They **can** start with a "v"
  (for "version").
- In order to be installable, the library manifests must be placed in the repository in one of the
  following ways:
  
  a) If the repository contains just **one single library**, its `Manifest.json` file **should** be placed
     in the repository's root directory.
  b) If you ship **several libraries** in one repository, or you want to place the `Manifest.json` file
     outside of the root directory, you **must** provide a `qooxdoo.json` file in the root dir. This
     file has the following syntax:
     
 ```
 {
   "contribs": [
    { "path":"./path/to/dir-containing-manifest1" },
    { "path":"./path/to/dir-containing-manifest2" },
    ...
  ]
}
```
- Make sure to keep the "qooxdoo-version" key up to date (see below)

### Contribution compatibility management

The contrib system uses [semver](http://semver.org) and [semver ranges](https://github.com/npm/node-semver#ranges) 
to manage dependencies and compatibilites. The main dependeny is between the qooxdoo framework used by the application 
under development and the contribution (which has been also been developed with a particular qooxdoo version). 

The qooxdoo framework version can be found in the [top level `version.txt` file](https://github.com/qooxdoo/qooxdoo/blob/master/version.txt). 
The contrib declares its compatibility with qooxdoo versions using the `qooxdoo-versions` OR `qooxdoo-range` entries in `Manifest.json`
(See [this example](https://github.com/cboulanger/qx-contrib-Dialog/blob/master/Manifest.json#L21)). `qooxdoo-versions` takes an array 
of version numbers. You need to specify each and every version that you want to support, and any new qooxdoo version will break 
compatibility. This is part of the original, now defunct contrib system. It will be supported for some time, but might be deprecated 
in the future. We strongly suggest to use the `qooxdoo-range` key instead, which takes a [semver range string](https://github.com/npm/node-semver#ranges).
This allows for a much more flexible and automated dependency management. You can, for example, declare that the contrib will be 
compatible with qooxdoo versions starting with 5.02 up until version 6, i.e. as long as there is no breaking change (which is guaranteed 
by the semver specs), using `5.0.2 - 6.x` as the `qooxdoo-range`.

The `qx contrib` commands handle the compatibility data generated by semver strictly. That, for example, why you won't get much (or any) compatible contribs when executing `qx list` with a newly generated project based on the current master version of qooxdoo 
(version `6.0.0-alpha`), because few or no contribs have updated their `qooxdoo-versions` or `qooxdoo-ranged` information, even though
technically, they are compatible. In order to install these contributions, you need to do the following:
```
qx contrib list --all # this will list all available contribs, regardless of compatibility
qx contrib install <repo name> --release <release tag>
```

### Publishing contribs

The CLI makes it really easy to publish releases of your contrib library. Say you have a local clone
of the GitHub repository of your contrib library. After committing all changes to your code and pushing
them to the master branch of your repo, you can execute `qx contrib publish`. The command has the 
following options:
```
  -T, --token           Use a GitHub access token
  -t, --type            Set the release type
           [string] [choices: "major", "premajor", "minor", "preminor", "patch",
                                    "prepatch", "prerelease"] [default: "patch"]
  -I, --noninteractive  Do not prompt user
```
You need to supply a valid GitHub token or export the token as an environment variable:
```
export GITHUB_ACCESS_TOKEN=<your token> 
```
The command takes care of incrementing the version of your application. By default, the patch version
number is increased, but you can choose among the release types stated above. The command will then 
commit the version bump and push it to the master branch before releasing the new version.


### compile.json
Documentation for the compile.json format is [docs/compile-json.md](docs/compile-json.md)

