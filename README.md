# NODRIVER (Reliability Fork)

This fork of `nodriver` focuses on stability and preventing indefinite hangs during browser automation. It introduces critical fixes for connection management and command execution.

## Key Improvements

### 1. CDP Command Timeouts
Commands sent to the browser can now have a timeout. This prevents your script from hanging forever if the browser becomes unresponsive or a network packet is lost.

- **Global Timeout**: Set the `NODRIVER_DEFAULT_TIMEOUT` environment variable (in seconds) to apply a default timeout to all commands.
- **Per-Command Timeout**: Pass the `timeout` parameter to `send()` or any CDP method.

```python
# Example: setting a 10-second timeout for a specific command
await page.send(cdp.runtime.evaluate("1+1"), timeout=10)
```

### 2. Fail-Safe Connection Handling
The library now proactively manages pending transactions when the connection state changes:
- **Immediate Rejection**: If the websocket connection is lost or the listener loop terminates, all pending commands are immediately failed with an exception instead of hanging indefinitely.
- **Synchronous Dispatch**: Commands are now awaited during the send phase, ensuring that connection issues are caught at the moment of dispatch.

### 3. Enhanced Error Reporting
Timeout errors now provide descriptive messages, including the method name and transaction ID, and use nested exceptions (`raise from`) to preserve debugging context.

## Why these changes?
In the original library, certain network conditions or browser hangs could cause the `await` on a command to never return. This fork ensures that every command either succeeds, times out, or fails with a clear error, making it suitable for production environments where reliability is paramount.

---

*Original README content follows below*

---

NODRIVER
=======================

### nodriver provides next level async webscraping and browser automation library for python with an easy interface which Just Makes Sense ™
...
