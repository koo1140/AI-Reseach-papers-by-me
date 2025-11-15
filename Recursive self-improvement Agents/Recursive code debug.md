# ✅ Full Automatic Debugging Workflow

Suppose we are working on an application with multiple files and nested folders.

We need this exact approach:


---

1. File and folder names cant fully hold value

A file may be called “extras” while it contains MANDATORY functions.

Resolve:

Each file goes alone through the model.
The model produces for each file individually:

a new meaningful file name

a compact description (the entire logic, nested functions, dependencies, flow)


The description compresses:

50k lines of code → ~500 lines of meaning

Function call graphs

Logic order

Data flows

Input/Output patterns


This description must be rich enough that any agent can understand the file without needing the full raw code.


---

2. Scanning process

Resolve:

A main agent with 0 memory gets the entry/main file → goes through it → generates independent sub-agents with 0 memory, each with a single goal.

Examples of independent goals:

Check that dependencies are up to date (and validate documentation online)

Check for mispelled variables, undefined references, incorrect pointers

Check for commented/uncommented code that shouldn’t exist

Check performance-relevant logic

Check file interfaces for mismatches

Check for unused imports, unreachable code, dead branches


Then we have N new agents for N files.
Each of these N agents:

gets one file

uses a separate “file descriptor agent”

fetches the best-fitting descriptions of related files

checks for function incompatibility, interface mismatches, signature drift, type inconsistencies, etc.


❗ NEW ADDITION (Dynamic Execution Layer):

Static analysis is not enough.
Therefore, each sub-agent can spawn a 0-memory mini-agent to execute code safely, but in extremely isolated pieces.

Mini-agents can:

run a single line of code

run one mathematical or logical expression

run an isolated function with mocked dependencies

run constructor/init code

test return values and side effects

detect runtime errors that static analysis cannot catch


They cannot run half the application (too complex),
but they can test small, isolated, meaningful slices of executable code.

This ensures no silent runtime errors slip through.


---

After all agents finish detection

Each agent outputs its own independent error list.
Errors are saved into a tracking folder.

Do not unify the outputs.

They must remain per-file, per-agent.

Merging later would only complicate processing.


This is only detection phase.

No error can slide away because both static and dynamic checks were run.


---

3. Debugging Phase

Since we tracked all errors per file, we start:

A new debugging agent

It gets one file of errors

It fetches only relevant code chunks from other files
(e.g., lines 2078–2134, based on description + dependency mapping)


The model then:

rewrites the broken function

fixes all stated problems

rechecks isolated logic using new dynamic mini-agents

outputs a new debug version of the file


A new independent agent is used for each debug file.

Old debug files are never overwritten.
They are kept for:

research

progress tracking

rollback

audits


They are never reused directly by the model to avoid contamination or overfitting.


---

4. Re-Scanning & Cycle

After finishing one round of debugging,
we run the entire detection system again:

Main agent

Sub-agents

Dynamic execution mini-agents

Error batching

Saving outputs


If new errors appear → another debug cycle starts.
If no errors appear → we break the cycle.


---

⭐ Result

A fully automatic, recursive, self-correcting debugging pipeline, where:

Naming and description steps make the codebase understandable

Static and dynamic scanning ensure no bug survives

Debug agents fix errors with high precision

Previous debug versions are always archived

The process loops until 0 errors remain
