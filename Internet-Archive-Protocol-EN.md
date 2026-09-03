# Protocol: Fetching Books from the Internet Archive into an AI Assistant

## Purpose

Bring the full text of a book from the Internet Archive directly into a conversation with an AI assistant (such as Claude) — no downloading archives, no unzipping, no manual file uploads. It's recommended to save this file to the assistant's project knowledge or reference files, so the protocol is known in every session without re-explaining it.

---

## The core rule

What's required from the researcher each time = **pasting one link**. No downloading, no extraction, no uploading.

Why: the code-execution environment for some assistants has no direct internet connection, so it cannot run a script that reaches out to the archive on its own. But the "fetch page" tool (web fetch) available to these assistants works on any link **that appears in the conversation itself** — i.e., any link the researcher pastes directly.

---

## Step one, always: paste the book's general page link

`https://archive.org/details/IDENTIFIER/...` — whatever its exact form. No need to know the text file's name in advance.

The item's page is fetched, and the **DOWNLOAD OPTIONS** section is checked to see **what is actually available for this specific book** — not every book carries the same formats. This check is mandatory before any fetch, since assuming a given format exists without verifying can be wrong.

## Priority ladder (tried in order, stopping at the first available format)

**1. `FULL TEXT` (a `..._djvu.txt` file)** — always best: pure text, ready immediately.
The fetch link runs through: `/stream/IDENTIFIER/part-name_djvu.txt`

**2. `PDF WITH TEXT` (a `..._text.pdf` file)** — if no djvu.txt exists. This PDF carries an OCR text layer under the images; it's fetched and its text extracted directly, with no manual download.

**3. `HOCR` (a `..._hocr.html` file)** — a last automatic fallback. HTML text containing words with their coordinates; readable but heavier and less clean than the two options above.

**4. None of the above is available** (the book is images only: `JP2 ZIP` or `IMAGE CONTAINER PDF` with no text layer) —
At this point, stop and say so explicitly. No automatic fetch is possible for a book with no OCR layer at all, because the fetch tool doesn't display page images for remote visual reading. The only option then is manually uploading the needed pages (or file) to be read as images — and this is a rare exception, not the norm.

**Note on `OCR SEARCH TEXT` (`_hocr_searchtext.txt.gz`):** this format sometimes exists but is compressed (gz), and the fetch tool doesn't decompress automatically — so it's effectively skipped in the ladder, moving on to what follows it.

## When the automated fetch itself fails, not the format

Tier four above concerns a book with no text layer at all. A different case can arise: the format exists and the text is complete and clean, but the automated fetch fails for a purely technical reason. Two known causes:

- **Explicit blocking of the `/details/` path**: some item pages block fetch tools from opening them at all, so even checking DOWNLOAD OPTIONS becomes impossible.
- **URL length**: some `/stream/` links encode the full filename in the URL, and this can exceed a technical limit in the fetch tool, especially when the filename is a long descriptive title in a non-Latin script.

**In both cases, the fix is: open the link yourself in a browser, save the page (Save As), and upload it directly as a file in the conversation.** This is a fundamentally different path from the manual upload described in tier four: there, the text is genuinely absent, so the page is uploaded to be read as an image; here, the text exists and is complete, and the obstacle lies only in the automated access route.

**An unconfirmed technical note:** some long `/stream/` links carry a decorative descriptive text segment between the item name and the actual filename, which appears removable without breaking the link. Removing it might shorten the URL enough to allow direct fetching without manual upload, but this has not yet been confirmed to work, and is worth testing before relying on it.

---

## Limitations to know

- **Very large books**: the text may be truncated in a single fetch, requiring it to be split across successive batches — with no uploading involved.
- **Book size versus conversation budget:** every fetched text draws on the "context window" available in that conversation, and this differs by subscription plan. A single volume of a mid-length critically-edited Arabic text (600–700 printed pages) can run into the hundreds of thousands of tokens — on the free plan this alone can use up a large share of the usage limit in one fetch, while paid plans offer a wider window that usually accommodates a full volume. Practical effect: on the free plan especially, requesting a specific chapter or page range rather than the whole book at once is often a practical necessity, not just an option.
- **Uneven OCR quality**: even when a text file exists, descriptive text (book and author names) is usually reliable, but numbers (death years, manuscript numbers) are more prone to scanning errors and need additional verification. Peer-reviewed studies record commercial OCR accuracy on classical Arabic texts at between 65% and 75%, contrary to vendor claims. More seriously, OCR can drop a word or an entire line without visible trace, not merely distort it. What reaches you is therefore a pointer to a place in the book, not a substitute for reading it in the original.
- **A link beats uploading the file**: when a book has a text layer, fetching it by link is lighter, faster, and cleaner than downloading and uploading it, because processing pages as images costs several times what plain text costs. Manual upload is reserved for the one case where no text layer exists at all. Likewise, if the assistant is left to search for the book itself, it may spend several successive searches and still fail, so pasting the link saves the researcher's budget, not the tool's.
- **Check the edition before fetching, not after**: the Archive holds several editions of the same book, some critically edited, some poor, some scanned from an old uncorrected printing. The item page usually states the publisher and year, so read it before you fetch. Text drawn from an edition that carries no scholarly weight corrupts whatever is built on it, however clean the fetch.
- **Zero fabrication**: if a fetch fails, or no text format exists, or part of the text can't be read, this is stated explicitly and no substitute is invented.

---

## After the fetch

The text is now present, and this is where the work begins rather than ends. What you do with it is not a reading that replaces your own, but an exploration that guides it: you ask about a meaning rather than a word, and it points you to passages you did not know were there. You ask whether this book holds anything bearing on your question, and it spares you leafing through what does not concern you. You ask it to order scattered material into a table, or to collate a passage across two editions to surface omissions and additions.

And each time, tell it: show me the text you built this on. But know that seeing the quotation is not the end of verification. The fetched text is evidence of a location; the judgment is not made until you return to the edition itself. The tool shortens the path to the source; it does not lift from you the duty of examining it.

---

## A worked example (tested and working)

General pattern: paste the link with your request, e.g.:
> "Fetch the text of this book from the following link: [link]"

**Real example:** Ibn Khaldun's Muqaddimah, the Tunis edition, edited by Ibrahim Shabbouh and Ihsan Abbas, Bayt al-Hikma 2007, the edition based on the author's own holograph copy (the Atif Efendi manuscript).

General page link: `https://archive.org/details/mokadema-khaldon-1`

The item holds two volumes, each in `FULL TEXT` format: `MokademaKhaldon1_djvu.txt` (the main text) and `MokademaKhaldon2_djvu.txt` (the appended indices and glossaries), both at the top of the priority ladder.

This example was actually tested while writing this protocol, not assumed.

---

A gift to researchers and students, from Dr. Mahmoud Zaki
