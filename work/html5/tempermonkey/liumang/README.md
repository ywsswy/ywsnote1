// ==UserScript==
// @name         laji-hidden & liumang-website & web-format
// @namespace    http://tampermonkey.net/
// @version      0.3
// @description  try to take over the world!
// @author       You
// @match        http://*/*
// @match        https://*/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
    // Your code here...
    // don't need jquery, don't need document.ready
    var Type = {attr: 0, click: 1};
    var list_liumang = ['www.hackhome.com'];
    // site can only be domain, not path
    var list_processing = [
                           {flag: false, site: 'localhost', selector: '#menubar-container', type: Type.attr, key: "hidden", value: true},
                           {flag: false, site: 'cquptx.cn', selector: '#cquptx_chat', type: Type.attr, key: "hidden", value: true},
                           {flag: false, site: 'cqu.pt', selector: '#_cqupt-side-box', type: Type.attr, key: "hidden", value: true},
                           {flag: false, site: 'csdn.net', selector: '.btn-readmore', type: Type.click},
                           {flag: true, site: 'www.bejson.com', selector: 'div#content-wrapper', type: Type.attr, key: "style", value: 'height: 888px;'},
                           {flag: true, site: 'www.bejson.com', selector: 'div#jsonformatter', type: Type.attr, key: "style", value: 'width: 48%; height: 888px;'},
                           {flag: true, site: 'www.bejson.com', selector: 'div.container.t-small-margin', type: Type.attr, key: "style", value: 'max-width: 1760px!important; width: 1760px!important'},
                           {flag: true, site: 'www.bejson.com', selector: 'div#aliLeftBox', type: Type.attr, key: "hidden", value: true},
                           {flag: true, site: 'www.bejson.com', selector: 'div.xf2-gg-left', type: Type.attr, key: "hidden", value: true},
                    ];
    function YwsMonitorProcessing(item,time) {
        var a = document.querySelector(item.selector);
        console.log('YwsMonitorProcessing, time: '+time+', info:'+JSON.stringify(item));
        if (a === null) {
            setTimeout(function(){YwsMonitorProcessing(item,time+1);},time*1000);
        } else {
            if (item.type === Type.click) {
                a.click();
            } else if (item.type === Type.attr) {
                a.setAttribute(item.key, item.value);
            }
        }
    }
    function YwsMain() {
        for (let item of list_processing) {
            if (true === item.flag) {
                //console.log('site:'+item.site);
                if (document.domain.search(new RegExp(item.site+'\$')) !== -1) {
                    console.log('regex ok:'+item.site+', invoke');
                    setTimeout(function(){YwsMonitorProcessing(item,1);},0);
                }
            }
        }

        if (list_liumang.indexOf(document.domain) !== -1) {
            alert('这是流氓网站');
        }
    }
    //window.addEventListener("load", YwsMain())
    YwsMain();
})();