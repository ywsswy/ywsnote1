using System;
using System.Collections;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using MySql.Data;
using MySql.Data.MySqlClient;
public class Mydb
{
    private MySqlConnection conn;
    //"Database='hhh';Data Source='123.206.27.78';User Id='root';Password='';charset='gbk';pooling=true";
    public Mydb(string strConn)
    {
        conn = new MySqlConnection(strConn);
    }
    /// <summary>
    /// 执行增删改语句返回受影响的行数
    /// </summary>
    public int NonQuery(string strSql)
    {
        int count = 0;
        MySqlCommand cmd = new MySqlCommand(strSql, conn);
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
    /// <summary>
    /// 执行查询语句并返回第一行第一列
    /// </summary>
    public object Scalar(string strSelect)
    {
        object obj = null;
        MySqlCommand cmd = new MySqlCommand(strSelect, conn);
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
            MySqlDataAdapter dr = new MySqlDataAdapter(strSelect, conn);
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
            MySqlDataAdapter dr = new MySqlDataAdapter(strSelect, conn);
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
    public MySqlDataReader reader(string strSelect)
    {
        MySqlDataReader rdr = null;
        MySqlCommand cmd = new MySqlCommand(strSelect, conn);
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
        MySqlCommand cmd = new MySqlCommand();
        cmd.Connection = conn;
        cmd.CommandType = CommandType.StoredProcedure;//指定执行存储过程
        cmd.CommandText = PropName;//"P_Pagination_Assignment";//储存过程名字
        //创建数据库参数对象
        MySqlParameter parTakeNum = new MySqlParameter("@skipNum", skipNum);
        MySqlParameter parShowNum = new MySqlParameter("@showNum", showNum);
        MySqlParameter parStrWhere = new MySqlParameter("@strWhere", strWhere);
        MySqlParameter parStrSort = new MySqlParameter("@strSort", strSort);
        //将参数添加到cmd参数集合中
        cmd.Parameters.Add(parTakeNum);
        cmd.Parameters.Add(parShowNum);
        cmd.Parameters.Add(parStrWhere);
        cmd.Parameters.Add(parStrSort);
        MySqlDataAdapter dr = new MySqlDataAdapter(cmd);
        DataTable dt = new DataTable();
        dr.Fill(dt);
        return dt;
    }
    /*
    /// <summary>
    /// 批量执行sql
    /// </summary>
    /// <param name="sqls"></param>
    /// <returns></returns>
    public bool execTran(List<string> sqls)
    {
        conn.Open();
        MySqlTransaction tran = conn.BeginTransaction();//先实例OleDbTransaction类，使用这个事务使用的是con 这个连接，使用BeginTransaction这个方法来开始执行这个事务
        MySqlCommand cmd = new MySqlCommand();
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
     */
}

