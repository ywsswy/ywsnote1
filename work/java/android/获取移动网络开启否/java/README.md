///////////////////////////////////////////
package com.yws.netopen;
import java.lang.reflect.Method;
import android.app.Activity;
import android.content.Context;
import android.net.ConnectivityManager;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.View.OnClickListener;
import android.view.Window;
import android.widget.Button;
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
		requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_main);
		final Button button1 = (Button) findViewById(R.id.button_1);
		button1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				if(getMobileDataState(MainActivity.this, null)){
					button1.setText("hh");
				}
				else{
					button1.setText("ww");
				}
			}
		});
	}
    
    
    /** 
     * 返回手机移动数据的状态 
     * 
     * @param pContext 
     * @param arg 
     *            默认填null 
     * @return true 连接 false 未连接 
     */  
    public static boolean getMobileDataState(Context pContext, Object[] arg) {  
      
        try {  
      
            ConnectivityManager mConnectivityManager = (ConnectivityManager) pContext.getSystemService(Context.CONNECTIVITY_SERVICE);  
      
            Class ownerClass = mConnectivityManager.getClass();  
      
            Class[] argsClass = null;  
            if (arg != null) {  
                argsClass = new Class[1];  
                argsClass[0] = arg.getClass();  
            }  
      
            Method method = ownerClass.getMethod("getMobileDataEnabled", argsClass);  
      
            Boolean isOpen = (Boolean) method.invoke(mConnectivityManager, arg);  
      
            return isOpen;  
      
        } catch (Exception e) {  
            // TODO: handle exception  
      
            System.out.println("得到移动数据状态出错");  
            return false;  
        }  
      
    }
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
        if (id == R.id.action_settings) {
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
}

