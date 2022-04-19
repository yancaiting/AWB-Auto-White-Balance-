# AWB-Auto-White-Balance-
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/video.hpp>
#include <fstream>
#include <iostream>
#include <stdio.h>
#include <stdlib.h>
#include <ctime>
using namespace cv;
using namespace std;

vector<Mat>readimages(string img);

int main() {
	Mat images = imread("C:\\picture\\dataset\\subset\\000084.bmp");
	Mat AWBimages;
	imshow("原始圖像", images);
	vector<Mat> imageRGB;

	int count = 0;
	int countt = 0;
	int zero = 0;
	int one = 0;
	int two = 0;
	int zeroo = 0;
	int onee = 0;
	int twoo = 0;
	double ch0, ch1, ch2, ch00, ch11, ch22;
	double  avg_ch_0, avg_ch_1, avg_ch_2,avg_ch0, avg_ch1, avg_ch2, avg_ch00, avg_ch11, avg_ch22;
	for (int i = 489;i < 556;i++) 
	{
		for (int j = 410;j < 643;j++)
		{	
			ch0 = images.at<Vec3b>(i, j)[0]; //B
			ch1 = images.at<Vec3b>(i, j)[1]; //G
			ch2 = images.at<Vec3b>(i, j)[2]; //R
			ch0 = zero + ch0;
			ch1 = one + ch1;
			ch2 = two + ch2;
			zero = ch0;
			one = ch1;
			two = ch2;
			count++;
			//cout << "CH0 = " << ch0<<endl;
			//cout << "CH1 = " << ch1<<endl;
		}
		for (int j = 643;j < 875;j++)
		{
			ch00 = images.at<Vec3b>(i, j)[0]; //B
			ch11 = images.at<Vec3b>(i, j)[1]; //G
			ch22 = images.at<Vec3b>(i, j)[2]; //R
			ch00 = zeroo + ch00;
			ch11 = onee + ch11;
			ch22 = twoo + ch22;
			zeroo = ch00;
			onee = ch11;
			twoo = ch22;
			countt++;
			//cout << "CH0 = " << ch0<<endl;
			//cout << "CH1 = " << ch1<<endl;
		}
	}
	//cout <<"count = " <<count;
	avg_ch_0 = ch0 / count;
	avg_ch_1 = ch1 / count;
	avg_ch_2 = ch2 / count;
	avg_ch00 = ch00 / countt;
	avg_ch11 = ch11 / countt;
	avg_ch22 = ch22 / countt;
	avg_ch0 = (avg_ch_0 + avg_ch00) / 2;
	avg_ch1 = (avg_ch_1 + avg_ch11) / 2;
	avg_ch2 = (avg_ch_2 + avg_ch22) / 2;
	cout << "白平衡前灰階RGB : " << endl;
	cout << "B = " << avg_ch0 << endl;
	cout << "G = " << avg_ch1 << endl;
	cout << "R = " << avg_ch2 << endl;
	//需要調整的RGB分量的增益
	double KR, KG, KB;
	KB = (avg_ch2 + avg_ch1 + avg_ch0) / (3 * avg_ch0);
	KG = (avg_ch2 + avg_ch1 + avg_ch0) / (3 * avg_ch1);
	KR = (avg_ch2 + avg_ch1 + avg_ch0) / (3 * avg_ch2);
	//cout << "kb = " << KB;
	//RGB三通道分離
	split(images, imageRGB);
 
	//調整RGB三個通道各自的值
	imageRGB[0] = imageRGB[0] * KB;
	imageRGB[1] = imageRGB[1] * KG;
	imageRGB[2] = imageRGB[2] * KR;

	//RGB三通道圖像合併
	merge(imageRGB, images);
	imshow("白平衡調整過", images);
	//////////////////////////////////////////////////////////////////////////////////////
	int coun_t = 0;
	int coun_tt = 0;
	int zer_o = 0;
	int on_e = 0;
	int tw_o = 0;
	int zer_oo = 0;
	int on_ee = 0;
	int tw_oo = 0;
	double ch_0, ch_1, ch_2, ch_00, ch_11, ch_22;
	double  avg_c_h_0, avg_c_h_1, avg_c_h_2, aavg_ch0, aavg_ch1, aavg_ch2, avg_ch000, avg_ch111, avg_ch222;
	 for (int i = 489;i < 556;i++) 
	{
		for (int j = 410;j < 643;j++)
		{
			ch_0 = images.at<Vec3b>(i, j)[0]; //B
			ch_1 = images.at<Vec3b>(i, j)[1]; //G
			ch_2 = images.at<Vec3b>(i, j)[2]; //R
			ch_0 = zer_o + ch_0;
			ch_1 = on_e + ch_1;
			ch_2 = tw_o + ch_2;
			zer_o = ch_0;
			on_e = ch_1;
			tw_o = ch_2;
			coun_t++;
		}
		for (int j = 643;j < 875;j++)
		{
			ch_00 = images.at<Vec3b>(i, j)[0]; //B
			ch_11 = images.at<Vec3b>(i, j)[1]; //G
			ch_22 = images.at<Vec3b>(i, j)[2]; //R
			ch_00 = zer_oo + ch_00;
			ch_11 = on_ee + ch_11;
			ch_22 = tw_oo + ch_22;
			zer_oo = ch_00;
			on_ee = ch_11;
			tw_oo = ch_22;
			coun_tt++;
		}
	}
	avg_c_h_0 = ch_0 / coun_t;
	avg_c_h_1 = ch_1 / coun_t;
	avg_c_h_2 = ch_2 / coun_t;
	avg_ch000 = ch_00 / coun_tt;
	avg_ch111 = ch_11 / coun_tt;
	avg_ch222 = ch_22 / coun_tt;
	aavg_ch0 = (avg_c_h_0 + avg_ch000) / 2;
	aavg_ch1 = (avg_c_h_1 + avg_ch111) / 2;
	aavg_ch2 = (avg_c_h_2 + avg_ch222) / 2;
	cout << "白平衡後灰階RGB : " << endl;
	cout << "B = " << aavg_ch0 << endl;
	cout << "G = " << aavg_ch1 << endl;
	cout << "R = " << aavg_ch2 << endl;
//////////////////////////////////////////////////////////////////////////////////////////


	waitKey(0);
}


