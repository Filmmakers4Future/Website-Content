HiddenFromArchive: true
HiddenFromAuthor: true
HiddenFromCategory: true
HiddenFromSearch: true
HiddenFromSitemap: true
Section: true
SectionId: map
NoMarkdown: true

<script type="text/javascript">
	// Default map location and configuration
	var mapBaseURL = "https://kartevonmorgen.org/"
    var currentTag = ["#greenfilm"]
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
	function mapControl(element, loc, zoom, tag) {
		// If opened by onclick disable default (adding # to the url)
		if (event && element) {
			event.preventDefault();
			// Change the active element (style)
			if (element.parentElement.id == "categoryDropdown") {
				activeElement.classList.remove("active");
				element.classList.add("active");
				activeElement = element;
			}
			};
		// Checks if a new location, tag or zoom level is passed to the function
		// Using the default values if not
		currentLocation = Object.is(loc, null) ? currentLocation : loc;
		currentZoomLevel = Object.is(zoom, null) ? currentZoomLevel : zoom;
		currentTag = Object.is(tag, null) ? currentTag : tag;
		if (tag) {
			// Only change the search so a changed position by the user is not overwritten
			var url = `${mapBaseURL}#/?search=${encodeURI(currentTag.join(' ')).replace(/#/g,'%23')}`;
		} else {
			var url = `${mapBaseURL}#/?center=${currentLocation.join(',')}&zoom=${currentZoomLevel}&search=${encodeURI(currentTag.join(' ')).replace(/#/g,'%23')}`;
		}
		// Change iframe URL
		document.getElementById('greenProductionMap').src = url;
	}
	// Load the map via cookie or button click
	function loadMap(runtype) {
		if (runtype == "button") {
			cookieChoice = document.getElementById('saveSetting').checked
			if (cookieChoice) {
				createCookie("map", cookieChoice, 365)
			}
		}
		document.getElementById('map').style.background = 'none';
		document.getElementById('mapContainer').style["display"] = "";
		document.getElementById('privacyWarning').style["display"] = "none";
		mapControl(null, null, null, null);
	}
	// Cookie helpers
	function createCookie(cookieName,value,daysToExpire){
		var date = new Date();
		date.setTime(date.getTime()+(daysToExpire*24*60*60*1000));
		document.cookie = cookieName + "=" + value + "; expires=" + date.toGMTString();
	}
	function accessCookie(cookieName) {
		var name = cookieName + "=";
		var allCookieArray = document.cookie.split(';');
		for(var i=0; i<allCookieArray.length; i++) {
			var temp = allCookieArray[i].trim();
			if (temp.indexOf(name)==0)
			return temp.substring(name.length,temp.length);
 	  	}
		return "";
	}
	// Checks if a cookie exist and shows the map in case
	function cookieCheck() {
		var mapCookie = accessCookie("map");
		if (mapCookie === String(true)) {
			loadMap("cookie");
		}
	}
</script>

