HiddenFromArchive: true
HiddenFromAuthor: true
HiddenFromCategory: true
HiddenFromSearch: true
HiddenFromSitemap: true
Section: true
SectionId: map

<script type="text/javascript">
	// Default map location and configuration
	var mapBaseURL = "https://kartevonmorgen.org/"
    var currentTag = ["greenfilm"]
    var currentLocation = [37.788,-30.938];
	var currentZoomLevel = 3.00;
	
	// Search for a location using the nominatim openstreetmap api
	function searchLocation(search) {
		event.preventDefault();
		if(search.value) {
			var xhr = new XMLHttpRequest();
			xhr.open('GET', `https://nominatim.openstreetmap.org/search?q=${search.value}&format=json&polygon=1&addressdetails=0`);
			xhr.onload = function() {
				if (xhr.status === 200) {
					var response = JSON.parse(xhr.responseText)
					var zoomLevel = getZoomLevel(response[0]["boundingbox"]);
					mapControl(null, [response[0]["lat"], response[0]["lon"]], zoomLevel, null)
					search.value = '';
				}
				else {
					alert('Search failed.  Returned status of ' + xhr.status);
				}
			};
			xhr.send(); 
		}
	}
	// Get a suitable zoom level for the size of the searched entity
	function getZoomLevel(boundingBox) {
		var size = Math.max(boundingBox[1]-boundingBox[0], boundingBox[3]-boundingBox[2]);
		// Look up zoom level: TO BE REPLACED BY SOME FANCY FORMULAR!!
		if (size < 0.05) { return 15} else
		if (size < 0.1) { return 14} else
		if (size < 0.3) { return 12} else
		if (size < 0.5) { return 11} else
		if (size < 1) { return 10} else
		if (size < 2) { return 9} else
		if (size < 4) { return 8} else
		if (size < 5) { return 6} else
		if (size < 10) { return 4} else {
			return 3.00
		}
	}
	// Function to control the iframe content
	function mapControl(event, loc, zoom, tag) {
		// If opened by onclick disable default (adding # to the url)
		if (event) {event.preventDefault()};
		// Checks if a new location, tag or zoom level is passed to the function
		// Using the default values if not
		currentLocation = Object.is(loc, null) ? currentLocation : loc;
		currentZoomLevel = Object.is(zoom, null) ? currentZoomLevel : zoom;
		currentTag = Object.is(tag, null) ? currentTag : tag;
		// Change iframe URL
		if (tag) {
			// Only change the search so a changed position by the user is not overwritten
			document.getElementById('greenProductionMap').src = `${mapBaseURL}#/?search=${currentTag.join('%20%23').replace(/^/,'%23')}`;
		} else {
			document.getElementById('greenProductionMap').src = `${mapBaseURL}#/?center=${currentLocation.join(',')}&zoom=${currentZoomLevel}&search=${currentTag.join('%20%23').replace(/^/,'%23')}`;
		}
	}
</script>

