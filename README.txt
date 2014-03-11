
Section cleaning and manipulation.
First I looked at common faults using the following links:
http://www.tulane.edu/~panda2/Analysis2/datclean/dataclean.htm 
http://www.dwinfocenter.org/errors.html
http://www.nedarc.org/tutorials/analyzingData/cleanTheData/purposeDataCleaning.html
http://logic.stanford.edu/classes/cs246/lectures/lecture13.pdf
http://en.wikipedia.org/wiki/Data_cleansing - 
http://office.microsoft.com/en-gb/excel-help/top-ten-ways-to-clean-your-data-HA010221840.aspx#BMfixing_dates_and_times

This allowed me to be aware of the faults to look for whilst looking at the data.
Error 1:
Column Shift
	A group of columns had been shifted across, by having several commas placed at the beginning of the lines. This was shown up easiest by viewing it in excel and scrolling down quickly. 
	(It was line 180-454 columns a-e)

Fix 1:
	I saw that shifted columns had : ,,,,,006 at the beginning of each of the lines. I used regex to replacereplace ^,,,,,006 with 006, meaning the columns returned to their initial place. I used excel to check that excel encapsulated all of the affected rows.



Error 2:
Seperators embedded inside the data
		Some Agency Project ID's had commas placed in them. Due to this being the seperator for the file type, this meant the columns shifted, and the data was misaligned, and some was missing. This was also found by using excel to scroll down. To find which attribute the fault originated from I compared it to the row above and below, this showed that there was a , for thousands put in place. 
			
Fix 2:
	Due to this only being several rows I found the appropriate rows, and removed the comma, but left the original value.


The  data from here was loaded into google refine.


Error 3:
Summation records
	Summation records were placed inside the data. This is not needed as they can be calculate as needed, and will be otherwise treated as data, but only contains data partially as it is a summation.

Fix 3:
	A text facet was placed on the Agency name, this showed there were multiple occurrences of the summation records.
	I filtered the dataset based on the facet of 'Unique Investment' and removed the rows that remained. 

Error 4:
Blank Rows
	Whilst they do not affect the output directly by injecting invalid rows, blank rows can cause complications as they may create an input if sorting by a key value. 
Fix 4:
	A text facet was placed on Unique Investment Id, the data was filtered by blank rows and removed.
	
Error 5:
Mistakes and Accronyms
	There were mistakes on input, acronym's used, and completely invalid input ons everal columns, mostly the agency name, but also some invesstment groups. This means that the data would be claimiing to have more groups or representing the wrong part. They should all use common names and stadards. The errors were misspelling of agencies, acronyms used, different naming standards (Transport Department vs Department of Transport), acronyms used, whitespace, and some had been called 'the department'.
	
Fix 5:
	Clustering the columnns allows for the data to be grouped together. I used both key collisison and nearest neighbour. However it was seen that the acronyms were  not found at first. This meant i grouped by acronym and then renamed them to conform to the same as the official name.
	For the rows called 'the department' I checked their agency code, the investment title, alongside their position in the record structure and changed their actual name. 
	

Error 6:
Invalid data consistency 
	There was invalid data consistency across the range of columns. The Department of Agriculture was mostly 5, however there was a '6' instead for one of the rows. This means if the agency code was used that it would fail.
	
Fix 6:
	I combined the agency name and code and checked the facet, it showed that some Department of agriculture was named 6 instead of 5, so I changed this based on the later part, by browsing the rows to ensure this was correct, and then changed its agency number.
	

Error 7:
Invalid column names
	I viewed the column headers, and found "Lifecycle cost" had no meaning or units. This means that the data looses its meaning unless it could be rectified, as it could be cost value, time value, as there was no unit of measurement..
Fix 7:
	I retrieved a copy of the original dataset, found out that it was $m, and put this in the title. This was also shown in some of the rows.

Error 8:
Quantity in the field:
	For the Lifecycle Cost some of the data had "($m)" placed in, as this was missing in the header. This meant as it was originally numbers that I changed it 
Fix 8:
	I used regex on the columnn to remove all occurences of $m and the whitespace before/after it. The information was provided in the column title

Error 9:
Repeated rows:
	I applied a text facet to 'Project id' and had the facet sort by count. 4 rows ha a count of two. Duplicate data is misrepresentative, especially if it s a large value or large proportionately.
	
Fix 9:
I filtered by each and found each had the same agency, start data and completion data. Each one of the rows had one deletted so it remained only one per side.
	


