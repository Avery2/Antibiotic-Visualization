# Visualization Design

Creation of a static (i.e., single image) visualization that effectively communicates antibiotics data. [View it here](http://www.averychan.site/Visualization-Design/img/Tableau-Antibiotics.pdf).

## Technology used

* **R** for initial data exploration then data manipulation and variable creation
* **Tableau** for parallel visualization prototyping and then heavy lifing of design
* Final touches in **Figma**

## Journey

I started with just building some context for the data. I aimed to visualize "how effective are these antibiotics." After a quick google search I learned a low minimum inhibitory concentration (MIC) value means the antibiotic was more effective, and that was enough context for the data to get going.

I started explore the data in R. I was able to get a few summarise of the data. But a lot of the heavy lifting happened here, I needed to pivot the data so that I could facet it by `Antibiotic` and `Bacteria`. Once I got this done and did some fiddling with a faceted dot plot, I had a rough idea that I wanted to encode the `Bacteria` by `MIC` value faceted by `Antibiotic`. This new graph would aim to communciate which antibiotic was generally (across the 16 bacteria) more effective.

<div align="center">
  <img width="45%" alt="R analysis" src="https://user-images.githubusercontent.com/53503018/134571056-65974510-a182-4885-a305-97c05fc90da9.png"/>
  <img width="45%" alt="a" src="https://user-images.githubusercontent.com/53503018/134572692-e51a218b-d588-47d7-8076-cd8df011495d.png">
  
  <p>The "faceted dot plot" and first Tableau visual</p>
</div>

<!-- <img width="45%" alt="b" src="https://user-images.githubusercontent.com/53503018/134572698-17c9e8ca-d04d-4de1-ac03-ff8de3eb03d3.png"> -->
<!-- <img width="45%" alt="c" src="https://user-images.githubusercontent.com/53503018/134572702-6d5cfd41-e8d9-4e6c-a5fb-1663d8ff5eb8.png"> -->

After these initial visuals, I noticed that it was still very hard to compare the effectiveness of each antibiotic. A large part of this is because I had chosen to facet by `Antibiotic`, it was hard to compare the `MIC` value for the same `Bacteria` across the faceted data. I would have the same issue (but worse) if I had faceted by `Bacteria` instead, so I devided to encode this comparison with color. To do this, I went back to R and generated a new variable called `Relative Scores` that encoded, per `Antibiotic`-`Bacteria` pair, what the relative effectiveness was compared to the other `Antibiotic`-`Bacteria` pairs (where `Bacteria` is the same). `Relative Scores` took on a value of `[1, 2, 3]` with `1` meaning it had the lowest `MIC` and `3` meaning it had the most `MIC`. I also sorted within each facet by `MIC` which made it easier to compare in aggreate the different `Antibiotic` facets.

<div align="center">
  <img width="45%" alt="d" src="https://user-images.githubusercontent.com/53503018/134572706-cb135732-34c5-49c8-ba77-c5001f5c4f74.png">
  <img width="45%" alt="e" src="https://user-images.githubusercontent.com/53503018/134572707-cc223bd1-f31b-443b-9e13-b0548eaa3cab.png">
  
  <p>Two charts that used the new <code>Relative Scores</code></p>
</div>

<div align="center">
  <img width="45%" alt="failedbar" src="https://user-images.githubusercontent.com/53503018/134573798-1ed1a0f2-80a8-4cee-b40b-312543c800ec.png">
  
  <p>Experimenting with adding a bar chart summary for <code>Relative Scores</code></p>
</div>

At this point there was a lot of information not yet immidiately visible, so I did a few changes. I changed the y-axis to use symbols because it was very hard to read the vertical bacteria names, especially since they were shuffled. I also changed the color scheme because the `Relative Score` is actually an ordinal variable, so it makes sense to have a color scheme that is ordinal (shades of blue) rather than one that is more suited for nominal variables (stop light colors).

I then added a dot plot below the bar chart for `Relative Score`. This was because although `Relative Score` was already encoded in the color of the bar chart, it was difficult to compare the aggregate `Relative Score` for each `Antibiotic`. Positioning encodes this much better, especially since they are sorted by `MIC` and the slight correlation between `MIC` and `Relative Score` led to a clustering effect in the dot plot.

Lastly, I realized the original choice I made in the beginning to encode `Bacteria` faceted by `Antibiotics` on the x-axis meant that it was still really hard to do `Bacteria`-based queries like "What is the best `Antibiotic` for `Bacteria`?" To solve this, I added a table in the bottom right. I added a table to the `Bacteria` symbol legend to aid with this. It's probably a major design sin but luckily I'm not a designer.

<div align="center">
  <img width="45%" alt="f" src="https://user-images.githubusercontent.com/53503018/134572708-84f7a3ca-82d0-4a47-b0eb-1978adddc3d6.png">
  <img width="45%" alt="addchart" src="https://user-images.githubusercontent.com/53503018/134573801-ae30a384-002a-4960-84dc-24f10ff081a2.png">
  
  <p>Adding symbols to encode bacteria and a secondary lookup table for bacteria-based queries</p>
</div>

[View the final visualization here!](http://www.averychan.site/Visualization-Design/img/Tableau-Antibiotics.pdf)

## Analysis

