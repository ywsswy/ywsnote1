<!DOCTYPE HTML>
<html>
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
        <script type="text/javascript" src="{{ url_for('static', filename='js/jquery-2.2.4.min.js') }}"></script>
		<script type="text/javascript" src="http://hammerjs.github.io/dist/hammer.min.js"></script>
        <script type="text/javascript" src="{{ url_for('static', filename='js/yIwannasee.js') }}?{{ randomNum }}"></script>
        <link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='css/yIwannasee.css') }}?{{ randomNum }}" />
		<link rel="icon" href="data:;base64,=">
        <title>搜索页</title>
    </head>
    <body id="tt2">
    </body>
	
		
	<script type="text/javascript">
		var hammerTest = new Hammer(document.getElementById('tt2'));
		var scaleCount = 1;
			hammerTest.get('pinch').set({
				enable: true
			});
		hammerTest.on("pinchout",e => {
            //console.log("触发" + (count++));
            if(scaleCount >= 2.5){
                //alert("max");
                return;
            }
            scaleCount += 0.03;
            document.body.style.zoom = scaleCount;
        });

        hammerTest.on("pinchin",e => {
            if(scaleCount <= 0.6){
                //alert("min");
                return;
            }
            scaleCount -= 0.03;
            document.body.style.zoom = scaleCount;
        });
	</script>
</html>
