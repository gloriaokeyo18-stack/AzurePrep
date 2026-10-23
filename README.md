# AzurePrep — Learn the Why
### Interactive Microsoft Azure Fundamentals (AZ-900) Certification Platform

> **Core Philosophy:** *Every question should teach something, regardless of whether the learner gets it right or wrong.*

---

## ⚠️ Important Disclaimer

**AzurePrep is an independent educational tool developed to help learners understand cloud computing and prepare for the AZ-900 certification exam.**

- **Not an Official Tool:** This application is **not** affiliated with, endorsed by, sponsored by, or produced by Microsoft Corporation.
- **Trademarks:** *Microsoft*, *Azure*, *Microsoft Entra ID*, *Microsoft Defender*, *Microsoft Purview*, and any related product or service names are registered trademarks of Microsoft Corporation.
- **Exam Scores:** Practice questions, assessments, and mock examination scores generated within this application are strictly for diagnostic and learning purposes. Performance in AzurePrep does not guarantee a passing score on the official Microsoft AZ-900 proctored certification exam.

---

## 📖 Table of Contents

1. [Product Overview & Problem Statement](#-product-overview--problem-statement)
2. [Architectural Overview & Diagrams](#-architectural-overview--diagrams)
   - [System Component Architecture](#system-component-architecture)
   - [Quiz Engine State Machine](#quiz-engine-state-machine)
   - [Data Model & Entity Relationships](#data-model--entity-relationships)
3. [Key Features](#-key-features)
4. [AZ-900 Exam Domain Structure](#-az-900-exam-domain-structure)
5. [Data Schemas & Type Definitions](#-data-schemas--type-definitions)
6. [How-To Guide](#-how-to-guide)
   - [Taking a Topic-Focused Quiz](#1-taking-a-topic-focused-quiz)
   - [Taking a Timed Mock Exam](#2-taking-a-timed-mock-exam)
   - [Reviewing & Retrying Mistakes](#3-reviewing--retrying-mistakes)
   - [Bookmarking & Custom Practice](#4-bookmarking--custom-practice)
   - [Configuring Domain Weights](#5-configuring-domain-weights)
   - [Backing Up and Restoring Progress](#6-backing-up-and-restoring-progress)
7. [Technologies Used](#-technologies-used)
8. [Testing & Content Integrity](#-testing--content-integrity)
9. [Local Development & Setup](#-local-development--setup)

---

## 🎯 Product Overview & Problem Statement

### The Problem
When studying for entry-level cloud credentials like the AZ-900, many students drill static multiple-choice question dumps. They memorise letter answers (`A`, `C`, etc.) without understanding *why* the answer is correct or *why* the alternative options fail. As soon as Microsoft slightly rephrases the scenario in the real exam, rote learners struggle.

### The Solution
**AzurePrep — Learn the Why** transforms quiz practice into a guided learning experience:
- **Option-Specific Feedback:** Rather than generic *"Incorrect, try again"* messages, every distractor is thoroughly unpacked with its actual Azure purpose and why it does not fit the scenario.
- **Dual Accuracy Tracking:** Separates **First-Attempt Accuracy** from repeated retries, preventing the illusion of mastery from repeated exposure.
- **Spaced Mistake Review:** Retains every missed question in an isolated error ledger until the learner retries and marks it as understood.
- **Configurable Exam Simulation:** Mimics the official 45-minute AZ-900 test environment with explanations locked until submission.

---

## 🏗️ Architectural Overview & Diagrams

### System Component Architecture

```mermaid
graph TD
    subgraph UI_Layer [User Interface Layer (React 19 + Tailwind CSS)]
        Nav[Navbar & Global Stats Pill]
        Dash[Learning Dashboard & Recommendations]
        TopicView[Practise by Topic Browser]
        MixedView[Mixed Practice Configurator]
        MockView[Timed Mock Exam Launcher]
        MistakesView[Review My Mistakes & Retry Hub]
        BookmarksView[Bookmarked Questions Deck]
        AnalyticsView[Progress & Telemetry View]
        Modals[Weights & Profile Modals]
    end

    subgraph Core_Engine [Interactive Quiz Engine]
        QE[QuizEngine Component]
        QPalette[Question Palette & Navigation Grid]
        EvalView[Option Breakdown & Reinforcement Card]
        QSummary[QuizSummary & Domain Breakdown Card]
    end

    subgraph State_Management [Context & Pure Logic]
        QContext[QuizContext Provider]
        LogicUtils[Pure Logic Utilities (quizLogic.ts)]
        EvalEngine[evaluateAnswer]
        StatCalc[calculateTopicStats & calculateOverallStats]
        Sampler[sampleMockExamQuestions]
    end

    subgraph Data_Storage [Persistent Storage & Content Bank]
        QBank[(Verified AZ-900 Question Bank (36 Qs))]
        DomainsCfg[(Official Exam Domains Config)]
        LocalStorage[(Browser LocalStorage Engine)]
    end

    Nav --> QContext
    Dash --> QContext
    TopicView --> QContext
    MixedView --> QContext
    MockView --> QContext
    MistakesView --> QContext
    BookmarksView --> QContext
    AnalyticsView --> QContext
    Modals --> QContext

    QContext --> QE
    QE --> QPalette
    QE --> EvalView
    QE --> QSummary

    QContext --> LogicUtils
    LogicUtils --> EvalEngine
    LogicUtils --> StatCalc
    LogicUtils --> Sampler

    QContext --> QBank
    QContext --> DomainsCfg
    QContext <--> LocalStorage
