# 🚀 Fully Automated LoRA Training from a Single Image - ComfyUI Workflow

This repository presents a powerful, end-to-end **ComfyUI workflow** that automates the entire process of creating a custom LoRA model from **just one reference image**. Forget manual dataset creation and complex setup steps; this workflow leverages advanced AI models and specific custom nodes to streamline LoRA generation, enabling you to teach your AI unique characters or objects with unprecedented ease.

---

## ✨ How It Works: The Automated Pipeline

This workflow is designed as a seamless, integrated pipeline within ComfyUI, handling multiple complex stages automatically:

1.  **📸 Intelligent Prompt Generation via LLM (Gemini)**:
    * You provide a single reference image (e.g., a character or a product) as input.
    * An integrated Large Language Model (LLM), specifically `FL_GeminiVideoCaptioner` (from `ComfyUI_Fill-Nodes`), intelligently analyzes your input image and generates **20 diverse and contextually rich prompts**.
    * These prompts are meticulously crafted to ensure the subject (character or product) remains consistent while exploring varied placements, actions (for characters), expressions, viewpoints, and lighting conditions. Special care is taken to avoid anthropomorphism for products and ensure relevant character expressions/poses.

2.  **🎨 High-Fidelity Image Generation with FLUX.1 Kontext**:
    * The 20 LLM-generated prompts are fed into the powerful **FLUX.1 Kontext** model.
    * This state-of-the-art generative model, along with its "in-context" capabilities (via `ReferenceLatent` and `FluxGuidance` nodes), produces 20 high-quality images. Each image is a unique variation of your original subject within the described scenarios, maintaining remarkable visual consistency.

3.  **📦 Automated Data Preparation**:
    * Each generated image is automatically saved, and crucially, a corresponding `.txt` caption file is created by `SaveImageKJ` (from `ComfyUI-KJNodes`). This caption file contains the precise prompt that generated the image, which is essential for effective LoRA training.
    * The images and their captions are organized into a dataset folder, ready for the training phase.

4.  **🧠 Integrated LoRA Training (ComfyUI-FluxTrainer)**:
    * Unlike workflows that only prepare data for external tools, this workflow fully integrates the LoRA training process directly within ComfyUI using the `ComfyUI-FluxTrainer` nodes.
    * It handles model loading, optimizer configuration, dataset integration, and the training loop itself, outputting your custom LoRA model directly.

---

## 💡 Key Features & Workflow Highlights

* **Single Image to LoRA**: The core concept – go from one image to a trained LoRA model.
* **LLM-Powered Prompting**: Utilizes Google's Gemini (via `FL_GeminiVideoCaptioner`) for intelligent, varied, and context-aware prompt generation, saving immense manual effort.
* **FLUX.1 Kontext Consistency**: Leverages the "in-context" capabilities of FLUX.1 Kontext for unparalleled subject consistency across diverse generated images.
* **Strict Type Handling**: Robust logic ensures appropriate prompts and generations for **Characters** (varied expressions, poses) and **Products** (different angles, states), preventing unwanted mixing.
* **Automated Dataset Creation**: Generates 20 high-quality images with accompanying `.txt` captions.
* **Integrated LoRA Training**: Performs the actual LoRA training directly within ComfyUI using `ComfyUI-FluxTrainer`.
* **User-Friendly**: Designed to minimize manual intervention, even for complex generative tasks.

---

## 🛠️ Getting Started

To use this powerful workflow, you'll need the following:

