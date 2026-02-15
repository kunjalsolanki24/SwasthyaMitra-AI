# SwasthyaMitra AI - Requirements Document
## AWS Idea Submission

---

## 1. Project Goal

Build a simple, practical AI healthcare guidance system for Indian citizens that helps them:
- understand symptom severity
- find nearby hospitals
- discover government healthcare schemes
- make timely healthcare decisions

This is a **working prototype** designed for real-world impact in Bharat, not a large enterprise system.

---

## 2. Problem Statement

### Healthcare Challenges in India

**65% of India's population lives in rural areas** with limited healthcare access.

Key problems:
- **Cannot judge symptom seriousness**: People don't know if chest pain is acidity or heart attack
- **Treatment is delayed**: Uncertainty leads to late medical care
- **Hospital access gap**: Rural citizens don't know where nearest hospital is
- **Scheme awareness is low**: Ayushman Bharat (₹5 lakh coverage) goes unused

### Real-World Impact

This leads to:
- **Self-medication**: People treat serious conditions at home
- **Financial burden**: Families spend savings on treatment unaware of free schemes
- **Preventable emergencies**: Delayed care for urgent symptoms
- **Healthcare inequality**: Urban-rural divide in health information access

**Example**: A farmer in Bihar with chest pain doesn't know if it's serious. By the time he reaches a doctor 15 km away, it may be too late.

---

## 3. Solution Overview

**SwasthyaMitra AI** is a simple chat-based assistant that provides:
- AI-powered symptom risk assessment (Low / Medium / Urgent)
- Nearby hospital recommendations with distance and contact
- Government scheme eligibility information
- Instant guidance in Hindi and English

### Core Approach

```
User describes symptoms
        ↓
AI analyzes and classifies risk
        ↓
System shows hospitals and schemes
        ↓
User makes informed decision
```

### Key Features

- **Low literacy support**: Simple chat interface, minimal text
- **Hindi usage**: Amazon Translate for language accessibility
- **Mobile users**: Works on any smartphone with 3G
- **No app required**: Web-based + WhatsApp integration

---

## 4. Core MVP Features (Build Only These)

### Feature 1: Chat Interface
- Simple text input box
- Clean, mobile-first design
- Minimal clicks to get results
- Works on low-end smartphones

### Feature 2: Symptom Input
- User types symptoms in natural language
- Supports English and basic Hindi
- Examples shown: "chest pain", "बुखार और सिरदर्द"
- No complex medical terminology required

### Feature 3: AI Risk Classification
- **Low Risk**: Common conditions, self-care recommended
- **Medium Risk**: Consult doctor within 24-48 hours
- **Urgent Risk**: Seek immediate medical attention

Powered by **Amazon Bedrock** for intelligent analysis.

### Feature 4: Nearby Hospital Display
- Shows 3-5 nearest hospitals
- Displays: Name, distance, type (govt/private), emergency availability, phone
- Sorted by distance
- Prioritizes emergency hospitals for urgent cases

Data stored in **DynamoDB** with geospatial queries.

### Feature 5: Government Scheme Suggestions
- Shows 2-3 relevant schemes
- Displays: Name, eligibility, benefits, how to apply
- Covers Ayushman Bharat and state schemes
- Simple eligibility criteria (income, age, state)

### Feature 6: WhatsApp Chatbot (Future Demo)
- User sends symptom via WhatsApp
- Bot responds with risk, hospitals, schemes
- No app installation needed
- Reaches 500M+ Indian WhatsApp users

---

## 5. Demo Flow (2-Minute Presentation)

### Step-by-Step User Journey

```
Step 1: User opens web app or sends WhatsApp message
        ↓
Step 2: User types: "chest pain and sweating"
        ↓
Step 3: Amazon Bedrock analyzes symptoms
        ↓
Step 4: System returns: 🚨 URGENT - Possible cardiac emergency
        ↓
Step 5: Display nearest hospital with emergency (1.2 km)
        ↓
Step 6: Show Ayushman Bharat scheme (covers emergency care)
        ↓
Step 7: User calls 108 and reaches hospital in time
```

### Emergency Detection Example

**Input**: "difficulty breathing and chest pain"

**Output**:
```
🚨 URGENT - SEEK IMMEDIATE MEDICAL ATTENTION

Your symptoms may indicate a serious cardiac or respiratory condition.

⚠️ CALL 108 IMMEDIATELY ⚠️

Nearest Emergency Hospital:
🏥 City Hospital - 1.2 km away
📞 0120-1234567
🚨 Emergency services available 24/7

Government Scheme:
💚 Ayushman Bharat PMJAY
Free emergency treatment up to ₹5 lakh

⚠️ This is not a medical diagnosis. Consult a doctor immediately.
```

