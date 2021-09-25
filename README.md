# Visualization Design

Creation of a static (i.e., single image) visualization that effectively communicates antibiotics data. [View it here](http://www.averychan.site/Visualization-Design/img/Tableau-Antibiotics.pdf).

## Technology used

*   **R** for initial data exploration then data manipulation and variable creation
*   **Tableau** for parallel visualization prototyping and then heavy lifing of design
*   Final touches in **Figma**

## Journey

I started with just building some context for the data. I aimed to visualize "how effective are these antibiotics." After a quick google search, I learned a low minimum inhibitory concentration (MIC) value means the antibiotic was more effective, and that was enough context for the data to get going.

I started exploring the data in R. I got a rough summary of the data and found I needed to do some data manipulation. I needed to pivot the data to facet it by `Antibiotic` and `Bacteria`. Once I got this done and did some fiddling with a faceted dot plot, I had a rough idea that I wanted to encode `Bacteria` by `MIC` value faceted by `Antibiotic`. This new graph would aim to communicate which antibiotic was generally more effective (across the 16 bacteria).

![R analysis](https://user-images.githubusercontent.com/53503018/134571056-65974510-a182-4885-a305-97c05fc90da9.png)

![a](https://user-images.githubusercontent.com/53503018/134572692-e51a218b-d588-47d7-8076-cd8df011495d.png)

The "faceted dot plot" and first Tableau visual

After these initial visuals, I noticed that it was still hard to compare the effectiveness of each antibiotic. A large part of this is because I had chosen to facet by `Antibiotic`, it was hard to compare the `MIC` value for the same `Bacteria` across the faceted data. I would have the same issue (but worse) if I had faceted `Bacteria` instead, so I decided to encode this comparison with color. To do this, I went back to R to generate a new variable called `Relative Scores` that encoded an `Antibiotic`'s performance compared to the others for a given `Bacteria`. `Relative Scores` took on a value of `[1, 2, 3]` with `1` meaning it had the lowest MIC and `3` meaning it had the most MIC. Additionally, within each facet, I sorted by `MIC` which to make it easier to compare in aggregate the different `Antibiotic` facets.

![d](https://user-images.githubusercontent.com/53503018/134572706-cb135732-34c5-49c8-ba77-c5001f5c4f74.png)

![e](https://user-images.githubusercontent.com/53503018/134572707-cc223bd1-f31b-443b-9e13-b0548eaa3cab.png)

Two charts that used the new `Relative Scores`

![failedbar](https://user-images.githubusercontent.com/53503018/134573798-1ed1a0f2-80a8-4cee-b40b-312543c800ec.png)

Experimenting with adding a bar chart summary for `Relative Scores`

At this point, there was a lot of information not yet immediately visible, so I made a few changes. I changed the y-axis to use symbols because it was hard to read the vertical bacteria names, especially since sorting by MIC shuffled them. I also changed the color scheme because the `Relative Score` is an ordinal variable, so an ordinal color scheme (shades of blue) makes more sense than one that is more suited for nominal variables (stoplight colors).

I then added a dot plot below the bar chart for `Relative Score` since it was difficult to compare the aggregate `Relative Score` for each `Antibiotic`. This dual encodes `Relative Score` with color and position. Positioning encodes this much better, especially since sorting `MIC` led to a clustering effect in the dot plot.

Lastly, I realized the original choice I made to encode `Bacteria` faceted by `Antibiotics` on the x-axis meant that it was still really hard to do \`Bacteria\`-based queries like "What is the Best `Antibiotic` for (`Bacteria`)?" To solve this, I added a table to the `Bacteria` symbol legend to aid with this. It's probably a design sin, but luckily I'm not a designer.

![f](https://user-images.githubusercontent.com/53503018/134572708-84f7a3ca-82d0-4a47-b0eb-1978adddc3d6.png)

![addchart](https://user-images.githubusercontent.com/53503018/134573801-ae30a384-002a-4960-84dc-24f10ff081a2.png)

Adding symbols to encode bacteria and a secondary lookup table for bacteria-based queries

[View the final visualization here!](http://www.averychan.site/Visualization-Design/img/Tableau-Antibiotics.pdf)

## Analysis
