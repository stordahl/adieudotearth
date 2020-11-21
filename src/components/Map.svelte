<script>
  import mapStyles from '../map-styles';
  import { onMount } from 'svelte';

	let container;
	let zoom = 4;
  let center = {lat: 44.9778, lng: -93.2650};
  let eventsDataArr = [];
  const api = "https://eonet.sci.gsfc.nasa.gov/api/v2.1/events";
  let infowindow;

  const createMarkers = ( map ) => {
      fetch(api)
      .then(res => {
        return  res.json();
      })
      .then( data => {
        eventsDataArr = data.events;
        console.log(eventsDataArr)
        return eventsDataArr;
      })
      .then( eventsDataArr => {
        for(let i = 0; i < eventsDataArr.length; i++) {
          let { id } = eventsDataArr[i].categories[0]      
          let positionRaw = eventsDataArr[i].geometries[0].coordinates
          let position = { lat: positionRaw[1], lng: positionRaw[0] }
          let srcUrl = eventsDataArr[i].sources[0].url
          let title = eventsDataArr[i].title 
          //creating markers
            let marker = new google.maps.Marker({
              position,  
              map,
              title,
              icon: "/"+ id + ".png",
            })
            //adding info window
            marker.addListener("click", () => {
            infowindow = new google.maps.InfoWindow({
              content: `
                <a href="${ srcUrl }" target=_blank><strong>${ title }</strong></a>
                <p>lat: ${ position.lat }, lng: ${ position.lng }</p>
              `,
            });
            infowindow.open(map, marker);
          })
          marker.setMap(map)
        }
      })
      .catch(err => console.log(err));
  }

	onMount( async () => {    
    const map = await new google.maps.Map(container, {
      zoom,
      center,
      styles: mapStyles
    });
    createMarkers( map );
	});
</script>

<style>
  .full-screen {
      width: 80vw;
      height: 80vh;
      margin:auto;
      margin-top:10vh;
  }

  @media screen and (max-width:768px){
    .full-screen {
      width: 90vw;
      /* height: 90vh; */
      margin:auto;
      margin-top:10vh;
    }
  }
</style>

<div class="full-screen" bind:this={container} />
