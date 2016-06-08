- title : The F#orce awakens
- description : Exploring the Star Wars social network with F#.
- author : Evelina Gabasova
- theme : white
- transition : none

******************************************************


- data-background : images/kylo.gif

<table>
<tr>
  <td class="noborder" style="width:50%;"></td>
   <td class="noborder" style="width:50%;">

## <div class="white">The F#orce Awakens</div>
## <div class="white">Evelina Gabasova</div>
<div class="white">@evelgab </div><br /><br />
<div class="white">*Warning* </div>
<div class="white">Contains *some* spoilers</div>
</td> 
</tr>
</table>

---

- data-background : images/tcga-dna.jpg

' During the day, I do bioinformatics and computational biology,
' researching mechanisms of early carcinogenesis at Cambridge
' University - I deal with DNA, genes, proteins etc...
' It's a very important work, we work on pancreatic cancer which 
' one of the least understood types of cancer. It's very hard to treat
' and mortality hasn't improved much over the last 10 years, so it's a big 
' and important challenge. We use quite a lot of methods, based on 
' statistics, machine learning and network analysis. 
' But biology is complex and sometimes it's a bit too much...

---

- data-background : images/pathways.png

' do you know the feeling? So because I love working with data, outside
' of work I like to play with other datasets, datasets that are
' probably less useful...


</section>
<section data-background-video="https://s3-eu-west-1.amazonaws.com/evelinag/intro.mp4" data-background-color="#000000">
</section>
<section data-background-transition="fade" data-background="images/SWLogo.png" data-background-color="#000000">


---

![](images/scripts.png)

---

- data-background : #f2a063

![script structure](images/script-example.png)

' This structure is highly formalised, names have to be in boldface and centered etc.

-------

- data-background : white

![](images/network-relations.png)

-----

- data-background : #212d30

### Parsing scripts

    [lang=html]
    <pre>
    ...
        <b> INT. MOS EISLEY SPACEPORT - DOCKING BAY 94 </b>

    Chewbacca leads the group into a giant dirt pit that is 
    Docking Bay 94. Resting in the middle of the huge hole is a 
    large, round, beat-up, pieced-together hunk of junk that 
    could only loosely be called a starship.

        <b> LUKE </b>
    What a piece of junk.

    The tall figure of Han Solo comes down the boarding ramp.
    ...
    </pre>

' fsharp data has decent html parser which lets you select all 
' the elements in bold    

-----

- data-background : images/parsing-code-original2.png

<div style="background-color: rgba(0, 0, 0, 0.9); color: #000000; padding: 20px;">
  <h1 style="color: #ffffff">Parsing scripts</h3>
</div>


-----
- data-background : #212d30

### Parsing scripts

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

### Parsing with active patterns

    let (|SceneTitle|Name|Word|) (text:string) =
        let scenePattern = "[ 0-9]*(INT.|EXT.)[ A-Z0-9]"
        let namePattern = "^[/A-Z0-9]+[-]*[/A-Z0-9 ]*[-]*[/A-Z0-9 ]+$"
        if Regex.Match(text, scenePattern).Success then
            SceneTitle text
        elif Regex.Match(text, namePattern).Success then
            Name text
        else Word

-----
- data-background : #550080

# (| Active patterns |)

hide complexity behind readable code

-----

- data-background : images/itsatrap3.gif

--------

![Tion Medon 1](images/tion-medon.png)

--------

<img src="images/tion-medon-closeup.png" style="width: 900px" />

--------

- data-background : images/tion-medon-photo.png

-------

- data-background : images/r2d2beeps-loop3.gif

--------

![R2-D2](images/artoo.png)

--------

![Chewbacca](images/chewie.png)

--------

- data-background : #550080

## Number of *common* mentions

# ⬇

## Number of interactions

--------

![](  images/equations1.png)

--------

![](images/equations2.png)

--------

- data-background : images/ewoks.gif

--------

- data-background : images/lonely-luke.gif

--------

- data-background : #000000

### Theorem: 

# There's an API for everything.

### Proof: 

### [swapi.co](https://swapi.co/)

----------

- data-background : #000000

![swapi](images/swapi.png)

