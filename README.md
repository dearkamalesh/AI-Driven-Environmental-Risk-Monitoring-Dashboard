# AI-Driven Environmental Risk Monitoring Dashboard

A comprehensive full-stack application for monitoring environmental data, detecting anomalies, forecasting trends, and managing risk alerts across multiple locations.

## Features

### Core Functionality
- **Real-time Data Ingestion**: IoT sensor data ingestion with validation and OpenAQ API integration
- **Interactive Dashboard**: Map-based visualization with clustered location pins and severity indicators
- **Analytics Engine**: Rolling features calculation, anomaly detection using z-score analysis
- **AI Forecasting**: Time-series forecasting with confidence intervals and trend analysis
- **Risk Assessment**: Multi-metric risk scoring with configurable thresholds
- **Alert System**: Automated notifications for high-severity events

### Technical Stack
- **Frontend**: Next.js 13 with App Router, TypeScript, Tailwind CSS, React Query
- **Maps**: Leaflet with React-Leaflet for interactive mapping
- **Charts**: Recharts for time-series visualization and forecast ribbons
- **Tables**: TanStack Table for data management
- **Backend**: Node.js + Express with TypeScript, comprehensive API endpoints
- **Database**: PostgreSQL with time-series optimized schema
- **Authentication**: JWT for admin access, API keys for device ingestion
- **Validation**: Zod schemas throughout the stack
- **Monitoring**: Pino logger with structured logging
- **Jobs**: Node-cron for scheduled tasks (OpenAQ sync, forecasting, risk scoring)

## Quick Start

### Prerequisites
- Node.js 18+
- PostgreSQL database
- pnpm package manager

### Installation

1. **Clone and install dependencies**:
```bash
git clone <repository>
cd environmental-monitoring-dashboard
pnpm install
```

2. **Set up environment variables**:
```bash
# API Environment
cp apps/api/.env.example apps/api/.env
# Edit apps/api/.env with your database URL and secrets

# Web Environment  
cp apps/web/.env.local.example apps/web/.env.local
# Edit with your API URL
```

3. **Database setup**:
```bash
# Run migrations
pnpm migrate

# Seed with sample data
pnpm seed
```

4. **Start development servers**:
```bash
# Start both API and web servers
pnpm dev
```

The application will be available at:
- Web Dashboard: http://localhost:3000
- API Server: http://localhost:3001

### Default Credentials
- **Admin Login**: admin@example.com / admin123
- **Demo API Key**: Generated during seeding (check console output)

## API Endpoints

### Public Endpoints
- `GET /health` - System health check
- `GET /locations` - List all monitoring locations
- `GET /metrics/:locationId` - Get measurement data with anomaly detection
- `GET /forecast/:locationId` - Generate/retrieve forecasts
- `GET /risk/:locationId` - Current risk assessment
- `GET /alerts/:locationId` - Alert history

### Data Ingestion (API Key Required)
- `POST /ingest/iot` - Ingest IoT sensor data
- `POST /ingest/openaq/sync` - Sync OpenAQ air quality data

### Admin Endpoints (JWT Required)
- `POST /admin/login` - Admin authentication
- `POST /locations` - Create new monitoring location
- `GET /admin/thresholds` - Manage risk thresholds
- `POST /admin/thresholds` - Update risk thresholds
- `GET /admin/api-keys` - Manage API keys
- `POST /admin/api-keys` - Generate new API keys
- `POST /alerts/test` - Send test alerts

## Data Ingestion

### IoT Sensor Format
```json
{
  "device_id": "sensor_001",
  "measurements": [
    {
      "location_id": "uuid-here",
      "source": "iot_sensor",
      "ts": "2024-01-01T12:00:00Z",
      "pm25": 25.5,
      "pm10": 45.2,
      "no2": 30.1,
      "ph": 7.2,
      "noise_db": 65.5,
      "temperature": 22.3,
      "humidity": 68.2
    }
  ]
}
```

### Example cURL
```bash
curl -X POST http://localhost:3001/ingest/iot \
  -H "Content-Type: application/json" \
  -H "X-API-Key: your-api-key-here" \
  -d @sample-data.json
```

## Architecture

### Database Schema
- **locations**: Geographic monitoring points
- **measurements**: Time-series sensor data with comprehensive metrics
- **forecasts**: AI-generated predictions with confidence intervals
- **risk_events**: Calculated risk assessments and severity scores
- **alerts**: Notification records and delivery status
- **thresholds**: Configurable risk level definitions
- **api_keys**: Device authentication and access control
- **admins**: Administrative user accounts

### Risk Assessment
- **5-Level Severity Scale**: Normal (1) → Critical (5)
- **Multi-Metric Analysis**: PM2.5, PM10, NO2, O3, pH, noise, temperature
- **Configurable Thresholds**: Global and location-specific limits
- **Anomaly Detection**: Statistical z-score analysis (|z| > 3)
- **Automated Alerts**: Triggered for severity ≥ 4

### Forecasting System
- **Simple Persistence Model**: Baseline using moving averages and trend analysis
- **Confidence Intervals**: Dynamic uncertainty bounds
- **Multiple Horizons**: 1h, 6h, 24h predictions
- **Pluggable Architecture**: Ready for XGBoost/Prophet integration

## Scheduled Jobs

- **OpenAQ Sync**: Hourly air quality data import
- **Feature Updates**: 15-minute rolling statistics calculation  
- **Forecast Generation**: Hourly prediction updates
- **Risk Scoring**: 10-minute risk assessment and alerting

## Production Deployment

1. **Build the application**:
```bash
pnpm build
```

2. **Environment Configuration**:
- Set production database URL
- Configure JWT secrets
- Set up OpenAQ API key
- Configure alert channels (email/SMS)

3. **Database Migration**:
```bash
NODE_ENV=production pnpm migrate
```

4. **Process Management**:
- Use PM2 or similar for the API server
- Deploy frontend to Vercel/Netlify
- Set up reverse proxy (nginx)

## Development

### Project Structure
```
apps/
  api/          # Express.js backend
  web/          # Next.js frontend
packages/
  shared/       # Shared TypeScript types and schemas
```

### Key Commands
- `pnpm dev` - Start development servers
- `pnpm build` - Build for production
- `pnpm lint` - Code linting
- `pnpm test` - Run tests
- `pnpm migrate` - Run database migrations
- `pnpm seed` - Seed with sample data

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

## License

MIT License - see LICENSE file for details