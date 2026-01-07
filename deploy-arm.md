# ARM Template Deployment Guide
## HR Assistant WITHOUT Responsible AI

This guide walks you through deploying Azure OpenAI Service using an ARM template.

**Time Required:** ~2 minutes  
**Prerequisites:** 
- Azure CLI installed, OR
- Access to Azure Cloud Shell

---

## 🚀 Quick Deployment

### Option A: Azure CLI (Local)

1. **Login to Azure**
   ```bash
   az login
   ```

2. **Set Your Subscription** (if you have multiple)
   ```bash
   az account list --output table
   az account set --subscription "Your Subscription Name"
   ```

3. **Create Resource Group**
   ```bash
   az group create \
     --name rg-hr-assistant-demo \
     --location eastus
   ```

4. **Deploy ARM Template**
   ```bash
   az deployment group create \
     --resource-group rg-hr-assistant-demo \
     --template-file deploy-arm.json \
     --parameters modelName=gpt-4o
   ```

5. **Get Outputs**
   ```bash
   az deployment group show \
     --resource-group rg-hr-assistant-demo \
     --name deploy-arm \
     --query properties.outputs
   ```

   Copy the `openAIEndpoint` value.

6. **Get API Key**
   ```bash
   az cognitiveservices account keys list \
     --resource-group rg-hr-assistant-demo \
     --name $(az deployment group show \
       --resource-group rg-hr-assistant-demo \
       --name deploy-arm \
       --query properties.outputs.openAIServiceName.value -o tsv)
   ```

---

### Option B: Azure Cloud Shell (No Install Needed)

1. **Open Cloud Shell**
   - Go to https://portal.azure.com
   - Click the Cloud Shell icon (>_) in the top navigation
   - Select "Bash"

2. **Upload ARM Template**
   - Click the "Upload/Download files" icon
   - Upload `deploy-arm.json` from your computer

3. **Create Resource Group**
   ```bash
   az group create \
     --name rg-hr-assistant-demo \
     --location eastus
   ```

4. **Deploy Template**
   ```bash
   az deployment group create \
     --resource-group rg-hr-assistant-demo \
     --template-file deploy-arm.json
   ```

5. **View Outputs**
   The deployment will output:
   - `openAIEndpoint`: Your endpoint URL
   - `openAIServiceName`: Your service name
   - `modelDeploymentName`: Your model name (gpt-4o)

6. **Get API Key**
   ```bash
   az cognitiveservices account keys list \
     --resource-group rg-hr-assistant-demo \
     --name <openAIServiceName from outputs>
   ```

---

### Option C: Azure Portal (Deploy Custom Template)

1. **Go to Azure Portal**
   - Navigate to https://portal.azure.com

2. **Search for "Deploy a custom template"**
   - In the search bar, type "deploy a custom template"
   - Click on "Deploy a custom template"

3. **Build Your Template**
   - Click "Build your own template in the editor"
   - Copy/paste the contents of `deploy-arm.json`
   - Click "Save"

4. **Configure Parameters**
   - **Subscription**: Select your subscription
   - **Resource group**: Create new → `rg-hr-assistant-demo`
   - **Region**: `East US` (or your preferred region)
   - **Model Name**: `gpt-4o` (or `gpt-4o-mini` for lower cost)
   - Leave other parameters as default

5. **Review and Create**
   - Click "Review + create"
   - Click "Create"
   - Wait ~2 minutes

6. **Get Outputs**
   - Once deployed, click "Outputs" in the left menu
   - Copy the `openAIEndpoint` value
   - Note the `openAIServiceName`

7. **Get API Key**
   - Navigate to your OpenAI resource
   - Click "Keys and Endpoint"
   - Copy Key 1

---

## 🔧 Customizing the Deployment

### Change Model to GPT-4o-mini (Lower Cost)

```bash
az deployment group create \
  --resource-group rg-hr-assistant-demo \
  --template-file deploy-arm.json \
  --parameters modelName=gpt-4o-mini
```

### Change Capacity (TPM Limit)