----------

- data-background : #000000

![swapi-langs](images/swapi-langs.png)

----------

- data-background : images/darthvader-darkside-loop3.gif

----------

- data-background : images/csharp-swapi.gif

----------

- data-background : #000000

![swapi-langs](images/swapi-now.png)

----------

- data-background : #000000

### <div class="sw">fsharp-swapi</div>

[github.com/evelinag/fsharp-swapi](https://github.com/evelinag/fsharp-swapi)

[swapi.co](https://swapi.co/documentation)

---

- data-background : #000000

### <div class="sw">scenes</div>
# + 
### <div class="sw">characters</div>

----------
![](images/film_analysis.png)

---

<img src="images/gender-analysis.png" style="width: 1200px" />

---

<script type="text/javascript" src="https://www.google.com/jsapi"></script>
<script type="text/javascript">
    google.load("visualization", "1", {packages:["corechart"]})
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 79.4117647058823}]}, {"c" : [{"v": "Episode 2"}, {"v": 78.968253968254}]}, {"c" : [{"v": "Episode 3"}, {"v": 81.7901234567901}]}, {"c" : [{"v": "Episode 4"}, {"v": 83.7310195227766}]}, {"c" : [{"v": "Episode 5"}, {"v": 79.9472295514512}]}, {"c" : [{"v": "Episode 6"}, {"v": 78.6516853932584}]}, {"c" : [{"v": "Episode 7"}, {"v": 68.4210526315789}]}]});
    var options = {"hAxis":{"title":"Percentage of male characters (%)","viewWindowMode":"explicit","viewWindow":{"max":100,"min":0}},"legend":{"position":"none"},"title":"Ratio of male characters","width":1000,"height":550} 
    var chart = new google.visualization.BarChart(document.getElementById('82c61e2a-02a3-497e-92e2-0d339641687c'));
    chart.draw(data, options);
}
</script>
<div id="82c61e2a-02a3-497e-92e2-0d339641687c" style="width: 800px; height: 600px;"></div>

---

- data-background : images/rey_bb8-loop2.gif

----------

# <div class="sw">visualization</div>

----------

![](images/d3js.png)

' Exporting and importing the network with Json?
' Because F# works very well with JSON, maybe I can just use something that will read the JSON 

----------

![](images/fable.png)

----------

![](images/fable-logo.png)
![](images/fable-samples1.png)

----------

![](images/fable-logo.png)
![](images/fable-samples2.png)

----------

- data-background : images/networks/full_network-darth-vader.png

<a href="images/networks/interactions-merged.html" style="color: transparent;"> Big link to full network <br /> Big link to full network <br />Big link to full network </a>

----------

### <div class="sw">the phantom menace</div>

<a href="images/networks/episode1-interactions.html"><img src="images/networks/episode1.png" style="height:600px"/> </a>


----------

### <div class="sw">a new hope</div>

<a href="images/networks/episode4-interactions.html"><img src="images/networks/episode4.png" style="height:600px"/> </a>


----------

### <div class="sw">the force awakens</div>

<a href="images/networks/episode7-interactions.html"><img src="images/networks/episode7_network.png" style="height:600px"/> </a>

-----

# Network structure

How do the the movies differ?

-----

### <div class="sw"> Size </div>

<script type="text/javascript">
    google.load("visualization", "1", {packages:["corechart"]})
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 38}]}, {"c" : [{"v": "Episode 2"}, {"v": 33}]}, {"c" : [{"v": "Episode 3"}, {"v": 25}]}, {"c" : [{"v": "Episode 4"}, {"v": 22}]}, {"c" : [{"v": "Episode 5"}, {"v": 21}]}, {"c" : [{"v": "Episode 6"}, {"v": 20}]}, {"c" : [{"v": "Episode 7"}, {"v": 27}]}]});
    var options = {"colors":["#3bc4c4"],"hAxis":{"title":"Number of characters","viewWindowMode":"explicit","viewWindow":{"max":40,"min":0}},"legend":{"position":"none"},"title":"Number of characters","width":1000,"height":550} 
    var chart = new google.visualization.BarChart(document.getElementById('793fedd1-18b7-4674-bbd9-333211512707'));
    chart.draw(data, options);
}
</script>
<div id="793fedd1-18b7-4674-bbd9-333211512707" style="width: 800px; height: 600px;"></div>

