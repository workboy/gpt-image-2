---
name: "gpt-image-2"
description: "Use when the user asks to generate an image using the `gpt-image-2` model via sub2api. Requires `SUB2API_BASE_URL` and `SUB2API_API_KEY` to be set in the environment. Uses the `scripts/generate.py` CLI script."
---

# GPT-Image-2 Generation Skill

Creates images using the `gpt-image-2` model via the user's custom `sub2api` gateway.

## When to use
- Generate a new image from a prompt using `gpt-image-2`.

## Workflow
1. Collect the input prompt from the user.
2. Check if the user specified a size. If not, default to `1024x1024`.
3. Check if the environment variables `SUB2API_BASE_URL` and `SUB2API_API_KEY` are available. If not, ask the user to provide them or set them in the environment.
4. Run the bundled CLI script `scripts/generate.py` with the following arguments:
   `python scripts/generate.py --prompt "<your prompt>" --size "<size>"`
5. The script will either print image URL(s) or save base64 image payloads into `./output` and print the saved file path(s). Present the URL or embed the saved local file if the interface supports it.

### Image Generation

To generate an image based on a prompt, you can run the `generate.py` script from the skill directory.

```bash
# Generate a single image
python3 ~/.codex/skills/gpt-image-2/scripts/generate.py --prompt "A futuristic city at sunset, cyberpunk style"

# Generate an image with specific size
python3 ~/.codex/skills/gpt-image-2/scripts/generate.py --prompt "A cute cat" --size "1024x1024"

# Generate multiple images (if supported by backend)
python3 ~/.codex/skills/gpt-image-2/scripts/generate.py --prompt "A landscape" --n 2
```

### Image-to-Image (Image Edits/Modifications)

To modify or use an existing image as the base for generation, use the `--image` argument to pass a local image file path.

```bash
# Edit an existing image based on a prompt
python3 ~/.codex/skills/gpt-image-2/scripts/generate.py --prompt "Make it winter, add snow" --image "/path/to/base_image.png"
```

The script will automatically detect the `--image` argument, switch to the `/v1/images/edits` endpoint, and upload your image via `multipart/form-data`.

#### Output

The script outputs to the `./output/` directory relative to where it was run, or to a custom directory specified by `--output-dir`.

## Authentication
- `SUB2API_BASE_URL` must point to the root URL of the sub2api gateway (e.g., `https://ai.wbit.org` or `http://ec2...:8080`).
- `SUB2API_API_KEY` must be a valid API key from the sub2api dashboard.

## CLI Usage Example
```bash
# Example invocation
python scripts/generate.py --prompt "A futuristic city at sunset, synthwave style" --size "1024x1024"
```
