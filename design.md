# SwasthyaMitra AI - Design Document
## AWS Idea Submission - Executable System Design

---

## 1. Goal

Design a **simple, practical AI healthcare guidance system** for Indian citizens that can be built into a working prototype using AWS services.

The design focuses on:
- **Real-world usability**: Works for low-literacy, Hindi-speaking users
- **Fast implementation**: Buildable in 2-4 weeks
- **Demo-ready execution**: Can be shown live to judges
- **Impact in Bharat**: Solves genuine healthcare access problem

**This is not a large enterprise system.** It's a focused, executable prototype.

---

## 2. Problem Context

### Healthcare Challenges in India

**In India**:
- People cannot judge symptom seriousness
- Treatment is delayed due to uncertainty
- Rural citizens lack hospital access
- Awareness of schemes like Ayushman Bharat is low

### Real-World Impact

This leads to:
- **Self-medication**: People treat serious conditions at home
- **Financial burden**: Families spend savings unaware of free schemes
- **Preventable emergencies**: Delayed care for urgent symptoms

**Example**: A farmer with chest pain doesn't know if it's acidity or heart attack. By the time he reaches a doctor, it may be too late.

### What's Needed

A simple AI guidance system that:
- Analyzes symptoms instantly
- Classifies urgency (Low / Medium / Urgent)
- Shows nearby hospitals
- Displays government schemes
- Works in Hindi on any smartphone

---

## 3. Solution Architecture (Simple & Practical)

### System Flow

```
User (Mobile/Web)
        ↓
   Chat Interface
        ↓
   Backend Service
        ↓
Amazon Bedrock (AI Risk Guidance)
        ↓
Hospital & Scheme Data (DynamoDB)
        ↓
   Response to User
```

**This entire flow is designed for a working prototype.**

### Architecture Diagram

```
┌─────────────────────────────────────────────────────┐
│                   USER LAYER                         │
├──────────────────────┬──────────────────────────────┤
│   Web App (React)    │   WhatsApp (Future)          │
│   Mobile-First PWA   │   Twilio Integration         │
└──────────┬───────────┴──────────────┬───────────────┘
           │                          │
           └──────────┬───────────────┘
                      ↓
           ┌──────────────────────┐
           │  Backend API         │
           │  FastAPI Service     │
           └──────────┬───────────┘
                      │
        ┌─────────────┼─────────────┬─────────────┐
        ↓             ↓             ↓             ↓
┌───────────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ Amazon        │ │ Amazon   │ │ DynamoDB │ │ Twilio   │
│ Bedrock       │ │Translate │ │ Tables   │ │WhatsApp  │
│ (Claude)      │ │ (Hindi)  │ │          │ │   API    │
└───────────────┘ └──────────┘ └──────────┘ └──────────┘
```

### Key Principles

- **Keep it simple**: No unnecessary complexity
- **AWS-first**: Leverage managed services
- **Stateless**: No session management
- **Fast**: < 5 second response time
- **Scalable**: Can grow from prototype to production

---

## 4. Core Features (Demo-Oriented)

### Feature 1: Symptom Input Through Chat
- Simple text input box
- Natural language processing via Amazon Bedrock
- Supports English and basic Hindi
- Examples shown: "chest pain", "बुखार और सिरदर्द"

### Feature 2: AI-Based Risk Guidance
- **Low**: Common conditions, self-care recommended
- **Medium**: Consult doctor within 24-48 hours
- **Urgent**: Seek immediate medical attention

Powered by **Amazon Bedrock** with Claude model.

### Feature 3: Nearby Hospital Suggestions
- Shows 3-5 nearest hospitals
- Displays: Name, distance, type, emergency availability, phone
- Sorted by distance
- Prioritizes emergency hospitals for urgent cases

Data stored in **DynamoDB** with geospatial queries.

### Feature 4: Government Scheme Suggestions
- Shows 2-3 relevant schemes
- Displays: Name, eligibility, benefits, how to apply
- Covers Ayushman Bharat and state schemes

### Feature 5: Simple, Accessible UI
- Mobile-first design
- Large buttons and clear icons
- Minimal text, visual indicators
- Works on low-end smartphones

### Feature 6: Hindi Language Support
- **Amazon Translate** converts responses to Hindi
- Critical for rural user accessibility
- Supports mixed language input

