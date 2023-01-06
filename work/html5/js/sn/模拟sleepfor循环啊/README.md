g_yws_stop = false;
function YwsWhileSleep() {
        console.log('YwsWhileSleep...');
        if (!g_yws_stop) {
        	setTimeout(function(){YwsWhileSleep();}, 1000);
	}
        // do something
}