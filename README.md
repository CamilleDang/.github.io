# Camille's CS 180 Portfolio!

## Project 1: Prokudin-Gorskii :) 

*Follow along with the code here: https://github.com/CamilleDang/.github.io*

### Overview
This project is based on bringing color to the Prokudin-Gorskii photo collection. Sergei Mikhailovich Prokudin-Gorskii, a man well-ahead of his time, was convinced that there would eventually be colored photos. In the early 1900s, he took many pictures, recording three exposures of every scene he took onto a glass plate using a red, green, and blue filter. He never got to piece them together to make a colored image, but we can now! So cool!

### Example of the input image from the Prokudin-Gorskii photo collection:

<img height="500" alt="cathedral jpg" src="cathedral.jpg">

### Naive Approach
I first started by taking each image (which has the 'red', 'green', and 'blue' filtered images stacked upon each other, as shown above) and extracting the three separate images. Then, I created an align function that takes in two images, and returns the best x and y offset that minimizes a loss between the first image's pixels and the second image's pixels within an a given window. By exhaustively searching for image1's x and y offset that minimizes the loss, the function will find the closest similarity between the two images as possible. I used the Euclidean distance as my loss function and searched over window sizes of [-15, 15] to find the x and y offset that returned the lowest Euclidean distance between the pixels of the first and second image.

Once the offset is found, we adjust the image accordingly: I adjusted the image in the align_final function after finding the optimal offset.
I effectively used this method for all three jpg files), using the **blue** channel as the base for all three.

### Further Improvements:
After the naive implementation, I also implemented a crop function that takes in an image and a percentage, and returns the image after cropping that percentage of the image. I 
I then applied this function to each of the R, G, and B images, cropping each image by around 15% to get rid of borders that made channel alignment less accurate.
I also implemented the NCC align function and added the option to add various loss functions in the align function, but ended up preferring and using the Euclidean norm for the rest of my images.

### Examples:

| Images Without Alignment | Images With Alignment | 
|:-------------------------:|:-------------------------:|
|<img width="500" alt="cathedral w/o alignment" src="cathedralog.jpg"> Cathedral |  <img width="500" alt="cathedral w/ alignment" src="cathedralfit.jpg"> R: (3, 12), G: (2, 5)|
|<img width="500" alt="monastery w/o alignment" src="monasteryog.jpg"> Monastery |  <img width="500" alt="monastery w/ alignment" src="monasteryfit.jpg"> R: (2, 3), G: (2, -3)|
|<img width="500" alt="tobolsk w/o alignment" src="tobolskog.jpg"> Tobolsk |  <img width="500" alt="tobolsk w/ alignment" src="tobolskfit.jpg"> R: (3, 6), G: (2, 3)|

### Implementing the Image Pyramid

This naive align implementation is effective for smaller jpg files, such as the ones shown above, but is not efficient enough for larger images because the computation and iteration for the align function is far too expensive. Thus, I implemented a recursive image pyramid that downscales an image and base image by 2 until it reaches a certain smaller limit (which I initially set as a 512-pixel limit on either the width or the height), which is when I call my align function to find the optimal offset x and y for that downscaled image. After finding the optimal offset for that downscaled image, we recursively multiply by 2 for each corresponding layer of recursion until we find the optimal offset for the original large image without the computational inefficiency of the exhaustive window search on the larger image.  

### Further Improvements / Struggles 
At first, my was running at about 1.5 minutes, and thus to optimize, I tried recursing to a smaller base case (decreasing the 512-pixel width/height limit of the image to 128) and also slightly decreasing the pixel window to [-10, 10]. This brought my runtime to around 30 seconds for all .tif images. 

Additionally, I aligned all images with the **blue** channel, as I had done previously, which worked for all except for one: Emir. This is likely due to Emir's predominantly strong blue clothing, and the fact that I am using the actual R, G, B pixel values to minimize loss. To fix this, I aligned Emir's image with the **green** channel as the base, which aligned much better. This is shown at the bottom of the examples.

