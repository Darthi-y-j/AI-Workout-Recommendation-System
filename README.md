# AI Fitness & Goal Predictor | Professional Edition

A sophisticated, enterprise-grade fitness recommendation system powered by Google's Gemini AI. This single-page application provides personalized workout plans and predictive goal analysis for fitness professionals and their clients.

![Professional Dashboard](https://img.shields.io/badge/UI-Professional%20Dashboard-teal) ![AI Powered](https://img.shields.io/badge/AI-Gemini%202.5%20Flash-blue) ![Firebase](https://img.shields.io/badge/Database-Firebase-orange)

## 🎯 Features

### Core Functionality
- **AI-Powered Workout Generation**: Leverages Google Gemini 2.5 Flash to create personalized, structured daily workout routines
- **Predictive Goal Analysis**: Provides realistic, data-driven predictions for goal achievement timelines
- **Real-time BMI Calculation**: Automatic BMI computation as users input height and weight
- **Equipment-Aware Recommendations**: Customizes workout plans based on available equipment (Cable, Pin Loaded, Free Weights, Body Weight, Cardio Machines)

### Professional Dashboard
- **Executive Navigation Bar**: Status indicators, security badges, and export functionality
- **Hero Dashboard**: Real-time client statistics and goal tracking
- **Live Executive Snapshot**: Dynamic updates showing:
  - Active client information
  - Primary fitness goal
  - BMI synchronization
  - Activity rhythm tracking
- **Coach Intelligence Panel**: Advanced analytics including:
  - Readiness Score (0-100%) with visual progress bar
  - Weight Delta calculations
  - Strategy Focus recommendations based on goals and activity levels

### Data Persistence
- **Firebase Integration**: Automatic saving and retrieval of workout plans
- **Real-time Sync**: Last saved plan automatically loads on page refresh
- **Secure Authentication**: Supports both custom token and anonymous authentication
- **User-specific Data**: Plans stored in isolated user paths

### User Experience
- **Responsive Design**: Fully responsive layout for desktop, tablet, and mobile devices
- **Modern UI/UX**: Professional gradient backgrounds, glassmorphic cards, and smooth animations
- **Form Validation**: Comprehensive input validation with helpful error messages
- **Loading States**: Visual feedback during AI processing with retry logic (up to 3 attempts)

## 🛠️ Technologies

- **Frontend Framework**: Vanilla JavaScript (ES6 Modules)
- **Styling**: Tailwind CSS (CDN) + Custom CSS
- **AI Integration**: Google Gemini 2.5 Flash Preview API
- **Database**: Firebase Firestore
- **Authentication**: Firebase Auth (Anonymous/Custom Token)
- **Fonts**: Google Fonts (Inter)

## 📋 Prerequisites

- Modern web browser (Chrome, Firefox, Safari, Edge)
- Firebase project with Firestore enabled
- Google Gemini API key
- (Optional) Canvas environment for automatic configuration injection

## 🚀 Quick Start

### Option 1: Standalone Deployment

1. **Clone or download** the `AI Workout_Recommendation_System.html` file

2. **Configure Firebase**:
   - Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
   - Enable Firestore Database
   - Enable Anonymous Authentication (or set up Custom Token authentication)
   - Copy your Firebase configuration

3. **Update Firebase Configuration**:
   ```javascript
   // In the HTML file, locate the Firebase initialization section (around line 597)
   // Replace with your Firebase config:
   const firebaseConfig = {
       apiKey: "your-api-key",
       authDomain: "your-project.firebaseapp.com",
       projectId: "your-project-id",
       storageBucket: "your-project.appspot.com",
       messagingSenderId: "123456789",
       appId: "your-app-id"
   };
   ```

4. **Update Gemini API Key**:
   ```javascript
   // Locate the API key (around line 607)
   const apiKey = "your-gemini-api-key";
   ```

5. **Open the HTML file** in a web browser or deploy to a web server

### Option 2: Canvas Environment Integration

The application is designed to work with Canvas environments that inject configuration:

```javascript
// Expected global variables:
__app_id          // Application identifier
__firebase_config  // JSON string of Firebase configuration
__initial_auth_token  // Optional custom authentication token
```

## 📁 File Structure

```
project/
├── AI Workout_Recommendation_System.html  # Main application file
└── README.md                              # This file
```

## 🔧 Configuration

### Firebase Security Rules

Ensure your Firestore security rules allow authenticated users to read/write their own data:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/{appId}/users/{userId}/workout_plans/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### API Endpoints

- **Gemini API**: `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent`
- **Firestore Path**: `/artifacts/{appId}/users/{userId}/workout_plans/latest`

## 📖 Usage Guide

### For Fitness Professionals

1. **Client Intake**:
   - Enter client's full name
   - Input age, gender, height, current weight, and target weight
   - Select primary fitness goal (Weight Loss, Muscle Gain, Strength, Endurance)
   - Choose current activity level (Sedentary, Moderate, Active)
   - Select available equipment resources

2. **Review Live Metrics**:
   - Watch the Executive Snapshot update in real-time
   - Monitor Readiness Score as inputs change
   - Review Weight Delta and Strategy Focus recommendations

3. **Generate Plan**:
   - Click "ANALYZE AND GENERATE PLAN"
   - Wait for AI processing (typically 2-5 seconds)
   - Review the structured workout routine and predictive analysis

4. **Access Saved Plans**:
   - Previously generated plans automatically load on page refresh
   - Plans are saved per user session

### Input Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| Full Name | Text | Yes | Client's full name |
| Age | Number | Yes | Age in years (minimum 16) |
| Gender | Select | Yes | Male, Female, or Other |
| Height | Number | Yes | Height in centimeters (minimum 100) |
| Current Weight | Number | Yes | Current weight in kilograms (minimum 30) |
| Target Weight | Number | Yes | Target weight in kilograms (minimum 30) |
| Fitness Goal | Select | Yes | Primary fitness objective |
| Activity Level | Select | Yes | Current exercise frequency |
| Equipment | Checkboxes | No | Available equipment resources |

## 🏗️ Architecture

### Key Functions

#### `initFirebase()`
Initializes Firebase app, Firestore, and authentication. Supports both custom token and anonymous authentication.

#### `generateRecommendation(event)`
Main workflow function that:
- Collects form data
- Validates inputs
- Calls Gemini API with structured schema
- Implements retry logic (3 attempts with exponential backoff)
- Renders results and saves to Firestore

#### `renderPlan(data)`
Renders the AI-generated workout plan in a structured format with sections (Warm-up, Strength, Cardio, Cooldown, etc.)

#### `updateExecutiveSnapshot(snapshot)`
Updates the live dashboard with real-time client metrics:
- Client name and goal
- BMI synchronization
- Activity rhythm
- Readiness score calculation
- Weight delta computation
- Strategy focus derivation

#### `savePlanToFirestore(planData)`
Persists workout plans to Firestore with timestamp and user metadata.

#### `listenForLastPlan()`
Real-time listener that automatically loads the last saved plan when available.

### Data Flow

```
User Input → Form Validation → Executive Snapshot Update
                                      ↓
                            Gemini API Call (with retry)
                                      ↓
                            JSON Response Parsing
                                      ↓
                    Render Plan + Save to Firestore
                                      ↓
                            Real-time Sync Listener
```

## 🔐 Security Considerations

- **API Key**: Currently embedded in code. For production, consider:
  - Using environment variables
  - Implementing a backend proxy for API calls
  - Using Firebase Cloud Functions as an intermediary

- **Firebase Rules**: Ensure proper security rules are configured (see Configuration section)

- **Authentication**: Supports secure authentication methods (custom tokens recommended for production)

## 🎨 Customization

### Styling
The application uses CSS custom properties for easy theming:

```css
:root {
    --surface: #ffffff;
    --accent: #0f766e;
    --accent-soft: #22d3ee;
    --slate-ink: #0f172a;
    --muted-slate: #64748b;
    --bg-start: #eef5ff;
    --bg-end: #fef9f5;
}
```

### Gemini Prompt Customization
Modify the `userQuery` template in `generateRecommendation()` to adjust AI behavior and response format.

## 🐛 Troubleshooting

### Common Issues

**Firebase Connection Errors**
- Verify Firebase configuration is correct
- Check Firestore is enabled in Firebase Console
- Ensure authentication method is enabled

**API Errors**
- Verify Gemini API key is valid
- Check API quota limits
- Review browser console for detailed error messages

**Plan Not Loading**
- Check Firestore security rules
- Verify user authentication status
- Review browser console for Firestore errors

**BMI Not Calculating**
- Ensure both height and weight fields have valid numeric values
- Check browser console for JavaScript errors

## 📊 API Response Schema

The application expects the following JSON structure from Gemini:

```json
{
  "workout_plan": [
    {
      "section": "Warm-up",
      "items": [
        {
          "exercise": "Exercise Name",
          "details": "3 sets x 12 reps"
        }
      ]
    }
  ],
  "prediction": "A single professional sentence predicting goal achievement timeline."
}
```

## 🔄 Version History

- **v1.0** - Initial professional release with:
  - Executive dashboard interface
  - Real-time snapshot updates
  - Coach intelligence panel
  - Firebase persistence
  - Gemini AI integration

## 📝 License

This project is provided as-is for professional use. Ensure compliance with:
- Google Gemini API Terms of Service
- Firebase Terms of Service
- Any applicable data privacy regulations (GDPR, CCPA, etc.)

## 🤝 Contributing

This is a single-file application designed for easy deployment. For modifications:
1. Maintain the single-file structure
2. Test Firebase integration thoroughly
3. Verify responsive design across devices
4. Ensure API error handling remains robust

## 📞 Support

For issues or questions:
1. Check the Troubleshooting section
2. Review browser console for error messages
3. Verify Firebase and API configurations
4. Test with a fresh browser session

## 🎯 Future Enhancements

Potential improvements:
- Multi-week program generation
- Nutrition recommendations integration
- Progress tracking and analytics
- Export functionality (PDF, CSV)
- Multi-language support
- Advanced goal customization
- Integration with fitness wearables

---

**Built with precision for fitness professionals who demand excellence.**

