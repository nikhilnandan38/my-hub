---
name: token-saver
description: Minimize token use and usage. Use when the user asks to save tokens or usage, or when a PDF, image or large file is attached or mentioned. Converts PDFs and images to Markdown text first, reads only the parts needed, and suggests a cheaper model for simple steps.
---

# token-saver

## Rules

1. **Convert before reading.** Never read a PDF or image in full if a text version can be made. Write the text to `/tmp/tokensaver/<name>.md` and work from that file afterwards. Don't re-read the original.
   - PDF: run `pdftotext -layout file.pdf /tmp/tokensaver/<name>.md`. If `pdftotext` is missing, try `python3 -c` with `pypdf`. If neither works, read only the needed pages with the Read tool's `pages` parameter, and save what you read to the .md file.
   - Image: run `tesseract image.png /tmp/tokensaver/<name>` for text. If tesseract is missing, view the image once, write the text it contains to the .md file, and work from that.
2. **Read in pieces.** For large text files, search with Grep first, then read only the matching ranges with offset and limit.
3. **Keep output short.** Summarize tool results. Don't paste long command output back into the chat.
4. **Pick the cheapest model that can do the step.**
   - Lookups, conversion, renaming, formatting, listing: Haiku.
   - Normal writing and coding: Sonnet.
   - Hard reasoning, architecture, difficult debugging: Opus.
   - This skill cannot switch the model of the current session. Tell the user the one-line command to switch, for example `/model haiku`, and let them decide.
   - For a single step that doesn't need the current model, delegate it with the Agent tool and set `model` to `haiku` or `sonnet`.
5. **Estimate before big jobs.** For a long task, state in one line what it will cost in rough terms and the cheaper option, then continue unless the user objects.
