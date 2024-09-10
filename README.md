# Camille's CS 180 Portfolio!

## Project 1: Prokudin-Gorskii :) 

### Overview
This project is based on bringing color to the Prokudin-Gorskii photo collection. Sergei Mikhailovich Prokudin-Gorskii, a man well-ahead of his time, was convinced that there would eventually be colored photos. In the early 1900s, he took many pictures, recording three exposures of every scene he took onto a glass plate using a red, green, and blue filter. He never got to piece them together to make a colored image, but we can now! So cool!

### Example of the input image from the Prokudin-Gorskii photo collection:

<img height="500" alt="cathedral jpg" src="cathedral.jpg">

### Naive Approach
I first started by taking each image (which has the 'red', 'green', and 'blue' filtered images stacked upon each other, as shown above) and extracting the three separate images. Then, I created an align function that takes in two images, and returns the best x and y offset that minimizes a loss between the first image's position according to the second image within an exhaustive search of a given window, finding the closest similarity between the two images as possible. I used the Euclidean distance as my loss function and searched over window sizes of [-15, 15] to find the x and y offset that returned the lowest Euclidean distance between the pixels of the first and second image.
Once the offset is found, we adjust the image accordingly.

I effectively used this method for all three jpg files, using the blue channel as the base image.

### Further Improvements:
After the naive implementation, I also implemented a crop function that takes in an image and a percentage, and returns the image after cropping that percentage of the image. I 
I then applied this function to each of the R, G, and B images, cropping each image by around 15% to get rid of borders that made channel alignment less accurate.
I also implemented the NCC align function and added the option to add various loss functions in the align function, but ended up preferring and using the Euclidean norm for the rest of my images.

### Examples:

| Images Without Alignment | Images With Alignment | 
|:-------------------------:|:-------------------------:|
|<img width="500" alt="cathedral w/o alignment" src="cathedralfit.jpg"> Cathedral|  <img width="500" alt="cathedral w/ alignment" src="cathedralfit.jpg"> R: (3, 12), G: (2, 5)|
|<img width="500" alt="monastery w/o alignment" src="cathedralfit.jpg"> Monastery |  <img width="500" alt="monastery w/ alignment" src="monasteryfit.jpg"> R: (2, 3), G: (2, -3)|
|<img width="500" alt="tobolsk w/o alignment" src="cathedralfit.jpg"> Tobolsk |  <img width="500" alt="tobolsk w/ alignment" src="tobolskfit.jpg"> R: (3, 6), G: (2, 3)|

### Implementing the Image Pyramid
This does not work for larger images because the computation and iteration for the align function is far too expensive with larger pixels. Thus, I implemented a recursive image pyramid that downscales an image and base image by 2 until it reaches a certain smaller limit (which I initially set as a 512-pixel limit on either the width or the height), which is when I call my align function to receive the optimal offset x and y. 

### Further Improvements
My code was running at about 1.5 minutes, and thus to optimize, I triedrecursing to a smaller base case and also changing the pixel window to smaller

### Examples:

|  |  |
|:-------------------------:|:-------------------------:|
| <img width="600" alt="Church" src="churchfit.jpg">  Church, R: (-4, 58), G: (4, 25) | <img width="600" alt="Harvesters" src="harvestersfit.jpg"> Harvesters, R: (13, 124), G: (16, 59) |
|<img width="600" alt="Sculpture fit" src="sculpturefit.jpg"> Sculpture, R: (-27, 140), G: (-11, 33) |  <img width="600" alt="Lady" src="ladyfit.jpg"> Lady, R: (11, 112), G: (9, 49) |
|<img width="600" alt="Icon fit" src="iconfit.jpg"> Icon, R: (23, 89), G: (17, 40) |  <img width="600" alt="Melons" src="melons.jpg"> Melons, R: (13, 179), G: (10, 82) |
|<img width="600" alt="Train fit" src="trainnfit.jpg"> Train, R: (32, 87), G: (5, 42) |  <img width="600" alt="Onion Church fit" src="onionchurchfit.jpg"> Onion Church, R: (36, 108), G: (26, 51) |
|<img width="600" alt="Self Portrait Fit" src="selfportraitfit.jpg"> Self Portrait, R: (37, 176), G: (29, 78) |  <img width="600" alt="Three Generations fit" src="threegenerationsfit.jpg"> Three Generations, R: (11, 112), G: (14, 53) |
|<img width="600" alt="Emir Reg Alignment" src="emirfit.jpg"> Emir (*aligned on **blue** channel*), R: (-630, 153), G: (24, 49) |  <img width="600" alt="Emir Green" src="emirswitch.jpg"> Emir (*aligned on **green** channel*), R: (17, 57), B: (-24, -49) |

### Bells and Whistles: Better Features - Gradients
I tried another 
|  |  |
|:-------------------------:|:-------------------------:|
| <img width="600" alt="Church" src="churchfit.jpg">  Church, R: (-4, 58), G: (4, 25) | <img width="600" alt="Harvesters" src="harvestersfit.jpg"> Harvesters, R: (13, 124), G: (16, 59) |


emir gradient offsets: 
red: 41, 105
green: 24, 49

### Bells and Whistles: Automatic Cropping



