package yws;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.*;
import java.io.File;  
public class Main {
	static int g_font_default_height = 10;
	public static String g_getfile_name = null;
	public static YLabel log1;
	public static YLabel log2;
	public static YLabel log3;
	public static ImageIcon g_i1 = new ImageIcon("D:\\program files (x86)\\Users\\admin\\workspace\\YUI\\src\\1.png");
	public static YLabel g_l1;
	static int button_height = 50;
	static int button_width = 100;
	static int big_pic_width = 300;
	
	
	public static void main(String[] args) throws InterruptedException {
		YForm mf1 = new YForm(600,400,"主窗口");
		
		Container cp1 = mf1.getContentPane();
		Point cpp1 = cp1.getLocationOnScreen();
		Dimension cps1 = cp1.getSize();
		
		
		log1 = new YLabel(0, cps1.height-g_font_default_height, 100, g_font_default_height, "hh",mf1);
		log2 = new YLabel(100, cps1.height-g_font_default_height, 100, g_font_default_height, "xx",mf1);
		log3 = new YLabel(200, cps1.height-g_font_default_height, 100, g_font_default_height, "xx",mf1);
		YButton b1 = new YButton(0, 0, button_width, button_height, "打开二维码", mf1);
		YButton b2 = new YButton(b1.getX(), b1.getY()+button_height, button_width, button_height, "识别二维码", mf1);
		
	//	YLabel l2 = new YLabel(b1.getX()+b1.getWidth(),100, 100, 100,"是帅哥！",mf1);
		
		g_l1 = new YLabel(b1.getX()+button_width,0,big_pic_width,big_pic_width,"帅哥！",mf1);
		YFileChooser yf1 = new YFileChooser(b1);
		b2.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                System.out.println("hhh");
            }
        });
	    
		
		g_i1=new ImageIcon("D:\\1.jpg");
				//("D:\\program files (x86)\\Users\\admin\\workspace\\YUI\\src\\1.png");
		//g_i1 = new ImageIcon("D:\\program files (x86)\\Users\\admin\\workspace\\YUI\\src\\1.png");
		g_i1.setImage(g_i1.getImage().getScaledInstance(l1.getWidth(),l1.getHeight(),Image.SCALE_DEFAULT));
		l1.setIcon(g_i1);
		//l2.setIcon(i1);
		//log1.setText(String.valueOf(i1.getIconWidth()));
		
		log1.setText(String.valueOf("w:"+l1.getWidth()));
		log2.setText(String.valueOf("w:"+l1.getHeight()));
		
 
		
	}
}

