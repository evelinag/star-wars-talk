- title : The F#orce awakens
- description : Exploring the Star Wars social network with F#.
- author : Evelina Gabasova
- theme : white
- transition : none

******************************************************

- data-background : #000000

# Warning

Contains *some* spoilers for episodes I - VII <br/>

--------

- data-background : images/StarWarsIntro-full-noloop.gif

' Main message: F# is good for data science
' - parsing script files
' - displaying the network with D3.js
' - interoperability with R
' - type providers - swapi
' - type providers for neo4j? Fill database with swapi, then perform queries
' - why even do this - fun interpretable data - find how methods perform & 
' compare -> then use them on more complex data (some cancer example)
' - other applications of the same methods
'    - type providers for data sources - show how to access data sources in other languages? python, R, scala?
'     - polyglot programming - combination with R
'    - network analysis - social network analysis on Twitter, biology, distribution networks

' Takeaway points:
' - F# is a good language for data science - allows good combination with other methodologies
' + real-world data science is messy
' + how to write a parser in a nice way
' + principles of network analysis

' The Galactic Republic is confused. Not only are the Star Wars scripts available just in poorly formatted HTML format, but there are also characters who are important in the films but they do not speak in the scripts at all! The Jedi Council has summoned a young Jedi knight Evelina to help the republic. The F#orce is strong with her but her task is not simple. She has to parse the scripts, extract the Star Wars social network and finally answer the most important question in the galaxy: Why were the prequels crap?

----------
- data-background : #000000

# <div class="sw"> the star wars </div>
# <div class="sw"> social network</div>

' about me
' what I was trying to do
' example of several programming tasks & how to solve them

------

![script structure](images/script-example.png)

' This structure is highly formalised, names have to be in boldface and centered etc.

-----

![imsdb](images/imsdb.png)

-----

- data-background : #212d30

# Parsing scripts

    [lang=html]
    <b> INT. MOS EISLEY SPACEPORT - DOCKING BAY 94 </b>

    Chewbacca leads the group into a giant dirt pit that is 
    Docking Bay 94. Resting in the middle of the huge hole is a 
    large, round, beat-up, pieced-together hunk of junk that 
    could only loosely be called a starship.

    <b> LUKE </b>
    What a piece of junk.

    The tall figure of Han Solo comes down the boarding ramp.

-----
- data-background : #212d30

# Parsing scripts

    open FSharp.Data

    let url = "http://www.imsdb.com/scripts/Star-Wars-A-New-Hope.html"
    let episodeHtml = 
        HtmlDocument.Load(url).Descendants("pre") |> Seq.head

    let boldItems = 
        episodeHtml.Elements("b") 
        |> List.map (fun x -> x.InnerText().Trim())

-----
- data-background : #212d30

# Parsing scripts

<img src="images/parsing-code-original.png" style="width: 960px" />


-----
- data-background : #212d30

# Parsing scripts

    let rec parseScenes sceneAcc characterAcc (items: string list) =
       match items with
       | item::rest ->
           match item with
           | SceneTitle title -> 
                // add the finished scene to the scene accumulator
                let fullScene = List.rev characterAcc
                parseScenes (fullScene::sceneAcc) [] rest
           | Name name -> 
                // add character's name to the character accumulator
                parseScenes sceneAcc (name::characterAcc) rest
           | Word -> // do nothing
                parseScenes sceneAcc characterAcc rest
       | [] -> List.rev sceneAcc     

-----
- data-background : #212d30

# Parsing with active patterns

    let (|SceneTitle|Name|Word|) (text:string) =
        let scenePattern = "[ 0-9]*(INT.|EXT.)[ A-Z0-9]"
        let namePattern = "^[/A-Z0-9]+[-]*[/A-Z0-9 ]*[-]*[/A-Z0-9 ]+$"
        if Regex.Match(text, scenePattern).Success then
            SceneTitle text
        elif Regex.Match(text, namePattern).Success then
            Name text
        else Word

-----
- data-background : #212d30

# (| Active patterns |)

hide complexity behind readable code

-------

- data-background : images/trashcompactor-loop3.gif

-------

![script structure - ugly](images/script-example2.png)

-------

- data-background : images/r2d2beeps-loop3.gif

--------

![R2-D2](images/artoo.png)

--------

![Chewbacca](images/chewie.png)


--------

![](  images/equations1.png)

--------

![](images/equations2.png)

--------

- data-background : images/darth-maul-loop3.gif

--------

- data-background : images/lonelyluke-loop3.gif

--------

![Tion Medon 1](images/tion-medon.png)

--------

<img src="images/tion-medon-closeup.png" style="width: 900px" />

--------

# List of Star Wars characters

