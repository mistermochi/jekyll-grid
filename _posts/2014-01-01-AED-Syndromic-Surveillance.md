---
layout: post
title: 急症室傳染病症狀監測
desc: 以急診室求診的主要病徵為依據，追蹤市內主要傳染病症的流行程度。
tags: [公共衛生實況]
level: 第一級
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-sheetrock/1.1.4/dist/sheetrock.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/locale/zh-hk.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.1/Chart.min.js"></script>

Last updated by Centre for Health Protection: 
<span id="lastmodified"></span>

Last updated by Our Application: 
<span id="lastrequested"></span>

<div class="chart-container" style="position: relative; height:600px;">
    <canvas id="chart" height="600" width="600"></canvas>
</div>

<table id="statistics" class="table table-condensed table-striped"></table>
  
<script>  
  //update chart
      var flu = [];
      var hfmd = [];
      var ge = [];      
      var conjunctivitis = [];      
      var labels = [];
var chartColors = {
	red: 'rgb(255, 99, 132)',
	orange: 'rgb(255, 159, 64)',
	yellow: 'rgb(255, 205, 86)',
	green: 'rgb(75, 192, 192)',
	blue: 'rgb(54, 162, 235)',
	purple: 'rgb(153, 102, 255)',
	grey: 'rgb(201, 203, 207)'
};
	function parseDate(dateString){
		return moment(dateString,'DD-MMM-YYYY','en');
	}
	function getData(fetchSize){
		
    $('#statistics').sheetrock({
      url: mySpreadsheet,  
      query: "select D,E,F,G,H order by D desc",
      fetchSize: fetchSize,
  
      labels: ['End date of the week 該周的結束日期', 'Weekly average rate for the influenza-like illness syndrome group 流行性感冒病類症狀組的每周平均比率', 'Weekly average rate for the hand, foot and mouth disease syndrome group 手足口病症狀組的每周平均比率', 'Weekly average rate for the acute gastroenteritis syndrome group 急性腸道傳染病症狀組的每周平均比率', 'Weekly average rate for the acute conjunctivitis syndrome group 急性結膜炎症狀組的每周平均比率'],
      callback: updateChart
    });  
    
	}

		var ctx = document.getElementById("chart").getContext("2d");
		var cfg = {
			type: 'bar',
			data: {
				labels: [],
				datasets: [{
					label: "Influenza-like Illness Syndrome",
					data: [],
					type: 'line',
					pointRadius: 0,
					fill: false,
					lineTension: 0,
					borderWidth: 2,

                    backgroundColor: chartColors.blue,
                    borderColor: chartColors.blue,
				},{
					label: "Hand Foot and Mouth Disease Syndrome",
					data: [],
					type: 'line',
					pointRadius: 0,
					fill: false,
					lineTension: 0,
					borderWidth: 2
				},{
					label: "Acute Gastroenteritis Syndrome",
					data: [],
					type: 'line',
					pointRadius: 0,
					fill: false,
					lineTension: 0,
					borderWidth: 2,

                    backgroundColor: chartColors.red,
                    borderColor: chartColors.red,          
				},{
					label: "Acute Conjunctivitis Syndrome",
					data: [],
					type: 'line',
					pointRadius: 0,
					fill: false,
					lineTension: 0,
					borderWidth: 5,

                    backgroundColor: chartColors.green,
                    borderColor: chartColors.green,
				}]
			},
			options: {
                responsive: true,		
		maintainAspectRatio: false,
                title:{
                    display:true,
                    text:'急症科傳染病症狀監測 \n Accident and Emergency Departments Communicable Diseases Syndromic Surveillance'
                },
				scales: {
					xAxes: [{
						type: 'time',
						distribution: 'series',
						time: {
							parser: parseDate
						}
					}],
					yAxes: [{
						scaleLabel: {
							display: true,
							labelString: 'Weekly Average Rate'
						}
					}]
				}
			}
		};
		var chart = new Chart(ctx, cfg);
    function updateChart(error, options, response) {
      if (!response.rows){
      	return;
      }
      for (var i = 1; i < response.rows.length; i++) {
        labels.push(response.rows[i].cellsArray[0]);
        flu.push(response.rows[i].cellsArray[1]);
        hfmd.push(response.rows[i].cellsArray[2]);
        ge.push(response.rows[i].cellsArray[3]);        
        conjunctivitis.push(response.rows[i].cellsArray[4]);
      }
      chart.config.data.datasets[0].data = flu;
      chart.config.data.datasets[1].data = hfmd;
      chart.config.data.datasets[2].data = ge;
      chart.config.data.datasets[3].data = conjunctivitis;      
      chart.config.data.labels = labels;
      console.log(chart.config.data);
      chart.update();
      getData(500);
		}

    var mySpreadsheet = 'https://docs.google.com/spreadsheets/d/1xgMyJ5BT1R-1ZFukNy6oH4_SwaCSag6voon7a4yVkLo/edit?#gid=0';
  	getData(500);
</script>
