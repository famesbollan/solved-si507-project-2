Download Link: https://assignmentchef.com/product/solved-si507-project-2
<br>
<h1>Project Overview</h1>

You will create a program to scrape and search information about National Sites (Parks, Heritage Sites, Trails, and other entities) from nps.gov. You will also add the ability to look up nearby places using the Google Places API and to display National Sites and Nearby Places on a map using plotly.




Starter code:

<a href="https://www.dropbox.com/s/64fldvjy2pl8a7g/proj2_nps.py?dl=0">proj2_nps.py</a>

<a href="https://www.dropbox.com/s/kfr98sy7vwygxw7/secrets.py?dl=0">secrets.py</a>

Test file:

<a href="https://www.dropbox.com/s/ae7hsrol1sen73x/proj2_nps_test.py?dl=0">proj2_nps_test.py</a>

* Part of your project will be graded using this test file.




You only need to submit proj2_nps.py file.




Also please observe the following:

<ul>

 <li>Do not change the name of the file proj2_nps.py</li>

 <li>Do not change any of the contents of the file proj2_nps_test.py

  <ul>

   <li>You can create other files, including other test files, if you would like, but you may not change this file or rename the main program file.</li>

  </ul></li>

</ul>




Failure to follow these guidelines may result in point deductions.

<h1>Part 1 (80 points)</h1>

In part 1 you will scrape nps.gov with the goal of being able to print out information about any National Site listed on the site, organized by state. Information will include the site name, site type, and the physical (or mailing) address. Your program will start crawling at <a href="https://www.nps.gov/index.htm">https://www.nps.gov/index.htm</a>, and from there crawl pages for particular states and then pages for particular sites. The links to state pages can be accessed from the dropdown box labeled “FIND A PARK.”










To pass the included tests, you will need to edit the function in the starter code get_sites_for_state(state_abbr) that takes a state abbreviation and returns a list of NationalSites that are in that state. The required attributes for the NationalSite class can be seen in the skeleton code file (proj2_nps.py).




