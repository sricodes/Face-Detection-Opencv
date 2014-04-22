/** This code is for better understanding of cascade harr training method of face detection **/
 
#include <time.h>
#include <stdio.h>
#include <string.h>
#include "cv.h"
#include "highgui.h"
#include "cvaux.h"
int lastX=-1;
int lastY=-1;
IplImage* imgTracking;
int posX;
int posY;

/**           
**/


CvHaarClassifierCascade       *cascade;     // this will be loaded in the cvdetectobject blah blha 
CvMemStorage                  *storage;    // for storage 
char *face_cascade="haarcascade_frontalface_alt2.xml";   // name give to the data file to make the coding more clear and understanding 

FILE *ptr_file;

void detectFacialFeatures( IplImage *img ,IplImage *temp_img ,int img_no)
{
    char image[100],msg[100],temp_image[100];
    float m[6];
    double factor = 1;
    CvMat M = cvMat( 2, 3, CV_32F, m );
    int w = (img)->width;
    int h = (img)->height;
    CvSeq* faces;
    CvRect *r;

    m[0] = (float)(factor*cos(0.0));
    m[1] = (float)(factor*sin(0.0));
    m[2] = w*0.5f;
    m[3] = -m[1];
    m[4] = m[0];
    m[5] = h*0.5f;
    
    cvGetQuadrangleSubPix(img, temp_img, &M);
    CvMemStorage* storage=cvCreateMemStorage(0);
    cvClearMemStorage( storage );
    
    if( cascade )
        faces = cvHaarDetectObjects(img,cascade, storage, 1.2, 2, CV_HAAR_DO_CANNY_PRUNING, cvSize(20, 20));
    else
        printf("\nFrontal face cascade not loaded\n");

    printf("\n no of faces detected are %d",faces->total);
    

    /* for each face found, draw a red box */
    for(int i = 0 ; i < ( faces ? faces->total : 0 ) ; i++ )
    {        
        r = ( CvRect* )cvGetSeqElem( faces, i );
        cvRectangle( img,cvPoint( r->x, r->y ),cvPoint( r->x + r->width, r->y + r->height ),
                     CV_RGB( 255, 0, 0 ), 1, 8, 0 );    
            printf("\n face_x=%d face_y=%d wd=%d ht=%d",r->x,r->y,r->width,r->height);
            
	    fprintf(ptr_file,"\nface %d th is having face_x=%d face_y=%d wd=%d ht=%d",i+1,r->x,r->y,r->width,r->height);
            
    }    
      
	if(faces->total==1){
	  
	                 posX=r->x;
                        posY=r->y;
			 printf(" %d %d ",r->x,r->y);
			  lastX = posX;
		           lastY = posY; 
    
              if(lastX>=0 && lastY>=0 && posX>=0 && posY>=0){
                 cvLine(imgTracking, cvPoint(posX, posY), cvPoint(lastX, lastY), cvScalar(255,0,0), 4);
              //   lastX = posX;
		//  lastY = posY;             
		//  cvWaitKey(0);
	  
	              }
		  
		  cvNamedWindow("track",0);
	          cvShowImage("track",imgTracking);
	        
	  
	  
	}
        
        
        
        
        
        
        
          
          CvScalar c=cvScalar(double(255),double(255),double(255));
	      CvFont font;
          cvInitFont(&font, CV_FONT_HERSHEY_SIMPLEX, 0.5, 0.5);
          cvPutText(img, "FACE OF RAM ", cvPoint(r->x, r->y), &font, c);
	  
         
          cvNamedWindow("face_wind");
	  
         cvShowImage("face_wind",img);
	 
	 
   

	} 
  
  
int main(){
                ptr_file =fopen("output.txt", "w"); // file to open to write the x and y 

		if (!ptr_file)
			return 1;
			
         
		
		
  
  
  
  cascade = ( CvHaarClassifierCascade* )cvLoad( face_cascade, 0, 0, 0 );
  
  IplImage* a;    
  CvCapture* capture = cvCaptureFromCAM(-1);  
  IplImage *real;
  cvNamedWindow("C", CV_WINDOW_AUTOSIZE);
  
  real=cvQueryFrame(capture);
  
  imgTracking=cvCreateImage(cvGetSize(real),IPL_DEPTH_8U, 3);
  cvZero(imgTracking); //covert the image, 'imgTracking' to black

    do{
      real=cvQueryFrame(capture);
      cvLine(imgTracking, cvPoint(posX, posY), cvPoint(lastX, lastY), cvScalar(0,0,255), 4);
      cvAdd(real,imgTracking,real);
      
      cvShowImage("C", real);
      a=real;  
      detectFacialFeatures(a,a,1);
        
      if(cvWaitKey(33)==27)break;
	    
    }while(1);
  
       
              cvWaitKey(0);
          //    system("vlc"); 
	      
 fclose(ptr_file);
return 0 ;  
}
