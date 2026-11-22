# Project Proposal & MVP Specification: Tal-Bot

**An Intelligent AI Agent for Academic Regulations & Student Rights — Jerusalem College of Technology (JCT)**

---

## 1. Introduction & Background

The Academic Regulations (*Takanon*) of the Jerusalem College of Technology (JCT / Lev Academic Center) is a comprehensive, multi-page legal-academic document. It governs all aspects of student life, including reserve duty (*Miluim*) accommodations, pregnancy and childbirth rights, special exam sessions (*Moed Muechad* / *Moed Gimmel*), grade appeal procedures, and campus-specific Judaic studies (*Kodesh* / *Midrasha*) requirements.

When critical situations arise—such as emergency military call-ups, family events, or medical leaves—students often struggle to navigate this dense document. A correct answer frequently requires cross-referencing multiple sections, understanding specific personal constraints (e.g., exact number of reserve duty days, weeks elapsed post-childbirth, or specific campus affiliation), and meeting strict submission deadlines. As a result, students miss out on entitled accommodations or place a heavy administrative burden on secretarial staff and student representatives.

**Tal-Bot** addresses this challenge by providing an **Intelligent Multi-Turn Conversational Agent based on Retrieval-Augmented Generation (RAG)**. The system allows students to ask queries in natural language, actively identifies missing context through clarifying dialogues, retrieves relevant clauses directly from the official JCT Academic Regulations, and delivers grounded, actionable responses backed by exact citations.

---

## 2. Project Goals & Vision

1. **Instant, Personalized Guidance:** Provide 24/7 precise answers tailored to the student's campus (Lev, Tal, or Lustig), degree program, and personal circumstances.
2. **Strict Grounding & Zero Hallucination Focus:** Every response must cite exact clause numbers and appendices from the official JCT Academic Regulations.
3. **Agentic Multi-Turn Interaction:** When a student provides an incomplete query (e.g., *"I did military reserve duty, what are my rights?"*), the agent actively asks targeted follow-up questions (Disambiguation Dialog) to verify eligibility conditions rather than making assumptions.
4. **Actionable Step-by-Step Guidance:** Provide clear instructions on immediate next steps, including required documentation, deadlines, and submission channels (e.g., *Lev-Net* portal or Academic Secretariat).

---

## 3. System Architecture & Tech Stack

The system architecture consists of three core layers:

```
[ User Input / Chat Interface ]
               │
               ▼
 [ Multi-Turn Agent Controller ] ──── (Detects missing parameters / Clarification loop)
               │
               ▼
  [ RAG Pipeline (Embedding + Retriever) ]
               │
               ├─► [ Vector DB / Knowledge Base ] ── (Indexed Academic Regulations)
               │
               ▼
   [ LLM Core (Gemini Engine) ] ──────► [ Grounded Response + Citations + Actionable Steps ]
```

### Key Technical Components:
* **Vector Database & Semantic Chunking:** Structured chunking of the JCT Academic Regulations PDF/Markdown text with metadata tagging (section numbers, campus specificity, event category).
* **Retrieval-Augmented Generation (RAG):** Contextual retrieval algorithm fetching top-k relevant bylaws for any query.
* **Conversational Agent Framework:** Context-aware dialogue controller handling multi-turn history, parameter extraction, and clarification prompting.
* **LLM Engine:** Gemini 3.1 Pro for accurate Hebrew & English natural language understanding, reasoning, and synthesis.

---

## 4. MVP (Minimum Viable Product) Scope

The initial MVP focuses on three core high-impact scenarios covering the majority of student inquiries:

1. **Scenario 1 (Primary MVP Focus): Life Events & Academic Accommodations**
   * Covers Reserve Duty (*Miluim*), Pregnancy & Childbirth, Marriage, and Bereavement.
   * Handles multi-turn clarifying conversations to compute exact eligibility based on dates and durations.
2. **Scenario 2: Judaic Studies (*Kodesh* / *Midrasha*) Requirements & GPA Calculation**
   * Campus-specific rules (Lev, Tal, Lustig) regarding credit requirements, pass/fail policies, and GPA weighting.
3. **Scenario 3: Exam Regulations, Special Sessions (*Moed Muechad* / *Moed Gimmel*), and Grade Appeals**
   * Rules regarding overlapping exams, illness during finals, grade appeal submission windows, and score revision risks.

---

## 5. Detailed MVP Scenarios & Examples

### Scenario 1: Life Events & Accommodations

#### Example 1.1: Student post-childbirth preparing for exams
* **User Input:** *"I gave birth three weeks ago and have an exam next week. Do I have to take it or am I entitled to another date?"*
* **System Process (Agent + RAG):**
  1. Retrieves **Appendix 3 (Clause 21.4 - Childbirth Accommodations)** and **Clause 12 (Special Exam Dates)**.
  2. Evaluates eligibility: Exam falls within 15 weeks post-childbirth.
* **System Output (Tal-Bot):**
  > Congratulations! According to the Academic Regulations (Clauses 21.4.9 and 21.4.12), because the exam takes place within **15 weeks of childbirth**, you are eligible to defer the exam and receive a **Special Exam Date (*Moed Muechad*)** or defer it to *Moed Gimmel* during the next cycle of the course.
  >
  > **Steps to claim your rights:**
  > 1. Submit your **Birth Certificate** to the Academic Secretariat within 30 days from the start of your maternity leave.
  > 2. Submit a request for a Special Exam Date via the **Lev-Net** portal as early as possible, and no later than two weeks after *Moed Bet* grades are published.
  >
  > *Note:* If you choose to take the exam within 15 weeks post-childbirth, you are entitled to a **25% extra time extension** (with prior approval) and unrestricted bathroom breaks during the test if within 6 weeks post-childbirth.
  >
  > Would you like me to check potential exemptions for missed assignments or attendance during this period?

