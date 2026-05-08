# Interview Preparator AI

A browser-based mock interview coach that provides instant feedback on your delivery, body language, and answer structure using AI-powered analysis.
Built in under 24 hours at HackUTA 2025.

## 🎯 Features

- **Real-time Video Analysis**: Uses MediaPipe Face Mesh to track head movement and attention
- **Speech Recognition**: Transcribes your answers in real-time using the Web Speech API
- **STAR Framework Analysis**: Evaluates answer structure (Situation, Task, Action, Result)
- **Speaking Metrics**: Measures pace (WPM), filler words, and provides actionable feedback
- **🆕 Answer Relevance Scoring**: AI-powered keyword analysis determines if your answer is on-topic (0-100 score)
- **🆕 Auto-End Detection**: Intelligent silence detection and wrap-up phrase recognition
- **🆕 Auto-Advance to Next Question**: Seamlessly practice multiple questions with configurable auto-advance
- **🆕 Keyboard Shortcuts**: Space to start/stop, N to skip/next question
- **Instant Reports**: Get comprehensive feedback immediately after each practice session
- **Export Results**: Download your session data as JSON or CSV

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ and npm
- Modern browser with HTTPS support (Chrome, Edge, or Safari recommended)
- Camera and microphone access

### Installation

```bash
# Clone or download this repository
cd interview-preparator-ai

# Install client dependencies
cd client
npm install

# (Optional) Install server dependencies
cd ../server
npm install
```

### Running the Application

**Frontend Only (Recommended for MVP):**

```bash
cd client
npm run dev
```

Open https://localhost:5173 in your browser (HTTPS required for camera access).

**With Backend (Optional):**

Terminal 1 - Frontend:
```bash
cd client
npm run dev
```

Terminal 2 - Backend:
```bash
cd server
npm run dev
```

## 🔒 HTTPS Requirement

**CRITICAL**: The browser's `getUserMedia` API requires HTTPS in production. For local development:

- Vite dev server provides HTTPS by default on localhost
- For production, deploy behind a reverse proxy with SSL (nginx, Cloudflare, etc.)
- Or use services like Vercel/Netlify that provide automatic HTTPS

## 🌐 Browser Support

### Required Features
- **getUserMedia API**: Chrome 53+, Firefox 36+, Safari 11+, Edge 12+
- **Web Speech Recognition**: Chrome 25+, Edge 79+, Safari 14.1+

### Compatibility Notes
- **Firefox**: No native Speech Recognition support (transcript will not be captured)
- **Safari**: iOS Safari requires user gesture to start media capture
- **Best Experience**: Chrome/Edge on desktop

If a browser doesn't support a feature, the app will show a warning and gracefully degrade.

## 📁 Project Structure

```
interview-preparator-ai/
├── client/                  # React frontend
│   ├── src/
│   │   ├── components/     # UI components
│   │   │   ├── CameraPreview.tsx
│   │   │   ├── LandmarkOverlay.tsx
│   │   │   ├── Timer.tsx
│   │   │   ├── MetricCard.tsx
│   │   │   ├── StarBars.tsx
│   │   │   ├── RecorderControls.tsx
│   │   │   ├── QuestionPicker.tsx
│   │   │   └── FeedbackPanel.tsx
│   │   ├── features/       # Business logic
│   │   │   ├── capture/    # getUserMedia
│   │   │   ├── speech/     # Speech Recognition
│   │   │   ├── landmarks/  # MediaPipe Face Mesh
│   │   │   ├── analysis/   # Text, STAR, attention
│   │   │   └── state/      # Zustand store
│   │   ├── pages/          # Route pages
│   │   │   ├── Home.tsx
│   │   │   ├── Session.tsx
│   │   │   └── Report.tsx
│   │   └── lib/            # Utilities & data
│   │       ├── questions.ts
│   │       └── utils.ts
│   └── package.json
├── server/                  # Optional Express backend
│   ├── routes/
│   │   ├── transcribe.ts   # Whisper integration stub
│   │   └── score.ts        # LLM scoring stub
│   └── index.ts
├── .env.example
└── README.md
```

## 🎓 How It Works

### 1. Session Start
- Select your role (Frontend/Backend/Data Engineer)
- Choose answer duration (1-5 minutes)
- Grant camera and microphone permissions

### 2. Recording
- Random behavioral question is displayed
- Live video shows your face with landmark tracking overlay
- Speech is transcribed in real-time
- Attention and movement metrics are computed

### 3. Analysis
- **Text Analysis**: Word count, WPM, filler words
- **STAR Scoring**: Detects Situation, Task, Action, Result components
- **Attention Metrics**: Head stability, gaze tracking, movement rating

