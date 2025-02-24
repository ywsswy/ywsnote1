永远不相信前端系列



def yGet_vc_control():
    return time.time()
    # return 'v5'


    templatePara = {}
    templatePara['vc_control'] = yGet_vc_control()
    return Response(response=render_template('info.html',**templatePara))



<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='css/yIwannasee.css') }}?{{ vc_control }}" />
