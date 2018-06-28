---
layout: post
title: 基層醫療診所地圖
desc: 衛生署提供的基層醫療診所資料，包括地址、電話、駐診醫生等。
tags: [醫療服務地圖]
level: 第二級
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chrono-node@1.3.5/chrono.min.js"></script>
<!-- Google Maps API javascript -->

<script type="text/javascript">

var tableid = '1VGXeyVVrQujQ73WRaEVil6IvwFUWR-7w5FVXdbXV'; //the table id
var map;
var mapDiv;
var storedResponse;

/* INITIALIZE - initialize the map and geocoder */

function authenticated(){ google.maps.event.addDomListener(window, 'load', initialize); }



  function changePosition(position){

    if (position){
      map.setZoom(13);
      map.panTo(new google.maps.LatLng(position.coords.latitude, position.coords.longitude));
    }
    
  }
  
function initialize() {
  mapDiv = document.getElementById('map_canvas');
  map = new google.maps.Map(mapDiv, {
    center: new google.maps.LatLng(22.38269281766774, 114.10987863448963), //the center lat and long
    zoom: 11, //zoom
    mapTypeId: google.maps.MapTypeId.ROADMAP //the map style
  });
   var viewport = document.querySelector("meta[name=viewport]");
   viewport.setAttribute('content', 'initial-scale=1.0, user-scalable=no');
   mapDiv.style.width = '100%';
   mapDiv.style.height = '500px';
  
   var layer = new google.maps.FusionTablesLayer({
      map: map,
      heatmap: { enabled: false },
      query: {
        select: "col1",
        from: "1VGXeyVVrQujQ73WRaEVil6IvwFUWR-7w5FVXdbXV",
        where: ""
      },
      options: {
        styleId: 2,
        templateId: 2
      }
    });
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(changePosition, function(e){ });
    }
}

</script>
<script type="text/javascript" src="https://maps.google.com/maps/api/js?key=AIzaSyAm_dMcHD1az_PJ3HNyEH1A-ED-EvzUunE&callback=authenticated&v=3"></script>
  <div id="map_canvas"></div>