### 4. Report
- View all metrics with color-coded performance indicators
- Read actionable suggestions for improvement
- Review full transcript with highlights
- Export data for tracking progress over time

## 🧪 Analysis Methods

### Speaking Pace
- **Target**: 110-150 words per minute
- **Calculation**: Total words / duration in minutes
- **Rating**: Slow (<110), Good (110-150), Fast (>150)

### Filler Words
- Detects: "um", "uh", "like", "you know", "sort of", etc.
- Reports count and rate per minute
- Suggests pausing instead of using fillers

### STAR Framework
- **Keyword-based detection** for each component:
  - **S**ituation: "project", "working on", "context"
  - **T**ask: "challenge", "needed to", "objective"
  - **A**ction: "I did", "implemented", "my approach"
  - **R**esult: "achieved", "improved", "saved", metrics
- Scores each component 0-100
- Highlights missing sections

### Attention & Body Language
- Uses MediaPipe Face Mesh landmarks
- Calculates head pose (yaw, pitch, roll) from keypoints
- Tracks variance and stability over time
- Measures gaze drift from baseline
- Combines into 0-100 attention score

## 🔧 Configuration

### Environment Variables

Copy `.env.example` to `.env`:

```bash
# Use backend API for transcription
VITE_USE_BACKEND=false

# Backend URL
API_BASE_URL=http://localhost:3001

# OpenAI API Key (for Whisper transcription)
OPENAI_API_KEY=your_key_here
```

### Question Bank

Edit `client/src/lib/questions.ts` to customize questions:

```typescript
export const QUESTIONS = {
  frontend_react: [
    { id: 'fr1', q: 'Your custom question here...' },
  ],
  // Add more roles/skills
};
```

## 🚢 Deployment

### Frontend (Static Hosting)

```bash
cd client
npm run build
```

Deploy the `dist/` folder to:
- **Vercel**: `vercel deploy`
- **Netlify**: Drag & drop `dist/` folder
- **GitHub Pages**: Configure with SSL

### Backend (Optional)

```bash
cd server
npm run build
```

Deploy to:
- **Railway**: `railway up`
- **Render**: Connect GitHub repo
- **Heroku**: `heroku create && git push heroku main`

## 🧩 Extending the Project

### Add Whisper Transcription

1. Install OpenAI SDK:
```bash
cd server
npm install openai
```

2. Set API key in `.env`:
```
OPENAI_API_KEY=sk-...
```

3. Uncomment Whisper code in `server/routes/transcribe.ts`

4. Enable in client `.env`:
```
VITE_USE_BACKEND=true
```

### Add LLM-Based Feedback

1. Uncomment code in `server/routes/score.ts`
2. Configure GPT-4 prompts for role-specific evaluation
3. Update client to call `/api/score` endpoint

### Store Past Sessions

1. Add IndexedDB integration in client
2. Create history page to review past sessions
3. Add progress tracking over time

### Advanced Analytics

- Emotion detection from facial expressions
- Speaking tone/confidence analysis
- Question difficulty levels (junior/mid/senior)
- Comparison with high-performing answers

## 🐛 Troubleshooting

### Camera Not Working
- Ensure HTTPS is enabled
- Check browser permissions
- Try a different browser (Chrome recommended)

### No Transcript Appearing
- Speech Recognition only works in Chrome/Edge/Safari 14.1+
- Check microphone permissions
- Speak clearly and at normal pace
- Consider using backend Whisper as fallback

### Face Tracking Laggy
- MediaPipe runs on CPU by default
- Reduce video resolution in `useCapture.ts`
- Close other tabs/apps
- Use a more powerful device

### Build Errors
- Clear node_modules and reinstall
- Check Node.js version (18+ required)
- Ensure all dependencies match package.json

## 📊 Testing

```bash
# Type checking
cd client
npm run typecheck

# Linting
npm run lint

# Unit tests (if added)
npm test
```

## 🤝 Contributing

This is an MVP implementation. Contributions welcome:

1. Fork the repository
2. Create a feature branch
3. Make your changes with tests
4. Submit a pull request

## 📄 License

MIT License - feel free to use this project for learning or commercial purposes.

## 🙏 Acknowledgments

- **MediaPipe** for face mesh tracking
- **Web Speech API** for browser-based transcription
- **React** and **Vite** for fast development
- **TailwindCSS** for styling
- **Zustand** for state management

## 📞 Support

For issues or questions:
- Check browser console for errors
- Review troubleshooting section
- Open an issue with reproduction steps

---

**Happy interviewing! 🎤✨**
