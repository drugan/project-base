# Commerce 2.x project template

This fork is intended for development and installs the
[drugan/commerce:8.4-2.x](https://github.com/drugan/commerce)
version of the [Commerce](https://github.com/drupalcommerce/commerce/) module.
Also, [Commerce Multistore](https://github.com/drugan/commerce_multistore),
[Devel+kint+webprofiler](https://www.drupal.org/project/devel) and
[Color Field](https://www.drupal.org/project/color_field) modules are
downloaded and installed by default.

Use [Composer](https://getcomposer.org/) to get Drupal + Commerce 2.x with all
dependencies.

Based on [drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project).

## Usage

First you need to [install composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> Note: The instructions below refer to the [global composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar)
for your setup.

After that you can create the project:

```
composer create-project --repository=https://raw.githubusercontent.com/drugan/project-base/8.x/packages.json drugan/project-base some-dir --stability dev
```

Done! Use `composer require ...` to download additional modules and themes:

```
cd some-dir
composer require drupal/examples:"1.x-dev"
```

The `composer create-project` command passes ownership of all files to the
project that is created. You should create a new git repository, and commit
all files not excluded by the `.gitignore` file:

```
git init
git add .
git commit -m "Initial commit"
```

## What does the template do?

* Drupal is installed in the `web` directory.
* Drupal Commerce core modules are placed in `web/modules/commerce_core_modules/`
* Modules (packages of type `drupal-module`) are placed in `web/modules/contrib/`
* Theme (packages of type `drupal-theme`) are placed in `web/themes/contrib/`
* Profiles (packages of type `drupal-profile`) are placed in `web/profiles/contrib/`
* Creates default writable versions of `settings.php` and `services.yml`.
* Creates the `web/sites/default/files` directory.
* Latest version of DrupalConsole is installed locally for use at `bin/drupal`.

## Updating Drupal Core

This project will attempt to keep all of your Drupal Core files up-to-date; the
project [drupal-composer/drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold)
is used to ensure that your scaffold files are updated every time drupal/core is
updated. If you customize any of the "scaffolding" files (commonly .htaccess),
you may need to merge conflicts if any of your modified files are updated in a
new release of Drupal core.

Follow the steps below to update your core files.

1. Run `composer update drupal/core --with-dependencies` to update Drupal Core
   and its dependencies.
1. Run `git diff` to determine if any of the scaffolding files have changed.
   Review the files for any changes and restore any customizations to
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `web` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish
   to perform these steps on a branch, and use `git merge` to combine the
   updated core files with your customized files. This facilitates the use
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html).
   Alternatively, you may set up [extMerge](https://gist.github.com/drugan/72ddb1a7f0d6db14dcb7b59825b37928)
   as the merging tool. This setup is not necessary if your changes are simple;
   keeping all of your modifications at the beginning or end of the file is a
   good strategy to keep merges easy.

## Generate composer.json from existing project

With using [the "Composer Generate" drush extension](https://www.drupal.org/project/composer_generate)
you can now generate a basic `composer.json` file from an existing project.
Note that the generated `composer.json` might differ from this project's file.

## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### Should I commit the scaffolding files?

The [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) plugin can download the scaffold files (like
index.php, update.php, …) to the web/ directory of your project. If you have not customized those files you could choose
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the drupal-scaffold plugin after every install or update of your project. You can
achieve that by registering `@drupal-scaffold` as post-install and post-update command in your `composer.json`:

```json
"scripts": {
    "drupal-scaffold": "DrupalComposer\\DrupalScaffold\\Plugin::scaffold",
    "post-install-cmd": [
        "@drupal-scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@drupal-scaffold",
        "..."
    ]
},
```

### Can I commit the entire project including nested git repositories?

Please, follow [this guide](https://gist.github.com/drugan/fd81d791d52c815f898e3bdd3af7dbef).


### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull
request is often a better solution), you can do so with the
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra
section of `composer.json`:
```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL to patch"
        }
    }
}
```
