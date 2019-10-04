# Zapier CLI Reference

These are the generated docs for all Zapier platform CLI commands.

You can install the CLI with `npm install -g zapier-platform-cli`.

```bash
$ npm install -g zapier-platform-cli
```

# Commands

## build

  > Builds a pushable zip from the current directory.

  **Usage:** `zapier build`

  
Builds a ready-to-upload zip file, but does not upload / push the zip file. Generally you'd use `zapier push` which does this and `zapier upload` together.

It does the following steps:

* Creates a temporary folder
* Copies all code into the temporary folder
* Adds an entry point `zapierwrapper.js`
* Generates and validates app definition.
* Detects dependencies via browserify (optional)
* Zips up all needed `.js` files. If you want to include more files, add a "includeInBuild" property (array with strings of regexp paths) to your `.zapierapprc`.
* Moves the zip to `build/build.zip` and `build/source.zip`

> If you get live errors like `Error: Cannot find module 'some-path'`, try disabling dependency detection.

**Arguments**


* `--disable-dependency-detection` -- _optional_, disables walking required files to slim the build
* `--include-js-map` -- _optional_, include .js.map files (usually source maps)

```bash
$ zapier build
# Building project.
#
#   Copying project to temp directory - done!
#   Installing project dependencies - done!
#   Applying entry point file - done!
#   Validating project - done!
#   Building app definition.json - done!
#   Zipping project and dependencies - done!
#   Cleaning up temp directory - done!
#
# Build complete!
```


## collaborate

  > Manage the admins on your project. Can optionally --remove.

  **Usage:** `zapier collaborate [user@example.com]`

  
Give any user registered on Zapier the ability to collaborate on your app. Commonly, this is useful for teammates, contractors, or other developers who might want to make changes on your app. Only admin access is supported. If you'd only like to provide read-only or testing access, try `zapier invite`.

**Arguments**

* _none_ -- print all admins
* `email [user@example.com]` -- _optional_, which user to add/remove
* `--remove` -- _optional_, elect to remove this user
* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier collaborate
# The admins on your app "Example" listed below.
#
# ┌──────────────────┬───────┬──────────┐
# │ Email            │ Role  │ Status   │
# ├──────────────────┼───────┼──────────┤
# │ user@example.com │ admin │ accepted │
# └──────────────────┴───────┴──────────┘

$ zapier collaborate user@example.com
# Preparing to add admin user@example.com to your app "Example".
#
#   Adding user@example.com - done!
#
# Admins updated! Try viewing them with `zapier collaborate`.

