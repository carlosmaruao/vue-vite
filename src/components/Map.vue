<template>
  <div class="w-[80%] h-[80%] bg-black" ref="chartdiv" id="chartdiv">
  </div>
</template>

<script>

//https://www.amcharts.com/docs/v4/chart-types/map/map-lines/




import * as am4core from "@amcharts/amcharts4/core";
import * as am4maps from "@amcharts/amcharts4/maps";
import am4geodata_worldLow from "@amcharts/amcharts4-geodata/worldLow";
// import am4themes_animated from "@amcharts/amcharts4/themes/animated";




export default {
  name: 'HelloWorld',
  data() {
    return {
      totalAttack: 4,
      speedAnimation: 1500, //ms
    }
  },
  mounted() {
    let totalAttack = this.totalAttack
    let speedAnimation = this.speedAnimation
    // Create map instance
    var chart = am4core.create("chartdiv", am4maps.MapChart);

    // Set map definition
    chart.geodata = am4geodata_worldLow;

    // Set projection
    chart.projection = new am4maps.projections.Miller();
    //***************************** START ******************************** */


    // Create map polygon series
    var polygonSeries = chart.series.push(new am4maps.MapPolygonSeries());

    // Make map load polygon (like country names) data from GeoJSON
    polygonSeries.useGeodata = true;

    // Configure series
    var polygonTemplate = polygonSeries.mapPolygons.template;
    polygonTemplate.tooltipText = "{name}";
    polygonTemplate.fill = am4core.color("#999"); 

    // Create hover state and set alternative fill color
    var hs = polygonTemplate.states.create("hover");
    hs.properties.fill = am4core.color("#ccc");

    // Remove Antarctica
    polygonSeries.exclude = ["AQ"];


    //***************************** END ******************************** */
    //***************************** START ******************************** */

    // // Focus Indonesia
    // chart.events.on("ready", function (ev) {
    //   polygonSeries.getPolygonById("ID").isHover = true;
    // });


    // // Add heat rule
    // polygonSeries.heatRules.push({
    //   "property": "fill",
    //   "target": polygonSeries.mapPolygons.template,
    //   "min": am4core.color("#ffffff"),
    //   "max": am4core.color("#AAAA00")
    // });

    // // Add heat legend
    // var heatLegend = chart.createChild(am4maps.HeatLegend);
    // heatLegend.series = polygonSeries;
    // heatLegend.width = am4core.percent(100);

    // polygonSeries.mapPolygons.template.events.on("over", function (ev) {
    //   if (!isNaN(ev.target.dataItem.value)) {
    //     heatLegend.valueAxis.showTooltipAt(ev.target.dataItem.value)
    //   }
    //   else {
    //     heatLegend.valueAxis.hideTooltip();
    //   }
    // });

    // polygonSeries.mapPolygons.template.events.on("out", function (ev) {
    //   heatLegend.valueAxis.hideTooltip();
    // });

    // // Add expectancy data
    // polygonSeries.data = [
    //   { id: "AF", value: 60.524 },
    //   { id: "AL", value: 77.185 },
    //   { id: "DZ", value: 70.874 },
    //   { id: "AO", value: 51.498 },
    //   { id: "AR", value: 76.128 },
    //   { id: "AM", value: 74.469 },
    //   { id: "AU", value: 82.364 },
    //   { id: "AT", value: 80.965 },
    //   { id: "AZ", value: 70.686 },
    //   { id: "BH", value: 76.474 },
    //   { id: "BD", value: 70.258 },
    //   { id: "BY", value: 69.829 },
    //   { id: "BE", value: 80.373 },
    //   { id: "BJ", value: 59.165 },
    //   { id: "BT", value: 67.888 },
    //   { id: "BO", value: 66.969 },
    //   { id: "BA", value: 76.211 },
    //   { id: "BW", value: 47.152 },
    //   { id: "BR", value: 73.667 },
    //   { id: "BN", value: 78.35 },
    //   { id: "BG", value: 73.448 },
    //   { id: "BF", value: 55.932 },
    //   { id: "BI", value: 53.637 },
    //   { id: "KH", value: 71.577 },
    //   { id: "CM", value: 54.61 },
    //   { id: "CA", value: 81.323 },
    //   { id: "CV", value: 74.771 },
    //   { id: "CF", value: 49.517 },
    //   { id: "TD", value: 50.724 },
    //   { id: "CL", value: 79.691 },
    //   { id: "CN", value: 75.178 },
    //   { id: "CO", value: 73.835 },
    //   { id: "KM", value: 60.661 },
    //   { id: "CD", value: 49.643 },
    //   { id: "CG", value: 58.32 },
    //   { id: "CR", value: 79.712 },
    //   { id: "CI", value: 50.367 },
    //   { id: "HR", value: 76.881 },
    //   { id: "CU", value: 79.088 },
    //   { id: "CY", value: 79.674 },
    //   { id: "CZ", value: 77.552 },
    //   { id: "DK", value: 79.251 },
    //   { id: "GL", value: 79.251 },
    //   { id: "DJ", value: 61.319 },
    //   { id: "DO", value: 73.181 },
    //   { id: "EC", value: 76.195 },
    //   { id: "EG", value: 70.933 },
    //   { id: "SV", value: 72.361 },
    //   { id: "GQ", value: 52.562 },
    //   { id: "ER", value: 62.329 },
    //   { id: "EE", value: 74.335 },
    //   { id: "ET", value: 62.983 },
    //   { id: "FJ", value: 69.626 },
    //   { id: "FI", value: 80.362 },
    //   { id: "FR", value: 81.663 },
    //   { id: "GA", value: 63.115 },
    //   { id: "GF", value: 79.9 },
    //   { id: "GM", value: 58.59 },
    //   { id: "GE", value: 74.162 },
    //   { id: "DE", value: 80.578 },
    //   { id: "GH", value: 60.979 },
    //   { id: "GR", value: 80.593 },
    //   { id: "GT", value: 71.77 },
    //   { id: "GN", value: 55.865 },
    //   { id: "GW", value: 54.054 },
    //   { id: "GY", value: 66.134 },
    //   { id: "HT", value: 62.746 },
    //   { id: "HN", value: 73.503 },
    //   { id: "HK", value: 83.199 },
    //   { id: "HU", value: 74.491 },
    //   { id: "IS", value: 81.96 },
    //   { id: "IN", value: 66.168 },
    //   { id: "ID", value: 70.624 },
    //   { id: "IR", value: 73.736 },
    //   { id: "IQ", value: 69.181 },
    //   { id: "IE", value: 80.531 },
    //   { id: "IL", value: 81.641 },
    //   { id: "IT", value: 82.235 },
    //   { id: "JM", value: 73.338 },
    //   { id: "JP", value: 83.418 },
    //   { id: "JO", value: 73.7 },
    //   { id: "KZ", value: 66.394 },
    //   { id: "KE", value: 61.115 },
    //   { id: "KP", value: 69.701 },
    //   { id: "KR", value: 81.294 },
    //   { id: "KW", value: 74.186 },
    //   { id: "KG", value: 67.37 },
    //   { id: "LA", value: 67.865 },
    //   { id: "LV", value: 72.045 },
    //   { id: "LB", value: 79.716 },
    //   { id: "LS", value: 48.947 },
    //   { id: "LR", value: 60.23 },
    //   { id: "LY", value: 75.13 },
    //   { id: "LT", value: 71.942 },
    //   { id: "LU", value: 80.371 },
    //   { id: "MK", value: 75.041 },
    //   { id: "MG", value: 64.28 },
    //   { id: "MW", value: 54.798 },
    //   { id: "MY", value: 74.836 },
    //   { id: "ML", value: 54.622 },
    //   { id: "MR", value: 61.39 },
    //   { id: "MU", value: 73.453 },
    //   { id: "MX", value: 77.281 },
    //   { id: "MD", value: 68.779 },
    //   { id: "MN", value: 67.286 },
    //   { id: "ME", value: 74.715 },
    //   { id: "MA", value: 70.714 },
    //   { id: "EH", value: 70.714 },
    //   { id: "MZ", value: 49.91 },
    //   { id: "MM", value: 65.009 },
    //   { id: "NA", value: 64.014 },
    //   { id: "NP", value: 67.989 },
    //   { id: "NL", value: 80.906 },
    //   { id: "NZ", value: 80.982 },
    //   { id: "NI", value: 74.515 },
    //   { id: "NE", value: 57.934 },
    //   { id: "NG", value: 52.116 },
    //   { id: "NO", value: 81.367 },
    //   { id: "SJ", value: 81.367 },
    //   { id: "OM", value: 76.287 },
    //   { id: "PK", value: 66.42 },
    //   { id: "PA", value: 77.342 },
    //   { id: "PG", value: 62.288 },
    //   { id: "PY", value: 72.181 },
    //   { id: "PE", value: 74.525 },
    //   { id: "PH", value: 68.538 },
    //   { id: "PL", value: 76.239 },
    //   { id: "PT", value: 79.732 },
    //   { id: "QA", value: 78.231 },
    //   { id: "RO", value: 73.718 },
    //   { id: "RU", value: 67.874 },
    //   { id: "RW", value: 63.563 },
    //   { id: "SA", value: 75.264 },
    //   { id: "SN", value: 63.3 },
    //   { id: "RS", value: 73.934 },
    //   { id: "SL", value: 45.338 },
    //   { id: "SG", value: 82.155 },
    //   { id: "SK", value: 75.272 },
    //   { id: "SI", value: 79.444 },
    //   { id: "SB", value: 67.465 },
    //   { id: "SO", value: 54 },
    //   { id: "ZA", value: 56.271 },
    //   { id: "SS", value: 54.666 },
    //   { id: "ES", value: 81.958 },
    //   { id: "LK", value: 74.116 },
    //   { id: "SD", value: 61.875 },
    //   { id: "SR", value: 70.794 },
    //   { id: "SZ", value: 48.91 },
    //   { id: "SE", value: 81.69 },
    //   { id: "CH", value: 82.471 },
    //   { id: "SY", value: 71 },
    //   { id: "TW", value: 79.45 },
    //   { id: "TJ", value: 67.118 },
    //   { id: "TZ", value: 60.885 },
    //   { id: "TH", value: 74.225 },
    //   { id: "TL", value: 67.033 },
    //   { id: "TG", value: 56.198 },
    //   { id: "TT", value: 69.761 },
    //   { id: "TN", value: 75.632 },
    //   { id: "TR", value: 74.938 },
    //   { id: "TM", value: 65.299 },
    //   { id: "UG", value: 58.668 },
    //   { id: "UA", value: 68.414 },
    //   { id: "AE", value: 76.671 },
    //   { id: "GB", value: 80.396 },
    //   { id: "US", value: 78.797 },
    //   { id: "UY", value: 77.084 },
    //   { id: "UZ", value: 68.117 },
    //   { id: "VE", value: 74.477 },
    //   { id: "PS", value: 73.018 },
    //   { id: "VN", value: 75.793 },
    //   { id: "YE", value: 62.923 },
    //   { id: "ZM", value: 57.037 },
    //   { id: "ZW", value: 58.142 }
    // ];
    //***************************** END ******************************** */

    //***************************** START ******************************** */
    // /* Northern Europe */
    // var series1 = chart.series.push(new am4maps.MapPolygonSeries());
    // series1.name = "Northern Europe";
    // series1.useGeodata = true;
    // series1.include = ["FI", "DK", "SE", "NO", "LT", "LV", "EE", "IS"];
    // series1.mapPolygons.template.tooltipText = "{name}";
    // series1.mapPolygons.template.fill = am4core.color("#96BDC6");
    // series1.fill = am4core.color("#96BDC6");


    // chart.legend = new am4maps.Legend();
    // chart.legend.position = "right";
    // chart.legend.align = "right";
    //***************************** END ******************************** */

    // // Remove Antarctica
    // polygonSeries.exclude = ["AQ"];
    let dataMonitoring = [
      {
        source: [47.88514389176945, 35.466255282648866],
        destination: [-1.9324077813156306, 120.95707202193014]
      },
      {
        source: [54.86450795257499, 158.02681881599315],
        destination: [-6.591075394883076, 107.19887285526326]
      },
      {
        source: [-33.43268777475732, 25.37720858988057],
        destination: [-6.273695105058572, 106.8005065437078]
      },
      {
        source: [57.114327059301154, 93.98186028673435],
        destination: [-7.316284616783103, 112.93868758726877]
      },
      {
        source: [42.034433050261256, 124.12682914804248],
        destination: [0.8246505698564869, 109.498278747671]
      },
      {
        source: [-37.447885069302295, 146.79944477943454],
        destination: [2.8384966896187147, 98.74170211207091]
      },
      {
        source: [15.354908481502253, 11.18850448403243],
        destination: [-6.604872308249618, 106.29707312423967]
      },
      {
        source: [20.363728108489013, 48.323970354680874],
        destination: [-3.200501187066752, 114.61925005788537]
      },
      {
        source: [-5.235780141035137, -58.855520763627666],
        destination: [-0.5097547380814649, 102.20909017385861]
      },
      {
        source: [37.966649205886824, -104.17139902335576],
        destination: [-6.604872308249618, 106.29707312423967]
      },
      {
        source: [54.48723649790835, -65.36818187299636],
        destination: [-3.200501187066752, 114.61925005788537]
      },
      {
        source: [37.966649205886824, -104.17139902335576],
        destination: [-4.110232185783703, 137.1215904015985]
      },
      {
        source: [56.11996150302935, -67.81731418497219],
        destination: [-6.146760773331923, 106.7709373991547]
      },
      {
        source: [29.895958663510946, -8.306119444218757],
        destination: [-6.146760773331923, 106.7709373991547]
      },
      {
        source: [27.719083218985574, 64.67686191692475],
        destination: [0.4194248067920472, 99.49417367212754]
      },
      {
        source: [35.13429386910776, 136.34403561423815],
        destination: [-2.5007556511419886, 101.6179204288162]
      },
      {
        source: [36.10709277789241, 93.21943023261342],
        destination: [-2.957974447970681, 128.55586726379823]
      },
      {
        source: [49.1581813805781, 79.7305248447709],
        destination: [-3.5886940204062774, 103.16202894321911]
      },
      {
        source: [-21.385083012451805, 144.80014498465135],
        destination: [-5.315963573213842, 119.97385671238301]
      },
      {
        source: [8.3074017303305, 99.56151007421359],
        destination: [-7.060339867697774, 111.47142871351194]
      },
    ];
    //***************************** START POINT MARKER******************************** */

    // Create image series
    var imageSeries = chart.series.push(new am4maps.MapImageSeries());

    // Create a circle image in image series template so it gets replicated to all new images
    var imageSeriesTemplate = imageSeries.mapImages.template;
    var circle = imageSeriesTemplate.createChild(am4core.Circle);
    circle.radius = 5;
    circle.fill = am4core.color("#ff0000");
    circle.stroke = am4core.color("#ffcc00");
    circle.strokeWidth = 1;
    circle.nonScaling = true;
    circle.tooltipText = "{title}";


    // Set property fields
    imageSeriesTemplate.propertyFields.latitude = "latitude";
    imageSeriesTemplate.propertyFields.longitude = "longitude";

    // Add data for the three cities
    imageSeries.data = [];
    const ListSourceAttack = [];
    const ListDestination = [];
    dataMonitoring.map(val => {
      const data = {
        "latitude": val.source[0],
        "longitude": val.source[1],
        "title": null //nama tooltip
      }
      ListSourceAttack.indexOf(val.source) === -1 ? ListSourceAttack.push(val.source) : ''
      ListDestination.indexOf(val.destination) === -1 ? ListDestination.push(val.destination) : ''
      imageSeries.data.push(data)
    }) 
    //***************************** END ******************************** */ 


    // let counter = 0
    var items = Array(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11); 
    // setInterval(() => {
    setData(totalAttack)
    // }, 1000);


    // console.log(randomizedArr);
    function setData(tots) {
      // define a function to generate random numbers
      function randomSort(a, b) {
        return Math.random() - 0.5;
      }

      // call the sort() method with the randomSort function
      const randomizedArr = items.sort(randomSort);
      // console.log(randomizedArr)
      for (let i = 0; i < tots; i++) {
        const randInd = randomizedArr[i]
        const element = dataMonitoring[randInd];
        // console.log(dataMonitoring[randInd])
        // console.log('====')
        // setTimeout(() => {

    let sumber = ListSourceAttack[Math.floor(Math.random() * ListSourceAttack.length)]; 
    let target = ListDestination[Math.floor(Math.random() * ListDestination.length)]; 
        runAttack(sumber, target, speedAnimation);
        // }, 1000);
        // counter++
      }
      // no = items[Math.floor(Math.random() * items.length)];


      // dataMonitoring.map(val => {

      //   if (counter < no) { 
      //     runAttack(val.source, val.destination, val.speed);
      //   }else{

      //     console.log(counter, no)
      //   }
      //   counter++
      // })
      // setData()
    }

    // runAttack([aa], [dd], 1000);
    //***************************** START ANIMATION PLANE ******************************** */

    function runAttack(dari, tujuan, speed) {

    var typeSerangan = ['#ff0000', '#ffcc00', '#8338ec'];
    let randTypeSerangan = typeSerangan[Math.floor(Math.random() * typeSerangan.length)];
      // Add line series
      var lineSeries = chart.series.push(new am4maps.MapLineSeries());
      lineSeries.mapLines.template.strokeWidth = 2;
      lineSeries.mapLines.template.stroke = randTypeSerangan; // am4core.color("#e63946"); 
      lineSeries.mapLines.template.nonScalingStroke = true;


      var line = lineSeries.mapLines.create();
      line.multiGeoLine = [[
        { "latitude": dari[0], "longitude": dari[1] },
        { "latitude": tujuan[0], "longitude": tujuan[1] }
      ]];


      // //arah panah
      // let arrow = line.arrow;
      // arrow.position = 1;

      // Add a map object to line
      var bullet = line.lineObjects.create();
      bullet.nonScaling = true;
      bullet.position = 0.5;
      bullet.width = 18;
      bullet.height = 18;
      bullet.horizontalCenter = "middle";
      bullet.verticalCenter = "middle";


      var plane = bullet.createChild(am4core.Sprite);
      plane.scale = 0.05;
      plane.path = "M33,177 c117,-26 193,-77 292,-78 c25,2 77,13 78,78 c-2,60 -53,74 -77,74 c-100,1 -174,-50 -293,-72 l0,-2 l-2,5 l5,-5 l0,3 l-3,-3 l3,3 l-2,0 l3,0 l0,0";
      plane.fill = randTypeSerangan; // am4core.color("#ff0000");
      plane.strokeOpacity = 0;

      function goPlane() {
        // line.multiGeoLine = []
        var animation = bullet.animate({
          from: 0,
          to: 1,
          property: "position"
        }, speed, am4core.ease.sinInOut);


        // var from = bullet.position, to;
        // if (from == 0) {


        setInterval(() => {
          // console.log(lineSeries.dispose())
          plane.dispose()

          line.dispose()
          animation.dispose()
          // chart.series = []
          // line.dispose()
          // line = []
          // setTimeout(() => { 
          // chart.dispose() 
          // }, 1000);
          // setData()
        }, speedAnimation);

        //   console.log('xxx')
        //   to = 1;
        //   plane.rotation = 0;
        // }
        // else {
        //   console.log('finish')   
        // }
      }

      // setTimeout(() => { 
      goPlane();
      // }, 200);

      // runAttack()
    }


    setInterval(() => {
      // console.log(lineSeries.dispose())
      // plane.dispose() 

      // line.dispose()
      // animation.dispose() 
      // chart.series = []
      // line.dispose()
      // line = []
      // setTimeout(() => { 
      // chart.dispose()
      let array = [1, 2, 3, 4, 5]
      let randomElement = array[Math.floor(Math.random() * array.length)];
      setData(randomElement)
      // }, 1000);
      // setData()
    }, speedAnimation);
    //***************************** START ANIMATION PLANE ******************************** */




    //***************************** END ******************************** */

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