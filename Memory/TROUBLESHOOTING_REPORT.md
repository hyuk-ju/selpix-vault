---
type: research
note_status: fleeting
confidence_level: low
source_agent: openclaw-main
created: 2026-02-19
---
# Troubleshooting Report 🛠️

## 1. Web Search & Browser Infrastructure
**Problem**: Agents complain about "Brave API key missing" and "Browser tab not connected".
**Root Cause**:
- **Search**: The `.env` file is missing `BRAVE_API_KEY`, but agents may be attempting to use the default `web_search` tool which often maps to Brave.
- **Browser**: The `fast-browser-use` container (`openclaw-sandbox:squad`) is running, but the connection might be unstable or the agent is trying to attach to a closed CDPS session.

**Solution (Do Not Proceed Yet)**:
1.  **Switch Search Engine**: Explicitly force agents to use the installed `duckduckgo` skill (which requires no API key) instead of generic `web_search`.
2.  **Browser Reset**: Restart the browser service via `docker restart openclaw_agent` to refresh the connection.
3.  **Environment**: Add `BRAVE_API_KEY` to `.env` if Brave is preferred (optional, DuckDuckGo is a fine alternative).

## 2. Coupang "Number Type" JSON Error
**Problem**: Coupang API rejects option fields (Quantity/Capacity) with "Number type only" error, even when you think you are sending numbers.
**Root Cause**:
- **JSON Serialization**: In JavaScript/Node.js, if the data comes from an LLM response or string input, it looks like `"3"` (String) instead of `3` (Number).
- **Strict Typing**: Coupang's legacy API often requires purely numeric types for certain `attributeValueName` or specific metadata fields.

**Technical Fix Strategy**:
1.  **Payload Inspection**: We must log the *exact raw JSON* being sent right before the API call to verify the data type.
    ```javascript
    console.log(JSON.stringify(payload, null, 2)); // Check if quotes are around numbers
    ```
2.  **Force Conversion**: Explicitly wrap variables in `Number()` or `parseInt()`:
    ```javascript
    // Bad
    quantity: "3"
    
    // Good
    quantity: Number(data.quantity)
    ```
3.  **Schema Validation**: Use a library like `zod` to enforce numeric types before transmission.

---
**Status**: Analysis complete. Waiting for your approval to execute these fixes.

---
## 관련 문서
- [[📊 대시보드|📊 대시보드]]
- [[Projects/셀픽스-쿠팡-파이프라인|🛒 쿠팡 파이프라인]]
- [[Memory/운영-규칙|⚙️ 운영 규칙]]
- [[Memory/장기기억|🧠 장기기억]]
- [[History/지금까지-한-일|📅 전체 타임라인]]