-----

### Density
![Network](images/network-basic.png)

-----
### Density
![Network](images/full.png)

-----
### Density

<br />

$$$
\begin{align}
\text{Density} &= \frac{\text{Existing connections}}{\text{Potential connections}} \\
& \\
&= \frac{\text{Existing connections}}{\frac{1}{2}N(N-1)}
\end{align}

-----

### <div class="sw"> Density </div>

<script type="text/javascript">
    google.load("visualization", "1", {packages:["corechart"]})
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 19.203413940256}]}, {"c" : [{"v": "Episode 2"}, {"v": 19.1287878787879}]}, {"c" : [{"v": "Episode 3"}, {"v": 25.296442687747}]}, {"c" : [{"v": "Episode 4"}, {"v": 28.5714285714286}]}, {"c" : [{"v": "Episode 5"}, {"v": 26.1904761904762}]}, {"c" : [{"v": "Episode 6"}, {"v": 31.5789473684211}]}, {"c" : [{"v": "Episode 7"}, {"v": 26.2108262108262}]}]});
    var options = {"colors":["#c43b80"],"hAxis":{"title":"Density (%)","viewWindowMode":"explicit","viewWindow":{"max":35,"min":15}},"legend":{"position":"none"},"title":"Network density","width":1000,"height":550}
    var chart = new google.visualization.BarChart(document.getElementById('526513e7-b223-41f2-8ba0-0008433196f8'));
    chart.draw(data, options);
}
</script>
<div id="526513e7-b223-41f2-8ba0-0008433196f8" style="width: 800px; height: 600px;"></div>

-----
### Clustering coefficient
![Network](images/network-basic.png)

-----
### Clustering coefficient
![Clustering](images/clustering1.png)

-----
### Clustering coefficient
![Clustering](images/clustering2.png)

-----
### Clustering coefficient
![Clustering](images/clustering3.png)

-----
### Clustering coefficient
![Clustering](images/clustering4.png)

-----
### Clustering coefficient
![Clustering](images/clustering5.png)

-----
### Clustering coefficient

<br />

$$$
K_v = \text{Number of neighbours of $v$} \\
E_v = \text{Number of links between neighbours of $v$} \\ \\
\text{Clustering}(v) = \frac{E_v}{\frac{1}{2} K_v (K_v - 1)}

-----
### Clustering coefficient

<br />

$$$
K_v = \text{Number of neighbours of $v$} \\
E_v = \text{Number of links between neighbours of $v$} \\ \\
\text{Clustering}(\text{network}) = \frac{1}{N} \sum_v \frac{E_v}{\frac{1}{2}  K_v (K_v - 1)}

--------

### <div class="sw"> Clustering Coefficient </div>

<script type="text/javascript">
    google.load("visualization", "1", {packages:["corechart"]})
    google.setOnLoadCallback(drawChart);
function drawChart() {
    var data = new google.visualization.DataTable({"cols": [{"type": "string" ,"id": "Column 1" ,"label": "Column 1" }, {"type": "number" ,"id": "Column 2" ,"label": "Column 2" }], "rows" : [{"c" : [{"v": "Episode 1"}, {"v": 0.447572132301196}]}, {"c" : [{"v": "Episode 2"}, {"v": 0.486666666666667}]}, {"c" : [{"v": "Episode 3"}, {"v": 0.498947368421053}]}, {"c" : [{"v": "Episode 4"}, {"v": 0.559808612440191}]}, {"c" : [{"v": "Episode 5"}, {"v": 0.604651162790698}]}, {"c" : [{"v": "Episode 6"}, {"v": 0.656992084432718}]}, {"c" : [{"v": "Episode 7"}, {"v": 0.588387096774194}]}]});
    var options = {"hAxis":{"title":"Clustering coefficient"},"legend":{"position":"none"},"title":"Clustering coefficient (transitivity)","width":1000,"height":550} 
    var chart = new google.visualization.BarChart(document.getElementById('c7a96a91-f091-4462-b2fd-2ce7d06df07b'));
    chart.draw(data, options);
}
</script>
<div id="c7a96a91-f091-4462-b2fd-2ce7d06df07b" style="width: 800px; height: 600px;"></div>

