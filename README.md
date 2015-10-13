> Russian version located near this file. Just look for it if needed.

# Using Composer with Cotonti #


## Current state and problematic ##

As for current stable release of Cotonti ([Siena brunch](https://github.com/Cotonti/valencia)) we have internal routines that covers installation of Plugins and Modules for Cotonti.

Some bottlenecks we had for now:
1. No mechanics for program install Cotonti themes or third-party libraries. User must do it by himself with manual copy/check/setup operations.
2. No way to control any JS libraries used in Project. Many developers distribute own extension with common JS libraries (such as jQuery UI or Bootstrap) so we get mess of it on production sites.
3. No version control system for any if installed components (plugs, modules, themes, libs), so no compatibility check or requirement check.
4. We had no ability to install components from any centralized repository (even official).

This troubles could be resolved only with a complex solution and it's impossible to release it as a Cotonti internals. So one solution is to use ready made software for that, but with possible seamless integration with internal installation functionality.

One of the perspective piece of software that kind is [Composer](https://getcomposer.org/) (a PHP package manager). Now Composer had became the de-facto standard for managing dependencies in the PHP community.

The main features for our project is that Composer eliminates almost all current bottlenecks and flexible enough to be partially integrated in Cotonti without breaking major compatibility.

## Valencia ##

For the new experimental brunch of Cotonti ([codename Valencia](https://github.com/Cotonti/valencia)) we try to add some features to standardize and simplify using of Composer with Cotonti package.

### Cotonti package ###

Valencia had own `composer.json`, so Cotonti itself is a composer compliant package now. It brings some benefits.

 * First, as a regular package Cotonti can be faster deployed via Composer `create-project` command:
	```
	composer create-project cotonti/valencia [folder] [version]
	```
	It creates full copy of Cotonti repo in defined `folder`.

 * Second, some of required components bundled with Cotonti now distributed as Composer compliant web-component. Now its a `jQuery` library and `Bootstrap` CSS framework. So it can be updated on local machine with one command within root folder of Cotonti installation:
	```
	composer update
	```

## Installing web-components ##

Now you can install addition components for your needs via Composer. For example tries to add [font-awesome](http://fontawesome.io/) package:
```
composer require components/font-awesome
```
And here it is, in `lib/components/font-awesome` folder. We can use it for now.

## Package connector ##

If we need not only web (JS/CSS) components, but have to utilize PHP libraries as well we need to use such powerful Composer feature as auto loading.
And further integration thats way is Package Connector. This is class extension to Cotonti to provide bridge to Composer installed packages.
The main aim of that class is add tiny use of Composer generated autoloader. On the other side we can get important info about installed components and libraries, such as name, version, author, etc.
Now after we install some PHP library to Cotonti in «one-click»:
```
composer require michelf/php-markdown ~1.5
```
[this command installs [php-markdown](https://michelf.ca/projects/php-markdown/) library with latest version (up to 2.0).]
We had no need for addition `include` or `require` for your code. Just «use» it now:
```php
use Michelf\MarkdownExtra;
$parser = new MarkdownExtra;
$my_html = $parser->transform('**Markdown text**');
```

Detailed info can be found in Library [repository](https://github.com/macik/PackageConnector).

## Installing Cotonti components ##

[Under development now]

# Technical details #

