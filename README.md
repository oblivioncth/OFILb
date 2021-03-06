# OFILb (Obby's Flashpoint Importer for LaunchBox)
OFILb (pronounced "Awful-B") is an importer tool for [LaunchBox](https://www.launchbox-app.com/) that allows one to add platforms and playlists from [BlueMaxima's Flashpoint](https://bluemaxima.org/flashpoint/) project to their collection. It is fully automated and only requires the user to provide the paths to the LaunchBox/Flashpoint installs, choose which Platforms/Playlists they wish to import, and select between a few import mode options. Once the import is started the current progress is displayed and any errors that occur are shown to the user, with resolvable errors including a prompt for what the user would like to do. After the process has completed LaunchBox can be started and the games from Flashpoint can be played like those from any other Platform.

For Platforms, the importer is capable of importing each game/animation along with any additional apps, images, and most of the metadata fields (i.e. Title, Description, etc, see below).

## Function
This utility makes use of its sister project [CLIFp (Command-line Interface for Flashpoint)](https://github.com/oblivioncth/CLIFp) to allow LaunchBox to actually start and exit the games correctly. It is automatically deployed into your Flashpoint installation (updated if necessary) at the end of a successful import and the latest version of CLIFp will be included in each release of this utility so it is not generally something the end-user needs to concern themselves with.

Before making any changes to your LaunchBox collection any XML files that will be altered are automatically backed up (only one backup is maintained at once so any previous backup will be overwritten) and if any unrecoverable errors occur during the import any partial changes are reverted and the backups are restored; however, while LaunchBox itself also makes periodic backups of your XML data **it is strongly suggested that  you consider making a manual backup of your LaunchBox\Data folder to be safe.** No responsibility is held for the loss of data due to use of this tool.

OFILb can safely be used multiple times on the same collection to update the selected Platforms and Playlists if that have already been imported previously. The method with which to handle existing entries is selected within the program before each import.

The import time will vary, correlated with how many Platforms/Playlists you have selected, but more significantly the image mode you choose, which is expanded on later. Importing the entire collection usually takes 5-10 minutes with the recommended settings but can take longer with a more basic PC. The vast majority of the processing time is due to the plethora of images that have to be copied/symlinked when games processed so the speed of your storage device is the most significant factor. Running the importer for updates should be significantly faster it first checks to see if the source image from the new import source is actually different than your current one before copying/linking it.

You will still be able to use the standard Flashpoint launcher as normal after completing an import.

# Compatability
### Flashpoint Infinity/Flashpoint Ultimate
This tool was made with the express purpose of using it with Flashpoint Ultimate (i.e. all games/animations pre-downloaded), but since the 0.2 rewrite of CLIFp it should work with Infinity as well. Just note that use with Infinity is less rigorously tested.

### General
While testing for 100% compatibility is infeasible given the size of Flashpoint, OFILb was designed with full compatibility in mind.

The ":message:" feature of Flashpoint, commonly used to automatically show usage instructions for some games before they are started, is supported. The entries that use it are added as additional-apps to their respective games as they once were when Flashpoint came packaged with LaunchBox. All messages are displayed in a pop-up dialog via CLIFp.

Viewing extras (which are simply a folder) is also supported and the corresponding additional apps that open these folders will be added when importing a platform.

Since Flashpoint originally used LaunchBox as its launcher, most fields within Flashpoint have a one-to-one equivalent (or close enough equivalent) LaunchBox field. That being said there are a few fields that are unique to Flashpoint that do not have matching field and so they are simply excluded during the import, resulting in a relatively minor loss of information for each game in your collection.

### Version Matching
Each release of this application targets a specific version or versions of BlueMaxima's Flashpoint and while newer releases will sometimes contain general improvements to functionality, they will largely be created to match the changes made between each Flashpoint release and therefore maintain compatibility. These matches are shown below:
| OFILb Version  |Included CLIFp Version | Target Flashpoint Version       |
|----------------|-----------------------|---------------------------------|
| 0.1            | 0.1                   | 8.1 ("Spirit of Adventure")     |
| 0.1.1          | 0.1.1                 | 8.2 ("Approaching Planet Nine") |
| 0.1.2, 0.1.2.1 | 0.3                   | 8.2 ("Approaching Planet Nine") |
| 0.1.3          | 0.3.2                 | 9.0 ("Glorious Sunset")         |
| 0.3, 0.3.0.1   | 0.4                   | 9.0 ("Glorious Sunset")         |

Using a version of OFILb that does not target the version of Flashpoint you wish to use it with is highly discouraged as some features may not work correctly or at all and in some cases the utility may fail to function entirely or even damage the Flashpoint install it is used with.

### Metadata Fields

Currently the following fields in LaunchBox will be populated for each game, which is limited by what is available within Flashpoint:

- Title
- Series
- Developer
- Publisher
- Platform
- Sort Title
- Date Added
- Date Modified
- Broken Flag
- Play Mode
- Status
- Region
- Notes
- Source
- Release Date
- Version
- Library

## Usage
### Primary Usage
 1. Ensure Flashpoint and LaunchBox are both not running
 2. Manually specify or browse for the path to your LaunchBox install, the utility will let you know if there are any problems. If everything is OK the icon next to the install path will change to a green check
 3. Manually specify or browse for the path to your Flashpoint install, the utility will let you know if there are any problems. If everything is OK the icon next to the install path will change to a green check
 4. The lists of available Platforms and Playlists will quickly load
 5. Select which Platforms and Playlists you want to import. Existing entries that are considered an update will be highlighted in green
 6. If importing Playlists, select a Playlist Game Mode. These are described with the nearby Help button in the program, but here is a basic overview of their differences:
	 - **Selected Platforms Only** - Only games that are present within the selected platforms will be included
	 - **Force All** - All games in the playlist will be included, importing portions of unselected platforms as required
 7. If any entries you have selected are for updates you may select update mode settings. These are described with the nearby Help button in the program, but here is a basic overview of their differences:
    - (Exclusive) **New Only** - Only adds new games
    - (Exclusive) **New & Existing** - Adds new games and updates the non-user specific metadata for games already in your collection
    - (Applies to either of the above) **Remove Missing** - Removes any games from your collection for the selected Platforms that are no longer in Flashpoint
 8. Select a method to handle game images. These are described with the nearby Help button in the program, but here is a basic overview of their differences:
    - **Copy** - Copies all relevant images from Flashpoint into your LaunchBox install (slow import)
    - **Reference** - Changes your LaunchBox install configuration to directly use the Flashpoint images in-place (slow image refresh)
    - **Symlink** - Creates a symbolic link to all relevant images from Flashpoint into your LaunchBox install. Overall the best option

 9. Press the "Start Import" button

The symbolic link related options for handling images require the importer to be run as an administrator or for you to enable [Developer mode](https://www.howtogeek.com/292914/what-is-developer-mode-in-windows-10/#:~:text=How%20to%20Enable%20Developer%20Mode,be%20put%20into%20Developer%20Mode.) within Windows 10

**Example:**

![OFILb Example Usage](https://i.imgur.com/OuKuThO.png)

### Other Features
 - If for whatever reason you want to only deploy or update CLIFp there is an option for doing so in the Tools menu
 - You can select whether or not you want to include "Explicit" games in each import session using the relevant check-able option in the Tools menu
 - Animations are not included by default since LaunchBox is more games oriented; however, you can choose to include them using the relevant check-able option in the Tools menu
 - The playlist import feature is "smart" in the sense that it won't include games that you aren't importing. So if you only want to import the Flash platform for example and a couple playlists, you wont have to worry about useless entries in the playlist that point to games from other platforms you didn't import. This of course does not apply if you are using the "Force All" playlist game mode.
 
## Limitations
 - Although general compatibility is quite high, compatibility with every single title cannot be assured. Issues with a title or group of titles will be fixed as they are discovered.
 - The "smart" feature of the Playlist import portion of the tool has the drawback that only games that were included in the same import will be considered for that playlist. If you previously imported a Platform and now want to import a Playlist that contains games from that Platform you must make sure you select it again for it to be updated/re-imported in order for those games to be added to that Playlist. Alternatively, you can use the "Force All" playlist game mode, but this will also possibly add new platforms you did not previously import.
- If you are using Infinity you will be able to play any game that is imported, even if it hasn't been played yet in Flashpoint; images for the games however will not be present until they've been seen/loaded in Flashpoint at least once and require the importer to be ran again afterwards. The requirement to have the images load in Flashpoint's launcher first is one I can do nothing about, but I hope to eventually make it so you don't need to run the importer again if you're using the LaunchBox symlink option for images.

## Source
This tool was written in C++ 17 along with Qt 5 and currently only targets Windows Vista and above; however, this tool can easily be ported to Linux with minimal changes, though to what end I am not sure since this is for a Windows application. The source includes an easy-to-use .pro file if you wish to build the application in Qt Creator and the available latest release was compiled in Qt Creator using MSVC 2019 and a static compilation of Qt 5.15.0. Other than a C++ 17 capable compiler and Qt 5.15.x+ all files required to compile this software are included, with the exception of a standard make file.

All functions/variables under the "Qx" (QExtended) namespace belong to a small, personal library I maintain to always have access to frequently used functionality in my projects. A pre-compiled static version of this library is provided with the source for this tool. If anyone truly needs it, I can provide the source for this library as well.
