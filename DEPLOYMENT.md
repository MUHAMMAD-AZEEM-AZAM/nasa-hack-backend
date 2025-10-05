# Heroku Deployment Guide

## Files Created for Deployment

1. **`Procfile`** - Tells Heroku how to run your app
2. **`runtime.txt`** - Specifies Python version
3. **`app.json`** - App configuration for Heroku
4. **`requirements.txt`** - Python dependencies (already exists)
5. **`env_example.txt`** - Environment variables template

## Deployment Steps

### 1. Set up Heroku CLI
```bash
# Install Heroku CLI if not already installed
# Download from: https://devcenter.heroku.com/articles/heroku-cli

# Login to Heroku
heroku login
```

### 2. Create Heroku App
```bash
# Navigate to backend directory
cd backend

# Create a new Heroku app
heroku create your-app-name

# Or if you already have an app
heroku git:remote -a your-existing-app-name
```

### 3. Set Environment Variables
```bash
# Set your API key
heroku config:set AIMLAPI_KEY="your_actual_aimlapi_key"

# Set MongoDB connection string
heroku config:set MONGODB_URL="your_mongodb_atlas_connection_string"

# Set database name (optional, defaults to nasa_hackathon)
heroku config:set MONGODB_DB_NAME="nasa_hackathon"

# Set collection name (optional, defaults to organism_data)
heroku config:set MONGODB_COLLECTION_NAME="organism_data"
```

### 4. Deploy
```bash
# Add all files
git add .

# Commit changes
git commit -m "Add Heroku deployment files"

# Deploy to Heroku
git push heroku main

# Or if using master branch
git push heroku master
```

### 5. Check Deployment
```bash
# Check app status
heroku ps

# View logs
heroku logs --tail

# Open your app in browser
heroku open
```

## Troubleshooting

### Common Issues:

1. **H14 Error (No web processes running)**
   - ✅ **Fixed**: Created `Procfile` with correct web process command

2. **Build Failures**
   - Check `requirements.txt` has all dependencies
   - Ensure Python version in `runtime.txt` is supported

3. **Environment Variables**
   - Make sure all required env vars are set in Heroku
   - Check with: `heroku config`

4. **Port Issues**
   - ✅ **Fixed**: Using `$PORT` environment variable in Procfile

### Useful Commands:

```bash
# Check environment variables
heroku config

# View app logs
heroku logs --tail

# Restart app
heroku restart

# Scale web dynos
heroku ps:scale web=1

# Check app status
heroku ps

# Open app in browser
heroku open

# Access app shell
heroku run bash
```

## Environment Variables Required:

- `AIMLAPI_KEY` - Your AI/ML API key
- `MONGODB_URL` - MongoDB Atlas connection string
- `MONGODB_DB_NAME` - Database name (optional)
- `MONGODB_COLLECTION_NAME` - Collection name (optional)

## Testing Your Deployment:

1. **Health Check**: `https://your-app.herokuapp.com/health`
2. **Root Endpoint**: `https://your-app.herokuapp.com/`
3. **Test LLM**: `https://your-app.herokuapp.com/test-llm`
4. **Search API**: POST to `https://your-app.herokuapp.com/search`

## Frontend Integration:

Update your frontend API URL in `src/lib/api.ts`:
```typescript
const API_BASE_URL = 'https://your-app.herokuapp.com';
```

## Cost Considerations:

- **Free Tier**: Limited dyno hours (550 hours/month)
- **Basic Tier**: $7/month for always-on dyno
- **Hobby Tier**: $5/month for basic features

For production use, consider upgrading to Basic or Hobby tier to avoid sleep issues.
