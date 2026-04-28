# K-Means Clustering — Interactive Demo

An interactive, step-by-step visualization of the **K-means clustering algorithm**, built for teaching. Runs entirely in the browser — no build step, no dependencies, just one HTML file.

**🔗 Live demo:** https://mdszn9.github.io/kmeans-visualization/

---

## What is this?

K-means is one of the foundational clustering algorithms in machine learning. Students often understand the *math* but struggle to picture how the algorithm actually unfolds. This demo lets you:

- **Watch every phase**: clicking *Next Step* alternates between the **Assign** phase (each point joins its nearest centroid) and the **Update** phase (each centroid moves to the mean of its cluster).
- **Compare difficulty**: six built-in datasets ranging from textbook-easy blobs to tricky non-convex moons.
- **Investigate failure modes**: place centroids manually in adversarial spots and watch K-means get stuck in a local minimum — a vivid argument for why **initialization matters** and why **k-means++** exists.

It's designed for an undergrad ML class, but anyone curious about how K-means works can play with it.

---

## How K-means works (the 30-second version)

Given a set of points and a chosen number of clusters K:

1. **Initialize** — pick K starting positions for the centroids.
2. **Assign** — each point joins the cluster of its nearest centroid (Euclidean distance).
3. **Update** — each centroid moves to the *mean* of the points currently assigned to it.
4. **Repeat** steps 2 and 3 until no point changes cluster — that's **convergence**.

K-means minimizes **inertia** (also called WCSS — within-cluster sum of squares): the sum of squared distances from each point to its centroid. The algorithm always converges, but only to a **local** minimum — meaning the starting centroids strongly influence the final result.

---

## Features

### Step control
| Control | What it does |
|---|---|
| **Next Step** | Run *one phase* — either Assign or Update. The status panel tells you which is next. |
| **Full Iteration** | Run a complete Assign + Update pair. |
| **Auto-play** | Run iterations automatically. Speed slider: 100 ms – 1500 ms per phase. |
| **Reset** | Re-run from the same initial centroids. |
| **New Data** | Regenerate the dataset from a fresh seed (preset stays the same). |
| **New Init** | Re-initialize centroids with a fresh seed (or clear manual placements). |

### Datasets (presets)
| Preset | Description | Default K |
|---|---|---|
| **Easy** | 3 well-separated Gaussian blobs | 3 |
| **Medium** | 4 closer blobs with mild overlap | 4 |
| **Hard** | 5 overlapping blobs | 5 |
| **Challenging — elongated** | 3 horizontal stripes (K-means struggles with non-spherical clusters) | 3 |
| **Challenging — uneven** | 4 clusters with very different sizes and densities | 4 |
| **Tricky — moons** | Two interlocking crescents (non-convex — K-means *can't* solve this correctly) | 2 |

### Initialization methods
- **k-means++** (default) — smart seeding that picks each new centroid with probability proportional to its squared distance from the nearest existing one. Usually finds a near-optimal solution.
- **Random** — uniformly random points in the plot area.
- **Forgy** — pick K random *data points* as the starting centroids.
- **Manual — click to place** — drop centroids anywhere by clicking on the canvas. Drag any centroid at any time to reposition (even mid-iteration). Great for showing students how a bad init can trap K-means in a local minimum.

### Live state panel
- **Iteration** count
- Current **phase** (Init / Assign / Update / Done)
- **Inertia** (WCSS) after each Assign step
- **Reassignments** — how many points changed cluster on the last Assign step (when this hits 0, we've converged)

### Visual cues
- Points are colored by their current cluster assignment (gray = unassigned).
- Centroids are large diamonds with the cluster index.
- After a centroid **moves**, a dashed arrow shows where it came from.
- After points are **reassigned**, faint lines connect each point to its centroid (until the next Update).

---

## Suggested teaching flow

1. **Start with Easy.** Let auto-play run. Students see clean convergence in 2–3 iterations.
2. **Switch to Hard.** Run a few times with **New Init** to show K-means landing on different solutions.
3. **Try the moons preset.** Even with k-means++, K-means can't recover the two crescents — a perfect motivation for *why we need other algorithms* (DBSCAN, spectral clustering, etc.).
4. **Use Manual init.** Place all K centroids in one corner. Auto-play. Watch K-means struggle. Now place them at the true cluster centers — converges instantly. The contrast drives home why **initialization matters**.
5. **Drag a converged centroid.** Show that K-means will recover (or not, on hard data) on the next step.

---

## Running locally

The project is a single static HTML file with no build step. Any of these works:

```bash
# With Python
python3 -m http.server 8000

# Or just open the file directly in your browser
open index.html
```

Then visit http://localhost:8000/

---

## Tech notes

- **Vanilla JS, no framework, no dependencies.** ~800 lines total in `index.html`.
- Renders to a single `<canvas>` with HiDPI/Retina-aware scaling.
- Seeded PRNG (mulberry32) so datasets and initializations are reproducible.
- The state machine runs `init → assign → update → assign → update → … → done`. Convergence is detected when an Assign step produces zero reassignments.

---

## Repository

- **Source:** https://github.com/mdszn9/kmeans-visualization
- **Live demo:** https://mdszn9.github.io/kmeans-visualization/