' How many of your friends in the network are also friends with each other
' What it means in terms of the story


-----
### Degree
![Network](images/network-basic.png)

-----
### Degree
![Degree](images/degree1.png)

-----
### Degree
![Degree](images/degree2.png)

------
### Degree

<br />

$$$
\text{Degree}(v) = \text{Number of links }v \leftrightarrow v' \\
v \neq v'


-----
### Betweenness
![Betweenness](images/network-basic.png)

-----
### Betweenness
![Betweenness](images/betweenness1.png)

-----
### Betweenness
![Betweenness](images/betweenness2.png)

-----
### Betweenness
![Betweenness](images/betweenness3.png)

-----
### Betweenness
![Betweenness](images/betweenness4.png)

-----
### Betweenness

<br />

$$$
S_v = \text{Number of shortest paths between $a$ and $b$ through $v$} \\
S = \text{Number of shortest paths between $a$ and $b$} \\ \\
\text{Betweenness}(v)_{ab} = \frac{S_v}{S}

-----
### Betweenness

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

    b <- betweenness(network)


-----
- data-background : #212d30

# F#

    open RProvider.igraph

    let b = R.betweenness(network)

---

### <div class="sw"> Centrality </div>

<small>
<table>
<tr>
  <td class="noborder" style="padding-right: 100px">

| | Name | Degree |
|---|-----|-----|
| 1. | POE | 16 |
| 2. | FINN | 14 |
| 3. | HAN | 14 |
| 4. | CHEWBACCA | 12 |
| 5. | BB-8 | 12 |

</td>
  <td class="noborder">
  <div class="fragment">

| | Name | Betweenness |
|---|-----|-----|
1. | POE | 97.2 |
2. | KYLO REN | 71.9 |
3. | REY | 38.7 |
4. | BB-8 | 29.1 |
5. | FINN | 26.8 |

</div>
</td>
</tr>
</table>
</small>

---

- data-background : images/poe_dancing0.gif

---

- data-background : images/obi-wan-noloop.gif

---

- data-background : images/darthvader-wins-loop2.gif

----------

### Star Wars network in Neo4j

![](images/neo4j.png)

----------


### Network analysis in the real world

<table>
<tr>
<td class="noborder">

![F# network](images/fsharp-network.png)

</td><td class="noborder" style="vertical-align: top">

<h2 style="color: #e67e00">@fsharporg</h2>

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
### Network analysis in the real world

![Real-world network](images/hungarian-factory.png)

<font size="5">Source: A. Barabasi - Network Science, 2016.</font>

-----
## Network analysis in the real world

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

### Learning more 

 - The F# Foundation [www.fsharp.org](http://www.fsharp.org)

 - FsLab package [www.fslab.org](http://www.fslab.org)

 - igraph package [igraph.org/r](http://igraph.org/r/)

</td>
</tr>
</table>

----------

### Star Wars resources

- [Internet Movie Script Database](http://www.imsdb.com/)

- [swapi : Star Wars API](https://swapi.co/)

- [The Star Wars social network](http://evelinag.com/blog/2015/12-15-star-wars-social-network/index.html)

- [Star Wars social networks: The Force Awakens](http://evelinag.com/blog/2016/01-25-social-network-force-awakens/index.html)

- [Star Wars Neo4j demo](http://portal.graphgist.org/graph_gists/855363c7-cdeb-4c8b-b4a5-b72c8f2388e3)

----------

- data-background : images/kyloapproves-loop3.gif

-----

- data-background : images/kylo.gif

<table>
<tr>
  <td class="noborder" style="width:40%;"></td>
   <td class="noborder" style="width:60%;">

## <div class="white">Evelina Gabasova</div>
<div class="white">@evelgab </div><br />
<div class="white">evelinag.com/star-wars-talk</div><br />
<div class="white">github.com/evelinag</div>
</td> 
</tr>
</table>
