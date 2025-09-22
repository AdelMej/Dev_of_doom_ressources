## 🏷️ Title: [Short descriptive title]

### 📌 Labels:
- 🔵 Feature
- 🟢 High Priority

### 👤 Assigned to:
- @your-teammate

### 📖 User Story:
As a **developer**,  
I want to **add a doubly linked list to the binary tree**,  
So that **I can store and access hierarchical data more flexibly**.

### ✅ Acceptance Criteria:
- Linked list nodes can point to both next and previous nodes
- Binary tree nodes can store and link to these lists
- Passes unit tests covering insertion/removal

### 📅 Due date:
- 2025-08-10

### 🗒️ Notes:
- Remember to update documentation after implementation

## 🔧 Implementation Checklist

- [ ] Define `dll_node_t` struct with `prev` and `next` pointers
- [ ] Integrate DLL into binary tree node structure
- [ ] Implement insert function
- [ ] Implement delete function
- [ ] Write unit tests for all operations
- [ ] Run Valgrind to check memory leaks
- [ ] Document functions in README
