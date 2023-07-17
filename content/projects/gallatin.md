---
title: "gallatin"
date: 2023-05-03
mainSectionTitle: "gallatin"
---

Gallatin is a web application that exports playlists into users' spotify account using machine learning algorithm. Playlist is determined by user's preference on song's emotion and user's current location / weather factors. It is built with Python frameworks (scikit-learn, numpy, Flask) and external APIs (Spotify API, OpenWeather API). It maps songs to a specific emotion from four categories -- anger, happiness, sadness, and relaxed.

- Gallatin uses OpenWeather API to check the weather of the user's current location
- Gallatin collect training data from Spotify song objects (including danceability, valence, energy, etc)
- Gallatin prepares a classification machine learning algorithm by using scikit-learn and numpy library to train the model to correlate different weather factors to a specific emotion. 
- Gallatin let the user run the calculations manually.
- Gallatin exports the songs into a Spotify playlist via Spotify API

#### Github: https://github.com/dk3156/gallatin

## Setup

### Requirements

* Python 3.10 or higher
* Spotify account and developer app with client id and secret
  * Alternatively, you can use the included sample credentials and account information

### Installation

1. Clone the repository
2. Open the terminal,  install prerequisites with `pip3 install -r requirements.txt`
3. Run app with `python3 main.py`
4. Web will run on http://localhost:8080
5. When prompted, type Spotify UserID
   1. The user ID for the test account is `31zmbimflej3gpsj3yxpwpm4qeyy`
6. ClientID and Secret Key values in gallatin.py MUST match with those in user's spotify dashboard setting
7. Copy and paste the redirect url on the terminal, when prompted "Enter the redirect url"
8. Choose options for playlist
9. Check spotify playlist panels!

## Test User

username: carosa8039@syinxun.com
password: carosa8039
userid:   31zmbimflej3gpsj3yxpwpm4qeyy

## Supported emotions

* happy
  * Sunny, up to 25C
  * Partly cloudy, any temp
  * Cloudy, 5-25C
* relaxed
* sad
  * Below 5C
* angry
  * Heavy rain
  * Thunder
  * Light/


## Weather

* Temperature
* Raining
* Cloud coverage
* Thunder
* Snow


Your choice: angry
[{'danceability': 0.727, 'energy': 0.834, 'key': 2, 'loudness': -5.851, 'mode': 1, 'speechiness': 0.0496, 'acousticness': 0.165, 'instrumentalness': 0, 'liveness': 0.105, 'valence': 0.816, 'tempo': 131.7, 'type': 'audio_features', 'id': '5RsUlxLto4NZbhJpqJbHfN', 'uri': 'spotify:track:5RsUlxLto4NZbhJpqJbHfN', 'track_href': 'https://api.spotify.com/v1/tracks/5RsUlxLto4NZbhJpqJbHfN', 'analysis_url': 'https://api.spotify.com/v1/audio-analysis/5RsUlxLto4NZbhJpqJbHfN', 'duration_ms': 194120, 'time_signature': 4}]

  SUNNY = 113
  PARTLY_CLOUDY = 116
  CLOUDY = 119
  VERY_CLOUDY = 122
  FOG = 143
  LIGHT_SHOWERS = 176
  LIGHT_SLEET_SHOWERS = 179
  LIGHT_SLEET = 182
  THUNDERY_SHOWERS = 200
  LIGHT_SNOW = 227
  HEAVY_SNOW = 230
  LIGHT_RAIN = 266
  HEAVY_SHOWERS = 299
  HEAVY_RAIN = 302
  LIGHT_SNOW_SHOWERS = 323
  HEAVY_SNOW_SHOWERS = 335
  THUNDERY_HEAVY_RAIN = 389
  THUNDERY_SNOW_SHOWERS = 392
  
Watch the demonstration video:

{{< youtube pYn3GoogPI4>}}