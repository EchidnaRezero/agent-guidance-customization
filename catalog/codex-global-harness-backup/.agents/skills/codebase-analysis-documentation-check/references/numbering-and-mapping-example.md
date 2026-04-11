# Numbering and Mapping Example

## Overall View

```text
A. Web Request Flow
|-- A1. Route Entry
|-- A2. Input Validation
`-- A3. Response Output

B. Background Job Flow
|-- B1. Job Trigger
|-- B2. Worker Logic
`-- B3. Result Save
```

## [A1] Route Entry: receives the HTTP request

- The request enters through the route handler.
- The handler passes the request body to validation.

## [A2] Input Validation: rejects invalid request data

- The validator checks required fields and value format.
- Valid data moves to the response logic.

## [A3] Response Output: returns the final HTTP response

- The response builder creates the output body.
- The route handler returns the response to the client.

## [B1] Job Trigger: starts one background job

- A scheduler or button starts the job.
- The trigger passes the job payload to the worker.

## [B2] Worker Logic: processes the job payload

- The worker reads the payload and performs the main task.
- The result moves to the save step.

## [B3] Result Save: stores the processed result

- The saver writes the result to storage.
- The job ends after the save succeeds.
