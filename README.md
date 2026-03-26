# 🧊 FridgeWise

**AI-powered grocery management — snap a receipt, track your pantry, reduce food waste.**

FridgeWise is a mobile application that uses computer vision to convert grocery receipt photos into a structured digital inventory. It tracks expiration dates, alerts you when food is expiring, and suggests recipes based on what you have on hand.

> ⚠️ **Note:** This is a showcase repository. The source code is in a private repo as FridgeWise is being developed as a potential business venture. This repo contains architecture documentation, sample outputs, and demo materials.

---

## 🎯 Problem

The average household wastes **$1,500+ per year** on food that expires before it's used. Most people don't track what's in their fridge, forget what they bought, and end up throwing out groceries they didn't know they had.

## 💡 Solution

1. **Snap** a photo of your grocery receipt
2. **FridgeWise** extracts every item using an AI vision model
3. Your **digital pantry** automatically populates with items and estimated expiry dates
4. Get **alerts** when food is expiring and **recipe suggestions** to use it up

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Flutter Mobile App                   │
│                  (Dart — iOS & Android)                 │
│                                                         │
│   ┌──────────┐  ┌──────────────┐  ┌──────────────────┐  │
│   │  Camer   │  │   Pantry     │  │  Recipe          │  │
│   │  Capture │  │   Dashboard  │  │  Suggestions     │  │
│   └─────┬────┘  └──────┬───────┘  └────────┬─────────┘  │
│         │              │                   │            │
└─────────┼──────────────┼───────────────────┼────────────┘
          │              │                   │
          ▼              ▼                   ▼
┌─────────────────────────────────────────────────────────┐
│                   FastAPI Backend                       │
│                 (Python — Dockerized)                   │
│                                                         │
│   ┌──────────────┐  ┌─────────────┐  ┌───────────────┐  │
│   │  Receipt     │  │  Inventory  │  │  Recipe       │  │
│   │  Processing  │  │  Managemen  │  │  Engine       │  │
│   └──────┬───────┘  └──────┬──────┘  └───────────────┘  │
│          │                 │                            │
│          ▼                 ▼                            │
│   ┌──────────────┐  ┌─────────────┐                     │
│   │  Ollama      │  │ PostgreSQL  │                     │
│   │  Vision Model│  │ Database    │                     │
│   │  (Distilled) │  │             │                     │
│   └──────────────┘  └─────────────┘                     │
└─────────────────────────────────────────────────────────┘
```

### Tech Stack

| Layer | Technology |
|-------|-----------|
| **Mobile Frontend** | Flutter (Dart) |
| **Backend API** | FastAPI (Python) |
| **AI/ML** | Ollama vision model (distilled for receipt parsing) |
| **Database** | PostgreSQL |
| **Containerization** | Docker |
| **Image Processing** | Pillow, python-multipart |
| **HTTP Client** | httpx |

---

## 🤖 AI Pipeline

### Receipt → Structured Data

The core of FridgeWise is a vision model pipeline that converts raw receipt images into structured JSON:

**Input:** Photo of a grocery receipt

**Processing:**
1. Image is uploaded via the FastAPI endpoint
2. Image is preprocessed and sent to a locally hosted Ollama vision model
3. The model extracts item names, quantities, and categories
4. Raw output is validated and normalized into structured JSON

**Output:**
```json
{
  "receipt_items": [
    {
      "name": "Organic Whole Milk",
      "quantity": 1,
      "category": "dairy",
      "price": 5.49
    },
    {
      "name": "Sourdough Bread",
      "quantity": 1,
      "category": "bakery",
      "price": 4.99
    },
    {
      "name": "Chicken Breast",
      "quantity": 2,
      "category": "meat",
      "price": 12.98
    },
    {
      "name": "Baby Spinach",
      "quantity": 1,
      "category": "produce",
      "price": 3.99
    }
  ],
  "store": "Loblaws",
  "date": "2026-03-10",
  "total": 27.45
}
```

### Model Distillation

The base Ollama vision model is being distilled into a smaller, task-specific model optimized for grocery receipt parsing:

- **Training data:** Manually labeled grocery receipt images from multiple Canadian and US retailers
- **Goal:** Reduce inference latency and resource requirements while maintaining extraction accuracy for the specific domain of grocery receipts
- **Approach:** Knowledge distillation from a larger teacher model into a smaller student model tuned for receipt OCR and item extraction

---

## 🗺️ Roadmap

- [x] FastAPI backend with receipt upload endpoint
- [x] Ollama vision model integration for receipt parsing
- [x] Structured JSON extraction pipeline
- [x] Docker containerization
- [x] PostgreSQL inventory storage
- [ ] Model distillation (in progress — collecting training data)
- [ ] Expiration date estimation model (per product category)
- [ ] Recipe suggestion engine (based on expiring items)
- [ ] Flutter mobile app frontend
- [ ] Push notifications for expiring items
- [ ] Multi-store receipt support (Loblaws, Metro, Walmart, Costco, etc.)

---

## 🎬 Demo

> *Demo video/GIF coming soon — will show the full receipt-to-inventory pipeline in action.*

![FridgeWise_Demo_GIF](https://github.com/user-attachments/assets/74a85a50-f898-4170-86a3-08df4322cfee)


---

## 📬 Contact

**Connor Todd**  
📧 ctodd678@gmail.com  
📍 Toronto, ON  
🔗 [GitHub](https://github.com/ctodd678)
