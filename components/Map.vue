<template>
  <div id="map" class="map" style="height:calc(100vh - 3.25rem);" />
</template>

<script>
export default {
  name: "MapBox",
  mounted() {
    this.createMap()
  },
  methods: {
    createMap: () => {
      const mapboxgl = require("mapbox-gl")
      // mapboxgl.accessToken =
      //   "pk.eyJ1Ijoid3FyNjI0IiwiYSI6ImNpdTh0djF0cTAwMG0yb3BqMGwxeHJ4ZWQifQ.ZxuqJ97h61SLdI-CtV0HIw"
      // const map = new mapboxgl.Map({
      //   container: "map",
      //   style: "mapbox://styles/wqr624/cjlnqdmy96gku2rpdplxlo5pt",
      //   center: [-3, 55],
      //   zoom: 2
      // })

      mapboxgl.accessToken =
        "pk.eyJ1IjoidG9ueWNoYW4iLCJhIjoiY2p1aTFpaG0wMTV3MTQ0dWpvaHBxN2wycCJ9.Wn0OLDywvPKrpPEQHFd1_g"
      const map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/tonychan/cjujkp29y2fqr1fp1e4ixcetn",
        center: [115.8, 28.6], // 经纬度
        zoom: 3 // 3 差不多
      })
      map.addControl(new mapboxgl.NavigationControl())

      map.on("mouseover", "symbols", function(e) {
        console.log(e)
        map.getCanvas().style.cursor = "pointer"
      })

      // Change it back to a pointer when it leaves.
      map.on("mouseleave", "symbols", function(e) {
        console.log(e)
        map.getCanvas().style.cursor = ""
      })

      map.on("click", function(e) {
        var features = map.queryRenderedFeatures(e.point, {
          layers: ["travels"]
        })
        console.log(features.length)
        if (!features.length) {
          return
        }
        var feature = features[0]

        map.flyTo({
          center: feature.geometry.coordinates,
          zoom: map.getZoom() > 4 ? map.getZoom() : 4,
          bearing: 0,
          speed: 1,
          curve: 2,
          easing: function(t) {
            return t
          }
        })
        new mapboxgl.Popup({ offset: [0, -15] })
          .setLngLat(feature.geometry.coordinates)
          .setHTML(
            `<div class="tags has-addons"><span class="tag is-success is-medium">${
              feature.properties.place_name.split(", ")[0]
            }</span><span class="tag is-warning is-medium">${
              feature.properties.place_name.split(", ")[1]
            }</span><span class="tag is-danger is-medium">${
              feature.properties.year !== undefined
                ? feature.properties.year
                : "Home"
            }</span></div>`
          )
          .setLngLat(feature.geometry.coordinates)
          .addTo(map)
      })
    }
  }
}
</script>
<style></style>
