# Use-What-You-Have Makeup Coach - High Level Architecture Diagram

## ONE PAGE COMPLETE APP FLOW

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                 │
│                    USE-WHAT-YOU-HAVE MAKEUP COACH - COMPLETE SYSTEM FLOW                       │
│                                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘


                                         ┌─────────────────────┐
                                         │   USER (Frontend)   │
                                         │   Web/Mobile App    │
                                         └──────────┬──────────┘
                                                    │
                                    ┌───────────────┼───────────────┐
                                    │               │               │
                            ┌───────▼────────┐ ┌──▼────────┐ ┌────▼────────┐
                            │ 1. ONBOARDING  │ │ 2. CAMERA │ │ 3. TUTORIAL │
                            │ - Signup/Login │ │ - Capture │ │ - Learn     │
                            │ - Preferences  │ │ - Upload  │ │ - Library   │
                            │ - Skin Tone    │ │ - Process │ │ - Tips      │
                            └────────────────┘ └──┬────────┘ └─────────────┘
                                                   │
                                    ┌──────────────┼──────────────┐
                                    │                             │
                          ┌─────────▼────────────┐      ┌────────▼─────────┐
                          │  AI/ML PROCESSING    │      │  APP BACKEND     │
                          │  ┌────────────────┐  │      │  ┌────────────┐  │
                          │  │ Face Detection │  │      │  │ API Server │  │
                          │  ├────────────────┤  │      │  │ (FastAPI)  │  │
                          │  │ Skin Analysis  │  │      │  └────────────┘  │
                          │  ├────────────────┤  │      │  ┌────────────┐  │
                          │  │ Undertone ID   │  │      │  │ Auth Mgmt  │  │
                          │  ├────────────────┤  │      │  │ (JWT)      │  │
                          │  │ Color Harmony  │  │      │  └────────────┘  │
                          │  ├────────────────┤  │      │  ┌────────────┐  │
                          │  │ Face Shape     │  │      │  │ User Svc   │  │
                          │  └────────────────┘  │      │  │ (Profiles) │  │
                          └──────────┬───────────┘      │  └────────────┘  │
                                     │                 └────────┬──────────┘
                                     │                          │
                          ┌──────────▼──────────┐              │
                          │ RECOMMENDATIONS      │              │
                          │ Generate 3-5 looks  │              │
                          │ with:               │              │
                          │ - Product list      │              │
                          │ - Steps (8-12)      │              │
                          │ - Alternatives      │              │
                          │ - Confidence level  │              │
                          └──────────┬──────────┘              │
                                     │                        │
                    ┌────────────────┼────────────────┐       │
                    │                │                │       │
            ┌───────▼──────┐  ┌──────▼──────┐  ┌────▼───┐   │
            │ 4. RESULTS   │  │ 5. SAVE     │  │ 6. BUY │   │
            │ - Show Looks │  │ - Bookmark  │  │ - Shop │   │
            │ - Try-on AR  │  │ - History   │  │ - Cart │   │
            │ - Share      │  │ - Library   │  │ - Order│   │
            └──────┬───────┘  └─────┬──────┘  └────┬───┘   │
                   │                │              │        │
                   └────────────────┼──────────────┘        │
                                    │                       │
                          ┌─────────▼────────┐             │
                          │  DATA STORAGE    │             │
                          │  & SERVICES      │◄────────────┘
                          │  ┌────────────┐  │
                          │  │ PostgreSQL │  │
                          │  │ - Users    │  │
                          │  │ - Looks    │  │
                          │  │ - Feedback │  │
                          │  └────────────┘  │
                          │  ┌────────────┐  │
                          │  │ MongoDB    │  │
                          │  │ - Analytics│  │
                          │  │ - Cache    │  │
                          │  └────────────┘  │
                          └──────────────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
            ┌───────▼──────┐  ┌────▼────────┐ ┌──▼──────────┐
            │ PARTNERSHIPS │  │ CONTENT CDN  │ │ MONITORING  │
            │ - Sephora    │  │ - Videos     │ │ - Analytics │
            │ - Ulta       │  │ - Tutorials  │ │ - Errors    │
            │ - Affiliate  │  │ - Images     │ │ - Perf      │
            └──────────────┘  └─────────────┘ └─────────────┘
