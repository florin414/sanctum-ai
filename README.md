# Sanctum AI 🧠🛡️

![Go](https://img.shields.io/badge/go-1.22+-00ADD8?logo=go)
![Security](https://img.shields.io/badge/Security-PII%20Redaction-red)
![Governance](https://img.shields.io/badge/Cost-FinOps%20Guardrails-green)

**Sanctum AI** este un strat de guvernanță (Governance Layer) care se interpune între infrastructura corporației și furnizorii de modele AI (LLMs).

Într-un mediu enterprise, accesul direct la API-urile publice (ex: GPT-4) prezintă riscuri majore: scurgeri de date personale (PII) și costuri necontrolate. Sanctum AI rezolvă aceste probleme printr-o arhitectură de tip **Smart Proxy** de înaltă performanță.

## 🏗️ Arhitectură și Decizii Tehnice

### 1. Zero-Trust PII Masking
Sistemul interceptează prompt-urile și identifică entități sensibile (CNP, Email, IBAN) folosind o combinație de Regex optimizat și NLP ușor (Named Entity Recognition).
* **Strategie:** Datele sunt înlocuite cu token-uri sintetice (ex: `<PERSON_1>`) înainte de a părăsi perimetrul securizat.
* **Re-hydration:** La primirea răspunsului de la LLM, token-urile sunt reînlocuite cu datele originale, asigurând o experiență transparentă pentru utilizator.

### 2. FinOps & Cost Guardrails
Implementează rate-limiting semantic și bugete la nivel de departament folosind **Redis**.
* *Exemplu:* "Echipa de Marketing are un buget de $500/zi. După atingerea a 80%, traficul este redus (throttled), iar la 100% este blocat."

### 3. Model Agnostic Router
Permite rutarea dinamică a cererilor: întrebările simple sunt direcționate către modele ieftine (ex: Llama 3 hostat local), iar cele complexe către modele SOTA (State of the Art), optimizând costurile cu până la 60%.
