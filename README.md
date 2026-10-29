# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look
Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

Name: Yuuvasri R
Reg No: 212225230313
# Features:
Detects the face in an image.
Places a stylish sunglass overlay perfectly on the face.
Works seamlessly with individual passport-size photos.
Customizable for different sunglasses styles or photo types.
Technologies Used:
Python
OpenCV for image processing
Numpy for array manipulations
How to Use:
Clone this repository.
Add your passport-sized photo to the images folder.
Run the script to see your "cool" transformation!
Applications:
Learning basic image processing techniques.
Adding flair to your photos for fun.
Practicing computer vision workflows.
Feel free to fork, contribute, or customize this project for your creative needs!

# Program
```
import cv2
import matplotlib.pyplot as plt

#Read image

img = cv2.imread("fa.jpeg")

#Make a copy

overlay = img.copy()

#Glass positions

#Left glass moved slightly to the RIGHT

left = (170, 160)
right = (255, 160)

#Glass size

axes = (26, 15)

#Slight tilt

angle = 5

#Draw two lenses

for cx, cy in [left, right]:
    # Lens
    cv2.ellipse(
        overlay,
        (cx, cy),
        axes,
        angle,
        0,
        360,
        (35, 35, 35),
        -1
    )
    
    # Lens border

    cv2.ellipse(
        overlay,
        (cx, cy),
        axes,
        angle,
        0,
        360,
        (10, 10, 10),
        2
    )
    
#Bridge between the two glasses

cv2.line(
    overlay,
    (left[0] + 24, left[1]),
    (right[0] - 24, right[1]),
    (10, 10, 10),
    2
)

#Left arm

cv2.line(
    overlay,
    (left[0] - 25, left[1]),
    (left[0] - 45, left[1] - 4),
    (10, 10, 10),
    2
)

#Right arm

cv2.line(
    overlay,
    (right[0] + 25, right[1]),
    (right[0] + 45, right[1] - 4),
    (10, 10, 10),
    2
)

#Blend with original image

result = cv2.addWeighted(
    overlay,
    0.85,
    img,
    0.15,
    0
)

#Save result

cv2.imwrite(
    "passport_with_sunglasses.png",
    result
)

#Display TWO images

plt.figure(figsize=(10, 5))

#Original

plt.subplot(1, 2, 1)

plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))

plt.title("Original")

plt.axis("off")

#Sunglasses

plt.subplot(1, 2, 2)

plt.imshow(cv2.cvtColor(result, cv2.COLOR_BGR2RGB))

plt.title("Sunglasses")

plt.axis("off")

plt.tight_layout()

plt.show()

print("Sunglasses added successfully")
```
