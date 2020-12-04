using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
namespace MySql0613
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        private void Form1_Load(object sender, EventArgs e)
        {
        }
        private void button3_Click(object sender, EventArgs e)
        {
            dt1 = db1.getDataTable("select * from info");
            label3.Text = dt1.Rows[0]["name"].ToString();//访问datatable中的数据
//dt1.Columns.Count
        }
        static public Mydb db1 = new Mydb("Database='hhh';Data Source='123.206.27.78';User Id='root';Password='';charset='gbk';pooling=true");
        static public DataTable dt1;
    }
}

