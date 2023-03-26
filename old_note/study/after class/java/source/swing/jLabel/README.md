import javax.swing.*;
public class YForm extends JFrame {
	public YForm(int width,int height){
		super();
		this.getContentPane().setLayout(null);
		this.setSize(300, 200);
		this.setVisible(true);
	}
	public static void main(String[] args) {
		YForm mf1 = new YForm(300,200);
		mf1.setTitle("主窗口");
		JLabel log1 = new JLabel();
		log1.setText("皮力升是小猪！");
	    log1.setBounds(100,20, 100, 100);//x,y,width,heitht
		mf1.add(log1);
	}
}

