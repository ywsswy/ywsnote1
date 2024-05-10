////////////////////////////
    <form  class="form form-horizontal" id="form1" runat="server">
        <div class="row cl">
			<label class="form-label col-xs-4 col-sm-2"><span class="c-red">*</span>奶牛编号：</label>
			<div class="formControls col-xs-8 col-sm-9">
				<input type="text" class="input-text" value="" placeholder="奶牛编号" name="first">
			</div>
		</div>
		<div class="row cl">
			<label class="form-label col-xs-4 col-sm-2">分配设备</label>
			<div class="formControls col-xs-8 col-sm-9">
				<input type="text" class="input-text" value="" placeholder="分配设备" id="" name="second">
			</div>
		</div>
		<div class="row cl">
			<div class="col-xs-8 col-sm-9 col-xs-offset-4 col-sm-offset-2">
                <asp:Button ID="Button1" class="btn btn-primary radius" name="third" runat="server" OnClick="Button1_Click" Text="保存" />
			</div>
		</div>
    </form>
首先前端是要是form-runat-server提交表单
其次input要有name唯一值
然后button-runat-server触发提交
后台
string s1 = Request["first"].ToString();
