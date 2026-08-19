# NAME: Nithila S
# REG.NO: 212224040224
# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## program
```
import cv2
import matplotlib.pyplot as plt

photo = cv2.imread("photo.jpeg")

glasses = cv2.imread("sunglass.jpg", cv2.IMREAD_UNCHANGED)

print("Photo shape:", photo.shape)
print("Glasses shape:", glasses.shape)
plt.figure(figsize=(6, 8))

plt.imshow(cv2.cvtColor(photo, cv2.COLOR_BGR2RGB))
plt.axis("off")

plt.show()
glasses_crop = glasses[260:600, :]
hsv = cv2.cvtColor(glasses_crop, cv2.COLOR_BGR2HSV)

mask = cv2.inRange(
    hsv,
    np.array([0, 25, 40]),
    np.array([179, 255, 255])
)
kernel = np.ones((3, 3), np.uint8)
mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
points = cv2.findNonZero(mask)

x, y, w, h = cv2.boundingRect(points)

glasses_crop = glasses_crop[y:y+h, x:x+w]
mask = mask[y:y+h, x:x+w]

new_width = 430
new_height = int(h * new_width / w)

glasses_resized = cv2.resize(
    glasses_crop,
    (new_width, new_height)
)

mask_resized = cv2.resize(
    mask,
    (new_width, new_height)
)
x1 = 355
y1 = 455

x2 = x1 + new_width
y2 = y1 + new_height
roi = photo[y1:y2, x1:x2]
alpha = mask_resized.astype(float) / 255.0
alpha = alpha[:, :, np.newaxis]
blended = (
    glasses_resized.astype(float) * alpha
    + roi.astype(float) * (1 - alpha)
)

blended = blended.astype(np.uint8)

output = photo.copy()
output[y1:y2, x1:x2] = blended
plt.figure(figsize=(12, 6))

plt.subplot(1, 2, 1)
plt.imshow(cv2.cvtColor(photo, cv2.COLOR_BGR2RGB))
plt.title("Original")
plt.axis("off")

plt.subplot(1, 2, 2)
plt.imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
plt.title("Sunglasses Automatically Positioned")
plt.axis("off")

plt.tight_layout()
plt.show()
```
## output:
<img width="686" height="871" alt="Screenshot 2026-08-19 193932" src="https://github.com/user-attachments/assets/5440b381-5166-4c94-a5d0-3c05e643e0c9" />

<img width="884" height="525" alt="Screenshot 2026-08-19 193953" src="https://github.com/user-attachments/assets/65c64b01-6d44-4923-954a-b027b2ac5bf3" />
