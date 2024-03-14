# Dockerfile-Laravel

Provides a PHP generator to produce Dockerfiles and related files. It's target is mostly Laravel, but should work for most PHP applications.

## Usage

This project provides a CLI application that generates a Dockerfile for your Laravel applications! 


### Current Features
The Dockerfile generated by this project aims to provide support for:
1. Running your app by default in an [Nginx](https://www.nginx.com/resources/glossary/nginx/)-flavored web server.
2. Or, running your app in an [Octane](https://www.nginx.com/resources/glossary/nginx/)-flavored web server: FrankenPHP, RoadRunner, Swoole.
3. Building static assets using [Vite](https://laravel.com/docs/10.x/vite#introduction) or [Mix](https://laravel-mix.com/) 
4. Running the [Scheduler](https://laravel.com/docs/10.x/scheduling#running-the-scheduler) for your background tasks
5. Generating [Fly.io](https://fly.io/) relevant scripts essential for deploying your application as a Laravel Fly App, if applicable

The Dockerfile generated uses the base image published from https://github.com/fly-apps/laravel-docker/tree/main. Every feature from the Docker image published from that repository, is of course, by nature of things-passed-over to a dependent, also available in the Dockerfile generated by this project ( unless overridden ).

### Installation

#### Local
Run `composer require fly-apps/dockerfile-laravel` in the Laravel project's base directory. After a successful installation, you should now have the binary available at your project's bin directory: `vendor/bin/dockerfile-laravel`. 

#### Global 
Run `composer global require fly-apps/dockerfile-laravel`. After a successful installation, you should now have the binary available at your composer's, bin directory: `<path-to-composer-home>/vendor/bin/dockerfile-laravel`. You can view the path to your composer's home directory by checking with `composer -n config --global home`.

### Aliasing 
Once dockerfile-laravel has successfully been installed, you can create an alias for it by running: `alias dockerfile-laravel='/path-to/vendor/bin/dockerfile-laravel'`. Aliasing will allow you to run `dockerfile-laravel` instead of including the full path to your dockerfile-laravel installation.

If you want to make this package available even outside of your current directory, you can add it to your shell configuration file in your home directory, such as `~/.zshrc` or `~/.bashrc`, like so:

```
alias dockerfile-laravel="/path-to/vendor/bin/dockerfile-laravel"
```
Restart your terminal afterwards, and you should be able to access the `dockerfile-laravel` globally, thanks to this alias saved in your shell configuration.

### Generating the Dockerfile

Now that you have the dockerfile-laravel installed, you can use it to generate a Dockerfile for your Laravel application. Simply run the `generate` command of the package:

```
# Locally run:
vendor/bin/dockerfile-laravel generate

# or, with Alias configured:
dockerfile-laravel generate
```

And that should generate a Dockerfile at the current directory the command was run in.

### Testing out the Dockerfile
With your Dockerfile generated, you should now be able to build this Dockerfile into a Docker image: 

```
docker build -t <image-name> . 
```

You can run an instance of this image afterwards:

```
docker run -p <port-in-your-machine>:8080 <image-name>
```

And access a containerized instance of your Laravel app in the browser by visiting `127.0.0.1:<port-in-your-machine>`!

## Local Development

Feeling a little adventurous, and want to make some experiments with the package? You can do some local development if you want.

### Get Familiar with These
Of course, the age-old-question when starting with any new repository: *where does one start*? First, get familiar with the VIP files of this project:
1. [app/Commands/GenerateCommand.php](https://github.com/fly-apps/dockerfile-laravel/blob/d3254571d874b24340d614ff62c931d07495365e/app/Commands/GenerateCommand.php#L8)
    - This is the entrypoint to the application. This is in charge of generating a Dockerfile in a project's base directory based on the dockerfile.blade.php template, and the flags passed to it.

2. [resources/views/dockerfile.blade.php](https://github.com/fly-apps/dockerfile-laravel/blob/d3254571d874b24340d614ff62c931d07495365e/resources/views/dockerfile.blade.php#L1)
    - This is the template used to generate a Dockerfile 


### Finish Set Up
Before diving into making local development changes, there're some to-do's to get you setup with local development on the package:

1. Clone the repository
2. Get the repo dependencies with `composer install`
3. And, just in case you ever find an error about files not being available, you might be able to fix this with providing the proper permission to the box package used by Laravel Zero framework: `chmod 755 vendor/laravel-zero/framework/bin/box`. 

Then, yes. After set up, you can make your changes!

### Testing Changes

Once you've made your changes, you can test them locally by running `php dockerfile-laravel generate`. This will call the command found in `app/Commands/GenerateCommand.php`. 

### Building Changes

Great! You've tested your changes locally. It's time to re-build the stand-alone application for the repository, so that your changes get included in the standalone application that is known as dockerfile-laravel. 

Simply run the build command:

```
php dockerfile-laravel app:build
```
#### Build View Changes 
If changes were made in any of the view files, make sure to clear the cached views( For now, this can be done by manually deleting the cached files ). The path to where the views' caches are stored is configured in `config/view.php`. 

Remember, if you don't delete cached views, your fresh "view changes" are unlikely to be included in the features of the built application.

So, if your view changes don't seem to be working, do delete any cached view files you find, then re-build your changes.

#### Testing the newly-Built Application
Once you've successfully built your changes, you have to test how it turned out. Simply, in the base of any Laravel project of your choosing run the following:
```
<path-to-dockerfile-laravel-dir>/builds/dockerfile-laravel generate
```
And that's it! You should now see a fresh Dockerfile available for your project.

## Contribution

Once you've cooked up and tested your local changes, you can make a Pull Request to include it in this repository. 

Of course, you don't have to create a PR. If there are any feature or bug that makes sense for the repository, you can also create an Issue about it. 

Once submitted, the Laravel team at Fly.io will review your PR/Issue and decide whether it can be successfully added into the official repository. Your contribution will be much appreciated---thank you!