#### Shortcuts 
<div class="row justify-content-left text-white">
	<div class="col-xl-3 col-lg-2 col-md-4 col-sm-4 col-10 pt-2">
		<div class="dropdown" id="cities">
			<button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
				Select City
			</button>
			<div class="dropdown-menu scrollable-menu" aria-labelledby="dropdownMenuButton">
				<a class="dropdown-item" href="#" onclick="mapControl([45.493,-73.692], 10.00, null);">Canada - Montréal</a>
				<a class="dropdown-item" href="#" onclick="mapControl([46.803,-71.293], 10.00, null);">Canada - Québec</a>
				<a class="dropdown-item" href="#" onclick="mapControl([43.680,-79.443], 10.00, null);">Canada - Toronto</a>
				<a class="dropdown-item" href="#" onclick="mapControl([49.253,-123.139], 10.00, null);">Canada - Vancouver</a>
				<a class="dropdown-item" href="#" onclick="mapControl([52.503,13.293], 11.00, null);">Germany - Berlin</a>
				<a class="dropdown-item" href="#" onclick="mapControl([50.975,11.014], 11.00, null);">Germany - Erfurt</a>
				<a class="dropdown-item" href="#" onclick="mapControl([53.548,9.957], 11.00, null);">Germany - Hamburg</a>
				<a class="dropdown-item" href="#" onclick="mapControl([50.939,6.944], 11.00, null);">Germany - Köln</a>
				<a class="dropdown-item" href="#" onclick="mapControl([51.340,12.335], 11.00, null);">Germany - Leipzig</a>
				<a class="dropdown-item" href="#" onclick="mapControl([48.134,11.544], 11.00, null);">Germany - München</a>
				<a class="dropdown-item" href="#" onclick="mapControl([52.399,13.011], 11.00, null);">Germany - Potsdam</a>
				<a class="dropdown-item" href="#" onclick="mapControl([51.452,-2.606], 10.00, null);">UK - Bristol</a>
				<a class="dropdown-item" href="#" onclick="mapControl([51.480,-3.190], 10.00, null);">UK - Cardiff</a>
				<a class="dropdown-item" href="#" onclick="mapControl([51.500,-0.196], 10.00, null);">UK - London</a>
				<a class="dropdown-item" href="#" onclick="mapControl([33.747,-84.398], 10.00, null);">USA - Atlanta</a>
				<a class="dropdown-item" href="#" onclick="mapControl([41.877,-87.670], 10.00, null);">USA - Chicago</a>
				<a class="dropdown-item" href="#" onclick="mapControl([34.026,-118.264], 10.00, null);">USA - Los Angeles</a>
				<a class="dropdown-item" href="#" onclick="mapControl([25.778,-80.211], 10.00, null);">USA - Miami</a>
				<a class="dropdown-item" href="#" onclick="mapControl([44.958,-93.309], 10.00, null);">USA - Minneapolis</a>
				<a class="dropdown-item" href="#" onclick="mapControl([29.931,-90.102], 10.00, null);">USA - New Orleans</a>
				<a class="dropdown-item" href="#" onclick="mapControl([40.679,-73.996], 10.00, null);">USA - New Yorck</a>
				<a class="dropdown-item" href="#" onclick="mapControl([47.591,-122.324], 10.00, null);">USA - Seattle</a>
			</div>
		</div>
	</div>
	<div class="col-xl-3 col-lg-2 col-md-4 col-sm-4 col-10 pt-2">
			<div class="dropdown" id="region">
				<button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
					Select Region
				</button>
				<div class="dropdown-menu scrollable-menu" aria-labelledby="dropdownMenuButton">
					<a class="dropdown-item" href="#" onclick="mapControl([46.195,7.031], 5.00, null);">Europe</a>
					<a class="dropdown-item" href="#" onclick="mapControl([43.069,-96.328], 4.00, null);">North America</a>
				</div>
			</div>
	</div>
	<div class="col-xl-3 col-lg-2 col-md-4 col-sm-4 col-10 pt-2">
		<div class="dropdown" id="region">
			<button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
				Select Category
			</button>
			<div class="dropdown-menu scrollable-menu" aria-labelledby="dropdownMenuButton">
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm']);">All</a>
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm', 'rental']);">Rentals</a>
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm', 'catering']);">Catering</a>
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm', 'costume']);">Costume</a>
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm', 'makeup']);">Makeup</a>
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm', 'transport']);">Transportation</a>
				<a class="dropdown-item" href="#" onclick="mapControl(null, null, ['greenfilm', 'hotel']);">Hotels</a>
			</div>
		</div>
	</div>
	<div class="col-xl-3 col-lg-5 col-md-4 col-sm-4 col-10 pt-2">
		<form onsubmit="searchLocation(document.getElementById('locationSearch'))">
			<div class="input-group">
				<input type="text" id="locationSearch" class="form-control" placeholder="Search">
				<div class="input-group-append">
					<button class="btn btn-success" type="submit">Go</button> 
				</div>
			</div>
		</form>
	</div>
</div>

<noscript>
	<div class="text-center text-white pt-5 pb-5">
	 <b>To use this map it is necessary to enable JavaScript.<br>
	 You can follow <a href="https://www.enable-javascript.com/">the instructions</a> to enable JavaScript in your web browser.</b>
	</div>
</noscript>

<iframe class="pt-3" id="greenProductionMap" name="greenProductionMap" style="height:75vh;display: none;border: none;" width="100%" height="100%"></iframe>

**Please note:** Not all [available tags and cities](#mapHowTo) are listed in the scrollable shortcuts menu. In case you miss a link [contact us](/contact/).

<!-- Connects to kartevonmorgen.org only if javascript is enabled -->
<script>
	document.getElementById('greenProductionMap').style["display"] = "inline-block";
	mapControl(null, null, null, null);
</script>