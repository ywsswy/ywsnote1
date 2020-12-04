		ImageIcon i1=new ImageIcon("D:\\program files (x86)\\Users\\admin\\workspace\\YUI\\src\\1.png");
		
		i1.setImage(i1.getImage().getScaledInstance(300,200,Image.SCALE_DEFAULT));
		l1.setIcon(i1);
		//l2.setIcon(i1);
		//log1.setText(String.valueOf(i1.getIconWidth()));
		
		log1.setText(String.valueOf("w:"+i1.getIconWidth()));
		log2.setText(String.valueOf("w:"+i1.getIconHeight()));
//设置图片高200和宽300
