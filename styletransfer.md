    import functools
    import os

    from matplotlib import gridspec
    import matplotlib.pylab as plt
    import numpy as np
    import tensorflow as tf
    import tensorflow_hub as hub

    print("TF Version: ", tf.__version__)
    print("TF-Hub version: ", hub.__version__)
    print("Eager mode enabled: ", tf.executing_eagerly())
    print("GPU available: ", tf.test.is_gpu_available())

 TF Version:  2.4.1
 TF-Hub version:  0.11.0
 Eager mode enabled:  True
 GPU available:  False
 
   
    def crop_center(image):
    """Returns a cropped square image."""
    shape = image.shape
    new_shape = min(shape[1], shape[2])
    offset_y = max(shape[1] - shape[2], 0) // 2
    offset_x = max(shape[2] - shape[1], 0) // 2
    image = tf.image.crop_to_bounding_box(
    image, offset_y, offset_x, new_shape, new_shape)
    return image

    @functools.lru_cache(maxsize=None)
    def load_image(image_url, image_size=(256, 256), preserve_aspect_ratio=True):
    """Loads and preprocesses images."""
    # Cache image file locally.
    image_path = tf.keras.utils.get_file(os.path.basename(image_url)[-128:], image_url)
    # Load and convert to float32 numpy array, add batch dimension, and normalize to range [0, 1].
    img = plt.imread(image_path).astype(np.float32)[np.newaxis, ...]
    if img.max() > 1.0:
    img = img / 255.
    if len(img.shape) == 3:
    img = tf.stack([img, img, img], axis=-1)
    img = crop_center(img)
    img = tf.image.resize(img, image_size, preserve_aspect_ratio=True)
    return img

    def show_n(images, titles=('',)):
    n = len(images)
    image_sizes = [image.shape[1] for image in images]
    w = (image_sizes[0] * 6) // 320
    plt.figure(figsize=(w  * n, w))
    gs = gridspec.GridSpec(1, n, width_ratios=image_sizes)
    for i in range(n):
    plt.subplot(gs[i])
    plt.imshow(images[i][0], aspect='equal')
    plt.axis('off')
    plt.title(titles[i] if len(titles) > i else '')
    plt.show()
    
    
    content_image_url = 'https://freight.cargo.site/w/700/q/75/i/f5b40cad9f62b64336b97b464cafd4a5c73731d48c9f39101180ebce18f17fc7/a22222.jpg'  # @param      {type:"string"}
    style_image_url = 'https://freight.cargo.site/w/700/q/75/i/dcb8ee9b077b9939bbc4304c91affaf4b6317c5d82c8524ace40e8f532434c87/a22222222.jpg'  # @param {type:"string"}
    output_image_size = 484  # @param {type:"integer"}

    # The content image size can be arbitrary.
    content_img_size = (output_image_size, output_image_size)
    # The style prediction model was trained with image size 256 and it's the 
    # recommended image size for the style image (though, other sizes work as 
    # well but will lead to different results).
    style_img_size = (256, 256)  # Recommended to keep it at 256.

    content_image = load_image(content_image_url, content_img_size)
    style_image = load_image(style_image_url, style_img_size)
    style_image = tf.nn.avg_pool(style_image, ksize=[3,3], strides=[1,1], padding='SAME')
    show_n([content_image, style_image], ['Content image', 'Style image'])
    
    
<img width="1052" alt="10" src="https://user-images.githubusercontent.com/73337770/114380262-31e7d080-9b8a-11eb-8486-e3eb5ac75d53.png">

    hub_handle = 'https://tfhub.dev/google/magenta/arbitrary-image-stylization-v1-256/2'
    hub_module = hub.load(hub_handle)
    
    
    
    
 
