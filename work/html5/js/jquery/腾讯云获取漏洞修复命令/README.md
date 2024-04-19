var a = $('div.app-yunjing-table.auth-modal-table').eq(0).find('tbody').eq(0).find('tr').size();
var s = "";
for (var i = 0; i < a; i++) {
    s = s + $('div.app-yunjing-table.auth-modal-table').eq(0).find('tbody').eq(0).find('tr').eq(i).find('td>div>div').eq(3).html(); 
    s = s + ";";
} 
console.log(s);