1.  **ComfyUI Installation**:
    * Ensure you have ComfyUI set up and running.
    * [ComfyUI Official GitHub](https://github.com/comfyanonymous/ComfyUI)

2.  **Essential Custom Nodes**: Install these via the [ComfyUI Manager](https://github.com/ltdrdata/ComfyUI-Manager):
    * **ComfyUI-Impact-Pack**: `https://github.com/ltdrdata/ComfyUI-Impact-Pack`
    * **WAS Node Suite**: `https://github.com/WASasquatch/was-node-suite-comfyui`
    * **ComfyUI_Fill-Nodes**: `https://github.com/filliptm/ComfyUI_Fill-Nodes` (Contains `FL_GeminiVideoCaptioner`)
    * **ComfyUI-KJNodes**: `https://github.com/kijai/ComfyUI-KJNodes` (For `SaveImageKJ`)
    * **comfyui_tinyterranodes**: `https://github.com/TinyTerra/ComfyUI_tinyterranodes` (For path/folder inputs)
    * **ComfyUI-FluxTrainer**: `https://github.com/BlackForestLabs/ComfyUI-FluxTrainer` (For integrated LoRA training)
    * **ComfyUI-SP-Nodes**: `https://github.com/pythongosssss/ComfyUI-SP-Nodes` (For `LoraLoaderByPath` in the training validation part)
    * **pysssss (ComfyUI-Custom-Scripts)**: `https://github.com/pythongosssss/ComfyUI-Custom-Scripts` (For `ShowText|pysssss`)

3.  **Required Models**:
    * **FLUX.1 Kontext Diffusion Model**: The core generative model (e.g., `flux1-dev-kontext_fp8_scaled.safetensors`).
    * **FLUX VAE**: The autoencoder (`ae.safetensors`).
    * **FLUX Dual CLIP Text Encoders**: (`clip_l.safetensors`, `t5xxl_fp8_e4m3fn_scaled.safetensors`).
    * **FLUX Training Models**: (For `ComfyUI-FluxTrainer`, e.g., `flux1-dev.safetensors`, `t5xxl_fp16.safetensors`).
    * You can typically find these models on [Black Forest Labs' Hugging Face](https://huggingface.co/Comfy-Org/Lumina_Image_2.0_Repackaged) or their official channels [bfl.ai](https://bfl.ai/).

**Workflow Instructions:**

1.  **Download the Workflow**: Get the `workflow.json` file from this repository.
2.  **Load into ComfyUI**: Drag and drop the `workflow.json` file directly into your ComfyUI interface.
3.  **Input Your Reference Image**: Connect your single character or product image to the `Load Image` node. This image will be used by the LLM and the `ReferenceLatent` nodes for consistency.
4.  **Configure LLM API Key**: In the `FL Gemini (Fill nodes)` node (likely labeled "FL_GeminiVideoCaptioner"), enter your **Gemini API Key**.
5.  **Define Your Subject Type & Keyword**:
    * Locate the `INSTRUCTIONS` node (`Text Multiline`). The instructions within it guide the LLM's prompt generation.
    * You'll primarily interact with the inputs connected to the `ttN text` nodes in the "INPUT IMAGE" and "Dataset Preparation" groups:
        * **Trigger word**: Enter the unique trigger word for your LoRA (e.g., `loreal`, `johndoe`).
        * **FOLDER FOR LORA AND DATASET**: Specify the name of the subfolder where your generated images and LoRA will be saved (e.g., `my_custom_lora`). **Ensure this is unique for each new LoRA project.**
        * **YOUR PATH TO COMFYUI OUTPUT**: Confirm or set the base path to your ComfyUI output directory (e.g., `/workspace/ComfyUI/output/`).
6.  **Run the Workflow**: Press "Queue Prompt" in ComfyUI. The workflow will:
    * Generate 20 distinct prompts using Gemini.
    * Generate 20 varied images using FLUX.1 Kontext based on these prompts and your reference image.
    * Save these images and their captions.
    * Initiate and complete the LoRA training process within ComfyUI itself.
7.  **Access Your LoRA**: Your trained LoRA model will be saved in the specified output folder. The `ShowText|pysssss` node connected to `FluxTrainEnd` will display the full path to your generated LoRA file for easy retrieval.

---

## 🔗 Useful Links

* **Black Forest Labs (FLUX.1 Kontext)**: Learn more about the FLUX.1 Kontext model and its capabilities.
    * [https://bfl.ai/](https://bfl.ai/)
    * [FLUX.1 Kontext ArXiv Paper (PDF)](https://arxiv.org/pdf/2506.15742)

---