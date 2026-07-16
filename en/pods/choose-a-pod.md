# Choose a Pod

Selecting the optimal Pod configuration is crucial when planning your SubModel deployment. The choice of VRAM, RAM, vCPU, and storage (both Temporary and Persistent) will have a significant impact on your project's performance and efficiency.

This page provides guidance on how to choose the right Pod configuration. These are general recommendations, so it's important to consider your specific requirements when making the selection.

## Overview

Understanding the specific needs of your model is key. You can usually find detailed information in the model card description on platforms like Hugging Face, or in the `config.json` file of your model.

There are also tools available to help you assess and calculate the specific requirements of your model, such as:

- [Hugging Face's Model Memory Usage Calculator](https://huggingface.co/spaces/hf-accelerate/model-memory-usage)
- [Vokturz' Can It Run LLM Calculator](https://huggingface.co/spaces/Vokturz/can-it-run-llm)
- [Alexander Smirnov’s VRAM Estimator](https://vram.asmirnov.xyz)

Using these tools will give you a better understanding of what to look for in a Pod.

When moving on to Pod selection, focus on the following key factors:

- **GPU**
- **VRAM**
- **Disk Size**

Each of these elements plays a vital role in the performance and efficiency of your deployment. By carefully considering these factors along with the specific needs of your project, as identified in your initial research, you'll be well-positioned to choose the most appropriate Pod configuration.

## GPU

The type and power of the GPU are essential for your project's processing capabilities, particularly in tasks involving graphics processing and machine learning.

### Importance

The GPU in your Pod is crucial for processing complex algorithms, particularly for tasks such as data science, video processing, and machine learning. A more powerful GPU can significantly accelerate computations and enable more complex tasks.

### Selection Criteria

- **Task Demands**: Consider the intensity and nature of the GPU tasks in your project.
- **Compatibility**: Ensure that the GPU is compatible with your chosen software and frameworks.
- **Power Efficiency**: Take into account the power consumption of the GPU, especially for long-term deployments.

## VRAM

VRAM (Video RAM) is particularly important for tasks that demand heavy graphical processing and rendering. It is the dedicated memory used by your GPU to store image data for display.

### Importance

VRAM is crucial for intensive tasks, as it acts as the memory for the GPU, allowing for quick data storage and access. More VRAM is necessary for handling larger textures and more complex graphics, which is particularly important for high-resolution displays and advanced 3D rendering.

### Selection Criteria

- **Graphics Demands**: More VRAM is needed for graphically intense tasks such as 3D rendering, gaming, or training AI models with large datasets.
- **Parallel Processing Needs**: Tasks requiring the simultaneous processing of multiple data streams benefit from more VRAM.
- **Future-Proofing**: Opting for more VRAM can help make your setup more adaptable to future needs.

## Storage

Proper storage, including both temporary and persistent options, is crucial for smooth operations and effective data management.

### Importance

Disk space, both temporary and persistent, is important for data storage, caching, and ensuring that your project has the necessary space to run smoothly.

### Selection Criteria

- **Data Volume**: Estimate the amount of data your project will generate and process.
- **Speed Requirements**: Faster disk speeds can boost overall system performance.
- **Data Retention**: Consider the balance between temporary (volatile) and persistent (non-volatile) storage based on your data retention policies.