### Examples:

|  |  |
|:-------------------------:|:-------------------------:|
| <img width="600" alt="Church" src="churchfit.jpg">  Church, R: (-4, 58), G: (4, 25) | <img width="600" alt="Harvesters" src="harvestersfit.jpg"> Harvesters, R: (13, 124), G: (16, 59) |
|<img width="600" alt="Sculpture fit" src="sculpturefit.jpg"> Sculpture, R: (-27, 140), G: (-11, 33) |  <img width="600" alt="Lady" src="ladyfit.jpg"> Lady, R: (11, 112), G: (9, 49) |
|<img width="600" alt="Icon fit" src="iconfit.jpg"> Icon, R: (23, 89), G: (17, 40) |  <img width="600" alt="Melons" src="melons.jpg"> Melons, R: (13, 179), G: (10, 82) |
|<img width="600" alt="Train fit" src="trainnfit.jpg"> Train, R: (32, 87), G: (5, 42) |  <img width="600" alt="Onion Church fit" src="onionchurchfit.jpg"> Onion Church, R: (36, 108), G: (26, 51) |
|<img width="600" alt="Self Portrait Fit" src="selfportraitfit.jpg"> Self Portrait, R: (37, 176), G: (29, 78) |  <img width="600" alt="Three Generations fit" src="threegenerationsfit.jpg"> Three Generations, R: (11, 112), G: (14, 53) |
|<img width="600" alt="Emir Reg Alignment" src="emirfit.jpg"> Emir (*aligned on **blue** channel*), R: (-630, 153), G: (24, 49) |  <img width="600" alt="Emir Green" src="emirswitch.jpg"> Emir (*aligned on **green** channel*), R: (17, 57), B: (-24, -49) |

### Extra Examples

Three more extra examples (tif files: Lugano Grass, Lugano Ocean, and Lastochkino gni︠e︡zdo) from the Prokudin-Gorskii photo collection, using L2 euclidean norm. It turned out very pretty!

| | |  |
|:-------------------------:|:-------------------------:|:-------------------------:|
| <img width="500" alt="image 1 Alignment" src="eximage1fit.jpg"> Lugano Grass R: (38, 86), G: (13, 36) | <img width="500" alt="Emir Green" src="eximageah.jpg"> Lugano Ocean R: (-29, 92), B: (-16, 41) | <img width="500" alt="Emir Gradient Align" src="try.jpg"> Lastochkino gni︠e︡zdo R: (-8, 75), G: (-2, -2) |

### Bells and Whistles: Better Features - Gradients

For bells and whistles, I tried minimizing a different loss function. Instead of mapping by RGB similarity, which can sometimes be faulty (shown above by Emir), I wrote a function that finds the Euclidean distance between the gradients of image 1 and image 2.

This method effectively worked for all images in finding the best offsets, albeit being slightly slower than the RGB similarity method (taking around 2 minutes per image). However, typically, comparing pixel gradients tend to be more robust than simply comparing RGB similarities. This is because direct comparison of RGB values are purely based on color values and can be affected by specific lighting or color representation. On the other hand, comparing pixels gradients can capture the differences in direction and intensity across the image, which are better for comparing more structural qualities of an image as opposed to purely comparing based on color. A direct example of this is in the Emir image -- comparing by gradient finds the best structural similarity as opposed to comparing by color, which is skewed by Emir's very blue clothing.

| RBG Similarity (Blue Base) | RBG Similarity (Green Base) | Gradient Similarity (Blue Base) |
|:-------------------------:|:-------------------------:|:-------------------------:|
| <img width="500" alt="Emir Reg Alignment" src="emirfit.jpg"> R: (-630, 153), G: (24, 49) | <img width="500" alt="Emir Green" src="emirswitch.jpg"> R: (17, 57), B: (-24, -49) | <img width="500" alt="Emir Gradient Align" src="emirgradient.jpg"> R: (41, 105), G: (24, 49) |

### Reflection

This was so much fun!!! Yay!!! 🤩



