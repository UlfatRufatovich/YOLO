# YOLO
using Alturos.Yolo;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO;
using System.Drawing.Imaging;

namespace ai
{
    public partial class Form1 : Form
    {

        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            OpenFileDialog sec = new OpenFileDialog();
            if (sec.ShowDialog() == DialogResult.OK)
                pictureBox1.Image = Image.FromFile(sec.FileName);
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void button2_Click(object sender, EventArgs e)
        {
          
            {
                var configFilePath = @"path\to\yolov3.cfg";      
                var weightsFilePath = @"path\to\yolov3.weights";  
                var namesFilePath = @"path\to\coco.names";       
                int gpuDevice = 0;
                try
                {
                    using (var yoloWrapper = new YoloWrapper(configFilePath, weightsFilePath, namesFilePath, gpuDevice))
                    {
                        // Step 1: Save the pictureBox image to a temporary file
                        string tempImagePath = Path.Combine(Path.GetTempPath(), "tempImage.jpg");
                        pictureBox1.Image.Save(tempImagePath, ImageFormat.Jpeg);

                        // Step 2: Detect objects using the saved image file path
                        var items = yoloWrapper.Detect(tempImagePath);

                        // Step 3: Bind detected items to the data source
                        yoloTrackingItemBindingSource.DataSource = items;

                        // Optional: Delete the temp file after use
                        if (File.Exists(tempImagePath))
                        {
                            File.Delete(tempImagePath);
                        }
                    }
                }
                catch (Exception ex)
                {
                    MessageBox.Show("Error: " + ex.Message);
                }
            }
             
        }
    }
}
