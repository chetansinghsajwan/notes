## Syllabus
**INTRODUCTION TO IMAGE PROCESSINGAND COMPUTER VISION:** Pixels, Intensity, Coordinate Conventions, Sampling and Quantization, Histogram Analysis, Videos, Image Processing Pipeline, Image Processing and Computer Vision Research Areas: Low-level, Mid-Level and High-Level Vision.  
Unit - I  
**INTRODUCTION TO MATLAB/ OCTAVE:** Basic Operations, Image / Video handling, Flow Control, Vectorization.  
10  
**INTRODUCTION TO PYTHON:** Basic Operations, Lists, Tuples, Strings, Dictionaries, Flow Control, numpy, Image/Video handling, OpenCV, PIL, Orange.
# What is Computer Vision?
Computer vision is a field of artificial intelligence and computer science that deals with enabling computers to interpret and analyze images and videos from the real world
Image processing is one of the methods that is used for computer vision along with other Machine learning techniques, CNN etc.
# What is Image Processing?
Image processing is the process of transforming an image into a digital form and performing certain operations to get some useful information from it.
## What is an Image?
An image is represented by its dimensions (height and width) based on the number of pixels.
For example, if the dimensions of an image are 500 x 400 (width x height), the total number of pixels in the image is 200000.
## What is a Pixel?
A **pixel**(or picture element) is the smallest item of information in an image. Pixels are arranged in a 2-dimensional grid, represented using squares. Each pixel is a sample of an original image, where more samples typically provide more-accurate representations of the original. The intensity of each pixel is variable; in color systems, each pixel has typically three or four components such as red, green, and blue, or cyan, magenta, yellow, and black.
The word _pixel_ is based on a contraction of _pix_ ("pictures") and _el_ (for "element").
## Pixel Intensity
Pixel Intensity is the value of the pixel. The intensity value for each pixel is a single value for a gray-level image, or three values for a color image.
## What is Image resolution?
The term _resolution_ is often used as a pixel count in digital imaging. When the pixel counts are referred to as resolution, the convention is to describe the _pixel resolution_ with the set of two numbers. The first number is the number of pixel columns (width) and the second is the number of pixel rows (height), for example as _640 by 480_. Another popular convention is to cite resolution as the total number of pixels in the image, typically given as number of megapixels, which can be calculated by multiplying pixel columns by pixel rows and dividing by one million. An image that is 2048 pixels in width and 1536 pixels in height has a total of 2048×1536 = 3,145,728 pixels or 3.1 megapixels. One could refer to it as 2048 by 1536 or a 3.1-megapixel image. Other conventions include describing pixels per length unit or pixels per area unit, such as pixels per inch or per square inch.
## **What is DPI / PPI?**
**DPI** refers to **dots per inch** when using an ink based printer. It is a measure of resolution or image quality. Typically, the higher the dpi count, the better the print quality. This term is still used when discussing digital image quality; however, this is not the correct term.
**PPI** describes the resolution, in pixels, of an image to be printed within a specified space. For instance, a 100x100-pixel image that is printed in a 1-inch square could be said to have 100 pixels per inch, regardless of the printer's DPI capability. Used in this way, the measurement is only meaningful when printing an image. Good quality photographs usually require 300 pixels per inch when printed.
## Dot
A **pixel** is not a **dot**. **Pixels** relate to digital images as seen on your monitor and captured in your camera. **Dots** relate to the droplets of ink your printer spits out of the print head and its ability to print those images in detail.
## Imaging
**Imaging** is the representation or reproduction of an object's form; especially a visual representation (i.e., the formation of an image).
## Analog Image
When we capture the image of an object, we use image sensors to sense the incoming light and form the image. Image sensors convert the incoming light from an object into electrical signals that can be stored and viewed later. These analog signals are continuous. The images are stored in an analog form. Thus, the image formed has continuous variation in the tone.
We cannot process analog images by a computer. Analog signals contain infinite points, and we need infinite memory to store them. We need to convert the analog images into digital images to store and process by a computer.
## Analog Image to Digital Image Conversion
An analog image is converted to a digital image by digitizing the analog signals. We apply sampling and quantization to the analog signals to convert them into digital form.
A digital image is formed by arranging pixels in rows and columns. Each pixel has a particular integral value. The computer process that integral value and show us that pixel, the arrangement of the pixels form the digital image.
We use sampling and quantization to change the continuous analog image into quantized integral values that will represent each pixel and ultimately form the digital image.
![[/Untitled 8.png|Untitled 8.png]]
## What is Image Sampling
Sampling is the process of converting an analog signal into discrete values. In layman's terms, we can say that sampling is the process of recording an analog signal at regular intervals of time. A sampling function is applied to the analog signal that results in the sampled signal.
We get a finite number of samples of an analog signal. The number of samples gives us the number of pixels. More samples will result in higher image quality of the digital image because of more pixels.
## What is Image quantization
After sampling the analog signal, we will apply quantization. Quantization digitizes the amplitude of the sampled signal. Quantization is done by rounding off the amplitude of each sample and then assigning a different value according to its amplitude. Each value will represent a different color tone.
# Difference between Sampling vs Quantization
- Sampling establishes the number of pixels in a digital image, whereas quantization establishes the color of each pixel.
- Sampling digitalizes an analog signal's x-axis, whereas quantization digitalizes its y-axis.
- During sampling, an analog signal's amplitude value is noted at predetermined intervals.  if we're talking about quantization, the amplitude values are rounded off, and the values are given.
- Sampling is carried out before quantization, whereas quantization is carried out following sampling.
  

> [!important]  
> Digital Image Processing and its applications – Buzztech