$ zapier collaborate user@example.com --remove
# Preparing to remove admin user@example.com from your app "Example".
#
#   Removing user@example.com - done!
#
# Admins updated! Try viewing them with `zapier collaborate`.
```


## convert

  > Converts a Legacy Web Builder or Visual Builder app to a CLI app.

  **Usage:** `zapier convert appid path`

  
Creates a new CLI app from an existing app.

If you're converting a **Legacy Web Builder** app: the new app contains code stubs only. It is supposed to get you started - it isn't going to create a complete app!

After running this, you'll have a new app in your directory, with stubs for your trigger and actions.  If you re-run this command on an existing directory it will leave existing files alone and not clobber them.

Once you've run the command, make sure to run `zapier push` to see it in the editor.

If you're converting a **Visual Builder** app, then it will be identical and ready to push and use immediately! You'll need to do a `zapier push` before the new version is visible in the editor, but otherwise you're good to go.

**Arguments**

* `appid [value]` -- **required**, Get the appid from "https://zapier.com/app/developer", clicking on an integration, and taking the number after "/app" in the url.
* `location [value]` -- **required**, Relative to your current path - IE: `.` for current directory
* `--version=1.0.0` -- _optional_, Convert a specific version. Required when converting a Visual Builder app

```bash
$ zapier convert 1234 .
# Let's convert your app!
#
#   Downloading app from Zapier - done!
#   Writing triggers/trigger.js - done!
#   Writing package.json - done!
#   Writing index.js - done!
#   Copy ./index.js - done!
#   Copy ./package.json - done!
#   Copy ./triggers/trigger.js - done!
#
# Finished! You might need to `npm install` then try `zapier test`!
```


## delete

  > Delete a version of your app (or the whole app) as long as it has no users/Zaps.

  **Usage:** `zapier delete version 1.0.0`

  
A utility to allow deleting app versions that aren't used.

> The app version needs to have no users/Zaps in order to be deleted.

**Arguments**

* `appOrVersion [{app,version}]` -- **required**, delete the whole app, or just a version?
* `version [1.0.0]` -- _optional_, the version to delete


```bash
$ zapier delete version 1.0.0
# Preparing to delete version 1.0.0 of your app "Example".
#
#   Deleting 1.0.0 - done!
#   Deletion successful!
```


## describe

  > Describes the current app.

  **Usage:** `zapier describe`

  
Prints a human readable enumeration of your app's triggers, searches, and actions as seen by Zapier. Useful to understand how your resources convert and relate to different actions.

> These are the same actions we'd display in our editor!

* `Noun` -- your action's noun
* `Label` -- your action's label
* `Resource` -- the resource (if any) this action is tied to
* `Available Methods` -- testable methods for this action

**Arguments**



* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier describe
# A description of your app "Example" listed below.
#
# Triggers
#
# ┌────────────┬────────────────────┬──────────────┬───────────────────────────────────────────────┐
# │ Noun       │ Label              │ Resource Ref │ Available Methods                             │
# ├────────────┼────────────────────┼──────────────┼───────────────────────────────────────────────┤
# │ Member     │ Updated Subscriber │ member       │ triggers.updated_member.operation.perform     │
# │            │                    │              │ triggers.updated_member.operation.inputFields │
# │            │                    │              │ resources.member.list.operation.perform       │
# │            │                    │              │ resources.member.list.operation.inputFields   │
# └────────────┴────────────────────┴──────────────┴───────────────────────────────────────────────┘
#
# Searches
#
#  Nothing found for searches, maybe try the `zapier scaffold` command?
#
# Creates
#
#  Nothing found for creates, maybe try the `zapier scaffold` command?
#
# If you'd like to add more, try the `zapier scaffold` command to kickstart!
```


## env

  > Read, write, and delete environment variables.

  **Usage:** `zapier env 1.0.0 CLIENT_SECRET 12345`

  
Manage the environment of your app so that `process.env` has the necessary variables, making it easy to match a local environment with a production environment via `CLIENT_SECRET=12345 zapier test`.

**Arguments**

* `version [1.0.0]` -- **required**, the app version's environment to work on
* `key [CLIENT_SECRET]` -- _optional_, the uppercase key of the environment variable to set
* `value [12345]` -- _optional_, the raw value to set to the key
* `--remove` -- _optional_, optionally remove environment variable with this key
* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier env 1.0.0
# The env of your "Example" listed below.
#
# ┌─────────┬───────────────┬───────┐
# │ Version │ Key           │ Value │
# ├─────────┼───────────────┼───────┤
# │ 1.0.0   │ CLIENT_SECRET │ 12345 │
# └─────────┴───────────────┴───────┘
#
# Try setting an env with the `zapier env 1.0.0 CLIENT_SECRET 12345` command.

$ zapier env 1.0.0 CLIENT_SECRET 12345
# Preparing to set environment CLIENT_SECRET for your 1.0.0 "Example".
#
#   Setting CLIENT_SECRET to "12345" - done!
#
# Environment updated! Try viewing it with `zapier env 1.0.0`.

