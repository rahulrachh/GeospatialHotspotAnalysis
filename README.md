# Hotspot Analysis on Restaurants of Toronto, Canada

## Requirement

In this project, you are required to do spatial hotspot analysis. In particular, you need to complete two different hotspot analysis tasks 

### Hot zone analysis
This task will needs to perform a range join operation on a rectangle datasets and a point dataset. For each rectangle, the number of points located within the rectangle will be obtained. The hotter rectangle means that it include more points. So this task is to calculate the hotness of all the rectangles. 

### Hot cell analysis

#### Description
This task will focus on applying spatial statistics to spatio-temporal big data in order to identify statistically significant spatial hot spots using Apache Spark. The topic of this task is from ACM SIGSPATIAL GISCUP 2016.

The Problem Definition page is here: [http://sigspatial2016.sigspatial.org/giscup2016/problem](http://sigspatial2016.sigspatial.org/giscup2016/problem) 

The Submit Format page is here: [http://sigspatial2016.sigspatial.org/giscup2016/submit](http://sigspatial2016.sigspatial.org/giscup2016/submit)

#### Special requirement (different from GIS CUP)
As stated in the Problem Definition page, in this task, you are asked to implement a Spark program to calculate the Getis-Ord statistic of Toronto, Canada restaurant  datasets. We call it "**Hot cell analysis**"

To reduce the computation power need，we made the following changes:

1. The input will be a monthly restaurant  dataset from 2009 - 2012. For example, "foursquare\_data\_2009-01\_point.csv", "foursquare\_data\_2010-02\_point.csv".
2. Each cell unit size is 0.01 * 0.01 in terms of latitude and longitude degrees.
3. You should use 1 day as the Time Step size. The first day of a month is step 1. Every month has 31 days.
4. You only need to consider Pick-up Location.
5. We don't use Jaccard similarity to check your answer.
However, you don't need to worry about how to decide the cell coordinates because the code template generated cell coordinates. You just need to write the rest of the task.

## User Defined Function

### ST_Contains

Input: queryRectangle:String, pointString:String

Output: Boolean (true or false)

Definition: You first need to parse the queryRectangle (e.g., "-155.940114,19.081331,-155.618917,19.5307") and pointString (e.g., "-88.331492,32.324142") to a format that you are comfortable with. Then check whether the queryRectangle fully contains the point. Consider on-boundary point. If the point is within the rectangle then return true otherwise return false.

## Coding template specification

### Input parameters

1. Output path (Mandatory)
2. Task name: "hotzoneanalysis" or "hotcellanalysis"
3. Task parameters: (1) Hot zone (2 parameters): Toronto, Canada restaurant data path, zone path(2) Hot cell (1 parameter): Toronto, Canada restaurant data path

Example
```
test/output hotzoneanalysis src/resources/point-hotzone.csv src/resources/zone-hotzone.csv hotcellanalysis src/resources/foursquare__sample_100000.csv
```

Note: 

1. The number/order of tasks do not matter.
2. But, the first 7 of our final test cases will be hot zone analysis, the last 8 will be hot cell analysis.




### Input data format
The main function/entrace is "Entrance.scala" scala file.

1. Point data: the input point dataset is the pickup point of New York restaurant  datasets. The data format of this phase is the original format of Toronto, Canada restaurant  which is different from the previous phase. But the coding template already parsed it for you. Find the data from our asu google drive shared folder: [https://drive.google.com/drive/folders/1W4GLKNsGlgXp7fHtDlhHEBdLVw_IuAXh?usp=sharing](https://drive.google.com/drive/folders/1W4GLKNsGlgXp7fHtDlhHEBdLVw_IuAXh?usp=sharing). To get the permission to download the data, you have to use your asu gmail account.

2. Zone data (only for hot zone analysis): at "src/resources/zone-hotzone" of the template

#### Hot zone analysis
The input point data can be any small subset of Toronto, Canada restaurant dataset.

#### Hot cell analysis
The input point data is a monthly Toronto, Canada restaurant  dataset (2009-2012) like "foursquare\_data\_2009-01\_point.csv"

### Output data format

#### Hot zone analysis
All zones with their count, sorted by "rectangle" string in an ascending order. 

```
"-73.795658,40.743334,-73.753772,40.779114",1
"-73.797297,40.738291,-73.775740,40.770411",1
"-73.832707,40.620010,-73.746541,40.665414",20
```


#### Hot cell analysis
The coordinates of top 50 hotest cells sorted by their G score in a descending order. Note, DO NOT OUTPUT G score.

```
-7399,4075
-7399,4075
-7399,4075
```
### Example answers
An example input and answer are put in "testcase" folder of the coding template. 

1. hotcell-example-answer-withZscore-sample.csv -- answer of the sampled input data with score.
2. hotcell-example-answer-withZscore.csv -- answer with score of the full input data from Google Drive shared folder.
3. hotcell-example-answer.csv -- the expected output result of using the foursquare_data_2009-01_point.csv as the input.
4. hotcell-example-input.txt -- the input for running the jar file.

## Where you need to change
DO NOT DELETE any existing code in the coding template unless you see this "YOU NEED TO CHANGE THIS PART"

### Hot zone analysis

In the code template,

1. You need to change "**HotzoneAnalysis.scala** and **HotzoneUtils.scala**".
2. Replace the logic of User Defined Function ST\_Contains in HotzoneUtils.scala. You can add additional methods in this class if you need.
3. The coding template has loaded the data and wrote the first step, range join query, for you. Please finish the rest of the task.
4. The output DataFrame should be sorted by you according to "rectangle" string.

### Hot cell analysis
In the code template,

1. You need to change "**HotcellAnalysis.scala** and **HotcellUtils.scala**".
2. The coding template has loaded the data and decided the cell coordinate, x, y, z and their min and max. Please finish the rest of the task.
3. The output DataFrame should be sorted by you according to G-score. The coding template will take the first 50 to output. DO NOT OUTPUT G-score.


## Tips (Optional)
This section is same with that in Phase 1.
### How to debug your code in IDE

If you are using the Scala template

1. Use IntelliJ Idea with Scala plug-in or any other Scala IDE.
2. Append ```.master("local[*]")``` after ```..config("spark.serializer",classOf[KryoSerializer].getName)``` to tell IDE the master IP is localhost.
3. In some cases, you may need to go to "build.sbt" file and change ```% "provided"``` to ```% "compile"``` in order to debug your code in IDE
4. Run your code in IDE
5. **You must revert Step 2 and 3 above and recompile your code before use spark-submit!!!**

### How to submit your code to Spark
If you are using the Scala template

1. Go to project root folder
2. Run ```sbt clean assembly```. You may need to install sbt in order to run this command.
3. Find the packaged jar in "./target/scala-2.11/Apache--Hotspot-Analysis-assembly-0.1.0.jar"
4. Now, you can run the jar on Spark with spark-submit command. If you already have set up the Hadoop and Spark test environment, you should be able to run the spark-submit command from your command line. Submit the jar to Spark using Spark command "./bin/spark-submit". A pseudo code example: ```./bin/spark-submit ~/GitHub/Hotspot-Analysis-Apache--Template/target/scala-2.11/Apache--Hotspot-Analysis-assembly-0.1.0.jar test/output hotzoneanalysis src/resources/point-hotzone.csv src/resources/zone-hotzone.csv hotcellanalysis src/resources/foursquare_data_2009-01_point.csv```
