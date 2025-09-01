# Deployment Guide for Render

## Prerequisites
1. Code pushed to GitHub
2. Render account connected to GitHub

## Steps to Deploy on Render

### 1. Create Web Service
- Go to Render dashboard
- Click "New" → "Web Service"
- Connect your GitHub repository: `Gaurav560/backend_footage_flow`
- Choose branch: `master`

### 2. Configure Build & Deploy Settings
- **Name**: `footage-flow-backend`
- **Environment**: `Python 3`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `python app.py`

### 3. Set Environment Variables
Add these in Render dashboard → Environment tab:

```
GOOGLE_CLIENT_ID=724469503053-4hlt6hvsttage9ii33hn4n7l1j59tnef.apps.googleusercontent.com
GCP_PROJECT_ID=footage-flow-468712
GEMINI_API_KEY=AIzaSyCGON4hUzN2oHJAEOzSTSmFdXVs_UHFCNs
WHISPER_MODEL=base
WHISPER_LANGUAGE=en
UPLOAD_FOLDER=uploads/videos
MAX_CONTENT_LENGTH=500000000
FLASK_ENV=production
SECRET_KEY=a1654bf6b8d952823796bc1401c8171abd3d691404738a39ce1bf2c4996d7f3d
```

### 4. For Google Cloud Services (if needed)
Add the entire service account JSON as an environment variable:
```
GOOGLE_APPLICATION_CREDENTIALS_JSON={"type":"service_account","project_id":"footage-flow-468712",...}
```

### 5. Enable FFmpeg Buildpack
In Render dashboard → Settings → Build & Deploy:
- Add buildpack URL: `https://github.com/jonathanong/heroku-buildpack-ffmpeg-latest.git`

## Important Notes

1. **FFmpeg**: Will be installed automatically via buildpack
2. **File Storage**: Render has ephemeral storage - uploaded files are temporary
3. **Database**: SQLite will work but data may not persist between deploys
4. **Logs**: Check Render logs for any deployment issues

## Testing After Deployment
1. Check health endpoint: `https://your-app.onrender.com/health`
2. Test video upload functionality
3. Monitor logs for any FFmpeg-related issues

## Troubleshooting
- If FFmpeg not found: Check buildpack is properly configured
- If environment variables missing: Verify they're set in Render dashboard
- If Google services fail: Check credentials are properly configured
