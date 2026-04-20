[FrontPage](../FrontPage.md)

# What's this?
- This product is "Auto Chasing Turtle".
- By autonomous control, this robot recognizes people's face and approaches to the detected human. The scene that it is working in real time can be seen by iPad. It is built by using Kinect as sensor and using Linaro kernel + Android + openFrameworks as application framework.
- [YouTube Video](http://www.youtube.com/watch?v=8EgfAk5RBVo)

![DSC_0012.jpg](AutoChasingTurtle_attachment_DSC_0012.jpg)
![DSC_0001.jpg](AutoChasingTurtle_attachment_DSC_0001.jpg)
![DSC_0006.jpg](AutoChasingTurtle_attachment_DSC_0006.jpg)


# The thing to prepare
- Hardware
    - [beagleboard-xM](http://beagleboard.org/hardware-xM)
    - [KONDO Animal](http://kondo-robot.com/product/kondo-animal.html)
    - [Kinect](http://www.xbox.com/en-US/kinect)
    - WiFiRouter
    - battery
        - 12V/1A
        - 10V/1A
        - 5V/3A
- Software
    - [ofxDroidKinect](http://www.noritsuna.com/archives/2011/01/openframeworks_kinect_android.html)
        - [Linaro Kernel](http://git.linaro.org/gitweb?p=people/jstultz/linux.git;a=summary)
        - [Android(Embedded Master)](https://github.com/OESF/Embedded-Master-ARM)
    - Robo controller
        - [OESF Future Systems WG](http://fswg.oesf.biz/)

## Reference data
- Please setup this environment.
    - [ofxDroidKinect](http://www.noritsuna.com/archives/2011/01/openframeworks_kinect_android.html)

## download
- This source code
[AutoChasingTurtle.zip](AutoChasingTurtle_attachment_AutoChasingTurtle.zip)
- Original Customized Linaro
[Linaro-kernel_android_1104.tar.gz](AutoChasingTurtle_attachment_Linaro-kernel_android_1104.tar.gz)
    - How to build
```
install Ubuntu10.10 later
sudo apt-get install gcc-arm-linux-gnueabi
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- uImage
```

### license
- GPL2.0

## slide
- [on SlideShare](http://www.slideshare.net/noritsuna/auto-chasing-turtle)
- download pptx:
    - [AutoChasingTurtle.pptx](AutoChasingTurtle_attachment_AutoChasingTurtle.pptx)

## Speech&Award
### [Linaro Developer Summit 11/05](https://wiki.linaro.org/Events/2011-05-LDS)
- We speech.

### [Laval Virtual 2011](http://www.laval-virtual.org/#Awards)
- We are nominated.

### [Campus Party 2011 Columbia](http://www.campus-party.com.co/2011/contenidos_estelares.html#KinectTurtle)
- We speech.


# Detail explanation for source code
## Hardware
- Connect all hardwares.
1. battery
1. [beagleboard-xM](http://beagleboard.org/hardware-xM)
1. [Kinect](http://www.xbox.com/en-US/kinect)
1. WiFiRouter


## Software
### What doing?
1. Take the RGB camera's image
    1. Save this image as jpeg
1. Try to recognize face
    1. If fail, search random.
1. Calculate the course which it should follow.
    1. Move kinect's Angle
1. Calculate the distance to target


### Detect Face
- Take the RGB camera's image
    - Take from Kinect's RGB camera by ofxDroidKinect.
        - testApp.cpp : 36 line
```
void testApp::draw() {
   kinect.draw(0, 0, 480, 320);
   flame++;
   if (flame > 5) {
        flame = 0;
        colorImg->setFromPixels(kinect.getPixels(), kinect.width, kinect.height);
   }
}
```

- Save this image as jpeg
    - Android's FaceDetector class can't detect by the Kinect's Bitmap format. Therefore change format Bitmap to JPEG.
        - OFActivity.java : 53-72 line
```
Bitmap bitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);
byte[] pixels = OFAndroid.getImgPixels();
```
 
```
if (pixels != null) {
    for (int i=0;i<w;i++) {  
        for (int j=0;j<h;j++){
            int index = (j * w + i) * 3;
            bitmap.setPixel(i, j, Color.rgb(pixels[index++], pixels[index++], pixels[index]));
        }
   }
```
    
```
    try {
       // save adcard 
        FileOutputStream fos = new FileOutputStream("/sdcard/screenshot.jpg");
        bitmap.compress(Bitmap.CompressFormat.JPEG, 100, fos);
        fos.flush();
        fos.close();
    } catch (Exception e) {
        // check exception
    }
```

- Try to recognize face
    - Use Android's FaceDetector class
        - OFActivity.java : 77-80 line
```
    FaceDetector.Face[] faces = new FaceDetector.Face[1];
    if (bitmap != null) {
        FaceDetector detector = new FaceDetector(w, h, faces.length);
        int numFaces = detector.findFaces(bitmap, faces);
```

- If fail, search random.
    - 
        - OFActivity.java : 138-144 line
```
} else {
    /*
     * search turn
     */ 
    Random random = new Random();
    int res = random.nextInt(3);
    if (res == 0)			DroidBot.getInstance().turnRight2();
    else if (res == 1)		DroidBot.getInstance().turnRight4();
    else if (res == 2)		DroidBot.getInstance().turnLeft2();
    else if (res == 3)		DroidBot.getInstance().turnLeft4();
}
```

### Calculate Course & Destance
- Calculate the course which it should follow.
    - RGB image is divided into 4 pieces at a width direction. And the course is diceded by Face position.
        - OFActivity.java : 103-109 line
```
if (pointX > 0 && pointX < w/4) {
    DroidBot.getInstance().turnRight2();    // right position
} else if (pointX >= w/4 && pointX <= 3*w/4) {
    ;                                       // center position
} else if (pointX > 3*w/4 && pointX <= w) {
    DroidBot.getInstance().turnLeft2();     // left position
}
```

- Move kinect's Angle
    - The maximum kinect's Angle is "30". And the kinect's Angle is diceded on the basis of it by Face position.
        - OFActivity.java : 117-119 line
```
int angle = 30 - pointY*30/h;
if (angle > 0 && angle <= 30)
    OFAndroid.setAngle(angle);
```

- Calculate the distance to target
    - Get from Kinect's Z(depth) camera by ofxDroidKinect.
    - The kinect's Angle is diceded on the basis of it by Face position. 
        - OFActivity.java : 127-132 line
```
int dist = OFAndroid.getDistance(pointX, pointY);
if (dist < 100)                     DroidBot.getInstance().walkBack4();
else if (dist >= 100 && dist < 150) DroidBot.getInstance().walkToward4();
else if (dist >= 150 && dist < 200) DroidBot.getInstance().walkToward8();
else if (dist >= 200 && dist < 300) DroidBot.getInstance().walkToward16();
else if (dist >= 300)               DroidBot.getInstance().walkToward32();
```


### Control Robot
- KONDO Animal is controled by serial.
- KONDO Animal uses RCB-3 control unit. and send to control command by serial.
    - Please look "kameserial.c & kameserial.h".
        - [RCB-3/RCB-3J Commands References](http://www.robotshop.com/content/PDF/rcb3commandref3-01150e.pdf)


### Connect iPad
- iPad viewer is VNC client.
    - Using [Android VNC Server](http://code.google.com/p/android-vnc-server/).
        - init.rc
 
```
service vncserver /data/androidvncserver -k /dev/input/event0 -t /dev/input/event0
```

# Contact us
    - info @ siprop.org 


# At the last
- If this is helpful to you, Please donate for "2011 Japanese Earthquake and Tsunami".
- We are Japanese community team. We pray Japan revives.


### Information
- [CrisisWiki](http://crisiswiki.org/2011_Sendai_Japan_Earthquake_and_Tsunami)

### Donation
- [2011 Japanese Earthquake and Tsunami by Google](http://www.google.co.jp/intl/en/crisisresponse/japanquake2011.html)
- [American Red Cross: Japan Earthquake and Pacific Tsunami by Amazon](http://www.amazon.com/b/ref=amb_link_355543322_2?ie=UTF8&node=2673660011&pf_rd_m=ATVPDKIKX0DER&pf_rd_s=right-csm-1&pf_rd_r=1S3HDB87J2WENR9ZWV1J&pf_rd_t=101&pf_rd_p=1290864082&pf_rd_i=507846)
- [2011 Japan Earthquake&   Tsunami Relief by eBay](http://donations.ebay.com/charity_event_61.html)


### Contact us
- info
    - info atmark siprop.org
- Noritsuna
    - noritsuna atmark siprop.org