$ zapier env 1.0.0 CLIENT_SECRET --remove
# Preparing to remove environment CLIENT_SECRET for your 1.0.0 "Example".
#
#   Deleting CLIENT_SECRET - done!
#
# Environment updated! Try viewing it with `zapier env 1.0.0`.
```


## help

  > Lists all the commands you can use.

  **Usage:** `zapier help [command]`

  
Prints documentation to the terminal screen.

Generally - the `zapier` command works off of two files:

 * ~/.zapierrc      (home directory identifies the deploy key & user)
 * ./.zapierapprc   (current directory identifies the app)

The `zapier login` and `zapier register "Example"` or `zapier link` commands will help manage those files. All commands listed below.

**Arguments**

* _none_ -- print all commands
* `cmd [value]` -- _optional_, the command to view docs for
* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier help apps
$ zapier help scaffold
$ zapier help
# Usage: zapier COMMAND [command-specific-arguments] [--command-specific-options]
#
# ┌─────────────┬───────────────────────────────────────┬────────────────────────────────────────────────────────────────────────────┐
# │ Command     │ Example                               │ Help                                                                       │
# ├─────────────┼───────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────┤
# │ apps        │ zapier apps                           │ Lists all the apps you can access.                                         │
# │ build       │ zapier build                          │ Builds a uploadable zip from the current directory.                        │
# │ collaborate │ zapier collaborate [user@example.com] │ Manage the admins on your project. Can optionally --remove.         │
# │ push        │ zapier push                           │ Build and upload the current app - does not promote.                       │
# │ deprecate   │ zapier deprecate 1.0.0 2017-01-20     │ Mark a non-production version of your app as deprecated by a certain date. │
# │ describe    │ zapier describe                       │ Describes the current app.                                                 │
# │ env         │ zapier env 1.0.0 CLIENT_SECRET 12345  │ Read and write environment variables.                                      │
# │ help        │ zapier help [command]                 │ Lists all the commands you can use.                                        │
# │ history     │ zapier history                        │ Prints all recent history for your app.                                    │
# │ init        │ zapier init path                      │ Initializes a new zapier app in a directory.                               │
# │ invite      │ zapier invite [user@example.com]      │ Manage the invitees/testers on your project. Can optionally --remove.      │
# │ link        │ zapier link                           │ Link the current directory to an app you have access to.                   │
# │ login       │ zapier login                          │ Configure your `~/.zapierrc` with a deploy key.                            │
$ │ logout      │ zapier logout                         │ Deactivates all your personal deploy keys and resets `~/.zapierrc`.        │
# │ logs        │ zapier logs                           │ Prints recent logs. See help for filter arguments.                         │
# │ migrate     │ zapier migrate 1.0.0 1.0.1 [10%]      │ Migrate users from one version to another.                                 │
# │ promote     │ zapier promote 1.0.0                  │ Promotes a specific version to public access.                              │
# │ register    │ zapier register "Example"             │ Registers a new app in your account.                                       │
# │ scaffold    │ zapier scaffold resource "Contact"    │ Adds a starting resource, trigger, action or search to your app.           │
# │ test        │ zapier test                           │ Tests your app via `npm test`.                                             │
# │ upload      │ zapier upload                         │ Upload the last build as a version.                                        │
# │ validate    │ zapier validate                       │ Validates the current project.                                             │
# │ versions    │ zapier versions                       │ Lists all the versions of the current app.                                 │
# └─────────────┴───────────────────────────────────────┴────────────────────────────────────────────────────────────────────────────┘
```


## invite

  > Manage the invitees/testers on your project. Can optionally specify a version or --remove.

  **Usage:** `zapier invite [user@example.com] [1.0.0]`

  
Invite any user registered on Zapier to test your app. Commonly, this is useful for teammates, contractors, or other team members who might want to test, QA, or view your app versions. If you'd like to provide full admin access, try `zapier collaborate`.

> Send an email directly, which contains a one-time use link for them only - or share the public URL to "bulk" invite folks!

**Arguments**

* _none_ -- print all invitees
* `email [user@example.com]` -- _optional_, which user to add/remove
* `version [1.0.0]` -- _optional_, only invite to a specific version
* `--remove` -- _optional_, elect to remove this user
* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier invite
# The invitees on your app listed below.

