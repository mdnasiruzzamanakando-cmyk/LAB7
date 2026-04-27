# LAB7
Here's a professional GitHub README for your Lab 7 project.

---

## GitHub README.md

```markdown
# SoundWave Music Streaming Service - Lab 7

## Design Patterns Implementation

**Student:** Akando Md Nasiruzzaman  
**Student ID:** 23605108  
**Course:** Software Development (Computer Science & Engineering)  
**Semester:** Spring 2026  
**Instructor:** Kaiming Fang  
**Due Date:** April 27th, 2026

---

## Overview

This project implements the backend core for a music streaming application called SoundWave. The goal is to demonstrate four fundamental design patterns by solving common software architecture problems in a music streaming context.

The system handles:
- Nested playlists containing both songs and other playlists
- Multiple playback modes (Sequential, Shuffle)
- Media creation without coupling to concrete classes
- Various reports without modifying existing media classes

---

## Design Patterns Used

| Pattern | Problem Solved |
|---------|----------------|
| Composite | Nested playlist structure where playlists can contain songs or other playlists |
| Strategy | Runtime-switchable playback algorithms |
| Factory | Creating different media types without client code using constructors directly |
| Visitor | Generating reports without modifying existing media classes |

---

## Project Structure

```
lab7-code/
├── composite/
│   ├── __init__.py
│   ├── media_component.py    # Abstract base class
│   ├── song.py                # Leaf node
│   └── playlist.py            # Composite node
├── strategy/
│   ├── __init__.py
│   ├── playback_strategy.py   # Strategy interface
│   ├── sequential_play.py     # Concrete strategy
│   ├── shuffle_play.py        # Concrete strategy
│   └── player_context.py      # Context class
├── factory/
│   ├── __init__.py
│   ├── media_factory.py       # Factory class
│   ├── video.py               # Product class
│   └── podcast.py             # Product class
├── visitor/
│   ├── __init__.py
│   ├── media_visitor.py       # Visitor interface
│   ├── duration_calculator.py # Concrete visitor
│   ├── genre_count_visitor.py # Concrete visitor
│   └── explicit_content_visitor.py # Concrete visitor
└── main.py                    # Entry point and tests
```

---

## Requirements

- Python 3.7 or higher
- No external dependencies (uses only standard library)

---

## Installation

Clone the repository:

```bash
git clone https://github.com/yourusername/soundwave-lab7.git
cd soundwave-lab7
```

---

## Running the Program

Execute the main program:

```bash
python main.py
```

Or run from the project root:

```bash
python -m main
```

---

## Expected Output

When you run the program, you should see:

```
==================================================
  SoundWave Music Streaming Service
  Student: Akando Md Nasiruzzaman
  ID: 23605108
==================================================

TASK 2: Composite & Strategy Patterns
--------------------------------------------------

Testing Sequential Playback:
Switched to Sequential playback mode
[Sequential Mode] Playing in order:
1. === Playing Playlist: My Favorites ===
2. 📁 Entering playlist: Rock Classics
3. Playing song: Bohemian Rhapsody [355s]
4. 📁 Entering playlist: Pop Hits
5. Playing song: Imagine [183s]
6. Playing song: Billie Jean [294s]
7. Playing video: Inception Scene [180s]
8. Playing podcast: Tech Talk [2700s]

Testing Shuffle Playback:
Switched to Shuffle playback mode
[Shuffle Mode] Playing in random order:
1. Playing podcast: Tech Talk [2700s]
2. Playing song: Billie Jean [294s]
3. Playing video: Inception Scene [180s]
4. Playing song: Imagine [183s]
5. Playing song: Bohemian Rhapsody [355s]

TASK 3: Visitor & Factory Patterns
--------------------------------------------------

Media Factory Demo:
  Created: Shape of You by Ed Sheeran
  Created: Python Tutorial (directed by Tech Guru)
  Created: Daily News (Ep.101 with News Anchor)

Duration Calculator Visitor:
Total Duration: 4915 seconds (81 minutes, 55 seconds)

Genre Distribution: {'Rock': 1, 'Pop': 2, 'Video': 1, 'Podcast': 1}
```

---

## Code Examples

### Composite Pattern

```python
# Create a nested playlist structure
rock_playlist = Playlist("Rock Classics")
rock_playlist.add_media(Song("Bohemian Rhapsody", "Queen", 355, "Rock"))

pop_playlist = Playlist("Pop Hits")
pop_playlist.add_media(Song("Imagine", "John Lennon", 183, "Pop"))

main_playlist = Playlist("My Favorites")
main_playlist.add_media(rock_playlist)  # Playlist inside playlist
main_playlist.add_media(pop_playlist)

# Both Song and Playlist respond to play()
for media in main_playlist.get_components():
    media.play()  # Works for both leaf and composite
```

### Strategy Pattern

```python
# Create player with playlist
player = PlayerContext([main_playlist])

