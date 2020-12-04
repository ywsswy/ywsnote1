using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data;
using System.Data.OleDb;
using System.Configuration;
public partial class SQLHelp : System.Web.UI.Page
{
    protected void Page_Load(object sender, EventArgs e)
    {
    }
    private OleDbConnection conn;
    public SQLHelp()
    {
        string strConn = ConfigurationManager.ConnectionStrings["strCon"].ToString();
        conn = new OleDbConnection(strConn);
    }
    /// <summary>
    /// 执行增删改语句返回受影响的行数
    /// </summary>
    public int NonQuery(string strSql)
    {
        int count = 0;
        OleDbCommand cmd = new OleDbCommand(strSql, conn);
        //try
        //{
        conn.Open();
        count = cmd.ExecuteNonQuery();
        //}
        //finally
        //{
        conn.Close();
        //   }
        return count;
    }
        /// <summary>d
        /// 执行查询语句并返回第一行第一列
        /// </summary>
        public object Scalar(string strSelect)
        {
            object obj = null;
            SqlCommand cmd = new SqlCommand(strSelect, conn);
            //try
            //{
            conn.Open();
            obj = cmd.ExecuteScalar();
            //}
            //finally
            //{
            conn.Close();
            //    }
            return obj;
        }
        /// <summary>
        /// 返回DataTable
        /// </summary>
        public DataTable getDataTable(string strSelect)
        {
            DataTable dt = new DataTable();
            try
            {
                SqlDataAdapter dr = new SqlDataAdapter(strSelect, conn);
                dr.Fill(dt);
            }
            catch (Exception)
            {
                throw;
            }
            return dt;
        }
        /// <summary>
        /// 给指定DataSet添加一张DataTable
        /// </summary>
        public DataSet getDataSet(DataSet ds, string strSelect, string tableName)
        {
            try
            {
                SqlDataAdapter dr = new SqlDataAdapter(strSelect, conn);
                dr.Fill(ds, tableName);
            }
            catch (Exception)
            {
                throw;
            }
            return ds;
        }
        /// <summary>
        /// 执行查询语句返回一个阅读器
        /// </summary>
        public SqlDataReader reader(string strSelect)
        {
            SqlDataReader rdr = null;
            SqlCommand cmd = new SqlCommand(strSelect, conn);
            try
            {
                conn.Open();
                rdr = cmd.ExecuteReader(CommandBehavior.CloseConnection);
            }
            finally
            {
                conn.Close();
            }
            return rdr;
        }
        /// <summary>
        /// 分页存储过程
        /// </summary>
        /// <param name="skipNum">跳过的条数</param>
        /// <param name="showNum">显示的条数</param>
        /// <param name="strWhere">条件(至少一个条件)</param>
        /// <param name="strSort">排序(至少一个字段)</param>
        public DataTable execPaginationProcGetDatable(string PropName, string skipNum, string showNum, string strWhere, string strSort)
        {
            SqlCommand cmd = new SqlCommand();
            cmd.Connection = conn;
            cmd.CommandType = CommandType.StoredProcedure;//指定执行存储过程
            cmd.CommandText = PropName;//"P_Pagination_Assignment";//储存过程名字
            //创建数据库参数对象
            SqlParameter parTakeNum = new SqlParameter("@skipNum", skipNum);
            SqlParameter parShowNum = new SqlParameter("@showNum", showNum);
            SqlParameter parStrWhere = new SqlParameter("@strWhere", strWhere);
            SqlParameter parStrSort = new SqlParameter("@strSort", strSort);
            //将参数添加到cmd参数集合中
            cmd.Parameters.Add(parTakeNum);
            cmd.Parameters.Add(parShowNum);
            cmd.Parameters.Add(parStrWhere);
            cmd.Parameters.Add(parStrSort);
            SqlDataAdapter dr = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            dr.Fill(dt);
            return dt;
        }
        /// <summary>
        /// 批量执行sql
        /// </summary>
        /// <param name="sqls"></param>
        /// <returns></returns>
        public bool execTran(List<string> sqls)
        {
            conn.Open();
            SqlTransaction tran = conn.BeginTransaction();//先实例SqlTransaction类，使用这个事务使用的是con 这个连接，使用BeginTransaction这个方法来开始执行这个事务
            SqlCommand cmd = new SqlCommand();
            cmd.Connection = conn;
            cmd.Transaction = tran;
            try
            {
                foreach (var item in sqls)
                {
                    //在try{} 块里执行sqlcommand命令，
                    cmd.CommandText = item;
                    cmd.ExecuteNonQuery();
                }
                tran.Commit();//如果sql命令都执行成功，则执行commit这个方法，执行这些操作
                conn.Close();
                return true;
            }
            catch
            {
                //Label1.Text = "添加失败";
                tran.Rollback();//如何执行不成功，发生异常，则执行rollback方法，回滚到事务操作开始之前；
                conn.Close();
                return false;
            }
        }
    }
}
