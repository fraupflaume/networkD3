D3 JavaScript Network Graphs from R
===================================

Development version: 0.4.9000 [![CRAN
Version](http://www.r-pkg.org/badges/version/networkD3)](https://CRAN.R-project.org/package=networkD3)
[![Build
Status](https://travis-ci.org/christophergandrud/networkD3.svg?branch=master)](https://travis-ci.org/christophergandrud/networkD3)
![CRAN Monthly
Downloads](http://cranlogs.r-pkg.org/badges/last-month/networkD3) ![CRAN
Total Downloads](http://cranlogs.r-pkg.org/badges/grand-total/networkD3)

This README includes information on set up and a number of basic
examples. For more information see the package's [main
page](http://christophergandrud.github.io/networkD3/).

----

**** Difference between this fork and the original repo ****
----

><span style="color: #003b70;">This fork includes the parameter `linkStrength` in the function `forceNetwork`. This is a parameter that will allow the user to create more uniform networks, by reducing the power in the other forces (primarily `charge`).</span>

----


Usage
-----

Here's an example of `simpleNetwork`:

    library(networkD3)

    # Create fake data
    src <- c("A", "A", "A", "A", "B", "B", "C", "C", "D")
    target <- c("B", "C", "D", "J", "E", "F", "G", "H", "I")
    networkData <- data.frame(src, target)

    # Plot
    simpleNetwork(networkData)

Here's `forceNetwork`:

    # Load data
    data(MisLinks)
    data(MisNodes)

    # Plot
    forceNetwork(Links = MisLinks, Nodes = MisNodes, Source = "source",
                 Target = "target", Value = "value", NodeID = "name",
                 Group = "group", opacity = 0.7,
                 colourScale = JS("d3.scaleOrdinal(d3.schemeCategory20);"))

Here's `sankeyNetwork` using a downloaded JSON data file:

    # Recreate Bostock Sankey diagram: http://bost.ocks.org/mike/sankey/
    # Load energy projection data
    URL <- paste0("https://cdn.rawgit.com/christophergandrud/networkD3/",
                  "master/JSONdata/energy.json")
    Energy <- jsonlite::fromJSON(URL)

    # Plot
    sankeyNetwork(Links = Energy$links, Nodes = Energy$nodes, Source = "source",
                 Target = "target", Value = "value", NodeID = "name",
                 units = "TWh", fontSize = 12, nodeWidth = 30)

### Interacting with igraph

You can use [igraph](http://igraph.org/r/) to create network graph data
that can be plotted with **networkD3**. The `igraph_to_networkD3`
function converts igraph graphs to lists that work well with
**networkD3**. For example:

    # Load igraph
    library(igraph)

    # Use igraph to make the graph and find membership
    karate <- make_graph("Zachary")
    wc <- cluster_walktrap(karate)
    members <- membership(wc)

    # Convert to object suitable for networkD3
    karate_d3 <- igraph_to_networkD3(karate, group = members)

    # Create force directed network plot
    forceNetwork(Links = karate_d3$links, Nodes = karate_d3$nodes, 
                 Source = 'source', Target = 'target', NodeID = 'name', 
                 Group = 'group')

### Saving to an external file

Use `saveNetwork` to save a network to stand alone HTML file:

    library(magrittr)

    simpleNetwork(networkData) %>% saveNetwork(file = 'Net1.html')

Note
----

networkD3 began as a port of
[d3Network](http://christophergandrud.github.io/d3Network/) package to
the [htmlwidgets](https://github.com/ramnathv/htmlwidgets) framework.
d3Network is no longer supported.
