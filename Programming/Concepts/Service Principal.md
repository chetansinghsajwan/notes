A **service principal** is a **non-human identity** used by applications, scripts, or automated jobs to authenticate and interact with system — instead of using a real user's credentials.
---
**Simple analogy:**
- A **user account** = a person's ID card to enter the office.
- A **service principal** = a machine's ID card — used by a pipeline, CI/CD tool, or application to enter on its own without a human logging in.
---
**Why not just use a user account for automation?**
|Problem with user accounts|How service principal solves it|
|---|---|
|If the user leaves, pipelines break|Service principal exists independently of any person|
|User password changes break automation|Service principal uses tokens/secrets, managed separately|
|Hard to audit — was it the human or a script?|Clear separation — all automated actions logged under service principal|
|User has too many permissions|Service principal can be given only the permissions it needs|