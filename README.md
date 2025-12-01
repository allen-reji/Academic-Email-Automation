# Academic Email Automation

## Smart Email Management System for Students using FastAPI, UiPath RPA, and LLMs

### Overview

This project demonstrates an intelligent email-automation platform designed to help students manage the overwhelming volume of placement-related communication. Students at VIT receive dozens of emails every week containing recruitment announcements, deadlines, shortlists, and general academic notifications. Monitoring these manually is time-consuming and often leads to missed opportunities.

This system automates the entire workflow:

* Extracting emails
* Classifying important messages
* Performing eligibility checks
* Reading shortlist/selection attachments
* Summarising long emails using an LLM
* Generating ICS calendar invites via UiPath
* Displaying all results through a clean web interface

The solution integrates **UiPath RPA** for inbox automation and **FastAPI + SQLite + HTML/JS** for backend and frontend logic, creating a fully functional end-to-end academic email assistant.

---

# Table of Contents

1. [Project Motivation](#project-motivation)
2. [Key Features](#key-features)
3. [Architecture](#architecture)
4. [Tech Stack](#tech-stack)
5. [System Workflow](#system-workflow)
6. [Installation](#installation)
7. [Running the System](#running-the-system)
8. [UiPath Workflow Integration](#uipath-workflow-integration)
9. [Email Classification Logic](#email-classification-logic)
10. [Limitations & Future Work](#limitations--future-work)
11. [Contributors](#contributors)

---

# Project Motivation

Students are exposed to an overwhelming volume of daily emails, and manually identifying critical placement-related communication is inefficient and error-prone. Important details such as eligibility criteria, deadlines, interview schedules, and shortlists are often missed.

This project solves that problem by creating an automated system that transforms raw inbox data into **structured, actionable insights** that students can trust.

---

# Key Features

### 1. Automated Email Extraction (UiPath)

* Reads unread inbox emails
* Downloads and scans attachments
* Sends extracted data to backend for analysis

### 2. Intelligent Email Classification

Emails are automatically categorized into:

1. **New Company Announcement**
2. **Shortlist / Round Progression**
3. **Final Selection**
4. **General Academic Notices**

Classification uses both keyword logic and LLM-based semantic understanding.

### 3. Eligibility Checker

* Extracts CGPA, branch, 10th/12th marks from the user profile (SQLite)
* Compares with company criteria
* Displays “Eligible” / “Not Eligible” instantly

### 4. Excel Shortlist Processing

* Parses Excel sheets automatically
* Checks if student's name or registration number appears
* Works even when names are inside attachments

### 5. LLM-Based Email Summaries

Long announcements are summarized intelligently to quickly surface key information.

### 6. Calendar Automation (ICS Generation)

Via UiPath:

* Automatically identifies dates from emails
* Generates ICS files
* Sends them to student mailbox for one-click calendar import

### 7. Web Dashboard

Includes:

* Signup & login
* User profile view
* Dashboard showing categorized emails
* Embedded summaries, calendar buttons

---

# Architecture

### High-Level Architecture Diagram

```
                ┌──────────────┐
                │ Student Inbox│
                └───────┬──────┘
                        │ (RPA reads emails)
                  ┌─────▼──────┐
                  │   UiPath   │
                  │  Workflows │
                  └─────┬──────┘
                        │ Extracted data (JSON + files)
                  ┌─────▼──────────┐
                  │   FastAPI API  │
                  └─────┬──────────┘
                        │
      ┌─────────────────▼──────────────────┐
      │ Classification | Eligibility | LLM │
      └─────────────────┬──────────────────┘
                        │
               ┌────────▼─────────┐
               │  SQLite Database │
               └────────┬─────────┘
                        │
              ┌─────────▼──────────┐
              │   Web Frontend     │
              │  (HTML/CSS/JS)     │
              └────────────────────┘
```

Each component communicates seamlessly, forming a hybrid **RPA + AI-driven academic assistant**.

---

# Tech Stack

### Backend

* **Python**
* **FastAPI**
* **SQLite (users.db)**
* **SQLAlchemy ORM**

### Frontend

* HTML
* CSS
* JavaScript

### Automation (RPA)

* **UiPath Studio**
* Email Activities
* Excel Processing
* File Handling
* ICS Event Creation

### AI / NLP

* LLM-based summarization
* Semantic extraction
* Deadline recognition

# System Workflow

### 1. UiPath Workflow

* Fetch unread emails
* Extract content & attachments
* Send POST request to FastAPI

### 2. FastAPI Processing

* Parse email
* Detect category
* Run eligibility logic
* Scan Excel files if needed
* Generate summary using NLP
* Store results into SQLite DB

### 3. User Dashboard

* Students view categorized alerts
* Can add deadlines to calendar
* Can view summaries & decisions instantly

---

# Installation

### Step 1: Clone Repository

```
git clone https://github.com/allen-reji/Academic-Email-Automation
cd Academic-Email-Automation
```

### Step 2: Create Virtual Env

```
python -m venv venv
source venv/bin/activate         (Linux/Mac)
venv\Scripts\activate            (Windows)
```

### Step 3: Install Dependencies

```
pip install -r requirements.txt
```

### Step 4: Initialize Database

The repository already includes `users.db`.
If you wish to recreate it:

```
python
>>> from app.database import Base, engine
>>> Base.metadata.create_all(engine)
>>> exit()
```

---

# Running the System

### Start Backend Server

```
uvicorn app.main:app --reload --port 5000
```

### Access Frontend

Open:

```
http://localhost:5000
```

HTML files are located under:

```
app/static/
```

---

# UiPath Workflow Integration

Two main workflows are used:

### 1. **Email Extraction Bot**

* Reads student inbox
* Downloads Excel/PDF attachments
* Sends content to FastAPI using HTTP Request activity

### 2. **ICS Calendar Bot**

* Creates ICS file
* Sends it to student mailbox
* Triggered via backend request

The workflows correspond to `.nupkg` files included in the repo.

---

# Email Classification Logic

### **Type 1 — Company Announcements**

* Extracts CGPA, backlog criteria
* Matches with user data
* Returns **Eligible / Not Eligible**


### **Type 2 — Shortlist Notifications**

* Reads Excel attachments
* Checks for student registration number


### **Type 3 — Final Selection**

* Detects student name in selection lists


### **Type 4 — General Academic Notices**

* LLM generates readable summary


---

# Limitations & Future Work

### Current Limitations

* Not yet deployed for large-scale multi-user usage
* Limited to VIT-style email formats
* Backend uses SQLite, not cloud database
* LLM integration is local; cloud models can be added later

### Planned Improvements

* Mobile app with push notifications
* Multi-user authentication system
* Cloud deployment with PostgreSQL
* Advanced deadline extraction with NER
* Batch processing of emails

---

# Contributors

| Name                       |Email ID                         |
| -------------------------- | --------------------------------|
| **Nidhish Balasubramanya** | nidhishbalasubramanya@gmail.com |
| **A Nathania Rachael**     | nathaniarachael@gmail.com       |
| **Allen Reji**             | allenreji@gmail.com             |
| **Jacob Cherian M**        | jakecherian10@gmail.com         |



