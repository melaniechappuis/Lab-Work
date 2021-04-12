    from matplotlib import pyplot as plt

    import numpy as np

    import math

    myImage = np.zeros((600,600))
    
    PI = 3.14159
    width_frequency = 3.14159 / 100
    frequency = width_frequency * 200
    
    for i in range(600):
    for j in range(600):
        t = math.sin(i / frequency) * math.sin(j / frequency) + math.atan(j / frequency) * math.cos(i / frequency);
        myImage[i,j]=t
    
    PI = 3.14159 * 2
    segments = 600
    spacing = PI * 100 / segments
    size = 80
    centre = 130
    
    for i in range(340):
    for j in range(320):
        if abs(centre-i) < size and abs(centre-j) < size:
            myImage[i,j]=2
    
    PI = 3.14159 * 2
    segments = 800
    spacing = PI * 2 / segments
    size = 60
    centre = 300   

    for i in range(640):
    for j in range(620):
        if abs(centre-j) < size and abs(centre-i) < size:
            myImage[i,j]=2

    plt.imshow(myImage, interpolation="bilinear", clim=(0,1),cmap="gray")
    plt.show()

![wave](https://user-images.githubusercontent.com/73337770/114400200-a1b18780-9b99-11eb-9086-b0b82f9ab201.png)

    myImage = np.zeros((240,320))

    width_frequency = 3.14159/30

    frequency = width_frequency * 130

    for i in range(240):
    for j in range(320):
        t = math.sin(math.sqrt(i * j) * frequency)
        myImage[i,j]=t
        
    TWO_PI = 3.14159 * 2
    segments = 100
    spacing = TWO_PI / segments
    size = 30

    for i in range(segments):
    y = math.sin(spacing * i) * size
    x = math.cos(spacing * i) * size
    myImage[math.floor(x) + 90, math.floor(y) + 90] = 1000

    centre = 200
    size = 40

    for i in range(240):
    for j in range(620):
        x_dist = abs(centre-j)
        y_dist = abs(centre-i)
        dist = math.sqrt(x_dist * x_dist + y_dist * y_dist)
        if dist < size:
            myImage[i,j] = 1

    plt.imshow(myImage, interpolation="bilinear", clim=(0,1),cmap="gray")
    plt.show()
    
   ![bubbles](https://user-images.githubusercontent.com/73337770/114401904-408ab380-9b9b-11eb-85e4-2c3c02234a41.png)

