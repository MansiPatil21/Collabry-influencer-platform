# Collabry: Final Technical Demo Brief

This guide summarizes the "technical heart" of the Collabry platform. Use these bullet points to answer the professor's questions about how the app works "under the hood."

---

## 1. The Architecture (High-Level)
*   **The Stack:** React 19 (Frontend), Spring Boot 3.4 (Backend), MySQL/H2 (Database), and Docker (Orchestration).
*   **The Workflow:** When a user clicks a button (e.g., "Accept Invite"), the **Frontend** sends an Axios request with a **JWT Token** in the header. The **Spring Security** layer verifies the token, and the request is routed to a **REST Controller**. The controller delegates the business logic to a **Service**, which uses a **JPA Repository** to update the database.

---

## 2. Technical Interconnects (The "Business Logic")
*   **The Collaboration Lifecycle:** This is our most complex state machine. 
    *   It starts with a `Campaign` (linked to a User). 
    *   It creates a `CollaborationInvitation`. 
    *   If accepted, it creates a `CollaborationRecord`. 
    *   This link connects 4 different entities: Brand, Influencer, Campaign, and Payment.
*   **Role-Based Access (RBAC):** We use a custom `Role` enum (BRAND, INFLUENCER, ADMIN) to control routing. If an influencer tries to access an `/admin` endpoint, the JWT filter blocks them at the security layer (403 Forbidden).

---

## 3. The AI "Brain" (Groq Integration)
*   **The Implementation:** We built a `GroqApiClient` that communicates with the Groq API (a high-speed LLM service).
*   **The Logic:** We don't just "chat" with the AI. We send **Structured Context** (JSON data about the campaign and influencer) along with a **System Prompt** that acts as a "Marketing Specialist."
*   **The Result:** The AI returns a numerical **Match Score** and a justification, which our backend parses and sends to the frontend.

---

## 4. Software Quality & Principles
*   **SOLID Principles:** We used these to prevent "Spaghetti Code."
    *   *Example:* Our `EmailService` is an interface with two implementations (SMTP and Console). This follows the **Dependency Inversion Principle**, allowing us to swap email providers without changing our Auth logic.
*   **TDD (Test-Driven Development):** We used TDD specifically for the **Cloudinary Image Upload** feature. We wrote the failing tests first, then implemented the controller/service to make them pass.
*   **Code Smells:** We scanned the project with **DesigniteJava** to find design flaws (like "Cyclic Dependencies" or "Large Classes") and refactored them for the final submission.

---

## 5. Potential Demo Questions & Answers
*   **Q: Why use JWT instead of standard Sessions?**
    *   *A: JWT is stateless. This makes our backend scalable and allows us to easily handle authentication across different services (like our AI micro-service) without sharing session state.*
*   **Q: How do you handle database safety?**
    *   *A: We use Spring Data JPA with `@Transactional` annotations. This ensures that if a step in a complex flow (like processing a payment) fails, the entire transaction rolls back and the data stays consistent.*
*   **Q: What was the hardest technical challenge?**
    *   *A: Synchronizing the state machine between the Brand and Influencer dashboards. We had to ensure that when an influencer "Submits Content," the Brand's view updates instantly with the correct deliverable URL and status.*

---

**Good luck! You've got this.**
