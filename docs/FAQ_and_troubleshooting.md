# FAQ & Troubleshooting

## Frequently Asked Questions

### **Q: How do I add AI to my existing Flask application?**
**A:** See the [Integration with Existing Applications](../README.md#integration-with-existing-applications) section in the main README for step-by-step instructions.

### **Q: What Azure AI models are supported?**  
**A:** All Azure AI Foundry models including chat, reasoning, and multimodal models. Configure your deployed model name in settings.

### **Q: How do I enable multimodal capabilities?**
**A:** Enable "Multimodal" in `/settings` and ensure your model supports vision/audio (like gpt-4o, phi-4-multimodal). Upload images/audio via the 📎 button.

### **Q: How do I use reasoning model capabilities?**
**A:** Enable "Reasoning" in `/settings` and use reasoning-capable models. These models provide advanced problem-solving and step-by-step reasoning. Set "Reasoning Effort" to control depth (low/medium/high) and enable "Show Reasoning" to see the thinking process.

### **Q: How do I use structured input/output capabilities?**
**A:** Enable "Structured Output" in `/settings`, set "Response Format" to "JSON", and define a JSON schema in the "JSON Schema" field. The AI will return responses matching your specified schema structure, perfect for system integration and data processing workflows.

### **Q: Do I need API keys for Azure deployment?**
**A:** No! When deployed to Azure using `azd up`, the application automatically uses Managed Identity for secure authentication with Azure AI Foundry. API keys are only needed for local development testing.

## Troubleshooting Common Issues

### Deployment Issues

**"Endpoint not found" or 401 Authentication errors:**
- Verify your Azure AI Foundry endpoint URL format: `https://your-project.services.ai.azure.com/models`
- ⚠️ **Common mistake**: Using `https://your-project.openai.azure.com/` (incorrect format)
- Ensure Managed Identity has proper permissions:
  - "Cognitive Services OpenAI User" role for model access
  - "Azure AI Developer" role for project access
- Use the "🧪 Test Config" button to validate connection
- Check that your model deployments are active in Azure AI Foundry
- Verify `AZURE_USE_MANAGED_IDENTITY=true` is set in App Service configuration

**Azure deployment fails with "azd up" command:**
- Ensure you have Contributor access to your Azure subscription
- Check if you've reached subscription limits for App Service or AI services
- Try running `azd auth login` to refresh your authentication
- Clear previous deployments with `azd down` before retrying

**Application not starting after deployment:**
- Check App Service logs in Azure portal for detailed errors (under Monitoring > Log stream)
- Verify Managed Identity permissions are properly configured for Azure AI Foundry resources
- Ensure all required Azure AI Foundry environment variables are set in App Service Configuration
- Review deployment logs for any missing dependencies or configuration issues
- Check that Azure AI Foundry project and model deployments are active

**"ModuleNotFoundError" or dependency issues:**
- Verify all files from `requirements.txt` were properly deployed
- Check Python version compatibility (requires Python 3.8+)
- Review deployment logs in Azure App Service for package installation errors

**File upload failures (multimodal scenarios):**
- Check file size limits (images: 5MB, audio: 10MB by default)
- Verify supported file formats (JPEG, PNG for images; MP3, WAV for audio)
- Ensure "Enable Multimodal" is checked in settings
- Confirm your model supports multimodal capabilities

### Integration-Specific Issues

**"Managed Identity permissions" errors after integration:**
- **Cause**: Step 1 (Azure Resources Setup) not completed properly
- **Symptoms**: 403 Forbidden errors when accessing AI endpoints
- **Solution**: Verify managed identity roles are assigned correctly using `az role assignment list`

**"AIPlaygroundCode module not found" errors:**
- **Cause**: Step 2 (Copy and Merge Files) incomplete
- **Symptoms**: ImportError or ModuleNotFoundError when starting application
- **Solution**: Ensure `AIPlaygroundCode` folder is copied to your application root and dependencies are merged

**"AZURE_INFERENCE_ENDPOINT not set" configuration errors:**
- **Cause**: Step 3 (App Service Configuration) not completed
- **Symptoms**: Configuration errors on `/settings` page, "Test Config" button fails
- **Solution**: Verify environment variables are set in App Service Configuration blade

**"/settings route not found" (404 errors):**
- **Cause**: Step 4 (Add AI Routes) not implemented
- **Symptoms**: 404 Not Found when navigating to `/settings`
- **Solution**: Add the settings routes to your Flask application and restart

**Chat interface not appearing on pages:**
- **Cause**: Step 5 (Add AI Interface) not completed properly
- **Symptoms**: No chat interface visible, JavaScript errors in browser console
- **Solution**: Copy complete HTML, CSS, and JavaScript from `retail_home.html` to your templates

**"Template not found" errors for settings page:**
- **Cause**: Templates path not configured correctly
- **Symptoms**: Jinja2 TemplateNotFound error when accessing `/settings`
- **Solution**: Ensure Flask can find `AIPlaygroundCode/templates/settings.html` or update template path

### Installation & Runtime Issues

**Python not found error:**
- **Windows**: Add Python to PATH during installation, or use `py` command
- **Mac/Linux**: Use `python3` and `pip3` instead of `python` and `pip`
- **Verification**: Run `python --version` to confirm Python 3.8+ is installed

**Port 5000 already in use:**
- **Solution 1**: Change port in `app.py`: modify `app.run(port=5001)` 
- **Solution 2**: Kill existing process using port 5000
- **Windows**: `netstat -ano | findstr :5000` then `taskkill /PID <process_id> /F`

**Permission errors during pip install:**
- **Solution 1**: Use `pip install --user -r requirements.txt`
- **Solution 2**: Use virtual environment (recommended):
  ```bash
  python -m venv venv
  # Windows: venv\Scripts\activate
  # Mac/Linux: source venv/bin/activate
  pip install -r requirements.txt
  ```

**Configuration test fails:**
- Verify endpoint URL format exactly matches: `https://your-project.services.ai.azure.com/models`
- Check API key is correct and has proper permissions
- Ensure models are deployed and active in Azure AI Foundry
- Verify model deployment names match exactly

**"Session cookie too large" warnings:**
- **Cause**: Large conversation history stored in browser session
- **Solution**: Clear browser cookies for localhost:5000 or use incognito mode
- **Impact**: Warning only - does not affect functionality

**Audio processing appears stuck:**
- **Check**: Look at terminal/console logs for "Successfully processed audio message"
- **Solution**: If processing succeeded but UI doesn't show result, refresh page
- **Alternative**: Clear browser session and retry

**Chat interface not responding:**
- Verify "Test Config" passes successfully
- Check browser console (F12) for JavaScript errors
- Ensure you've saved the configuration before testing
- Try refreshing the page

**"Response continued but truncated to manage session size" message:**
- **Cause**: Long AI responses are automatically truncated to manage browser session size
- **Behavior**: Responses over 1,000 characters in chat interface show this message
- **Impact**: This is normal session management - functionality works correctly
- **Note**: Complete responses are still processed, only display is truncated