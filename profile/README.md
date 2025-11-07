# CareerSwipe

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Product Vision](#product-vision)
   2.1. [Problem](#problem)
   2.2. [Solution](#solution)
   2.3. [Technology Stack](#technology-stack)
   2.4. [Account Types](#account-types)
   2.5. [User Interface](#user-interface)
3. [Database Schema](#database-schema)
   3.1. [users](#1-users)
   3.2. [candidate_profiles](#2-candidate_profiles)
   3.3. [candidate_skills](#3-candidate_skills)
   3.4. [candidate_wanted_positions](#4-candidate_wanted_positions)
   3.5. [candidate_wanted_locations](#5-candidate_wanted_locations)
   3.6. [candidate_experiences](#6-candidate_experiences)
   3.7. [candidate_education](#7-candidate_education)
   3.8. [candidate_certificates](#8-candidate_certificates)
   3.9. [candidate_projects](#9-candidate_projects)
   3.10. [candidate_achievements](#10-candidate_achievements)
   3.11. [candidate_languages](#11-candidate_languages)
   3.12. [candidate_desired_seniority](#12-candidate_desired_seniority)
   3.13. [companies](#13-companies)
   3.14. [recruiter_profiles](#14-recruiter_profiles)
   3.15. [jobs](#15-jobs)
   3.16. [job_skills](#16-job_skills)
4. [Swipe & Matching Tables](#swipe--matching-tables)
   4.1. [candidate_swipes](#17-candidate_swipes)
   4.2. [recruiter_swipes](#18-recruiter_swipes)
   4.3. [card_locks](#19-card_locks)
   4.4. [matches](#20-matches)
5. [Chat & Communication Tables](#chat--communication-tables)
   5.1. [conversations](#21-conversations)
   5.2. [messages](#22-messages)
6. [Recommendation Algorithm Tables](#recommendation-algorithm-tables)
   6.1. [matching_signals](#23-matching_signals)
   6.2. [match_feedback_events](#24-match_feedback_events)
   6.3. [regional_demand_supply](#25-regional_demand_supply)
7. [Analytics & Audit Tables](#analytics--audit-tables)
   7.1. [user_sessions](#26-user_sessions)
   7.2. [audit_logs](#27-audit_logs)
   7.3. [swipe_analytics](#28-swipe_analytics)
8. [Future-Proofing Tables](#future-proofing-tables-not-mvp)
   8.1. [premium_features](#29-premium_features-future)
   8.2. [user_premium_features](#30-user_premium_features-future)
   8.3. [scheduled_interviews](#31-scheduled_interviews-future)
   8.4. [starred_jobs](#32-starred_jobs-future---premium)
   8.5. [match_history_archive](#33-match_history_archive-future---premium)
9. [Key Relationships Summary](#key-relationships-summary)
   9.1. [One-to-One Relationships](#one-to-one-relationships)
   9.2. [One-to-Many Relationships](#one-to-many-relationships)
   9.3. [Many-to-Many Relationships](#many-to-many-relationships-via-junction-tables)
10. [Database Design Principles](#database-design-principles)
   10.1. [Normalization](#1-normalization)
   10.2. [Extensibility](#2-extensibility)
   10.3. [Performance Optimization](#3-performance-optimization)
   10.4. [Data Integrity](#4-data-integrity)
   10.5. [Security & Compliance](#5-security--compliance)
   10.6. [Scalability Considerations](#6-scalability-considerations)
11. [Critical Business Logic Enforced by Schema](#critical-business-logic-enforced-by-schema)
    11.1. [Swipe Mechanics](#swipe-mechanics)
    11.2. [Match Lifecycle](#match-lifecycle)
    11.3. [Algorithm Intelligence](#algorithm-intelligence)
12. [Migration Strategy](#migration-strategy)
    12.1. [Phase 1: MVP Core (Immediate)](#phase-1-mvp-core-immediate)
    12.2. [Phase 2: Algorithm Enhancement (Post-MVP)](#phase-2-algorithm-enhancement-post-mvp)
    12.3. [Phase 3: Premium Features (Future)](#phase-3-premium-features-future)
    12.4. [Phase 4: Company Accounts (Future)](#phase-4-company-accounts-future)
13. [Technical Implementation Notes](#technical-implementation-notes)
    13.1. [Technologies](#technologies)
    13.2. [Critical Queries to Optimize](#critical-queries-to-optimize)
    13.3. [Data Retention Policies](#data-retention-policies)
14. [API Endpoint Design](#api-endpoint-design-rest-over-https)
    14.1. [Conventions](#conventions)
    14.2. [Problem+JSON Error](#problemjson-error)
    14.3. [Auth & Session](#auth--session)
    14.4. [Users](#users)
    14.5. [Candidate Profiles](#candidate-profiles)
    14.6. [Recruiters & Jobs](#recruiters--jobs)
    14.7. [Swipe & Matching](#swipe--matching)
    14.8. [Card Locks](#card-locks-realtime-safety)
    14.9. [Chat](#chat)
    14.10. [Discovery & Explainability](#discovery--explainability)
    14.11. [Admin](#admin-futureflagged-hide-in-prod-until-ready)
15. [Authentication Flow](#authentication-flow-linkedin-oauth2--jwt)
    15.1. [Tokens](#tokens)
    15.2. [Sequence](#sequence)
    15.3. [Security Controls](#security-controls)
16. [Matching Algorithm Specification](#matching-algorithm-specification-mvp--v1)
    16.1. [MVP Heuristic Scoring](#mvp-heuristic-scoring)
    16.2. [Exploration/Exploitation](#explorationexploitation)
    16.3. [Learning Loop](#learning-loop-v1)
    16.4. [Explainability API](#explainability-api)
17. [UI/UX Wireframes](#uiux-wireframes-textual-blueprint)
    17.1. [Login / Registration](#login--registration)
    17.2. [Main Swipe View (Candidate)](#main-swipe-view-candidate)
    17.3. [Main Swipe View (Recruiter)](#main-swipe-view-recruiter)
    17.4. [Matches View](#matches-view)
    17.5. [Chat View](#chat-view)
    17.6. [Job Creation/Edit](#job-creationedit)
18. [Real-Time Architecture](#realtime-architecture-websocket)
    18.1. [Connection](#connection)
    18.2. [Events (JSON, envelope)](#events-json-envelope)
    18.3. [Backpressure & QoS](#backpressure--qos)
19. [Video Integration](#video-integration-youtube--s3-fallback)
    19.1. [Candidate/Job 30s Video](#candidatejob-30s-video)
    19.2. [Privacy & Abuse](#privacy--abuse)
20. [Testing Strategy & QA Gates](#testing-strategy--qa-gates)
    20.1. [Levels](#levels)
    20.2. [CI/CD](#cicd-github-actions)
    20.3. [Test Data](#test-data)
21. [OpenAPI 3.1](#openapi-31-mvp-extract)
22. [Axum Route Skeletons](#axum-rust-route-skeletons-excerpt)
23. [Data & Cache Contracts](#data--cache-contracts-mvp)
    23.1. [Redis Keys](#redis-keys)
    23.2. [Idempotency](#idempotency)
24. [Environment & Config](#environment--config)
25. [Open Questions / Decisions Needed](#open-questions--decisions-needed)

## Executive Summary

**CareerSwipe** is a gamified job matching platform that transforms the traditional job search experience into an engaging, swipe-based interface. By combining the intuitive mechanics of dating apps with intelligent job matching algorithms, CareerSwipe makes job hunting fun for candidates while providing recruiters with a streamlined candidate discovery process.

**Version**: 0.1
**Document Date**: 05.11.2025  
**Target Launch**: 01.01.2026

## Product Vision

### Problem

- **For Candidates**: Traditional job search is tedious, overwhelming, and demotivating. Platforms like LinkedIn are cluttered with irrelevant postings and require excessive manual filtering.
- **For Recruiters**: Sorting through hundreds of unqualified applications wastes time and resources. Quality candidate discovery is inefficient.

### Solution

CareerSwipe creates a curated, two-sided matching marketplace where:
- Candidates swipe through personalized job opportunities presented as engaging cards
- Recruiters swipe through pre-filtered candidates matched to specific positions
- Both parties must mutually "like" each other to connect (preventing spam and unwanted outreach)
- Machine learning continuously improves match quality based on user behavior

### Technology Stack

**Frontend:**
- Swift UI / Jetpack Compose (iOS/Android mobile apps)
- SvelteKit (Web application)

**Backend:**
- Rust Axum (API server)
- JWT + OAuth 2.0 (Authentication) - **Subject to change**

**Database & Storage:**
- PostgreSQL (Primary database)
- Redis (Caching, session management, card locks)
- AWS S3 (Video storage) - **Subject to change**

**Infrastructure:**
- AWS - **Subject to change**
- Docker + Kubernetes (Containerization & orchestration) - **Subject to change**
- GitHub Actions (CI/CD) - **Subject to change**
- CloudFront / CDN (Video delivery) - **Subject to change**

**Machine Learning:**
- Python (ML pipeline) - **Subject to change**
- PyTorch (Model training) - **Subject to change**
- Apache Airflow (ML workflow orchestration) - **Subject to change**

---

### Account Types

#### 1. Candidate Account

**Primary User**: Job seekers (entry-level to executive)

**Key Features:**
- Swipe-based job discovery
- Video profile presentation (optional 30s)
- Comprehensive profile with resume data
- Match management
- One-to-one chat with recruiters
- Location and preference settings

**User Journey:**
1. Register via LinkedIn or manual CV upload
2. Complete profile (auto-filled from LinkedIn/CV)
3. Start swiping through personalized job cards
4. Receive matches when recruiters swipe right
5. Chat and schedule interviews with recruiters

#### 2. Recruiter Account

**Primary User**: HR professionals, hiring managers, talent acquisition specialists

**Key Features:**
- Multi-position management
- Candidate discovery per job posting
- Company profile setup (MVP: manual entry)
- Swipe-based candidate review
- Match management across positions

**User Journey:**
1. Register via LinkedIn or manual entry (company invites only in non-MVP)
2. Set up company information (company information transferred automaticaly in non-MVP)
3. Create job postings (with video optional)
4. Swipe through matched candidates for each position
5. Engage with matched candidates via chat

#### 3. Company Account *(Post-MVP)*

**Primary User**: Company administrators

**Key Features:**
- Centralized company profile management
- Recruiter invitation and management
- Company-wide settings and policies
- Analytics dashboard

**Note**: Not included in MVP scope, company data is managed within recruiter accounts using `_mvp` suffixed fields.

#### 4. Admin Account *(Future)*

**Primary User**: Platform administrators

**Key Features:**
- User management
- Content moderation
- Platform analytics
- System configuration

**Note**: Not included in MVP scope.

---

### User Interface

#### Core Views (All Account Types)

##### 1. **Login / Registration View**

- LinkedIn OAuth integration (primary)
- Manual registration with email/phone
- Password recovery flow
- Terms & conditions acceptance

##### 2. **Main Swipe View** (Core Feature)

**Layout:**
- Full-screen card presentation
- Swipeable left (dislike) / right (like)
- Scrollable card content
- Top navigation: Logo, Matches counter, Profile icon
- Bottom action buttons: Dislike, Info, Like

**Candidate Card Content (for Recruiters):**
- Video presentation (if provided)
- Profile photo
- Name & current title
- About me section
- Skills (max 5 keywords)
- Experience highlights
- Education summary
- Languages
- Location & work preferences
- Salary expectations (if provided)

**Job Card Content (for Candidates):**
- Company logo (or anonymous badge)
- Job title
- Video presentation (if provided)
- Short description
- Salary range
- Location & remote options
- Seniority level
- Required skills (max 5)
- Company info (unless anonymized)

##### 3. **Matches View**

- Grid/list of active matches
- Match timestamp
- Last message preview
- Unread message indicator
- Swipe to delete match (with confirmation)

##### 4. **Chat View**

- One-to-one messaging interface
- Text messages and link sharing only
- Read receipts
- Message timestamps
- Delete conversation option

##### 5. **User Profile View**

- Editable profile information
- Account settings
- Logout option

**For Candidates:**
- Personal information
- Video upload/management
- Work experience CRUD
- Education CRUD
- Skills management
- Preferences (location, salary, remote)

**For Recruiters:**
- Personal information
- Company details (MVP: manual)
- Active job postings list
- Create new job button

##### 6. **Settings View**

- Notification preferences
- Account privacy
- Data management
- Delete account option
- Language selection (English only in MVP)

#### Recruiter-Specific Views

##### 7. **Job Selection View**

- List of active job postings
- Create new job button
- Edit/archive existing jobs
- Select job to view candidates for
- Job performance metrics (views, matches)

##### 8. **Job Creation/Edit View**

- Job title input
- Video upload (optional, max 30s)
- Description text area (max 500 chars)
- Salary range inputs
- Location selection
- Work arrangement toggles (remote/onsite/hybrid)
- Seniority dropdown (optional)
- Skills input (max 5)
- Expiration date picker
- Company anonymization toggle
- Save/publish button

## Database Schema 

This schema is designed for extensibility, normalized structure, and future-proofing. All timestamps use UTC. Soft deletes are implemented where data preservation is critical for analytics and compliance.

### 1. **users**

Master table for all user types (Candidate, Recruiter, Company, Admin)

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique user identifier |
| email | VARCHAR(255) | UNIQUE, NOT NULL | User email |
| phone | VARCHAR(20) | NULLABLE | User phone number |
| password_hash | VARCHAR(255) | NULLABLE | Hashed password (null for OAuth-only) |
| user_type | ENUM | NOT NULL | 'candidate', 'recruiter', 'company', 'admin' |
| account_status | ENUM | NOT NULL, DEFAULT 'active' | 'active', 'suspended', 'deleted' |
| linkedin_id | VARCHAR(255) | UNIQUE, NULLABLE | LinkedIn OAuth identifier |
| oauth_provider | VARCHAR(50) | NULLABLE | 'linkedin', future: 'google' |
| oauth_token_encrypted | TEXT | NULLABLE | Encrypted OAuth refresh token |
| oauth_token_expires_at | TIMESTAMP | NULLABLE | Token expiration |
| is_premium | BOOLEAN | DEFAULT FALSE | Premium account flag |
| premium_expires_at | TIMESTAMP | NULLABLE | Premium subscription expiration |
| created_at | TIMESTAMP | DEFAULT NOW() | Account creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update timestamp |
| last_login_at | TIMESTAMP | NULLABLE | Last successful login |
| deleted_at | TIMESTAMP | NULLABLE | Soft delete timestamp |

**Indexes:**
- `idx_users_email` on (email)
- `idx_users_linkedin_id` on (linkedin_id)
- `idx_users_type` on (user_type)

---

### 2. **candidate_profiles**

Extended profile data for candidates

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Profile identifier |
| user_id | UUID | FOREIGN KEY → users(id), UNIQUE | Reference to user |
| first_name | VARCHAR(100) | NOT NULL | First name |
| last_name | VARCHAR(100) | NOT NULL | Last name |
| video_url | VARCHAR(500) | NULLABLE | YouTube private video URL |
| video_duration_seconds | INTEGER | NULLABLE | Video length (max 30) |
| about_me | TEXT | NULLABLE | One paragraph description (max 500 chars) |
| specialization | VARCHAR(255) | NOT NULL | Primary specialization |
| current_location_city | VARCHAR(100) | NOT NULL | Current city |
| current_location_country | VARCHAR(100) | NOT NULL | Current country |
| desired_salary_min | DECIMAL(10,2) | NULLABLE | Minimum desired salary |
| desired_salary_max | DECIMAL(10,2) | NULLABLE | Maximum desired salary |
| desired_salary_currency | VARCHAR(3) | NULLABLE | ISO currency code (USD, EUR) |
| remote_preference | ENUM | NULLABLE | 'remote', 'onsite', 'hybrid', 'flexible' |
| profile_completion_percentage | INTEGER | DEFAULT 0 | Calculated profile completeness |
| is_actively_looking | BOOLEAN | DEFAULT TRUE | Active job search flag |
| created_at | TIMESTAMP | DEFAULT NOW() | Profile creation |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last profile update |

**Indexes:**
- `idx_candidate_user_id` on (user_id)
- `idx_candidate_location` on (current_location_city, current_location_country)

---

### 3. **candidate_skills**

Skills associated with candidates (many-to-many via tags)

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Skill entry identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| skill_name | VARCHAR(100) | NOT NULL | Skill keyword (max 5 per candidate) |
| skill_order | INTEGER | NOT NULL | Display order (1-5) |
| created_at | TIMESTAMP | DEFAULT NOW() | When skill was added |

**Indexes:**
- `idx_candidate_skills_profile` on (candidate_profile_id)
- `idx_candidate_skills_name` on (skill_name)

**Constraints:**
- Max 5 skills per candidate (enforced at application level)

---

### 4. **candidate_wanted_positions**

Desired job titles/types for candidates

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Position entry identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| position_title | VARCHAR(200) | NOT NULL | Desired position name |
| position_order | INTEGER | NOT NULL | Display order (1-5) |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |

**Indexes:**
- `idx_wanted_positions_profile` on (candidate_profile_id)

**Constraints:**
- Max 5 positions per candidate

---

### 5. **candidate_wanted_locations**

Preferred job locations for candidates

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Location entry identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| location_type | ENUM | NOT NULL | 'city', 'country', 'region' |
| location_value | VARCHAR(200) | NOT NULL | Location name |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |

**Indexes:**
- `idx_wanted_locations_profile` on (candidate_profile_id)

---

### 6. **candidate_experiences**

Work experience entries

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Experience identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| position_title | VARCHAR(200) | NOT NULL | Job title |
| company_name | VARCHAR(200) | NULLABLE | Company name (optional) |
| start_date | DATE | NOT NULL | Start date |
| end_date | DATE | NULLABLE | End date (null = current) |
| description | TEXT | NULLABLE | Role description |
| display_order | INTEGER | NOT NULL | Display order |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_experience_profile` on (candidate_profile_id)

---

### 7. **candidate_education**

Educational background

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Education identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| degree_title | VARCHAR(200) | NOT NULL | Degree/qualification name |
| institution_name | VARCHAR(200) | NULLABLE | School/university name |
| start_date | DATE | NOT NULL | Start date |
| end_date | DATE | NULLABLE | End date (null = ongoing) |
| description | TEXT | NULLABLE | Additional details |
| display_order | INTEGER | NOT NULL | Display order |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_education_profile` on (candidate_profile_id)

---

### 8. **candidate_certificates**

Professional certifications

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Certificate identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| certificate_name | VARCHAR(200) | NOT NULL | Certificate name |
| issue_date | DATE | NOT NULL | Date obtained |
| description | TEXT | NULLABLE | Additional details |
| display_order | INTEGER | NOT NULL | Display order |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_certificates_profile` on (candidate_profile_id)

---

### 9. **candidate_projects**

Portfolio projects

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Project identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| project_name | VARCHAR(200) | NOT NULL | Project name |
| start_date | DATE | NOT NULL | Start date |
| end_date | DATE | NULLABLE | End date (null = ongoing) |
| description | TEXT | NULLABLE | Project description |
| display_order | INTEGER | NOT NULL | Display order |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_projects_profile` on (candidate_profile_id)

---

### 10. **candidate_achievements**

Other notable achievements

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Achievement identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| achievement_name | VARCHAR(200) | NOT NULL | Achievement name |
| start_date | DATE | NOT NULL | Start/award date |
| end_date | DATE | NULLABLE | End date if applicable |
| description | TEXT | NULLABLE | Details |
| display_order | INTEGER | NOT NULL | Display order |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_achievements_profile` on (candidate_profile_id)

---

### 11. **candidate_languages**

Language proficiencies

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Language identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| language_name | VARCHAR(100) | NOT NULL | Language name |
| proficiency_level | ENUM | NOT NULL | 'A1', 'A2', 'B1', 'B2', 'C1', 'C2', 'native' |
| certificate_name | VARCHAR(200) | NULLABLE | Certificate name if any |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |

**Indexes:**
- `idx_languages_profile` on (candidate_profile_id)

---

### 12. **candidate_desired_seniority**

Seniority levels candidates are interested in

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Entry identifier |
| candidate_profile_id | UUID | FOREIGN KEY → candidate_profiles(id) | Reference to profile |
| seniority_level | ENUM | NOT NULL | 'intern', 'junior', 'medior', 'senior', 'manager', 'exec' |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |

**Indexes:**
- `idx_desired_seniority_profile` on (candidate_profile_id)

---

### 13. **companies**

Company/organization information

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Company identifier |
| company_name | VARCHAR(200) | NOT NULL | Official company name |
| company_description | TEXT | NULLABLE | Company overview |
| logo_url | VARCHAR(500) | NULLABLE | Company logo URL |
| industry | VARCHAR(100) | NULLABLE | Industry sector |
| company_size | ENUM | NULLABLE | 'startup', 'small', 'medium', 'large', 'enterprise' |
| website_url | VARCHAR(500) | NULLABLE | Company website |
| is_anonymized | BOOLEAN | DEFAULT FALSE | Hide company name from candidates |
| allow_multiple_applications | BOOLEAN | DEFAULT TRUE | Allow candidates to apply to multiple jobs |
| created_at | TIMESTAMP | DEFAULT NOW() | Company creation |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_company_name` on (company_name)

**Note:** For MVP, company info lives within recruiter profiles. This table is future-proofed for Company Account feature.

---

### 14. **recruiter_profiles**

Extended profile data for recruiters

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Profile identifier |
| user_id | UUID | FOREIGN KEY → users(id), UNIQUE | Reference to user |
| company_id | UUID | FOREIGN KEY → companies(id), NULLABLE | Reference to company (future) |
| first_name | VARCHAR(100) | NOT NULL | First name |
| last_name | VARCHAR(100) | NOT NULL | Last name |
| job_title | VARCHAR(200) | NULLABLE | Recruiter's title |
| company_name_mvp | VARCHAR(200) | NOT NULL | Company name (MVP only) |
| company_description_mvp | TEXT | NULLABLE | Company description (MVP only) |
| company_logo_url_mvp | VARCHAR(500) | NULLABLE | Company logo (MVP only) |
| company_industry_mvp | VARCHAR(100) | NULLABLE | Industry (MVP only) |
| company_size_mvp | ENUM | NULLABLE | Size (MVP only) |
| company_website_mvp | VARCHAR(500) | NULLABLE | Website (MVP only) |
| allow_multiple_applications_mvp | BOOLEAN | DEFAULT TRUE | Settings (MVP only) |
| created_at | TIMESTAMP | DEFAULT NOW() | Profile creation |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
- `idx_recruiter_user_id` on (user_id)
- `idx_recruiter_company_id` on (company_id)

**Note:** `_mvp` suffix columns will be deprecated when Company Account feature is implemented.

---

### 15. **jobs**

Job postings created by recruiters

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Job identifier |
| recruiter_id | UUID | FOREIGN KEY → recruiter_profiles(id) | Posting recruiter |
| company_id | UUID | FOREIGN KEY → companies(id), NULLABLE | Company reference (future) |
| job_title | VARCHAR(200) | NOT NULL | Position title |
| video_url | VARCHAR(500) | NULLABLE | YouTube private video URL (max 30s) |
| video_duration_seconds | INTEGER | NULLABLE | Video length |
| job_description | TEXT | NULLABLE | One paragraph description (max 500 chars) |
| salary_min | DECIMAL(10,2) | NOT NULL | Minimum salary |
| salary_max | DECIMAL(10,2) | NOT NULL | Maximum salary |
| salary_currency | VARCHAR(3) | NOT NULL | ISO currency code |
| location_city | VARCHAR(100) | NOT NULL | Job city |
| location_country | VARCHAR(100) | NOT NULL | Job country |
| is_remote | BOOLEAN | DEFAULT FALSE | Remote work allowed |
| is_onsite | BOOLEAN | DEFAULT FALSE | Onsite required |
| is_hybrid | BOOLEAN | DEFAULT FALSE | Hybrid model |
| seniority_level | ENUM | NULLABLE | 'intern', 'junior', 'medior', 'senior', 'manager', 'exec' |
| job_source_url | VARCHAR(500) | NULLABLE | Original job posting URL (for parsing) |
| is_company_anonymized | BOOLEAN | DEFAULT FALSE | Hide company identity |
| status | ENUM | NOT NULL, DEFAULT 'active' | 'active', 'paused', 'expired', 'archived' |
| expiration_date | TIMESTAMP | NOT NULL | Job expiration (not visible to candidates) |
| created_at | TIMESTAMP | DEFAULT NOW() | Job creation |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |
| archived_at | TIMESTAMP | NULLABLE | Soft archive timestamp |

**Indexes:**
- `idx_jobs_recruiter` on (recruiter_id)
- `idx_jobs_status` on (status)
- `idx_jobs_expiration` on (expiration_date)
- `idx_jobs_location` on (location_city, location_country)

---

### 16. **job_skills**

Skills required for jobs

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Skill entry identifier |
| job_id | UUID | FOREIGN KEY → jobs(id) | Reference to job |
| skill_name | VARCHAR(100) | NOT NULL | Skill keyword (max 5 per job) |
| skill_order | INTEGER | NOT NULL | Display order (1-5) |
| created_at | TIMESTAMP | DEFAULT NOW() | When added |

**Indexes:**
- `idx_job_skills_job` on (job_id)
- `idx_job_skills_name` on (skill_name)

**Constraints:**
- Max 5 skills per job

---

## Swipe & Matching Tables

### 17. **candidate_swipes**

Candidate swipe actions on jobs

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Swipe identifier |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Swiping candidate |
| job_id | UUID | FOREIGN KEY → jobs(id) | Swiped job |
| swipe_direction | ENUM | NOT NULL | 'like', 'dislike', 'superlike' (future premium) |
| swiped_at | TIMESTAMP | DEFAULT NOW() | Swipe timestamp |

**Indexes:**
- `idx_candidate_swipes_candidate` on (candidate_id)
- `idx_candidate_swipes_job` on (job_id)
- `idx_candidate_swipes_lookup` on (candidate_id, job_id) UNIQUE

---

### 18. **recruiter_swipes**

Recruiter swipe actions on candidates for specific jobs

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Swipe identifier |
| recruiter_id | UUID | FOREIGN KEY → recruiter_profiles(id) | Swiping recruiter |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Swiped candidate |
| job_id | UUID | FOREIGN KEY → jobs(id) | Job context |
| swipe_direction | ENUM | NOT NULL | 'like', 'dislike' |
| swiped_at | TIMESTAMP | DEFAULT NOW() | Swipe timestamp |

**Indexes:**
- `idx_recruiter_swipes_recruiter` on (recruiter_id)
- `idx_recruiter_swipes_candidate` on (candidate_id)
- `idx_recruiter_swipes_job` on (job_id)
- `idx_recruiter_swipes_lookup` on (candidate_id, job_id, recruiter_id) UNIQUE

**Business Logic:**
- When one recruiter dislikes, mark as disliked for ALL recruiters for that job
- Prevents same candidate-job pair from appearing twice to any recruiter

---

### 19. **card_locks**

Real-time locking to prevent concurrent swipes

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Lock identifier |
| entity_type | ENUM | NOT NULL | 'candidate', 'job' |
| entity_id | UUID | NOT NULL | ID of locked entity |
| locked_by_user_id | UUID | FOREIGN KEY → users(id) | User holding lock |
| locked_at | TIMESTAMP | DEFAULT NOW() | Lock acquired time |
| expires_at | TIMESTAMP | NOT NULL | Auto-release time (e.g., 30 seconds) |

**Indexes:**
- `idx_card_locks_entity` on (entity_type, entity_id) UNIQUE
- `idx_card_locks_expiration` on (expires_at)

**Business Logic:**
- Before showing card, check if locked
- Acquire lock when card displayed
- Auto-release after timeout or swipe action
- Prevents two recruiters from swiping same candidate simultaneously

---

### 20. **matches**

Mutual likes between candidates and jobs

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Match identifier |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Matched candidate |
| job_id | UUID | FOREIGN KEY → jobs(id) | Matched job |
| recruiter_id | UUID | FOREIGN KEY → recruiter_profiles(id) | Recruiter who matched |
| matched_at | TIMESTAMP | DEFAULT NOW() | Match creation time |
| match_status | ENUM | NOT NULL, DEFAULT 'active' | 'active', 'cancelled_by_candidate', 'cancelled_by_recruiter' |
| cancelled_at | TIMESTAMP | NULLABLE | Cancellation timestamp |
| cancelled_by_user_id | UUID | FOREIGN KEY → users(id), NULLABLE | Who cancelled |

**Indexes:**
- `idx_matches_candidate` on (candidate_id)
- `idx_matches_job` on (job_id)
- `idx_matches_recruiter` on (recruiter_id)
- `idx_matches_lookup` on (candidate_id, job_id) UNIQUE
- `idx_matches_status` on (match_status)

**Business Logic:**
- Created when both parties swipe right
- Match visible across all recruiters from same company
- Cancelled match prevents re-matching for same job forever

---

## Chat & Communication Tables

### 21. **conversations**

Chat conversations between matched parties

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Conversation identifier |
| match_id | UUID | FOREIGN KEY → matches(id), UNIQUE | Associated match |
| created_at | TIMESTAMP | DEFAULT NOW() | Conversation start |
| last_message_at | TIMESTAMP | NULLABLE | Last message timestamp |
| is_active | BOOLEAN | DEFAULT TRUE | Active status |

**Indexes:**
- `idx_conversations_match` on (match_id)

---

### 22. **messages**

Individual chat messages

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Message identifier |
| conversation_id | UUID | FOREIGN KEY → conversations(id) | Parent conversation |
| sender_user_id | UUID | FOREIGN KEY → users(id) | Message sender |
| message_type | ENUM | NOT NULL, DEFAULT 'text' | 'text', 'link' |
| message_content | TEXT | NOT NULL | Message body |
| sent_at | TIMESTAMP | DEFAULT NOW() | Send timestamp |
| read_at | TIMESTAMP | NULLABLE | Read receipt timestamp |
| is_deleted | BOOLEAN | DEFAULT FALSE | Soft delete flag |

**Indexes:**
- `idx_messages_conversation` on (conversation_id)
- `idx_messages_sender` on (sender_user_id)
- `idx_messages_sent_at` on (sent_at)

---

## Recommendation Algorithm Tables

### 23. **matching_signals**

Intermediate signals used by matching algorithm

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Signal identifier |
| entity_type | ENUM | NOT NULL | 'candidate_to_job', 'job_to_candidate' |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Candidate reference |
| job_id | UUID | FOREIGN KEY → jobs(id) | Job reference |
| skill_overlap_score | DECIMAL(5,4) | NULLABLE | 0.0000 - 1.0000 |
| location_match_score | DECIMAL(5,4) | NULLABLE | 0.0000 - 1.0000 |
| salary_compatibility_score | DECIMAL(5,4) | NULLABLE | 0.0000 - 1.0000 |
| seniority_match_score | DECIMAL(5,4) | NULLABLE | 0.0000 - 1.0000 |
| remote_preference_match_score | DECIMAL(5,4) | NULLABLE | 0.0000 - 1.0000 |
| total_match_score | DECIMAL(5,4) | NOT NULL | Weighted aggregate score |
| calculated_at | TIMESTAMP | DEFAULT NOW() | Score calculation time |
| expires_at | TIMESTAMP | NOT NULL | Cache expiration (recalculate after) |

**Indexes:**
- `idx_signals_candidate_job` on (candidate_id, job_id) UNIQUE
- `idx_signals_score` on (total_match_score)
- `idx_signals_expiration` on (expires_at)

**Business Logic:**
- Pre-computed scores for performance
- Cached with expiration for freshness
- Used to rank cards in swipe queue

---

### 24. **match_feedback_events**

Training data for ML algorithm

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Event identifier |
| match_id | UUID | FOREIGN KEY → matches(id), NULLABLE | Match reference (if applicable) |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Candidate reference |
| job_id | UUID | FOREIGN KEY → jobs(id) | Job reference |
| event_type | ENUM | NOT NULL | 'match_created', 'chat_initiated', 'match_cancelled', 'hire_completed' (future) |
| event_timestamp | TIMESTAMP | DEFAULT NOW() | Event occurrence time |
| metadata_json | JSONB | NULLABLE | Additional context data |

**Indexes:**
- `idx_feedback_events_match` on (match_id)
- `idx_feedback_events_candidate` on (candidate_id)
- `idx_feedback_events_job` on (job_id)
- `idx_feedback_events_type` on (event_type)
- `idx_feedback_events_timestamp` on (event_timestamp)

**Business Logic:**
- Captures positive signals (matches, chats, hires)
- Captures negative signals (cancellations)
- Fed into continuous learning pipeline

---

### 25. **regional_demand_supply**

Market dynamics tracking for algorithm

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Entry identifier |
| location_city | VARCHAR(100) | NOT NULL | City name |
| location_country | VARCHAR(100) | NOT NULL | Country name |
| skill_category | VARCHAR(100) | NOT NULL | Skill/industry category |
| active_jobs_count | INTEGER | DEFAULT 0 | Number of active jobs |
| active_candidates_count | INTEGER | DEFAULT 0 | Number of active candidates |
| demand_supply_ratio | DECIMAL(10,4) | NOT NULL | Jobs/Candidates ratio |
| snapshot_date | DATE | NOT NULL | Data snapshot date |
| created_at | TIMESTAMP | DEFAULT NOW() | Record creation |

**Indexes:**
- `idx_demand_supply_location` on (location_city, location_country)
- `idx_demand_supply_skill` on (skill_category)
- `idx_demand_supply_snapshot` on (snapshot_date)
- `idx_demand_supply_lookup` on (location_city, location_country, skill_category, snapshot_date) UNIQUE

**Business Logic:**
- Updated daily via batch job
- Influences card prioritization (show scarce opportunities first)
- Used for explainability ("High demand in your area")

---

## Analytics & Audit Tables

### 26. **user_sessions**

Login sessions for security and analytics

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Session identifier |
| user_id | UUID | FOREIGN KEY → users(id) | Session owner |
| session_token_hash | VARCHAR(255) | UNIQUE, NOT NULL | Hashed session token |
| device_type | VARCHAR(50) | NULLABLE | 'ios', 'android', 'web' |
| device_identifier | VARCHAR(255) | NULLABLE | Device UUID |
| ip_address | VARCHAR(45) | NULLABLE | Login IP address |
| user_agent | TEXT | NULLABLE | Browser/app user agent |
| created_at | TIMESTAMP | DEFAULT NOW() | Session start |
| expires_at | TIMESTAMP | NOT NULL | Session expiration |
| last_activity_at | TIMESTAMP | DEFAULT NOW() | Last activity timestamp |
| is_active | BOOLEAN | DEFAULT TRUE | Session active status |

**Indexes:**
- `idx_sessions_user` on (user_id)
- `idx_sessions_token` on (session_token_hash)
- `idx_sessions_expiration` on (expires_at)

---

### 27. **audit_logs**

Comprehensive audit trail for compliance

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Log identifier |
| user_id | UUID | FOREIGN KEY → users(id), NULLABLE | Acting user |
| action_type | VARCHAR(100) | NOT NULL | Action performed (e.g., 'profile_updated', 'job_created') |
| entity_type | VARCHAR(50) | NULLABLE | Affected entity type |
| entity_id | UUID | NULLABLE | Affected entity ID |
| old_value_json | JSONB | NULLABLE | Previous state |
| new_value_json | JSONB | NULLABLE | New state |
| ip_address | VARCHAR(45) | NULLABLE | Request IP |
| timestamp | TIMESTAMP | DEFAULT NOW() | Action timestamp |

**Indexes:**
- `idx_audit_user` on (user_id)
- `idx_audit_action` on (action_type)
- `idx_audit_timestamp` on (timestamp)
- `idx_audit_entity` on (entity_type, entity_id)

---

### 28. **swipe_analytics**

Aggregated swipe metrics for algorithm tuning

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Analytics identifier |
| date | DATE | NOT NULL | Aggregation date |
| user_type | ENUM | NOT NULL | 'candidate', 'recruiter' |
| entity_id | UUID | NOT NULL | User/job/candidate ID |
| total_swipes | INTEGER | DEFAULT 0 | Total swipes |
| total_likes | INTEGER | DEFAULT 0 | Right swipes |
| total_dislikes | INTEGER | DEFAULT 0 | Left swipes |
| total_matches | INTEGER | DEFAULT 0 | Matches created |
| average_match_score | DECIMAL(5,4) | NULLABLE | Avg match quality |
| created_at | TIMESTAMP | DEFAULT NOW() | Record creation |

**Indexes:**
- `idx_swipe_analytics_date` on (date)
- `idx_swipe_analytics_entity` on (entity_id)

---

## Future-Proofing Tables (Not MVP)

### 29. **premium_features** (Future)

Premium feature tracking

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Feature identifier |
| feature_name | VARCHAR(100) | UNIQUE, NOT NULL | Feature key |
| feature_description | TEXT | NULLABLE | Feature description |
| is_active | BOOLEAN | DEFAULT TRUE | Feature enabled status |

---

### 30. **user_premium_features** (Future)

User-specific premium feature access

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Access identifier |
| user_id | UUID | FOREIGN KEY → users(id) | User reference |
| feature_id | UUID | FOREIGN KEY → premium_features(id) | Feature reference |
| granted_at | TIMESTAMP | DEFAULT NOW() | Access granted time |
| expires_at | TIMESTAMP | NULLABLE | Access expiration |

---

### 31. **scheduled_interviews** (Future)

First touchpoint scheduling

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Interview identifier |
| match_id | UUID | FOREIGN KEY → matches(id) | Associated match |
| interview_type | ENUM | NOT NULL | 'phone_call', 'video_call', 'physical_interview' |
| scheduled_at | TIMESTAMP | NOT NULL | Interview time |
| duration_minutes | INTEGER | DEFAULT 30 | Expected duration |
| meeting_url | VARCHAR(500) | NULLABLE | Video call link |
| location_address | TEXT | NULLABLE | Physical meeting address |
| integration_provider | VARCHAR(50) | NULLABLE | 'google_meet', 'zoom', 'teams' |
| external_event_id | VARCHAR(255) | NULLABLE | Provider event ID |
| status | ENUM | NOT NULL, DEFAULT 'scheduled' | 'scheduled', 'completed', 'cancelled', 'no_show' |
| created_by_user_id | UUID | FOREIGN KEY → users(id) | Creator |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |
| cancelled_at | TIMESTAMP | NULLABLE | Cancellation time |

**Indexes:**
- `idx_interviews_match` on (match_id)
- `idx_interviews_scheduled` on (scheduled_at)
- `idx_interviews_status` on (status)

---

### 32. **starred_jobs** (Future - Premium)

Jobs starred/bookmarked by candidates

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Star identifier |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Candidate reference |
| job_id | UUID | FOREIGN KEY → jobs(id) | Starred job |
| starred_at | TIMESTAMP | DEFAULT NOW() | When starred |

**Indexes:**
- `idx_starred_candidate` on (candidate_id)
- `idx_starred_job` on (job_id)
- `idx_starred_lookup` on (candidate_id, job_id) UNIQUE

---

### 33. **match_history_archive** (Future - Premium)

Historical match data for premium users

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Archive identifier |
| original_match_id | UUID | NOT NULL | Original match ID |
| candidate_id | UUID | FOREIGN KEY → candidate_profiles(id) | Candidate reference |
| job_id | UUID | FOREIGN KEY → jobs(id) | Job reference |
| recruiter_id | UUID | FOREIGN KEY → recruiter_profiles(id) | Recruiter reference |
| matched_at | TIMESTAMP | NOT NULL | Match creation time |
| cancelled_at | TIMESTAMP | NOT NULL | Match cancellation time |
| cancellation_reason | TEXT | NULLABLE | Optional cancellation reason |
| conversation_data_json | JSONB | NULLABLE | Archived conversation |
| archived_at | TIMESTAMP | DEFAULT NOW() | Archival timestamp |

**Indexes:**
- `idx_history_candidate` on (candidate_id)
- `idx_history_job` on (job_id)
- `idx_history_archived` on (archived_at)

---

## Key Relationships Summary

### One-to-One Relationships

- `users` ↔ `candidate_profiles` (1:1)
- `users` ↔ `recruiter_profiles` (1:1)
- `matches` ↔ `conversations` (1:1)

### One-to-Many Relationships

- `candidate_profiles` → `candidate_skills` (1:N, max 5)
- `candidate_profiles` → `candidate_wanted_positions` (1:N, max 5)
- `candidate_profiles` → `candidate_wanted_locations` (1:N)
- `candidate_profiles` → `candidate_experiences` (1:N)
- `candidate_profiles` → `candidate_education` (1:N)
- `candidate_profiles` → `candidate_certificates` (1:N)
- `candidate_profiles` → `candidate_projects` (1:N)
- `candidate_profiles` → `candidate_achievements` (1:N)
- `candidate_profiles` → `candidate_languages` (1:N)
- `candidate_profiles` → `candidate_desired_seniority` (1:N)
- `recruiter_profiles` → `jobs` (1:N)
- `jobs` → `job_skills` (1:N, max 5)
- `conversations` → `messages` (1:N)
- `users` → `user_sessions` (1:N)

### Many-to-Many Relationships (via junction tables)

- `candidates` ↔ `jobs` via `candidate_swipes`
- `recruiters` ↔ `candidates` via `recruiter_swipes` (context: job)
- `candidates` ↔ `jobs` via `matches`

---

## Database Design Principles

### 1. **Normalization**

- 3NF compliance to minimize redundancy
- Separate tables for repeating groups (skills, experiences, etc.)
- Clear separation of concerns (profiles, actions, analytics)

### 2. **Extensibility**

- `JSONB` columns for flexible metadata storage
- Enum types easily expandable
- Future tables pre-designed but not enforced
- MVP-specific columns clearly marked with `_mvp` suffix

### 3. **Performance Optimization**

- Strategic indexes on foreign keys and query columns
- Composite indexes for common query patterns
- Soft deletes preserve data for analytics
- Cached matching signals reduce computation

### 4. **Data Integrity**

- Foreign key constraints enforce referential integrity
- Check constraints for data validation (enforced at application level where complex)
- Unique constraints prevent duplicates
- NOT NULL constraints ensure required data

### 5. **Security & Compliance**

- Password hashing (never store plaintext)
- OAuth token encryption
- Comprehensive audit logging
- Soft deletes for GDPR compliance
- IP address tracking for security

### 6. **Scalability Considerations**

- UUID primary keys for distributed systems
- Timestamp-based partitioning ready (e.g., `audit_logs`, `messages`)
- Read replicas supported by immutable historical data
- Caching layer supported by `expires_at` patterns

---

## Critical Business Logic Enforced by Schema

### Swipe Mechanics

1. **No Double-Viewing**: `UNIQUE` constraints on swipe tables prevent duplicate swipes
2. **Real-Time Locking**: `card_locks` table with expiration ensures single-viewer access
3. **Cross-Recruiter Coordination**: `recruiter_swipes` dislike applies to all recruiters for same job
4. **Match Prevention**: Cancelled matches tracked to prevent re-matching same job

### Match Lifecycle

1. **Mutual Consent**: Match created only when both `candidate_swipes.like` and `recruiter_swipes.like` exist
2. **Conversation Creation**: Automatic conversation spawned on match
3. **Cancellation Tracking**: `cancelled_by_user_id` preserves who ended relationship
4. **Historical Preservation**: Deleted matches moved to archive (future premium feature)

### Algorithm Intelligence

1. **Pre-Computed Scores**: `matching_signals` table caches expensive calculations
2. **Feedback Loop**: `match_feedback_events` enables continuous learning
3. **Market Dynamics**: `regional_demand_supply` influences card prioritization
4. **Explainability**: Individual signal scores enable transparent recommendations

---

## Technical Implementation Notes

### Technologies

- **Database**: PostgreSQL (JSONB, advanced indexing, partitioning)
- **Caching**: Redis (session management, card locks, matching signals)
- **File Storage**: AWS S3 (videos, logos)
- **Video Processing**: FFmpeg (validation, transcoding)
- **Real-Time**: WebSockets (chat, card lock notifications)
<!-- - **Search**: Elasticsearch (future: manual search for premium users) -->

### Critical Queries to Optimize

1. **Swipe Queue Generation**: 
   ```sql
   SELECT jobs.* FROM jobs
   LEFT JOIN candidate_swipes ON jobs.id = candidate_swipes.job_id AND candidate_swipes.candidate_id = ?
   WHERE candidate_swipes.id IS NULL
   AND jobs.status = 'active'
   AND jobs.expiration_date > NOW()
   ORDER BY matching_signals.total_match_score DESC
   LIMIT 10
   ```

2. **Card Lock Check**:
   ```sql
   SELECT * FROM card_locks
   WHERE entity_type = ? AND entity_id = ?
   AND expires_at > NOW()
   FOR UPDATE SKIP LOCKED
   ```

3. **Match Detection**:
   ```sql
   SELECT * FROM candidate_swipes cs
   INNER JOIN recruiter_swipes rs ON cs.job_id = rs.job_id AND cs.candidate_id = rs.candidate_id
   WHERE cs.swipe_direction = 'like' AND rs.swipe_direction = 'like'
   AND NOT EXISTS (SELECT 1 FROM matches WHERE candidate_id = cs.candidate_id AND job_id = cs.job_id)
   ```

### Data Retention Policies

- **Active Data**: Indefinite (users, profiles, jobs, matches)
- **Swipe History**: 2 years (analytics, ML training)
- **Chat Messages**: Indefinite (unless deleted by user)
- **Audit Logs**: 7 years (compliance)
- **Sessions**: Auto-expire after 30 days inactivity
- **Card Locks**: Auto-expire after 30 seconds
- **Matching Signals**: Re-calculate daily, cache 24 hours


## API Endpoint Design (REST over HTTPS)

This section defines the MVP HTTP API surface for iOS/Android/Web. Style: resource‑oriented, JSON only, idempotent `PUT`, partial updates via `PATCH`. All timestamps ISO‑8601 UTC.

### Conventions

* Base URL: `https://api.careerswipe.app/v1`
* Auth: `Authorization: Bearer <JWT>` unless noted.
* Ids: UUID v4 (lowercase, hyphenated).
* Errors: RFC 7807 Problem+JSON.
* Pagination: `?page[size]=N&cursor=opaque` (forward‑only cursor).
* Filtering: `?filter[field]=value`.
* Sorting: `?sort=-field,field2`.
* Rate limits: `429` with headers `X-RateLimit-*`.

### Problem+JSON Error

```json
{
  "type": "https://docs.careerswipe.app/errors/validation",
  "title": "Validation failed",
  "status": 422,
  "detail": "salary_min must be <= salary_max",
  "instance": "/v1/jobs/…",
  "errors": [{"path": "salary_min", "code": "lte", "message": "…"}]
}
```

### Auth & Session

* `POST /auth/linkedin/start` → { `auth_url` }
* `POST /auth/linkedin/callback` (no auth) → Exchange `code` → `{ access_token, refresh_token, expires_in, user }`
* `POST /auth/token` → exchange `refresh_token` → `{ access_token, expires_in }`
* `POST /auth/logout` → revoke refresh, invalidate sessions
* `GET /auth/me` → current user & derived profile

#### Request/Response

```http
POST /auth/linkedin/callback
{
  "code": "<oauth-authorize-code>",
  "redirect_uri": "https://app.careerswipe.app/oauth"
}
→ 200
{
  "access_token": "jwt…",
  "refresh_token": "r.jwt…",
  "expires_in": 3600,
  "user": {"id": "…", "user_type": "candidate|recruiter", "email": "…"}
}
```

### Users

* `GET /users/{id}` (self or admin‑scope)
* `PATCH /users/{id}` (self) — fields: `phone`, `language`, `is_premium`
* `DELETE /users/{id}` — soft delete (GDPR)

### Candidate Profiles

* `GET /candidates/{id}`
* `PATCH /candidates/{id}` — updatable core fields
* `PUT /candidates/{id}/video` — signed‑upload workflow (YouTube/S3 proxy), returns canonical `video_url`
* `GET /candidates/{id}/skills` · `PUT /candidates/{id}/skills` (replace up to 5)
* `GET /candidates/{id}/experiences` · `POST /candidates/{id}/experiences` · `PATCH /experiences/{exp_id}` · `DELETE /experiences/{exp_id}`
* Similar CRUD for: `education`, `certificates`, `projects`, `achievements`, `languages`, `wanted-positions`, `wanted-locations`, `desired-seniority`.

### Recruiters & Jobs

* `GET /recruiters/{id}` · `PATCH /recruiters/{id}`
* `GET /recruiters/{id}/jobs?status=active` (cursor)
* `POST /jobs` · `GET /jobs/{id}` · `PATCH /jobs/{id}` · `DELETE /jobs/{id}` (archive)
* `PUT /jobs/{id}/video` — signed upload
* `PUT /jobs/{id}/skills` — replace up to 5

#### Job JSON (trimmed)

```json
{
  "id": "uuid",
  "recruiter_id": "uuid",
  "job_title": "iOS Engineer",
  "job_description": "≤500 chars…",
  "salary_min": 50000,
  "salary_max": 70000,
  "salary_currency": "EUR",
  "location_city": "Amsterdam",
  "location_country": "NL",
  "is_remote": true,
  "is_onsite": false,
  "is_hybrid": false,
  "seniority_level": "medior",
  "video_url": "https://youtu.be/...",
  "status": "active",
  "expiration_date": "2026-02-01T00:00:00Z",
  "skills": ["Swift", "iOS", "UIKit"],
  "created_at": "…",
  "updated_at": "…"
}
```

### Swipe & Matching

* Candidate swipes on jobs

  * `POST /swipes/candidates` `{ candidate_id, job_id, direction: "like|dislike" }` → `201`
* Recruiter swipes on candidates (per job)

  * `POST /swipes/recruiters` `{ recruiter_id, job_id, candidate_id, direction }` → `201`
    * Swipe queues

  * `GET /feed/candidates?cursor=…` → ranked jobs for current candidate
  * `GET /feed/recruiters?job_id=…&cursor=…` → ranked candidates for that job
* Matches

  * `GET /matches?status=active`
  * `DELETE /matches/{id}` → cancel with reason

### Card Locks (Real‑Time Safety)

* `POST /locks` `{ entity_type, entity_id }` → `201 { lock_id, expires_at }`
* `DELETE /locks/{id}`
* (Also mirrored over WebSocket events, §5)

### Chat

* `GET /matches/{match_id}/conversation`
* `POST /matches/{match_id}/messages` `{ message_type, message_content }`
* `GET /matches/{match_id}/messages?cursor=…`
* `POST /matches/{match_id}/messages/{id}/read`

### Discovery & Explainability

* `GET /explanations/candidate/{candidate_id}/job/{job_id}` → per‑pair signal breakdown
* `GET /regions/demand-supply?city=…&country=…&skill=…&date=YYYY-MM-DD` → snapshot

### Admin (Future‑flagged; hide in PROD until ready)

* `GET /admin/audit-logs?…` (staff JWT with `role=admin`)

---

## Authentication Flow (LinkedIn OAuth2 + JWT)

### Tokens

* **Access Token**: JWT (RS256), 60 minutes. Claims: `sub`, `typ`, `user_type`, `scopes`, `session_id`, `iat`, `exp`.
* **Refresh Token**: JWT (AES‑GCM encrypted at rest), 30 days, rotatable.

### Sequence

1. Client → LinkedIn authorize (scopes: `r_liteprofile`, `r_emailaddress`).
2. Backend exchanges code → gets LinkedIn access token.
3. Backend fetches profile + email; upserts `users` & `{candidate|recruiter}_profiles`.
4. Backend issues `access_token` + `refresh_token`.
5. Client stores tokens (Keystore/Keychain/SecureStorage).
6. Silent refresh on 401 or 5 minutes before expiry.

### Security Controls

* PKCE for mobile/web.
* Refresh token rotation with reuse detection → revoke session on theft.
* IP + UA fingerprint bound to session (`user_sessions`).
* `X-Content-Type-Options: nosniff`, `HSTS`, `SameSite=Lax` for web cookies (if used).
* SCIM‑like deletion export for GDPR (account deletion queues PII purge jobs).

---

## Matching Algorithm Specification (MVP → v1)

### MVP Heuristic Scoring

Total score ∈ [0,1].

```
score = w_skill*S + w_location*L + w_salary*P + w_seniority*R + w_remote*W + w_recency*C + w_market*M
```

Default weights (tuneable via feature flags):

* `w_skill=0.35`, `w_location=0.15`, `w_salary=0.15`, `w_seniority=0.10`, `w_remote=0.10`, `w_recency=0.10`, `w_market=0.05`.

Signals:

* **S (Skill overlap)**: Jaccard over top‑5 skills (synonym map).
* **L (Location)**: 1.0 same city; 0.7 same country; 0.4 within candidate wanted regions; else 0.
* **P (Salary compatibility)**: 1.0 if `[salary_min, salary_max]` overlaps candidate desired; piecewise linear penalty otherwise.
* **R (Seniority)**: mapping distance (exact=1, off by 1=0.6, else 0.2).
* **W (Work arrangement)**: match of `remote/onsite/hybrid` vs candidate `remote_preference`.
* **C (Recency)**: `exp(-age_days/30)` for job freshness.
* **M (Market dynamics)**: normalize demand/supply for city+skill; higher when jobs/candidates is low.

### Exploration/Exploitation

* ε‑greedy feed injection: with ε=0.1, randomly insert 1 out of 10 cards from long tail candidates/jobs to learn.
* Diversity constraint: max 2 cards in a row with identical `company_id` / `skill cluster`.

### Learning Loop (v1)

* Train weekly logistic model (PyTorch) predicting `match_created` → `chat_initiated` as positive label.
* Features: above signals + interaction features (time‑of‑day, dwell time quartiles, prior likes/dislikes by cluster).
* Calibrate with Platt scaling; fallback to heuristic when model confidence < 0.55.

### Explainability API

Return top‑3 contributing signals with reason phrases, e.g.:

```json
{"job_id":"…","candidate_id":"…","top_factors":[
 {"key":"skills","weight":0.35,"value":0.8,"reason":"4/5 required skills match"},
 {"key":"salary","weight":0.15,"value":1.0,"reason":"Salary fits your range"},
 {"key":"location","weight":0.15,"value":0.7,"reason":"Same country"}
]}
```

---

## UI/UX Wireframes (textual blueprint)

### Login / Registration

* Header: Logo center.
* Buttons: `Continue with LinkedIn` (primary), `Use email` (secondary).
* Footer links: Terms, Privacy.

### Main Swipe View (Candidate)

* Top bar: Logo (left), Matches badge (center), Profile avatar (right).
* Card: full‑bleed; stacked info sections (video/thumbnail → title → company → salary → chips for skills → location & remote badges).
* Bottom: `✖` (left), `ⓘ` (center sheet), `♥` (right). Swipe gestures left/right.
* Info sheet: scroll with full description, company blurb, perks.

### Main Swipe View (Recruiter)

* Job selector pill at top (if multiple jobs).
* Candidate card: avatar/video, name+current title, 3 bullets of experience, skills chips, languages, location, salary expectation badge.

### Matches View

* Grid list with chat preview; unread indicator dot; swipe‑to‑delete with confirm.

### Chat View

* Standard bubble view; read receipts; long‑press to delete own message; paste link unfurl (server‑side scraper disabled in MVP — show as plain link).

### Job Creation/Edit

* Multi‑step form (Stepper): Basics → Details → Skills → Video → Preview & Publish.
* Validation inline; max lengths; live counter for 500‑char description.

---

## Real‑Time Architecture (WebSocket)

### Connection

* URL: `wss://rt.careerswipe.app/v1/socket`
* Auth: JWT in `Sec-WebSocket-Protocol: bearer,<jwt>` (fallback: query `?token=` for web only).

### Events (JSON, envelope)

```json
{ "type": "event", "topic": "matches", "op": "created", "data": { … }, "ts": "…" }
```

Topics & Ops:

* `locks`: `acquired`, `released`, `expired`  — `{ entity_type, entity_id, lock_id, expires_at, by_user }`
* `feed`: `refill` — server tells client to fetch next page
* `matches`: `created`, `cancelled`
* `messages`: `created`, `read`
* `presence` (future): `join`, `leave`

### Backpressure & QoS

* Heartbeat `ping` every 20s; disconnect on 3 missed.
* Server buffer limit 100 messages/conn; drop oldest with `op: "dropped"` notice.

---

## Video Integration (YouTube + S3 Fallback)

### Candidate/Job 30s Video

* Accept: MP4/H.264, ≤ 50 MB, ≤ 30 s, 1080p max.
* Flow:

  1. Client requests `PUT /…/video` → backend returns short‑lived upload URL (to S3) and a job id.
  2. Backend transcodes/validates (FFmpeg on AWS Lambda or ECS Fargate task).
  3. On success, backend uploads to YouTube (unlisted), stores canonical `video_url` (YouTube), deletes S3 temp.
  4. Failure → status `processing_failed` with reason.

### Privacy & Abuse

* Only unlisted links; disallow comments.
* Hash perceptual fingerprint to detect re‑uploads of banned content.
* Content moderation queue (manual for MVP, automated later).

---

## Testing Strategy & QA Gates

### Levels

* **Unit**: Rust (Axum) — handlers, services, repos (use `sqlx::query!` compile‑time checks). Python ML unit tests with PyTest.
* **Integration**: Spin Postgres+Redis+MinIO in Docker; run API conformance via `OpenAPI` contract tests.
* **Load**: k6 scenarios — swipe feed (p95 < 150ms at 1k RPS), chat send/receive latency p95 < 300ms.
* **E2E**: Mobile Detox/XCUITest smoke; Web Playwright flows (register → swipe → match → chat).
* **Security**: JWT fuzz, SSRF hardening on link unfurls (disabled), OAuth misuse (PKCE), rate‑limit tests.

### CI/CD (GitHub Actions)

* Jobs: `lint` (rustfmt, clippy), `test`, `db-migrate-dryrun`, `oas-validate`, `docker-build`, `k6-smoke` (nightly), `deploy-staging`.
* Quality gates: All tests green; coverage ≥ 80% core; OpenAPI diff approved.

### Test Data

* Seeding script creates: 2000 candidates × 5 skills, 500 jobs, balanced locations, synthetic chats.

---

## OpenAPI 3.1 (MVP extract)

```yaml
openapi: 3.1.0
info:
  title: CareerSwipe API
  version: 0.1.0
servers:
  - url: https://api.careerswipe.app/v1
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    Problem:
      type: object
      properties:
        type: {type: string}
        title: {type: string}
        status: {type: integer}
        detail: {type: string}
        instance: {type: string}
        errors:
          type: array
          items:
            type: object
            properties:
              path: {type: string}
              code: {type: string}
              message: {type: string}
    Job:
      type: object
      required: [id, recruiter_id, job_title, salary_min, salary_max, salary_currency, location_city, location_country, status, expiration_date]
      properties:
        id: {type: string, format: uuid}
        recruiter_id: {type: string, format: uuid}
        job_title: {type: string, maxLength: 200}
        job_description: {type: string, maxLength: 500}
        salary_min: {type: number}
        salary_max: {type: number}
        salary_currency: {type: string, maxLength: 3}
        location_city: {type: string}
        location_country: {type: string}
        is_remote: {type: boolean}
        is_onsite: {type: boolean}
        is_hybrid: {type: boolean}
        seniority_level: {type: string, enum: [intern, junior, medior, senior, manager, exec]}
        video_url: {type: string}
        status: {type: string, enum: [active, paused, expired, archived]}
        expiration_date: {type: string, format: date-time}
        skills:
          type: array
          maxItems: 5
          items: {type: string}
paths:
  /auth/me:
    get:
      security: [{ bearerAuth: [] }]
      responses:
        '200': { description: OK }
  /jobs:
    post:
      security: [{ bearerAuth: [] }]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Job'
      responses:
        '201': { description: Created }
  /feed/candidates:
    get:
      security: [{ bearerAuth: [] }]
      parameters:
        - in: query
          name: cursor
          schema: {type: string}
      responses:
        '200': { description: OK }
```

---

## Axum (Rust) Route Skeletons (excerpt)

```rust
pub fn routes() -> Router {
  Router::new()
    .route("/v1/auth/me", get(auth::me))
    .route("/v1/jobs", post(jobs::create))
    .route("/v1/jobs/:id", get(jobs::get).patch(jobs::update).delete(jobs::archive))
    .route("/v1/feed/candidates", get(feed::candidate_feed))
    .route("/v1/swipes/candidates", post(swipes::candidate_swipe))
    .route("/v1/swipes/recruiters", post(swipes::recruiter_swipe))
    .route("/v1/matches", get(matches::list))
    .route("/v1/matches/:id", delete(matches::cancel))
    .route("/v1/matches/:id/messages", get(messages::list).post(messages::create))
}
```

---

## Data & Cache Contracts (MVP)

### Redis Keys

* `lock:{entity_type}:{entity_id}` → `{ lock_id, user_id, expires_at }` (TTL=30s)
* `feed:candidate:{candidate_id}` → sorted set of job_ids by score (TTL=10m)
* `feed:recruiter:{job_id}` → sorted set of candidate_ids (TTL=10m)
* `session:{session_id}` → metadata & refresh rotation guard

### Idempotency

* Header `Idempotency-Key` on POST (swipes, messages, video upload init). Server stores response for 24h keyed by user+path+key.

## Environment & Config

* Secrets: AWS KMS; parameter store with strict IAM.
* Feature flags: `matching.weights`, `feed.epsilon`, `video.enabled`.
* Observability: traces (OTLP → Tempo), logs (JSON → Loki), metrics (Prometheus). SLOs with alerting (PagerDuty).

## Open Questions / Decisions Needed

1. Legal basis for storing salary expectations (consent vs. legitimate interest).
2. Jurisdiction for data residency (EU region recommended).
3. Abuse/misuse policy thresholds (video moderation SLA).
4. Mobile push provider (Firebase/APNS) timeline for post‑MVP notifications.
