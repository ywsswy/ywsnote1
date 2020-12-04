#!/usr/bin/env python3

import flask
import urllib
import optparse

class YGlobal(object):
    flask_app = flask.Flask(__name__)

@YGlobal.flask_app.route('/url_api',methods=['GET'])
def UrlApi():
    ra_url = flask.request.args.get('url')
    header = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'}
    html_rq = urllib.request.Request(ra_url, headers = header)
    html_json = urllib.request.urlopen(html_rq).read()
    resp = flask.Response(html_json)
    resp.headers['Access-Control-Allow-Origin'] = '*'
    print(ra_url)
    print(html_json)
    return resp

if __name__ == '__main__':
    parser = optparse.OptionParser()
    parser.add_option("-p", default="91", action="store", help="set the webapp's port") # 这种参数名后面要指定值，可以通过options.p访问
    (YGlobal.options, args) = parser.parse_args()
    YGlobal.flask_app.config.update({'TEMPLATES_AUTO_RELOAD': True})
    YGlobal.flask_app.run(port=int(YGlobal.options.p), host='0.0.0.0') #, ssl_context='adhoc')