At the basic level, each NationalSite (instance) should be created with a name, type (e.g., ‘National Park,’ ‘National Monument’, ‘National Historic Site’), and description. All of these can be found on the landing page for a particular state (e.g., <a href="https://www.nps.gov/state/mi/index.htm">https://www.nps.gov/state/mi/index.htm</a>).




In addition, you should visit the detail page for each site to extract additional information–in particular the physical address of the site. To do this, you will have to crawl one level deeper into the site, and extract information from the site-specific pages (e.g., <a href="https://www.nps.gov/isro/index.htm">https://www.nps.gov/isro/index.htm</a>).




Printing a NationalSite object should return a string representation of itself (using __str__( )) of the following form: &lt;name&gt; (&lt;type&gt;): &lt;address string&gt;




For example:

Isle Royale (National Park): 800 East Lakeshore Drive, Houghton, MI 49931




Finally, though you should really consider doing this first to dramatically speed up your development time, implement caching so that you only have to visit each URL within nps.gov once (and subsequent attempts to visit, say  <a href="https://www.nps.gov/state/mi/index.htm">https://www.nps.gov/state/mi/index.htm</a> or <a href="https://www.nps.gov/isro/index.htm">https://www.nps.gov/isro/index.htm</a> are satisfied using the cache rather than another HTTP request).




Grading

<ul>

 <li>[30 points] Implement basic searching by state and creation of NationalSites with name and type. Pass test_basic_search( )</li>

 <li>[30 points] Implement adding address information to NationalSites by crawling. Pass test_addresses( )</li>

 <li>[10 points] Implement __str__( ) as specified. Pass test_str( )</li>

 <li>[10 points] Add caching so that you never have to visit a page on the NPS site more than once.</li>

</ul>

<h1>Part 2 (40 points)</h1>




Implement a function get_nearby_places(site_object) that looks up a site by name using the Google Places API and returns a list of up to 20 nearby places, where “nearby” is defined as within 10km (note: 20 results is the default maximum number returned by the Google Places API without paging).




Getting the list of nearby places will require two calls to the google API: one to get the GPS coordinates for a site (tip: do a text search for &lt;site.name&gt; &lt;site.type&gt; to ensure a more precise match–it turns out there are lots of places called “Death Valley” that aren’t National Parks!), and another one to get the nearby places. Documentation on the Google Places API can be found here: <a href="https://developers.google.com/places/web-service/search">https://developers.google.com/places/web-service/search</a>.




You will need to get a Google API key following instructions <a href="https://www.dropbox.com/s/uhbhueweyc3eq06/Google%20Place%20API%20Instruction.docx?dl=0">here</a>. Implementing caching for this portion of the project is STRONGLY recommended.




get_nearby_places(site_object)should return a list of NearbyPlace objects.




At a minimum, a NearbyPlace needs to have the name of the place as an attribute. You may find it useful to add other attributes as well. A NearbyPlace should include a __str__( ) method, which simply prints the Place name.




Note:

<ul>

 <li>If you do a search on a NationalSite using &lt;site name&gt; &lt;site type&gt; (e.g., “Death Valley National Park” or “Motor Cities National Heritage Area”) and Google Places does not return any results (or returns results, but none of them have the specific name you searched for), your list of “Nearby Places” should be an empty list.</li>

</ul>




Grading:

<ul>

 <li>[30 points] Return a list of up to 20 NearbyPlaces from get_nearby_places(site_object). Each place has a properly configured name Pass test_nearby_search( ).</li>

 <li>[10 points] adding caching so that you only do a particular nearby search once (items in your cache don’t need to expire, even though technically the data could change given enough time.)</li>

</ul>

<h1>Part 3 (40 Points)</h1>

Use plotly to display maps of NationalSites within a state and NearbyPlaces near at NationalSite.




Implement two functions:




plot_sites_for_state(state_abbr) and plot_nearby_for_site(site_object)




Here are some details about each function:

<ul>

 <li>plot_sites_for_state(state_abbr):

  <ul>

   <li>Takes a state abbreviation</li>

   <li>Creates a plotly map scatter plot (or mbox scatter plot) that contains all of the NationalSites found for that state <em>that Google Places was able to find GPS coordinates for</em>.

    <ul>

     <li>Any Sites that don’t have GPS coordinates should be removed before creating the map</li>

    </ul></li>

   <li>The map should be centered and scaled appropriately so that all of the sites are visible and that there is a reasonable amount of “padding” around the edges of the map (i.e., so that all of the sites are comfortably within the map frame and not all the way at the edge)</li>

   <li>All Sites should be displayed with the same type of marker, and each should display the name of the site when a user hovers over the marker (this is the default behavior in plotly if each data point has a ‘text’ field correctly set).</li>

  </ul></li>

 <li>plot_nearby_for_site(site_object)

  <ul>

   <li>Takes a NationalSite object</li>

   <li>Creates a plotly map scatter plot (or mbox scatter plot) that contains all of the NearbyPlaces for the specified site.

    <ul>

     <li>If a NationalSite is provided that Google Places can’t find GPS coordinates for, the map should not be created (you can handle the error however you deem appropriate—but your program should not crash)</li>

    </ul></li>

   <li>The map should be centered and scaled appropriately so that all of the places are visible and that there is a reasonable amount of “padding” around the edges of the map (i.e., so that all of the sites are comfortably within the map frame and not all the way at the edge)</li>

   <li>The NationalSite should be displayed with a <em>different </em>marker than the NearbyPlaces. Note that the NationalSite may be returned as a result by Google Places, in which case it needs to be removed before the map is plotted. The NationalSite and all NearbyPlaces should display their name when a user hovers over the marker in plotly.</li>

  </ul></li>

</ul>




Here are examples of each:On the left is the result of calling plot_sites_for_state(‘mi’).

On the right is the result of calling plot_nearby_for_site(NationalSite(‘National Lakeshore’, ‘Sleeping Bear Dunes’))







Don’t worry about the fact that some of the markers appear to be off by a few fractions of a degree. This seems to have something to do with the projection we are using for plotly in our code (‘albers usa’) which doesn’t agree with the coordinates being produced by Google. If the data is correct and the maps are more or less in the right area, you will not get points off.




Grading:

<ul>

 <li>[20 points] Correct implementation of plot_sites_for_state(state_abbr)</li>

 <li>[20 points] Correct implementation of plot_nearby_for_site(site_object)</li>

</ul>




<h1>Part 4 (40 Points)</h1>

Make the program interactive. Here is a list of commands your program should accept and how it should handle them:




list &lt;stateabbr&gt;

available anytime

lists all National Sites in a state

valid inputs: a two-letter state abbreviation

nearby &lt;result_number&gt;

available only if there is an active result set

lists all Places near a given result

valid input for &lt;result number&gt;: an integer 1 to len(result_set_size)

map

available only if there is an active result set

displays the current results on a map

exit

exits the program

help

lists available commands (these instructions)




Note: a “result set” here refers to a list of NationalSites for a state or a list of NearbyPlaces for a NationalSite. You can implement this concept however you like, as long as the above semantics are preserved. <strong>This part shouldn’t be run when running the test, but it needs to be run when running the proj2_nps.py file.</strong>




A few notes on user experience:

<ul>

 <li>When the program starts, it is nice to tell the user what she is able to do. Don’t just give the user a blinking cursor with no information about what input she can provide. (This may seem obvious, but you’d be surprised how many such programs we have received.)</li>

 <li>Please number your result lists, so the user can see what number she is supposed to enter to search for a nearby place. Otherwise, the user has to count, which is…not great user experience.</li>

 <li>Your program will be much more usable if at each point the user knows what her options are. So, if the user searches first types “list mi” and your program provides her with a list of national sites in Michigan, you might want to then print above the user input prompt something like ‘Type “nearby &lt;result number&gt;” to search for places near one of the national sites above, “map” to map the list of national sites, or ‘list &lt;state&gt;” to do a search for another state:’ Or a less verbose version of something like that. Even though we will know what to do, it’s a good practice to write your programs so that they could be used by someone who is not familiar with it.</li>

</ul>




Here is a sample run of the program (albeit, with less elegant user experience than what is described above):


