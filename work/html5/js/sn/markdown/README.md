<!DOCTYPE HTML>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
        <script type="text/javascript" src="{{ url_for('static', filename='js/marked.min.js') }}"></script>
        <meta name="referrer" content="never">
        <link rel="icon" href="data:;base64,=">
        <title> </title>
    </head>
    <body>
        <div id="md">{{ md|safe }}</div>
        <script type="text/javascript">
            window.onload=function(){
                document.body.innerHTML = marked(document.getElementById('md').innerHTML);
            };
        </script>
    </body>
</html>
