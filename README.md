Cordova Google Play Games Services + iOS Game Center
====================================================

This plugin enables Cordova app access to a subset of the Google Play Games
Services API and the iOS Game Center API.

iOS support is provided via a dependency on the [Wizcorp Game Center
plugin](<https://github.com/Wizcorp/phonegap-plugin-gameCenter>). The iOS API
calls have been wrapped in a JavaScript shim to match those shown below. See
[this plugin source file](<www/iOSGooglePlayGamesPlugin.js>) for a list of the
available functions in the iOS implementation.

Google Play Game Services Setup
-------------------------------

This plugin includes a variable named `GPSAPPID` for use with Android apps,
which you must define when you install the plugin into your project.

Follow the instructions in [Setting Up Google Play Games
Services](<https://developers.google.com/games/services/console/enabling>) to
link your app in the Google Play Developer Console with your Google Play Game
Services. During this process, you will be provided with an *Application ID*
that you will need when installing this plugin.

>   **NOTE:** do not confuse the Google Play Game Services *Application ID* with
>   your Cordova *Application ID* -- they are not the same IDs.

The `GPSAPPID` is a 12-13 digit numeric prefix to the *OAuth2 Client ID*
associated with your linked game app. Google refers to this prefix number as the
*Application ID*. After you have successfully linked your app to your game
service, you can locate this 12-13 digit number by viewing the details of your
app, inside the *Linked apps* section of the *Game services* panel in the Google
Play Developer Console.

### Short version

Go into the *Google Play Developer Console* \> *Game Services*-\> *Add New Game*
and fill out the information and hit *Save*. On the next page you will see the
title and a number next to it. That number is your `GPSAPPID` (*Application
ID*).

Installing the plugin using Cordova CLI
---------------------------------------

To install this plugin, and its dependencies, into your Cordova CLI project, use
the following command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cordova plugin add https://github.com/01org/cordova-google-play-games-services.git --variable GPSAPPID="_APPID_"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Where you have substituted \_APPID\_ with your app’s *Game Services Application
ID* (see previous section above).

Installing the plugin using the Intel XDK
-----------------------------------------

To install this plugin using the Intel® XDK, you have three options:

1.  Find the plugin in the *Featured Plugins* list using the *Cordova Plugin
    Explorer.*

2.  Use the *Third-Party Plugins* option of the *Cordova Plugin Explorer* and
    fill out the dialog as a *Cordova plugin registry* plugin using
    `com.intel.googleplaygamesplugin` as the plugin ID.

3.  Use the *Third-Party Plugins* option and fill out the dialog as a *Git repo*
    plugin, similar to the following image:

![](<docs/xdk-git-repo-import-dialog.png>)

Where the *123456789012* value is substituted with your app’s *Game Services
Application ID*.

>   **NOTE:** If you leave the *Git Ref* field blank you will retrieve the
>   latest commits on the master branch of that git repo, which may not be what
>   you want. Instead, you should specify a *Tag* or a *Commit* hash from the
>   repo. The repo contains [a list of the available
>   tags](<https://github.com/01org/cordova-google-play-games-services/tags>).

API
---

The plugin's JavaScript API is shown below. All plugin methods are accessed via
a `GooglePlayGamesPlugin` object.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
connect()                      call after deviceready. This connects the native api

authenticate(success,failure)   Authenticate the user against a google play
                                account.  Will call success or failure functions.
                                If fail, a message will be passed from the google play

logout(success,failure)          Sign the user out of Google Play Games Services
                                 for your app

addAchievement(achievement_id,success,failure)  unlock the achievement for the user

showAchievements(success,failure)  Show the achievements for the user.
                                    If there is an error (not logged in),
                                    the message will be passed in.  This is a
                                    new activity that gets launched

updateLeaderboardScore(leaderboard_id,score,success,failure)
                                    Update the score for the selected
                                    leaderboard for the user

showLeaderboardScore(leaderboard_id,success,failure)
                                    Show the leaderboard.
                                    If there is an error (not logged in),
                                    the message will be passed in the error.  This is a
                                    new activity that gets launched


saveGame(id,data)
                            Save a game to local storage.  Id is the key and data is JSON data
                            If no id is specficied, we use only one save game slot named "default"
                            If an entry exists, we overwrite the data

loadSavedGame(id)
                            Load a saved games data.
                            If no id is specified, we load the "default" saved game state.

deleteSavedGame(id)
                            Delete a saved game from local storage.
                            If no id is specified, we delete the "default" saved game state.

getAllSavedGames()
                            Returns an object with all the saved games.
                            The key is the id used to save.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NOTES
-----

Achievements and leaderboards are created in the Google Play Dashboard. They
will give you a string parameter for you to call the functions with.

### Testing

To test your app:

-   build your application and submit it in the Google Play Store as an Alpha
    APK

-   goto `Google Play Developer Console -> Game Services` and link your
    application to the game service you created

-   after your app and game service are linked, enable testing for the app and
    game service

https://developers.google.com/games/services/console/testpub\#enabling\_accounts\_for\_testing

When testing, make sure you add the test user under "Testing" in the game
service you created.

### Adding API's

The APIs provided in this plugin are a small subset of the Google Play Games
Services API, but can easily be extended. Feel free to fork and contribute
extended the APIs. Any APIs that require a new activity, like logging in, should
be implemented in `GooglePlayGamesServices.java`. All other API's can be added
to `GooglePlayGamesPlugin.java`.
