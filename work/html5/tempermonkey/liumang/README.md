// ==UserScript==
// @name         laji-hidden & liumang-website
// @namespace    http://tampermonkey.net/
// @version      0.2
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
    var Type = {hide: 0, click: 1};
    var list_liumang = ['www.hackhome.com'];
    var list_processing = [{flag: true, site: 'localhost', selector: '#menubar-container', type: Type.hide},
                {flag: true, site: 'cquptx.cn', selector: '#cquptx_chat', type: Type.hide},
                {flag: true, site: 'cqu.pt', selector: '#_cqupt-side-box', type: Type.hide},
                {flag: true, site: 'csdn.net', selector: '.btn-readmore', type: Type.click}
                    ];
    function YwsMonitorProcessing(item,time){
        var a = document.querySelector(item.selector);
        console.log('hello'+time);
        if(a === null){
            setTimeout(function(){YwsMonitorProcessing(item,time+1);},time*1000);
        }else{
            if(item.type === Type.hide){
                a.setAttribute("hidden",true);
            }else if(item.type === Type.click){
                a.click();
            }
        }
    }
    function YwsMain(){
        for (let item of list_processing){
            if(true === item.flag && document.domain.search(new RegExp(item.site+'\$')) !== -1){
                setTimeout(function(){YwsMonitorProcessing(item,1);},0);
            }
        }

        if(list_liumang.indexOf(document.domain) !== -1){
            alert('流氓网站');
        }
    }
    //window.addEventListener("load", YwsMain())
    YwsMain();
})();