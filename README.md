# 🛡️ AI Chatbot & CRM Tagging for Insurance - Internal Web Tool

## 📌 Overview

This project is an **internal AI assistant tool** for a local insurance company. It supports marketing teams in **conversationally recommending VHIS insurance plans** while also building a **CRM tagging pipeline** to assist with user classification and campaign retargeting. The chatbot leverages Google Gemini and a local semantic similarity model to provide accurate, personalized suggestions.

---

## ✅ Core Features

### 1. 🔍 AI Chatbot (Main Feature)

- Conversational interface to assist users in exploring VHIS insurance plans.
- Gemini API integration (Google Generative Language) for natural replies.
- Prompting logic trained with insurance plan metadata.
- Semantic model to match user queries to most relevant plans.

### 2. 🏷️ CRM Tagging Pipeline (Main Feature)

- Uses internal semantic model to extract top matching tags.
- Tracks frequency of tag usage per user.
- Logs into MongoDB (`TagLog`) to build CRM profiles.
- Top tag is inferred and matched against `insurance.json` to enrich CRM.

### 3. 🔐 Authentication (Optional Feature)

- Google Sign-in / Username login with session handling.

### 4. 💳 Insurance Browsing & Payment (Optional Feature)

- Intergrated with Zalopay. 
- View insurance package details.
- Simulate deposit / transfer operations.

---

## 🎯 Target Audience

- Internal marketing team members.
- Use case: pre-screening leads, generating interest, guiding suitable campaigns.

### 📷 Use case Diagram

Below are some visual examples to better understand the use case and flow of the tool:

**Chatbot Interface**  
  ![Chatbot Interface](./pictures/system_usecase.png)


## 🏗️ Architecture

- **Frontend**: React (ShadCN UI + TailwindCSS).
- **Backend**: Node.js + Express + MongoDB.
- **AI Services**:
  - Google Gemini API.
  - Local Semantic Search Model (`POST 192.168.4.154:8000/sendMsg`).

**Deployment Diagram**  
  ![Deployment Diagram](./pictures/system_usecase.png)

---

## 📂 Folder Structure

```
├── model
│   ├── chatModel.ts        # MongoDB schema for conversation history
│   ├── tagLog.ts           # Logs tag frequencies and top tag for CRM
├── service
│   ├── chatService.ts      # Main logic combining Gemini + tag + DB
│   ├── geminiService.ts    # Clean Gemini API abstraction
│   ├── boxChatService.ts   # Calls to semantic tag model
├── data
│   └── insurance.json      # Raw plan data with name, type, features
```

---

## 🔍 Implementation Walkthrough

### 🧠 Gemini Prompting Flow

```ts
const prompt = buildPrompt(userInput, tags);
// Includes top tag-based plan names
// Ensures only natural language returned
```

### 🧾 CRM Tag Logging Logic

```ts
await TagLog.create({
  name: //name of insurance,
  type: //type of insurance, ex: "Flexi Premium",
  targetAudience: [ //target of that products
      "VIP clients",
      "those with pre-existing conditions", 
      ....
  ],
  features: [ //what does that products bring to
      "Private room",
      "Full hospital expense coverage", 
      ....
  ]
})
```

## Here are the flow user interact with our system: 
-   ![Login Diagram](./pictures/login_sequence.png)
Login pipeline

--- 

-   ![Payment Diagram](./pictures/payment_sequence.png)
Payment pipeline

---

-   ![AI Diagram](./pictures/ai_pipeline_sequence.png)
AI pipeline

---

### 🔬 Evaluation

## ✅ What Worked

Gemini responds fluently with contextual coherence.

Combining Gemini + tag-based similarity produces more relevant suggestions.

CRM pipeline lets consultants understand repeated interests.

## ⚠️ Challenges

Local similarity model was too naive on surface-level keywords.

Data like "premium" or "children" were matched incorrectly.

Confidence score did not correlate well with intent.

## 💡 Lessons & Recommendations

⚡️ Need real customer Q&A for training.

✨ Add product descriptions (semantic-rich) to improve embedding.

♻️ Use Gemini to rerank or reason instead of relying fully on vector score.

## ☑️ Next Improvements

Integrate usage analytics

Add customer intent classification

Explore fine-tuned Gemini on internal corpus

## 💰 Cost & Feasibility

### Deployment

- **Local hosting** (192.168.4.154) for semantic model → Low infra cost.
- **Gemini API** (Free tier or billing depending on usage).

### Scaling Strategy

- Add Redis for caching recent tag queries.
- Migrate to cloud DB if internal use scales beyond expected.

---

## 👥 Team Members

| Role               | Name             | Responsibilities                                                                 |
|--------------------|------------------|----------------------------------------------------------------------------------|
| 🧠 **AI Developer** | **Tran Minh Khang** | - Prompt engineering<br>- Gemini API integration<br>- Semantic similarity model |
| 🎨 **Frontend Developer** | **Ly Vinh Thai**   | - Chat UI/UX<br>- Insurance plan viewer<br>- CRM tag visualization              |
| 🛠️ **Backend Developer** | **Le Hoang Viet**   | - RESTful API & routing<br>- MongoDB schema design<br>- Pipeline coordination    |

---

**End of README**

