# Learnly – AI‑Powered E‑Learning Platform

**Learnly** is a full‑stack online learning platform that integrates Artificial Intelligence to enhance both teaching and learning experiences. Built with a modern microservices architecture, it provides a rich set of features for students, instructors, and administrators, including an AI tutor, automated interview assessment, and AI‑assisted course creation.

---

## Overview

Learnly aims to bridge the gap between traditional LMS (Learning Management Systems) and intelligent, personalized education. The platform allows students to browse, enroll, and study courses with the support of an **AI Tutor Bot** that answers questions based on actual course materials (using RAG). An **AI Interview** module assesses students' knowledge through spoken Q&A, and instructors can generate entire course outlines in seconds using the **AI Syllabus Creator**.

The system is designed to simulate a real‑world e‑learning business, complete with online payments via VNPAY, a management dashboard for instructors, and an admin panel for approving instructor accounts.

---

## Features

### 👨‍🎓 For Students
- **Course Marketplace** – Browse, search, and view detailed information about courses.
- **Enrollment & Payment** – Register for free or paid courses; secure online payment via VNPAY integration.
- **Learning Space** – Study with video lessons (YouTube/Cloudflare R2), reading materials, and interactive quizzes.
- **Progress Tracking** – View personal progress for each course and lesson; automatically or manually mark lessons as completed.
- **AI Tutor Bot (RAG Chatbot)** – Ask questions about the current lesson and get instant, accurate answers based on the course content. Also supports summarization and course recommendations.
- **AI Interview** – Participate in an oral interview conducted by AI that evaluates your understanding and provides detailed feedback and scores.
- **Personal Notes** – Take private notes on any lesson (with soft delete).
- **Discussions** – Comment and discuss each lesson with other students and the instructor.
- **Course Reviews** – Rate and review completed courses.

### 👩‍🏫 For Instructors
- **Dashboard** – Monitor revenue, student enrollment, and growth metrics in real‑time.
- **Course Management** – Create and manage courses, modules, and content (video, reading, quiz) with an intuitive UI.
- **Student Management** – View detailed progress and quiz results for each student.
- **AI Syllabus Creator** – Generate a full course outline (modules, lessons, quiz questions) from a simple topic description using AI.
- **Quiz Management** – Create question banks and view statistical results of student submissions.

### 🛡️ For Administrators
- **Instructor Approval** – Review and approve/reject instructor registration requests to ensure quality control.
- **System Overview** – Monitor the number of users, courses, and service health.

---

## Tech Stack

**Frontend:** Next.js 16.2.2, Tailwind CSS, Shadcn/ui

**Backend:** Java Spring Boot 3 (REST API, Spring Data JPA, Spring Security)

**AI Service:** Python FastAPI, LangChain, Google Gemini 2.5 Flash

**Databases:** MySQL 8 (relational), ChromaDB (vector), Redis (cache/session)

**Embedding Model:** `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

**Payment:** VNPAY Sandbox

**DevOps:** Docker, Docker Compose

---

## Architecture

The system follows a **microservices** pattern with three core independent services – Frontend, Business Backend, and AI Service – all containerized and communicating via REST APIs.

<img width="1411" height="601" alt="Khoa luan drawio" src="https://github.com/user-attachments/assets/b6e9118c-3efb-4c0f-b063-6b973701d35d" />


**High‑level flow:**
1. **Next.js** serves the user interface and communicates with the Spring Boot backend.
2. **Spring Boot** handles all business logic, user authentication, course management, and payment processing. It also acts as a gateway for AI requests.
3. **FastAPI** receives AI‑specific tasks (chat, interview, syllabus generation). It embeds queries, retrieves relevant context from ChromaDB, and calls the Gemini LLM to generate answers.
4. **MySQL** stores relational data. **Redis** caches frequent queries and maintains short‑term chat history. **ChromaDB** stores course knowledge and long‑term user memory as vectors.
5. All services are orchestrated by **Docker Compose** for easy local setup.

---

## Getting Started

### Prerequisites
- Docker & Docker Compose
- Java 17+ & Maven (if running Spring Boot without Docker)
- Node.js 18+ & npm/yarn (if running frontend without Docker)
- Python 3.10+ & pip (if running AI service without Docker)
- Gemini API key

### Environment Variables

Before launching the application, you must configure the environment variables. A template file `.env.example` is provided in the root directory.

1. Copy `.env.example` to `.env` in the root:
   ```bash
   cp .env.example .env
   ```

2. Open the `.env` file and customize the essential variables for each service:
   - **Database (MySQL):** `MYSQL_DATABASE` and `MYSQL_ROOT_PASSWORD` for setting up the local db connection.
   - **Redis Connection:** Configurations for `SPRING_REDIS_HOST` and `SPRING_REDIS_PORT` to manage active cache databases.
   - **VNPAY Credentials:** Configure Sandbox merchant info (`VNPAY_TMN_CODE`, `VNPAY_HASH_SECRET`, `VNPAY_URL`, `VNPAY_RETURN_URL`) for secure demo payments.
   - **AI tutor credentials:** Supply `GEMINI_API_KEY` for Google Gemini LLM API calls and `HF_TOKEN` for Hugging Face services.

---

### Quick Start with Docker

1. **Clone the repository**
   ```bash
   git clone https://github.com/anhhao247/Elearning-App.git
   cd learnly
   ```

2. **Configure environment files**
   Create a `.env` file in the root directory using the template:
   ```bash
   cp .env.example .env
   # Edit the values in .env to include your own credentials and API keys
   ```

3. **Launch the services**
   Build and start the containerized application stack in detached mode:
   ```bash
   docker compose up -d --build
   ```

4. **Verify running containers**
   Check the status of the containers:
   ```bash
   docker compose ps
   ```
   Once healthy, access the services:
   - **Frontend:** `http://localhost:3000`
   - **Backend API:** `http://localhost:8080`
   - **AI Service:** `http://localhost:8000`

