---
layout: post
title: 本地流感流行情況
desc: 衛生署每週更新的流感監測數據，定點監測、實驗室監測、流感樣疾病爆發監測、醫院監測，以及流感季節期間的嚴重流感個案監測。
level: 第一級
tags: [公共衛生實況]
proj-num: 2
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-sheetrock/1.1.4/dist/sheetrock.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/locale/zh-hk.js"></script>
<script src="https://code.highcharts.com/stock/highstock.js"></script>




### 實驗室監測
公共衞生化驗服務處所收集的呼吸道樣本中，季節性流感病毒陽性百分比。當中分別顯示季節性流感病毒的甲型(H1)、甲型(H3)、乙型及丙型流感株類。

<div id="lab_surveillance" style="height: 400px; min-width: 310px"></div>

### 流感樣疾病爆發監測

<div id="outbreak_surveillance" style="height: 400px; min-width: 310px"></div>

### 備註 Remarks
- Since week 7 of 2014 (week ending 10 Feb, 2014), the Public Health Laboratory Services Branch has adopted new genetic tests as the primary tests for various respiratory viruses including influenza C.
- 由2014年第7周(截至2月10日的一周)起，公共衞生化驗服務處採用新的基因測試作為各種呼吸系統病毒的基本化驗方法，當中包括丙型流感。
- The surveillance system for severe influenza cases among adult patients aged 18 years or above was only activated intermittently during influenza seasons before 2018.
- 加強監測季節性流感嚴重個案系統(年齡為十八歲或以上)在2018年前只於流感季節期間運作。
- Note: All data of the recent few weeks are provisional figures and subject to further revision.
- 註: 最近數周的數據是臨時數字，可能會因資料的更新而作出修訂。


[原始數據](https://docs.google.com/spreadsheets/d/e/2PACX-1vTvpcEo56ToyySz2JPIH9tn4x78WYndxxcsJUHqrLsrREYJs2TWvTG2YUMwat5Bfyrl2KR0Ey2lSvlH/pubhtml)

數據等級：[第一級](/faq/#datalevel)
資料來源：[衛生防護中心 呼吸疾病辦事處 流感速遞](https://www.chp.gov.hk/tc/resources/29/304.html)

<script>  
	
var settings =  {

        rangeSelector: {
            selected: 4,
	    buttons: [{
		    type: 'ytd',
		    text: '一星期'
		}, {
		    type: 'month',
		    count: 1,
		    text: '一個月'
		}, {
		    type: 'month',
		    count: 3,
		    text: '三個月'
		}, {
		    type: 'month',
		    count: 6,
		    text: '六個月'
		}, {
		    type: 'year',
		    count: 1,
		    text: '一年'
		}, {
		    type: 'all',
		    text: '全部'
		}],
        },
	
    	time: {
        	useUTC: false
	},
        yAxis: {
            plotLines: [{
                value: 0,
                width: 2,
                color: 'silver'
            }]
        },
        tooltip: {
            pointFormat: '<span style="color:{series.color}">{series.name}</span>: <b>{point.y}</b>)<br/>',
            split: true
        }
    };
	var flu_sample_count_series = [];
	var flu_outbreaks = [];
	
    function updateChart(error, options, response) {
      console.log(response.rows);
      for (var i = 3; i < 7; i++) {
      	seriesNum = i-3;
	dataLabel = ["A(H1)甲型(H1)", "A(H3)甲型(H3)", "B乙型", "C丙型"]
      	flu_sample_count_series[seriesNum] = {name:dataLabel[seriesNum],data:[]};
	for (var j=1; j<response.rows.length; j++){
		flu_sample_count_series[seriesNum].data.push(
			[moment(response.rows[j].cellsArray[0], 'DD/MM/YYYY').valueOf(),
			parseInt(response.rows[j].cellsArray[i])]);
	}
      }
      Highcharts.stockChart('lab_surveillance', $.extend(settings, { series : flu_sample_count_series}));
      for (var i = 7; i < 8; i++) {
	flu_outbreaks[0] = {name:"學校/院舍爆發宗數", data: []};
	for (var j=1; j<response.rows.length; j++){
		flu_outbreaks[0].data.push(
			[moment(response.rows[j].cellsArray[0], 'DD/MM/YYYY').valueOf(),
			parseInt(response.rows[j].cellsArray[i])]);
	}	
      }
      Highcharts.stockChart('outbreak_surveillance', $.extend(settings, { series : flu_outbreaks}));

    }

    var mySpreadsheet = 'https://docs.google.com/spreadsheets/d/1EB5xOIotB7cWWyjXakpgwxJ1BypCaN5eEyPYYvYngkQ/edit?#gid=0';
    // Load an entire worksheet.
    sheetrock({
      url: mySpreadsheet,  
      query: "select D,E,F,G,H,I,J,Q order by D",
      callback: updateChart
    });
</script>
