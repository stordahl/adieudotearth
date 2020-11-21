<script>
  // export let lat;
  // export let lng;
  export let center
  export let map

  const marker = new google.maps.Marker({
    map,
    position: center,
    title: "🔥",
  });

</script>

<i>{marker}</i>