**Focus is on functionality, not complexity.**

---

## 5. Real-Life Data Focus

### Hospital Data (Real & Publicly Available)

**The system will use real, publicly available data:**

**Sources**:
- **National Health Portal** (nhp.gov.in)
- **State health directories**
- **District health centers database**

**Coverage** (50+ hospitals for prototype):
- Government hospitals
- Private hospitals
- District health centers
- Primary Health Centers (PHC)

**Example Data**:
```json
{
  "hospital_id": "h001",
  "name": "AIIMS Delhi",
  "type": "Government",
  "city": "Delhi",
  "state": "Delhi",
  "location": {"lat": 28.5672, "lng": 77.2100},
  "emergency": true,
  "phone": "011-26588500",
  "services": ["Emergency", "ICU", "Surgery", "Cardiology"],
  "beds": 2500,
  "ambulance": true
}
```

**Example Sources**:
- AIIMS hospitals across India
- State government hospitals
- District hospitals from NHM portal
- PHCs from state health departments

### Government Scheme Data (Real & Publicly Available)

**Sources**:
- **Ayushman Bharat** official portal (pmjay.gov.in)
- **State health department** websites
- **Ministry of Health and Family Welfare**

**Coverage** (20+ schemes for prototype):
- **Ayushman Bharat PMJAY** (national)
- **State-level schemes**: UP, Bihar, Maharashtra, Karnataka
- **Low-income healthcare benefits**
- **Senior citizen schemes**

**Example Data**:
```json
{
  "scheme_id": "s001",
  "name": "Ayushman Bharat PMJAY",
  "type": "National",
  "state": "all",
  "eligibility": {
    "income_max": 500000,
    "age_min": 0,
    "family_size_max": 10
  },
  "benefits": "Free treatment up to ₹5 lakh per year",
  "coverage": "Secondary and tertiary care hospitalization",
  "how_to_apply": "Visit nearest Ayushman Mitra or CSC",
  "documents": ["Aadhaar", "Income certificate"],
  "url": "https://pmjay.gov.in",
  "helpline": "14555"
}
```

**This ensures relevance and credibility.**

---

## 6. AWS Services Utilised

### Amazon Bedrock - Symptom Analysis & Risk Classification

**Purpose**: AI-powered understanding and classification of symptoms

**Why Bedrock**:
- Pre-trained foundation models (Claude, Llama)
- No model training required
- Serverless and scalable
- Built-in safety guardrails
- Available in ap-south-1 (Mumbai) region

**How We Use It**:
```python
import boto3
import json

bedrock = boto3.client('bedrock-runtime', region_name='ap-south-1')

def analyze_symptom(symptom_text):
    prompt = f"""You are a medical triage assistant for India.
    Classify these symptoms as LOW, MEDIUM, or URGENT risk.
    
    Symptoms: {symptom_text}
    
    Respond in JSON format:
    {{
      "risk_level": "LOW|MEDIUM|URGENT",
      "confidence": 0-100,
      "reasoning": "brief explanation in simple language"
    }}"""
    
    response = bedrock.invoke_model(
        modelId='anthropic.claude-v2',
        body=json.dumps({
            "prompt": prompt,
            "max_tokens": 200,
            "temperature": 0.3
        })
    )
    
    return parse_response(response)
```

**Example**:
- Input: "chest pain and sweating"
- Bedrock Output: `{"risk_level": "URGENT", "confidence": 95, "reasoning": "Chest pain with sweating may indicate cardiac emergency"}`

**Cost**: ~$0.01 per request (prototype budget-friendly)

### Amazon Translate - Hindi Language Support

**Purpose**: Translate responses to Hindi for accessibility

**Why Translate**:
- Supports Hindi with high accuracy
- Real-time translation
- Simple API integration
- Serverless, pay-per-use

**How We Use It**:
```python
translate = boto3.client('translate', region_name='ap-south-1')

def translate_to_hindi(text):
    response = translate.translate_text(
        Text=text,
        SourceLanguageCode='en',
        TargetLanguageCode='hi'
    )
    return response['TranslatedText']
```

**Example**:
- Input: "Seek immediate medical attention"
- Output: "तुरंत चिकित्सा सहायता लें"

**Cost**: ~$15 per million characters

### DynamoDB - Hospital & Scheme Dataset Storage