# ┌───────────────────┬──────────┬──────────┬─────────┐
# │ Email             │ Role     │ Status   │ Version │
# ├───────────────────┼──────────┼──────────┼─────────┤
# │ user@example.com  │ invitees │ accepted │ 1.0.0   │
# └───────────────────┴──────────┴──────────┴─────────┘
#
# Don't want to send individual invite emails? Use this public link and share with anyone on the web:
#
#   https://zapier.com/platform/public-invite/1/222dcd03aed943a8676dc80e2427a40d/

$ zapier invite user@example.com 1.0.0
# Preparing to add invitee user@example.com to your app "Example (1.0.0)".
#
#   Adding user@example.com - done!
#
# Invitees updated! Try viewing them with `zapier invite`.

$ zapier invite user@example.com --remove
# Preparing to remove invitee user@example.com from your app "Example".
#
#   Removing user@example.com - done!
#
# Invitees updated! Try viewing them with `zapier invite`.
```


## link

  > Link the current directory to an app you have access to.

  **Usage:** `zapier link`

  
Link the current directory to an app you have access to. It is fairly uncommon to run this command - more often you'd just `git clone git@github.com:example-inc/example.git` which would have a `.zapierapprc` file already included. If not, you'd need to be an admin on the app and use this command to regenerate the `.zapierapprc` file.

Or, if you are making an app from scratch - you should prefer `zapier init`.

> This will change the `./.zapierapprc` (which identifies the app assosciated with the current directory).

**Arguments**



* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier link
# Which app would you like to link the current directory to?
#
# ┌────────┬─────────────┬────────────┬─────────────────────┬────────┐
# │ Number │ Title       │ Unique Key │ Timestamp           │ Linked │
# ├────────┼─────────────┼────────────┼─────────────────────┼────────┤
# │ 1      │ Example     │ Example    │ 2016-01-01T22:19:28 │ ✔      │
# └────────┴─────────────┴────────────┴─────────────────────┴────────┘
#      ...or type any title to create new app!
#
# Which app number do you want to link? You also may type a new app title to create one. (Ctrl-C to cancel)
#
  1
#
#   Selecting existing app "Example" - done!
#   Setting up `.zapierapprc` file - done!
#
# Finished! You can `zapier push` now to build & upload a version!
```


## logs

  > Prints recent logs. See help for filter arguments.

  **Usage:** `zapier logs`

  
Get the logs that are automatically collected during the running of your app. Either explicitly during `z.console.log()`, automatically via `z.request()`, or any sort of traceback or error.

> Does not collect or list the errors found locally during `zapier test`.

**Arguments**


* `--version=value` -- _optional_, display only this version's logs (default is all versions)
* `--status={any,success,error}` -- _optional_, display only success logs (status code < 400 / info) or error (status code > 400 / tracebacks). Default is `any`
* `--type={console,bundle,http}` -- _optional_, display only console, bundle, or http logs. Default is `console`
* `--detailed` -- _optional_, show detailed logs (like request/response body and headers)
* `--user=user@example.com` -- _optional_, display only this user's logs. Default is `me`
* `--limit=50` -- _optional_, control the maximum result size. Default is `50`
* `--format={plain,json,raw,row,table}` -- _optional_, display format. Default is `table`
* `--help` -- _optional_, prints this help text
* `--debug` -- _optional_, print debug API calls and tracebacks

```bash
$ zapier logs
# The logs of your app "Example" listed below.
#
# ┌──────────────────────────────────────────────────────┐
# │ = 1 =                                                │
# │     Log       │ console says hello world!            │
# │     Version   │ 1.0.0                                │
# │     Step      │ 99c16565-1547-4b16-bcb5-45189d9d8afa │
# │     Timestamp │ 2016-01-01T23:04:36-05:00            │
# └───────────────┴──────────────────────────────────────┘

$ zapier logs --type=http
# The logs of your app "Example" listed below.
#
# ┌────────────────────────────────────────────────────────┐
# │ = 1 =                                                  │
# │     Status      │ 200                                  │
# │     URL         │ https://httpbin.org/get               │
# │     Querystring │ hello=world                          │
# │     Version     │ 1.0.0                                │
# │     Step        │ 99c16565-1547-4b16-bcb5-45189d9d8afa │
# │     Timestamp   │ 2016-01-01T23:04:36-05:00            │
# └─────────────────┴──────────────────────────────────────┘
```


