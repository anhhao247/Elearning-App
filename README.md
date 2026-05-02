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
- **Learning Space** – Study with video lessons (YouTube/Vimeo), reading materials, and interactive quizzes.
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

**Frontend:** Next.js 14, Tailwind CSS, Shadcn/ui

**Backend:** Java Spring Boot 3 (REST API, Spring Data JPA, Spring Security)

**AI Service:** Python FastAPI, LangChain, Google Gemini 2.5 Flash

**Databases:** MySQL 8 (relational), ChromaDB (vector), Redis (cache/session)

**Embedding Model:** `keepitreal/vietnamese-sbert`

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
- Docker & Docker Compose (recommended 2.x)
- Java 17+ & Maven (if running Spring Boot without Docker)
- Node.js 18+ & npm/yarn (if running frontend without Docker)
- Python 3.10+ & pip (if running AI service without Docker)
- A Google AI Studio API key (for Gemini)

### Quick Start with Docker

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/learnly.git
   cd learnly
