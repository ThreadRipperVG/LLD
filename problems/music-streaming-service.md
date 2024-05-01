# Designing an Online Music Streaming Service Like Spotify

This article focuses on developing an object-oriented design for an Online Music Streaming Service similar to Spotify using Python. 

The system aims to deliver a comprehensive music streaming experience.

## System Requirements

The Online Music Streaming Service should:

1. **User Account Management**: Manage user registrations, profiles, and subscriptions.
2. **Music Library Management**: Maintain a library of songs, artists, and albums.
3. **Streaming and Playback**: Enable streaming of music and manage playback settings.
4. **Playlist Management**: Allow users to create and manage personalized playlists.
5. **User Recommendation System**: Offer music suggestions based on preferences and listening history.

## Core Use Cases

1. **Registering and Managing User Accounts**
2. **Browsing and Streaming Music**
3. **Creating and Editing Playlists**
4. **Recommending Music**
5. **Handling Subscriptions and Payments**

## UML/Class Diagrams

Key Classes:

- `MusicStreamingService`: Manages the system.
- `User`: Represents a subscriber.
- `Song`: Represents an individual music track.
- `Playlist`: Manages a collection of songs.
- `Subscription`: Handles subscription details.

## Java Implementation

### User Class

Manages user account information and preferences.

```python
from enum import Enum
from typing import List
from datetime import datetime
from random import randint


# Subscription Class
class SubscriptionType(Enum):
    FREE = 1
    PREMIUM = 2
    STUDENT = 3
    FAMILY = 4


class Subscription:
    def __init__(
        self,
        price: int,
        fromDate: int,
        toDate: int,
        subscriptionType: SubscriptionType = SubscriptionType.FREE,
    ):
        self.subscriptionType = subscriptionType
        self.price = price
        self.fromDate = fromDate
        self.toDate = toDate


# Song Class
class GenreType(Enum):
    JAZZ = 1
    POP = 2
    INDIE = 3
    CLASSICAL = 4
    ROMANTIC = 5
    ADVENTUROUS = 6


class Song:
    def __init__(
        self,
        songId: str,
        name: str,
        artist: str,
        album: str,
        year: str,
        genre: List[str],
        duration: int,
        audioFile: str,
    ):
        self.songId = songId
        self.name = name
        self.artist = artist
        self.album = album
        self.year = year
        self.genre = genre
        self.duration = audioFile
        self.audioFile = audioFile

    def editSong(
        self,
        name: str,
        artist: str,
        album: str,
        year: str,
        genre: List[str],
        audioFile: str,
    ):
        self.name = name
        self.artist = artist
        self.album = album
        self.year = year
        self.genre = genre
        self.audioFile = audioFile

    def play(self):
        print("Playing song", self.name, "by", self.artist)
        pass


# Playlist Class
class Playlist:
    def __init__(self, playlistId: str, name: str, songs: List[Song]):
        self.playlistId = playlistId
        self.name = name
        self.songs = songs

    def editPlaylist(self, playlistId: str, name: str, songs: List[Song]):
        self.playlistId = playlistId
        self.name = name
        self.songs = songs

    def addSongs(self, songs: List[Song]) -> None:
        self.songs.extend(songs)

    def removeSong(self, song: Song):
        self.playlists.remove(song)

    def reorderPlaylist(self, songs: List[Song]):
        self.songIds = songs


# User Class
class User:
    def __init__(
        self, userId: str, name: str, age: int, subscription: List[Subscription] = []
    ):
        self.userId = userId
        self.name = name
        self.age = age
        self.subscription = subscription
        self.history = []
        self.playlists = []
        if self.subscription == []:
            self.subscription.append(
                Subscription(0, datetime.now(), -1, SubscriptionType.FREE)
            )

    # Getter Setter for individual component
    def createPlaylist(self, playlist: Playlist) -> None:
        self.playlists.append(playlist)

    def deletePlaylist(self, playlist: Playlist):
        self.playlists.remove(playlist)

    def editUserDetails(self, name: str, age: str):
        self.name = name
        self.age = age

    def editUserSubscription(self, subscription: Subscription):
        self.subscription.append(subscription)

    def getCurrentSubscription(self) -> Subscription:
        dateNow = datetime.now()
        for i in range(len(self.subscription) - 1, -1, -1):
            if (
                self.subscription[i].toDate == -1
                or self.subscription[i].toDate >= dateNow
            ):
                currSubs = self.subscription[i]
                print(
                    "Current subscription is",
                    self.subscription[i].subscriptionType,
                    "priced at",
                    str(self.subscription[i].price),
                    "from",
                    str(self.subscription[i].fromDate),
                    "to",
                    str(self.subscription[i].toDate),
                )
                return currSubs

    def playSong(self, song: Song):
        self.history.append([song, datetime.now()])
        song.play()


# Music Service Class
class MusicService:
    def __init__(
        self,
        users: List[User],
        songsList: List[Song],
        defaultPlaylists: List[Playlist],
    ):
        self.users = users
        self.songsList = songsList
        self.defaultPlaylists = defaultPlaylists

    def addUser(self, user: User) -> None:
        self.users.append(user)

    def removeUser(self, user: User):
        self.users.remove(user)

    def searchSong(self, searchText: str, searchField: str) -> Song:
        for song in self.songsList:
            if searchText.lower() in song["searchField"]:
                return song
        raise ValueError("Song with details not found")

    def getRecommendationForUser(self, user: User) -> Song:
        songsList = []
        for playlist in user.playlists:
            for song in playlist.songs:
                songsList.append(song)
        for playlist in self.defaultPlaylists:
            for song in playlist.songs:
                songsList.append(song)
        return songsList[randint(0, len(songsList) - 1)]

    def playSongForUser(self, user: User, song: Song):
        user.playSong(song)


# Driver Code

# Create User
usersList: List[User] = []
for i in range(5):
    usersList.append(User(i, "UserName" + str(i), i + 25, []))
    print(
        "Created User",
        usersList[i].name,
        "subscription",
        str(usersList[i].getCurrentSubscription()),
    )

# remove User
usersList[0].editUserSubscription(
    Subscription(100, datetime.now(), -1, SubscriptionType.PREMIUM)
)
usersList[0].getCurrentSubscription()

# Edit subscription
usersList[0].editUserSubscription(
    Subscription(100, datetime.now(), -1, SubscriptionType.PREMIUM)
)
usersList[0].getCurrentSubscription()

# Create Song
songsList: List[Song] = []
for i in range(10):
    songsList.append(
        Song(
            i,
            "SongName" + str(i),
            "Artist",
            "Album" + str(i),
            i + 200,
            [GenreType.POP, GenreType.ADVENTUROUS],
            100,
            "AudioFile" + str(i),
        )
    )
    print("Created Song", songsList[i].name, "file location", songsList[i].audioFile)

playlists: List[Playlist] = []
for i in range(0, 10, 2):
    playlists.append(
        Playlist(str(i), "PlaylistName" + str(i), [songsList[i], songsList[i + 1]])
    )

musicService = MusicService(usersList, songsList, [playlists[3], playlists[4]])

usersList[0].createPlaylist(playlists[0])
usersList[0].createPlaylist(playlists[1])
usersList[1].createPlaylist(playlists[1])
usersList[1].createPlaylist(playlists[2])
usersList[2].createPlaylist(playlists[2])
usersList[2].createPlaylist(playlists[3])

musicService.playSongForUser(usersList[0], songsList[4])
musicService.playSongForUser(usersList[0], songsList[3])
musicService.playSongForUser(usersList[0], songsList[5])
musicService.playSongForUser(usersList[0], songsList[4])
print(usersList[0].history)

print(musicService.getRecommendationForUser(usersList[1]))
print(musicService.getRecommendationForUser(usersList[1]))
print(musicService.getRecommendationForUser(usersList[1]))

```