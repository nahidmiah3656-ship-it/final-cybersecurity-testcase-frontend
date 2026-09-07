# Final Cybersecurity Test Case Frontend

This project contains the exact 137 Test Cases 1-137 supplied by the user in `public/test-cases.md`.

The Markdown file is the single source of truth. The UI renders it directly and does not generate or rewrite test cases.

Security payloads are rendered as text/code only. Raw HTML is not enabled, unsafe URL schemes are sanitized, and a restrictive CSP is included. Normal HTTPS links can only open after an intentional user click.
