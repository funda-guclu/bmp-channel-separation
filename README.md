# Project Report: 24-Bit to 8-Bit BMP Image Channel Separation in C

## Project Approach and Design
The primary objective of this project was to develop a C program capable of reading a 24-bit 
color BMP image, separating it into its fundamental Red, Green, and Blue (RGB) channels, and 
saving each channel as a new, individual 8-bit BMP file. The core design philosophy was 
centered around modularity, encapsulation, and robust memory management. Instead of 
placing all logic within a single main function, the task was broken down into distinct, reusable 
functions, each with a single, clear responsibility.

## Key Functions and Their Roles

- *Provided by Instructor:*  
  - ImageRead()  
    Opens the specified BMP file in binary mode, reads the file and info headers to obtain image 
    dimensions and color depth, loads the optional color palette, and reads the raw pixel data into 
    a dynamically allocated IMAGE structure.

  - ImageWrite()  
    Saves an IMAGE structure to disk by writing the headers, optional palette, and pixel data in 
    the correct BMP format. Ensures that 8-bit images include their palette.

- *Provided Collaboratively in Class:*  
  - createPalette()  
    This function is a specialized utility that generates a 256-entry color palette required for 8-bit 
    grayscale images. It accepts a character ('r', 'g', or 'b') to determine the type of palette to create. 
    It then allocates memory for 256 palette entries and loops through them, creating a color gradient. 
    For example, if given the character 'r', it creates a palette that maps index 0 to black, index 255 
    to pure red, and all the values in between to corresponding shades of red, which allows the single 
    channel image to be visualized correctly.

- *Implemented by Student:*  
  - processAndSaveChannel()  
    This function is the core of the program's logic, designed to encapsulate the entire process of 
    separating one color channel and saving it. It creates a new temporary IMAGE structure and 
    configures its headers to match the 8-bit BMP format. It then calls createPalette() to generate 
    the appropriate color map for the channel ('r', 'g', or 'b'). Next, it allocates memory for the 
    new pixel data and loops through the original 24-bit image, extracting only the specific color data 
    (e.g., just the red values) and placing it into the new 8-bit data block. After calling ImageWrite() 
    to save the file, it responsibly frees all the temporary memory it allocated for the new image, 
    its palette, and its data, ensuring there are no memory leaks.

  - main()  
    The main() function acts as the high-level controller for the entire program. Its job is to 
    orchestrate the workflow by calling the other functions in the correct order. It starts by 
    calling ImageRead() to load the source fruit.bmp file and performs a check to ensure it is a 24-bit 
    image. It then calls processAndSaveChannel() three separate times, once for each color channel, 
    to create the "red8.bmp", "green8.bmp", and "blue8.bmp" files. Once all channels have been 
    processed and saved, it performs the final cleanup by freeing the memory that was allocated 
    for the original image, completing the program's execution.

## Used Image
- fruit.bmp  
  ![Original Image](https://raw.githubusercontent.com/funda-guclu/bmp-channel-separation/refs/heads/main/fruit.bmp)

## Produced Images
- red8.bmp  
  ![Red Channel](https://raw.githubusercontent.com/funda-guclu/bmp-channel-separation/refs/heads/main/red8.bmp)
- green8.bmp  
  ![Green Channel](https://raw.githubusercontent.com/funda-guclu/bmp-channel-separation/refs/heads/main/green8.bmp)
- blue8.bmp  
  ![Blue Channel](https://raw.githubusercontent.com/funda-guclu/bmp-channel-separation/refs/heads/main/blue8.bmp)

## 24-Bit to 8-Bit Conversion Process
In a 24-bit BMP image, each pixel is represented by 3 bytes (24 bits) containing the Blue, Green, 
and Red components respectively. Each of these components holds an intensity value between 0 
and 255. In contrast, an 8-bit BMP image represents each pixel with only a single byte, which may 
either store a grayscale intensity or serve as an index into a color palette.

In this project, the conversion was performed as follows:

1. *Selecting a single channel from the 24-bit data*  
   Only one color component (R, G, or B) was extracted from the original image.

2. *Forming the 8-bit pixel data*  
   The chosen channel’s value was directly stored as a single byte in the range 0–255.

3. *Adding a palette*  
   A custom 256-entry color palette was generated to allow proper visualization of the 
   single-channel image. For example, the red channel palette maps index 0 to black, index 
   255 to pure red, and intermediate values to shades of red.

4. *Saving the result*  
   The generated 8-bit data was saved to disk together with the BMP headers and the appropriate 
   color palette.

This approach reduces memory usage to one-third of the original and enables clear visual 
inspection of each color channel individually.

## Conclusion and Evaluation
This project successfully implemented the separation of a 24-bit BMP image into its individual 
red, green, and blue channels, converting each into an 8-bit BMP with an appropriate color 
palette. The conversion process reduced the file size while allowing clear visualization of each 
channel separately. The program’s modular structure improved code clarity and maintainability, 
and proper memory management ensured reliable execution without leaks. Overall, the project 
demonstrated practical skills in image processing, binary file handling, and dynamic memory 
allocation.