[swapi.co](https://swapi.co/)

----------

- data-background : #000000

![swapi](images/swapi.png)

----------

- data-background : #000000

![swapi-langs](images/swapi-langs.png)

----------

- data-background : images/darthvader-darkside-loop3.gif

----------

- data-background : images/darthvader-nope-loop3.gif

----------

- data-background : images/csharp-swapi.gif

----------

- data-background : #000000

# <div class="sw">fsharp-swapi</div>

[github.com/evelinag/fsharp-swapi](https://github.com/evelinag/fsharp-swapi)

[swapi.co](https://swapi.co/documentation)


----------

# <div class="sw">visualization</div>


----------

![](images/d3js.png)

' Exporting and importing the network with Json?

----------

- data-background : images/networks/full_network-darth-vader.png

<a href="images/networks/interactions-merged.html" style="color: transparent;"> Big link to full network <br /> Big link to full network <br />Big link to full network </a>

----------

# <div class="sw">the phantom menace</div>

<a href="images/networks/episode1-interactions.html"><img src="images/networks/episode1.png" style="height:600px"/> </a>


----------

# <div class="sw">a new hope</div>

<a href="images/networks/episode4-interactions.html"><img src="images/networks/episode4.png" style="height:600px"/> </a>


----------

# <div class="sw">the force awakens</div>

<a href="images/networks/episode7-interactions.html"><img src="images/networks/episode7_network.png" style="height:600px"/> </a>

----------

# <div class="sw">network analysis</div>

Who is the most central character?

- Degree
- Betweenness

-----
# Degree
![Network](images/network-basic.png)

-----
# Degree
![Degree](images/degree1.png)

-----
# Degree
![Degree](images/degree2.png)

------
# Degree

<br />

$$$
\text{Degree}(v) = \text{Number of links }v \leftrightarrow v' \\
v \neq v'



-----
# Betweenness
![Betweenness](images/network-basic.png)

-----
# Betweenness
![Betweenness](images/betweenness1.png)

-----
# Betweenness
![Betweenness](images/betweenness2.png)

-----
# Betweenness
![Betweenness](images/betweenness3.png)

-----
# Betweenness
![Betweenness](images/betweenness4.png)

-----
# Betweenness

<br />

$$$
S_v = \text{Number of shortest paths between $a$ and $b$ through $v$} \\
S = \text{Number of shortest paths between $a$ and $b$} \\ \\
\text{Betweenness}(v)_{ab} = \frac{S_v}{S}

-----
# Betweenness

<br />

$$$
S_v = \text{Number of shortest paths between $a$ and $b$ through $v$} \\
S = \text{Number of shortest paths between $a$ and $b$} \\ \\
\text{Betweenness}(v) = \sum_{ab} \frac{S_v}{S}

-----
- data-background : #212d30

# R

    [lang=R]
    library(igraph)

    centrality <- betweenness(network)


-----
- data-background : #212d30

# F#

    open RProvider.igraph

    let centrality = R.betweenness(network)


-----
- data-background : #212d30

# <div class="sw"> Star Wars </div>
# <div class="sw"> Degree and betweenness</div>

-----

<table>
<tr>
  <td class="noborder">

## Original trilogy

| | Name | Degree |
|---|-----|-----|
| 1. | LUKE | 26 |
| 2. | C-3PO | 20 |
| 3. | LEIA | 19 |
| 4. | HAN | 16 |
| 5. | DARTH VADER | 16 |

</td>
  <td class="noborder">

## Prequels 

| | Name | Degree |
|---|-----|-----|
| 1. | ANAKIN | 40 |
| 2. | PADME | 34 |
| 3. | OBI-WAN | 32 |
| 4. | QUI-GON | 27 |
| 5. | JAR JAR | 24 |

  </td>
  </tr>
</table>

-----

<table>
<tr>
  <td class="noborder">

## Original trilogy

| | Name | Betweenness |
|---|-----|-----|
1. | LUKE | 108.6 |
2. | C-3PO | 68.5 |
3. | VADER | 56.2 |
4. | LEIA | 45.0 |
5. | HAN | 30.2 |

</td>
  <td class="noborder">

## Prequels 

| | Name | Betweenness |
|---|-----|-----|
1. | OBI-WAN | 210.4 |
2. | PADME | 209.4 |
3. | QUI-GON | 140.7 |
4. | EMPEROR | 103.2 |
5. | JAR JAR | 87.1 |

  </td>
  </tr>
</table>

-----

- data-background : images/jarjar-loop3.gif

------

# <div class="sw"> the force awakens </div>

<table>
<tr>
  <td class="noborder">

| | Name | Degree |
|---|-----|-----|
| 1. | POE | 16 |
| 2. | FINN | 14 |
| 3. | HAN | 14 |
| 4. | CHEWBACCA | 12 |
| 5. | BB-8 | 12 |

</td>
  <td class="noborder">

| | Name | Betweenness |
|---|-----|-----|
1. | KYLO REN | 35.5 |
2. | POE | 20.3 |
3. | FINN | 14.0 |
4. | HAN | 14.0 |
5. | REY | 13.5 |

</td>
</tr>
</table>

-----

- data-background : images/rey-loop3.gif

------

- data-background : #000000

# <div class="sw"> who is the most central? </div>

------

- data-background : images/obi-wan-loop3.gif

-----

- data-background : images/c3po-loop3.gif


-----
# Network structure

How do the the movies differ?

- Size
- Density
- Clustering coefficient

-----
# Size

<script type="text/javascript" src="https://www.google.com/jsapi"></script>
<script type="text/javascript">
    google.load("visualization", "1", {packages:["corechart"]})
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 38}]}, {"c" : [{"v": "Episode 2"}, {"v": 33}]}, {"c" : [{"v": "Episode 3"}, {"v": 25}]}, {"c" : [{"v": "Episode 4"}, {"v": 22}]}, {"c" : [{"v": "Episode 5"}, {"v": 21}]}, {"c" : [{"v": "Episode 6"}, {"v": 20}]}, {"c" : [{"v": "Episode 7"}, {"v": 27}]}]});
    var options = {"colors":["#c43b80"],"hAxis":{"title":"Number of characters","viewWindowMode":"explicit","viewWindow":{"max":40,"min":0}},"legend":{"position":"none"},"title":"Number of characters","width":1000,"height":600} 
    var chart = new google.visualization.BarChart(document.getElementById('55ec49cf-f2ee-4e40-ad93-2fb5e6943f7b'));
    chart.draw(data, options);
}
</script>
<div id="55ec49cf-f2ee-4e40-ad93-2fb5e6943f7b" style="width: 1200px; height: 600px"></div>

