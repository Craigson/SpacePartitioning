## SpacePartitioning
A collection of c++11 Space Partitioning templates for Cinder

This is a work in progress; currently only ```KdTree``` and ```Bin-lattice / Grid``` are available, but ```HashTable```, ```Octree``` and ```BSPTree``` are on their way.

**Basic Use:**  
Each classes share the same syntax for common tasks.

```c++
// Create a 3d KdTree (use alias for KdTree<3,float>)
sp::KdTree3 kdTree; 

// Insert elements
for( auto node : mNodes ) {
	const vec3& pos = node->getPosition();
	void* userData 	= static_cast<void*>( node );
	kdTree.insert( pos, userData );
}

// Nearest neighbor search
auto nn = kdTree.nearestNeighborSearch( vec3( 0.0f ) );

// Nearest neighbor search and get the distance square to the position
float distSq;
nn = kdTree.nearestNeighborSearch( vec3( 1.0f, 2.0f, 3.0f ), &distSq );

// Range search
auto neighbors = kdTree.rangeSearch( vec3( 0.0f ), 35.0f );
for( auto neighbor : neighbors ) {
	auto node = neighbor.first;
	auto distSq = neighbor.second;
	// ...
}

// Visitor Range search
auto neighbors = kdTree.rangeSearch( vec3( 0.0f ), 35.0f, 
	[]( KdTree2::Node* node, float distSq ) {
		// ...
	} );

```

**Test suite:**  
Running [```test.cpp```](test.cpp) should output something along those lines. It might be usefull to run these tests with your own data as the results might changes drastically according its type and distribution.

```
2d Brute Force

	Range search 				49.102ms		418 neighbors
	Nearest search 				43.73ms			[   -4.055,   -4.027]
_____________________________________________________________________________________________

KdTree2

	Memory Footprint 			38.147mb
	Inserting 1000000 objects 	1502.52ms
	Range Search 				0.344992ms 		418 neighbors
	Range Search lambda 		0.177979ms 		418 neighbors
	Nearest Search 				0.0180006ms 	[   -4.055,   -4.027]
	Clearing structure 			117.568ms
_____________________________________________________________________________________________

Grid2 k = 1

	Memory Footprint 			16.2114mb
	Inserting 1000000 objects 	414.334ms
	Range Search 				0.333965ms 		418 neighbors
	Range Search lambda 		0.0990033ms 	418 neighbors
	Nearest Search 				0.102997ms 		[   -4.055,   -4.027]
	Clearing structure 			81.276ms
_____________________________________________________________________________________________

Grid2 k = 2

	Memory Footprint 			15.497mb
	Inserting 1000000 objects 	413.715ms
	Range Search 				0.414014ms 		418 neighbors
	Range Search lambda 		0.265002ms 		418 neighbors
	Nearest Search 				0.274003ms 		[   -4.055,   -4.027]
	Clearing structure 			90.509ms
_____________________________________________________________________________________________

Grid2 k = 3

	Memory Footprint 			15.3184mb
	Inserting 1000000 objects 	398.919ms
	Range Search 				0.252008ms 		418 neighbors
	Range Search lambda 		0.262022ms 		418 neighbors
	Nearest Search 				0.342011ms 		[   -4.055,   -4.027]
	Clearing structure 			85.196ms
_____________________________________________________________________________________________

Grid2 k = 4

	Memory Footprint 			15.2743mb
	Inserting 1000000 objects 	433.826ms
	Range Search 				0.512004ms 		418 neighbors
	Range Search lambda 		0.286996ms 		418 neighbors
	Nearest Search 				1.08701ms 		[   -4.055,   -4.027]
	Clearing structure 			83.948ms
_____________________________________________________________________________________________

Unbounded-Grid2 k = 1

	Memory Footprint 			16.2114mb
	Inserting 1000000 objects 	7199.5ms
	Range Search 				0.238001ms 		418 neighbors
	Range Search lambda 		0.0879765ms		418 neighbors
	Nearest Search 				0.0680089ms 	[   -4.055,   -4.027]
	Clearing structure 			92.284ms
_____________________________________________________________________________________________



3d Brute Force

	Range search 				43.762ms			4 neighbors
	Nearest search 				47.096ms			[    4.103,    4.016,    5.801]
_____________________________________________________________________________________________

KdTree3

	Memory Footprint 			38.147mb
	Inserting 1000000 objects 	1690.55ms
	Range Search 				0.0470281ms 		4 neighbors
	Range Search lambda 		0.0199676ms 		4 neighbors
	Nearest Search 				0.0140071ms 		[    4.103,    4.016,    5.801]
	Clearing structure 			124.938ms
_____________________________________________________________________________________________

Grid3 k = 1

	Memory Footprint 			217.201mb
	Inserting 1000000 objects 	790.416ms
	Range Search 				0.0939965ms 		4 neighbors
	Range Search lambda 		0.0550151ms 		4 neighbors
	Nearest Search 				0.301957ms 			[    4.103,    4.016,    5.801]
	Clearing structure 			572.092ms
_____________________________________________________________________________________________

Grid3 k = 2

	Memory Footprint 			47.1774mb
	Inserting 1000000 objects 	762.119ms
	Range Search 				0.0430346ms 		4 neighbors
	Range Search lambda 		0.0169873ms 		4 neighbors
	Nearest Search 				0.151992ms 			[    4.103,    4.016,    5.801]
	Clearing structure 			162.023ms
_____________________________________________________________________________________________

Grid3 k = 3

	Memory Footprint 			25.9244mb
	Inserting 1000000 objects 	612.086ms
	Range Search 				0.0349879ms 		4 neighbors
	Range Search lambda 		0.00804663ms 		4 neighbors
	Nearest Search 				0.126004ms 			[    4.103,    4.016,    5.801]
	Clearing structure 			114.062ms
_____________________________________________________________________________________________

Grid3 k = 4

	Memory Footprint 			23.2906mb
	Inserting 1000000 objects 	488.936ms
	Range Search 				0.0550151ms 		4 neighbors
	Range Search lambda 		0.0270009ms 		4 neighbors
	Nearest Search 				0.325024ms 			[    4.103,    4.016,    5.801]
	Clearing structure 			106.216ms
_____________________________________________________________________________________________

Unbounded-Grid3 k = 3

	Memory Footprint 			25.9244mb
	Inserting 1000000 objects 	11005.8ms
	Range Search 				0.0190139ms 		4 neighbors
	Range Search lambda 		0.00500679ms 		4 neighbors
	Nearest Search 				0.0960231ms 		[    4.103,    4.016,    5.801]
	Clearing structure 			117.241ms
_____________________________________________________________________________________________



```