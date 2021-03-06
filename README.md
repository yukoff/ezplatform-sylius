# eZ Platform & Sylius eCommerce integration

This is a proof of concept integration of [eZ Platform](https://github.com/ezsystems/ezplatform) & [Sylius eCommerce](https://github.com/sylius/sylius). eZ Platform being a really good CMS, and Sylius being a really good eCommerce solution, both running on Symfony full stack framework, and considering the fact that there is no quality solution that does both CMS and eCommerce right, it is only natural to try and connect the two.

Here both, eZ Platform and Sylius, run on same Symfony full stack instance. [A bundle](https://github.com/netgen/NetgenEzSyliusBundle) exists to assist the integration, by implementing custom user providers that make it possible to connect the users together.

Sylius users act as primary users, while eZ Platform users are secondary, you can use them (if they're connected to Sylius users) or not, it's up to you. However, you will at least need one Sylius admin user to be connected to one eZ Platform admin user in order to enable seamless login to both admin panels.

## Limitations

Due to way Sylius users are implemented, it is currently not possible to be logged in with both Sylius admin and Sylius shop users. Use a second browser or incognito mode to login both ways. Hopefully, this will be solved by the time this integration reaches a stable release.

If using legacy administration interface, login with Sylius users will not work, since legacy administration interface does not use Symfony security component. You will need to use standard eZ Publish users to login.

## Installation instructions

Run the following instructions to install the integration. This will first install Sylius, then install eZ Platform, and finally connect the admin users from Sylius and eZ Platform together.

```bash
$ git clone git@github.com:netgen/ezplatform-sylius.git
$ cd ezplatform-sylius
$ composer install
$ php app/console --env=prod sylius:install
$ php app/console --env=prod ezplatform:install clean
$ php app/console --env=prod assetic:dump
$ php app/console --env=prod ezsylius:user:connect
```

To connect the users, when asked, select the `admin` Sylius user type, then enter the e-mail address of Sylius admin user (the one you specified while running `sylius:install` command), then enter the username of eZ Platform admin user (usually `admin`).

Once all this is done, you will need to run Gulp to generate assets for Sylius backend interface. From project root, run the following:

```bash
$ npm install
$ gulp
```

## eZ Publish legacy administration

When installing and activating eZ Publish legacy administration interface, be sure to activate the `EzSyliusBundle` after `EzPublishLegacyBundle`.

To activate support for Sylius Product field type in legacy administration interface, activate `ezsylius` legacy extension in your INI files, regenerate legacy autoloads and clear legacy caches.

## Access the eZ Platform admin interface

eZ Platform admin interface is available in standard `/ez` route. Use the Sylius admin username and password to login.

## Access the Sylius shop and Sylius admin interface

If you didn't change any parameters while running `composer install`, Sylius shop will be available at `/shop`, while Sylius admin interface will be available at `/sylius/admin/`. If you don't like these paths, you can change them in `app/config/sylius_parameters.yml`.
