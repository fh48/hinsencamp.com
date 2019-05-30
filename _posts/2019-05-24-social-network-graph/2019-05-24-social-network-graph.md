---
title: "Build a Social Network in JavaScript with Graphs!"
description: "Learn how to build a graph data structure to power a social network 🌐 in JavaScript."
author: "Fabian Hinsenkamp"
published_at: 2019-05-24 08:06:30.486476
categories: [javascript, node.js, tutorial]
---

> _How would you design the data structure for a very large social network like Facebook or Linkedin?_

Such kind of question is known to be asked by top-tier tech companies like
Google, Amazon, Facebook and Linkedin as part of their recruitment process.

The reason is, that social networks are a great use case for graph data structures. In this tutorial, we will dive into the topic with an hands-on example and build a social network ourselves! Thereby, we will learn how a graph works and why it's such an important and powerful data structure.

The tutorial is also suitable for beginners, the only pre-requirement is a basic understanding of object-oriented JavaScript. If you want to read some about graph theory up-front, check the additional resources, in the [resources section](#resources) at the bottom of this article.

Later on, we use some helper functions, you find them with the rest of the code in this [repo](https://github.com/fh48/tutorial_SocialNetworkGraph);

Let's start with getting a basic understanding of what we actually want to achieve!

## What is a Social Network at its Core?

When we try to describe what's a social network at its core, we quickly end up talking about users and connections between them.
Typically users have some kind of connection to other users. Even though theoretically millions of connections are possible, most users don't have more than a couple hundred connections. To put it differently, users don't have connections to most other users in the network.
Just think about it. How many friends do you have on Facebook compared to the number of existing profiles worldwide? Friendship circles are a common pattern, these consist of a limited amount of users sharing many common connections.

Now, after having thought of the basic interaction of users in social networks, we can start building a data structure that allows us to easily implement these requirements. In the next section you will see why the graph data structure is a great fit for this problem.

## Why Graphs?

Simply put, graphs are nothing but a collection of nodes and edges which connect them. In the books, you find nodes often also called vertices. In general, nodes can represent any kind of abstract data object. In the context of a social network, it's obvious to represent users by nodes. But also other abstract entities like groups, companies, events, etc. can be modeled as nodes.

The connections between nodes are called Edges. It exists a range of different types of edges, which allow you to model all kinds of relations between nodes. Read the article _Graph Data Structures for Beginners_ by [@amejiarosario](https://twitter.com/amejiarosario) to learn more about the differences between directed, undirected, cyclic and acyclic graphs. You find the link in the [resources section](#resources).

What do you think? Sounds promising, right? Let's dive right into building a graph and see if it's actually as good.

## Create the Graph

[Above](#A-social-network-at-its-core), we have figured out what's the core functionality of a social network. To represent this we will build a graph with nodes representing users and bi-directional edges to model the equal connection between users.

We implement the graph in an object-oriented fashion. Therefore, we start writing a `Graph` constructor function which contains an empty object as it's only property.

```javascript
function Graph() {
  this.graph = {};
}
```

Now To continue the implementation, we add `getter` and `setter` methods to our graph. To add a node, we simply add the user as a key-value pair to the `graph` object and use the user's name as the key. Note, in production unique ids would the better choice.

```javascript
Graph.prototype.addUser = function(user) {
  this.graph[user.name] = user;
};
```

For the `getter` method we simply return the user which we retrieve by the name passed as properties.

```javascript
Graph.prototype.getNode = function(name) {
  return this.graph[name];
};
```

Next, we create the Node constructor function.

## Create Nodes

The constructor function for the nodes comes only with a name and a friends property.

```javascript
function Node(user) {
  this.name = user.name;
  this.friends = {};
}
```

In general, there are two approaches to how graphs can represent nodes and their relations to each other.

The first approach, which we will apply here is called `adjacency list` and relies on a list, kept by each individual node, storing all of the node's edges.

```javascript
a -> { b c }
b -> { a d }
c -> { a }
d -> { b c }
```

The second approach is called `adjacency matrix`. Such are especially useful for complex (directed and weighted edges) and highly dense graphs. Read more about the benefits of each representation in _When are adjacency lists or matrices the better choice?_ you find the link in the [resources section](#resources).

The `friends` property acts as our `adjacency list` and stores all connected users. We could simply use an array or a set to store the connections' names.
However, an object is more performant as we will need to check for already existing connections when we create an edge.

## Create Edges

The last missing piece to complete the basic network, is a method to add connections between nodes. As we decided on bidirectional edges, we need to add the connection to both involved nodes. To do so, we call `addConnection` within itself with the user's node we want to connect with.

```javascript
Node.prototype.addConnection = function(user) {
  if (!this.friends[user.name]) {
    this.friends[user.name] = { name: user.name };
    user.addConnection(this);
  }
};
```

Thanks to the condition which wraps the actual logic, we do not end up in an infinite loop. Having all this in place, we can actually start adding users to our network!

## Grow the Network!

To start our network, let's create a couple of nodes and connect them. Therefore, we first create a couple of nodes.

```javascript
const fabian = new Node({ name: "Fabian" });
const rey = new Node({ name: "Rey" });
const ellie = new Node({ name: "Ellie" });
const cassi = new Node({ name: "Cassi" });
```

Next, we instantiate a graph and add the nodes to it.

```javascript
const graph = new Graph();

graph.addNode(fabian);
graph.addNode(rey);
graph.addNode(ellie);
graph.addNode(cassi);
```

In the final step, we connect nodes to each other.

```javascript
graph.get("Fabian").addConnection(graph.get("Rey"));
graph.get("Fabian").addConnection(graph.get("Ellie"));
graph.get("Fabian").addConnection(graph.get("Cassi"));

graph.get("Ellie").addConnection(graph.get("Cassi"));
```

You can use the helper function `writeToJSON` to export your graph to a json to get a better overview. In this [repo](https://github.com/fh48/tutorial_SocialNetworkGraph) you can find it.

```javascript
writeToJSON(graph.graph, "graph");
```

Pretty cool, right?

## Visualise the Network!

If you want to see the result, just push the button and check how the result looks like.

<div class="graph-visualiser">
  <div class="wrapper">
      <textarea
        class="graph-text-area form-control"
        id="graphdata"
        rows="4"
        cols="50"
      >
      </textarea>
      <button id="load-graph-btn" class="load-graph-btn btn btn-primary text-light" onclick="loadData()">Visualise Graph</button>
      <div class="canvas-wrapper">
      <svg class="canvas" width="200" height="200"></svg>
      </div>
    </div>
    <script src="https://d3js.org/d3.v4.min.js"></script>
    <script src="index.js"></script>
  </div>

As a next step, you should run another helper function - the network generator. It generates random networks with up to 150 users.

```javascript
generateRandomNetwork(graph, 10);

writeToJSON(graph.graph, "graph");
```

Play around with the number of participants. You will see, with increasing network size it becomes quickly very complicated to keep an overview by just looking at the JSON object. For a better overview, you can drop the JSON object in the visualiser as well.

## Conclusion

We have built the initial data structure for a social network. Therefore, we have created constructors for the graph and nodes representing users. Moreover, we added edges connecting these nodes bi-directional. This structure represents a solid foundation to build more powerful features on top of it. Here are some hints of what could be added next:

- Methods to delete edges and nodes
- Different types of nodes like "groups" or "companies"
- Search Algorithms like Breadth-first search (BFS)
- Recommend users new friends by comparing sets of edges.

Let me know what interests you the most on twitter [@hinsencamp](https://twitter.com/hinsencamp)! Based on your feedback, I will choose the next tutorial topic.
When you are interested in going into production with a graph-based solution you should consider reading more about
graph databases, which provide many features of graphs out of the box. It's worth having a look at the following free graph databases [Neo4J](https://neo4j.com/), [OrientDB](https://orientdb.com/) and [GunDB](https://github.com/amark/gun).

## Resources

- [The Javascript Developer’s Guide to Graphs and Detecting Cycles in Them](https://hackernoon.com/the-javascript-developers-guide-to-graphs-and-detecting-cycles-in-them-96f4f619d563)
- [When are adjacency lists or matrices the better choice?](https://cs.stackexchange.com/questions/79322/when-are-adjacency-lists-or-matrices-the-better-choice)
- [Graph Data Structures for Beginners](https://adrianmejia.com/blog/2018/05/14/data-structures-for-beginners-graphs-time-complexity-tutorial/)
- [Using Graph Theory to Build a Simple Recommendation Engine in JavaScript](https://medium.com/@keithwhor/using-graph-theory-to-build-a-simple-recommendation-engine-in-javascript-ec43394b35a3)
- [What is the JavaScript equivalent to a C# HashSet?](https://stackoverflow.com/questions/24196067/what-is-the-javascript-equivalent-to-a-c-sharp-hashset)
