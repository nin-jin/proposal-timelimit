# Proposal: Time limits

Time limits for executions.

# Motivation

Sometimes, developer can't predict execution duration and want to limit to prevent freeze whole application for long time.

# Language changes

## New classes

```typescript
class TimeLimitError extends Error {
  timeout : number
  message = 'The time limit is reached'
}
```

## New syntax

`try` should receive optional timeout in parenthesises:

```
const timeout = 1000

try(timeout) {
  // potential long execution
} catch( error ) {
  if( error instanceof TimeLimitError ) {
    // timeouted
  } else {
    // other exceptions
  }
}
```

# Usages

- Prevent freeze-attack to the JS sandbox that runs a user provided code. Example: https://sandbox.js.hyoo.ru/#script=for%28%3B%3B%29%3B
- Time slicing to prevent execution greater than animation frame.
- Prevent app freeze on acidental infinite loop. As example: hot reload in dev mode are stoped on bugs like infinite recursion.
- Stop executions when result is obsoleted after some timeout. Actual for real time data like trading rates, video/audio stream etc.
