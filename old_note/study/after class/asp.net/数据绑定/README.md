	<asp:GridView ID="GridView1" runat="server" AutoGenerateColumns="False" DataKeyNames="lin_id" DataSourceID="AccessDataSource1">
            <Columns>
                <asp:BoundField DataField="lin_id" HeaderText="lin_id" InsertVisible="False" ReadOnly="True" SortExpression="lin_id" />
                <asp:BoundField DataField="lnc_name" HeaderText="lnc_name" SortExpression="lnc_name" />
                <asp:CheckBoxField DataField="lnc_enable" HeaderText="lnc_enable" SortExpression="lnc_enable" />
                <asp:BoundField DataField="lnc_order" HeaderText="lnc_order" SortExpression="lnc_order" />
            </Columns>
        </asp:GridView>
        <asp:AccessDataSource ID="AccessDataSource1" runat="server" DataFile="~/db/news.mdb" 
            SelectCommand="SELECT * FROM [list_newsclass]">
        </asp:AccessDataSource>
        <asp:DropDownList ID="DropDownList1" runat="server" DataSourceID="ads_ddl" 
            DataTextField="lnc_name" DataValueField="lnc_id"></asp:DropDownList>
<asp:BulletedList ID="BulletedList1" runat="server"></asp:BulletedList>
	
string strAccessConnectionString= @"Provider=Microsoft.ACE.OLEDB.12.0;" +
@"Data Source=C:\Database\TestDB.mdb;";
