using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using My0422car;
namespace _0422car
{
    public partial class Form1 : Form
    {
        static int recon = 520;
        static int remov = 520;
        static bool recap = false;
        static int reser = 520;
        static My0422car.CarDll c1;// = new CarDll();
        public Form1()
        {
            InitializeComponent();
        }
        private void button1_Click(object sender, EventArgs e)
        {
            recon = c1.CarConnect();
            label1.Text = "con:" + recon.ToString();
        }
        private void button2_Click(object sender, EventArgs e)
        {
            remov = c1.move(1);
            label1.Text = "mov:" + remov.ToString();
        }
    }
}

