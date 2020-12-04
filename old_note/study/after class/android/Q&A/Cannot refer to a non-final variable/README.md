////////////////////////////////////////////
在使用Java局部内部类或者匿名内部类时，
若该类调用了所在方法的局部变量，
则该局部变量必须使用final关键字来修饰，
否则将会出现编译错误
“Cannot refer to a non-final variable * inside an inner class defined in a different method”
原因如下：在方法中定义的变量时局部变量，
当方法返回时，局部变量(str1,str2)对应的栈就被回收了，
当方法内部类去访问局部变量时就会发生错误。
当在变量前加上final时，变量就不在是真的变量了，成了常量，
这样在编译器进行编译时(即编译阶段)就会用变量的值来代替变量，
这样就不会出现变量清除后，再访问变量的错误。
比如下面的代码，必须有final
        final Button bb=new Button(getApplicationContext());
		WindowManager wm=(WindowManager)getApplicationContext().getSystemService("window");
		WindowManager.LayoutParams wmParams = new WindowManager.LayoutParams(); 
		wmParams.type=2002; //这里是关键，你也可以试试2003
		wmParams.format=1; /** *这里的flags也很关键 *代码实际是wmParams.flags |= FLAG_NOT_FOCUSABLE; *40的由来是wmParams的默认属性（32）+ FLAG_NOT_FOCUSABLE（8） */
		wmParams.flags=40;
		wmParams.width=100;
		wmParams.height=333;
		wmParams.x=111;
		wm.addView(bb, wmParams);//创建View
		bb.setText("hh");
		
		bb.setOnClickListener(new OnClickListener() { 
			@Override 
			public void onClick(View v) { 
				if(yi%2 == 1)
					bb.setText("?");
				else {
					bb.setText("!");
				}
				yi++;
			}
		});
