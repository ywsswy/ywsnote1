#!/usr/bin/env python3
import flask
class YError(ValueError):
    pass

class YGlobal(object):
    flask_app = flask.Flask(__name__)
    args = None

if __name__ == '__main__':
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument('-p', '--port', default='80', action='store',
        help='set the webapp\'s port')
    parser.add_argument('-e', '--encrypt', action='store_true',
        help='ENCRYPT: specify security options')
    parser.add_argument('-s', '--https', action='store_true',
        help='use https')
    parser.add_argument('-l', '--localhost', action="store_true",
        help='use localhost')
    YGlobal.args = parser.parse_args()
    YGlobal.flask_app.config.update({'TEMPLATES_AUTO_RELOAD': True})
    YGlobal.flask_app.config.update({'SECRET_KEY': 'fajweofajjjgljxz'})
    host = None if YGlobal.args.localhost else '0.0.0.0'
    ssl_context = 'adhoc' if YGlobal.args.https else None
    YGlobal.flask_app.run(port=int(YGlobal.args.port), host=host, ssl_context=ssl_context)