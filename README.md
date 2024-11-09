This repository contains my CA 1 Assignment for the Software Engineering 3 module, completed during my third year of Computer Science at TU Dublin.

The assignment focuses on designing a web-based music player application. The system’s structural and behavioral components are modeled using UML diagrams created in IBM Rational Software Architect Designer.

# Project Overview

This project models a music player application with distinct user roles and functionalities. Two primary user types are identified:

- **Subscribers**: Can create and manage playlists, and add or view tracks as desired.
- **Administrators**: Have extended permissions, including the ability to mark tracks as unavailable, effectively removing them from all user playlists.

# Deliverables

### 1. Class Diagram
   - Models core entities and their relationships, including:
     - **User**: A base class for all users, with subclasses for Subscriber and Administrator specifying unique permissions and actions.
     - **Playlist**: Represents user-created collections of tracks, with attributes and methods for playlist management.
     - **Track** and **Song**: Model individual songs and their metadata (title, artist, duration). Tracks are linked to playlists.
     - **ErrorHandler**: A singleton class responsible for managing error handling and validation.
   - Key features include inheritance for user roles and composition for playlists containing tracks.

### 2. Sequence Diagrams
   - Detail interactions for essential user actions:
     - **View Playlist**: Sequence for a subscriber viewing playlist details.
     - **Add Track**: Sequence for adding a track to a playlist.
     - **Remove Song**: Sequence for an administrator to mark a song as unavailable, removing it from all playlists.

# Use Case Summaries

- **View Playlist**: Subscribers can view the details of a playlist, displaying track information with error handling for access issues.
- **Add Track**: Subscribers can add selected tracks to playlists, with options to confirm or cancel the addition.
- **Remove Song**: Administrators can mark a song as unavailable, removing it from all playlists and restricting access.

## Class Diagram Explanation

The class diagram models the structure of the music playlist application, focusing on entities and interactions that support playlist management and administrative control over songs:

1. **User and Subclasses**:
   - **User**: Base class representing all users, with attributes like `userID`, `username`, and `password`, and methods for `login` and `logout`.
   - **Subscriber**: Inherits from User, adding attributes like `email` and `subscriptionType`, and methods such as `viewPlaylist` and `addTrack` for playlist management.
   - **Administrator**: Inherits from User, including methods like `removeSong` and `markSongUnavailable` for song management.

2. **Playlist**:
   - Represents a subscriber’s playlist, with attributes such as `playlistID`, `name`, `creationDate`, and `totalTracks`. Methods like `addTrack` and `removeTrack` allow playlist modification.

3. **Track and Song**:
   - **Track**: Represents an instance of a song within a playlist, containing `trackID`, `name`, `artist`, and `isAvailable`.
   - **Song**: Stores the actual music data, including metadata like `title`, `artist`, and `genre`. Multiple tracks can reference the same song across different playlists.

4. **ErrorHandler**:
   - A singleton class that manages error handling with methods like `showError` and `redirectToPrevPage`, ensuring consistent error management across the application.

5. **Relationships**:
   - A Subscriber can manage multiple Playlists (1-to-many).
   - A Playlist contains multiple Tracks, each linked to a single Song.
   - ErrorHandler is accessible by all classes for centralized error management.

## Sequence Diagram Explanations

### Sequence Diagram 1: View Playlist
   - **Purpose**: Demonstrates how a Subscriber views the details of a playlist.
   - **Flow**:
     1. The Subscriber calls `viewPlaylist(playlistID)`, which verifies access permissions using `validateAccess` in the Playlist class.
     2. If access is granted, the system retrieves playlist details with `retrievePlaylistDetails`, including metadata.
     3. A loop iterates over each track in the playlist, calling `getDetails` on each Track to fetch song information.
     4. The system displays the playlist and track details to the Subscriber.
     5. If access is denied, **ErrorHandler** shows an error message and redirects the user.

   - **Multiplicity & Relationships**:
     - A Subscriber has multiple Playlists (1-to-many).
     - A Playlist contains multiple Tracks (1-to-many).
     - ErrorHandler provides centralized error handling.

### Sequence Diagram 2: Add Track
   - **Purpose**: Describes how a Subscriber adds a track to one or more playlists.
   - **Flow**:
     1. The Subscriber initiates `addTrack(trackID)`, prompting the system to display available playlists using `displayPlaylists`.
     2. The Subscriber selects one or more playlists and confirms the addition.
     3. For each selected playlist, a loop iterates, calling `addTrack(trackID)` on both the Playlist and Track classes to add the track.
     4. If the Subscriber cancels the action, `cancelTrackAddition()` is called, making no changes.
     5. Upon completion, the system returns the Subscriber to the previous page.

   - **Multiplicity & Relationships**:
     - A Subscriber manages multiple Playlists (1-to-many).
     - A Track can be added to multiple Playlists (many-to-many).
     - Error handling is triggered if the operation is canceled.

### Sequence Diagram 3: Remove Song
   - **Purpose**: Demonstrates the steps an Administrator follows to remove a song and mark it as unavailable.
   - **Flow**:
     1. The Administrator selects `removeSong(songID)`, calling `markUnavailable` on the Song to set availability to false.
     2. If the song ID is invalid, ErrorHandler displays a “Song not found” error and redirects the user.
     3. If the song is valid, the system iterates through playlists containing the song, calling `removeTrack(trackID)` to remove it.
     4. The Administrator confirms the song's unavailability with `confirmSongUnavailability()`.

   - **Multiplicity & Relationships**:
     - An Administrator interacts with multiple Songs (1-to-many).
     - Each Song can be referenced by multiple Tracks across playlists (1-to-many).
     - Error handling is managed by ErrorHandler if the song ID is invalid.