# Switch strategies at runtime
player.set_playback_strategy(SequentialPlay())
player.play()  # Plays in order

player.set_playback_strategy(ShufflePlay())
player.play()  # Plays in random order
```

### Factory Pattern

```python
# Create media without using constructors directly
song = MediaFactory.create_audio("Shape of You", "Ed Sheeran", 233, "Pop")
video = MediaFactory.create_video("Inception", "Nolan", 180, "1080p")
podcast = MediaFactory.create_podcast("Tech Talk", "Jane Doe", 2700, "42")

# Generic factory method
media = MediaFactory.create_media("audio", "Title", "Artist", "180", "Pop")
```

### Visitor Pattern

```python
# Calculate total duration without modifying media classes
duration_calc = DurationCalculator()
main_playlist.accept(duration_calc)
print(f"Total duration: {duration_calc.get_total_duration()} seconds")

# Add new report without changing existing code
genre_counter = GenreCountVisitor()
main_playlist.accept(genre_counter)
print(genre_counter.get_genre_counts())
```

---

## Key Design Decisions

### Why Composite for Nested Playlists?

The Composite pattern allows treating individual songs and playlists uniformly. A playlist can contain songs or other playlists without the client needing to know the difference. This makes recursive operations like calculating total duration or playing all content trivial.

### Why Strategy for Playback Modes?

Playback algorithms are the definition of "encapsulate variation" from Slide 44. Different users want different playback orders, and the system should allow switching at runtime. Strategy pattern makes this clean without messy if-else statements in the player class.

### Why Factory for Media Creation?

Using direct constructors like `Song()` couples client code to concrete classes. Factory pattern centralizes creation logic, making it easier to add validation, logging, or new media types later without changing client code.

### Why Visitor for Reports?

According to Slide 28-31, Visitor lets you add new operations without modifying existing element classes. If we added `get_total_duration()` directly to Playlist, we would need to modify it every time we want a new report (genre count, explicit content check, etc.). Visitor separates traversal from operations, following the Open/Closed Principle.

---

## Key Files

| File | Purpose |
|------|---------|
| `composite/media_component.py` | Abstract interface for all media |
| `composite/song.py` | Leaf node implementation |
| `composite/playlist.py` | Composite node with children |
| `strategy/playback_strategy.py` | Strategy interface |
| `strategy/sequential_play.py` | Sequential algorithm |
| `strategy/shuffle_play.py` | Shuffle algorithm |
| `strategy/player_context.py` | Context using strategy |
| `factory/media_factory.py` | Factory for media creation |
| `visitor/media_visitor.py` | Visitor interface |
| `visitor/duration_calculator.py` | Duration report visitor |
| `main.py` | Entry point and tests |

---

## Testing

The program automatically demonstrates all four patterns:

1. **Composite**: Creates a playlist containing two sub-playlists plus individual media
2. **Strategy**: Plays the same playlist sequentially, then switches to shuffle
3. **Factory**: Creates songs, videos, and podcasts without direct constructors
4. **Visitor**: Calculates total duration and genre counts without modifying media classes

To add new tests, modify `main.py` or create additional visitor classes.

---

## Learning Outcomes

After completing this lab, I can:

- Identify when to apply Composite, Strategy, Factory, and Visitor patterns
- Implement pattern structures in Python
- Explain why each pattern fits specific requirements using course slides
- Apply the three design philosophies from Slide 44:
  - Program to an interface
  - Encapsulate variation
  - Favor composition over inheritance
- Justify design decisions with pattern trade-offs

---

## Reflection Summary

**Was Visitor over-engineered for this lab?**  
For a lab with only 4 media types and 3 reports, Visitor adds complexity. A simple method in Playlist would work for duration calculation. However, the pattern is essential for real systems with 50+ media types and 200+ reports where adding methods to every class is impossible.

**Does Visitor need to know concrete types?**  
Yes. Different media types have different attributes. Songs have genre and explicit flags. Videos have resolution. Podcasts have episode numbers. The double dispatch mechanism in Visitor handles this elegantly without `isinstance()` checks.

**How do pattern names help team communication?**  
Pattern names create a shared vocabulary. Saying "let's use Strategy for playback modes" instantly communicates the structure: an interface, concrete classes, and a context that holds the strategy. This saves hours of explanation.

---

## Author

**Akando Md Nasiruzzaman**  
Computer Science & Engineering  
Student ID: 23605108  
Spring 2026

---

## Acknowledgments

- Instructor: Kaiming Fang
- Course materials: l8.pdf (Design Patterns slides)
- Reference: Gamma et al. "Design Patterns: Elements of Reusable Object-Oriented Software" (Gang of Four)

---

## License

This project was created for educational purposes as part of coursework.

---

## Submission

This repository contains:
- Complete source code (Component A)
- Lab report (Component B)

The ZIP file `lab7.zip` includes both components as required.
