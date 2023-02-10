package yws;
import java.awt.Color;
import javax.swing.*;
//自建窗体类
public class YForm extends JFrame {
	public YForm(int width,int height){
		super();
		this.getContentPane().setLayout(null);
		this.setSize(width,height);
		this.getContentPane().setBackground(new Color(0x99FF66));
		this.setVisible(true);
	}
}
	//public static void main(String[] args) {
	//	YForm mf1 = new YForm(600,400);
	//	mf1.setTitle("主窗口");
