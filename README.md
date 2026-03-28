# gitpack

`gitpack` is a utility that other applications can use to provide one-click installs and auto-update functionality. The core premise is that `gitpack` based applications are installed and updated simply by pulling a git repository. `gitpack` manages the entire repository lifecycle:

- clones the repo and installs the application
- keeps the local copy of the repo updated and re-triggers the applications setup scripts as needed
- provides a facility for removing applications

## Setting up your project to use gitpack

Configuring is as simple as creating a `.gitpack.yml` file in the root of your project:

```
# .gitpack.yml
name: my-app                # The name your app will be installed as
entrypoint: bin/my-app.sh   # The executable to start your app
setup: bin/setup.sh         # A setup script to install dependencies
```

The executable defined by `setup` should be idempotent. It will be run both on initial install, but also any time the application is self-updated.

The executable defined by `entrypoint` will be symlinked into `~/.local/bin/<name>` (where name also comes from the config file), to make your application available to your users.

## Creating a One-Click Install Command

There are two options for creating an install script for a `gitpack` based application:

### Option 1: Shared hosting (convenient)

This method uses an official hosted copy of `gitpack` to handle the install. Simply replace `[Git URL]` with the URL of your projects repository, and this command as-is will install it:

`curl -fsSL https://gitpack.htlc.io | sh -s -- install [Git URL]`

### Option 2: Vendored hosting (more secure)

If you don't want to trust the official hosted copy of gitpack, you can easily vendor it yourself in a variety of ways: 
- fork this repo
- clone this repo as a submodule
- download the raw `gitpack` file

Regardless of the method you choose, you just need to make gitpack available at a public URL. From there, the one-click install command takes the form:

`curl -fsSL [vendored gitpack url] | sh -s -- install [Git URL]`

Make sure to configure the above command by replacing the content in square brackets:
- `[vendored gitpack url]` is the public URL that hosts gitpack.
- `[Git URL]` is the public git repo where your project can be cloned from.
