// ==UserScript==
// @name         szt
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
    var yws_add_css = document.createElement('style');
    yws_add_css.type = 'text/css';
    yws_add_css.innerHTML='*{font-family: '+String.fromCharCode(23665)+String.fromCharCode(33258)+String.fromCharCode(20307)+' !important;}';
    document.getElementsByTagName('head')[0].appendChild(yws_add_css);
})();