```

---

## STEP-BY-STEP USER JOURNEY

### STEP 1: User Opens App
- **What happens**: User sees onboarding screen
- **What the app does**: Loads from GitHub repo (frontend)
- **Tech**: React/Vue frontend served via AWS CloudFront CDN

### STEP 2: User Signs Up / Logs In
- **What happens**: User enters email/password
- **What the app does**: 
  - Validates credentials
  - Creates JWT token
  - Stores user preferences (skin tone, face shape, etc.)
- **Tech**: AWS Cognito for authentication, PostgreSQL for user data

### STEP 3: User Takes a Photo
- **What happens**: User opens camera and takes selfie
- **What the app does**:
  - Sends image to backend API
  - Runs facial recognition (AWS SageMaker)
  - Extracts: face shape, skin tone, undertones
  - Analyzes makeup requirements
- **Tech**: Python FastAPI backend, TensorFlow model for analysis

### STEP 4: AI Generates Recommendations
- **What happens**: App shows 3-5 makeup look options
- **What the app does**:
  - Compares user's face with makeup database
  - Generates product recommendations
  - Creates step-by-step tutorial
  - Calculates confidence scores
- **Tech**: Machine learning model (TensorFlow/PyTorch)

### STEP 5: User Sees Results
- **What happens**: Shows makeup looks with products & instructions
- **What the app does**:
  - Renders AR preview (try-on)
  - Shows similar looks
  - Displays product links
  - Allows sharing to social media
- **Tech**: WebGL for AR, social media API integration

### STEP 6: User Saves or Purchases
- **What happens**: User can save look or buy products
- **What the app does**:
  - Saves to user profile (MongoDB)
  - Tracks for personalization
  - Routes to affiliate link for products
- **Tech**: MongoDB for quick storage, Affiliate API for e-commerce

---

## DATA FLOW DIAGRAM

```
┌──────────────┐
│ User Photo   │
└──────┬───────┘
       │
       ▼ (Encrypted HTTPS)
┌─────────────────────┐
│ API Gateway         │
│ (Rate Limiting)     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Authentication      │
│ (JWT Validation)    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐         ┌──────────────────┐
│ Makeup Service      │────────▶│ SageMaker Model  │
│ (FastAPI)           │         │ (AI Inference)   │
└──────────┬──────────┘         └──────────────────┘
           │
           ▼
┌──────────────────────────┐
│ Database Queries         │
│ - Get user preferences   │
│ - Fetch product database │
│ - Store results          │
└──────────┬───────────────┘
           │
           ▼
┌─────────────────────┐
│ Response Builder    │
│ Format JSON         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Send to Frontend    │
│ (Encrypted HTTPS)   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ User Sees Results   │
└─────────────────────┘
```

---

## KEY COMPONENTS SUMMARY

| Component | Purpose | Technology |
|-----------|---------|-----------|
| **Frontend** | User interface | React/Vue, HTML/CSS, JavaScript |
| **Authentication** | Secure login | AWS Cognito, JWT tokens |
| **Backend API** | Process requests | Python FastAPI, Node.js |
| **AI/ML Model** | Analyze faces | TensorFlow, PyTorch, AWS SageMaker |
| **Database** | Store data | PostgreSQL (users), MongoDB (looks) |
| **Cache** | Fast access | Redis |
| **CDN** | Deliver content | AWS CloudFront |
| **Storage** | Files/images | AWS S3 |
| **Monitoring** | Track health | CloudWatch, Datadog |

---

## APP WORKFLOW IN 60 SECONDS

1. **User uploads photo** → 
2. **AI analyzes face** (skin tone, shape, features) → 
3. **Algorithm matches face to makeup styles** → 
4. **Pulls recommended products from database** → 
5. **Generates 3-5 personalized looks** → 
6. **Shows step-by-step tutorial** → 
7. **User saves, shares, or buys** → 
8. **App learns & improves recommendations**

---

## KEY FEATURES AT A GLANCE

✅ **Real-time Processing**: Face analysis in <2 seconds
✅ **Personalized**: Learns from user preferences
✅ **Multiple Looks**: 3-5 options per session
✅ **Step-by-Step**: Easy-to-follow tutorials
✅ **Product Integration**: Links to buy makeup
✅ **AR Try-On**: Virtual makeup preview
✅ **Social Sharing**: Share looks with friends
✅ **History Tracking**: Save favorite looks
✅ **Inclusive**: Works for all skin tones
✅ **Scalable**: Handles 100k+ concurrent users

---

**This diagram shows the complete flow from user opening the app to getting personalized makeup recommendations!**
