var yws_add_jquery = document.createElement('script');
yws_add_jquery.src = 'http://code.jquery.com/jquery-1.11.0.min.js';
yws_add_jquery.type='text/javascript';
document.getElementsByTagName('head')[0].appendChild(yws_add_jquery);
var s_size = $('center>table>tbody>tr').size();
var t_a = 0;
var t_b = 0;
for (i = 2; i < s_size; i++) {
  var txt = $('center>table>tbody>tr').eq(i).find('td').eq(0).text();
  if (txt != 'xxx.cc' && txt != 'xxx.h') {
    $('center>table>tbody>tr').eq(i).hide();
  } else {
    var cov = $('center>table>tbody>tr').eq(i).find('td').eq(4).text();
    var spl = cov.split('/');
    var a = parseInt(spl[0]);
    var b = parseInt(spl[1]);
    t_a = t_a + a;
    t_b = t_b + b;
  }
}
console.log(t_a + " / " + t_b + " = " + t_a / t_b * 100);