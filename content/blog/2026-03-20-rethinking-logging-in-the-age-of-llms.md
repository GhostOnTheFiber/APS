+++
title = "Rethinking Logging in the Age of LLMs"
date = 2026-03-20
description = "Why traditional logging levels fall short for AI agents, and a proposal for an LLM-specific log level that balances context with noise."
[taxonomies]
tags = ["logging", "llm", "observability"]
+++

You normally have `Error`, `Warning`, `Info`, and `Debug` as your levels for logging. It's worked well. In dev, you run at `Info`, and in cases where you really need to dive deeper, you switch to `Debug` so you can trace what the program is doing. In prod, you might keep it to `Warning` or `Error`, only catching core problems and keeping your logging infrastructure from generating and processing lots of unneeded information. This has worked out very well and is a multi-billion dollar industry in itself.

I think this needs to change with the advent of LLMs and the extensive use of coding agents. A large part of getting a good result from an LLM or agent is the context you feed it. Too little and the agent has to guess, usually incorrectly, or it starts hallucinating. Too much context can have the same result; the agent can no longer see the forest for the trees, you might say. Providers like OpenAI, Anthropic, and Google now offer context windows up to 1 million tokens, but you see degradations in model performance once you start approaching those limits.

This is why I think logging has to change. We can't just feed in errors or warnings. That's too little context for the agent. It's missing information that it then needs to go find. What file did the warning come from? What data was being processed right before the error? But if we try to dump `Info` or `Debug` logs, we could quickly end up overfilling the context window. I've been thinking about an approach that introduces an `LLM` logging level, an amalgamation of `Error`, `Warning`, and `Info`, designed to show the full context and "trace" of issues without the noise.

## What This Might Look Like

Say you have a service that processes user uploads. A file comes in, validation fails, and the request errors out. Here's what you'd typically see at each logging level.

**Error only (prod):**

```
[ERROR] 2026-03-20 14:32:01 - FileProcessingService: Upload failed for request abc-123. ValidationError: unsupported file type.
```

An agent would have little context around what happened and would need to dig into the codebase to understand.

**Debug (what dev might look like):**

```
[INFO]  2026-03-20 14:32:00 - Server: Incoming POST /api/upload from 192.168.1.42
[DEBUG] 2026-03-20 14:32:00 - AuthMiddleware: Token validated for user_id=usr-887
[DEBUG] 2026-03-20 14:32:00 - AuthMiddleware: Permissions check passed: ["upload:write"]
[DEBUG] 2026-03-20 14:32:00 - RequestParser: Content-Type: multipart/form-data
[DEBUG] 2026-03-20 14:32:00 - RequestParser: Boundary detected, parsing body
[DEBUG] 2026-03-20 14:32:00 - RequestParser: File field "document" found, size=4.2MB
[DEBUG] 2026-03-20 14:32:00 - RequestParser: Additional field "category" = "reports"
[INFO]  2026-03-20 14:32:00 - FileProcessingService: Processing upload for request abc-123
[DEBUG] 2026-03-20 14:32:00 - FileProcessingService: Temp file written to /tmp/uploads/abc-123.tmp
[DEBUG] 2026-03-20 14:32:01 - FileValidator: Reading file header bytes
[DEBUG] 2026-03-20 14:32:01 - FileValidator: Detected MIME type: application/x-zip-compressed
[DEBUG] 2026-03-20 14:32:01 - FileValidator: Checking against allowed types: [pdf, docx, png, jpg]
[ERROR] 2026-03-20 14:32:01 - FileProcessingService: Upload failed for request abc-123. ValidationError: unsupported file type.
[DEBUG] 2026-03-20 14:32:01 - FileProcessingService: Cleaning up temp file /tmp/uploads/abc-123.tmp
[DEBUG] 2026-03-20 14:32:01 - Server: Returning 422 to client
```

Now the agent has everything it needs but it's buried inside a wall of noise. This would usually only be a snippet of the debug logs as well.

**LLM level:**

```
[LLM] 2026-03-20 14:32:01 - FileProcessingService: Upload failed for request abc-123
  trigger: ValidationError - unsupported file type
  file: src/services/FileProcessingService.ts:142 → validateUpload()
  input: { filename: "data-export.zip", mime: "application/x-zip-compressed", size: "4.2MB", category: "reports" }
  context: FileProcessingService, FileValidator [pdf, docx, png, jpg] got zip.
  recent_trace:
    - AuthMiddleware: POST /api/upload
    - FileProcessingService: Temp file written,
    - FileValidator: MIME detection returned application/x-zip-compressed
    - FileProcessingService: Upload failed
    - Server: Responce 422
```

The output and format of this is made up, but it shows the overall concept. Being able to deliver a cleaner representation of the issue.

Interested to hear how other people are thinking about this problem.
