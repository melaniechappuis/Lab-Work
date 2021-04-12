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
    
    
    outputs = hub_module(tf.constant(content_image), tf.constant(style_image))
    stylized_image = outputs[0]
    
    
    show_n([content_image, style_image, stylized_image], titles=['Original content image', 'Style image', 'Stylized image'])
    
   
    <img width="1323" alt="111" src="https://user-images.githubusercontent.com/73337770/114380679-9e62cf80-9b8a-11eb-9e4a-928f48cb960b.png">
 
    content_urls = dict(
    tree_rome='https://freight.cargo.site/w/700/q/75/i/f5b40cad9f62b64336b97b464cafd4a5c73731d48c9f39101180ebce18f17fc7/a22222.jpg',
    mountains='https://freight.cargo.site/w/700/q/75/i/ee2abc0fef2209851307be33daa461c4e8bf9ee802d35ad945a0e7c187fef230/DSC_0039.jpg',
    julia='https://freight.cargo.site/w/3000/q/75/i/224f2a2fbb3364928a8c3162b0650426958c13e43bd98f8afa03e2e16eeb6d3e/julia.jpg',
    monprofil='https://freight.cargo.site/w/711/q/75/i/4dfdb07a13525a23ee0deb42040067b7ad09680cc12e5fa325201dc518ba95bb/fprofil.jpg'
    )
    style_urls = dict(
    poster_metaform='https://freight.cargo.site/w/1500/q/75/i/af645b984de76db3450cf5788634dac34a15c84d041e18fdb5fa9c392a2904d3/metaformposterfinal.jpg',
            voyager1='https://freight.cargo.site/w/1448/q/94/i/69e8fabcb84dd412eaa78dfd2448d73d7ef59ee40b30cfb3b67e38a69e05c24f/40406703_2326507840709274_8350644749710917632_o.jpeg',
    familytree='https://freight.cargo.site/w/2631/q/94/i/0321bc064d0fa1e2d771d258accc2d9ece499711af0d7df5c2795bea6acc5b67/treepainting.jpg',
    portugal='https://freight.cargo.site/w/2335/q/75/i/1bef452041855c0ef6aa5cea15f4aae44f904c1e0f23b95acb21c2f4f816bd3a/a2222.jpg',
    bubble='https://freight.cargo.site/w/600/q/75/i/a5ca95ac0bbc3d162d24eab276cfb734b66a926244adb8cdfe8aedd2eaa7e0df/1bubble.jpg',
    rice_rat='https://freight.cargo.site/w/1000/q/75/i/46ad780d2e9e75427743a4d78851db4b411121702916c85517717368f313bb47/zuniga-s-dark-rice-rat.jpg',
    om='https://freight.cargo.site/w/1500/q/75/i/725e28e4e8993de9e4a2eec489ed39e3df128fdce86824791929a7f186727afc/deepsleep-m.jpg',
    mum='https://freight.cargo.site/w/1500/q/75/i/df111c788a01b96edab8dffc59725cd2d7322386373bb2ec21c1133506f0b52a/mamann.jpg',

    )

    content_image_size = 384
    style_image_size = 384
    content_images = {k: load_image(v, (content_image_size, content_image_size)) for k, v in content_urls.items()}
    style_images = {k: load_image(v, (style_image_size, style_image_size)) for k, v in style_urls.items()}
    style_images = {k: tf.nn.avg_pool(style_image, ksize=[3,3], strides=[1,1], padding='SAME') for k, style_image in style_images.items()}

    
    content_name = 'monprofil'  # @param ['sea_turtle', 'tuebingen', 'grace_hopper']
    style_name = 'mum'  # @param ['kanagawa_great_wave', 'kandinsky_composition_7', 'hubble_pillars_of_creation', 'van_gogh_starry_night', 'turner_nantes', 'munch_scream', 'picasso_demoiselles_avignon', 'picasso_violin', 'picasso_bottle_of_rum', 'fire', 'derkovits_woman_head', 'amadeo_style_life', 'derkovtis_talig', 'amadeo_cardoso']

    stylized_image = hub_module(tf.constant(content_images[content_name]),
                            tf.constant(style_images[style_name]))[0]

    show_n([content_images[content_name], style_images[style_name], stylized_image],
       titles=['Original content image', 'Style image', 'Stylized image'])
    
    
   <img width="1209" alt="342" src="https://user-images.githubusercontent.com/73337770/114381168-1d580800-9b8b-11eb-993c-131bba71d66b.png">

    