## push

  > Build and upload the current app - does not promote.

  **Usage:** `zapier push`

  
A shortcut for `zapier build && zapier upload` - this is our recommended way to push an app. This is a common workflow:

1. Make changes in `index.js` or other files.
2. Run `zapier test`.
3. Run `zapier push`.
4. QA/experiment in the Zapier.com Zap editor.
5. Go to 1 and repeat.

> Note: this is always a safe operation as live/production apps are protected from pushes. You must use `zapier promote` or `zapier migrate` to impact live users.

> Note: this command will create (or overwrite) an AppVersion that matches the ones listed in your `package.json`. If you want to push to a new version, increment the "version" key in `package.json`.

If you have not yet registered your app, this command will prompt you for your app title and to register the app.

```bash
$ zapier push
# Preparing to build and upload app.
#
#   Copying project to temp directory - done!
#   Installing project dependencies - done!
#   Applying entry point file - done!
#   Validating project - done!
#   Building app definition.json - done!
#   Zipping project and dependencies - done!
#   Cleaning up temp directory - done!
#   Uploading version 1.0.0 - done!
#
# Build and upload complete! Try loading the Zapier editor now, or try `zapier promote` to put it into rotation or `zapier migrate` to move users over
```


## register

  > Registers a new app in your account.

  **Usage:** `zapier register "Example"`

  
This command registers your app with Zapier. After running this, you can run `zapier push` to push a version of your app that you can use in the Zapier editor.

> This will change the  `./.zapierapprc` (which identifies the app associated with the current directory).

**Arguments**

* `title ["My App Name"]` -- **required**,


```bash
$ zapier register "Example"
# Let's register your app "Example" on Zapier!
#
#   Creating a new app named "Example" on Zapier - done!
#   Setting up .zapierapprc file - done!
#   Applying entry point file - done!
#
# Finished!
```


## scaffold

  > Adds a starting resource, trigger, action or search to your app.

  **Usage:** `zapier scaffold {resource|trigger|search|create} "Name"`

  
The scaffold command does two general things:

* Creates a new destination file like `resources/contact.js`
* (Attempts to) import and register it inside your entry `index.js`

You can mix and match several options to customize the created scaffold for your project.

> Note, we may fail to rewrite your `index.js` so you may need to handle the require and registration yourself.

**Arguments**

* `type [{resource,trigger,search,create}]` -- **required**, what type of thing are you creating
* `name ["Some Name"]` -- **required**, the name of the new thing to create
* `--dest={type}s/{name}` -- _optional_, sets the new file's path. Default is `{type}s/{name}`
* `--entry=index.js` -- _optional_, where to import the new file. Default is `index.js`

```bash
$ zapier scaffold resource "Contact"
$ zapier scaffold resource "Contact" --entry=index.js
$ zapier scaffold resource "Contag Tag" --dest=resources/tag
$ zapier scaffold resource "Tag" --entry=index.js --dest=resources/tag
# Adding resource scaffold to your project.
#
#   Writing new resources/tag.js - done!
#   Rewriting your index.js - done!
#
# Finished! We did the best we could, you might gut check your files though.
```


## upload

  > Upload the last build as a version.

  **Usage:** `zapier upload`

  
Upload the zip files already built by `zapier build` in build/build.zip and build/source.zip. The version and other app details are read by Zapier from the zip files.

> Note: we generally recommend using `zapier push` which does both `zapier build && zapier upload` in one step.

```bash
$ zapier upload
# Preparing to upload a new version.
#
#   Uploading version 1.0.0 - done!
#
# Upload of build/build.zip and build/source.zip complete! Try `zapier versions` now!
```