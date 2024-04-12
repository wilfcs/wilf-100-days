DSA ->

Trapping Rain Water

Number of Good Paths

Collect Coins in a Tree

Modify Graph Edge Weights

Reconstruct Itinerary

Longest Increasing Path in a Matrix

HLD ->

Introduction
- Location databases are used in many real-world applications like Google Maps, food delivery apps, and taxi services
- Early attempt: Using pincodes/zipcodes to assign locations, but this has limitations in measuring actual distances between points

Requirements for Location Databases
1. Ability to measure distance between any two points accurately
2. Uniform representation of locations 
3. Ability to divide regions into smaller sub-regions (scalability)
4. Find points within a given proximity/radius

Using Coordinates (Latitude, Longitude)
- Allows measuring Euclidean distance between points
- Granularity is scalable by adding more decimal places
- But finding proximity is inefficient (checking all points)

QuadTrees
- Recursive spatial data structure that divides 2D space into 4 quadrants 
- Splits regions into 4 child nodes until desired granularity
- Efficient for range queries if data is uniformly distributed
- But inefficient if data is heavily skewed/clustered

Converting 2D to 1D 
- Range queries are efficient in 1D (using segment trees, interval trees)
- Can map 2D plane to 1D line while preserving proximity

Hilbert Curve
- Fractal space-filling curve that maps 2D plane to 1D line
- Preserves locality by mapping nearby 2D points to nearby 1D points
- Recursive construction allows arbitrary precision
- Minor locality issues at boundaries, but works well in practice

Advantages
- Efficient range and proximity queries using 1D representation
- Enables applications like food delivery, taxi services, dating apps etc.