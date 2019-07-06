# Notes on conducting a threat modeling session

## 0. Areas of focus

* Architecture
* Requirements
* Design

## 1. Draw the system

* Find a whiteboard
* Gather people familiar with the system & interested in discussion
* Draw something resembling an UML diagram or a message sequence diagram
  * Person who's drawing make's a statement about the functionality
    * *Our system is great, here's why (provides detailed tech explanation)*
  * Other people in the room are trying to prove he's wrong
    * *Yeah, but X*
  * Question as much as possible
  * Paste post-it notes for further discussion

* Security problems hide behind assumptions
  * Have a proper amount of plausible doubt
* Humans should be drawn too, if they're part of the system
* Post-it notes to see discussion points and have a further reference

## 2. STRIDE model

* **S** poofing (authentication)
* **T** ampering (integrity)
* **R** epudiation (auditability)
* **I** nformation Disclosure (confidentiality)
* **D** enial of Service (availability)
* **E** levation of priviledge (authorization)

## 3. TRIM model

* **T** ransfer
* **R** etention / removal
* **I** nference
* **M** inimisation

## 4. Threat modeling for UX

* Draw a table which tells a story from inserted components

| Actor        | Vulnerability            | System  | Technical impact    | Business impact |
| ------------ | ------------------------ | ------- | ------------------- | --------------- |
| User         | No strong authentication | BE      | Personal data shown | GDPR Fines      |

* Prioritize based on connections to a consequence you want
* In SCRUM, it's probably better to do threat modeling in a sprint after the implementation

## 5. Gamified threat modeling

* [OWASP Cornucopia](https://www.owasp.org/index.php/OWASP_Cornucopia)
* [Microsoft STRIDE card game](https://www.microsoft.com/en-us/download/details.aspx?id=20303)

## 6. Links, references

* [Common Weakness Enumeration](https://cwe.mitre.org/)