---

## Manual Local Setup (Alternative)

If you prefer to run the services individually on your local system:

### 1. Database & Redis Setup
- Start **MySQL 8** and create the database:
  ```sql
  CREATE DATABASE elearning_db_v1;
  ```
- Start **Redis** server on port `6379`.

### 2. Run Backend (Spring Boot)
1. Navigate to the backend directory and configure the environment variables in `elearning-be/.env` (refer to `.env.example`).
2. Run the application:
   ```bash
   cd elearning-be
   ./mvnw spring-boot:run
   ```
   The backend API will start on `http://localhost:8080`.

### 3. Run AI Service (FastAPI)
1. Navigate to the AI service directory and create a virtual environment:
   ```bash
   cd tutor-agent
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Set your environment variables in `tutor-agent/.env` and start the server:
   ```bash
   uvicorn main:app --reload --host 0.0.0.0 --port 8000
   ```

### 4. Run Frontend (Next.js)
1. Navigate to the frontend directory:
   ```bash
   cd elearning-fe
   npm install
   ```
2. Start the development server:
   ```bash
   npm run dev
   ```
   Access the UI at `http://localhost:3000`.

---

## API Documentation

The Spring Boot backend exposes a Swagger UI for API exploration and manual execution testing.

- **Swagger UI URL:** [http://localhost:8080/swagger-ui/index.html](http://localhost:8080/api/swagger-ui/swagger-ui/index.html#/) *(Only available when Backend is running)*

This interactive portal lists all controllers, security requirements (JWT bearer tokens), schemas, and provides direct test invocations.

---

## Database Structure (ERD)

The relational schema coordinates students, instructors, courses, quiz answers,...

<img width="1057" height="891" alt="image" src="https://github.com/user-attachments/assets/c67ffa4e-5f82-4cff-954e-1e7197e8a21c" />

---
## Screenshots & Demo

Review screenshot placeholders showing student features, chatbots, and dashboards below.

### 👨‍🎓 Student UI
*Visualizes course catalog search, enrollment, student notes, and learning content panels.*

| | |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/7efaee1c-e750-41d9-9401-f204731a1b80" width="400" /> | <img src="https://github.com/user-attachments/assets/ae2dd0f4-b1c5-4b10-a0d0-f72bdebfe35d" width="400" /> |
| <img src="https://github.com/user-attachments/assets/935bdb8c-4a99-4ace-9b74-ca1b932928e3" width="400" /> | <img src="https://github.com/user-attachments/assets/1aa61553-5ee4-4ff9-bc1c-0ab4d82e873c" width="400" /> |

### 🤖 Chatbot AI Tutor
*Demonstrates interactive lecture assistant powered by Retrieval-Augmented Generation (RAG) using lesson source material.*

| | |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/d40e6d92-e157-4331-ba6d-f59109b2172c" width="400" /> | <img src="https://github.com/user-attachments/assets/5ca16a98-2593-428a-bec6-e4626d578967" width="400" /> |
| <img src="https://github.com/user-attachments/assets/58d5f65a-0b20-4900-a9a9-ca1ae1afdbda" width="400" /> | |

### 👩‍🏫 Instructor Dashboard 
*Syllabus templates builder, revenue tracking diagrams, and quiz metrics panel.*

| | |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/09e3f9ef-35fb-45e9-bd05-fdeecc282d24" width="400" /> | <img src="https://github.com/user-attachments/assets/e5694276-c303-4115-b026-c69a9794fabd" width="400" /> |
| <img src="https://github.com/user-attachments/assets/82d729f1-4cd2-4b43-be56-a696e92b7a61" width="400" /> | |

### 🎯 Admin Dashboard

<img src="https://github.com/user-attachments/assets/ba74e686-d9d8-46f2-aeeb-242c994b57f6" width="800" />

---

## Contact

Get in touch for questions or project suggestions:

- **Email:** [anhhao9a9@gmail.com](mailto:anhhao9a9@gmail.com)
- **LinkedIn:** www.linkedin.com/in/leanhhao
