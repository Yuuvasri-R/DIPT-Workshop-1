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
import numpy as np
import math

# Read image
img = cv2.imread("img1.png")

if img is None:
    raise FileNotFoundError("Could not find fa.jpeg")

# Resize only for display if the image is very large
original = img.copy()
h, w = img.shape[:2]

display_scale = min(1.0, 1000 / w, 800 / h)
display_img = cv2.resize(
    img, None, fx=display_scale, fy=display_scale
)

points = []
window_name = "Click LEFT eye, then RIGHT eye"

def mouse_click(event, x, y, flags, param):
    global points

    if event == cv2.EVENT_LBUTTONDOWN and len(points) < 2:
        # Convert display coordinates to original image coordinates
        ox = int(x / display_scale)
        oy = int(y / display_scale)
        points.append((ox, oy))

        # Mark clicked point on displayed image
        cv2.circle(display_img, (x, y), 4, (0, 255, 0), -1)
        cv2.imshow(window_name, display_img)

        print("Selected point:", (ox, oy))

        if len(points) == 2:
            cv2.destroyWindow(window_name)

cv2.namedWindow(window_name)
cv2.setMouseCallback(window_name, mouse_click)

cv2.imshow(window_name, display_img)

while len(points) < 2:
    key = cv2.waitKey(20) & 0xFF
    if key == 27:  # ESC
        break

cv2.destroyAllWindows()

if len(points) != 2:
    raise ValueError("Please run the cell again and select both eyes.")

# Eye centers
left = points[0]
right = points[1]

# Calculate distance between eye centers
distance = math.dist(left, right)

# Sunglasses lens dimensions
lens_width = max(8, int(distance * 0.42))
lens_height = max(5, int(distance * 0.23))

overlay = original.copy()

# Draw lenses
for cx, cy in [left, right]:
    cv2.ellipse(
        overlay,
        (cx, cy),
        (lens_width, lens_height),
        0, 0, 360,
        (35, 35, 35),
        -1,
        cv2.LINE_AA
    )

    cv2.ellipse(
        overlay,
        (cx, cy),
        (lens_width, lens_height),
        0, 0, 360,
        (10, 10, 10),
        2,
        cv2.LINE_AA
    )

# Draw bridge
cv2.line(
    overlay,
    (left[0] + lens_width, left[1]),
    (right[0] - lens_width, right[1]),
    (10, 10, 10),
    3,
    cv2.LINE_AA
)

# Draw arms
cv2.line(
    overlay,
    (left[0] - lens_width, left[1]),
    (left[0] - 2 * lens_width, left[1] - 2),
    (10, 10, 10),
    2,
    cv2.LINE_AA
)

cv2.line(
    overlay,
    (right[0] + lens_width, right[1]),
    (right[0] + 2 * lens_width, right[1] - 2),
    (10, 10, 10),
    2,
    cv2.LINE_AA
)

# Blend with original
result = cv2.addWeighted(overlay, 0.88, original, 0.12, 0)

# Save result
cv2.imwrite("passport_with_sunglasses.png", result)

# Display final image in Jupyter
from matplotlib import pyplot as plt

plt.figure(figsize=(10, 6))
plt.imshow(cv2.cvtColor(result, cv2.COLOR_BGR2RGB))
plt.title("Sunglasses Added")
plt.axis("off")
plt.show()

print("Saved as passport_with_sunglasses.png")
```
