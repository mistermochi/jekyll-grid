---
layout: post
title: 健康資訊搜尋器
desc: 為你搜尋醫管局、衛生署、各大醫院及醫學院提供的資料。不定時更新。
level: 第一級
tags: [健康資訊]
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>

## 健康資訊搜尋器
本搜尋引擎將為你搜尋醫管局、衛生署、各大醫院及醫學院所提供的資料。<span id="db_count"></span>

<div class="total_data">
</div>
<form id="search_form">
	<input type="text" id="selector"  placeholder="Search..">
	<input type="submit" id="submit">
</form>	
<div class="result_count"></div>
<div class="result_link">
</div>

<script>
    function base64toImg(base64){
        return "<img src='data:image/png;base64,"+base64.replace(/["']/g, "")+"'>";
    }
    function updateSum(error, options, response) {
      console.log(response.rows);
      $('div.total_data').empty();
      $('div.total_data').text("We have indexed "+response.rows[0].cellsArray[0]+" patient education resources, and counting.")
    }
    function updateTotal(response) {
      if (!response || !response.rows || !response.rows[0] || !response.rows[0][0])
         return;
      console.log(response.rows);
      $('#db_count').empty();
      $('#db_count').text("本資料庫現存有 "+response.rows[0][0]+" 份資料。")
    }
    function updateChart(response) {
	if (!response.rows){
     	 $('div.result_count').empty();
         $('div.result_count').text("We have found no results.");
	  return;
	}
      console.log(response.rows);
      $('div.result_link').empty();
      $('div.result_count').empty();
      $('div.result_count').text("We have found " + response.rows.length + " results.");
      for (var i=0; i<response.rows.length; i++){
         $('div.result_link').append(
            "<a href='"+response.rows[i][0]+"'>"+
            "<p>"+response.rows[i][0]+"</p>"+
            "<p>"+response.rows[i][2]+"</p>"+
            base64toImg(response.rows[i][1])+
            "</a>"                                
         );
      }
    }
    // search function
    $('#search_form').on('submit', function(e) { //use on if jQuery 1.7+
        e.preventDefault();
	
        var query = $('#selector').val();

	$.ajax({
	    url: "https://script.google.com/macros/s/AKfycbzr0R-IGH3xbXPcIs81BF1q_oe_6SQ34t7F1GpZxsXMykTlXA/exec?q=" + query,

	    // The name of the callback parameter, as specified by the YQL service
	    jsonpCallback: 'callback',

	    // Tell jQuery we're expecting JSONP
	    dataType: "jsonp",

	    // Work with the response
	    success: updateChart
	});		
        $('div.result_link').text("Loading...");		    
    });	
    // Show total number
    
	$.ajax({
	    url: "https://script.google.com/macros/s/AKfycbzr0R-IGH3xbXPcIs81BF1q_oe_6SQ34t7F1GpZxsXMykTlXA/exec?total=t",

	    // The name of the callback parameter, as specified by the YQL service
	    jsonpCallback: 'callback',

	    // Tell jQuery we're expecting JSONP
	    dataType: "jsonp",

	    // Work with the response
	    success: updateTotal
	});		
			
</script>