**Purpose**: Store hospital and scheme data with fast queries

**Why DynamoDB**:
- Serverless, no server management
- Fast geospatial queries for hospital search
- Free tier: 25GB storage, 25 read/write units
- Scales automatically
- Low latency (< 10ms)

**Tables**:

**Hospitals Table**:
- Primary Key: `hospital_id`
- Attributes: name, type, location (lat/lng), emergency, phone, services
- Indexes: GSI on city for city-based queries

**Schemes Table**:
- Primary Key: `scheme_id`
- Attributes: name, type, eligibility, benefits, how_to_apply, url
- Indexes: GSI on state for state-specific schemes

**Geospatial Query** (Find nearby hospitals):
```python
from boto3.dynamodb.conditions import Key
import math

def find_nearby_hospitals(lat, lng, radius_km=10):
    # Scan all hospitals (prototype scale)
    table = dynamodb.Table('Hospitals')
    response = table.scan()
    hospitals = response['Items']
    
    # Calculate distances
    nearby = []
    for h in hospitals:
        distance = haversine_distance(
            lat, lng,
            h['location']['lat'],
            h['location']['lng']
        )
        if distance <= radius_km:
            h['distance_km'] = round(distance, 1)
            nearby.append(h)
    
    # Sort by distance
    return sorted(nearby, key=lambda x: x['distance_km'])[:5]

def haversine_distance(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    dlat = math.radians(lat2 - lat1)
    dlon = math.radians(lon2 - lon1)
    a = (math.sin(dlat/2)**2 + 
         math.cos(math.radians(lat1)) * math.cos(math.radians(lat2)) * 
         math.sin(dlon/2)**2)
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1-a))
    return R * c
```

### AWS Services Summary

These services ensure:
- **Scalability**: Serverless, auto-scaling
- **Reliability**: Managed services with high uptime
- **Rapid prototype development**: No infrastructure management

---

## 7. WhatsApp Chatbot Extension (Future Demo)

### Architecture

```
User
  ↓
WhatsApp message
  ↓
Backend webhook (FastAPI)
  ↓
Amazon Bedrock (AI analysis)
  ↓
DynamoDB (hospital & scheme data)
  ↓
Response with risk + hospital + scheme
  ↓
WhatsApp message to user
```

**Designed for rural accessibility.**

### Implementation

```python
from twilio.twiml.messaging_response import MessagingResponse

@app.post("/api/whatsapp-webhook")
async def whatsapp_webhook(request: Request):
    # Parse Twilio request
    form = await request.form()
    user_message = form.get('Body', '')
    
    # Analyze symptom (same logic as web)
    result = await analyze_symptom_internal(user_message)
    
    # Format for WhatsApp (text-only)
    response_text = f"""
{'🚨 URGENT' if result['risk_level'] == 'URGENT' else result['risk_level']}

{result['reasoning']}

{'⚠️ Call 108 immediately!' if result['risk_level'] == 'URGENT' else ''}

Nearest Hospital:
🏥 {result['hospitals'][0]['name']}
📍 {result['hospitals'][0]['distance_km']} km
📞 {result['hospitals'][0]['phone']}

Scheme: {result['schemes'][0]['name']}
{result['schemes'][0]['benefits']}

⚠️ Not a diagnosis. Consult a doctor.
"""
    
    # Send via Twilio
    resp = MessagingResponse()
    resp.message(response_text.strip())
    return str(resp)
```

### Why WhatsApp

- **500M+ users in India**
- **Familiar platform** (no new app to learn)
- **Works on any phone** (even feature phones)
- **Low data usage**
- **Rural accessibility**

---

## 8. Emergency Impact Logic

### Rule-Based Emergency Detection

**If symptoms indicate**:
- Chest pain
- Breathing difficulty
- Severe bleeding
- Unconsciousness
- Stroke symptoms
- Severe burns
- Poisoning

**System prioritizes**:
- **Urgent warning**: Red alert banner
- **Nearest hospital**: Emergency services only
- **Helpline guidance**: Display "CALL 108" prominently

### Implementation

