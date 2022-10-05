<script>
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
  
    onMount(async () => {
      if (browser) {
        const response = await fetch(`/locations.json`);
        const markers = await response.json();
  
        const L = await import('leaflet');
  
        const map = L.map('map');
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', 
          {attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'})
        .addTo(map);
  
  
        let bounds = [];
        
        for (let i = 0; i < markers.length; i++ ) {
          const marker = L.marker([markers[i].latitude, markers[i].longitude]).addTo(map);
          marker.bindPopup(markers[i].name);
          bounds.push([markers[i].latitude, markers[i].longitude]);
        }
  
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