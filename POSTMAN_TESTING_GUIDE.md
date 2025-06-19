# Media Planner Multi-Agent System - Postman Testing Guide

## 📁 Files to Import

You now have two files ready for Postman import:

1. **`Media-Planner-Multi-Agent-Collection.postman_collection.json`** - Main test collection
2. **`Media-Planner-Environment.postman_environment.json`** - Environment variables

## 🚀 Import Instructions

### Step 1: Import the Collection
1. Open Postman
2. Click **Import** button (top left)
3. Drag and drop or select `Media-Planner-Multi-Agent-Collection.postman_collection.json`
4. Click **Import**

### Step 2: Import the Environment
1. In Postman, go to **Environments** (left sidebar)
2. Click **Import** 
3. Select `Media-Planner-Environment.postman_environment.json`
4. Click **Import**

### Step 3: Activate Environment
1. In the top-right corner, click the environment dropdown
2. Select **"Media Planner Development"**
3. Verify variables are loaded (you should see `{{base_url}}` etc.)

## 🔧 Pre-Testing Setup

### 1. Start Required Services
```bash
# Terminal 1: Start Temporal CLI development server
temporal server start-dev --ui-port 8088

# Terminal 2: Start FastAPI backend
cd media-planner-infra
poetry install
poetry run uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### 2. Update Environment Variables (Optional)
- Go to **Environments** > **Media Planner Development**
- Update values if needed:
  - `base_url`: Change if not using localhost:8000
  - `tenant_id`: Use your actual tenant ID
  - `test_spreadsheet_id`: Use your Google Sheets ID for testing

## 📋 Test Collection Structure

### 🏥 Health Checks
**Start here to verify system status**
- **API Health Check**: Basic API connectivity
- **Temporal Health Check**: Verify Temporal server connection
- **All Agents Health Check**: Multi-agent system status

### 🤖 Agent Discovery
**Explore available agents**
- **List All Agents**: Get all agent information
- **Individual Agent Health**: Check each agent (Workspace, Planning, Insights)

### 📊 Individual Agent Tasks
**Test single agent capabilities**
- **Workspace Agent**: Google Sheets extraction, file parsing
- **Planning Agent**: Budget allocation, campaign optimization
- **Insights Agent**: Performance analysis, recommendations

### 🔄 Multi-Agent Workflows
**Test complete orchestrated workflows**
- **Campaign Planning Workflow**: Full end-to-end campaign planning
- Monitor progress via Temporal UI at http://localhost:8088

### 📈 Workflow Monitoring
**Track workflow execution**
- **Get Workflow Status**: Check status of running workflows
- Use `{{workflow_id}}` from previous workflow responses

### 🔗 Google API Integrations
**Test external integrations**
- **Google Sheets**: Data extraction and parsing
- **Google Drive**: File discovery and management

## 🧪 Recommended Testing Flow

### Quick Health Check (2 minutes)
1. Run **Health Checks** folder
2. Verify all services are running
3. Check agent availability

### Individual Agent Testing (10 minutes)
1. Start with **Workspace - Extract Google Sheets**
2. Test **Planning - Calculate Budget Allocation**
3. Try **Insights - Analyze Campaign Performance**
4. Review responses and validate data

### Complete Workflow Testing (15 minutes)
1. Run **Campaign Planning Workflow**
2. Copy `workflow_id` from response
3. Paste into environment variable `workflow_id`
4. Monitor with **Get Workflow Status**
5. Check Temporal UI for detailed execution: http://localhost:8088

### Integration Testing (10 minutes)
1. Test **Google Sheets - Extract Data**
2. Try **Google Drive - Discover Files**
3. Validate data extraction and parsing

## 🔍 Expected Responses

### Successful Agent Task Response
```json
{
  "task_id": "task_workspace_20241213_143022_123456",
  "success": true,
  "agent_type": "workspace",
  "task_type": "extract_google_sheets",
  "result": {
    "extracted_data": {...},
    "validation": {...}
  },
  "timestamp": "2024-12-13T14:30:22Z",
  "execution_time_ms": 1250
}
```

### Successful Workflow Response
```json
{
  "workflow_id": "workflow_campaign_planning_20241213_143022_789",
  "status": "started",
  "current_stage": "initialization",
  "results": {
    "message": "Multi-agent workflow started successfully",
    "temporal_workflow_id": "workflow_campaign_planning_20241213_143022_789"
  },
  "created_at": "2024-12-13T14:30:22Z"
}
```

## 🛠 Troubleshooting

### Common Issues

#### 1. Connection Refused (500 Error)
- **Problem**: API server not running
- **Solution**: Start FastAPI server: `poetry run uvicorn main:app --reload`

#### 2. Temporal Not Available
- **Problem**: Temporal server not running
- **Solution**: Start Temporal CLI: `temporal server start-dev --ui-port 8088`

#### 3. Agent Service Unavailable
- **Problem**: LangGraph agents not initialized
- **Solution**: Check FastAPI logs, verify agent service startup

#### 4. Google API Errors
- **Problem**: Google API credentials not configured
- **Solution**: Set up Google OAuth credentials in environment

### Debug Steps
1. Check **API Health Check** first
2. Review FastAPI logs in terminal
3. Check Temporal UI at http://localhost:8088
4. Verify environment variables are set correctly

## 📊 Monitoring & Observability

### Temporal Web UI (http://localhost:8088)
- **Workflows**: See all workflow executions
- **Activities**: Track individual agent tasks
- **Retry Attempts**: Monitor failure/retry patterns
- **Performance**: View execution times and throughput

### FastAPI Logs
- Real-time agent execution logs
- Error details and stack traces
- Performance metrics

### Postman Console
- Request/response details
- Environment variable values
- Test results and assertions

## 🚨 Important Notes

1. **Environment Variables**: Always select the correct environment before testing
2. **Workflow IDs**: Copy workflow IDs from responses to monitor status
3. **Google Sheets**: Use public or properly authenticated Google Sheets for testing
4. **Rate Limiting**: Be mindful of API rate limits during bulk testing
5. **Async Execution**: Workflows may take time; use status endpoints to monitor

## 📈 Performance Expectations

- **Health Checks**: < 1 second
- **Individual Agent Tasks**: 2-10 seconds
- **Multi-Agent Workflows**: 30-120 seconds
- **Google API Calls**: 3-15 seconds

## 🔄 Continuous Testing

For automated testing, run requests in this order:
1. Health Checks → Agent Discovery → Individual Tasks → Workflows → Monitoring

Use Postman's **Collection Runner** for batch execution and automated testing scenarios.

Happy testing! 🎉 