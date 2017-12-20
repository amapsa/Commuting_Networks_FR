# COMMUTING NETWORKS IN FRANCE
###### December 2015

Stuck in your car in Houston, in your subway in Paris, or on your scooter in Shanghai, you wonder why everyone always has to be at the same place at the same time. You wish (maybe) that there was a benevolent authority which could organize economic activity so that commuting were more bearable.

But assuming that such planning was possible and stable (it probably is not), at what level should it be done ? At the county level, the metropolitan area level, the region level ?

Even within the bounded objective of rationalizing commuting patterns, there is no straightforward answer. But data might save us all. As people pendulate from their flat to their work, and from their workplace back to their home, they draw a network. Applying some graph theory to those networks, we can detect communities linked by strong intra-commuting flows.

This is partly what I attempted to do in this project, realized at the Center for Urban Science and Progress in December 2015 with Dr. Tim Savage.

---
## Data

A network is composed of nodes linked by edges :
  * Nodes here are cities, but as city boundaries can lag economic development outside of them, I use the *unités urbaines* (urban units) defined by the French statistical institute INSEE
  * Edges are commuting flows between the urban units, they are directed (not symmetrical), and weighted by the number of daily commuters, from INSEE source
  * Distance and duration are pulled from the Google Maps API, so they are defined relative to the centroid of the urban units, and may hide some fine-grain dynamics (people from the Northern suburbs of Paris working in the capital possibly work in its Northern part)

## Network structure

Nodes in a network are usually described by their degree. In our case, in-degree is the number of people commuting to the urban unit from outside, while out-degree qualifies the reverse. 

As seen below, the number of people commuting to cities (in-degree) has a much bigger tail than the number of people leaving them (out-degree) : a few urban units are very attractive, while most of them attract few workers. Workplace is more concentrated than residency.

![alt-style](https://github.com/amapsa/Commuting_Networks_FR/blob/master/Figures/inout_degree_ink.jpg)

From those degrees, one can infer the centrality of a node in the network. Following [D. Gleich][Gleich], pagerank centrality is often used as it takes into account that an area is more "central" if it is connected to other well-connected areas. 
Unsurprisingly, the top ten most central nodes are the cores of the Metropolitan Areas in France. The legacy of French centralization of power is also visible in the score of Paris, well ahead any other urban unit.

| Rank | Urban Unit   | Pagerank Score |
|:----:|:------------:|:--------------:|
| 1    | Paris        | 4.41           |
| 2    | Toulouse     | 1.97           |
| 3    | Strasbourg   | 1.92           |
| 4    | Nantes       | 1.84           |
| 5    | Rennes       | 1.81           |
| 6    | Marseille - Aix-en-Pce | 1.79 |
| 7    | Montpellier | 1.76            |
| 8    | Bordeaux    | 1.52            |
| 9    | Lyon        | 1.44            |
| 10   | Lille       | 1.33            |

Note: scores have been multiplied by 100 for readability

## Community detection

Within graph theory, a community is defined as a group of nodes strongly connected between themselves , and (relatively) weakly connected to the rest of the network.

I try both the Louvain and the Combo method for community partition, and both give a modularity score above 0.9. Given that this score indicates the strength of connection within communities, and that the score is bounded between -1 and 1, we can safely say that the communities it detected are very strongly connected.

The overall modularity score [Appendix C] is 0.947, which indicates a set of internally strongly connected communities (given that the score is bounded between -1 and 1). The superposition between the detected communities and some of the new Metropolitan Areas shows that, despite their size, and ambition, the new areas fall short to regrouping all the Urban Units within a “commuting community” [Map 1].

![alt-style](https://github.com/amapsa/Commuting_Networks_FR/blob/master/Figures/Marseille_map.jpeg)

---

In the end, for all the complexity that multiple administrative boundaries may bring, we probably cannot design a single (though there are instances where we could do better - I am looking at you New York and Paris).

[Gleich]: https://arxiv.org/abs/1407.5107