Error 10:
Extreme values
	A text facet was placed on the lifecycle cost, and some were very large. This may have been to forgotten '.' or misplaced assuming it was in thousands not millions.
	
Fix 10:
I went through all the values >1000 and compared the lifecycle to the actual cost. Many results were 100x more than the actual cost, and so I dividied the lifecycle cost by 100. There were other factors to see if the cost was feasable, such as the duration of cost shown by the start end date, and finally I checked the remaining large rows seeing if I could find them in original dataset once I sorted, as some had a large value and short span but this was corrct (e.g. DOI).









SECTION Visualisation choice

Visualisation 1: Scatter Plot. 
This infographic was based on  http://bl.ocks.org/weiglemc/6185069 D3 Scatterplot Exampleby Github user weiglemc
This visualisation uses the following data from the dataset because it is neccessary to be able to see and account for why projects change from the original plan. The visualisation clearly portrays any deviation from the planned, with being able to simple review particular departments specific performance. This is appropriate for anybody concerened with department planning, or how the government is performing. As it is a time of austerity this is a large audience, as it is important to see if a particular department is constantly either overbudget or delayed. This enables comparison of department performance and brings into question the accountability for some extreme values and merit the postiive ones. Often question is placed on estimates being vastly over or under an expected value,a nd this shows how well the project planning performas aswell. By having the cost relative to the size, it means you can easily compare the size of the project to see if there is any direct correlation.

Visualisation 2: Sunburst. 

This infographic was based on http://bl.ocks.org/mbostock/4348373 Zoomable Sunburstby mbostock 
This visualisation uses the following data from the dataset because it is important to see how the work is divided throughout the information. The visualisation allows a clear view of how the projects budget is divided, oth by department, but then further in depth by also viewing by investment gropu, and project. This is suited to view how the budget is spent, to analyse where savings can be made. This helps understand wich investment areas are the large costs of the individual projects.




SECTION Interactivity
Both visualisations have common interaction of giving the user more details whilst hovering over, by using a tooltip. 

Visualisation Scatter plot:

The interactivity on visualisation one allows a user to filter, alongside rudimentary zooming. 

The user can filter out the agencies, by clicking the name on the agencies. A filter over a selection was used due to the large quanity of data meaning some elements would remain unseen. This allows quick selection of a particular target to be chosen, and instantly see their details to evaluate them, as opposed to having to see all the others,as there is a large range of values
	
The zoom allows more in depth analysis of particular range, and when combined allows quick evaluation of how a department is performing compared to the norm. As there is a large range, initially the group starts showing the outliers. However there are large amounts of information that is close together. The zoom allows for changing scope of requirements, from on ovderall analysis to view the extremes, to being able to see small variances by going further in.
	
Visualisation Sunburst:
The interactivity allows a user to manipulate the data to explore a particular group by clicking and viewing it in more depth. The key dynamically changes helping show the user the different range. This helps because it doesn't limit the scope of the project because the eye will automatically be able to take in the percentage easier by a further in depth traversal, due to the next layer being the full circle, and some data being very small and hard to differentiate together. This means that the overall can be evaluated, but also each individual project and investment title.




 
SECTION: Implementation

For the website I used bootstrap due to user familiartiy, witha  lower overhead. The web page was based on the 'starter page' example. This helps give the user clear layout of the site, and see what they are viewing and where by the navbar on top.

I chose the D3.js library for the visualisation for several reasons.
	It allows fast visualisation on the web, only requiring to give the location of the javascript source.
	Allows for dynamic options to be given,  giving flexibiility in how I chose to represent the data.
	There are a wide range of tools and principals to help manipulate and change the infographics, helping ease to develop and a smooth user experience. These tools extent to helping manipulate the data, to help prepare it into the correct format required.
	It is similar to my preivious experience with jquery and javascript.
	There are several examples helping understand how it works, working iwth the time constraints of the project.
	There is a live community, with Google groups and stack overflow often having answers from the developers.
	
Each visualisation is robust because they take the original data.

The data was prepared for visualisation locally, allowing for another csv to be used to replace the data. It is assumed it is already cleaned before it is put into place. The sunburst has to prepare the data by using 'D3's .nest' function the flat csv structure into a hierchical structure.

For source control I use a local git repository that was backed up in dropbox. this meant that changes could be tracked, and reverted if neccessary.
	