```python
EMERGENCY_KEYWORDS = [
    "chest pain", "difficulty breathing", "severe bleeding",
    "unconscious", "stroke", "heart attack", "seizure",
    "severe burn", "poisoning", "can't breathe",
    "severe allergic reaction"
]

def is_emergency(symptom_text):
    symptom_lower = symptom_text.lower()
    return any(keyword in symptom_lower for keyword in EMERGENCY_KEYWORDS)

def analyze_with_emergency_check(symptom_text):
    # Step 1: Check emergency keywords (instant)
    if is_emergency(symptom_text):
        return {
            "risk_level": "URGENT",
            "confidence": 100,
            "reasoning": "Emergency keywords detected. Seek immediate care.",
            "method": "rule-based"
        }
    
    # Step 2: Use Amazon Bedrock for analysis
    return analyze_with_bedrock(symptom_text)
```

### Emergency Response UI

```
🚨 URGENT - SEEK IMMEDIATE MEDICAL ATTENTION 🚨

Your symptoms may indicate a serious cardiac condition.

⚠️ CALL 108 IMMEDIATELY ⚠️

Nearest Emergency Hospital:
🏥 AIIMS Delhi - 2.3 km
📞 011-26588500
🚨 Emergency services available 24/7

Government Scheme:
💚 Ayushman Bharat PMJAY
Free emergency care up to ₹5 lakh

⚠️ This is not a medical diagnosis. Consult a doctor immediately.
```

**This demonstrates real-life usefulness.**

---

## 9. Deployment Approach (Prototype Ready)

### Technology Stack

**Frontend**: Lightweight web/PWA
- React 18 + Vite
- TailwindCSS for styling
- Mobile-first responsive design
- Deploy: Vercel (free tier, CDN included)

**Backend**: Simple API service
- FastAPI (Python 3.10)
- Async operations for performance
- AWS SDK (boto3) for Bedrock, Translate, DynamoDB
- Deploy: Render (free tier) or AWS Lambda

**AWS Services**: Integrated
- Amazon Bedrock (ap-south-1 region)
- Amazon Translate (ap-south-1 region)
- DynamoDB (ap-south-1 region)

**Minimal infrastructure**:
- No Kubernetes
- No microservices
- No complex load balancing
- No message queues

### Deployment Steps

**1. AWS Setup**:
```bash
# Enable Amazon Bedrock
# Create DynamoDB tables (Hospitals, Schemes)
# Configure IAM roles for backend access
```

**2. Backend Deployment**:
```bash
# Deploy to Render
git push origin main
# Render auto-deploys from GitHub

# Environment variables:
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
AWS_REGION=ap-south-1
```

**3. Frontend Deployment**:
```bash
# Deploy to Vercel
vercel deploy --prod

# Environment variable:
VITE_API_URL=https://swasthyamitra-api.onrender.com
```

**4. Data Population**:
```python
# Load 50 hospitals into DynamoDB
# Load 20 schemes into DynamoDB
```

### Goal

**Buildable within hackathon timeframe** (2-4 weeks).

---

## 10. Avoiding Over-Engineering

### This Design Intentionally Avoids

❌ **Complex microservices**: Single FastAPI app is sufficient  
❌ **Enterprise infrastructure**: No Kubernetes, no service mesh  
❌ **Heavy integrations**: No ABDM, no insurance, no EHR  
❌ **Large-scale data systems**: Simple DynamoDB tables  
❌ **Complex authentication**: Anonymous usage only  
❌ **API Gateway**: Direct backend calls (or simple AWS API Gateway)  
❌ **Message queues**: Synchronous requests are fine  
❌ **Caching layer**: Simple in-memory cache sufficient  
❌ **Monitoring dashboard**: Basic AWS CloudWatch  
❌ **CI/CD pipeline**: GitHub auto-deploy to Render/Vercel

### Focus Remains On

✅ **Working prototype**: Can be demoed live  
✅ **Clarity**: Simple, understandable architecture  
✅ **Impact**: Solves real problem for real users  
✅ **Feasibility**: Buildable in 2-4 weeks  
✅ **AWS integration**: Clear use of Bedrock, Translate, DynamoDB

### Code Example (Simple, Not Over-Engineered)

**Good**:
```python
@app.post("/api/analyze")
async def analyze_symptom(request: SymptomRequest):
    # Check emergency
    if is_emergency(request.symptom):
        risk_level = "URGENT"
    else:
        # Call Bedrock
        result = await analyze_with_bedrock(request.symptom)
        risk_level = result['risk_level']
    
    # Find hospitals
    hospitals = find_nearby_hospitals(request.location)
    
    # Find schemes
    schemes = find_eligible_schemes()
    
    return {"risk_level": risk_level, "hospitals": hospitals, "schemes": schemes}
```

