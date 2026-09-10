# AWS Bedrock AgentCore Customer Support Chatbot

## Project Overview
This project builds, tests, and evaluates a multi-turn customer support chatbot using the **Amazon Bedrock AgentCore managed harness**, AWS Lambda, DynamoDB, and Bedrock Evaluations[cite: 3, 4, 5]. The system handles three distinct conversational paths driven entirely by prompt engineering:
1. **Bug Reports** — systematically collects missing information (description, steps to reproduce, and environment) across conversational turns before invoking a gateway tool call to store the ticket in DynamoDB.
2. **Platform Questions** — answers order, shipping, return, and payment inquiries using strictly embedded FAQ documentation.
3. **Other Requests** — safely redirects out-of-scope inquiries or unsupported queries to the human support phone line (`1-800-555-0199`)[cite: 4, 5].

---

## Repository Structure & Core Deliverables
* **`system_prompt.txt`** — The primary prompt engineering deliverable defining category exclusivity, bug collection checks, and FAQ grounding rules[cite: 4, 5].
* **`agentcore_config.json`** — Stores active resource ARNs for the harness, gateway, and backend Lambda.
* **`harness-tests.json`** — Test suite covering the bug report, platform question, and out-of-scope routing paths[cite: 2].
* **`output_eval_dataset.jsonl`** — Evaluation dataset generated via the automated test script for Bedrock LLM-as-a-judge assessments.

---

## Verification Evidence & Required Screenshots (For Submission)

To fulfill all grading criteria, include the following visual evidence and terminal transcripts in your final submission document:

### 1. Bug Report Multi-Turn Chat Transcript & Tool Execution
*  Terminal log from `chat.py` demonstrating the sequential, multi-turn collection of troubleshooting parameters.
<img width="1677" height="682" alt="Screenshot 2026-09-10 221041" src="https://github.com/user-attachments/assets/fafb3082-c251-463b-a860-c4a0ecd8fd3c" />

---
  ### 2. 2. DynamoDB Table Record Scan
Description: AWS CLI or console scan output of the bug-report-tool-stack-bug-reports DynamoDB table
<img width="1313" height="659" alt="Screenshot 2026-09-10 221100" src="https://github.com/user-attachments/assets/f424a322-bb17-4408-86b2-094c747c0459" />



### 3. 3. Platform FAQ & Support Hand-off Transcripts
Description: Conversational logs for non-bug paths.
<img width="1115" height="480" alt="Screenshot 2026-09-10 220903" src="https://github.com/user-attachments/assets/537b50ca-bc10-477e-8843-4b4d5060c87f" />



### 4. 4. Bedrock Evaluation Job Results
Description: Screenshot of the completed Amazon Bedrock Evaluation console for job support-chatbot-eval-run-1.
<img width="1269" height="792" alt="Screenshot 2026-09-10 160746" src="https://github.com/user-attachments/assets/abb7b7a0-d3b8-4000-aba0-471b01a2fa2c" />
<img width="993" height="637" alt="Screenshot 2026-09-10 160654" src="https://github.com/user-attachments/assets/bf21047f-d720-44ca-bca1-66ea96c5c4b3" />
<img width="865" height="621" alt="Screenshot 2026-09-10 160708" src="https://github.com/user-attachments/assets/1fdbf6e0-28ec-4ee7-baa0-9acf68c817b9" />

### Observations & Testing Insights
Single-Turn vs. Multi-Turn Behavior: Initial testing indicated that single-turn bug prompts (e.g., "The checkout page crashes") do not immediately commit records to the database[cite: 1, 2]. Because the system prompt enforces a strict multi-turn collection policy, the initial turn correctly leaves the ticket uncreated while prompting the user for missing fields (stepsToReproduce and environment)[cite: 4, 5].

Database Persistence Verification: As confirmed through DynamoDB table scans, no records are written to the database during early conversational turns. Data is strictly committed only after all mandatory parameters are fully gathered, ensuring cleanly populated ticket entries (such as ticket ID 3fb68064-ca23-4976-a3e6-4ac6e97b6fd6) with an OPEN status.

Routing Precision: Running the LLM-as-a-judge evaluation workflow with Builtin.Correctness confirmed that the system prompt's category exclusivity rules successfully prevent cross-contamination (e.g., general FAQ inquiries never trigger accidental bug-reporting logic or unintended tool calls).

### Built WithAmazon Bedrock AgentCore managed harness - Runs the chatbot loop, sessions, and tool executions  
* Amazon Bedrock AgentCore Gateway - Exposes the bug report Lambda as a gateway tool
* Amazon Bedrock Evaluations - LLM-as-a-judge response evaluation
* AWS Lambda - Bug report tool runtime
* Amazon DynamoDB - Ticket storage database  License
























