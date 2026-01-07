# Azure Portal Deployment Guide
## HR Assistant WITHOUT Responsible AI

This guide walks you through deploying Azure OpenAI Service using the Azure Portal.

**Time Required:** ~5 minutes  
**Cost:** < $1 for testing  
**Prerequisites:** Azure subscription with access to Azure OpenAI

---

## 📋 Deployment Steps

### Step 1: Create Azure OpenAI Service

1. **Navigate to Azure Portal**
   - Go to https://portal.azure.com
   - Sign in with your Azure credentials

2. **Search for Azure OpenAI**
   - In the search bar at the top, type "Azure OpenAI"
   - Click "Azure OpenAI" from the results

3. **Create New Resource**
   - Click "+ Create" button
   - Fill in the following details:

   **Basics Tab:**
   - **Subscription**: Select your subscription
   - **Resource group**: 
     - Click "Create new"
     - Name it: `rg-hr-assistant-demo`
     - Click "OK"
   - **Region**: `East US` (or your preferred region)
   - **Name**: `openai-hr-demo-[yourname]` (must be globally unique)
   - **Pricing tier**: `Standard S0`

4. **Configure Network (Optional)**
   - **Network Tab**: Leave as default (All networks)
   - For production, you'd restrict this

5. **Review and Create**
   - Click "Review + create"
   - Review the details
   - Click "Create"
   - Wait ~2 minutes for deployment to complete

6. **Navigate to Resource**
   - Once deployment completes, click "Go to resource"

---

### Step 2: Deploy GPT-4o Model

1. **Open Azure OpenAI Studio**
   - In your OpenAI resource, look for "Overview" page
   - Click the button "Explore Foundry portal"
   - Or click "Go to Foundry Portal" 

2. **Create New Deployment**
   - In Azure Microsoft Foundry | OpenAI Studio, click "Deployments" in the left menu under Shared resources
   - Click "+ Deploy model" then "deploy base model"

3. **Configure Model Deployment**
   - **Select a model**: Choose `gpt-4o` (or `gpt-4o-mini` for lower cost) then click confirm
   - **Deployment name**: `gpt-4o`
   - **Deployment type**: `standard`
   - **Deployment details**: click customize
      - **Model version**: `Upgrade once new default version becomes available`
   - **Tokens per minute rate limit**: Leave default (e.g., 10K)

4. **Create Deployment**
   - Click "Deploy"
   - Wait ~30 seconds for deployment

5. **Verify Deployment**
   - You should see your deployment listed
   - Status should shw "Succeeded"

---

### Step 3: Get Your Credentials

1. **Get Endpoint**
   - Go back to Azure Portal (portal.azure.com)
   - Navigate to your OpenAI resource: `rg-hr-assistant-demo`
   - Click "Keys and Endpoint" in the left menu under the 'Resource Management" tab
   - **Copy the Endpoint** (looks like: `https://openai-hr-demo-yourname.openai.azure.com/`)

2. **Get API Key**
   - On the same "Keys and Endpoint" page
   - **Copy Key 1** (you can use Key 2 if you prefer)
   - ⚠️ **Keep this secure** - treat it like a password

3. **Note Model Deployment Name**
   - If you used a different name than `gpt-4o`, note it down
   - You'll need this when running the code

---

### Step 4: Set Environment Variables

**Windows (PowerShell):**
```powershell
$env:AZURE_OPENAI_ENDPOINT="https://openai-hr-demo-yourname.openai.azure.com/"
$env:AZURE_OPENAI_KEY="your-key-here"
```

**Windows (Command Prompt):**
```cmd
set AZURE_OPENAI_ENDPOINT=https://openai-hr-demo-yourname.openai.azure.com/
set AZURE_OPENAI_KEY=your-key-here
```

**Mac/Linux:**
```bash
export AZURE_OPENAI_ENDPOINT="https://openai-hr-demo-yourname.openai.azure.com/"
export AZURE_OPENAI_KEY="your-key-here"
```

