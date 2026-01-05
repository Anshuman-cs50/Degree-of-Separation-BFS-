## Degrees of Separation (Movie Co‑Starring Graph)

This project computes the **degrees of separation between two actors** based on their shared movie appearances.  
Given a dataset of people, movies, and cast lists (from IMDb-style CSV files), the program models a graph where:

- **Nodes** are people (actors).
- **Edges** exist between two people if they **co‑starred in at least one movie**.

Using **breadth‑first search (BFS)**, it finds the shortest path of co‑starring relationships between any two actors you specify.

---

## Project Structure

- `degrees.py` – Main program. Loads data, prompts for two names, and prints the shortest chain of movies and co‑stars connecting them.
- `util.py` – Helper classes for graph search (`Node`, `StackFrontier`, `QueueFrontier`) that implement the BFS frontier.
- `large/` – Larger CSV dataset (`people.csv`, `movies.csv`, `stars.csv`).
- `small/` – Smaller CSV dataset for faster testing and experimentation.

---

## How It Works

1. **Data loading**
   - Reads `people.csv`, `movies.csv`, and `stars.csv` from a chosen directory (`small` or `large`).
   - Builds:
     - `names`: map from lowercase person names → set of matching person IDs.
     - `people`: map from person ID → `{ name, birth, movies }`.
     - `movies`: map from movie ID → `{ title, year, stars }`.

2. **Graph model**
   - For a given person, their “neighbors” are all people who appeared in any of the same movies.
   - Each step in a path is represented as a pair `(movie_id, person_id)` meaning “go to this person via this movie”.

3. **Shortest path search**
   - Uses a **queue-based frontier** (`QueueFrontier`) to perform BFS from the source actor to the target.
   - When the target is found, reconstructs the path from target back to source by following parent pointers.
   - Prints the path as a sequence of:  
     `N: Person A and Person B starred in Movie`.

---

## Requirements

- **Python 3.8+**
- No third‑party libraries are required; only the Python standard library is used (`csv`, `sys`).

---

## Setup

1. **Clone or download** this repository.
2. Ensure the following structure exists (already present in this project):
   - `degrees.py`
   - `util.py`
   - `small/people.csv`, `small/movies.csv`, `small/stars.csv`
   - `large/people.csv`, `large/movies.csv`, `large/stars.csv`
3. (Optional) Open the project in your editor/IDE of choice.

> **Note:** The script currently uses an absolute `BASE_DIR` path inside `degrees.py`.  
> If you move this project to a different location, update `BASE_DIR` to match your new path, or modify the code to compute it dynamically from the script’s directory.

---

## Usage

From the project root, run:

- **Using the large dataset (default):**

```bash
python degrees.py
```

- **Using a specific dataset directory (`small` or `large`):**

```bash
python degrees.py small
# or
python degrees.py large
```

You will then be prompted for:

1. **Name:** the source actor’s name  
2. **Name:** the target actor’s name

If there are multiple people with the same name, the program will show all matching IDs and ask you to choose the intended one.

The output will either be:

- The number of **degrees of separation** and the detailed chain of movies and co‑stars, or
- `Not connected.` if no path exists in the dataset.

---

## Example Interaction

```text
Loading data...
Data loaded.
Name: Kevin Bacon
Name: Tom Hanks
2 degrees of separation.
1: Kevin Bacon and Actor X starred in Movie A
2: Actor X and Tom Hanks starred in Movie B
```

---

## Notes and Possible Improvements

- **Portability:** Replace the hard‑coded `BASE_DIR` in `degrees.py` with a dynamic path using `os.path.dirname(__file__)` so it works regardless of where the repo is located.
- **Performance:** For very large datasets, additional optimizations (like bidirectional BFS) could make searches faster.
- **Interface:** A simple web or GUI front‑end could be built on top of the existing search logic.

---

## Acknowledgements

This project is based on the classic **“Degrees of Separation”** / **“Six Degrees of Kevin Bacon”** problem and uses a graph search (BFS) approach to solve it.


