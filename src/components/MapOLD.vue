<template>
  <div class="w-[100%] h-[100%]" ref="chartdiv">
  </div>
</template>

<script>
import * as am5 from "@amcharts/amcharts5";
import * as am5map from "@amcharts/amcharts5/map";
import am5geodata_worldLow from "@amcharts/amcharts5-geodata/worldLow";
import am5themes_Animated from "@amcharts/amcharts5/themes/Animated";


export default {
  name: 'HelloWorld',
  mounted() {
    let root = am5.Root.new(this.$refs.chartdiv);
    // Set themes
    // https://www.amcharts.com/docs/v5/concepts/themes/
    root.setThemes([
      am5themes_Animated.new(root)
    ]);

    // Create the map chart
    // https://www.amcharts.com/docs/v5/charts/map-chart/
    let chart = root.container.children.push(am5map.MapChart.new(root, {
      panX: "rotateX",
      panY: "translateY",
      projection: am5map.geoEqualEarth(),
      homeGeoPoint: { latitude: 2, longitude: 2 }
    }));

    let cont = chart.children.push(am5.Container.new(root, {
      layout: root.horizontalLayout,
      x: 20,
      y: 40
    }));

    // Add labels and controls
    cont.children.push(am5.Label.new(root, {
      centerY: am5.p50,
      text: "Map"
    }));

    let switchButton = cont.children.push(am5.Button.new(root, {
      themeTags: ["switch"],
      centerY: am5.p50,
      icon: am5.Circle.new(root, {
        themeTags: ["icon"]
      })
    }));

    switchButton.on("active", function () {
      if (!switchButton.get("active")) {
        chart.set("projection", am5map.geoMercator());
        chart.set("panY", "translateY");
        chart.set("rotationY", 0);
        backgroundSeries.mapPolygons.template.set("fillOpacity", 0);
      } else {
        chart.set("projection", am5map.geoOrthographic());
        chart.set("panY", "rotateY")

        backgroundSeries.mapPolygons.template.set("fillOpacity", 0.1);
      }
    });

    cont.children.push(
      am5.Label.new(root, {
        centerY: am5.p50,
        text: "Globe"
      })
    );

    // Create series for background fill
    // https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/#Background_polygon
    let backgroundSeries = chart.series.push(am5map.MapPolygonSeries.new(root, {}));
    backgroundSeries.mapPolygons.template.setAll({
      fill: root.interfaceColors.get("alternativeBackground"),
      fillOpacity: 0,
      strokeOpacity: 0
    });

    // Add background polygon
    // https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/#Background_polygon
    backgroundSeries.data.push({
      geometry: am5map.getGeoRectangle(90, 180, -90, -180)
    });

    // Create main polygon series for countries
    // https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/
    let polygonSeries = chart.series.push(am5map.MapPolygonSeries.new(root, {
      geoJSON: am5geodata_worldLow
    }));

    // Create line series for trajectory lines
    // https://www.amcharts.com/docs/v5/charts/map-chart/map-line-series/
    let lineSeries = chart.series.push(am5map.MapLineSeries.new(root, {
      // stroke: root.interfaceColors.get("background"),
      lineType: "curved",
      // stroke: am5.color(0xff0000),
      stroke: 'transparent',
      strokeWidth: 0,
      strokeOpacity: 0
    }));
    lineSeries.mapLines.template.setAll({
      stroke: root.interfaceColors.get("alternativeBackground"),
      strokeOpacity: 0.3,
      strokeWidth: 2,
    });

    // Create point series for markers
    // https://www.amcharts.com/docs/v5/charts/map-chart/map-point-series/
    let pointSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

    // pointSeries.bullets.push(function () {
    //   let circle = am5.Circle.new(root, {
    //     radius: 7,
    //     tooltipText: "Drag me!",
    //     cursorOverStyle: "pointer",
    //     tooltipY: 0,
    //     fill: am5.color(0xffba00),
    //     stroke: root.interfaceColors.get("background"),
    //     strokeWidth: 2,
    //     draggable: true
    //   });

    //   circle.events.on("dragged", function (event) {
    //     let dataItem = event.target.dataItem;
    //     let projection = chart.get("projection");
    //     let geoPoint = chart.invert({ x: circle.x(), y: circle.y() });

    //     dataItem.setAll({
    //       longitude: geoPoint.longitude,
    //       latitude: geoPoint.latitude
    //     });
    //   });

    //   return am5.Bullet.new(root, {
    //     sprite: circle
    //   });
    // });

    demo()

    function adds() {

      let lineSeries = chart.series.push(am5map.MapLineSeries.new(root, {
        // stroke: root.interfaceColors.get("background"),
        lineType: "curved",
        stroke: am5.color(0xffcc00),
        // stroke: 'transparent',
        strokeWidth: 0,
        strokeOpacity: 0
      }));
      lineSeries.mapLines.template.setAll({
        stroke: root.interfaceColors.get("alternativeBackground"),
        strokeOpacity: 0.3,
        strokeWidth: 2,
      });

      let pointSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

      pointSeries.bullets.push(function () {
        let circle = am5.Circle.new(root, {
          radius: 3,
          tooltipText: "Drag me!",
          cursorOverStyle: "pointer",
          tooltipY: 0,
          fill: am5.color(0xff0000),
          stroke: am5.color(0xff0000),
          strokeWidth: 0,
          draggable: true
        });

        circle.events.on("dragged", function (event) {
          let dataItem = event.target.dataItem;
          let projection = chart.get("projection");
          let geoPoint = chart.invert({ x: circle.x(), y: circle.y() });

          dataItem.setAll({
            longitude: geoPoint.longitude,
            latitude: geoPoint.latitude
          });
        });

        return am5.Bullet.new(root, {
          sprite: circle
        });
      });


      // let paris = addCity({ latitude: 48.8567, longitude: 2.351 }, "Paris");
      // let toronto = addCity({ latitude: 43.8163, longitude: -79.4287 }, "Toronto");
      let la = pointSeries.pushDataItem({ latitude: 35.163962458933845, longitude: 102.96029604471065 }); //35.163962458933845, 102.96029604471065
      let havana = pointSeries.pushDataItem({ latitude: -6.3014793020286435, longitude: 107.2054775093461 });
      let lineDataItem = lineSeries.pushDataItem({
        pointsToConnect: [la, havana]
      });


      let planeSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

      let plane = am5.Graphics.new(root, {
        svgPath:
          "M33,177 c117,-26 193,-77 292,-78 c25,2 77,13 78,78 c-2,60 -53,74 -77,74 c-100,1 -174,-50 -293,-72 l0,-2 l-2,5 l5,-5 l0,3 l-3,-3 l3,3 l-2,0 l3,0 l0,0",
        scale: 0.05,
        centerY: am5.p50,
        centerX: am5.p50,
        fill: am5.color(0xff0000)
      });

      planeSeries.bullets.push(function () {
        let container = am5.Container.new(root, {});
        container.children.push(plane);
        return am5.Bullet.new(root, { sprite: container });
      });


      let planeDataItem = planeSeries.pushDataItem({
        lineDataItem: lineDataItem,
        positionOnLine: 0,
        autoRotate: true
      });
      planeDataItem.dataContext = {};

      planeDataItem.animate({
        key: "positionOnLine",
        to: 1,
        duration: 2500,
        loops: null,
        easing: am5.ease.yoyo(am5.ease.linear)
      });


      planeDataItem.on("positionOnLine", (value) => {
        if (planeDataItem.dataContext.prevPosition < value) {
          plane.set("rotation", 0);
        }

        if (planeDataItem.dataContext.prevPosition > value) {

          lineSeries.mapLines.template.setAll({
            stroke: root.interfaceColors.get("alternativeBackground"),
            strokeOpacity: 0,
            strokeWidth: 0,
          });
          planeDataItem.dispose()
          // pointSeries.dispose()
          planeSeries.bullets = [];


          setTimeout(() => {
            
          pointSeries.dispose()
          target3()
          }, 1000); 


          // lineDataItem.dispose()
          //     plane.set("rotation", -180);
        }
        planeDataItem.dataContext.prevPosition = value;

      });
    }


    function target3() {

      let lineSeries = chart.series.push(am5map.MapLineSeries.new(root, {
        // stroke: root.interfaceColors.get("background"),
        lineType: "curved",
        stroke: am5.color(0xff0000),
        // stroke: 'transparent',
        strokeWidth: 0,
        strokeOpacity: 0
      }));
      lineSeries.mapLines.template.setAll({
        stroke: root.interfaceColors.get("alternativeBackground"),
        strokeOpacity: 0.3,
        strokeWidth: 2,
      });

      let pointSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

      pointSeries.bullets.push(function () {
        let circle = am5.Circle.new(root, {
          radius: 3,
          tooltipText: "Drag me!",
          cursorOverStyle: "pointer",
          tooltipY: 0,
          fill: am5.color(0xff0000),
          stroke: am5.color(0xff0000),
          strokeWidth: 0,
          draggable: true
        });

        circle.events.on("dragged", function (event) {
          let dataItem = event.target.dataItem;
          let projection = chart.get("projection");
          let geoPoint = chart.invert({ x: circle.x(), y: circle.y() });

          dataItem.setAll({
            longitude: geoPoint.longitude,
            latitude: geoPoint.latitude
          });
        });

        return am5.Bullet.new(root, {
          sprite: circle
        });
      });

      let la = pointSeries.pushDataItem({ latitude: -29.702510846828527, longitude: 125.27620359593858 });
      let havana = pointSeries.pushDataItem({ latitude: -2.2343895077357607, longitude: 120.06230386626092 });
      let lineDataItem = lineSeries.pushDataItem({
        pointsToConnect: [la, havana]
      });


      let planeSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

      let plane = am5.Graphics.new(root, {
        svgPath:
          "M33,177 c117,-26 193,-77 292,-78 c25,2 77,13 78,78 c-2,60 -53,74 -77,74 c-100,1 -174,-50 -293,-72 l0,-2 l-2,5 l5,-5 l0,3 l-3,-3 l3,3 l-2,0 l3,0 l0,0",
        scale: 0.08,
        centerY: am5.p50,
        centerX: am5.p50,
        fill: am5.color(0xff0000)
      });

      planeSeries.bullets.push(function () {
        let container = am5.Container.new(root, {});
        container.children.push(plane);
        return am5.Bullet.new(root, { sprite: container });
      });


      let planeDataItem = planeSeries.pushDataItem({
        lineDataItem: lineDataItem,
        positionOnLine: 0,
        autoRotate: true
      });
      planeDataItem.dataContext = {};

      planeDataItem.animate({
        key: "positionOnLine",
        to: 1,
        duration: 2000,
        loops: null,
        easing: am5.ease.yoyo(am5.ease.linear)
      });


      planeDataItem.on("positionOnLine", (value) => {
        if (planeDataItem.dataContext.prevPosition < value) {
          plane.set("rotation", 0);
        }

        if (planeDataItem.dataContext.prevPosition > value) {

          lineSeries.mapLines.template.setAll({
            stroke: root.interfaceColors.get("alternativeBackground"),
            strokeOpacity: 0,
            strokeWidth: 0,
          });
          planeDataItem.dispose()
          // pointSeries.dispose()
          planeSeries.bullets = [];

          setTimeout(() => {
            pointSeries.dispose()
            demo()
          }, 800);


          // lineDataItem.dispose()
          //     plane.set("rotation", -180);
        }
        planeDataItem.dataContext.prevPosition = value;

      });
    }

    function demo() {


      let lineSeries = chart.series.push(am5map.MapLineSeries.new(root, {
        // stroke: root.interfaceColors.get("background"),
        lineType: "curved",
        stroke: am5.color(0xff0000),
        strokeWidth: 0,
        strokeOpacity: 0
      }));
      lineSeries.mapLines.template.setAll({
        stroke: root.interfaceColors.get("alternativeBackground"),
        strokeOpacity: 0.3,
        strokeWidth: 2,
      });

      let pointSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

      pointSeries.bullets.push(function () {
        let circle = am5.Circle.new(root, {
          radius: 3,
          tooltipText: "Drag me!",
          cursorOverStyle: "pointer",
          tooltipY: 0,
          fill: am5.color(0xff0000),
          stroke: am5.color(0xff0000),
          strokeWidth: 0,
          draggable: true
        });

        circle.events.on("dragged", function (event) {
          let dataItem = event.target.dataItem;
          let projection = chart.get("projection");
          let geoPoint = chart.invert({ x: circle.x(), y: circle.y() });

          dataItem.setAll({
            longitude: geoPoint.longitude,
            latitude: geoPoint.latitude
          });
        });

        return am5.Bullet.new(root, {
          sprite: circle
        });
      });


      let paris = pointSeries.pushDataItem({ latitude: 10.238207388760944, longitude: 49.57826762046132 }); //10.238207388760944, 49.57826762046132
      let toronto = pointSeries.pushDataItem({ latitude: 43.8163, longitude: -79.4287 }, "Toronto");
      let la = pointSeries.pushDataItem({ latitude: 34.3, longitude: -118.15 }, "Los Angeles");
      let havana = pointSeries.pushDataItem({ latitude: 2.6807699300325862, longitude: 116.29102494078602 }); //2.6807699300325862, 116.29102494078602
      let lineDataItem = lineSeries.pushDataItem({
        pointsToConnect: [paris, havana],
      },{
        pointsToConnect: [toronto, la],
      });
      



      let planeSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));

      let plane = am5.Graphics.new(root, {
        svgPath:
          "M33,177 c117,-26 193,-77 292,-78 c25,2 77,13 78,78 c-2,60 -53,74 -77,74 c-100,1 -174,-50 -293,-72 l0,-2 l-2,5 l5,-5 l0,3 l-3,-3 l3,3 l-2,0 l3,0 l0,0",
        scale: 0.08,
        centerY: am5.p50,
        centerX: am5.p50,
        fill: am5.color(0xff0000)
      });

      planeSeries.bullets.push(function () {
        let container = am5.Container.new(root, {});
        container.children.push(plane);
        return am5.Bullet.new(root, { sprite: container });
      });


      let planeDataItem = planeSeries.pushDataItem({
        lineDataItem: lineDataItem,
        positionOnLine: 0,
        autoRotate: true
      });
      planeDataItem.dataContext = {};

      planeDataItem.animate({
        key: "positionOnLine",
        to: 1,
        duration: 2000,
        loops: null,
        easing: am5.ease.yoyo(am5.ease.linear)
      });

      planeDataItem.on("positionOnLine", (value) => {
        if (planeDataItem.dataContext.prevPosition < value) {
          plane.set("rotation", 0);
        }

        if (planeDataItem.dataContext.prevPosition > value) {

          lineSeries.mapLines.template.setAll({
            stroke: root.interfaceColors.get("alternativeBackground"),
            strokeOpacity: 0,
            strokeWidth: 0,
          });
          planeDataItem.dispose()
          // pointSeries.dispose()
          planeSeries.bullets = [];


          setTimeout(() => {
            pointSeries.dispose()
            adds()
          }, 800);
 


          // lineDataItem.dispose()
          //     plane.set("rotation", -180);
        }
        planeDataItem.dataContext.prevPosition = value;

      });
    }


    function addCity(coords, title) {
      return pointSeries.pushDataItem({
        latitude: coords.latitude,
        longitude: coords.longitude
      });
    }

    // Make stuff animate on load
    chart.appear(1000, 100);
  },

  beforeDestroy() {
    if (this.root) {
      this.root.dispose();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>