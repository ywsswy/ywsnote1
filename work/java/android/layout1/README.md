<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"    
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:weightSum="3"  
    android:gravity="center_horizontal" >
    <!-- android:gravity="bottom|center_horizontal" > -->
    <Button
        android:id="@+id/button1"
        android:layout_width="0dp"  
        android:layout_height="wrap_content"  
        android:layout_weight="1"  
        android:text="left" />
		<DigitalClock 
		    android:id="@+id/AC"
       		android:gravity="center"
	
    <TableRow>
		<DigitalClock 
		    android:id="@+id/AC"
       		android:gravity="center"
		    android:layout_weight="3"/>
    </TableRow>
    <TableRow>
        <Button
            android:id="@+id/bu34"
            android:layout_height="wrap_content"
            android:text="Reset" 
            android:textColor="#ff0000"
            android:layout_weight="3"
            android:background="#ffffff"/>
    </TableRow>
    <TextView
       android:id="@+id/bu36"
       android:layout_height="wrap_content"
       android:gravity="center"
       android:text="Create By 石博鑫 and 十二班" 
       android:background="#ffffff"/>
</TableLayout>	    android:layout_weight="3"/>
