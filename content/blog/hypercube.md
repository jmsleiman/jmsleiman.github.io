---
title: "Hypercube Election"
date: "2015-12-08"
subtitle: "Leader election in an n-HyperCube"
---

# Simulation of a Leader Election in an n-HyperCube

## Demo

Shiny stuff first!

{{< hypercube-demo >}}

### Context

I've been working on this for quite some time, but never had the patience to actually finish it. However, now it's complete. You can find it on [GitHub](https://github.com/jmsleiman/hypercube-election). It's written in python 2 and has few dependencies.

#### Leader Elections

Leader elections are usually applied to distributed systems. A herd of computers on a network are trying to determine who to put in charge, organically and without outside knowledge. They have to communicate amongst each other (and preferably with the least amount of messaging) to determine who will be "Queen" and how will they communicate with the "Queen". This is useful when, for example, a herd of computers need to determine which node will be the authority on database data, or on logging data -- or even to nominate a central point to do these activites.

Other leader elections exist, and could be added onto this utility later, but for now it merely does the "normal" leader elections.

#### n-Hypercube

What is an `n-hypercube`? It's a `hypercube` with *n* nodes.

What is a hypercube? It's a topology where there are nodes, and each node is aware of a dimension *d*>. Every node has *d* connections among its neighbours and has no redundant connections. Essentially, a 2-cube is a square, and a 3-cube is a 3D cube, and a 4-cube is a tesseract.

### The Algorithm

(The algorithm being applied is a slight departure from the one I learned (thanks, [Dr. Paola Flocchini](https://www.site.uottawa.ca/~flocchin/)!. In my variant, nodes update their Queen edge to better serve a better minimum spanning tree.)

1. Message your neighbour on your 0 axis.
	i. Tell them your value, your rank, and challenge them to a duel
2. If you receive a message on any dimension, and you haven't reached the same rank, you hold the message for later.
	i. If you do match rank, or exceed it, read the message.
	ii. If you haven't been defeated, it's for you:
		a. If the value is better than yours, you become defeated, and the origin of the message is your Queen.
		b. If the value is worse than yours, you can rank up.
	iii. If you have been defeated, forward it to your queen.
3. If you increase your rank, you must message the node on the next edge.
4. If a node reaches a rank equal to the dimension of the cube, the election is over, and that node has won.

### Interesting Properties
One interesting consequence is that, because the hypercube is composed of many smaller cubes linked together (a 2-cube is two 1-cubes, a 3-cube is two 2-cubes, 4-cube is two 3-cubes etc), the algorithm actually cuts the number of active nodes in half at each round (where a round is defined as being a round of messaging, not a round of time); if all the link delays were equal, the nodes would resolve the election in `log(n)` amount of time.

### Running the Simulation
```
$ python main.py [d] [choice]
```

Where `d` is the dimensions to use (at very high dimensions, the program might fail to end in a reasonable amount of time), and `choice` indicates the direction of the election: whether high nodes or low nodes win.

Node names, values, and labels are generated randomly. The program simply shuffles through a list of values equal to `0...n` (`n` being the number of nodes, or `2^d`) to assign values, and the labels are just equal to the values. If you're interested in assigning custom values or custom topology, you can do so before calling the election; you can also supply your own list of nodes, if you're ok with the method I used to connect all the nodes.

#### Sample Output
The command I used:

```
hypercube-election$ python main.py 3 high
defender=0 attacker=1 event='duel'
me=0 message=1 queen=1 queen_edge=0 event='defeated'
defender=5 attacker=3 event='duel'
me=5 rank=1 event='deflected'
defender=1 attacker=0 event='duel'
me=1 rank=1 event='deflected'
defender=3 attacker=1 event='duel'
defender=3 attacker=1 event='duel'
defender=2 attacker=6 event='duel'
me=2 message=6 queen=6 queen_edge=0 event='defeated'
defender=3 attacker=1 event='duel'
defender=3 attacker=1 event='duel'
defender=3 attacker=1 event='duel'
defender=3 attacker=1 event='duel'
defender=6 attacker=2 event='duel'
me=6 rank=1 event='deflected'
defender=3 attacker=5 event='duel'
me=3 message=5 queen=5 queen_edge=0 event='defeated'
defender=3 attacker=1 event='duel'
defender=3 queen=5 attacker=1 event='forwarding challenge...'
defender=0 attacker=5 event='duel'
defender=0 queen=1 attacker=5 event='forwarding challenge...'
defender=4 attacker=7 event='duel'
me=4 message=7 queen=7 queen_edge=0 event='defeated'
defender=7 attacker=4 event='duel'
me=7 rank=1 event='deflected'
defender=5 attacker=1 event='duel'
me=5 rank=2 event='deflected'
defender=1 attacker=5 event='duel'
me=1 message=5 queen=5 queen_edge=1 event='defeated'
defender=4 attacker=6 event='duel'
defender=4 queen=7 attacker=6 event='forwarding challenge...'
defender=6 attacker=5 event='duel'
defender=2 attacker=7 event='duel'
defender=2 queen=6 attacker=7 event='forwarding challenge...'
defender=6 attacker=5 event='duel'
defender=6 attacker=5 event='duel'
defender=6 attacker=5 event='duel'
defender=7 attacker=6 event='duel'
me=7 rank=2 event='deflected'
defender=6 attacker=5 event='duel'
defender=6 attacker=5 event='duel'
defender=6 attacker=5 event='duel'
defender=6 attacker=5 event='duel'
defender=6 attacker=5 event='duel'
defender=6 attacker=7 event='duel'
me=6 message=7 queen=7 queen_edge=1 event='defeated'
defender=6 attacker=5 event='duel'
defender=6 queen=7 attacker=5 event='forwarding challenge...'
defender=1 attacker=7 event='duel'
defender=1 queen=5 attacker=7 event='forwarding challenge...'
defender=5 attacker=7 event='duel'
me=5 message=7 queen=7 queen_edge=2 event='defeated'
defender=7 attacker=5 event='duel'
me=7 rank=3 event='deflected'
Winner! Object Node:
Value: 7
Rank: 3
Connections: [&ltnode.Node object&gt, &ltnode.Node object&gt, &ltnode.Node object&gt]
Address: 0x7f925e9a7950
```

#### Formatting from DOT to PNG

A handy way to bulk-compile all the dot to png:

```
hypercube-election/output$ ls | xargs -I {} dot -Tpng {} -o {}.png
```
