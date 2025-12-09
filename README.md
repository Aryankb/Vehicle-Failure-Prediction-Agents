# Vehicle Analysis & Maintenance System 🚗

AI-powered vehicle diagnostics, maintenance scheduling, and performance analysis using specialized agents.

## Features

- **🔍 Diagnostic Agent**: Analyzes vehicle health, detects issues, interprets error codes
- **🔧 Maintenance Agent**: Provides maintenance recommendations and schedules
- **⚡ Performance Agent**: Analyzes efficiency, fuel economy, and driving metrics
- **🤖 Master Agent**: Intelligently routes queries to appropriate specialists
- **⏰ Scheduled Monitoring**: Automated hourly health checks with alert system
- **📊 Historical Tracking**: Maintains logs and analysis history

## Architecture

```
User Query → FastAPI → Master Agent → Specialized Agent → Tools (Fetch Data) → LLM Analysis → Response
```

### Agents

1. **Master Agent**: Routes queries based on intent
2. **Diagnostic Agent**: Health checks, error detection, troubleshooting
3. **Maintenance Agent**: Service schedules, fluid checks, preventive care
4. **Performance Agent**: Efficiency analysis, optimization recommendations

## Setup

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Configure API Key

```bash
# Copy example env file
cp .env.example .env

# Edit .env and add your Gemini API key
# Get key from: https://makersuite.google.com/app/apikey
```

Or export directly:
```bash
export GEMINI_API_KEY='your-api-key-here'
```

### 3. Run the API Server

```bash
python main.py
```

Server will start at `http://localhost:8000`

## API Usage

### Interactive Documentation

Visit `http://localhost:8000/docs` for Swagger UI

### Query a Vehicle

```bash
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{
    "vehicle_id": "VH001",
    "query": "Is my car healthy? Any issues?"
  }'
```

### Get Comprehensive Analysis

```bash
curl -X POST http://localhost:8000/analyze \
  -H "Content-Type: application/json" \
  -d '{
    "vehicle_id": "VH001"
  }'
```

### List Available Vehicles

```bash
curl http://localhost:8000/vehicles
```

### Get Vehicle Data

```bash
curl http://localhost:8000/vehicle/VH001
```

## Example Queries

- "Is my car healthy?"
- "Check engine light is on, what's wrong?"
- "What maintenance does my car need?"
- "How's my fuel efficiency?"
- "Should I be concerned about anything?"
- "When should I change the oil?"

## Scheduled Monitoring

Run automatic health checks on all vehicles:

### Manual Run

```bash
python monitor_cron.py
```

### Setup Cron Job (Hourly)

```bash
crontab -e
```

Add line:
```
0 * * * * cd /path/to/Vehicle-Failure-Prediction-Agents && /usr/bin/python3 monitor_cron.py >> cron.log 2>&1
```

For testing (every 5 minutes):
```
*/5 * * * * cd /path/to/Vehicle-Failure-Prediction-Agents && /usr/bin/python3 monitor_cron.py >> cron.log 2>&1
```

### Monitoring Output

- **Reports**: `dataset/monitoring_reports.json` - Full analysis history
- **Alerts**: `dataset/alerts.json` - Critical issues requiring attention
- **Logs**: `dataset/analysis_logs.json` - API query history

## Project Structure

```
Vehicle-Failure-Prediction-Agents/
├── main.py                 # FastAPI application
├── agents_final.py         # AI agents using PydanticAI
├── utils.py               # Data management utilities
├── monitor_cron.py        # Scheduled monitoring script
├── requirements.txt       # Python dependencies
├── .env.example          # Environment variables template
├── dataset/
│   ├── vehicle_realtime_data.json    # Vehicle database (simulated)
│   ├── analysis_logs.json            # API analysis history
│   ├── monitoring_reports.json       # Scheduled check reports
│   └── alerts.json                   # Critical alerts
└── README.md
```

## Sensor Reference Ranges

### Temperature
- Engine: 85-105°C (normal), >110°C (critical)
- Coolant: 80-95°C (normal), >105°C (critical)
- Motor (EV): 40-80°C (normal), >90°C (critical)

### Electrical
- 12V Battery: 12.4-14.8V (normal), <12.0V (critical)
- EV Battery SOC: >20% (normal), <10% (critical)

### Fluids
- Oil Pressure: 200-350 kPa (normal), <150 kPa (critical)
- Fuel Level: >25% (normal), <10% (critical)
- Brake Fluid: >70% (normal), <50% (critical)

### Tire Pressure
- Normal: 30-35 PSI
- Warning: 25-30 PSI
- Critical: <25 PSI

## Technologies Used

- **FastAPI**: Modern web framework
- **PydanticAI**: Agent framework with tool integration
- **Google Gemini**: LLM for analysis
- **Pydantic**: Data validation
- **Uvicorn**: ASGI server

## Development

### Add New Agent

1. Define agent in `agents_final.py`
2. Add system prompt with expertise
3. Create tools using `@agent.tool` decorator
4. Update master agent routing logic

### Add New Sensor

1. Update `SENSOR_RANGES` in `utils.py`
2. Add to vehicle data JSON
3. Update agent prompts if needed

## Notes

- Vehicle data is read from `dataset/vehicle_realtime_data.json` (simulates car API)
- No pre-trained ML models required - all analysis done by LLM
- Agents use tools to fetch data dynamically
- System designed for real-time and scheduled analysis

## License

MIT

## Support

For issues or questions, please open an issue on GitHub.
