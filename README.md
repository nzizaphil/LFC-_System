# Campus Lost & Found

## Project overview

Lost items on a busy campus are a daily nuisance. **Campus Lost & Found** is a simple, secure digital platform that helps students and staff report lost or found belongings, upload photos and descriptions, and get matched automatically with potential owners. Administrators use a secure portal to verify reports, confirm matches and help reunite items with their owners. The goal is to reduce the time items spend unclaimed and make the recovery process painless for everyone.

This repository contains the full-stack application:

- **Frontend:** React.js (user & admin interfaces)
- **Backend:** Node.js + Express (REST API)
- **Database:** MongoDB (5 collections)
- **Storage:** Images saved to cloud storage (S3 / similar) with URLs stored in the DB

---

## Who this is for

This README is written for people who may not be familiar with the tech stack. If you know nothing about React, Node, or Mongo — no problem. The app provides a web UI for reporting and browsing items and a secure admin portal for verification. Developers can find the implementation details below.

---

## Core functionality

- Report **Lost** or **Found** items with description, date, location and photos.
- Upload one or more **images** per report.
- Automatic **match suggestions** (text + image similarity, confidence score).
- **Admin verification**: admins review suggestions and mark matches as confirmed or rejected.
- Activity logs and secure retrieval workflow to protect owners’ privacy.

---

## High-level architecture (brief)

1. **React frontend**: Forms for reporting, gallery for browsing, user dashboard, admin portal.
2. **Node.js backend** (Express):
   - RESTful API endpoints for CRUD operations (reports, users, matches).
   - Authentication & role-based access (JWT for users and admins).
   - File-upload endpoints that forward images to cloud storage (e.g., AWS S3).
   - Matching service that runs heuristics (title/description text similarity, optional image-feature similarity).
3. **MongoDB** as the primary datastore. We'll model data in 5 collections (below).
4. **Cloud storage** for image assets; only URLs are saved in MongoDB.

---

## Database design — 3 collections (MongoDB)

> In MongoDB we use “collections” (similar to tables). Below are the 3 collections.

### 1. Users Collection

Holds user accounts (students, staff, admins).

* Simple authentication and profile management
* Role-based access (user/admin)
* Tracks user statistics

---

### **2. Items Collection**

* Single collection for BOTH lost and found items (distinguished by `type` field)
* Embedded reporter information for faster queries
* Built-in verification system for admins
* Auto-expiration after 90 days using TTL index
* Full-text search capability on title, description, and tags

### **3. Matches Collection**

* Stores suggested matches with confidence scores
* Simple embedded messaging system
* Tracks responses from both parties
* Admin can override suggestions

### Backend (Node.js) — implementation outline

This is a simple and practical setup that fits the app requirements.

### Frameworks & libraries

- **Express** for REST API
- **Mongoose** for MongoDB ODM
- **jsonwebtoken** for JWT auth
- **bcrypt** for password hashing
- **multer** for handling multipart/form-data uploads
- Cloud SDK (AWS SDK / Google Cloud Storage) to upload images
- Optional: **TensorFlow.js**, **clarifai**, or an external vision API for image similarity

---

## Security & privacy (important)

- Store **passwords hashed** with bcrypt.
- Use **HTTPS** in production.
- Issue **short-lived JWTs** and refresh tokens for session management.
- Restrict access to admin routes using role-based middleware.
- Limit the amount of sensitive information shown publicly; reveal owner contact details only after admin verification.
- Validate and sanitize all user input to prevent injection attacks.
- Rate-limit endpoints that accept uploads or repeated requests.

---

## **Links**

* **Jira:** [[LFTASK/summary](https://phil-nziza.atlassian.net/jira/software/projects/LFTASK/summary)]
* **Deployed Website:** [[Online](http://3.106.166.181)]
* **Instance Link:** [[Link](https://ap-southeast-2.console.aws.amazon.com/ec2/home?region=ap-southeast-2#InstanceDetails:instanceId=i-0f41cc31bd3de33ea)]
