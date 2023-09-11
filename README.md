# STREAMA

Ever had a huge bookshelf full of shows and movies? Ever wanted to digitalize them, but didn't have a good way of organizing the files? Worry no more! Streama is your own personal streaming app for just such a purpose! 

## Live-Demo
https://streama.demo-version.net/   
**credentials: username: demoUser | password: demoUser**  
Feel free to play around there as much as you like (everything is reset at night)  
We will keep this demo updated with our newest releases as they come, so that you can check out new features there first before deciding to deploy them in your own environments. 


## Table of contents:
- [The Application](#the-application)
  - [Settings](#settings)
  - [The Dashboard](#the-dashboard)
  - [The Player](#the-player)
  - [The Admin-Panel](#the-admin-panel)
  - [Accessing existing files](#accessing-existing-files)
  - [The Users](#the-users) 
- [Technical Details](#technical-details)
# The Application

### Settings
![Streama Settings Validation](http://i.imgur.com/oEMXLPk.gif)

When you first run the application, you will be redirected to the settings page. Here you enter your desired upload directory for the video files, your [theMovieDb.org API key](https://www.themoviedb.org/documentation/api) and a different base URL if so desired (useful for remote hosting).

Once you made adjustments to any of the settings, make sure to validate the value before saving.
- For the API-Key, the validation checks against theMovieDb.org to see if you've entered a valid API-key.
- For the upload directory, the application checks if it has read/write permissions for it.

### The Dashboard
![Streama Dashboard](http://new.tinygrab.com/d9072ef564654c6e245c442e9c7d95facd4b738538.png)

On the dashboard, a user can see their recently watched TV-shows and Movies and their progress (they can continue where they left off) as well start new shows and movies that they haven't yet seen. The "Continue Watching" Feature works by periodically updating the database (only while watching, of course!) with info about the currently watched Video and how far it has been seen.

If a Movie or Episode does not contain any video-files, it won't show up in the dashboard.

### The Player
![Streama Player](http://new.tinygrab.com/d9072ef56407e5d1ac40fab040aedc398a9abb3609.png)

The Streama-Player is (heavily) inspired by Netflix, so you get all the good stuff from there. For Shows, there is a "next episode" button and a handy episode/season browser. There are also the basics: volume-control, play/pause, and fullscreen. 
Later down the road I will add a feature to add subtitles and switch between video-files (for instance for different quality uploads). 
The player is HTML5-based and has only really been tested in Chrome so far.

##### The Episode Browser
I am especially proud of the Episode-Browser, which aims to function just like on Netflix. By default, the current video files season is selected. The user gets an overview of which other episodes there are in the season, how many seasons there are, and, as an added feature, the user sees all the added episodes, even if no video-files are added to them (thus greyed out).

![Streama Episode Browser](http://i.imgur.com/MLE6TpH.gif)

### The Admin-Panel
![Streama Admin](http://new.tinygrab.com/d9072ef56484ebb444cc2fc7bc11f18e9f1706f68f.png)

One of the most important things to me was to make managing shows, movies, and episodes as easy and fun as possible. For this, I made heavy use of the API from [theMovieDatabase.org](https://www.themoviedb.org/), which auto-fills the episodes, shows and movies with useful information and great images. This eases the user's role in adding content.

For example, creating a new TV-show and the episodes for the first season looks something like this:

![Streama Creating Show](http://i.imgur.com/TLptKdp.gif)

Uploading video-files for each episode is as easy as drag-and-drop!

![Streama Uploading Episode](http://i.imgur.com/StgES0S.gif)  

### Accessing existing files
If you want to avoid uploading every file, use our new and improved "Local File" Feature instead. You can define a local directory (I use one directly in which I created symlinks to all my mounted drives) and then you can either use the local file browser for individual movies / tv-Shows or you can use the bulk-create feature. 

#### The local file browser
You can access the local file browser from any movie or TV-show (anywhere where you would otherwise upload a file). Note: You need to first define the local directory in the settings.
![Movie Detail](https://files.gitter.im/dularion/streama/2Al4/image.png)
![Local File directory](https://files.gitter.im/dularion/streama/wY2h/image.png)

#### Bulk-Create from File
This MR addresses the issue "Batch Add Files #241".
Note: the TV-Show does not have to be present in order for this to work. Everything will be created by the backend. 

##### Running matcher & previewing the result
![sep-02-2017 01-24-34](https://user-images.githubusercontent.com/936076/29990709-8f34e8f8-8f7d-11e7-9d9b-955236bf2251.gif)

##### adding single matched file 
![sep-02-2017 01-24-12](https://user-images.githubusercontent.com/936076/29990718-a28e2af4-8f7d-11e7-9821-eba309b04011.gif)

##### adding files in bulk
![sep-02-2017 01-24-26](https://user-images.githubusercontent.com/936076/29990720-aa6dcb08-8f7d-11e7-8f1c-59cdadee10d6.gif)


##### Customizing the Matcher
Just like in Emby or Kodi, the matcher-regex can be altered. The two defaults are 
**Movie**: `/^(?<Name>.*)[_.]\(\d{4}\).*/`  
**TvShow/Episode**: `/^(?<Name>.+)[._]S(?<Season>\d{2})E(?<Episode>\d{2,3}).*/`   

In order to customize the regex, just add the regex in the bottom of the application.yml like so:
```yml
streama:
  regex:
    movies: ^(?<Name>.*)[_.]\(\d{4}\).*
    shows: ^(?<Name>.+)[._]S(?<Season>\d{2})E(?<Episode>\d{2,3}).*

```


### The Users
![Streama User Management](http://new.tinygrab.com/d9072ef564717c22dde948c726144b1b707a607adc.png)
Users can be invited and managed in the admin panel. By default, they are non-admins, meaning they can only view videos, not create them. You can make them admins with the press of a button. Since there is user-administration in place, I plan on expanding on this a lot! Another feature I want to add is the ability for users to add and administer some form of playlists. There is a lot of potentials to make this even better!

# Technical Details
This application is web-based, the server-side is written on [Grails 3](https://grails.org/) with [SpringSecurity](http://projects.spring.io/spring-security/) for login & user-handling. For all the front-end components, [AngularJS](https://angularjs.org/) is used and the player is completely HTML5-based. The application is essentially split into Grails for a REST-API, and AngularJS for the frontend.

Streama uses the [awesome API](https://www.themoviedb.org/documentation/api) from [theMovieDatabase](https://www.themoviedb.org) for all media-metadata.
