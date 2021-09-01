## 骚操作1，form的target设置到一个iframe的name上，这样input提交的时候就不会刷新页面，而是在iframe里打开，适合那些不想刷新页面又做不了ajax的情况
<form id="puppet_form" target="puppet_iframe">
<input type="submit" value="Send" onclick="YwsMain()"/>
</form>
<iframe name="puppet_iframe" style="display:none;"></iframe>