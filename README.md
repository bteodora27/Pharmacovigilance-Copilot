# Pharmacovigilance Signal Triage Copilot 💊🤖
**Agent Inteligent pentru Prioritizarea Semnalelor de Siguranță în Medicamente**

## 👥 Echipa
* **Back Andreea Teodora** 
* **Aluculesi Andru Casian** 

## 🎯 Definirea Problemei
Echipele de siguranță medicamentoasă (Pharmacovigilance) primesc anual milioane de rapoarte privind efectele adverse. Datele sunt frecvent incomplete, redundante și neuniforme (ex: denumiri comerciale diferite pentru aceeași substanță). Procesul manual de triaj este lent și predispus la erori, întârziind detectarea semnalelor critice. 
* **Inputs:** Rapoarte brute de reacții adverse (format JSON/CSV) extrase din baza de date publică openFDA FAERS, alături de cerințele utilizatorului în limbaj natural (ex: "Analizează semnalele pentru Nurofen").
* **Outputs:** Un "Signal Packet" (raport explicabil) generat automat care conține o listă prioritizată a semnalelor, metrici statistice de disproporționalitate (PRR, ROR), grafice de trend și explicații AI.

## 🧠 Soluția Propusă & Tipul de AI
Vom dezvolta un **Sistem Inteligent bazat pe un Agent AI Autonom (LLM)**. Agentul va acționa ca un "detector de fum", folosind lanțuri de prompting pentru a interpreta instrucțiunile și tehnici de **Tool Use** pentru a apela:
1. API-uri de preluare a datelor (openFDA).
2. Instrumente de normalizare a medicamentelor (RxNorm).
3. Scripturi Python pentru calculul matematic al disproporționalității semnalelor.

## 📈 Performanța Așteptată (KPIs)
* Reducerea timpului de triaj comparativ cu analiza manuală.
* Timp mai scurt de la apariția semnalului până la revizuirea umană (detecție timpurie).
* Grad ridicat de standardizare al "Signal Packets" generate.

## 🌍 Obiective de Dezvoltare Durabilă (SDGs) Impactate
* **SDG 3 (Sănătate și Bunăstare):** Soluția noastră previne mortalitatea și morbiditatea prin identificarea timpurie a combinațiilor periculoase medicament–eveniment, protejând pacienții.
* **SDG 9 (Industrie, Inovație și Infrastructură):** Modernizarea proceselor din industria farmaceutică prin integrarea tehnologiilor de automatizare și AI.

## 🏗️ Schema Arhitecturală a Soluției

![Schema Arhitecturala a Solutiei](schema_arhitectura.png)