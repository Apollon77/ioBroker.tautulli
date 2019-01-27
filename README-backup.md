![Logo](https://raw.githubusercontent.com/Zefau/ioBroker.tautulli/master/admin/tautulli.jpeg)
# ioBroker.tautulli
[Tautulli is a 3rd party application](https://tautulli.com/#about) that you can run alongside your Plex Media Server to monitor activity and track various statistics. Most importantly, these statistics include what has been watched, who watched it, when and where they watched it, and how it was watched. All statistics are presented in a nice and clean interface with many tables and graphs, which makes it easy to brag about your server to everyone else.

This adapter connects to the [Tautulli API](https://github.com/Tautulli/Tautulli/blob/master/API.md) and also receives webhook events from both Tautulli and Plex (the latter requires Plex Pass).

[![NPM version](http://img.shields.io/npm/v/iobroker.tautulli.svg)](https://www.npmjs.com/package/iobroker.tautulli)
[![Travis CI](https://travis-ci.org/Zefau/ioBroker.tautulli.svg?branch=master)](https://travis-ci.org/Zefau/ioBroker.tautulli)
[![Downloads](https://img.shields.io/npm/dm/iobroker.tautulli.svg)](https://www.npmjs.com/package/iobroker.tautulli)

[![NPM](https://nodei.co/npm/iobroker.tautulli.png?downloads=true)](https://nodei.co/npm/iobroker.tautulli/)


**Table of contents**
1. Setup instructions
   1. API settings
   2. Webhook settings
    1. JSON data format
    2. Webhook data
       1. List of available parameters
       2. Notification Text Modifiers
       3. Notification Exclusion Tags
2. Smart Home / Alexa integration using ioBroker.javascript
3. Changelog
4. Licence


## Setup instructions
Check out [Tautulli Preview](https://tautulli.com/#preview) and [install it on your preferred system](https://github.com/Tautulli/Tautulli-Wiki/wiki/Installation) if you are interested.

### API settings
Once Tautulli is installed, open the _Settings_ page from Tautulli dashboard and navigate to _Web Interface_. Scroll down to the _API_ section and make sure ```Enable API``` is checked. Copy the ```API key``` and enter it in the ioBroker.tautulli settings. Furthermore, add the Tautulli IP address and port to allow API communication.


### Webhook settings _(not yet implemented)_
Once installed open the settings page from Tautulli dashboard and navigate to Notification Agents as seen below:

![](/img/screenshot_install-01-settings.png)

Click _Add a new notification agent_ and _Webhook_.

For a webook, enter the ioBroker IP adress with the port set in ioBroker.tautulli settings in _Webhook URL_ following the form ```http://192.168.2.99:1990```:
![](/img/screenshot_install-02-webhook.png)


Furthermore, choose ```PUT``` for the _Webhook Method_ and enter any description you like in _Description_. Go to the _Triggers_ tab and select all options.

To test the connection, go to the _Test Notifications_ tab, fill in ```{"test": true}``` and click _Test Webhook_:

![](/img/screenshot_install-03-test.png)

The ioBroker Log should show:

```
tautulli.0	2018-12-31 18:08:38.241	info	Connection tested successfully!
tautulli.0	2018-12-31 18:08:38.241	info	Received an event from Tautulli.
```

Finally, go to _Data_ tab and set up all messages to your needs. **This is important for ioBroker.tautulli** to receive data. For example input, see below.


#### JSON data format
If set up correctly, the ioBroker.tautulli adapter will receive notification from Tautulli in the JSON format.

Please make sure the format **always** follows the format ```{"title": "...", "message": "..."}```. The placeholders ```...``` may be replaced by any string (and contain any variable defined in the [List of available parameters](https://github.com/Zefau/ioBroker.tautulli#list-of-available-parameters)).

##### Webhook data

| Type of Notification | Example of JSON data |
| -------------------- | -------------------- |
| Playback Start | ```{"title": "Playback of {media_type} started", "message": "{user} ({player}) started playing {title}."}``` |
| Playback Stop | _to be defined_ |
| Playback Pause | _to be defined_ |
| Playback Resume | _to be defined_ |
| Transcode Decision Change | _to be defined_ |
| Watched | _to be defined_ |
| Buffer Warning | _to be defined_ |
| User Concurrent Streams | _to be defined_ |
| User New Device | _to be defined_ |
| Recently Added | _to be defined_ |
| Plex Server Down | _to be defined_ |
| Plex Server Back Up | _to be defined_ |
| Plex Remote Access Down | _to be defined_ |
| Plex Remote Access Back Up | _to be defined_ |
| Plex Update Available | _to be defined_ |
| Tautulli Update Available | _to be defined_ |
 
###### List of available parameters
**Global**
| Parameter | Description |
| --------- | ----------- |
| {tautulli_version} | The current version of Tautulli. |
| {tautulli_remote} | The current git remote of Tautulli. |
| {tautulli_branch} | The current git branch of Tautulli. |
| {tautulli_commit} | The current git commit hash of Tautulli. |
| {server_name} | The name of your Plex Server. |
| {server_ip} | The connection IP address for your Plex Server. |
| {server_port} | The connection port for your Plex Server. |
| {server_url} | The connection URL for your Plex Server. |
| {server_platform} | The platform of your Plex Server. |
| {server_version} | The current version of your Plex Server. |
| {server_machine_id} | The unique identifier for your Plex Server. |
| {action} | The action that triggered the notification. |
| {current_year} | The year when the notfication is triggered. |
| {current_month} | The month when the notfication is triggered. (1 to 12) |
| {current_day} | The day when the notfication is triggered. (1 to 31) |
| {current_hour} | The hour when the notfication is triggered. (0 to 23) |
| {current_minute} | The minute when the notfication is triggered. (0 to 59) |
| {current_second} | The second when the notfication is triggered. (0 to 59) |
| {current_weekday} | The ISO weekday when the notfication is triggered. (1 (Mon) to 7 (Sun)) |
| {current_week} | The ISO week number when the notfication is triggered. (1 to 52) |
| {datestamp} | The date (in date format) when the notification is triggered. |
| {timestamp} | The time (in time format) when the notification is triggered. |
| {unixtime} | The unix timestamp when the notification is triggered. |
| {utctime} | The UTC timestamp in ISO format when the notification is triggered. |

**Stream Details**
| Parameter | Description |
| --------- | ----------- |
| {streams} | The number of concurrent streams. |
| {user_streams} | The number of concurrent streams by the person streaming. |
| {user} | The friendly name of the person streaming. |
| {username} | The username of the person streaming. |
| {user_email} | The email address of the person streaming. |
| {device} | The type of client device being used for playback. |
| {platform} | The type of client platform being used for playback. |
| {product} | The type of client product being used for playback. |
| {player} | The name of the player being used for playback. |
| {ip_address} | The IP address of the device being used for playback. |
| {stream_duration} | The duration (in minutes) for the stream. |
| {stream_time} | The duration (in time format) of the stream. |
| {remaining_duration} | The remaining duration (in minutes) of the stream. |
| {remaining_time} | The remaining duration (in time format) of the stream. |
| {progress_duration} | The last reported offset (in minutes) of the stream. |
| {progress_time} | The last reported offset (in time format) of the stream. |
| {progress_percent} | The last reported progress percent of the stream. |
| {transcode_decision} | The transcode decisions of the stream. |
| {video_decision} | The video transcode decisions of the stream. |
| {audio_decision} | The audio transcode decisions of the stream. |
| {subtitle_decision} | The subtitle transcode decisions of the stream. |
| {quality_profile} | The Plex quality profile of the stream. (e.g. Original, 4 Mbps 720p, etc.) |
| {optimized_version} | If the stream is an optimized version. (0 or 1) |
| {optimized_version_profile} | The optimized version profile of the stream. |
| {synced_version} | If the stream is an synced version. (0 or 1) |
| {live} | If the stream is live TV. (0 or 1) |
| {stream_local} | If the stream is local. (0 or 1) |
| {stream_location} | The network location of the stream. (lan or wan) |
| {stream_bandwidth} | The required bandwidth (in kbps) of the stream. (not the used bandwidth) |
| {stream_container} | The media container of the stream. |
| {stream_bitrate} | The bitrate (in kbps) of the stream. |
| {stream_aspect_ratio} | The aspect ratio of the stream. |
| {stream_video_codec} | The video codec of the stream. |
| {stream_video_codec_level} | The video codec level of the stream. |
| {stream_video_bitrate} | The video bitrate (in kbps) of the stream. |
| {stream_video_bit_depth} | The video bit depth of the stream. |
| {stream_video_framerate} | The video framerate of the stream. |
| {stream_video_ref_frames} | The video reference frames of the stream. |
| {stream_video_resolution} | The video resolution of the stream. |
| {stream_video_height} | The video height of the stream. |
| {stream_video_width} | The video width of the stream. |
| {stream_video_language} | The video language of the stream. |
| {stream_video_language_code} | The video language code of the stream. |
| {stream_audio_bitrate} | The audio bitrate of the stream. |
| {stream_audio_bitrate_mode} | The audio bitrate mode of the stream. (cbr or vbr) |
| {stream_audio_codec} | The audio codec of the stream. |
| {stream_audio_channels} | The audio channels of the stream. |
| {stream_audio_channel_layout} | The audio channel layout of the stream. |
| {stream_audio_sample_rate} | The audio sample rate (in Hz) of the stream. |
| {stream_audio_language} | The audio language of the stream. |
| {stream_audio_language_code} | The audio language code of the stream. |
| {stream_subtitle_codec} | The subtitle codec of the stream. |
| {stream_subtitle_container} | The subtitle container of the stream. |
| {stream_subtitle_format} | The subtitle format of the stream. |
| {stream_subtitle_forced} | If the subtitles are forced. (0 or 1) |
| {stream_subtitle_language} | The subtitle language of the stream. |
| {stream_subtitle_language_code} | The subtitle language code of the stream. |
| {stream_subtitle_location} | The subtitle location of the stream. |
| {transcode_container} | The media container of the transcoded stream. |
| {transcode_video_codec} | The video codec of the transcoded stream. |
| {transcode_video_width} | The video width of the transcoded stream. |
| {transcode_video_height} | The video height of the transcoded stream. |
| {transcode_audio_codec} | The audio codec of the transcoded stream. |
| {transcode_audio_channels} | The audio channels of the transcoded stream. |
| {transcode_hw_requested} | If hardware decoding/encoding was requested. (0 or 1) |
| {transcode_hw_decoding} | If hardware decoding is used. (0 or 1) |
| {transcode_hw_decode} | The hardware decoding codec. |
| {transcode_hw_decode_title} | The hardware decoding codec title. |
| {transcode_hw_encoding} | If hardware encoding is used. (0 or 1) |
| {transcode_hw_encode} | The hardware encoding codec. |
| {transcode_hw_encode_title} | The hardware encoding codec title. |
| {session_key} | The unique identifier for the session. |
| {transcode_key} | The unique identifier for the transcode session. |
| {session_id} | The unique identifier for the stream. |
| {user_id} | The unique identifier for the user. |
| {machine_id} | The unique identifier for the player. |

**Source Metadata Details**
| Parameter | Description |
| --------- | ----------- |
| {media_type} | The type of media. (movie, show, season, episode, artist, album, track, clip) |
| {title} | The full title of the item. |
| {library_name} | The library name of the item. |
| {show_name} | The title of the TV series. |
| {episode_name} | The title of the episode. |
| {artist_name} | The name of the artist. |
| {album_name} | The title of the album. |
| {track_name} | The title of the track. |
| {track_artist} | The name of the artist of the track. |
| {season_num} | The season number. (e.g. 1, or 1-3) |
| {season_num00} | The two digit season number. (e.g. 01, or 01-03) |
| {episode_num} | The episode number. (e.g. 6, or 6-10) |
| {episode_num00} | The two digit episode number. (e.g. 06, or 06-10) |
| {track_num} | The track number. (e.g. 4, or 4-10) |
| {track_num00} | The two digit track number. (e.g. 04, or 04-10) |
| {season_count} | The number of seasons. |
| {episode_count} | The number of episodes. |
| {album_count} | The number of albums. |
| {track_count} | The number of tracks. |
| {year} | The release year for the item. |
| {release_date} | The release date (in date format) for the item. |
| {air_date} | The air date (in date format) for the item. |
| {added_date} | The date (in date format) the item was added to Plex. |
| {updated_date} | The date (in date format) the item was updated on Plex. |
| {last_viewed_date} | The date (in date format) the item was last viewed on Plex. |
| {studio} | The studio for the item. |
| {content_rating} | The content rating for the item. (e.g. TV-MA, TV-PG, etc.) |
| {directors} | A list of directors for the item. |
| {writers} | A list of writers for the item. |
| {actors} | A list of actors for the item. |
| {genres} | A list of genres for the item. |
| {labels} | A list of labels for the item. |
| {collections} | A list of collections for the item. |
| {summary} | A short plot summary for the item. |
| {tagline} | A tagline for the media item. |
| {rating} | The rating (out of 10) for the item. |
| {critic_rating} | The critic rating (%) for the item. (Ratings source must be Rotten Tomatoes for the Plex Movie agent) |
| {audience_rating} | The audience rating (%) for the item. (Ratings source must be Rotten Tomatoes for the Plex Movie agent) |
| {duration} | The duration (in minutes) for the item. |
| {poster_url} | A URL for the movie, TV show, or album poster. |
| {plex_url} | The Plex URL to your server for the item. |
| {imdb_id} | The IMDB ID for the movie. (e.g. tt2488496) |
| {imdb_url} | The IMDB URL for the movie. |
| {thetvdb_id} | The TVDB ID for the TV show. (e.g. 121361) |
| {thetvdb_url} | The TVDB URL for the TV show. |
| {themoviedb_id} | The TMDb ID for the movie or TV show. (e.g. 15260) |
| {themoviedb_url} | The TMDb URL for the movie or TV show. |
| {tvmaze_id} | The TVmaze ID for the TV show. (e.g. 290) |
| {tvmaze_url} | The TVmaze URL for the TV show. |
| {lastfm_url} | The Last.fm URL for the album. |
| {trakt_url} | The trakt.tv URL for the movie or TV show. |
| {container} | The media container of the original media. |
| {bitrate} | The bitrate of the original media. |
| {aspect_ratio} | The aspect ratio of the original media. |
| {video_codec} | The video codec of the original media. |
| {video_codec_level} | The video codec level of the original media. |
| {video_bitrate} | The video bitrate of the original media. |
| {video_bit_depth} | The video bit depth of the original media. |
| {video_framerate} | The video framerate of the original media. |
| {video_ref_frames} | The video reference frames of the original media. |
| {video_resolution} | The video resolution of the original media. |
| {video_height} | The video height of the original media. |
| {video_width} | The video width of the original media. |
| {video_language} | The video language of the original media. |
| {video_language_code} | The video language code of the original media. |
| {audio_bitrate} | The audio bitrate of the original media. |
| {audio_bitrate_mode} | The audio bitrate mode of the original media. (cbr or vbr) |
| {audio_codec} | The audio codec of the original media. |
| {audio_channels} | The audio channels of the original media. |
| {audio_channel_layout} | The audio channel layout of the original media. |
| {audio_sample_rate} | The audio sample rate (in Hz) of the original media. |
| {audio_language} | The audio language of the original media. |
| {audio_language_code} | The audio language code of the original media. |
| {subtitle_codec} | The subtitle codec of the original media. |
| {subtitle_container} | The subtitle container of the original media. |
| {subtitle_format} | The subtitle format of the original media. |
| {subtitle_forced} | If the subtitles are forced. (0 or 1) |
| {subtitle_location} | The subtitle location of the original media. |
| {subtitle_language} | The subtitle language of the original media. |
| {subtitle_language_code} | The subtitle language code of the original media. |
| {file} | The file path to the item. |
| {filename} | The file name of the item. |
| {file_size} | The file size of the item. |
| {section_id} | The unique identifier for the library. |
| {rating_key} | The unique identifier for the movie, episode, or track. |
| {parent_rating_key} | The unique identifier for the season or album. |
| {grandparent_rating_key} | The unique identifier for the TV show or artist. |
| {thumb} | The Plex thumbnail for the movie or episode. |
| {parent_thumb} | The Plex thumbnail for the season or album. |
| {grandparent_thumb} | The Plex thumbnail for the TV show or artist. |
| {poster_thumb} | The Plex thumbnail for the poster image. |
| {poster_title} | The title for the poster image. |
| {indexes} | If the media has video preview thumbnails. (0 or 1) |

**Plex Update Available**
| Parameter | Description |
| --------- | ----------- |
| {update_version} | The available update version for your Plex Server. |
| {update_url} | The download URL for the available update. |
| {update_release_date} | The release date of the available update. |
| {update_channel} | The update channel. (Public or Plex Pass) |
| {update_platform} | The platform of your Plex Server. |
| {update_distro} | The distro of your Plex Server. |
| {update_distro_build} | The distro build of your Plex Server. |
| {update_requirements} | The requirements for the available update. |
| {update_extra_info} | Any extra info for the available update. |
| {update_changelog_added} | The added changelog for the available update. |
| {update_changelog_fixed} | The fixed changelog for the available update. |

**Tautulli Update Available**
| Parameter | Description |
| --------- | ----------- |
| {tautulli_update_version} | The available update version for Tautulli. |
| {tautulli_update_release_url} | The release page URL on GitHub. |
| {tautulli_update_tar} | The tar download URL for the available update. |
| {tautulli_update_zip} | The zip download URL for the available update. |
| {tautulli_update_commit} | The commit hash for the available update. |
| {tautulli_update_behind} | The number of commits behind for the available update. |
| {tautulli_update_changelog} | The changelog for the available update. |


###### Notification Text Modifiers


###### Notification Exclusion Tags


## Smart Home / Alexa integration using ioBroker.javascript
Will follow..


## Changelog

### 0.1.0 (2018-12-XX)
* (zefau) initial release


## License
The MIT License (MIT)

Copyright (c) 2018 Zefau <zefau@mailbox.org>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
