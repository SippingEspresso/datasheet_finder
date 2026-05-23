# Hermes Autonomous Operation Guidelines

These guidelines govern the behavior of the Hermes agent while operating autonomously, specifically during long-running or overnight tasks. Adherence ensures safety, resource efficiency, and project integrity.

## 1. Safety & System Integrity
*   **No Sudo/Root**: Never attempt to run commands with `sudo` or modify system-level configurations. Operate strictly within the project directory or the user's home folder.
*   **Dangerous Command Protection**: Maintain the `hermes safety-level` at `smart` or `manual`. Never set it to `off`.
*   **Path Isolation**: Do not delete or modify files outside of the `/home/rings48/datasheet_finder` directory unless explicitly required for project setup (e.g., updating `.bashrc` or local bin).
*   **Credential Protection**: Never log, print, or commit API keys or secrets to Git. Always use `.env` files and ensure they are read silently.

## 2. Error Recovery & Persistence
*   **Self-Correction**: If a tool call or script execution fails, analyze the stack trace/error message and attempt a maximum of 3 different logical approaches before stopping.
*   **State Logging**: Provide concise updates via `update_topic` after every significant step (e.g., "Finished testing 10/125 parts").
*   **Graceful Exit**: If an unrecoverable error occurs (e.g., API key revoked, no internet), log the specific reason in `OVERNIGHT_LOG.md` and terminate the session to save tokens.

## 3. Resource & API Management
*   **Anti-Scraping Delays**: To avoid detection for "bulk downloading" or scraping, the script MUST implement a mandatory **random delay of 2 to 10 seconds** between each API call or datasheet download.
*   **Mouser Rate Limits**: Adhere to the limit of **30 calls per minute**. The random delay mentioned above should naturally keep the script well below this threshold.
*   **Token Efficiency**: Keep context lean. If the session history becomes too large, summarize progress into a file and start a fresh chapter or sub-agent task.
*   **Ollama Memory**: Be mindful of the 7.2GB RAM limit. If the system slows down or swap usage spikes, pause for 60 seconds to allow the event loop to clear.

## 4. Development Standards
*   **Test-Driven Execution**: Before broad implementation, verify logic with at least one part from each of the "Top 20" companies in `IC_TEST_LIST.md`.
*   **Idempotency**: Ensure that scripts can be run multiple times without causing duplicate files or corrupted data.
*   **Output Formatting**: Ensure the `find_datasheet.py` output is clean and standard (JSON or clear Text) so it can be easily parsed by other tools or summarized in the final report.

## 5. Overnight Reporting
*   **Progress File**: Maintain a file named `PROGRESS.md` in the repo. Update it with:
    *   Current timestamp.
    *   Success/Failure count.
    *   Any parts that consistently failed to return a datasheet.
*   **Final Synthesis**: Upon finishing the task or hitting a limit, create a `FINAL_REPORT.md` summarizing the results for human review in the morning.

## 6. Version Control & Collaboration
*   **Atomic Commits**: Commit changes after every significant milestone (e.g., "feat: implement mouser api client", "test: complete verification for TI parts"). Avoid one giant commit at the end.
*   **Descriptive Messages**: Use the Conventional Commits format (e.g., `feat:`, `fix:`, `docs:`, `test:`) to make the history easy to follow.
*   **Verify Before Pushing**: Only push code that has been successfully tested. Never push a broken build or a script that is currently failing its primary objective.
*   **Regular Pushes**: Push to the remote repository after every 3-5 commits or after completing a major section of the `IC_TEST_LIST.md`. This ensures progress is visible and backed up even if the local session terminates unexpectedly.
*   **No Force Push**: Never use `git push --force`. Always pull/rebase if there are upstream changes (though unlikely during overnight autonomous runs).
