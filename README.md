# comfyui_pipeline

## Image Generation
## OpenSource Models
StableDiffusion
Pisxart
Flux

## Closed Source Models
Midjourney
Dall-E

## Popular Techniques
ControlNet
LoRA
IP-Adapter
Outpainting
Inpainting
Upscaling

"We generate images with diffusion models we are starting with random noise, and the model gradually refines it step by step, until it forms a coherent image. This process is guided by our prompt (the embedding)."
Some people call it making art from noise.

We have the base model itself, the CLIP model, which translates our prompt into something the model can work with (i.e., the embedding), and an autoencoder that decodes the output of the process, giving us a finished image we can use.

When working with higher-level UIs such as MidJourney, you don’t participate in this process, but in ComfyUI, you have more control.

## Key Building Blocks
Want to tie in how image generation works for this part so it makes sense.

### -CLIP
What might be new is dealing with the CLIP and VAE directly, which are hidden when using higher-level systems like MidJourney.

### -Positive and Negative Prompt
In one prompt, you’ll write what you want, and in the second, you’ll write what you don’t want.

-Link the CLIP model to the prompts directly. This model converts the text into a vector (a mathematical representation) that guides the model on how to shape the image.
### -Empty Latent Image
Next, we need to add an Empty Latent Image node. This node creates an “empty” latent image filled with random noise in latent space which will be the starting point for the diffusion process.
### -K-Sampler
The KSampler orchestrates the entire denoising process, i.e. it transforms random noise into the latent representation of our generated image.

### -VAE
Once all these nodes are in place, we’ll need to decode the final latent representation produced by the KSampler into a full-resolution image.
The VAE Decode node will handle this, so be sure to add that one too.

Models are quite large. I have gathered a few popular checkpoints here if you’re at a loss. The files you’re looking for will have .safetensors as the file extension.

### -LoRAs
-LoRAs are much smaller in size and can be attached to different checkpoints. They are fine-tuned add-ons to a model. Think of the base model (checkpoint) as the blueprint for building a house. The LoRA is like adding custom paint without changing the structure of the house itself.
-Every LoRA comes with its own instructions so be sure to read the model page on how it should be used. Sometimes you need to use trigger words or other keywords in the prompt for it to work well.

-LoRA needs to be compatible with the base model, you can’t mix them. This means if you are using Flux Dev as the base, you look for LoRAs specifically for Flux Dev.

## ControlNets and IP Adapters
### ControlNets
ControlNet and other conditioning models allow you to guide the image generation process more precisely based on edges, depth maps, scribbles, or poses of an image.

For example, if we use the Canny Edge of a dog with a compatible ControlNet model, the generated image would follow the outline while having freedom to interpret everything else, like color and texture.

That’s why it’s important to pick a base model with a big community behind it, so you’ll have plenty of good ControlNet models to choose from.

### IP Adapters
IP Adapter (Image Prompt Adapter) models, on the other hand, can be thought of as 1-image LoRAs. If you are still confused by LoRAs then think of them as images as prompts. They transfer styling from a reference image to a new image. IP Adapters thus let you transfer styles, merge images, and imitate elements like faces.
Beyond these two technologies, there are more popular techniques such as inpainting, outpainting, upscaling, and segmentation. 


## Connect the Depth Map
Next, I’ll add a new node i haven’t used before called Instruct Pix to Pix Conditioning. This node should facilitate conditioning for image-to-image translation tasks with precise control over the generated image.

I could potentially use the ControlNet node here instead, but I didn’t try it for this time workflow.