**Bad** (Over-engineered - Don't do this):
```python
class SymptomAnalysisService:
    def __init__(self, bedrock_client, cache_manager, logger, metrics, circuit_breaker):
        self.bedrock = bedrock_client
        self.cache = cache_manager
        self.logger = logger
        self.metrics = metrics
        self.circuit_breaker = circuit_breaker
    
    async def analyze(self, symptom: Symptom) -> AnalysisResult:
        # 200 lines of abstraction...
```

---

## 11. Innovation & Differentiation

### What Makes This Unique

**1. Healthcare guidance for Bharat users**:
- Designed for low literacy populations
- Hindi language support via Amazon Translate
- Works on 3G networks and low-end devices
- WhatsApp integration (familiar platform)

**2. AI + government scheme awareness together**:
- Not just symptom checking
- Connects users to ₹5 lakh benefits
- Unique angle: health guidance + financial relief
- Real government scheme data

**3. Simple chat-based approach**:
- No complex forms or medical terminology
- Conversational interface powered by Amazon Bedrock
- Minimal clicks to get results
- Natural language understanding

**4. Designed for low digital literacy**:
- Big buttons, clear icons
- Simple language (8th-grade reading level)
- Visual indicators (🚨, 🏥, 💚)
- Minimal text, maximum clarity

**5. Not just a chatbot — a decision-support tool**:
- Provides actionable guidance
- Shows nearest hospitals with distance
- Displays emergency helpline for urgent cases
- Connects to real government schemes
- Helps users make informed decisions

### Competitive Advantage

- **AWS-Powered**: Leverages Bedrock for intelligent analysis
- **Real Data**: Government hospital and scheme information
- **Accessibility**: WhatsApp reaches 500M+ Indian users
- **Speed**: < 5 second response time
- **Impact**: Addresses rural healthcare gap

---

## 12. Impact & Relevance

### Potential Outcomes

**1. Faster health decisions**:
- Users know urgency within 5 seconds
- Emergency cases identified immediately
- Reduces decision paralysis

**2. Improved scheme utilisation**:
- Users discover ₹5 lakh Ayushman Bharat benefits
- Financial burden reduced
- More people access free healthcare

**3. Support for rural communities**:
- WhatsApp integration reaches underserved populations
- Hindi language removes barriers
- Works on low-end devices with 3G

**4. Reduced treatment delays**:
- Urgent symptoms flagged immediately
- Nearest hospital shown with distance
- Emergency helpline (108) displayed prominently

### Scalable Across India

- **Target**: 10,000 users in first 3 months
- **Geographic Reach**: 10+ states
- **Language Support**: Hindi + English (expandable to regional languages)
- **Platform**: Web + WhatsApp (expandable to voice)

### Real-World Impact

**Scenario 1**: Farmer in Bihar with chest pain uses WhatsApp → Gets URGENT alert → Calls 108 → Reaches hospital in time → **Life saved**

**Scenario 2**: Mother in UP needs treatment for child → Discovers Ayushman Bharat eligibility → Gets free treatment worth ₹50,000 → **Financial relief**

**Scenario 3**: Student in Karnataka with mild fever → Gets LOW risk assessment → Avoids unnecessary hospital visit → **Healthcare efficiency**

---

## 13. Hackathon Win Strategy

### Judges Look For

1. **Real problem solving**: Addresses genuine healthcare challenge
2. **AWS usage clarity**: Clear, purposeful use of services
3. **Feasibility**: Buildable with working prototype
4. **Innovation**: Unique approach to problem
5. **Impact**: Potential to help millions

### This Design Focuses On

**1. Practical architecture**:
- Simple system flow: User → Chat → Bedrock → DynamoDB → Response
- No over-engineering (no microservices, Kubernetes)
- Clear deployment plan (Vercel + Render + AWS)
- Stateless, scalable design

**2. Real-life datasets**:
- 50+ hospitals from National Health Portal
- 20+ schemes from Ayushman Bharat portal
- Verified, publicly available data
- Credible sources for judges

**3. Buildable approach**:
- 2-4 week development timeline
- Core features only (no scope creep)
- Demo-ready in 2 minutes
- Clear success criteria

**4. Meaningful AI usage**:
- Amazon Bedrock for symptom understanding (not just keyword matching)
- Amazon Translate for Hindi accessibility (not just English)
- Emergency detection logic (rule-based + AI)
- Confidence scores and reasoning

**5. Working prototype potential is clear**:
- Judges can use it during presentation
- Emergency detection creates "wow" moment
- WhatsApp demo shows accessibility
- Real impact story resonates
- Technical feasibility is evident

### Presentation Strategy

**Opening** (30 seconds):
- "65% of Indians can't judge if symptoms are serious"
- "We built a working AI assistant using AWS Bedrock"

**Demo** (60 seconds):
- Live web demo: "chest pain" → URGENT alert
- Show nearest hospital with distance
- Display Ayushman Bharat scheme
- WhatsApp demo (if ready)

**Technical** (30 seconds):
- Architecture diagram
- Highlight Bedrock, Translate, DynamoDB
- Show response time < 5 seconds
- Explain emergency detection logic

**Impact** (30 seconds):
- Reaches 500M WhatsApp users
- Connects to ₹5 lakh government schemes
- Scalable to millions of users
- Real-world scenarios

**Close** (30 seconds):
- "This is not just an idea - it's a working prototype"
- "Built with AWS, ready to scale"
- "Real impact for Bharat"

---

## 14. Future Evolution (Post-Prototype)

### After Prototype

**Phase 2: Enhanced Features**
- **Regional languages**: Tamil, Telugu, Bengali, Marathi
- **Voice interaction**: Amazon Transcribe for voice input
- **WhatsApp full integration**: Production-ready bot
- **Telemedicine linkage**: Connect to doctor consultations

**Phase 3: Advanced Capabilities**
- **Preventive healthcare insights**: Health tips based on symptoms
- **Chronic disease management**: Diabetes, hypertension tracking
- **Wearable integration**: Connect fitness trackers
- **ABDM integration**: Ayushman Bharat Digital Mission

**Phase 4: Scale & Partnerships**
- **Government partnerships**: Official data integration
- **Hospital networks**: Direct appointment booking
- **Insurance integration**: Claim assistance
- **Pan-India coverage**: All 22 official languages

### Technology Evolution

- **Amazon SageMaker**: Custom ML models for better accuracy
- **AWS Lambda**: Serverless backend for cost optimization
- **Amazon CloudFront**: Global CDN for faster access
- **Amazon Personalize**: Personalized health recommendations

---

## 15. Final Vision

**SwasthyaMitra AI aims to become a simple digital health companion for every Indian citizen.**

### Helping People

- **Understand symptoms**: Know if symptoms are serious
- **Access hospitals**: Find nearest facilities instantly
- **Use healthcare schemes**: Discover ₹5 lakh benefits
- **Take timely decisions**: Avoid treatment delays

### With the Support of

- **AI**: Amazon Bedrock for intelligent symptom analysis
- **AWS**: Scalable, reliable infrastructure (Bedrock, Translate, DynamoDB)
- **Real Data**: Government hospital and scheme information
- **Accessibility**: Hindi language, WhatsApp, mobile-first design

### Ultimate Goal

**Reduce healthcare inequality in India by making health guidance accessible to everyone, everywhere.**

---

## 16. Conclusion

This design is **executable, realistic, and impactful**.

### It Shows

✅ **Clear AWS integration** with Bedrock, Translate, and DynamoDB  
✅ **Technical feasibility** - buildable in 2-4 weeks  
✅ **Real impact** - solves genuine healthcare problem in India  
✅ **Demo-ready** - can be shown live to judges  
✅ **Scalable architecture** - path from prototype to production  
✅ **No over-engineering** - simple, focused design  
✅ **Real data** - government hospitals and schemes  
✅ **Innovation** - AI + scheme awareness together

### Core Message

**This is not theoretical architecture. This is a blueprint for a working system that can be built, demoed, and scaled using AWS services.**

### Why This Wins

- Real problem for 700M+ Indians
- Clear, purposeful AWS usage
- Buildable in hackathon timeframe
- Demo-ready with wow factor
- Real impact potential

**Next Steps**: Build it, demo it, win it, scale it.

