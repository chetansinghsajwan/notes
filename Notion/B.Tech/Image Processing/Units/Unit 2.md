## Syllabus
**Image Processing Low Level Vision:** Image Enhancement in Spatial Domain, Image Enhancement in Frequency Domain, Edge Detection, Image Restoration, Color Image Processing, Wavelet Transform, Image Compression, Morphological Image Processing, Color Image Processing, Stereo Vision, Motion Analysis, Local and Image Features, Visual Saliency.
## Image Enhancement
Image enhancement refers to the process of highlighting certain information of an image, as well as weakening or removing any unnecessary information according to specific needs. For example, eliminating noise, revealing blurred details, and adjusting levels to highlight features of an image.
Image enhancement techniques can be divided into two broad categories:
1. Spatial domain
2. Frequency domain
## **Spatial Domain**
An image can be represented in the form of a 2D matrix where each element of the matrix represents pixel intensity. This state of 2D matrices that depict the intensity distribution of an image is called Spatial Domain. It can be represented as shown below-
For the RGB image, the spatial domain is represented as a 3D vector of 2D matrices. Each 2D matrix contains the intensities for a single color as shown in the image.
[![](https://miro.medium.com/v2/resize:fit:875/1*e81bkQrP7hmTwItdkaNlhw.png)](https://miro.medium.com/v2/resize:fit:875/1*e81bkQrP7hmTwItdkaNlhw.png)
[![](https://miro.medium.com/v2/resize:fit:875/1*ROX-sXhtmAAAJpNm3opj5w.png)](https://miro.medium.com/v2/resize:fit:875/1*ROX-sXhtmAAAJpNm3opj5w.png)
  
## **Frequency Domain**
In frequency-domain methods are based on Fourier Transform of an image. Roughly, the term frequency in an image tells about the rate of change of pixel values.
Below diagram depicts the conversion of image from spatial domain to frequency domain using Fourier Transformation-
[![](https://miro.medium.com/v2/resize:fit:320/1*5B1W23mYACW7yUFthL7z_Q.png)](https://miro.medium.com/v2/resize:fit:320/1*5B1W23mYACW7yUFthL7z_Q.png)

> [!important]  
> Why we need a domain other than spatial domain ?  
Many times, image processing tasks are best performed in a domain other than the spatial domain. Moreover, it is easy to detect some features in a particular domain, i.e., a new information can be obtained in other domains.