---

## 6. Real-Life Data Focus

### Hospital Data (Real & Publicly Available)

**Sources**:
- National Health Portal (nhp.gov.in)
- State health directories
- District health centers database
- Government hospital listings

**Coverage** (50+ hospitals for prototype):
- Government hospitals
- Private hospitals
- District health centers
- Primary Health Centers (PHC)

**Geographic Coverage**:
- Major cities: Delhi, Mumbai, Bangalore, Kolkata, Chennai
- Rural districts: Sample from UP, Bihar, Maharashtra, Karnataka

**Data Fields**:
```json
{
  "name": "AIIMS Delhi",
  "type": "Government",
  "location": {"lat": 28.5672, "lng": 77.2100},
  "emergency": true,
  "phone": "011-26588500",
  "services": ["Emergency", "ICU", "Surgery"]
}
```

### Government Scheme Data (Real & Publicly Available)

**Sources**:
- Ayushman Bharat official portal (pmjay.gov.in)
- State health department websites
- Ministry of Health and Family Welfare
- National Health Authority

**Coverage** (20+ schemes for prototype):
- National schemes: Ayushman Bharat PMJAY
- State-level schemes: UP, Bihar, Maharashtra, Karnataka, Tamil Nadu
- Low-income healthcare benefits
- Senior citizen schemes

**Data Fields**:
```json
{
  "name": "Ayushman Bharat PMJAY",
  "type": "National",
  "eligibility": "Income < ₹5 lakh/year",
  "benefits": "Free treatment up to ₹5 lakh",
  "how_to_apply": "Visit nearest Ayushman Mitra",
  "url": "https://pmjay.gov.in"
}
```

### Data Credibility

- All data sourced from official government portals
- Regular updates from public health databases
- Verified contact information
- Accurate eligibility criteria

This ensures **relevance and credibility** for judges and users.

---

## 7. Impact & Outcomes

### Immediate Benefits

1. **Faster health decisions**: Users know urgency within 5 seconds
2. **Reduced treatment delay**: Urgent cases identified immediately
3. **Improved scheme awareness**: Users discover ₹5 lakh benefits
4. **Support for rural Bharat**: WhatsApp reaches underserved populations

### Measurable Outcomes

- **Target Users**: 10,000 users in first 3 months
- **Geographic Reach**: 10+ states across India
- **Response Time**: < 5 seconds for all queries
- **Accessibility**: Works on 3G networks and low-end devices
- **Language Support**: Hindi + English

### Real-World Scenarios

**Scenario 1: Life-Saving Emergency Detection**
- Ramesh (farmer, Bihar) has chest pain and sweating
- Uses WhatsApp to describe symptoms
- Gets instant URGENT alert with nearest hospital
- Calls 108 and reaches emergency room in time
- **Outcome**: Potential life saved through early detection

**Scenario 2: Financial Relief Through Scheme Discovery**
- Priya (mother, UP) needs treatment for child's illness
- Discovers she qualifies for Ayushman Bharat
- Gets free treatment worth ₹50,000
- **Outcome**: Financial burden eliminated, family savings protected

**Scenario 3: Appropriate Care Level**
- Suresh (student, Karnataka) has mild fever and headache
- Gets LOW risk assessment with self-care advice
- Avoids unnecessary hospital visit
- **Outcome**: Healthcare system efficiency improved, user saves time and money

---

## 8. Out of Scope (Not in MVP)

### What We Will NOT Build

To keep the prototype focused and buildable, we explicitly exclude:

❌ **Electronic Health Records (EHR)**: No medical history storage  
❌ **Payment Integration**: No payment processing  
❌ **User Authentication**: Anonymous usage only  
❌ **Analytics Dashboard**: No admin panel  
❌ **Appointment Booking**: No hospital scheduling  
❌ **Telemedicine**: No video consultations  
❌ **Prescription Management**: No medication tracking  
❌ **Advanced Integrations**: No ABDM, insurance, pharmacy  
❌ **Full Multilingual Engine**: Only Hindi + English (not 22 languages)  
❌ **Voice Input**: Text-only for MVP  
❌ **Complex User Profiles**: No detailed data collection  
❌ **Over-Engineering**: No microservices, Kubernetes, complex infrastructure

### Why These Are Excluded

- **Time Constraint**: Focus on core value proposition
- **Complexity**: Avoid dependencies that slow development
- **Regulatory**: Minimize compliance requirements for prototype
- **Feasibility**: Build what can be demoed in 2 minutes

---

