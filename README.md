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

Looking at the map below in Provence, one can however feel disappointed: the extent of the yellow community (centered around the urban area of Marseille - Aix-en-Provence) is such that it spills over three *départements*. Dissolving the *département* in a Metropolitan Area has indeed been done around Lyon. This yellow region would however encompass cities that have little to do with each other beyond some commuting.

![alt-style](https://github.com/amapsa/Commuting_Networks_FR/blob/master/Figures/Marseille_map.jpeg)

---

In the end, though focusing solely on commuting patterns is obviously too narrow, it is however unlikely that we would today be able to encompass all relevant variables in a single algorithmic detection. It may bring down a dream of administrative rationalization, but it should not prevent us from using data as a guiding tool in the discussions to come.

As for the best level to deal with commuting problems, there may often be no possibility of a single entity large and consistent enougn. It remains that there are instances where transportation authorities could better coordinate themselves.

[Gleich]: https://arxiv.org/abs/1407.5107
