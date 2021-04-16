# Centroid Extraction from EPIC dataset

Centroids depict many important information in study of C. Elegans. Centroids can help in understanding the structure of worm, 
track the location of different cells during embryogenesis and much more.

This tutorial explains one of the approaches to extract centroids by image manipulation. Feel free to experiment on the 
implementation with the colab notebook linked below.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ZUOC01kuNiI9BkfhJQpEjg2v9XVJUMqM?usp=sharing)

## Method

The approach is divided into five major steps:
- Preprocessing of image
- Generation of locally enhanced image
- Centroid extraction 
- Centroid refining 
- Combining fragmented nuclei

### Step 1: Preprocessing of Images 
Preprocessing step consists of supressing noise of the image and converting image to single channeled image. To suppress the noise gaussian (𝜎 = 0.35) and 
medain filter both of kernel size 3X3 is used.

![Orignal Image](https://user-images.githubusercontent.com/57054296/114911821-ff193300-9e3c-11eb-94b3-2407e39a61e6.png)
![filterred image](https://user-images.githubusercontent.com/57054296/114912052-2cfe7780-9e3d-11eb-8e76-4a8be3eedfcf.png)

The images above contains the orignal image on the left and the filterred image on the right.
As we can see we still have some noise after filtering which might not be desireable, we will see the effect of this noise in further steps.

For now we are good to proceed furthur.

### Step 2: Generating Locally Enhanced Images
This is a crucial step of our process. Here we will manipulate pixels of the image such that we have the center
pixel at higher intensity w.r.t. surrounding pixels. 

To perform this task first we need to calculate the possible size of nulei present in the image. With the following equations: 

![image](https://user-images.githubusercontent.com/57054296/114984067-f9f4cc00-9eae-11eb-8333-6667d72408ff.png)

Here d<sub>max</sub> stands for the maximum diameter (say 80). And 𝛿l stands for difference between two consecutive diameters considered.

In ls we would have a list of probable diameters. e.g. [4,8,12,16.....]

Now for each diameter in ls we would do the following:
- Take a fixed kernerl of size (dia+1, dia+1) as shown below
- Add padding of (dia/2, dia/2, dia/2, dia/2) to the image.
- Convolve the padded image with the fixed kernel

e.g. kernel for diameter 4

| 1 | 1 | 1 | 1 | 1 |
|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 1 |
| 1 | 1 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 1 |
| 1 | 1 | 1 | 1 | 1 |

The purpose of above method is to implement a sliding window over the image which adds the pixel intensity of 
all other pixels in the window to the center pixel. Hence, locally enhancing it for a fixed diameter range.

If the input image was of dimensions (H,W) the output image would be of dimension (H,W,Smax).
We have locally enhanced image w.r.t. every fixed diameter in ls stacked in a 3D array.

To get final locally enhanced image we would take the maximum over 3rd dimension, the output obtained is as follows:

![image](https://user-images.githubusercontent.com/57054296/114989859-7e4a4d80-9eb5-11eb-98d7-82a8c4b269de.png)

### Step 3: Extracting Centroid

To extract centroids from the locally enhanced image, we would select a candidate region of fixed size (say 4). Again we would implement sliding window 
technique using unfold operation for efficiency. And generate a region matrix with the help of following operation in each window:
- Replace the value of pixel with intensity higher than center pixel with 1 else 0.
- Take the sum of the window and assign it to the center pixel corresponding to that window in the image.

Now, on the Rmat(region matrix) we would consider a threshold(say 0.99) and filter out redundant centers. from the Rmat. 

We would obtain first set of cwentroids at the end of these steps as follows:

![image](https://user-images.githubusercontent.com/57054296/114991212-f82f0680-9eb6-11eb-83d9-f96263ae9d0e.png)

As we can see there are too many centroids which does not represent any nuclei. This is because of the fact that even after supressing noise
it is not completely gone, as a matter of fact that we are doing every operation using a candidate region or a neighbourhood. This noise 
acts as maxima in some candidates and we obtain it to be centroid. 

### Step 4: Refining the centroids

To refine out unnecessary centroids we would map each centroid obtained onto locally enhanced image and calculate the shape scores for each centroid as follows: 

![image](https://user-images.githubusercontent.com/57054296/114992470-6aecb180-9eb8-11eb-9feb-50ebc5b8a307.png)

![image](https://user-images.githubusercontent.com/57054296/114992492-73dd8300-9eb8-11eb-8178-49b619f4e4b3.png)

![image](https://user-images.githubusercontent.com/57054296/114992529-7b049100-9eb8-11eb-8831-81b227c66783.png)

![image](https://user-images.githubusercontent.com/57054296/114992556-81930880-9eb8-11eb-9e98-4ac63d914205.png)

Output of the algorithm would be a shape matrix with dimension similar to the orignal image consisting shape score of each centroids.

The shape scores of the center which truly represent nucleus will be higher than once generated due to noise, due to the fact that 
noise will have less no of intense pixels surrounding it than the true centers. 

With the help of shape score we will also filter out few centroid due to fragmented nuclie, but not all.

By taking a shape threshold slightly less than the maximum shape score we obtain the following output:

![image](https://user-images.githubusercontent.com/57054296/114993419-6d034000-9eb9-11eb-85b6-3ed1ebf87cb8.png)

### Step 5: Combining fragmented centers

There might be cases where one might get multiple centroids really close enough to each other for a single nucleus.
To deal with such conditions we can calculate euclidean distance between each pair of centroids and eliminate them with the help of
a minimum threshold distance.

### Final Output
![image](https://user-images.githubusercontent.com/57054296/114994421-60331c00-9eba-11eb-9cb3-fc1ba09c48f5.png)

# References

https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0035550
