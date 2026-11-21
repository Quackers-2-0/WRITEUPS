# Challenge Name

FormCraft

# Description

FormCraft let users make forms and write their own JavaScript validation rules. The website said those rules ran inside a sandbox, while the flag was stored on the same server.

# Solution

The challenge included the Node.js server files, so I looked at how custom validators were checked and run. The application parsed our JavaScript with **Acorn**, blocked some dangerous words and expressions, and then ran the validator inside a Node.js `vm` context.

The biggest mistake was that the VM had access to the **host's `Object` constructor**. That gave us a way back to the host `Function` constructor without directly writing something like `.constructor` in the code.

The key expression was:

```javascript
Object.getOwnPropertyDescriptor(
  Object.getPrototypeOf(Object),
  'constructor'
).value
```

From there, a function created from a string could access Node.js objects such as `process`, then reach the filesystem. The AST filter checked the JavaScript syntax we submitted, but dangerous code inside a **string** wasn't checked in the same way. That was the hole in the sandbox.

We built a validator named `escapeFlag` that used this route to read useful server information and try flag paths with Node's `fs` module. Creating it through `/api/validator` worked, but the next test request gave me an unexpected response:

```text
Unknown validator
```

That was confusing because the validator had just been created. Looking at the source, the validators were stored in a normal **in-memory object**. The website also used a load balancer, so two requests could end up on different server processes that didn't share that memory.

The next approach was to keep the create-and-test requests together, using a persistent HTTP session or repeating the pair until they reached the same backend. The export doesn't keep the exact final successful request, so I can't say which retry option was the one that worked. I did confirm the flag afterward.

There was also a suspicious `deepMerge()` function that might allow prototype-related changes, but I don't have evidence that it was needed for this solve. The main lesson was that **a JavaScript sandbox isn't safe just because it blocks dangerous-looking syntax**, especially if objects from the host are passed into it.

# Flag

```text
CYBERLEAGUE{pr0t0_4st_vm_ch41n_c0mb0}
```

# Solved By

Kazikiri