## 9. Success Criteria (Demo-Ready)

### Prototype is Complete When:

✅ **Working Web App**: Deployed and accessible via URL  
✅ **Symptom Analysis**: User can input symptoms and get risk classification  
✅ **Hospital Display**: Shows nearest hospitals with accurate data  
✅ **Scheme Display**: Shows relevant government schemes  
✅ **WhatsApp Integration**: Bot responds to messages (future demo)  
✅ **Emergency Detection**: "chest pain" triggers instant URGENT alert  
✅ **Hindi Support**: Responses available in Hindi via Amazon Translate  
✅ **Response Time**: All queries respond within 5 seconds  
✅ **Mobile Responsive**: Works on smartphones (iOS and Android)  
✅ **Demo Ready**: Can be demonstrated live to judges

### Demo Success Indicators

- Judges can use the system during presentation
- Emergency detection creates "wow" moment
- WhatsApp integration demonstrates accessibility
- Clear AWS service integration (Bedrock, Translate, DynamoDB)
- Real impact story resonates with judges
- Technical feasibility is evident

---

## 10. User Stories

### Story 1: Emergency Symptom Recognition

**As a** rural citizen with limited medical knowledge  
**I want to** describe my symptoms and know if they're serious  
**So that** I can seek immediate care if needed and avoid delays

**Acceptance Criteria**:
- User can type symptoms in simple language
- System responds within 5 seconds
- Emergency symptoms trigger URGENT alert
- Emergency helpline (108) displayed prominently
- Nearest emergency hospital shown with distance

### Story 2: Hospital Discovery

**As a** user needing medical care  
**I want to** find nearby hospitals quickly  
**So that** I can reach treatment without wasting time

**Acceptance Criteria**:
- System shows 3-5 nearest hospitals
- Distance, contact, and emergency availability displayed
- Results sorted by distance
- Works with manual location input (city/district)
- Government and private hospitals distinguished

### Story 3: Scheme Awareness

**As a** low-income user  
**I want to** know which government schemes I qualify for  
**So that** I can access free or subsidized healthcare

**Acceptance Criteria**:
- System displays 2-3 relevant schemes
- Eligibility criteria clearly explained
- Benefits and application process provided
- Works without complex user profile
- Covers national and state-specific schemes

### Story 4: WhatsApp Accessibility

**As a** user with limited smartphone literacy  
**I want to** use WhatsApp to get health guidance  
**So that** I don't need to install or learn a new app

**Acceptance Criteria**:
- User sends symptom via WhatsApp message
- Bot responds with risk, hospitals, and schemes
- Response within 10 seconds
- No app installation required
- Works on any phone with WhatsApp

### Story 5: Hindi Language Support

**As a** Hindi-speaking user  
**I want to** receive guidance in Hindi  
**So that** I can understand health information clearly

**Acceptance Criteria**:
- System detects Hindi input
- Amazon Translate converts responses to Hindi
- Critical information (emergency alerts) in Hindi
- Language preference remembered
- Simple Hindi vocabulary (8th-grade level)

---

## 11. Innovation & Differentiation

### What Makes This Unique

1. **Healthcare guidance for Bharat users**
   - Designed for low literacy and rural populations
   - Hindi language support via Amazon Translate
   - Works on 3G networks and low-end devices

2. **AI + government scheme awareness together**
   - Not just symptom checking
   - Connects users to ₹5 lakh benefits
   - Unique angle: health guidance + financial relief

3. **Simple chat-based approach**
   - No complex forms or medical terminology
   - Conversational interface
   - Minimal clicks to get results

4. **Designed for low digital literacy**
   - Big buttons, clear icons
   - Simple language (8th-grade reading level)
   - WhatsApp integration (familiar platform)

5. **Not just a chatbot — a decision-support tool**
   - Provides actionable guidance
   - Shows nearest hospitals with distance
   - Displays emergency helpline for urgent cases
   - Connects to real government schemes

### Competitive Advantage

- **Accessibility**: WhatsApp reaches 500M+ Indian users
- **Relevance**: Real hospital and scheme data from government sources
- **Speed**: < 5 second response time
- **Impact**: Addresses rural healthcare gap
- **AWS-Powered**: Leverages Bedrock, Translate, DynamoDB

---

## 12. Hackathon Win Strategy

### What Judges Look For

1. **Real problem solving**: Addresses genuine healthcare challenge in India
2. **AWS usage clarity**: Clear, purposeful use of Bedrock, Translate, DynamoDB
3. **Feasibility**: Buildable in 2-4 weeks with working prototype
4. **Innovation**: Unique combination of AI + scheme awareness
5. **Impact**: Potential to help millions of Indians

