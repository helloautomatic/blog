## Qt开发过程中产生的笔记

```
// 通常需要将图片放入到一个label中，然后再将label放入到其它控件中，进行显示
 QLabel *imageLabel = new QLabel;
 QImage image("happyguy.png");
 imageLabel->setPixmap(QPixmap::fromImage(image));

 scrollArea = new QScrollArea;
 scrollArea->setBackgroundRole(QPalette::Dark);
 scrollArea->setWidget(imageLabel);

```
