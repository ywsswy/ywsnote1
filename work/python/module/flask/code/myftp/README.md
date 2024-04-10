#!/usr/bin/env python3

# 示例，不过上传下载不会有md5错乱问题...建议直接使用网盘替代

import flask
import time
import os
import hashlib

def after_request(response):
    response.headers['Access-Control-Allow-Origin'] = '*'
    return response

class YGlobal(object):
    flask_app = flask.Blueprint('proxy', __name__, static_folder='static')
    flask_app_proxy = flask.Flask(__name__, static_folder=None)
    flask_app_proxy.after_request(after_request)
    args = None

@YGlobal.flask_app.route('/',methods=['POST','GET'])
def Index():
    file_py_path = os.path.abspath(__file__)
    path_index = max(file_py_path.rfind('\\'),file_py_path.rfind('/')) # Windows NT: \\, Unix: /
    file_path = ''.join([file_py_path[0:path_index],'/static/file'])
    if flask.request.method == 'POST': #auth_OK save_file
        r_file = flask.request.files.get('file')
        if r_file is not None:
            print(r_file.filename)
            r_file.save('{}/{}'.format(file_path,time.time()))
        return flask.redirect(flask.url_for('proxy.Index'))
    else: #auth_OK show_download_file_list
        s_s = r'''
<!DOCTYPE HTML>
<meta charset="utf-8">
<meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
<title>fileExplorer</title>
<form action="" method="post" enctype="multipart/form-data">
    <input type="file" name="file"><input type="submit" value="submit">
</form>
'''
        for file_item in os.listdir(file_path):
            s_s += '<a href="{path}?{vc_control}">{name}</a><br />'.format(path = flask.url_for('proxy.static',filename = 'file/{}'.format(file_item)), name = file_item, vc_control = time.time())
        return s_s

if __name__ == '__main__':
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument('-p', '--port', default='80', action='store',
        help='set the webapp\'s port')
    parser.add_argument('-e', '--encrypt', action='store_true',
        help='ENCRYPT: specify security options')
    parser.add_argument('-s', '--https', action='store_true',
        help='whether to use https should usually be set in nginx, not here')
    parser.add_argument('-l', '--localhost', action="store_true",
        help='use localhost')
    parser.add_argument('-b', '--basepath', default='', action="store",
        help='running behind a proxy tell it should remove the basepath')  # 如果是windows要写成"/\myftp"
    YGlobal.args = parser.parse_args()
    YGlobal.flask_app_proxy.config.update({'TEMPLATES_AUTO_RELOAD': True})
    YGlobal.flask_app_proxy.config.update({'SECRET_KEY': 'wtf'})
    host = None if YGlobal.args.localhost else '0.0.0.0'
    # ssl_context = ('x.crt', 'x.key') if YGlobal.args.https else None  # 判断是否使用签发的ssh证书(apache type)
    ssl_context = 'adhoc' if YGlobal.args.https else None  # 判断是否使用flask自带的证书
    YGlobal.flask_app_proxy.register_blueprint(YGlobal.flask_app, url_prefix=YGlobal.args.basepath)
    YGlobal.flask_app_proxy.run(port=int(YGlobal.args.port), host=host, ssl_context=ssl_context)