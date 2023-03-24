# Automated_Map_View
<!DOCTYPE html>
<html>
<head>
	<title>GIS Animation Example - California</title>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/leaflet.css" />
	<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/leaflet.js"></script>
	<style>
		#map {
			height: 500px;
			width: 100%;
		}
	</style>
</head>
<body>
	<div id="map"></div>
	<script>
		var map = L.map('map').setView([37.5, -119], 6);

		L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
			maxZoom: 18,
			attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
				'<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ' +
				'Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
			id: 'mapbox.streets'
		}).addTo(map);

		var california = L.geoJSON(null, {
			style: {
				color: "red",
				fillOpacity: 0.1
			}
		}).addTo(map);

		var url = "https://raw.githubusercontent.com/codeforamerica/click_that_hood/master/public/data/california-counties.geojson";

		fetch(url)
			.then(function(response) {
				return response.json();
			})
			.then(function(data) {
				california.addData(data);
				animateCounties(data.features);
			});

		function animateCounties(counties) {
			var i = 0;

			function animateMarker() {
				if (i < counties.length) {
					var bounds = L.geoJSON(counties[i]).getBounds();
					map.fitBounds(bounds);
					i++;
					setTimeout(animateMarker, 2000);
				}
			}

			animateMarker();
		}
	</script>
</body>
</html>
