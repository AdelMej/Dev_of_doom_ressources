# 🚀 General Trello Setup for Coding Projects

---

## 🧱 Columns (Lists)

	1. **📥 Backlog**  
   - Raw ideas, feature requests, long-term goals  
   - Not ready to work on yet

2. **✅ To Do**  
   - Refined tasks, clear and ready to be picked up  
   - Described with user stories and checklists

3. **🚧 In Progress**  
   - What’s actively being worked on  
   - One person per task if possible

4. **🧪 Review / Testing**  
   - Code needs testing or code review  
   - Includes test writing, documentation, peer feedback

5. **⛔ Blocked / Bugged**  
   - Waiting on something (info, fix, merge conflict)  
   - Anything broken or stuck

6. **🏁 Done**  
   - Fully complete and functional, passes tests

---

## 🏷️ Labels (Use Colors for Priority and Type)

| Color     | Label Name       | Purpose                                      |
| --------- | ---------------- | -------------------------------------------- |
| 🔵 Blue   | Feature          | New functionality (e.g., authentication, UI) |
| 🔴 Red    | Bug              | Fix something that’s broken                  |
| 🟢 Green  | High Priority    | Must be done soon                            |
| 🟣 Purple | Documentation    | README, inline comments, project structure   |
| ⚫ Black   | Low Priority     | Optional ideas, cleanup tasks                |
| 🟠 Orange | Needs Discussion | Unclear requirements or design choices       |
	
> 💡 Tip: You can assign multiple labels to a card (e.g., `Feature` + `High Priority`).

---

## 📝 Template Card (General Feature)

**Title:**  
`[Feature] User login functionality`

**Description (User Story Style):**  
> *As a user, I want to log in securely so that I can access my account data.*

**Checklist Example:**
- [ ] Create login form
- [ ] Connect form to backend
- [ ] Validate credentials
- [ ] Handle incorrect logins
- [ ] Write unit/integration tests

**Labels:**  
`Feature`, `High Priority`

---

## 🐛 Template Card (Bug Fix)

**Title:**  
`[Bug] Crashes on empty input in search field`

**Description:**  
> Steps to reproduce:  
> 1. Go to search  
> 2. Submit with no input  
> 3. App crashes with null pointer error

**Checklist:**
- [ ] Reproduce the issue
- [ ] Patch logic to handle empty input
- [ ] Add regression test

**Labels:**  
`Bug`, `High Priority`

---

## 👥 Assigning Tasks

- Add yourself or teammates to the card via **“Members”**  
- Add **Due Dates** for short-term goals  
- Use **comments** for status updates and decisions

---

## 📅 Suggested Workflow

- Every Monday (or sprint start), move important cards from **Backlog** → **To Do**  
- During the week, pull from **To Do** into **In Progress**  
- When done, move to **Review / Testing** for peer checks  
- Finally, move to **Done**

---

