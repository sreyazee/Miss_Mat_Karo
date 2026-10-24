# 🚀 Miss Mat Karo AI

> **Never miss an important college deadline again.**

In college, critical notices get buried under hundreds of daily messages across dozens of WhatsApp groups. Missing a single update can cost you marks, fees, or opportunities. 

**Miss Mat Karo Agent** solves this. Just forward any official message, screenshot, or PDF notice to the agent—it extracts the essential details, filters out what isn't relevant to your batch or branch, and sends you timely reminders.

---

## 🎯 How It Works

The system operates using **3 specialized AI agents**:
## 🤖 The 3 Agents

### 1. Collector Agent
* **Input:** Raw forwarded text, screenshot, or PDF notice.
* **Output:** Extracts and standardizes key information into a clear format:
  * **What:** Event/task description
  * **Last Date:** Precise deadline
  * **Link:** Applicable registration/submission link

### 2. Filter Agent
* **Profile-Aware:** Stores your personal academic profile (e.g., *3rd Year, Computer Science, Section B*).
* **Action:** Compares incoming notices with your profile and discards non-matching notices (e.g., ignores MBA notices if you are an engineering student).

### 3. Reminder Agent
* **Daily Summary:** Delivers a simple list of tasks due tomorrow.
* **Timely Alerts:** Automatically sends a reminder notification **2 hours before** any deadline.

---

## 💡 Example

### **Input Notice:**
> *"Attention 3rd Year CSE Section B students! The Assignment 2 submission link is now live. Please submit your notebooks on the portal before tomorrow 10:00 AM. Also, MBA registration ends tonight."*

### **Agent Pipeline:**
1. **Collector:** Extracts Assignment 2 (Due: Tomorrow 10 AM) & MBA Registration (Due: Tonight).
2. **Filter:** Keeps Assignment 2 for 3rd Year CSE Section B; drops MBA Registration.
3. **Reminder:**
   * **Daily Agenda:** `Tomorrow 10:00 AM - Assignment 2 Due`
   * **Alert:** Sends a ping at `8:00 AM` on the day of submission.

---

## 🛠️ Tech Stack

* **Language:** Python
* **Agent Framework:** `uagents` / AI Agent Framework
* **NLP/LLM:** OpenAI / Local LLM for text parsing
* **Database/Storage:** SQLite / JSON profile database

---

## ⚙️ Quick Start

### 1. Clone the repository
```bash
git clone [https://github.com/your-username/miss-mat-karo-agent.git](https://github.com/your-username/miss-mat-karo-agent.git)
cd miss-mat-karo-agent
## 💻 Code Explanation (Core Pipeline)

Here is a look at how the agent pipeline processes incoming forwarded notices using Python and agent protocols:

```python
@notice_proto.on_message(NoticeMessage)
async def handle_incoming_notice(ctx: Context, sender: str, msg: NoticeMessage):
    ctx.logger.info("New notice received, processing through pipeline...")

    # 1. Collector Agent: Extract What, Last Date, and Link
    extracted_data = parse_notice_text(msg.raw_content)
    
    # 2. Filter Agent: Check if notice belongs to student's profile
    student_profile = {"year": "3rd", "branch": "CSE", "section": "B"}
    
    if not matches_profile(extracted_data, student_profile):
        ctx.logger.info("Notice filtered out: Not relevant to this student.")
        return

    # 3. Reminder Agent: Save task and schedule alert 2 hours prior
    schedule_deadline_reminder(
        task=extracted_data["what"], 
        deadline=extracted_data["last_date"], 
        hours_before=2
    )
    
    ctx.logger.info("Notice saved successfully! Reminder scheduled.")