```bash
az deployment group create \
  --resource-group rg-hr-assistant-demo \
  --template-file deploy-arm.json \
  --parameters modelCapacity=20
```

### Custom Service Name

```bash
az deployment group create \
  --resource-group rg-hr-assistant-demo \
  --template-file deploy-arm.json \
  --parameters openAIServiceName=my-custom-name-123
```

---

## 📋 Post-Deployment Steps

### 1. Set Environment Variables

**Windows (PowerShell):**
```powershell
$env:AZURE_OPENAI_ENDPOINT="<endpoint from outputs>"
$env:AZURE_OPENAI_KEY="<key from step 6>"
```

**Mac/Linux:**
```bash
export AZURE_OPENAI_ENDPOINT="<endpoint from outputs>"
export AZURE_OPENAI_KEY="<key from step 6>"
```

### 2. Install Dependencies
```bash
pip install openai
```

### 3. Run Demo
```bash
python hr_assistant.py
```

---

## 🔍 Verification

### Check Deployment Status
```bash
az deployment group list \
  --resource-group rg-hr-assistant-demo \
  --output table
```

### List Resources
```bash
az resource list \
  --resource-group rg-hr-assistant-demo \
  --output table
```

### Test OpenAI Service
```bash
az cognitiveservices account show \
  --resource-group rg-hr-assistant-demo \
  --name <openAIServiceName>
```

---

## 🛠️ Troubleshooting

### Error: "Location not available"
**Fix:** Change location parameter
```bash
--parameters location=westus
```

Available locations: eastus, eastus2, westus, westus2, westeurope, northeurope

### Error: "Resource name already exists"
**Fix:** Use a custom name
```bash
--parameters openAIServiceName=my-unique-name-$(date +%s)
```

### Error: "Quota exceeded"
**Fix:** 
- Reduce capacity: `--parameters modelCapacity=5`
- Or request quota increase in Azure Portal

### Deployment stuck or failed
**Check logs:**
```bash
az deployment group show \
  --resource-group rg-hr-assistant-demo \
  --name deploy-arm
```

---

## 🗑️ Cleanup

### Delete Everything
```bash
az group delete \
  --name rg-hr-assistant-demo \
  --yes \
  --no-wait
```

This removes all resources and stops all costs.

---

## 💰 Cost Estimation

Same as portal deployment:
- **GPT-4o**: ~$0.01-0.03 per demo run
- **GPT-4o-mini**: ~$0.001 per demo run
- **Testing budget**: < $1

---

## 📚 ARM Template Parameters

| Parameter | Description | Default | Allowed Values |
|-----------|-------------|---------|----------------|
| openAIServiceName | Service name (globally unique) | Auto-generated | Any valid name |
| location | Azure region | Resource group location | eastus, westus, etc. |
| modelDeploymentName | Deployment name | gpt-4o | Any name |
| modelName | Model to deploy | gpt-4o | gpt-4o, gpt-4o-mini |
| modelVersion | Model version | 2024-08-06 | Valid version |
| modelCapacity | TPM capacity (thousands) | 10 | 1-100 |

---

## 🎯 Next Steps

1. ✅ Deployment complete
2. ✅ Endpoint and key obtained
3. ✅ Environment variables set
4. Run `python hr_assistant.py`
5. See the problems without RAI
6. Compare with [hr-assistant-with-rai](https://github.com/CyberAutomatedSystems/hr-assistant-with-rai)

---

## 🔄 Redeployment

To update or redeploy:

```bash
# Update with new parameters
az deployment group create \
  --resource-group rg-hr-assistant-demo \
  --template-file deploy-arm.json \
  --parameters modelCapacity=20 \
  --mode Incremental
```

ARM templates are idempotent - safe to run multiple times.

---

## ✅ Deployment Checklist

- [ ] Azure CLI installed (or using Cloud Shell)
- [ ] Logged into Azure
- [ ] Resource group created
- [ ] ARM template deployed successfully
- [ ] Endpoint copied from outputs
- [ ] API key retrieved
- [ ] Environment variables set
- [ ] Python dependencies installed
- [ ] Demo runs successfully

**Ready for automation!** 🤖
