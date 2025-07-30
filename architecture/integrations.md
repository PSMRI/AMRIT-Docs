# Integrations

AMRIT is designed to be an interoperable and extensible platform, integrating with various systems to enhance functionality and streamline healthcare workflows. Below are the key integrations within AMRIT, along with open-source alternatives and opportunities for contribution.

***

### **1. CTI (Computer Telephony Integration) with C-Zentrix**

The CTI integration using C-Zentrix powers AMRIT’s health helpline functionality, enabling systems like 104 and 1097 to efficiently handle calls. Features include call routing, logging, and interaction history, ensuring helplines can function seamlessly and provide critical support to beneficiaries.

#### **Open Alternatives**

While C-Zentrix is the current integration, AMRIT can be extended to include open-source telephony solutions (eg:- [Asterisk](https://www.asterisk.org/) or [FreeSWITCH](https://freeswitch.com/)). Contributors are encouraged to propose and implement such alternatives to increase the platform's flexibility.

***

### **2. ABDM (Ayushman Bharat Digital Mission)**

AMRIT complies with the ABDM standards, ensuring seamless interoperability with India’s national digital health ecosystem. This integration supports the generation and use of ABHA (Ayushman Bharat Health Account) IDs, enabling connected care and aligning AMRIT with national healthcare policies.

The ABDM integration is central to AMRIT’s functionality. Developers and contributors are welcome to suggest enhancements or new features that align with evolving ABDM standards to strengthen AMRIT’s role in the digital health ecosystem.

***

### **3. Video Conferencing Solutions**

#### **Swymed**

Swymed enables seamless telemedicine capabilities within the AMRIT platform, supporting video consultations between patients and healthcare providers. This is crucial for remote diagnosis and care, especially in underserved or geographically isolated regions.

#### **Jitsi-Based Video Consultations**

AMRIT now supports integrated telemedicine capabilities through the implementation of [Jitsi Meet](https://jitsi.org/), an open-source, self-hosted video conferencing solution. This enables secure, real-time video consultations between healthcare providers and beneficiaries, which is crucial for delivering remote care, particularly in underserved and geographically isolated regions.

Jitsi Meet is currently integrated into the **ECD (Early Childhood Development)** module, allowing healthcare workers to initiate browser-based video consultations directly from the AMRIT platform. With the explicit consent of the beneficiary, a video call can be initiated to ensure ethical and informed care delivery.

Additionally, the system supports recording of consultations, which can be securely stored for future reference, training, or compliance purposes. The self-hosted deployment ensures complete control over data privacy, security, and infrastructure, while maintaining a seamless and scalable video communication experience.

***

### **4. OpenKM: Document Management and File Uploads**

The OpenKM integration simplifies document management within AMRIT by enabling secure file uploads and retrieval. This ensures patient records, test results, and other critical documents are always accessible when needed.

***
