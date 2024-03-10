import flask
class YGlobalDataMain(object):
    flask_app = flask.Flask(__name__)
# url api # begin #########################################################################################
import flask
# import time
import subprocess
def yGetVcControl():
    # return time.time()
    return 'inner_v1'
@YGlobalDataMain.flask_app.route('/',methods=['GET'])
def yUrlApi():
    r_query = flask.request.args.get('query')
    cmd = ['coffee', '-e', '{}'.format(r_query)]
    cmd_res = subprocess.run(cmd, stdout=subprocess.PIPE, universal_newlines=True, timeout=1.0)
    print(cmd_res.stdout)
    return cmd_res.stdout
if __name__ == '__main__':
    # main
    YGlobalDataMain.flask_app.config.update({'TEMPLATES_AUTO_RELOAD': True})
    # main
    YGlobalDataMain.flask_app.run(port=85, host='0.0.0.0', ssl_context='adhoc')

