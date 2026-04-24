# MyHealthPassport

MyHealthPassport is an Android application built with Jetpack Compose that acts as a comprehensive digital health assistant. It securely stores and manages patients' medical data, provides AI-powered health analysis, supports voice interaction, visualizes health trends, and offers emergency assistance — all in one place.



---

## Table of Contents

- [Purpose & Goals](#purpose--goals)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Authentication](#authentication)
- [Key Features](#key-features)
- [How to Run](#how-to-run)
- [Contributing](#contributing)
- [License](#license)

---

## Purpose & Goals

MyHealthPassport aims to:

- Give users a **single secure place** to store, access, and manage their personal medical data via a unique Medical ID.
- Leverage **AI agents** (Gemini & Mistral) to analyze symptoms, interpret medical reports, and generate personalized diet/exercise plans.
- Provide **visual health analytics** (blood pressure trends, blood sugar trends, medication usage) with AI-generated insights.
- Enable **voice-based interaction** so users can engage hands-free with health features.
- Offer **emergency contact access** and a **home screen widget** for quick health data visibility.
- Empower patients with actionable, AI-driven health recommendations without replacing professional medical advice.

---

## Project Structure

```
MyHealthPassport/
├── app/
│   └── src/
│       └── main/
│           ├── AndroidManifest.xml
│           └── java/com/example/myhealthpassport/
│               │
│               ├── MainActivity.kt               # App entry point, speech input, WorkManager setup
│               ├── Constants.kt                  # App-wide constants (API keys, config)
│               │
│               ├── auth/                         # Authentication layer
│               │   ├── AuthComponents.kt         # Reusable auth UI components
│               │   ├── GoogleSignInUtils.kt      # Google Sign-In helper
│               │   ├── LogInScreen.kt            # Login screen logic
│               │   └── SignUpScreen.kt           # Sign-up screen logic
│               │
│               ├── data/
│               │   ├── api/                      # Network layer
│               │   │   ├── AgentInstance.kt      # Retrofit instance for Mistral API
│               │   │   └── MistralAgentApi.kt    # Mistral API interface
│               │   └── database/                 # Room local database
│               │       ├── Entity.kt             # Room entity (EmergencyContact)
│               │       ├── EmergencyContactDao.kt
│               │       ├── EmergencyContactDatabase.kt
│               │       ├── EmergencyContactRepository.kt
│               │       ├── EmergencyContactViewModel.kt
│               │       └── EmergencyContactViewModelFactory.kt
│               │
│               ├── domain/
│               │   ├── mistralModel/             # Mistral API request/response models
│               │   │   ├── Choice.kt
│               │   │   ├── Message.kt
│               │   │   ├── MistralMessage.kt
│               │   │   ├── MistralRequest.kt
│               │   │   └── MistralResponse.kt
│               │   └── model/
│               │       └── UserHealthData.kt     # Core health data model
│               │
│               ├── ui/
│               │   ├── components/
│               │   │   └── FlipAnimation.kt      # Card flip animation component
│               │   ├── composables/              # All screens as Composables
│               │   │   ├── SplashScreen.kt       # Animated splash screen
│               │   │   ├── SignInScreen.kt        # Sign-in UI
│               │   │   ├── SignUpScreen.kt        # Sign-up UI
│               │   │   ├── NavigationDrawer.kt   # Side navigation drawer
│               │   │   ├── HealthInfo.kt         # Save/update Medical ID & health data
│               │   │   ├── GetHealthInfo.kt      # Retrieve health data by Medical ID
│               │   │   ├── PatientDetails.kt     # Display full patient profile
│               │   │   ├── HealthAiScreen.kt     # Gemini AI symptom checker (voice + text)
│               │   │   ├── AgentScreen.kt        # Mistral AI agent (diet & exercise plans)
│               │   │   ├── ChartScreen.kt        # Health analytics charts + AI insights
│               │   │   ├── EmergencyContacts.kt  # Emergency contacts list
│               │   │   ├── EmergencyContactsDataClass.kt
│               │   │   └── UiState.kt            # UI state sealed class
│               │   ├── navigation/
│               │   │   ├── NavGraph.kt           # Navigation host with all routes
│               │   │   └── Screen.kt             # Sealed class defining all routes
│               │   └── theme/
│               │       ├── Color.kt
│               │       ├── Theme.kt
│               │       └── Type.kt
│               │
│               ├── viewmodels/
│               │   ├── AiViewModel.kt            # Gemini AI state & logic
│               │   ├── AgentViewModel.kt         # Mistral agent state & logic
│               │   └── HealthViewModel.kt        # Health data CRUD with Firestore
│               │
│               └── widget/
│                   ├── MyHealthPassportWidget.kt # Glance AppWidget UI
│                   ├── HealthWidgetReceiver.kt   # Widget broadcast receiver
│                   └── HealthDataWorker.kt       # WorkManager periodic data sync
│
├── gradle/
│   └── libs.versions.toml                        # Centralized dependency versions
├── build.gradle.kts                              # Project-level Gradle config
└── app/build.gradle.kts                          # App-level Gradle config
```

### Navigation Routes

| Route | Screen | Description |
|---|---|---|
| `splash` | SplashScreen | Animated entry, redirects based on auth state |
| `signup` | SignUpScreen | Email/password & Google registration |
| `login` | SignInScreen | Email/password & Google login |
| `health_info` | HealthInfo | Create/update Medical ID and health data |
| `get_health_info` | GetHealthInfo | Retrieve health data by Medical ID |
| `patient_details/{patientData}` | PatientDetails | Full patient profile view |
| `health_ai_screen` | HealthAiScreen | Gemini AI symptom checker with voice |
| `agent_screen` | AgentScreen | Mistral AI diet & exercise planner |
| `chart_screen` | ChartScreen | Health trend charts with AI insights |
| `emergency_contacts` | EmergencyContacts | Emergency contacts list |
| `flip_animation` | FlipAnimation | Medical ID card flip view |

---

## Tech Stack

| Category | Technology | Version | Purpose |
|---|---|---|---|
| **Language** | Kotlin | 2.1.0 | Primary development language |
| **UI** | Jetpack Compose | BOM 2024.04.01 | Declarative UI framework |
| **UI** | Material 3 | via BOM | Design system & components |
| **UI** | Lottie Compose | 6.4.0 | Animated splash & loading screens |
| **Navigation** | Navigation Compose | 2.8.6 | In-app navigation with animated transitions |
| **AI** | Gemini AI (Google Generative AI) | 0.9.0 | Symptom checker & health chart insights |
| **AI** | Mistral AI (via REST API) | — | Personalized diet & exercise agent |
| **Backend** | Firebase Authentication | 23.0.0 | User auth (email/password + Google) |
| **Backend** | Firebase Cloud Firestore | 25.0.0 | Cloud storage for medical data |
| **Backend** | Firebase Storage | 21.0.0 | File/image storage |
| **Backend** | Firebase Crashlytics | 19.0.3 | Crash reporting |
| **Local DB** | Room | 2.5.0 | Local storage for emergency contacts |
| **Networking** | Retrofit | 2.11.0 | HTTP client for Mistral API |
| **Networking** | OkHttp | 4.12.0 | HTTP interceptor & logging |
| **Networking** | Gson Converter | 2.11.0 | JSON serialization/deserialization |
| **Image Loading** | Coil Compose | 2.7.0 | Async image loading |
| **Charts** | YCharts | 2.1.0 | Line charts & pie charts for health data |
| **Widget** | Glance AppWidget | 1.2.0-alpha01 | Home screen health widget |
| **Background** | WorkManager | 2.10.3 | Periodic widget data sync (every 15 min) |
| **Voice** | Android SpeechRecognizer | Android SDK | Voice input for AI interactions |
| **Credentials** | AndroidX Credentials | 1.5.0 | Google Sign-In credential management |
| **Build** | Gradle KTS + KSP | 8.10.0 / 2.1.0-1.0.29 | Build system & annotation processing |
| **Secrets** | Secrets Gradle Plugin | 2.0.1 | Secure API key management via local.properties |

---

## Authentication

MyHealthPassport uses **Firebase Authentication** with two sign-in methods:

### Email & Password
- Users register with email and password via `SignUpScreen`.
- Firebase handles credential validation, secure storage, and session management.
- Login is handled via `SignInScreen` using `FirebaseAuth.signInWithEmailAndPassword`.

### Google Sign-In
- Implemented via `GoogleSignInUtils.kt` using the **AndroidX Credentials API** (`androidx.credentials`) combined with `GoogleIdTokenRequestOptions`.
- On successful Google authentication, the ID token is exchanged for a Firebase credential using `GoogleAuthProvider.getCredential`, then signed in via `FirebaseAuth`.
- This approach uses the modern Credential Manager API instead of the deprecated `GoogleSignInClient`.

### Session Persistence
- `FirebaseAuth.getInstance().currentUser` is checked on the `SplashScreen` to determine whether to route the user to login or directly to the home screen.
- Firebase automatically persists the auth session across app restarts.

### Data Storage Post-Auth
- After authentication, user health data is stored in **Cloud Firestore** under a document keyed by the user's unique Medical ID.
- The `HealthViewModel` handles all Firestore read/write operations.

---

## Key Features

- **Medical ID System** — Create and update a personal Medical ID that stores health details (blood type, allergies, medications, conditions, emergency contacts) in Firestore.
- **AI Symptom Checker** — Powered by Gemini AI with voice input support via Android's `SpeechRecognizer`. Users can speak or type symptoms and receive AI analysis.
- **Mistral AI Agent** — Calls the Mistral API (`https://api.mistral.ai/`) via Retrofit to generate personalized diet and exercise plans based on the user's stored health data.
- **Health Analytics Charts** — YCharts-powered line charts for blood pressure and blood sugar trends, and a pie chart for medication usage. Each chart has an "Insights" button that sends the data to Gemini AI for detailed analysis.
- **Emergency Contacts** — Stored locally in a Room database. Supports add, view, and delete operations.
- **Home Screen Widget** — A Glance AppWidget that displays key health data, refreshed every 15 minutes via WorkManager.
- **Navigation Drawer** — A side drawer wrapping all authenticated screens for easy navigation.
- **Flip Animation** — A card flip animation to reveal the Medical ID card details.

---

## How to Run

### Prerequisites

- Android Studio Hedgehog or later
- Android device or emulator running API 26 (Android 8.0) or higher
- A Firebase project with Authentication and Firestore enabled
- A Gemini API key from [Google AI Studio](https://aistudio.google.com/)
- A Mistral API key from [Mistral AI](https://console.mistral.ai/)

### Setup Steps

**1. Clone the repository**
```bash
git clone https://github.com/anuragkanojiya1/MyHealthPassport.git
cd MyHealthPassport
```

**2. Connect Firebase**
- Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
- Enable **Authentication** (Email/Password and Google providers).
- Enable **Cloud Firestore** in test or production mode.
- Download the `google-services.json` file and place it at `app/google-services.json`.

**3. Add API Keys**

Add the following to your `local.properties` file (never commit this file):
```properties
GEMINI_API_KEY=your_gemini_api_key_here
MISTRAL_API_KEY=your_mistral_api_key_here
```

The Secrets Gradle Plugin will expose these as `BuildConfig` fields automatically.

**4. Open in Android Studio**
- Open the project root folder in Android Studio.
- Let Gradle sync complete.

**5. Build & Run**
```bash
# From Android Studio: Run > Run 'app'
# Or via terminal:
./gradlew assembleDebug
```
- Deploy to an emulator (API 26+) or a physical Android device.

### Build Variants

| Variant | Minify | Shrink Resources | Signing |
|---|---|---|---|
| `debug` | No | No | Debug key |
| `release` | Yes (ProGuard) | Yes | Release keystore via env vars |

For a release build, set the following environment variables:
```
KEYSTORE_PASSWORD=...
KEY_ALIAS=...
KEY_PASSWORD=...
```

---

## Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request describing your changes.

For major changes, open an issue first to discuss the proposal.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