#### Example 1.2: Reserve duty student who missed an exam (Multi-Turn Clarification)
* **User Input (Turn 1):** *"I did reserve duty this month and missed my Moed Aleph exam. What am I entitled to?"*
* **System Process (Agent):** Detects missing critical variables (number of reserve days, call-up type, prerequisite course status).
* **System Output (Turn 1):**
  > To give you an exact answer based on the Reserve Duty Regulations (Appendix 1, Clause 20), please clarify a few details:
  > 1. How many total days of reserve duty did you serve during the semester/exam period, and was it under Emergency Order (*Tzav 8*)?
  > 2. Is the course you missed a **prerequisite course** for next semester?
* **User Input (Turn 2):** *"I served 12 consecutive days under standard orders during finals, and yes, it is a prerequisite course."*
* **System Output (Final Turn):**
  > Thank you for the details. According to Reserve Duty Regulations (Clauses 20.5.4, 20.5.6, and 20.9):
  > * **Special Exam Session (*Moed Muechad*):** Since you served more than 10 cumulative days during the exam period, you are entitled to a Special Exam Date for any exam held during your service (scheduled within 45 days of the last exam or in the following semester).
  > * **Extra Time:** Having served 10+ days within 4 weeks prior to finals, you are eligible for a **25% extra time extension** on all exams that semester.
  > * **Conditional Advancement:** Because this is a prerequisite course, you may take the advanced course **on a conditional basis** until you complete the exam.
  >
  > **Action Required:** Upload your official Military Form 3010 via the **Lev-Net** portal or email the Reserve Duty Coordinator (`miluim@jct.ac.il`) within 60 days of service completion.

#### Example 1.3: Student getting married
* **User Input:** *"I'm getting married next month. What rights do I have regarding attendance and exams?"*
* **System Output (Tal-Bot):**
  > Congratulations! According to the Academic Regulations:
  > 1. **Approved Absence (Clause 18.7.1):** You are granted an approved absence starting **3 days before the wedding through the end of the Sheva Brachot week**. You are exempt from quizzes held during this period, and instructors must grant deadline extensions or exemptions for assignments (Clause 18.8).
  > 2. **Moed Gimmel Rights (Clause 11.1):** If an exam falls between **3 days prior to the wedding and 2 days post-Sheva Brachot**, your absence is justified and grants you eligibility for a **Moed Gimmel** exam. Submit your request via Lev-Net within two weeks of Moed Bet grade publication.

---

### Scenario 2: Judaic Studies Requirements by Campus

* **User Input:** *"I am studying Software Engineering at Campus Lev. How many Judaic credits do I need, and does the grade count toward my GPA?"*
* **System Output (Tal-Bot):**
  > According to Academic Regulations (Clause 8 & Clause 15.3):
  > 1. **Credit Requirement:** Full-time male students at Campus Lev are required to complete the designated Judaic studies credit quota per semester/year as specified in their track schedule.
  > 2. **GPA Weighting (Clause 15.3):** Graded Judaic studies courses required for degree completion **are factored into your cumulative GPA**, unless explicitly designated as Pass/Fail (*P/F*).
  > 3. **Academic Progression:** Satisfactory progress in Judaic studies is a prerequisite for advancing to the next academic year.

---

## 6. Academic Bylaws Mapping Matrix

| Category | Regulation Section | Key Topics Covered |
| :--- | :--- | :--- |
| **Reserve Duty (*Miluim*)** | Appendix 1 (Clause 20) | Special exam dates, extra time, attendance waivers, conditional prerequisites, credit conversions. |
| **Pregnancy & Childbirth** | Appendix 3 (Clause 21) | Maternity leave, 15-week special exam eligibility, time extensions, bathroom privileges. |
| **Absences & Personal Events** | Clauses 18.7, 18.8 | Weddings, mourning (*Shiva*), illness, assignment deadline extensions, quiz waivers. |
| **Exams & Special Dates** | Clauses 11, 12 | Moed Bet, Moed Muechad, Moed Gimmel, overlapping exam schedules, justified absences. |
| **Grade Appeals** | Clause 14 | Submission windows (within X days of publication), appeal procedure, risk of score reduction. |
| **Judaic Studies (*Kodesh*)** | Clauses 8, 15 | Credit requirements per campus (Lev, Tal, Lustig), GPA calculation, progression rules. |

---

## 7. Evaluation Metrics & Success Criteria

To ensure high quality and reliability for the MVP, the system will be evaluated against the following criteria:

1. **Factuality & Citation Accuracy (> 95%):** Every factual claim must be backed by correct section numbers from the JCT bylaws.
2. **Context Retrieval Precision (Top-K Recall):** High retrieval accuracy ensuring the RAG engine retrieves the exact clause governing the query.
3. **Multi-Turn Disambiguation Success:** The agent's ability to identify ambiguous user input and trigger appropriate clarification prompts.
4. **Actionability:** Clear, step-by-step instructions (forms, portals, contacts, deadlines) in every response.