# Node.js Final Project — Academic Year 2026-2027

VIVES University of Applied Sciences · Node.js · Milan Dima (milan.dima@vives.be)

This document is the only official description of the final project. It replaces every earlier version (PDF, Word documents and Toledo announcements).

---

## 1. In one minute

- **Build** your own REST API with Node.js, Express and MongoDB that meets requirements P1 to P15 below.
- **Deploy** it so it is publicly reachable, and **submit** it through Classroom 50 at least one week before your oral defense.
- **Defend** it in a 15-minute oral defense in which you explain and change your own code.

Your final grade is your **project score multiplied by how well you master your own code** in the oral defense.

> **A good project you cannot explain is a failed project.**

---

## 2. Grading

### Project score (out of 20)

Your project starts at 20/20. Points are deducted per requirement (P1–P15) and for hard penalties (H1–H5), see [section 7](#7-rubric). Every deduction comes with evidence: a file and line, a commit, or a request and its response.

### Oral mastery (0–100 %)

In the oral defense you answer four questions about your own code and make one small change in it, live. Each of these five items is scored 0 to 3:

| Score | Meaning |
|---|---|
| 3 | You explain it correctly, in your own words, and can go one step deeper |
| 2 | Mostly right, you need a small hint |
| 1 | Vague, or only after several hints |
| 0 | You cannot explain your own code |

Mastery = your points / 15.

### Final grade

| Oral mastery | Final grade |
|---|---|
| below 50 % | project score × mastery, **never more than 7/20**: a fail |
| 50 % up to 90 % | project score × mastery |
| 90 % or more | your project score |

Grades are rounded to whole points (x.5 rounds up).

**Example.** Your project scores 16/20. In the oral you score 12 out of 15 points: mastery 80 %. Your final grade is 16 × 0.80 = 12.8 → **13/20**. With 14 or 15 points (93 % or more) you would keep your 16/20. With 7 points or fewer (below 50 %) you fail, however good the project is.

> **A good project you cannot explain is a failed project.** The oral defense is not a formality: it decides how much of your project score counts.

---

## 3. Requirements

Every requirement has an id. The same ids are used in the rubric, in your evaluation report and in the feedback you receive.

| Id | Requirement | What "done" means |
|---|---|---|
| P1 | A useful application | A real domain with a purpose (a library, a sports club, a webshop...), not a collection of unrelated CRUD screens. |
| P2 | At least 17 endpoints over at least 4 linked collections | Count only real API endpoints (not `/health`). Every collection relates to at least one other collection. |
| P3 | Data modeling | **Embedding is the default in MongoDB.** Use a reference only when you have a reason, and write that reason in `SUBMISSION.md`. Schemas have constraints (`required`, `min`/`max`, `enum`, `unique`...). |
| P4 | Full CRUD | Create, read, update and delete for your collections, including on embedded subdocuments. |
| P5 | Validation | Joi validation on all input (body, params, query). Every ObjectId in a URL is validated **before** it reaches the database. |
| P6 | Authentication | JWT (or a similar technology). Tokens expire. Passwords are hashed (bcrypt). The secret comes from an environment variable. |
| P7 | Authorization | At least 3 roles, for example anonymous visitor, logged-in user and admin. Protected routes check the role. A user can never give themselves more rights (for example by sending `"role": "admin"` when registering). |
| P8 | Error handling | The API never crashes, whatever a client sends. Correct status codes (400, 401, 403, 404, 409, 500). One central error handler. |
| P9 | Middleware | Your own middleware where it has a purpose (auth, validation, errors...). Do not overdo it. |
| P10 | Tests | Unit tests and integration tests that test real behaviour. `npm test` passes. There is no minimum coverage, but the tests must be meaningful. |
| P11 | Code quality | Clear structure (routes, models, middleware...), readable code, configuration through environment variables (`.env`, never committed). |
| P12 | REST Client calls | A request in `requests/*.http` for every endpoint (VS Code REST Client). You don't need to include tokens. |
| P13 | API documentation | `docs/API.md` (or a published equivalent) describes every endpoint with an example request and response. |
| P14 | Deployment | The API is live and stable during the deployment window (see [section 5](#5-deadline-and-deployment-window)). `docs/DEPLOYMENT.md` explains step by step how to deploy it. |
| P15 | Git history | Commit regularly, one feature per commit, spread over the semester. |

The course labs cover every technique you need: Express (lab 04–05), MongoDB and modeling (lab 07–09), auth (lab 10), error handling (lab 11), testing (lab 12) and deployment (lab 13).

---

## 4. Starting and submitting

### Start from the template

Accept the **final project** through **your own group's page** in [Classroom 50 invites](./classroom50/README.md). Classroom 50 creates your private repository from the final-project template:

| Path | What |
|---|---|
| `SUBMISSION.md` | The form you fill in (see below) |
| `src/app.js`, `src/server.js` | Express app with `GET /health`; the server connects to `MONGODB_URI` |
| `test/` | One example test |
| `requests/` | REST Client calls |
| `docs/API.md`, `docs/DEPLOYMENT.md` | Templates for your documentation |
| `.env.example` | The environment variables your app needs |

### SUBMISSION.md

Fill in every `TODO`:

- **Student**, **GitHub username**, **Group** (TI, ACS or AO)
- **Live URL** of your API and the link to your **API documentation**
- **Test user** and **Test admin**: email and password of two **test accounts**, so your API can be tested. Never put a real password or a database connection string in this file.
- **Data model**: your collections, and for every relation whether it is embedded or referenced, and why.
- **Known limitations**: anything that does not work (yet).

### Self-check and submitting

```bash
npm run check     # shows what is still missing
npm run submit    # commit first; submits your current version
```

Every submission runs the same self-check in Classroom 50; the result appears under **Releases** in your repository. **The self-check does not grade your project.** It only tells you what is missing:

| Check | Passes when |
|---|---|
| C1 | `SUBMISSION.md` has no `TODO` left and the live URL is a URL |
| C2 | `npm test` passes |
| C3 | `requests/*.http` contain at least 17 requests |
| C4 | `docs/API.md` and `docs/DEPLOYMENT.md` exist and are no longer the template |
| C5 | No `node_modules`, `.env` or database connection strings with a password committed |
| C6 | `GET <live URL>/health` answers within 60 seconds |

You can submit as often as you like.

---

## 5. Deadline and deployment window

- **Deadline:** one week before your oral defense. The state of the `main` branch at the deadline is what gets evaluated; at that moment the repository becomes read-only.
- Exam dates are not known at the start of the year, so the due date shown in Classroom 50 is a placeholder until the exam schedule is published.
- **Deployment window:** your API must be reachable from the deadline until two days after your oral defense. It is checked regularly during that period.
- Late submission and an unreachable API cost points (H1, H2).

---

## 6. Oral defense

15 minutes, on your own laptop with your repository open.

| Minutes | What | Scored |
|---|---|---|
| 0–3 | Short demo of your live API | no |
| 3–10 | Four questions about **your own code** | 4 × 0–3 |
| 10–15 | One small change in your own code, live | 0–3 |

- The questions point at your own files and lines, for example: *"In your orders route you call `findById` with the id from the URL. What happens when the id is `abc`? Show where that is handled."*
- The live change is small and fits your own project, for example: *"Add a required field to one of your models, with Mongoose and Joi validation, and show the 400 response in your `.http` file."* For the author of the code this takes about five minutes.
- **Allowed:** your laptop, your repository, the course material, the official documentation of Node.js, Express, Mongoose and Joi.
- **Not allowed:** AI tools (ChatGPT, Copilot, Claude...), help from other people, and any code that is not yours.
- You may open your own files to answer. Not knowing where something is in your own project is part of the answer.
- By default the oral defense takes place on campus. AO students and approved exceptions take it on Teams with screen sharing; the format is the same.

---

## 7. Rubric

The project score starts at 20 and never goes below 0. A requirement never costs more than its maximum.

<!-- rubric -->
| Id | Requirement | Max deduction |
|---|---|---|
| P1 | A useful application with a coherent domain | 1 |
| P2 | Express API with at least 17 endpoints over at least 4 linked collections | 2 |
| P3 | Data modeling: embedding by default, every reference justified, schema constraints | 2 |
| P4 | Full CRUD, including on embedded subdocuments | 1 |
| P5 | Validation: Joi on all input, ObjectId validation before every database call | 2 |
| P6 | Authentication: JWT (or similar) with expiry, hashed passwords, secret from the environment | 2 |
| P7 | Authorization: at least 3 roles, protected routes, no privilege escalation | 2 |
| P8 | Error handling: the API never crashes, correct status codes, central error handler | 2 |
| P9 | Middleware: purposeful, not overdone | 1 |
| P10 | Tests: unit and integration tests, meaningful, npm test passes | 2 |
| P11 | Code quality and best practices: structure, readability, configuration via the environment | 1 |
| P12 | REST Client calls for every endpoint | 1 |
| P13 | API documentation: every endpoint with an example request and response | 1 |
| P14 | Deployment: live and stable in the window, deployment guide | 2 |
| P15 | Git history: regular commits, one feature per commit, spread over time | 1 |

| Id | Hard penalty | Deduction |
|---|---|---|
| H1 | Submitted late | -1 per day |
| H2 | Live API unreachable during the deployment window | -1 per day |
| H3 | The app does not start from a clean clone with the documented steps | -10 |
| H4 | Credentials (connection strings, secrets) committed | -2 |
| H5 | node_modules committed | -1 |
<!-- /rubric -->

H1 and H2 are capped at -7 each.

---

## 8. AI policy

- You may use AI tools while developing, like any other tool.
- You must be able to **explain and change every line you submit**. The oral defense checks exactly that, without AI tools.
- Commit regularly. Large amounts of code that appear in a few early commits are discussed at the oral defense and can lead to the plagiarism procedure of the exam regulations.

---

## 9. Second exam period

You continue in the same repository. The same rules apply, with the deadline one week before your new oral defense.

---

## 10. Checklist before the deadline

- [ ] `npm run check` passes C1 to C6
- [ ] At least 17 endpoints over 4 linked collections; references explained in `SUBMISSION.md`
- [ ] Joi on all input, ObjectIds validated, tokens expire, 3 roles, no self-promotion to admin
- [ ] The API never crashes on bad input; `npm test` passes
- [ ] `requests/*.http`, `docs/API.md`, `docs/DEPLOYMENT.md` complete
- [ ] Deployed, and it stays up until two days after your oral defense
- [ ] You can explain every file in your project without help

---

*This assignment is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Author: Milan Dima, VIVES University of Applied Sciences.*
