# Configurable Endpoints

SubModel's Configurable Endpoints feature empowers users to seamlessly deploy any large language model (LLM) through the integration of vLLM, a high-performance inference engine.

When opting for the **Serverless vLLM** deployment option, SubModel harnesses vLLM's advanced capabilities to efficiently load and execute your specified Hugging Face model. This integration streamlines the deployment process by abstracting away complex technical details, allowing you to focus on model selection and parameter customization while vLLM handles the intricacies of model loading, hardware optimization, and execution.

## Step-by-Step LLM Deployment Guide

1. **Initiate Deployment**  
   Navigate to the **Explore** section and select **Serverless vLLM** to begin deploying your chosen large language model.

2. **Configure vLLM Deployment**  
   In the vLLM deployment modal, provide the following details:
   - Select your preferred vLLM version
   - Specify the Hugging Face LLM repository name
   - (Optional) Provide your Hugging Face authentication token

3. **Review vLLM Parameters**  
   Proceed to the next step to examine and verify the vLLM configuration settings.

4. **Customize Endpoint Parameters**  
   On the Endpoint configuration page:
   - **Worker Configuration**: Prioritize GPU allocation by specifying your preferred GPU selection order
   - **Worker Specifications**: Define the number of Active Workers, Max Workers, and GPUs per Worker
   - **Container Configuration**:
     - (Optional) Select a pre-configured Template
     - Verify the Container Image utilizes your desired CUDA version
     - Specify the required Container Disk size
     - Review and confirm Environment Variables

5. **Finalize Deployment**  
   Complete the process by selecting **Deploy** to initiate the model deployment.

Upon successful deployment, your LLM will be accessible through the designated Endpoint, ready for API-based interactions and inference tasks.

> **Important Note**  
> SubModel's Configurable Endpoints support any model architecture compatible with [vLLM](https://github.com/vllm-project/vllm), offering flexibility in model deployment while maintaining optimal performance.

This enhanced version maintains the original technical content while improving readability, structure, and professional tone. The information is now presented in a more organized and accessible format, with clear section divisions and improved flow between steps. Technical terms are preserved while explanations are slightly expanded for better understanding.