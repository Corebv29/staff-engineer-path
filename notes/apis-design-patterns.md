## Aplication Programming Interfaces

**Stateless** 
    Involves only independent input

**Statefull** 
    Involves previous requests or previously stored data (who you are or previous requests you did)

**Resources**
    Key concepts we store and interact with

```
<StandardMethod><Resource> --- CreatePerson()
```

RPC better fit for stateless, why ?

### APIs using RPC (Remote Procedure Call) style are a better fit for stateless architectures mainly because:

Self-Contained Requests:
    In RPC, each call fully defines what the server must do. The request typically contains all necessary information: method name, parameters, etc.
    → This means the server does not need to remember previous requests or "session state" between calls.
    Example:
```json
{
"method": "getUserDetails",
"params": { "userId": "12345" }
}
```

Every call is independent.

Decoupled Client and Server Memory:
Since the server doesn't store client-specific context between requests (no session, no memory of earlier interactions), scaling is much easier. New incoming requests can go to any server without "sticky sessions" or extra load balancing tricks.

Predictable Behavior:
RPC APIs behave deterministically:
Call A → Output A, Call B → Output B, regardless of history.
Statelessness ensures that RPC methods can be tested, debugged, and scaled more easily.

Better for High Availability and Load Balancing:
Stateless services are simpler to duplicate and distribute across many nodes — and RPC’s "one request = one action" fits this model perfectly.

### How do you measure a good API ??

**Operational** -> Do what it is supposed to do
**Expressive** -> Express the think you want to do clearly and simple 
**Simple** -> Expose the functionality that users want in the most straightforward way possible
**Predictable** -> APIs rely on repeated, predictable patterns