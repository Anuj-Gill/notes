#tech-notes

### `@`
- Used to **specify a version** or **package scope**
- Examples:
  - `express@4.17.1` → install exact version
  - `@nestjs/core` → scoped package i.e. install core package from nestjs namespace

---

### `^` (caret)
- Allows **minor** and **patch** updates, but **not major** updates
- Example: `^4.17.1` → updates allowed within `4.x.x`
- Common default for most dependencies

---

### `~` (tilde)
- Allows **only patch** updates
- Example: `~4.17.1` → updates allowed within `4.17.x`
- Safer and more stable than `^`

---
