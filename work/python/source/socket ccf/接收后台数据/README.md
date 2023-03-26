flask
from flask import Flask,request
import json
import sys
import time
import os
app = Flask(__name__)
q_time = 0
@app.route('/',methods=['POST'])
def ccf():
    global q_time
    r_query = request.form.get('input')
    data = json.loads(r_query)
    with open('C:/ccf/{}/{}-{}.txt'.format(q_date,q_date,q_time),'a+') as f:
        for item in data:
            f.write(item)
            f.write('\n')
    q_time += 1
    return 'wrong'
if __name__ == '__main__':
    global q_date
    q_date = sys.argv[1]
    os.makedirs('C:/ccf/{}'.format(q_date))
    app.run(port=84,host='0.0.0.0')