-----
- data-background : images/senate.jpeg

-----
# Density
![Network](images/network-basic.png)

-----
# Density
![Network](images/full.png)

-----
# Density

<br />

$$$
\begin{align}
\text{Density} &= \frac{\text{Existing connections}}{\text{Potential connections}} \\
& \\
&= \frac{\text{Existing connections}}{\frac{1}{2}N(N-1)}
\end{align}



-----
# <div class="sw"> Density </div>

<script type="text/javascript">
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 9.60170697012802}]}, {"c" : [{"v": "Episode 2"}, {"v": 9.56439393939394}]}, {"c" : [{"v": "Episode 3"}, {"v": 12.8458498023715}]}, {"c" : [{"v": "Episode 4"}, {"v": 14.2857142857143}]}, {"c" : [{"v": "Episode 5"}, {"v": 13.0952380952381}]}, {"c" : [{"v": "Episode 6"}, {"v": 16.0818713450292}]}, {"c" : [{"v": "Episode 7"}, {"v": 13.1054131054131}]}]});
    var options = {"colors":["#3bc4c4"],"hAxis":{"title":"Density (%)","viewWindowMode":"explicit","viewWindow":{"max":18,"min":5}},"legend":{"position":"none"},"title":"Network density","width":1000,"height":600} 
    var chart = new google.visualization.BarChart(document.getElementById('b7f86168-0c86-4bf3-9adc-547c80d64440'));
    chart.draw(data, options);
}
</script>
<div id="b7f86168-0c86-4bf3-9adc-547c80d64440" style="width: 1200px; height: 600px"></div>

-----
# Clustering coefficient
![Network](images/network-basic.png)

-----
# Clustering coefficient
![Clustering](images/clustering1.png)

-----
# Clustering coefficient
![Clustering](images/clustering2.png)

-----
# Clustering coefficient
![Clustering](images/clustering3.png)

-----
# Clustering coefficient
![Clustering](images/clustering4.png)

-----
# Clustering coefficient
![Clustering](images/clustering5.png)

-----
# Clustering coefficient

<br />

$$$
K_v = \text{Number of neighbours of $v$} \\
E_v = \text{Number of links between neighbours of $v$} \\ \\
\text{Clustering}(v) = \frac{E_v}{\frac{1}{2} K_v (K_v - 1)}

-----
# Clustering coefficient

<br />

$$$
K_v = \text{Number of neighbours of $v$} \\
E_v = \text{Number of links between neighbours of $v$} \\ \\
\text{Clustering}(\text{network}) = \frac{1}{N} \sum_v \frac{E_v}{\frac{1}{2}  K_v (K_v - 1)}

-----

