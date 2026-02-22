# Memory Management Mastery Quiz

**Scenario-Based Questions:**

1. **The Leaky Web Server:** Your web server application starts with 100MB of RAM usage but grows to 2GB after a few days of operation. What memory issue is likely occurring, and how would you investigate it?

2. **The Crashing Demo:** During a critical product demo, your application crashes with a segmentation fault. You suspect it's memory-related. What are three possible causes, and what immediate steps would you take to diagnose the problem?

3. **The Multithreaded Challenge:** You're building a chat application where multiple users send messages simultaneously. How would you handle shared data between threads to avoid race conditions?

**Practical Application:**

4. **Language Choice Matters:** You're deciding between C++ and Java for a high-performance server application. How would memory management considerations influence your decision? What are the trade-offs?

5. **Security First:** A junior developer on your team writes code vulnerable to buffer overflows. How would you explain why this is a security issue, not just a bug, and what preventive measures would you recommend?

**Tool Mastery:**

6. **Debugging Arsenal:** Your C++ application occasionally crashes in production but works fine during development. Which two debugging tools would you use, and what's the key difference in their approach?

7. **Performance Profiling:** You notice your Java application is using 500MB of heap when you expect 50MB. What profiling techniques would you use to identify the memory bloat?

**Design Decisions:**

8. **Architecture Trade-offs:** You're designing a memory allocator for a multithreaded game engine. Why would you implement thread-specific "arenas," and how would this improve performance?

**Bonus - Real-World Problem Solving:**

9. **The Mystery Crash:** A user reports that your application crashes after exactly 45 minutes of use. What systematic approach would you take to isolate whether this is a memory-related issue?