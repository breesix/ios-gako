# Gako

**AI Powered Special Education Progress Tracker**

An iOS application that reimagines special education documentation by transforming natural speech into structured student progress reports. Built with Swift, SwiftUI, and powered by OpenAI GPT-4o mini.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Implementation Details](#implementation-details)
- [Getting Started](#getting-started)
- [Dependencies](#dependencies)
- [Design Patterns](#design-patterns)
- [Contributors](#contributors)

---

## Overview

In the world of special education, tracking daily progress is mandatory and crucial for every student's development. However, the traditional workflow is disconnected and exhausting. Teachers are forced to manually write detailed reports for each student individually, a time-consuming process that often leads to administrative burnout and takes valuable energy away from what matters most: interacting with the students.

**Gako** addresses this challenge by providing:

- Voice-to-text input that turns natural speech into structured data
- AI-powered classification that automatically organizes insights into student-specific sections
- Seamless parent communication with automatically generated and shared reports
- Intuitive progress tracking across activities and notes

The result is a seamless ecosystem where teachers can simply speak freely about their day, and the AI engine listens to the narrative, understands the context, and transforms scattered observations into organized, professional records without any extra effort.

---

## Key Features

### Voice-Powered Documentation
Powered by Apple Speech Recognition framework, teachers can speak naturally about their observations. The app listens in real-time and converts speech to text, eliminating the need for manual typing.

### AI-Generated Summaries
Leveraging OpenAI GPT-4o mini model, Gako automatically generates concise, professional summaries for each student based on their daily activities and notes, focusing on development milestones and areas needing attention.

### Student Progress Tracking
Comprehensive tracking of each student's activities with status indicators (independent/assisted), allowing teachers to monitor development patterns over time.

### Daily Notes & Observations
Flexible note-taking system that captures behavioral observations, achievements, and important events throughout the school day.

### Activity Management
Visual activity tracker to log daily tasks, monitor completion status, and identify learning patterns for each student.

### Personalized Onboarding
Guided onboarding flow to help teachers set up their classroom and understand the app's voice-to-text documentation workflow.

---

## Tech Stack

### Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Swift | 5.0+ | Primary programming language |
| SwiftUI | iOS 17+ | Declarative UI framework |
| SwiftData | iOS 17+ | Persistent data storage |
| Xcode | 16.0+ | Development environment |

### AI Integration

| Service | Purpose |
|---------|---------|
| [OpenAI Swift](https://github.com/MacPaw/OpenAI) | GPT-4o mini integration for AI summary generation |

### Apple Frameworks

| Framework | Purpose |
|-----------|---------|
| Speech | Real-time speech recognition for voice input |
| AVFoundation | Audio session management for recording |
| Foundation | Core utilities, networking, data handling |
| UIKit | Haptic feedback and legacy UI integration |

### Analytics

| Library | Purpose |
|---------|---------|
| [Mixpanel](https://mixpanel.com/) | User behavior analytics and event tracking |

### Requirements

- iOS 17.0+
- iPhone (Optimized)
- Xcode 15.4+
- Microphone permission for voice input

---

## Architecture

Gako implements **Clean Architecture** combined with **MVVM (Model-View-ViewModel)** pattern to ensure:

- **Separation of Concerns**: Clear boundaries between layers
- **Testability**: Business logic isolated for unit testing
- **Maintainability**: Modular codebase for efficient updates
- **Scalability**: Extensible design for new features

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                      PRESENTATION LAYER                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │     Views       │  │   ViewModels    │  │   Components    │  │
│  │   (SwiftUI)     │◄─┤   (Reactive)    │  │   (Reusable)    │  │
│  └─────────────────┘  └────────┬────────┘  └─────────────────┘  │
└────────────────────────────────┼────────────────────────────────┘
                                 │
┌────────────────────────────────┼────────────────────────────────┐
│                        DOMAIN LAYER                              │
│  ┌─────────────────┐  ┌────────▼────────┐  ┌─────────────────┐  │
│  │    Entities     │  │    UseCases     │  │  Repositories   │  │
│  │ (Student, Note, │  │ (Business Logic)│  │   (Protocols)   │  │
│  │ Activity, etc.) │  └────────┬────────┘  └────────┬────────┘  │
│  └─────────────────┘           │                    │           │
└────────────────────────────────┼────────────────────┼───────────┘
                                 │                    │
┌────────────────────────────────┼────────────────────┼───────────┐
│                         DATA LAYER                               │
│  ┌─────────────────┐  ┌────────▼────────────────────▼────────┐  │
│  │  SwiftData      │  │         DataSources                  │  │
│  │  (Context)      │◄─┤  (StudentDS, NoteDS, ActivityDS)     │  │
│  └─────────────────┘  └──────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                 │
┌────────────────────────────────┼────────────────────────────────┐
│                       EXTERNAL SERVICES                          │
│  ┌──────────────────────┐  ┌────────────────────────────────┐   │
│  │  Apple Speech API    │  │       OpenAI GPT-4o mini       │   │
│  │  (Voice Recognition) │  │     (AI Summary Generation)    │   │
│  └──────────────────────┘  └────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### Layer Responsibilities

| Layer | Components | Responsibility |
|-------|------------|----------------|
| Presentation | Views, ViewModels, Components | UI rendering, user interaction, state management |
| Domain | Entities, UseCases, Repositories | Business logic, entity definitions, data contracts |
| Data | DataSources, Services | Data persistence, API integration, speech recognition |

---

## Project Structure

```
Gako/
├── GakoApp.swift                          # App entry point
│
├── Constants/                              # App constants and configurations
│
├── Data/                                   # Data layer
│   ├── DataSource/
│   │   ├── StudentDataSourceImpl.swift
│   │   ├── NoteDataSourceImpl.swift
│   │   ├── ActivityDataSourceImpl.swift
│   │   └── SummaryDataSourceImpl.swift
│   │
│   └── Service/
│       ├── SpeechRecognation/
│       │   └── SpeechRecognizer.swift      # Apple Speech framework integration
│       │
│       ├── SummaryService/
│       │   ├── SummaryService.swift
│       │   └── SummaryLlamaService.swift   # NVIDIA LLaMA API integration
│       │
│       ├── ActivityNoteService/
│       │   └── ...
│       │
│       └── Analytics/
│           └── InputAnalyticsTracker.swift # Mixpanel integration
│
├── Domain/                                 # Business logic layer
│   ├── Entities/
│   │   ├── Student.swift
│   │   ├── Note.swift
│   │   ├── Activity.swift
│   │   └── Summary.swift
│   │
│   ├── Repositories/
│   │   ├── StudentRepository.swift
│   │   ├── NoteRepository.swift
│   │   ├── ActivityRepository.swift
│   │   └── SummaryRepository.swift
│   │
│   └── UseCases/
│       ├── StudentUseCase.swift
│       ├── NoteUseCase.swift
│       ├── ActivityUseCase.swift
│       └── SummaryUseCase.swift
│
├── Presentation/                           # Presentation layer
│   ├── OnBoarding/
│   │   └── ... (onboarding views)
│   │
│   ├── Shared/
│   │   └── ... (shared components)
│   │
│   ├── StudentTab/
│   │   ├── Components/
│   │   ├── Helper/
│   │   ├── Models/
│   │   ├── ViewModels/
│   │   │   ├── StudentViewModel.swift
│   │   │   ├── NoteViewModel.swift
│   │   │   └── ActivityViewModel.swift
│   │   └── Views/
│   │
│   └── SummaryTab/
│       ├── Components/
│       ├── ViewModels/
│       │   └── SummaryViewModel.swift
│       └── Views/
│
├── Resources/
│
└── Assets.xcassets/
```

---

## Implementation Details

### OpenAI GPT-4o mini Integration

The AI summary generation leverages OpenAI's GPT-4o mini model:

```swift
import OpenAI

class SummaryService {
    private let openAI: OpenAI
    private let summaryUseCase: SummaryUseCase
    
    func generateAndSaveSummaries(for students: [Student], on date: Date) async throws {
        let studentSummaries = try await generateStudentSummaries(for: students, on: date)
        
        for (student, summaryContent) in studentSummaries {
            if let existingSummary = try await summaryUseCase.fetchSummary(for: student, on: date) {
                existingSummary.summary = summaryContent
                try await summaryUseCase.updateSummary(existingSummary)
            } else {
                let newSummary = Summary(summary: summaryContent, createdAt: date, student: student)
                try await summaryUseCase.addSummary(newSummary, for: student)
            }
        }
    }
    
    private func generateIndividualSummary(for student: Student, on date: Date) async throws -> String {
        let query = ChatQuery(
            messages: [.init(role: .user, content: prompt)!],
            model: .gpt4_o_mini
        )
        // API call implementation...
    }
}
```

### SwiftData Persistence

Data persistence using Apple's SwiftData framework:

```swift
import SwiftUI
import SwiftData

@main
struct GakoApp: App {
    let container: ModelContainer
    
    init() {
        do {
            container = try ModelContainer(for: Student.self, Note.self, Activity.self)
            
            let context = container.mainContext
            
            // Initialize data sources with SwiftData context
            let studentDataSource = StudentDataSourceImpl(context: context)
            let studentRepository = StudentRepositoryImpl(dataSource: studentDataSource)
            let studentUseCase = StudentUseCaseImpl(repository: studentRepository)
            
            // ... additional initialization
        } catch {
            fatalError("Failed to create ModelContainer: \(error)")
        }
    }
}
```

### Speech Recognition Integration

Real-time voice-to-text using Apple Speech framework:

```swift
import Speech

class SpeechRecognizer: ObservableObject {
    private var recognitionTask: SFSpeechRecognitionTask?
    private let audioEngine = AVAudioEngine()
    private let speechRecognizer = SFSpeechRecognizer(locale: Locale(identifier: "id-ID"))
    
    func startRecording() throws {
        // Configure audio session and start recognition
    }
}
```

---

## Getting Started

### Prerequisites

- macOS Sonoma 14.0+
- Xcode 15.4+
- iOS 17.0+ device or simulator
- OpenAI API Key
- Mixpanel Token (optional, for analytics)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Breesix/GakoApp.git
   cd ios-gako
   ```

2. **Install CocoaPods dependencies**
   ```bash
   pod install
   ```

3. **Open in Xcode**
   ```bash
   open Gako.xcworkspace
   ```

4. **Configure API Keys**
   
   Update the API keys in `GakoApp.swift`:
   ```swift
   // Mixpanel
   Mixpanel.initialize(token: "YOUR_MIXPANEL_TOKEN", trackAutomaticEvents: true)
   
   // OpenAI API
   let openAI = OpenAI(apiToken: "YOUR_OPENAI_API_KEY")
   ```

5. **Build and Run**
   
   Select target device and press `Cmd + R`

---

## Dependencies

| Package | Source | Purpose |
|---------|--------|---------|
| [Mixpanel-swift](https://github.com/mixpanel/mixpanel-swift) | CocoaPods | User analytics and event tracking |

---

## Design Patterns

| Pattern | Implementation |
|---------|----------------|
| MVVM | Separation of View, ViewModel, and Model |
| Clean Architecture | Domain, Data, Presentation layers |
| Repository Pattern | Abstract data access layer |
| Use Case Pattern | Encapsulated business logic |
| DataSource Pattern | `StudentDataSourceImpl`, `NoteDataSourceImpl`, etc. |
| Observer | `@Published` properties with `@ObservableObject` |
| Dependency Injection | Repository/UseCase injection into ViewModels |

---

## Contributors

- **Rangga Hadi Putra** — Tech Lead
- **Akmal Hakim** — iOS Developer
- **Kevin Fairuz** — iOS Developer
- **Wisnu Maharyuan Hadiyasa** — Lead UI/UX Designer
- **Arshad Tareeq Buchori** — UI/UX Designer
- **Hans Jonathan Zebua** — Product Manager

---

## License

This project is for portfolio and educational purposes.

---

## Acknowledgments

- [OpenAI](https://openai.com/) — GPT-4o mini API
- [Mixpanel](https://mixpanel.com/) — Analytics platform
- Apple Developer Documentation — SwiftUI, SwiftData, Speech Framework
