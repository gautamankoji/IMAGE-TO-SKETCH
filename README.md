# IMAGE-TO-SKETCH

This project converts an image from the web into a sketch-style image. The process involves several steps including loading the image, converting it to grayscale, inverting the colors, applying a Gaussian blur, and performing dodging to generate the final sketch effect.

## Prerequisites

Make sure you have Python and pip installed on your system.

## Installation

1. **Create and Activate a Virtual Environment (Optional but Recommended):**

   Create a virtual environment:

   ```bash
   python -m venv venv
   ```

   Activate the virtual environment:

   - On Windows:

     ```bash
     venv\Scripts\activate
     ```

   - On macOS/Linux:

     ```bash
     source venv/bin/activate
     ```

2. **Install Dependencies:**

   Install the required Python packages using `requirements.txt`:

   ```bash
   pip install -r requirements.txt
   ```

   If you don't have a `requirements.txt` file, you can create one with the following content:

   ```plaintext
   imageio
   requests
   matplotlib
   numpy
   scipy
   ```

## Usage

1. **Load the Image:**

   Load an image from a URL and display it:

   ```python
   import imageio
   import requests
   import IPython.display as dp

   img = "https://www.lovethisimages.com/wp-content/uploads/2018/04/sorry-images-download-1.jpg"
   dp.Image(requests.get(img).content)
   ```

2. **Read and Convert to Grayscale:**

   Read the image and convert it to grayscale:

   ```python
   import numpy as np

   def grayscaleimg(rgb): 
       return np.dot(rgb[...,:3], [0.299, 0.587, 0.114])

   gryscl_img = grayscaleimg(source_img)
   ```

3. **Invert the Image:**

   Invert the grayscale image:

   ```python
   inv_img = (255 - gryscl_img)
   ```

4. **Apply Gaussian Blur:**

   Apply a Gaussian blur to the inverted image:

   ```python
   import scipy.ndimage

   blurred_img = scipy.ndimage.filters.gaussian_filter(inv_img, sigma=5)
   ```

5. **Perform Dodging:**

   Blend the grayscale and blurred images to create a sketch effect:

   ```python
   def dodging(blur_img, gryscl_img):
       resultant_dodge = blur_img * 255 / (255 - gryscl_img) 
       resultant_dodge[resultant_dodge > 255] = 255
       resultant_dodge[gryscl_img == 255] = 255
       return resultant_dodge.astype('uint8')

   target_img = dodging(blurred_img, gryscl_img)
   ```

6. **Save the Final Sketch Image:**

   Save the final sketch image:

   ```python
   import matplotlib.pyplot as plt
   plt.imsave('target_image.png', target_img, cmap='gray', vmin=0, vmax=255)
   ```

## Output

The resulting image will be saved in the project directory as `target_image.png` and will have a sketch-like appearance.

## Notes

- The Gaussian filter's `sigma` parameter controls the amount of blur. Adjust as needed for different effects.
- Replace the image URL with one of your choice if you want to process a different image.