<div id ="mapContainer" style="display:none">
	<div markdown="1">#### Shortcuts</div>
	<div class="row justify-content-center text-white">
		<div class="col pt-2">
			<div class="dropdown" id="region">
				<button class="btn btn-secondary dropdown-toggle" type="button" id="categoryDropdownButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
					Select Category
				</button>
				<ul class="dropdown-menu scrollable-menu" id="categoryDropdown" role="menu" aria-labelledby="categoryDropdownButton">
					<a class="dropdown-item font-weight-bold active" id="defaultActive" href="#" onclick="mapControl(this, null, null, ['#greenfilm']);">All Entries</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#filmmakers4future']);">Our Supporters</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">Accomodation</h6>
					<a class="dropdown-item font-weight-bold" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#accommodation']);">All Entries</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#accomodation', '#apartment']);">Apartments</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#accomodation', '#hotel']);">Hotels</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">Departments</h6>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#camera']);">Camera</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#catering']);">Catering</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#costume']);">Costume</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#electricians']);">Electricians</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#grip']);">Grip</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#hairandmakeup']);">Hair & Makeup</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#location']);">Location</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#postproduction']);">Post-Production</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#production']);">Production</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#props']);">Props</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#setdesign']);">Set-Design</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#sfx']);">SFX</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#sound']);">Sound</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#transportation']);">Transportation</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#vfx']);">VFX</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">Rentals</h6>
					<a class="dropdown-item font-weight-bold" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#rental']);">All Entries</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#rental', '#camera']);">Camera</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#rental', '#grip']);">Grip</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#rental', '#light']);">Light</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#rental', '#vehicles']);">Vehicles</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">Sharing</h6>
					<a class="dropdown-item font-weight-bold" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#sharing']);">All Entries</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#sharing', '#carsharing']);">Car Sharing</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#sharing', '#bikesharing']);">Bike Sharing</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">Other</h6>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#greenconsulting']);">Green Consulting</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#shootinglocation']);">Shooting Locations</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#filmcommission']);">Film Commissions</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, null, null, ['#greenfilm', '#filmfestival']);">Film Festival</a>
				</ul>
			</div>
		</div>
		<div class="col pt-2">
			<div class="dropdown" id="cities">
				<button class="btn btn-secondary dropdown-toggle" type="button" id="cityDropdownButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
					Select City
				</button>
				<ul class="dropdown-menu scrollable-menu" role="menu" aria-labelledby="cityDropdownButton">
					<h6 class="dropdown-header">Canada</h6>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [45.493,-73.692], 10.00, null);">Montréal</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [46.803,-71.293], 10.00, null);">Québec</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [43.680,-79.443], 10.00, null);">Toronto</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [49.253,-123.139], 10.00, null);">Vancouver</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">Germany</h6>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [52.503,13.293], 11.00, null);">Berlin</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [50.975,11.014], 11.00, null);">Erfurt</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [53.548,9.957], 11.00, null);">Hamburg</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [50.939,6.944], 11.00, null);">Köln</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [51.340,12.335], 11.00, null);">Leipzig</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [48.134,11.544], 11.00, null);">München</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [52.399,13.011], 11.00, null);">Potsdam</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">UK</h6>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [51.452,-2.606], 10.00, null);">Bristol</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [51.480,-3.190], 10.00, null);">Cardiff</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [51.500,-0.196], 10.00, null);">London</a>
					<div class="dropdown-divider"></div>
					<h6 class="dropdown-header">USA</h6>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [33.747,-84.398], 10.00, null);">Atlanta</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [41.877,-87.670], 10.00, null);">Chicago</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [34.026,-118.264], 10.00, null);">Los Angeles</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [25.778,-80.211], 10.00, null);">Miami</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [44.958,-93.309], 10.00, null);">Minneapolis</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [29.931,-90.102], 10.00, null);">New Orleans</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [40.679,-73.996], 10.00, null);">New Yorck</a>
					<a class="dropdown-item" href="#" onclick="mapControl(this, [47.591,-122.324], 10.00, null);">Seattle</a>
				</ul>
			</div>
		</div>
		<div class="col pt-2">
				<div class="dropdown" id="region">
					<button class="btn btn-secondary dropdown-toggle" type="button" id="regionDropdownButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
						Select Region
					</button>
					<div class="dropdown-menu scrollable-menu" role="menu" aria-labelledby="regionDropdownButton">
						<a class="dropdown-item" href="#" onclick="mapControl(this, [46.195,7.031], 5.00, null);">Europe</a>
						<a class="dropdown-item" href="#" onclick="mapControl(this, [43.069,-96.328], 4.00, null);">North America</a>
					</div>
				</div>
		</div>
		<div class="col-xl-5 col-lg-5 pt-2">
			<form onsubmit="searchLocation(document.getElementById('locationSearch'))">
				<div class="input-group">
					<input type="text" id="locationSearch" class="form-control" placeholder="Search Location">
					<div class="input-group-append">
						<button class="btn btn-success" type="submit">Go</button> 
					</div>
				</div>
			</form>
		</div>
	</div>
	<iframe class="pt-3" id="greenProductionMap" name="greenProductionMap" style="height:75vh;border: none;" width="100%" height="100%"></iframe>
	<div markdown="1">**Please note:** If we have forgotten a category or an important film city in the shortcuts, please [contact us](/contact/).</div>
</div>

<div class="text-center text-white pt-5 pb-5" id="privacyWarning" style="display:none;" markdown="1">
**Privacy notice**  
This map uses the external services [mapoftomorrow.org](http://mapoftomorrow.org/) and [openstreetmap.org](https://openstreetmap.org/).  
You can find more information about this in our [Privacy Policy](/privacy#thirdparty).  
<div class="form-check">
	<input class="form-check-input" type="checkbox" value="" id="saveSetting">
	<label title="Cookie name: map - Cookie value: true - Expires in 365 days" class="form-check-label" for="saveSetting">
		Do not show again (a cookie is set).
	</label>
</div>
<button type="button" class="btn btn-info mt-2" onclick="loadMap('button')">Allow and Load Map</button>
</div>

<noscript>
	<div class="text-center text-white pt-5 pb-5">
	 <b>To use this map it is necessary to enable JavaScript.<br>
	 You can follow <a href="https://www.enable-javascript.com/">the instructions</a> to enable JavaScript in your web browser.</b>
	</div>
</noscript>

<!-- Connects to kartevonmorgen.org only if javascript is enabled -->
<script>
	// show privacy warning - only shows when javascript is enabled since map cant be used without it anyway
	document.getElementById('privacyWarning').style["display"] = "";
	// Set default active element
	var activeElement = document.getElementById('defaultActive');
	// Check if cookie is present
	cookieCheck()
</script>