# <div class="sw"> Clustering coefficient </div>

<script type="text/javascript">
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 0.447572132301196}]}, {"c" : [{"v": "Episode 2"}, {"v": 0.486666666666667}]}, {"c" : [{"v": "Episode 3"}, {"v": 0.498947368421053}]}, {"c" : [{"v": "Episode 4"}, {"v": 0.559808612440191}]}, {"c" : [{"v": "Episode 5"}, {"v": 0.604651162790698}]}, {"c" : [{"v": "Episode 6"}, {"v": 0.656992084432718}]}, {"c" : [{"v": "Episode 7"}, {"v": 0.588387096774194}]}]});
    var options = {"hAxis":{"title":"Clustering coefficient"},"legend":{"position":"none"},"title":"Clustering coefficient (transitivity)","width":1000,"height":600} 
    var chart = new google.visualization.BarChart(document.getElementById('a84d0fe6-8cc8-4351-a397-114a1bc9ff0c'));
    chart.draw(data, options);
}
</script>
<div id="a84d0fe6-8cc8-4351-a397-114a1bc9ff0c" style="width: 1200px; height: 600px"></div>


-----
- data-background : #212d30

# R

    [lang=R]
    library(igraph) 

    density <- graph_density(network)
    clusteringCoef <- transitivity(network, "undirected")


-----
- data-background : #212d30

# F#

    open RProvider.igraph

    let density = R.graph_density(network)
    let clusteringCoef = R.transitivity(network, "undirected")
        

' The point is that data science is very polyglot
' and there's no point in re-implementing algorithms
' over and over again. Just use your favourite language
' and call other languages from it!

----------
- data-background : #212d30

# Google Charts with XPlot

    open XPlot.GoogleCharts

    densities
    |> Array.mapi (fun i c -> "Episode " + string (i+1), c * 100.0 )
    |> Chart.Bar

----------

# Star Wars network in Neo4j

![](images/neo4j.png)

----------
# Network analysis in the real world

<table>
<tr>
  <td class="noborder">

![F# network](images/fsharp-network.png)

</td>
<td class="noborder">

## Central accounts

1. @dsyme
2. @VisualFSharp
3. @migueldeicaza
4. @tomaspetricek
5. @c4fsharp 

<font size="5">November 2014</font>

</td>
</tr>
</table>

-----
# Network analysis in the real world

![Real-world network](images/hungarian-factory.png)

<font size="5">Source: A. Barabasi - Network Science, 2016.</font>

-----
# Network analysis in the real world

Social networks

Online chat communication

Email communication

Supply grids

Biological networks

...

' identify thought leaders, important hubs in the company
' Betweenness in biological pathways 
' Both cancer driver genes and tumour suppressor genes tend to have high betweenness in protein interaction networks 
' dependencies in code 
' code repositories - people who contribute to the same project

----------

- data-background : images/cell-betweenness.jpg

----------

- data-background : #550080

Parsing scripts in a functional way

Polyglot data science

Type providers

Network science

Having fun with interesting data

' Why does it make sense - playing with interesting data

----------

<table>
<tr>
<td class="noborder">

![F# logo](images/fsharp.png)

</td><td class="noborder">

# Learning more 

 - The F# Foundation [www.fsharp.org](http://www.fsharp.org)

 - FsLab package [www.fslab.org](http://www.fslab.org)

 - igraph package [igraph.org/r](http://igraph.org/r/)

</td>
</tr>
</table>

----------

# Star Wars resources

- [Internet Movie Script Database](http://www.imsdb.com/)

- [swapi : Star Wars API](https://swapi.co/)

- [The Star Wars social network](http://evelinag.com/blog/2015/12-15-star-wars-social-network/index.html)

- [Star Wars social networks: The Force Awakens](http://evelinag.com/blog/2016/01-25-social-network-force-awakens/index.html)

- [Star Wars Neo4j demo](http://portal.graphgist.org/graph_gists/855363c7-cdeb-4c8b-b4a5-b72c8f2388e3)

----------

# Thank you!

<table>
<tr>
  <td class="noborder"><img src="images/twitter.png" style="width:70px" /></td>
  <td style="vertical-align:middle" class="noborder"> [@evelgab](https://twitter.com/evelgab)</td>
</tr>
<tr>
  <td class="noborder"><img src="images/github.png" style="width:70px" /></td>
  <td class="noborder" style="vertical-align:middle" > [github.com/evelinag](https://github.com/evelinag)</td>
</tr>
<tr>
  <td class="noborder"></td>
  <td class="noborder" style="vertical-align:middle" > [evelinag.com](http://evelinag.com)</td>
</tr>
</table>

--------

- data-background : images/kyloapproves-loop3.gif