**Permanent Setup (.env file):**
Create a file named `.env` in your project directory:
```
AZURE_OPENAI_ENDPOINT=https://openai-hr-demo-yourname.openai.azure.com/
AZURE_OPENAI_KEY=your-key-here
```

Then use `python-dotenv` to load it:
```bash
pip install python-dotenv
```

Add to your code:
```python
from dotenv import load_dotenv
load_dotenv()
```

---

### Step 5: Run the Demo

1. **Install Python Dependencies**
   ```bash
   pip install openai
   ```

2. **Run the Application**
   ```bash
   python hr_assistant.py
   ```

3. **Expected Output**
   You should see:
   ```
   ⚠️  WARNING: This demo intentionally lacks RAI controls
   
   ================================================================================
   HR Assistant - WITHOUT Responsible AI Controls
   ================================================================================
   
   --- Test 1 ---
   Query: How much parental leave do fathers get compared to mothers?
   
   Response: [Shows biased response making gender assumptions]
   
   ⚠️ PROBLEM: May show bias in responses (gender assumptions)
   
   [... continues with Tests 2 and 3 ...]
   ```

---

## 🔍 Troubleshooting

### Error: "Resource not found"
- **Cause**: Incorrect endpoint or model deployment name
- **Fix**: 
  - Verify endpoint in Azure Portal → Keys and Endpoint
  - Verify model deployment name in Azure OpenAI Studio → Deployments
  - Update `hr_assistant.py` line 24 if your model isn't named `gpt-4o`

### Error: "Authentication failed"
- **Cause**: Incorrect API key
- **Fix**: 
  - Get a new key from Azure Portal → Keys and Endpoint
  - Make sure you copied the entire key (no spaces)
  - Regenerate keys if needed

### Error: "Rate limit exceeded"
- **Cause**: Too many requests to the API
- **Fix**: 
  - Wait a few minutes and try again
  - Increase TPM limit in deployment settings

### Error: "Module not found"
- **Cause**: Python package not installed
- **Fix**: 
  ```bash
  pip install openai
  # or
  pip install -r requirements.txt
  ```

### Model deployment not showing up
- **Cause**: Deployment still in progress
- **Fix**: Wait 1-2 minutes, refresh the page

---

## 💰 Cost Breakdown

### Azure OpenAI Service
- **GPT-4o**: ~$0.01-0.03 per demo run (3 queries)
- **GPT-4o-mini**: ~$0.001 per demo run (cheaper alternative)

### Monthly Estimates (Testing)
- **Light testing** (10 runs/day): < $10/month
- **Demo only** (5 runs total): < $1 total

### Cost Optimization Tips
- Use `gpt-4o-mini` for testing (10x cheaper)
- Delete resources when not in use
- Set up cost alerts in Azure

---

## 🗑️ Cleanup (Optional)

When you're done with the demo:

1. **Delete Resource Group**
   - Go to Azure Portal
   - Navigate to Resource Groups
   - Find `rg-hr-assistant-demo`
   - Click "Delete resource group"
   - Type the resource group name to confirm
   - Click "Delete"

This deletes everything (OpenAI service and all deployments) and stops all costs.

---

## 📚 Additional Resources

- [Azure OpenAI Documentation](https://learn.microsoft.com/azure/ai-services/openai/)
- [GPT-4o Model Details](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#gpt-4o-and-gpt-4-turbo)
- [Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)

---

## 🎯 Next Steps

Once this is working:
1. Test all 3 demo queries
2. Understand what problems you're seeing
3. Try the companion repo: [hr-assistant-with-rai](https://github.com/CyberAutomatedSystems/hr-assistant-with-rai)
4. See how RAI controls fix these problems!

---

## ✅ Deployment Checklist

- [ ] Azure OpenAI Service created
- [ ] GPT-4o model deployed
- [ ] Endpoint copied
- [ ] API key copied
- [ ] Environment variables set
- [ ] Dependencies installed (`pip install openai`)
- [ ] Demo runs successfully
- [ ] All 3 test queries showing problems

**Ready for your presentation!** 🚀
