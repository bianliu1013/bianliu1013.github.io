---
layout: post
title: How to draw opencv IplImage frame
date: 2015-09-21 11:23:32
tag: [c++,opencv,qt]
categories: c++
---

# 自己用过的方法有3：
## 1. 用CvvImage， 一个有名的工具类cvvImage.h &cvvImage.cpp
  请自行下载。
  用法：
  
```C
virtual void  DrawToHDC( HDC hDCDst, RECT *pDstRect );
```
缺点：某些机器上显示不出来，底层调用StretchDIBits函数都是成功，但是就是看不见视频。 所以有了方法2

## 2. gdiplus

```C
IplImage* m_CurrCvvImage; 

Gdiplus::Bitmap bitmap(m_CurrCvvImage->width, m_CurrCvvImage->height, m_CurrCvvImage->widthStep, 
        PixelFormat24bppRGB, (BYTE*)m_CurrCvvImage->imageData);
Gdiplus::Graphics graphics(hdc);
graphics.DrawImage(&bitmap, 0, 0);

// only used color image, you should refine it if apply to gray
```
缺点：万恶的闪烁。Can fixed it by re-paint all stuffs in Timer function instead of paint event method. not a good way, but...


## 3. qt项目
```C
// found on:　blog.sina.com.cn/s/blog_77ed43e301018wg8.html
#include <QVector>
#include <QRgb>
QImage IplImage2QImage(const IplImage *iplImage)
{
    int height = iplImage->height;
    int width = iplImage->width;

    if  (iplImage->depth == IPL_DEPTH_8U && iplImage->nChannels == 3)
    {
        const uchar *qImageBuffer = (const uchar*)iplImage->imageData;
        QImage img(qImageBuffer, width, height, QImage::Format_RGB888);
        return img.rgbSwapped();
    } else if  (iplImage->depth == IPL_DEPTH_8U && iplImage->nChannels == 1){
        const uchar *qImageBuffer = (const uchar*)iplImage->imageData;
        QImage img(qImageBuffer, width, height, QImage::Format_Indexed8);

        QVector<QRgb> colorTable;
        for (int i = 0; i < 256; i++){
            colorTable.push_back(qRgb(i, i, i));
        }
        img.setColorTable(colorTable);
        return img;
    }else{
        // qWarning() << "Image cannot be converted.";
        return QImage();
    }
}

// copied from _moc file, you should add this in your ui form.
// QGraphicsView *liveVideo; 

// set stylesheet in init
ui->liveVideo->setStyleSheet( "background: transparent;border:0px" );

// override paint event.
void MainDialog ::paintEvent( QPaintEvent *event )
{
    QPainter painter;
    painter. begin(this );
    painter. setRenderHint(QPainter ::Antialiasing);

    static HDC dc =  ui-> liveVideo->getDC ();
    int width = ui ->liveVideo-> size().width ();
    int height = ui ->liveVideo-> size().height ();
    player_-> DrawVideoFrame(painter , dc, 0, 0, width, height );
    update();
}

// add below line to your paint method:
// in player_->DrawVideoFrame function
painter.drawImage (QPoint( x, y ), IplImage2QImage( image));
```