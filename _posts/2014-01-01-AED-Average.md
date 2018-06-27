---
layout: post
title: 急症室輪候時間
desc: 各急症室每天在同一時間的平均等候時間。
proj-num: 1
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-sheetrock/1.1.4/dist/sheetrock.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/locale/zh-hk.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.1/Chart.js"></script>

<div id="charts">
</div>
<div id="hidden-charts" style="display: none;">
	<div id="chart-container" style="position: relative; height:200px;"><canvas id="chart" height="300" width="600"></canvas></div>
</div>

<table id="statistics" class="table table-condensed table-striped"></table>
  
<script>  
  //update chart 
    function createMatrix(N, M) {
    var matrix = new Array(N); // Array with initial size of N, not fixed!

    for (var i = 0; i < N; ++i) {
        matrix[i] = new Array(M);
    }

    return matrix;
}
function parseDate(dateString){
	return moment(dateString,'HH:mm','en');
}
     
      var labels = [];
      var dataMap = createMatrix(19,96);
	

		var ctx = document.getElementById("chart").getContext("2d");
		var cfg = {
			type: 'bar',
			options: {
                responsive: true,		
		maintainAspectRatio: false,
                title:{
                    display:true,
                    text:'急症科輪候時間 \n Accident and Emergency Department Waiting Time'
                },
				scales: {
					xAxes: [{
						type: 'time',
						distribution: 'series',
						time: {
							parser: null
						}
					}],
					yAxes: [{
						scaleLabel: {
							display: true,
							labelString: 'Estimated Waiting Time (hours)'
						}
					}]
				}
			}
		};
    function updateChart(error, options, response) {
      if (!response.rows){
      	return;
      }
      for (var i = 1; i < response.rows.length-1; i++) {
      	for (var j=0; j < response.rows[i].cellsArray.length; j++){
		if (j==0){
        		labels.push(response.rows[i].cellsArray[0]);
		} else {
			dataMap[j-1][i-1] = response.rows[i].cellsArray[j];
		}
	}
      }
      
      for (var i=0; i < 18; i++){
      
      var itm = document.getElementById("chart-container");
      var clone = itm.cloneNode(true);
      clone.id = "clone";
      var newClone = document.getElementById("charts").appendChild(clone);
      var chart = new Chart(newClone.firstChild.getContext("2d"), JSON.parse(JSON.stringify(cfg)));
      chart.config.options.scales.xAxes[0].time.parser = parseDate;
        chart.config.data = {};
	chart.config.data.datasets = new Array(1);
        chart.config.data.datasets[0] = {};
      	chart.config.data.datasets[0].data = dataMap[i];
      	chart.config.data.datasets[0].label = response.rows[0].cellsArray[i+1];  
      	chart.config.data.datasets[0].type = 'line';
      	chart.config.data.datasets[0].pointRadius = 0;
      	chart.config.data.datasets[0].borderWidth = 2;
      chart.config.data.labels = labels;
      
      chart.update();
      
      }
		}

    var mySpreadsheet = 'https://docs.google.com/spreadsheets/d/1XU_twWgBEorGgzykIcqbmjuBjwZHpV4jTkc1Go1twb0/edit#gid=648626318';
    $('#statistics').sheetrock({
      url: mySpreadsheet,
      callback: updateChart
    });  
</script>
