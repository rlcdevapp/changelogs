# RLCDEV Changelog

## v1.1
### New Features
- **Public question tags**  
  Added a tagging system so each problem can be filtered by topic, difficulty, and company. Tags now appear on all problem pages and list views.

- **AI grading for long-answer questions**  
  Integrated the Groq **Llama-3.3-70B-versatile** model using an on-demand grading pipeline.  
  Supports rubric-based scoring, reasoning checks, and explanation output.

- **Company list on homepage**  
  Added a structured company index so users can browse questions by company (Marvell, AMD, Qualcomm, Nvidia, etc.).

- **User feedback system**  
  Feedback form added to all problems and global navigation.

### UI / UX Improvements
- **New navigation bar**
  - Submit Question  
  - Feedback  
  - Cleaner, consistent spacing and layout
- **Improved problem pages**
  - Better formatting for long-answer questions
  - Clear metadata panel (tags, company, difficulty, type)

### Internal Changes
- Refactored question schema (tags, company arrays, grader hooks, metadata fields).
- Added clean separation between interactive, auto-graded, and AI-graded problems.
- Updated API response shapes and reduced payload size for faster page loads.
- Small performance improvements across homepage feed and problem viewer.

### Docker & Infrastructure
- **Containerized backend services**  
  All graders (C and Verilog) now run inside isolated Docker containers to guarantee deterministic builds and safe execution.
  
- **C backend container**
  - Alpine-based image with gcc and runtime tools  
  - Compiles user code inside a sandboxed environment  
  - Captures stdout/stderr and enforces CPU & memory limits  
  - Stateless per-run architecture for security and reproducibility  

- **Verilog backend container**
  - Includes the full Verilog toolchain (Icarus Verilog, waveform generator)  
  - Runs user testbenches to produce VCD files  
  - Container isolation prevents unlimited file I/O and system access  
  - Enforced runtime and size limits on waveform generation  

- **Shared container patterns**
  - Non-root execution  
  - Ephemeral file systems per request  
  - Consistent API contract between containers and core backend  
  - Graceful crash handling and timeout protection  

- **Improved Docker orchestration**
  - Reduced container size for faster cold boots  
  - Cleaned up build layers for more efficient deployments  
  - Standardized logs to help debug grading failures  

---

## v1.0 (MVP)
### Core Features
- **Initial set of 50 technical questions** from:
  - digital hardware  
  - embedded systems  
  - analog circuits  
  - VLSI  
  - PCB design  
  - control systems  

- **C backend**
  - Secure compile-and-run sandbox  
  - Input injection and stdout capture  
  - Test case evaluation in containerized environment

- **Verilog backend**
  - Icarus Verilog compile + simulate  
  - Waveform generation (VCD)  
  - Testbench-driven validation

- **Progress tracking**  
  Stored attempt history and completion state.

- **Basic feedback system**  
  Early version of user feedback submissions.

### Infrastructure
- **Initial Dockerized grader architecture**
  - First versions of C and Verilog containers  
  - Minimal orchestration with simple exec wrappers  
  - Stateless request–response model
- Basic auth, sessions, and DB schema for problems and test cases.
