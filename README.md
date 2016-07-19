# Pokemon GO GPS Emulator

<img width="1072" src=https://cloud.githubusercontent.com/assets/6434237/16940158/bc3917d6-4d3c-11e6-896b-0d800f4971a1.png>
<img width="480" src=https://cloud.githubusercontent.com/assets/6434237/16934843/3a988d10-4d0f-11e6-84e6-6dffe48a9e30.PNG>

This project uses Xcode Debug mode [Simulating a Location at Runtime](https://developer.apple.com/library/ios/recipes/xcode_help-debugger/articles/simulating_locations.html) to spoof GPS locations for non-jailbroken iOS devices. This allows players of Pokemon GO to send movement commands over a computer as opposed to doing the actual walking.

## Warning: Improper Use of this Tool Will Get You Banned!
As reported on [reddit](https://www.reddit.com/r/pokemongo/comments/4ry7my/psa_spoofing_gps_locations_will_get_you_banned/), spoofing your GPS coordinates in game could get you banned. Anecdotally, when you change your GPS coordinates drastically in a short period of time (say NYC to SF), you will be soft banned for anywhere between 10 mins to 3 hours. However, there have not been cases of permanent ban, so do this at your discretion. My guess is that the Niantic servers compute a delta distance over delta t and sets a threshold on the speed. Anything beyond the threshold will get your banned.

### Workaround
The latest repo has a startup routine that sets your startup location to be your current location. This would save you the hassle of looking up your GPS coordinates. The idea is you want to be as close to your current location as possible to not exceed the threshold. Also shutdown all background apps and refrain from fighting in a gym. The OS would shutdown the spoofing app when there's limited resources, causing you to teleport back to your original location.

## Main Components
- A blank iOS project, used in Debug mode for `Simulate Location`
- A web interface made via Sinatra to interact with PokemonGo from [PokemonGoControllerSuite](https://github.com/adin283/PokemonGoControllerSuite/tree/master/PokemonGoController)
- An AppleScript for sending GPS location signals

## System Requirement

- Xcode installed (Obviously you need a Mac, an Apple Developer Account is not needed if you have iOS 9 and above)
- Any iOS device with Pokemon GO installed

## Installation Instructions

### Start web server

```bash
sudo gem install sinatra
```
If you are in China, you can specify the source via -s http://gems.ruby-china.org

Go into the terminal, and run the following:

```
git clone https://github.com/huacnlee/PokemonGoMove.git
cd PokemonGoMove
./start-web 
```
This will try to set your startup location as your current location via your IP address. Note, this will not be very accurate as the API only returns up to 4 decimal places. If this fails, it will default to a location in California. If you would like to startup with a specific location you can run the following:

```
./start-web -l 123.456 -o 78.90 
```

You should see the debug messages below
```
== Sinatra (v1.4.7) has taken the stage on 3001 for development with backup from Puma
Puma starting in single mode...
* Version 3.4.0 (ruby 2.3.1-p112), codename: Owl Bowl Brawl
* Min threads: 0, max threads: 16
* Environment: development
* Listening on tcp://localhost:3001
Use Ctrl-C to stop
```

### Open foo.xcodeproj

Connect your iOS device and run the project. Remember in to turn simulate location on.
Very important: Debug->Simulate Location->PokemonLocation is checked, otherwise it will not work.

### Open a web browser

Now in a web browser open http://127.0.0.1:3001, you should see the following:

<img width="1072" src=https://cloud.githubusercontent.com/assets/6434237/16940158/bc3917d6-4d3c-11e6-896b-0d800f4971a1.png>

You can now interact with the webpage (try press left, up, right, down arrow keys on your keyboard) and the AppleScript will transmit the new GPS signal to the iOS device. Now feel free to relax and play the game without moving your legs.

### Crowdsourced Maps that Help You Catch all Pokemons:
Having trouble finding new pokemons? Try looking up the following map tools to find the pokemon you are looking for!
- [Pokecrew - Around the World] (http://www.pokecrew.com/?latitude=37.38870308649053&longitude=-122.08420737304687&zoom=14)
- [Pokemapper - Around the World] (https://pokemapper.co/)
- [Gotta Catch'em All - Boston] (https://www.google.com/maps/d/viewer?mid=1mA00u2SuvuNtGCifo0E-BVzTTWc)
- [Pokemon Go Washington - Seattle Area] (https://www.google.com/maps/d/viewer?mid=136ZOge0QKEGZpWMqa9XGe_iyu6Y)
