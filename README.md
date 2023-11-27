# Image_style_transfer

Please refer to the latest notebook. The 'Final' version.

I have used the benchmark VanGogh2Photo dataset for this task.
I have downloaded the dataset by git cloning the repo https://github.com/chickenfingerwu/CycleGAN-vangogh2photo-dataset in my pc and then uploaded the dataset folder.

A domain is the Van Gogh Paintings and B domain is the camera photos

Training set has 400 Van Gogh Paintings, 6287 camera photos, and test set has 874 Van Gogh Paintings, 1038 camera photos.
I have resized all the samples to (256x256)

# Model : 
I have used cycle gan for training : https://junyanz.github.io/CycleGAN/

It learns a map G_AB : A -> B and its inverse map G_BA : B -> A

I have used a resnet architecture for the generator:

Architecture :: 

Generator : 
            It has a conv layer followed by 2 down sampling layers.Then it has 9 residual blocks followed by 2 upsampling layers resulting same (256x256) image.


Discriminator : Made by conv layers

# Training : 

(https://arxiv.org/pdf/1703.10593.pdf)

It learns a map G_AB : A -> B and its inverse map G_BA : B -> A by using discriminators D_A and D_B.

The cycle GAN has following losses :

A)) Generator losses :: 

1) Identity loss = 0.5 * (d(A, G_AB(A)) + d(B, G_BA(B)))

It makes the generator to preserve content information like horses and zebras for transformed domain.

2) GAN loss = 0.5 * (d(disc(G_A(A)), 1) + d(disc(G_B(B), 1)))

It trains the generator to fool the discriminator.

3) Cycle loss = 0.5 * (d(A, G_BA(G_AB(A))) + d(B, G_AB(G_BA(B))))

It ensures that if you take an image from domain A, pass it through G_AB to get a translated image in domain B, and then pass that through G_BA, you should get an image that is similar to the original image from domain A.

*Generator loss = a * Identity loss + b * GAN loss + c * Cycle loss*


B)) Discriminator losses :: 

There is each discriminator for each domain. Each discriminator tries to discriminate between real and fake images in each domain. Fake images are transfered images from other domain.


## Compute metrics

We have used FID Score (Frenchet Inception Distance) and LPIPS Score (Learned Perceptual Image Patch Similarity) for evaluation.

FID Score : 

FID Score is the measure of the similarity between the distribution of generated images and real images. (https://torchmetrics.readthedocs.io/en/stable/image/frechet_inception_distance.html)

Lower FID scores generally indicate better image quality and realism.

LPIPS Score : 

Evaluate the distance between image patches. Lower means more similar.(https://pypi.org/project/lpips/)

# Conclusion :

FID score A = 1.2771543822509557e-08

FID score B = 1.7840944528579712

LPIPS score A = 0.0006569788092747331

LPIPS score B = 0.0007442692876793444


We see, this implementation gives better translation in domain A than B. 

Note: Domain A = VanGogh domain, domain B = camera photos.

Further training might solve this.

Notes and Further Work :

-> Getting it to work in the market we need to collect datasets of particular style and will need particular set of models for each pair of domains. Hence will require lots of memory as well.

-> Implement further methods like AdaIN, Neural Style Transfer.

-> Think of better way to do this!
