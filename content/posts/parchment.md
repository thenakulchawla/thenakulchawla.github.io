---
title: "Parchment - contextual logging in go"
date: 2024-06-08
draft: false
---

In many Go projects, structured logging becomes an essential tool for observability. When building microservices or backend APIs, logging with contextual information like request IDs, user IDs, or operation names becomes vital. One challenge, however, is passing logger instances through multiple layers of function calls without cluttering signatures or breaking separation of concerns.

This is where [parchment](https://github.com/thenakulchawla/parchment) comes in — a minimalist wrapper around [zerolog](https://github.com/rs/zerolog) that enables logger propagation using Go’s native context.Context.

### Why Pass Loggers via Context?

Traditionally, logger instances are passed around explicitly, like so:

```go
func handler(logger zerolog.Logger) {
    dbCall(logger)
}

func dbCall(logger zerolog.Logger) {
    logger.Info().Msg("querying db")
}
```


While this works, it leads to bloated function signatures and makes deep call stacks harder to trace. More importantly, you lose the ability to attach structured fields at the entry point (say, a request) and have them persist automatically across the stack.

Instead, by embedding the logger into a context, you can attach key-value pairs once, and automatically include them in all logs downstream.
### What Is Parchment?

parchment is a lightweight library that solves this elegantly:

1. It injects a zerolog.Logger into context.
2. It allows adding structured fields to the logger in-place, updating the context in a functional style.
3. It provides convenient helpers to extract the logger and log messages anywhere the context is available.

### Basic Usage

```go
package main

import (
	"context"

	"github.com/thenakulchawla/parchment"
)

func main() {
	ctx := context.Background()
	ctx = parchment.New(ctx)
	ctx = parchment.AddToLogger(ctx, []parchment.LoggerField{
		{Key: "request_id", Value: "abc-123"},
		{Key: "user", Value: "nakul"},
	})
	handle(ctx)
}

func handle(ctx context.Context) {
	log := parchment.FromContext(ctx)
	log.Info().Msg("handling request")
}

```

The output will look like:

```json
{"level":"info","request_id":"abc-123","user":"nakul","message":"handling request"}
```

### Inner Workings

parchment defines a context key type internally and stores a zerolog.Logger instance in the context. When calling AddToLogger, the library retrieves the existing logger, appends fields using With(), and returns a new logger injected into a new context.

To safely extract and use the logger, it provides functions like:

- FromContext(ctx context.Context) zerolog.Logger: safely retrieves the logger or returns a no-op logger.
- AddToLogger(ctx, fields): adds multiple fields.
- Convenience functions like Info(ctx), Error(ctx), Debug(ctx) return corresponding zerolog event writers.

This is done in a safe, type-checked way to prevent panics and ensure fallback behavior.
### Benefits

1. Cleaner Function Signatures

You no longer have to pass logger as an argument across functions. The context already exists for request-scoped data; adding the logger to it is natural and clean.

2. Lifecycle-bound Fields

With each function having access to the same context, structured fields like request_id, job_name, or retry_count remain consistent and automatically attached to logs.

3. Better Tracing and Observability

When logs across a request, job, or event share the same contextual fields, it’s easier to trace them in tools like Loki, Elasticsearch, or even just grep.

4. Plug-and-Play with zerolog

If you’re already using zerolog, parchment is a non-intrusive addition — just wrap your logger at the entry point and forget about it.

When Should You Use It?

parchment is ideal when:

- You’re building microservices and want per-request tracing.
- You have deep call stacks where passing the logger explicitly is cumbersome.
- You use zerolog and want a simple way to propagate fields.

If you’re building a CLI or small script where global logging suffices, parchment might be overkill. But for larger systems with concurrency, retries, and multiple goroutines — context-based logging is a huge win.

### Conclusion

Logging is more than just printing messages — it’s about maintaining observability across layers and lifecycles. With parchment, you can structure logs in a way that naturally fits Go’s idioms, especially its use of context.

Give it a try:
[github.com/thenakulchawla/parchment](https://github.com/thenakulchawla/parchment)

Let your logs follow your context — not the other way around.