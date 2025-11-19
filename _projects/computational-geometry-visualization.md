---
title: Computational Geometry Visualization
desc: Tool for the visualization of algorithms in computational geometry. Topics include convex hulls, line segment intersection, triangulation, range queries, and Voronoi diagrams.
date: 2025-11-05
language: en
ongoing: true
github: https://github.com/jan-keuchel/Computational-Geometry-Visualization
---
{% comment %}wallpaper: /assets/images/project_wallpapers/computational-geometry-visualization.png{% endcomment %}

## About this project 
In parallel with the lecture "Grafisch-geometrische Algorithmen", or, in English, "Computational Geometry" (lecture notes can be found [here]({% link /assets/lecture_notes_pdf/cg.pdf %})), I am working through the accompanying book [Computational Geometry](https://link.springer.com/book/10.1007/978-3-540-77974-2).
To test my understanding, I wanted to try to visualize some of the algorithms covered in the lecture and book.
This page is dedicated to said project and contains videos of those algorithms.
The source code of the project can be found on [GitHub](https://github.com/jan-keuchel/Computational-Geometry-Visualization).

## Convex Hull
The first problem encountered deals with calculating the convex hull of a given set $P$, which consists of $n$ points in 2D space.

### Brute Force approach
As it is better to have a suboptimal algorithm than none at all, the first approach is -- as so often -- to try and brute force the solution.
The algorithm checks for every possible directed edge, whether every other point is to the left of it.
If so, the edge is added to a list of convex hull edges $E$.
Later on, the convex hull $\mathcal{CH}(P)$ -- which is an ordered list of vertices in counter-clockwise direction -- is extracted from $E$.

This is really slow and takes $$\mathcal{O}(n^3)$$ time.
<video controls="" preload="none">
<source src="/assets/videos/CH_brute_force.mp4" type="video/mp4">
</video>

### Monotonous Chains
The next algorithm is already substantially faster -- see the lecture notes or the book for a detailed explanation. It takes only $$\mathcal{O}(n \log{n})$$ time.
<video controls="" preload="none">
<source src="/assets/videos/CH_graham_scan.mp4" type="video/mp4">
</video>

### Jarvis' March
The Jarvis' March algorithm starts from point $p \in \mathcal{CH}(P)$ -- e.g., the leftmost point.
From there, it brute forces the next point in the convex hull, essentially walking a perimeter around $P$.
The runtime of this algorithm depends on the output size and is therefore called an *output-sensitive* algorithm.

With $k$ vertices in the convex hull, it takes $\mathcal{O}(k \cdot n)$ time.
<video controls="" preload="none">
<source src="/assets/videos/CH_jarvis_march.mp4" type="video/mp4">
</video>
