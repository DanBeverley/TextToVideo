# Introduction
VQGAN stands for Vector Quantized Generative Adversarial Network, while CLIP stands for Contrastive Image-Language Pretraining. Whenever we say VQGAN-CLIP1, we refer to the interaction between these two networks. They’re separate models that work in tandem. The way they work is that VQGAN generates the images, while CLIP judges how well an image matches our text prompt. This interaction guides our generator to produce more accurate images.

# VQGAN
VQGAN employs two-stage structure by learning an intermediary representation before feeding it to a transformer. However, instead of downsampling the image, VQGAN uses a codebook to represent visual parts. The authors did not model the image from a pixel-level directly, but instead from the codewords of the learned codebook.

![image](https://github.com/DanBeverley/TextToVideo/assets/161696810/05fa62c4-8b4d-4249-b290-e6cab38943ec)

VQGAN was able to solve Transformer’s scaling problem by using an intermediate representation known as a codebook. This codebook serves as the bridge for the two-stage approach found in most image transformer techniques. The VQGAN learns a codebook of context-rich visual parts, whose composition is then modeled with an autoregressive transformer.

The codebook is generated through a process called vector quantization (VQ), i.e., the “VQ” part of “VQGAN.” Vector quantization is a signal processing technique for encoding vectors. It represents all visual parts found in the convolutional step in a quantized form, making it less computationally expensive once passed to a transformer network.

One can think of vector quantization as a process of dividing vectors into groups that have approximately the same number of points closest to them. Each group is then represented by a centroid (codeword), usually obtained via k-means or any other clustering algorithm. In the end, one learns a dictionary of centroids (codebook) and their corresponding members.

# CLIP
CLIP( Contrastive Language–Image Pre-training ) a model trained to determine which caption from a set of captions best fits with a given image.

OpenAI's CLIP model aims to learn generic visual concept with natural language supervision. This is because sandard computer vision model only work well on specific task, and require significant effort to adapt to a new task, hence have weak generalization capabilities. CLIP bridges the gap via learning directly from raw text about images at a web scale level. CLIP does not directly optimize for the performance of a benchmark task (e.g. CIFAR), so as to keep its "zero-shot" capabilities for generalization. More interestingly, CLIP shows that scaling a simple pre-training task - which is to learn "which text matches with which image", is sufficient to achieve competitive zero-shot performance on many image classification datasets.

![image](https://github.com/DanBeverley/TextToVideo/assets/161696810/17476c2d-f6ee-4047-89ab-b4c8d648c5d8)
![image](https://github.com/DanBeverley/TextToVideo/assets/161696810/18f9cf4e-fb0f-4f3c-8df2-9dd4685e2a40)

CLIP trains a text encoder (Bag-of-Words or Text Transformer) and an image encoder (ResNet or Image Transformer) which learns feature representations of a given pair of text and image. The scaled cosine similarity matrix of the image and text feature is computed, and the diagonal values are minimized to force the image feature match its corresponding text feature.

# How do VQGAN and CLIP work together

CLIP guides VQGAN towards an image that is the best match to a given text. CLIP is the “Perceptor” and VQGAN is the “Generator”. VQGAN like all GANs VQGAN takes in a noise vector, and outputs a (realistic) image. CLIP on the other hand takes in an image and text, and outputs the image features and text features respectively. The similarity between image and text can be represented by the cosine similarity of the learnt feature vectors.

By leveraging CLIPs capacities as a “steering wheel”, we can use CLIP to guide a search through VQGAN’s latent space to find images that match a text prompt very well according to CLIP.

source - VQGAN+CLIP — How does it work? https://alexasteinbruck.medium.com/vqgan-clip-how-does-it-work-210a5dca5e52

# Contributing
Contributions are welcome! Please feel free to submit pull requests or open issues for bug fixes, feature requests, or general feedback
