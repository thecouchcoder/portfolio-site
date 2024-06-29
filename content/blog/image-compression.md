+++
title = 'Optmizing Images by 95.2%'
date = 2024-06-29T06:38:23-05:00
+++

# Image Optimization Odyssey: Shrinking My App by 95.2%

Building a web app is an exciting journey, but sometimes you encounter unexpected detours. During the development of my app, [math-skill-tree](https://thecouchcoder.github.io/math-skill-tree/), I stumbled upon a roadblock: image optimization. The initial load size was a hefty 13.5MB! Ouch!

This post details my quest to optimize those images without sacrificing user experience.

## Stage 1: Exploring Common Solutions

The internet offers various image optimization solutions, like online tools and Content Delivery Networks (CDNs). These are fantastic options, but for a simple app, managing a CDN felt like overkill.

## Stage 2: Introducing Calibre to the Pipeline

Since I was already using Github Actions for deployment, I recognized an opportunity to streamline the process. Integrating image compression directly into the deployment pipeline would create a more efficient workflow. To achieve this, I found Calibre Image Action, a pre-built Github Action specifically designed for image optimization (https://github.com/calibreapp/image-actions). With a straightforward configuration, Calibre reduced the image size to 8.31MB using default settings. **That's a whopping 38.44% reduction!**

v Stage 3: Quality vs. Size: The Balancing Act

But I wasn't satisfied yet. I tweaked Calibre's settings and settled on a quality of 30 (the default is 80). This struck a good balance between image quality and size reduction, bringing us down to 7.05MB, **another 15.15%!**

While this was progress, it still felt like a large footprint for a simple app.

## Stage 4: Unveiling the Power of ImageMagick

Digging deeper, I discovered that many of my images were much larger than needed for the website. This is where ImageMagick, a powerful open-source tool for image manipulation, came to the rescue.

Using ImageMagick, I could specify the maximum dimensions for the images, ensuring they were all roughly the same size while maintaining the original aspect ratio. Importantly, I only wanted to resize the larger images, not those already at the correct size.

## Stage 5: Automating the Process with PowerShell

With a significant number of photos to process, I wrote a PowerShell script to automate the resizing for all images in my /assets directory. It will be shared at the end.

This final step was the game-changer. By combining compression and resizing, **I achieved a remarkable 95.2% reduction in image size**, bringing the total load down to a much more manageable 664kb.

## Every Byte Counts

This experience highlighted the importance of image optimization, especially for web applications. By taking the time to explore different techniques, I significantly improved the loading speed and user experience of my app.

Looking back, building a customizable Github Action that combines all these steps into one user-friendly package might be a valuable project for the future. What do you think?
