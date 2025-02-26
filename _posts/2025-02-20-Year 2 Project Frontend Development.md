---
layout: mypost
title: "Year 2 Project: Frontend Development"
categories: [Year 2 Project]
lang: en-us
---

Link to [Chinese Version](https://juntong20xx.github.io/posts/2025/02/20/year-2-project-%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91.html).

The project adopts a frontend-backend separation design. The frontend uses Vue framework with Node.js as the development environment.

![](https://miro.medium.com/v2/0*MAYv38yGNqZa4K1R.)

## Project Structure

```
.
├─public
├─src
│ ├─assets
│ └─components
├─index.html
├─jsconfig.json
├─package.json
└─vite.config.js
```

Each feature is implemented as an independent page, with each page being a Vue Component file.

Functionally divided into two parts: Control Interface and Login Interface.

## Control Interface

Purpose: Real-time display of connected module names, sensor parameters, and user operations.

### Frontend Design
- Display data in a table format
- Utilize Vue's dynamic reactivity for real-time table updates
- Color-coded operation prompts: 
  - "Update" in green 
  - "Logout" in red
  - Success/failure indicators with corresponding colored close buttons

### API Design
- Two main operations: 
  - GET for module list retrieval
  - PUT for action submission

Example using axios:
```javascript
await axios.put(`${this.backendUrl}/items`, { key: item.key, value: item.value })
```

### Testing Issues & Solutions
1. **Real-time refresh requirement**  

   Solution: Implement timer function after login for backend polling  

   Optimization: Avoid refresh during user focus or value modification

2. **Value inconsistency between display and actual parameters**

   Solution: Add warning text for unsaved changes

### Interface Preview
- Control interface display

  ![Screen](Control Page.png)

- Unsaved changes warning state

  ![image-20250226004301640](Control Page with Update.png)

## Login Interface

### Security Features
- TOTP authentication instead of passwords
- Customizable username as secondary authentication factor
- Enforced two-factor authentication

### Frontend Design
Standard login form with fields for:
- Username
- TOTP code
- Backend URL

### API Design
Send authentication request to user-specified backend containing username and TOTP. Backend returns verification result and token.

### Testing Issues & Solutions
**Frozen interface due to invalid backend**  

Solution: Implement timeout mechanism for connection attempts with user notification

![Login Page]( Login Page.png)

## Deployment
Build project using Node.js build command, deploy generated dist directory via Nginx or Cloudflare.

![Pages](Page GIF.gif)