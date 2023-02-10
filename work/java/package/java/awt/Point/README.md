		Container contentPane = mf1.getContentPane();
		// 注意只有窗口显示后getLocationOnScreen才可以调用，否则出错
		Point contentPos = contentPane.getLocationOnScreen();// 在屏幕的坐标
		Dimension size = contentPane.getSize(); // 可视区域的大小
		log2.setText(String.valueOf(size.width));

