Creation of a static (i.e., single image) visualization that effectively communicates [antibiotics data](https://github.com/Avery2/Visualization-Design/blob/main/antibiotics_data.csv).

* View the final visualization [here](http://www.averychan.site/Visualization-Design/img/Tableau-Antibiotics.pdf).
* View an incomplete but dynamic version [here](https://www.averychan.site/Visualization-Design/dynamic.html).

## Technology used

* **R** for initial data [exploration](https://www.averychan.site/Visualization-Design/Exploration.html) then data manipulation and variable creation.
* **Tableau** for parallel visualization prototyping and then heavy lifing of design.
* Final touches in **Figma**.

## Journey

I started with just building some context for the data. I aimed to visualize "how effective are these antibiotics." After a quick google search, I learned a low minimum inhibitory concentration (MIC) value means the antibiotic was more effective, and that was enough context for the data to get going.

I started exploring the data in R. I got a rough summary of the data and found I needed to do some data manipulation. I needed to pivot the data to facet it by `Antibiotic` and `Bacteria`. Once I got this done and did some fiddling with a faceted dot plot, I had a rough idea that I wanted to encode `Bacteria` by `MIC` value faceted by `Antibiotic`. This new graph would aim to communicate which antibiotic was generally more effective (across the 16 bacteria).

<div align="center">
  <img width="45%" alt="R analysis" src="https://user-images.githubusercontent.com/53503018/134571056-65974510-a182-4885-a305-97c05fc90da9.png"/>
  <img width="45%" alt="a" src="https://user-images.githubusercontent.com/53503018/134572692-e51a218b-d588-47d7-8076-cd8df011495d.png">

  <p>The "faceted dot plot" and first Tableau visual</p>
</div>

After these initial visuals, I noticed that it was still hard to compare the effectiveness of each antibiotic. A large part of this is because I had chosen to facet by `Antibiotic`, it was hard to compare the `MIC` value for the same `Bacteria` across the faceted data. I would have the same issue (but worse) if I had faceted `Bacteria` instead, so I decided to encode this comparison with color. To do this, I went back to R to generate a new variable called `Relative Scores` that encoded an `Antibiotic`'s performance compared to the others for a given `Bacteria`. `Relative Scores` took on a value of `[1, 2, 3]` with `1` meaning it had the lowest MIC and `3` meaning it had the most MIC. Additionally, within each facet, I sorted by `MIC` which to make it easier to compare in aggregate the different `Antibiotic` facets.

After these initial visuals, I noticed that it was still hard to compare the effectiveness of each antibiotic. A large part of this is because I had chosen to facet by `Antibiotic`, it was hard to compare the `MIC` value for the same `Bacteria` across the faceted data. I would have the same issue (but worse) if I had faceted `Bacteria` instead, so I decided to encode this comparison with color. To do this, I went back to R to generate a new variable called `Relative Scores` that encoded an `Antibiotic`'s performance compared to the others for a given `Bacteria`. `Relative Scores` took on a value of `[1, 2, 3]` with `1` meaning it had the lowest MIC and `3` meaning it had the most MIC. Additionally, within each facet, I sorted by `MIC` which to make it easier to compare in aggregate the different `Antibiotic` facets.

<div align="center">
  <img width="45%" alt="d" src="https://user-images.githubusercontent.com/53503018/134572706-cb135732-34c5-49c8-ba77-c5001f5c4f74.png">
  <img width="45%" alt="e" src="https://user-images.githubusercontent.com/53503018/134572707-cc223bd1-f31b-443b-9e13-b0548eaa3cab.png">

  <p>Two charts that used the new <code>Relative Scores</code></p>
</div>

<div align="center">
  <img width="45%" alt="failedbar" src="https://user-images.githubusercontent.com/53503018/134573798-1ed1a0f2-80a8-4cee-b40b-312543c800ec.png">

  <p>Experimenting with adding a bar chart summary for <code>Relative Scores</code></p>
</div>

At this point, there was a lot of information not yet immediately visible, so I made a few changes. I changed the y-axis to use symbols because it was hard to read the vertical bacteria names, especially since sorting by MIC shuffled them. I also changed the color scheme because the `Relative Score` is an ordinal variable, so an ordinal color scheme (shades of blue) makes more sense than one that is more suited for nominal variables (stoplight colors).

I then added a dot plot below the bar chart for `Relative Score` since it was difficult to compare the aggregate `Relative Score` for each `Antibiotic`. This dual encodes `Relative Score` with color and position. Positioning encodes this much better, especially since sorting `MIC` led to a slight clustering effect in the dot plot.

Lastly, I realized the original choice I made to encode `Bacteria` faceted by `Antibiotics` on the x-axis meant that it was still really hard to do \`Bacteria\`-based queries like "What is the Best `Antibiotic` for (`Bacteria`)?" To solve this, I added a table to the `Bacteria` symbol legend to aid with this. It's probably a design sin, but luckily I'm not a designer.

<div align="center">
  <img width="45%" alt="f" src="https://user-images.githubusercontent.com/53503018/134572708-84f7a3ca-82d0-4a47-b0eb-1978adddc3d6.png">
  <img width="45%" alt="addchart" src="https://user-images.githubusercontent.com/53503018/134573801-ae30a384-002a-4960-84dc-24f10ff081a2.png">
  
  <p>Adding symbols to encode bacteria and a secondary lookup table for bacteria-based queries</p>
  
  <img width="85%" alt="addcolorchart" src="https://user-images.githubusercontent.com/53503018/134781365-fcf9bc80-915b-4cd1-9d27-5aedff2c858f.png">
  <p>Final visualization (<a href="http://www.averychan.site/Visualization-Design/img/Tableau-Antibiotics.pdf">updated version here</a>)</p>
</div>

## Analysis

With this visual, I attempted to answer two high-level questions: What is the relative effectiveness of each antibiotic in general? What is the comparative effectiveness of each antibiotic for each bacteria? I had four visualizations that composed the final visual. Three shared an x-axis: A bar chart of the MIC measure, a dot plot of relative MIC variable, and a bar chart of the relative MIC score count. Additionally, there was a table of the best antibiotics for each bacteria. The first three visuals answer the first high-level question by allowing comparisons between the three antibiotics overall. The last visual answers the second high-level question directly. Contextually I was unsure where Gram Staining fell into the story I was trying to tell, so that data was encoded throughout the four visuals. I will go through these four visuals sequentially.

The first bar chart was the visual that encoded the raw quantitative data of the MIC measure. On the y-axis with a log scale are the MIC scores (encoded through length). On the x-axis, the data is bacteria grouped by an antibiotic. I chose to encode the MIC measure in this way because length encodes quantitative data well. The bars have color according to a variable I created that represents the MIC needed for an antibiotic compared to the others with a value of [1, 2, 3] or equivalently [Least, Middle, Most]. Lastly, I added the total and average MIC for each antibiotic and sorted within each antibiotic group by MIC score.

The second visualization was a dot plot encoding the relative MIC variable (through position) on the y-axis and shared the x-axis with the first bar chart. The reason for encoding information already encoded in the color of the first chart is because position is a better channel than color to encode an ordinal variable. Additionally, there was a slight clustering effect that helped visualize which antibiotic was the most effective across all 16 bacteria. Also encoded here in symbols (checkmark or 'x') is the gram-straining. The third visual is a simple bar chart to summarize the count of this same variable.

The last visual is separate from the rest. It is a table of the bacteria names with three columns that correspond to each antibiotic. There is a symbol in each cell where an antibiotic is the best for a particular bacteria. This table also doubles as a key for the bacteria symbols. I chose to make this table because specific queries like "what antibiotic is best for bacteria X?" were hard to make using the first three visualizations. You would have to decode the bacteria symbol three times spread over the x-axis almost randomly and compare the colors of the corresponding bars. This table fixes that. Because it is part of the key, there is no need to decode a symbol, and the information for which bacteria is best is still there. Additionally, the gram staining information is encoded here for similar reasons.