### Our Approach

**Practical architecture**:
- Simple system flow: User → Chat → Bedrock → DynamoDB → Response
- No over-engineering (no microservices, Kubernetes, complex infra)
- Clear deployment plan (Vercel + Render + AWS)

**Real-life datasets**:
- 50+ hospitals from National Health Portal
- 20+ schemes from Ayushman Bharat and state portals
- Verified, publicly available data

**Buildable approach**:
- 2-4 week development timeline
- Core features only (no scope creep)
- Demo-ready in 2 minutes

**Meaningful AI usage**:
- Amazon Bedrock for symptom understanding (not just keyword matching)
- Amazon Translate for Hindi accessibility (not just English)
- Emergency detection logic (rule-based + AI)

**Working prototype potential is clear**:
- Judges can use it during presentation
- Emergency detection creates "wow" moment
- WhatsApp demo shows accessibility
- Real impact story resonates

---

## 13. Technical Requirements

### Performance
- **Response Time**: < 5 seconds for symptom analysis
- **Uptime**: 95%+ availability during demo period
- **Scalability**: Support 100+ concurrent users (prototype scale)

### Accessibility
- **Mobile-First**: Optimized for smartphones
- **Low Bandwidth**: Works on 3G networks
- **Simple UI**: Usable by low-literacy users
- **Language**: Hindi + English support

### Data
- **Hospital Database**: 50+ hospitals across major cities and rural areas
- **Scheme Database**: 20+ government schemes (national + state-specific)
- **Emergency Keywords**: Predefined list for instant urgent classification

### AWS Services Used
- **Amazon Bedrock**: AI-powered symptom analysis
- **Amazon Translate**: Hindi language translation
- **DynamoDB**: Hospital and scheme data storage

---

## 14. Future Evolution (Post-Prototype)

### Phase 2: Enhanced Features
- **Regional languages**: Tamil, Telugu, Bengali, Marathi, Gujarati
- **Voice interaction**: Amazon Transcribe for voice input
- **WhatsApp full integration**: Production-ready bot with Twilio
- **Telemedicine linkage**: Connect to doctor consultations

### Phase 3: Advanced Capabilities
- **Preventive healthcare insights**: Health tips based on symptoms
- **Chronic disease management**: Diabetes, hypertension tracking
- **Wearable integration**: Connect fitness trackers
- **ABDM integration**: Ayushman Bharat Digital Mission

### Phase 4: Scale & Partnerships
- **Government partnerships**: Official data integration
- **Hospital networks**: Direct appointment booking
- **Insurance integration**: Claim assistance
- **Pan-India coverage**: All 22 official languages

---

## 15. Final Vision

**SwasthyaMitra AI aims to become a simple digital health companion for every Indian citizen.**

### Helping People:
- **Understand symptoms**: Know if symptoms are serious
- **Access hospitals**: Find nearest facilities instantly
- **Use healthcare schemes**: Discover ₹5 lakh benefits
- **Take timely decisions**: Avoid treatment delays

### With the Support of:
- **AI**: Amazon Bedrock for intelligent symptom analysis
- **AWS**: Scalable, reliable infrastructure
- **Real Data**: Government hospital and scheme information
- **Accessibility**: Hindi language, WhatsApp, mobile-first design

### Ultimate Goal:
**Reduce healthcare inequality in India by making health guidance accessible to everyone, everywhere.**

---

## 16. Conclusion

SwasthyaMitra AI is a **working prototype** that demonstrates how AWS AI services can solve critical healthcare challenges in India. 
“SwasthyaMitra AI empowers rural and **low-literacy communities in India** to access timely healthcare through **symptom analysis, government scheme recommendations, hospital guidance, and a simple multilingual, voice-enabled chat interface**. Built on a scalable, cloud-native architecture with a Progressive **Web App** frontend and microservices backend, this deployable solution addresses immediate healthcare access challenges while providing a foundation for future enhancements like telemedicine, predictive analytics, and broader public health insights

**Core Message**: This is not just an idea - it's a blueprint for a system that can be built and deployed to help millions of Indians make better healthcare decisions and  It offers symptom analysis, government scheme recommendations, hospital guidance, and a simple multilingual, voice-enabled chat interface, with potential to expand into telemedicine, predictive analytics, and broader public health solutions.”

**Why This Wins**:
- ✅ Real problem for 700M+ Indians
- ✅ Clear AWS integration (Bedrock, Translate, DynamoDB)
- ✅ Buildable in 2-4 weeks
- ✅ Demo-ready with wow factor
- ✅ Real impact potential

**Next Steps**: Build it, demo it, scale it.