# Latent Consistency Model for ComfyUI

![Context Node](./assets/preview.png)

This extension aims to integrate [Latent Consistency Model (LCM)](https://latent-consistency-models.github.io/) into [ComfyUI](https://github.com/comfyanonymous/ComfyUI).

Note that LCMs are a completely different class of models than Stable Diffusion, and the only available checkpoint currently is [LCM_Dreamshaper_v7](https://huggingface.co/SimianLuo/LCM_Dreamshaper_v7). Due to this, this implementation uses the [diffusers](https://huggingface.co/docs/diffusers/index) library, and not Comfy's own model loading mechanism.

## Installation

Simply clone this repo to your `custom_nodes/` directory:

```
git clone https://github.com/0xbitches/ComfyUI-LCM
```

Then restart ComfyUI.

## Img2Img / Vid2Vid

![vid2vid](./assets/vid2vid.gif)

For basic img2img, you can just use the `LCM_img2img_Sampler` node.

For vid2vid, you will want to install this helper node:
[ComfyUI-VideoHelperSuite](https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite).

Then, use the `Load Video` and `Video Combine` nodes to create a vid2vid workflow, or download [this workflow](./assets/lcm_vid2vid.json).

Huge thanks to [nagolinc](https://github.com/nagolinc) for implementing the pipeline.

## Known Issues

#### `ValueError: Non-consecutive added token '<|startoftext|>' found. Should have index 49408 but has index 49406 in saved vocabulary.`

To resolve this, locate your huggingface hub cache directory.

It will be something like `~/.cache/huggingface/hub/path_to_lcm_dreamshaper_v7/tokenizer/`. On Windows, it will roughly be `C:\Users\YourUserName\.cache\huggingface\hub\models--SimianLuo--LCM_Dreamshaper_v7\snapshots\c7f9b672c65a664af57d1de926819fd79cb26eb8\tokenizer\`.

Find the file `added_tokens.json` and change the contents to:

```
{
  "<|endoftext|>": 49409,
  "<|startoftext|>": 49408
}
```
