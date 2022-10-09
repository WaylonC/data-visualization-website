<script>
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
    import { feature } from "topojson";
  
    onMount(async () => {
      if (browser) {

        //my stuff
        const response = await fetch(
          "https://raw.githubusercontent.com/WaylonC/filedump/main/precincts_pops_votes_all_precincts_topojson_file_no_zeroes_redundant.json"
        );
        const json = await response.json();
        const topoData = feature(json, json.objects.precincts_pops_votes_all_precincts_topojson_file_no_zeroes_redundant); 


        //existing location stuff
        const l_response = await fetch(`/locations.json`);
        const markers = await l_response.json();
  
        //leaflet stuff
        const L = await import('leaflet');
        const map = L.map('map');
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', 
          {attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'})
        .addTo(map);
        let bounds = [];

        //more location stuff
        for (let i = 0; i < markers.length; i++ ) {
          const marker = L.marker([markers[i].latitude, markers[i].longitude]).addTo(map);
          marker.bindPopup(markers[i].name);
          bounds.push([markers[i].latitude, markers[i].longitude]);
        }

        //my stuff
        
  
        //leaflet stuff
        map.fitBounds(bounds);
      }
    });
  </script>
  
  <svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" crossorigin=""/>
  </svelte:head>
  
  <div id="map" />
  
  <style lang="scss">
    #map {
      height: 400px;
      width: 100%;
    }
  </style>