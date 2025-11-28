# Azure App Service AI Scenarios: Complete Sample with AI Foundry Integration

This sample application demonstrates how to implement various AI scenarios on Azure App Service using Azure AI Foundry. It provides production-ready code that you can integrate into your existing Flask applications by copying the `AIPlaygroundCode` package and following the integration steps.

**Ideal for**: Developers looking to add AI capabilities to existing Flask applications, learn Azure AI Foundry integration patterns, and implement enterprise-grade AI features with minimal setup effort.

## Key Scenarios Covered

**Conversational AI**: Natural language processing with context awareness and session management  
**Reasoning Models**: Advanced problem-solving capabilities with step-by-step analytical thinking  
**Structured Output**: JSON-formatted responses with schema validation for system integration  
**Multimodal Processing**: Image analysis and audio transcription using vision and audio models  
**Enterprise Chat**: Retail-optimized AI assistant with customer service and business intelligence scenarios

## Quick Start - Azure Deployment (Recommended)

### Prerequisites
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) installed
- [Azure Developer CLI (azd)](https://docs.microsoft.com/en-us/azure/developer/azure-developer-cli/install-azd) installed
- Azure subscription with contributor access
- **Optional**: [Azure AI Foundry](https://ai.azure.com) access with existing model deployments if you want to use existing endpoints:
  - **Chat/Reasoning/Image model**: `gpt-4o-mini` or similar for conversational AI, reasoning, and image analysis
  - **Audio model**: `gpt-4o-mini-audio-preview` or similar for audio transcription and processing
  - **Model Compatibility**: Use recommended models (`gpt-4o-mini`, `gpt-35-turbo`) to avoid deployment errors. Advanced models like `gpt-4o` may not be available in all regions

### One-Command Deployment

1. **Clone and Deploy**
   ```bash
   git clone https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample.git
   cd azure-app-service-ai-scenarios-integrated-sample
   azd up
   ```
   
### What to Expect During Deployment

   When you run `azd up`, you'll see these prompts:

   1. `Enter a unique environment name:` → `myai-demo`

   2. **Subscription Selection** → Use arrow keys to select

   3. **Resource Group** → Create new or use existing

   4. `Select a location to create the resource group in:` e.g. `East US`

   5. `Choose new (create AI services) or existing [new/existing]:` e.g. `existing`

   6. `Enter your project endpoint URL:` e.g. `https://your-project.services.ai.azure.com/models`

   7. `Enter your deployment name (gpt-4o-mini, gpt-4, gpt-35-turbo):` e.g. `gpt-4o-mini`

   8. `Enter your audio deployment name (gpt-4o-mini-audio-preview):` e.g. `gpt-4o-mini-audio-preview`

   9. `Select an Azure location to use:` e.g. `East US`

   **Automatic steps**: Package building, RBAC setup, provisioning, deployment

   **Expected deployment time**: 4-5 minutes for complete provisioning and deployment

2. **Azure AI Foundry Integration During Deployment**
   During `azd up`, you'll be prompted to configure AI setup:
   
   **Prompt: "Do you have an existing Azure AI Foundry endpoint?"**
   - **Answer "existing"**: If you have existing Azure AI Foundry resources
     - You'll be asked to provide your endpoint URL (e.g., `https://your-project.services.ai.azure.com/models`)
     - Managed Identity role permissions will be automatically configured on your endpoint
     - You'll specify your existing model deployment names (chat model name e.g. gpt-4o-mini and audio model name e.g. gpt-4o-mini-audio-preview)
   - **Answer "new"**: Creates new Azure AI Foundry resources automatically
     - Provisions new Azure AI Foundry project with required dependencies
     - Deploys chat and audio models with configurable names (defaults: `gpt-4o-mini` and `gpt-4o-mini-audio-preview`)
     - Configures Managed Identity integration and updates all required settings
   

   
   **Configuration is automatic** - all environment variables and permissions are set up during deployment!
   
   **Note**: You no longer need to manually configure AI settings or manage API keys - everything is handled automatically through Managed Identity integration.

3. **Test Your Application**
   - Click "🧪 Test Config" to verify connection, then start using AI scenarios!
   - Refer to Usage Examples section below to test manually with sample scenarios

### What Gets Deployed
- **Azure App Service** (Basic B2 SKU) with Python 3.11
- **Azure AI Foundry resources** (if "new" was chosen for existing endpoint):
  - AI Hub with cognitive services multi-service account
  - AI Project workspace with model deployments (`gpt-4o-mini`, `gpt-4o-mini-audio-preview`)
  - Storage account for AI project data and model artifacts
- **Managed Identity Configuration** (automatic for both new and existing endpoints):
  - System-assigned managed identity enabled on App Service
  - "Cognitive Services OpenAI User" role assigned to access AI endpoints
  - "AI Developer" role assigned for Azure AI Foundry project access
- All necessary environment variables configured automatically in App Service

## Local Development Setup

### Prerequisites

- **Python 3.8+** 
  - Check version: `python --version`
  - Download from: https://python.org/downloads/
- **Azure AI Foundry** with deployed models
  - 📖 **Setup Guide**: [Create and deploy models in Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/create-projects)
  - **Required models**: `gpt-4o-mini` (chat/reasoning/image) and `gpt-4o-mini-audio-preview` (audio)
  - *Alternative: Use `azd up` deployment method above for automatic setup*

### Step-by-Step Installation & Setup

#### Step 1: Install and Launch Application

1. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Launch Application**
   ```bash
   python app.py
   ```

3. **Verify Application is Running**
   - Open http://localhost:5000 in your browser
   - You should see the application homepage

#### Step 2: Configure AI Settings

1. **Open Settings Page**
   - Navigate to http://localhost:5000/settings

2. **Fill in Configuration Fields**
   - **Azure AI Foundry Endpoint**: `https://your-project.services.ai.azure.com/models`
   - **API Key**: Your Azure AI Foundry API key *(Note: For production deployment, Managed Identity is automatically configured)*
   - **Chat Model Name**: Your deployed chat model name (e.g., `gpt-4o-mini`)
   - **Audio Model Name**: Your audio model deployment (e.g., `gpt-4o-mini-audio-preview`)

3. **Save and Test Configuration**
   - Click **Save** to store your settings
   - Click **"🧪 Test Config"** to verify connection
   - Wait for success confirmation message

#### Step 3: Test Core Features

Refer to the **[Test Core Features](#test-core-features)** section below for detailed testing instructions with sample scenarios.

**✅ Success Indicators:**
- Configuration test shows success message
- Application loads at http://localhost:5000
- Chat interface is accessible and functional

## Test Core Features

*[Screenshot placeholder: Add application screenshot showing the main interface]*

*Note: These tests work for both local development and Azure deployment setups.*

### Step-by-Step Testing Guide

#### 1. Access Chat Interface
- Go to http://localhost:5000
- Click the floating AI chat button (bottom-right corner)
- Verify the chat popup opens correctly

#### 2. Test Basic Conversational AI

**Test these exact messages** (copy and paste):

1. **Identity Test**:
   - **Message**: `"Who are you and what can you help with?"`
   - **Expected**: AI identifies as Enterprise AI Assistant, explains capabilities

2. **Product Inquiry Test**:
   - **Message**: `"Tell me about features and price for Pro Gaming X1"`
   - **Expected**: Relevant product information response

3. **Customer Service Test**:
   - **Message**: `"What is the return policy and how do I process a customer refund?"`
   - **Expected**: Helpful customer service guidance

#### 3. Test Multimodal Features

**Image Analysis Testing**:
1. **Type message**: `"Analyze this laptop and tell me its specifications"`
2. **Click 📎 button** to upload file
3. **Select file**: `tests/test_inputs/laptop.jpeg`
4. **Send message**
5. **Expected**: AI describes laptop specifications from the image

**Audio Processing Testing**:
1. **Type message**: `"Transcribe this customer service call"`
2. **Click 📎 button** to upload file
3. **Select file**: `tests/test_inputs/test_customer_service_audio.mp3`
4. **Send message**
5. **Expected**: AI provides transcription of the audio content

#### 4. Test Advanced Reasoning Capabilities

**Complex Business Analysis Testing**:
**Message**: `"Zava's sales have dropped 15% this quarter. Walk me through a systematic approach to identify root causes and develop an action plan."`
**Expected**: Structured analytical approach with step-by-step reasoning process

**✅ Success Indicators:**
- All chat responses are relevant and helpful
- Image analysis identifies laptop features accurately
- Audio transcription provides readable text from audio file
- Reasoning responses show structured analytical thinking
- No error messages in chat interface
- File uploads complete successfully
- Responses may show truncation message for long content (this is normal)
- Configuration test shows green success message

**📁 Sample Test Files**: Browse `tests/test_inputs/` folder for additional sample images and audio files to test multimodal capabilities.

## Integration with Existing Applications

This section provides guidance for integrating AI capabilities into your existing Flask applications. Note that this requires additional Azure resource setup and dependency management.

### Prerequisites for Integration
- Azure AI Foundry endpoint with model deployments:
  - Chat/reasoning/image model (e.g., `gpt-4o-mini`)
  - Audio model (e.g., `gpt-4o-mini-audio-preview`)

### Step 1: Set Up Azure Resources

**Configure App Service Managed Identity for Azure AI Foundry:**
```bash
# Enable system-assigned managed identity for your App Service
az webapp identity assign --name <your-app-name> --resource-group <your-rg>

# Grant required roles to your App Service for Azure AI Foundry access
az role assignment create \
  --role "Cognitive Services OpenAI User" \
  --assignee <managed-identity-principal-id> \
  --scope <your-ai-foundry-resource-id>

# Grant AI Developer role for Azure AI Foundry project access  
az role assignment create \
  --role "Azure AI Developer" \
  --assignee <managed-identity-principal-id> \
  --scope <your-ai-foundry-resource-id>
```

### Step 2: Copy and Merge Files

**Copy the AIPlaygroundCode folder:**
```bash
cp -r AIPlaygroundCode/ /path/to/your-existing-app/
```

**Merge Dependencies (Important!):**
- **requirements.txt**: Merge the dependencies from this sample's `requirements.txt` with your existing requirements file
- **wsgi.py**: If you have an existing `wsgi.py`, ensure it properly references your Flask app instance
- **app.py routes**: Copy the AI-related routes from the sample `app.py` to your existing Flask application

**Example requirements.txt merge:**
```txt
# Your existing dependencies
flask==2.3.3
# ... your other dependencies

# Add these AI-related dependencies from the sample
azure-identity==1.15.0
openai==1.35.13
pillow==10.0.0
pydub==0.25.1
```

### Step 3: Update App Service Configuration

**Add AI configuration as App Service environment variables:**
```bash
# Add Azure AI Foundry configuration as App Service environment variables
az webapp config appsettings set --name <your-app-name> --resource-group <your-rg> --settings \
  AZURE_INFERENCE_ENDPOINT="https://your-project.services.ai.azure.com/models" \
  AZURE_AI_CHAT_DEPLOYMENT_NAME="gpt-4o-mini" \
  AZURE_AI_AUDIO_DEPLOYMENT_NAME="gpt-4o-mini-audio-preview" \
  AZURE_CLIENT_ID="system-assigned-managed-identity"
```



### Step 4: Add AI Settings Route to Your Flask App

Add the settings route to your existing Flask application:
```python
# Add these imports to your existing app.py
from AIPlaygroundCode.config import get_model_config, update_model_config, is_configured

# Add AI configuration routes
@app.route('/settings')
def settings():
    from flask import render_template
    config = get_model_config()
    return render_template('AIPlaygroundCode/templates/settings.html', config=config)

@app.route('/settings', methods=['POST'])
def update_settings():
    # Copy the complete settings update logic from the sample app.py
    # This includes form handling, validation, and configuration updates
    pass
```

### Step 5: Add AI Interface to Your Application

Integrate the AI chat interface into your existing application pages by copying specific sections from `AIPlaygroundCode/templates/retail_home.html`:

**Look for these markers in retail_home.html and copy to your templates:**

1. **HTML Structure** (copy the section marked with `<!-- AI Chat Interface Start -->`):
   ```html
   <!-- AI Chat Interface Start -->
   <div id="chat-popup" class="chat-popup">
       <!-- Complete chat popup structure -->
   </div>
   <!-- AI Chat Interface End -->
   ```

2. **CSS Styles** (copy the section marked with `/* AI Chat Styles Start */`):
   ```html
   <style>
   /* AI Chat Styles Start */
   .chat-popup { /* ... */ }
   /* AI Chat Styles End */
   </style>
   ```

3. **JavaScript Functions** (copy the section marked with `// AI Chat JavaScript Start`):
   ```html
   <script>
   // AI Chat JavaScript Start
   function toggleChat() { /* ... */ }
   // AI Chat JavaScript End
   </script>
   ```

**Integration Steps:**
- Copy each marked section from `retail_home.html` to your main application template
- Ensure the chat button appears on your pages (floating bottom-right)
- Test the chat interface opens and can send messages
- Verify file upload functionality works for multimodal features



#### What You Get After Integration:
- **New Route**: `/settings` for AI configuration
- **Settings Page**: Self-service configuration interface for Azure AI Foundry endpoints
- **AI Chat Interface**: Integrated chat functionality within your application pages
- **Secure Configuration**: Managed Identity authentication with Azure AI Foundry (no API keys required)  

## Resource Clean-up

To prevent incurring unnecessary charges, it's important to clean up your Azure resources after completing your work with the application.

### When to Clean Up
- After you have finished testing or demonstrating the application
- If the application is no longer needed or you have transitioned to a different project
- When you have completed development and are ready to decommission the application

### Removing Azure Resources
To delete all associated resources and shut down the application, execute the following command:
```bash
azd down
```
Please note that this process may take up to 10 minutes to complete.

**Alternative:** You can delete the resource group directly from the Azure Portal to clean up resources.

---



## Guidance

### Costs
Pricing varies per region and usage, so it isn't possible to predict exact costs for your usage. The majority of Azure resources used in this infrastructure are on usage-based pricing tiers.

You can try the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator) for the resources:

**Core Resources (always deployed):**
- **Azure App Service**: Basic B2 tier with 3.5 GB RAM, 10 GB storage. [Pricing](https://azure.microsoft.com/pricing/details/app-service/windows/)

**Azure AI Foundry Resources (deployed when "No" is chosen for existing endpoint):**
- **Azure AI Services**: Multi-service account with consumption-based pricing for model usage (tokens). [Pricing](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)
- **Azure AI Hub**: Management layer for AI projects (minimal cost)
- **Azure Storage Account**: Standard LRS for AI project data and model artifacts (minimal cost). [Pricing](https://azure.microsoft.com/pricing/details/storage/)

**Cost-saving tip**: Choose "Yes" when prompted about existing Azure AI Foundry endpoint to reuse existing resources and avoid duplicate charges.

**Cost Management**: To avoid unnecessary costs, remember to clean up your resources when no longer needed by running `azd down` or deleting the resource group in the Azure Portal.

### Security Guidelines
This template uses [Managed Identity](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview) for secure authentication between Azure services.

**Additional Security Measures to Consider:**
- Enable [Microsoft Defender for Cloud](https://learn.microsoft.com/azure/defender-for-cloud/) to secure your Azure resources
- Implement network security with [Virtual Networks](https://learn.microsoft.com/azure/app-service/networking-features) for App Service
- Configure [Azure Web Application Firewall](https://learn.microsoft.com/azure/web-application-firewall/) for additional protection
- Enable [GitHub secret scanning](https://docs.github.com/code-security/secret-scanning/about-secret-scanning) in your repository

> **Important Security Notice**  
> This template has been built to showcase Azure AI services and tools. We strongly recommend implementing additional security features before using this code in production environments.

For comprehensive security best practices for AI applications, visit our [official documentation](https://learn.microsoft.com/azure/ai-foundry/).

---

## Disclaimers

**Software Usage and Compliance:**
To the extent that this software includes components or code used in or derived from Microsoft products or services, including Microsoft Azure Services, you must comply with the Product Terms applicable to such Microsoft Products and Services. Nothing in this license will serve to supersede, amend, terminate, or modify any terms in the Product Terms for any Microsoft Products and Services.

**Export Regulations:**
You must comply with all domestic and international export laws and regulations that apply to the software, which include restrictions on destinations, end users, and end use. For further information on export restrictions, visit [https://aka.ms/exporting](https://aka.ms/exporting).

**Medical and Financial Services Disclaimer:**
This software is not designed, intended, or made available as a medical device, and should not be used as a substitute for professional medical advice, diagnosis, treatment, or judgment. Similarly, it is not intended as a substitute for professional financial advice or judgment.

**High-Risk Use:**
BY ACCESSING OR USING THE SOFTWARE, YOU ACKNOWLEDGE THAT THE SOFTWARE IS NOT DESIGNED OR INTENDED TO SUPPORT ANY USE IN WHICH A SERVICE INTERRUPTION, DEFECT, ERROR, OR OTHER FAILURE COULD RESULT IN DEATH OR SERIOUS BODILY INJURY OF ANY PERSON OR IN PHYSICAL OR ENVIRONMENTAL DAMAGE. YOUR HIGH-RISK USE OF THE SOFTWARE IS AT YOUR OWN RISK.

---

## Support and Feedback

- **Issues**: Report bugs or request features via [GitHub Issues](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample/issues)
- **Questions**: Use [GitHub Discussions](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample/discussions) for implementation questions  
- **Rate the Sample**: Star the [repository](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample) if this helped your project

---

## References

### **Implementation Guides**
- **[GitHub Repository](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample)** - Complete implementation guide and source code
- **[Project Structure](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample/blob/main/docs/PROJECT_STRUCTURE.md)** - Learn the high-level constructs and architecture
- **[Configuration Guide](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample/blob/main/docs/CONFIGURATION_GUIDE.md)** - Understand configuration options and environment setup
- **[Testing Guide](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample/blob/main/docs/TESTING_GUIDE.md)** - Learn how to test the application locally and on Azure
- **[FAQ & Troubleshooting](https://github.com/Azure-Samples/azure-app-service-ai-scenarios-integrated-sample/blob/main/docs/FAQ_and_troubleshooting.md)** - Frequently asked questions and troubleshooting guide

### **Azure AI Foundry Documentation**
- **[Chat Completions](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/use-chat-completions?tabs=python)** - Basic conversational AI implementation
- **[Reasoning Models](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/use-chat-reasoning?tabs=openai&pivots=programming-language-python)** - Advanced reasoning capabilities  
- **[Multimodal AI](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/use-chat-multi-modal?pivots=programming-language-python)** - Image and audio processing
- **[Structured Outputs](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/use-structured-outputs?pivots=programming-language-python)** - JSON schema validation

---

**Ready to implement AI scenarios in your application? Start with the Quick Start guide above!**