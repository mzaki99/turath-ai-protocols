# Protocol: Fetching Texts from the OpenITI Corpus into an AI Assistant

## What OpenITI is

An open corpus of Islamicate texts. Its original base draws heavily on the Al-Maktaba al-Shamela (Shamela) digital library, but it is not simply a copy of it: texts have been processed, given unified identifiers, and documented with structured metadata.

**Release 2025.1.9 (February 2026) — Arabic subcorpus:**

| Item | Count |
|---|---|
| Unique titles (excluding edition duplicates) | 8,755 |
| Total text files (all editions/versions) | 13,320 |
| Unique authors | 3,373 |
| Total words | 2,310,017,480 |

The release also includes a Persian subcorpus (351 titles) and a new subcorpus of digitally transcribed manuscripts (433 texts).

**The decisive advantage for a critical-edition researcher:** starting with the 2023 release, the primary (PRI) versions of texts have had modern editorial additions removed — editors' introductions, footnotes, indices — and these versions carry a `CLEANED_VERSION` tag in their metadata. This means the fetched text is closer to the author's bare text alone, substantially lowering the risk of attributing a modern editor's words to a premodern author — arguably the single greatest hazard in working with digitized classical texts.

**A second advantage:** every release carries a fixed DOI on Zenodo, so citing the digital text is academically possible and verifiable, something most digital libraries don't offer.

**What is not claimed:** the processing addressed the organizational layer (identifiers, metadata, tagging), not the text itself. The texts were not collated against their printed editions. The accuracy of the text remains the accuracy of its original source, and most of that source is the Shamela library, with all the variation between one book and another that this entails.

---

## Fetch mechanism

The text is available as a raw file on GitHub, fetched by pasting its link directly into the conversation — no downloading, no extraction, no uploading.

**Link structure:**

```
https://raw.githubusercontent.com/openiti/release/master/data/
[AuthorDeathYear+AuthorName]/[AuthorDeathYear+AuthorName.BookTitle]/[identifier]-ara1
```

The death year is in Hijri, four digits; author and title names follow OpenITI's Latin transliteration system (example author-ID format: `0748Dhahabi`).

**A note on file suffixes:** not every file ends at `-ara1`; some carry a further suffix such as `.mARkdown` or `.completed`, depending on its processing stage. The structure above is a general skeleton: take the exact filename from the GitHub repository or the metadata file rather than guessing it.

**The one non-trivial step: finding the identifier.** Unlike the Internet Archive, a generic pasted link isn't enough here; the author and book identifier must be known first. Two routes:

1. Search the organization's repositories on GitHub — `github.com/OpenITI` — Arabic texts are distributed across repositories organized by Hijri death-century.
2. Download the metadata file accompanying the release on Zenodo, and search it by title or author to extract the identifier.

Once the identifier is obtained once, fetching becomes instant in any later session.

**Current release on Zenodo:** `https://zenodo.org/records/17767721`

---

## Limitations to know

- **Not every book is present.** The corpus covers the period before the 10th Hijri century densely; later coverage is weaker. If a book isn't found, this is stated plainly — no similarly-named substitute is offered as if it were the requested book.
- **Multiple versions exist.** A single book may have more than one digital version, from different print editions. The adopted version is the one tagged `PRI` (primary). Identifying which print edition a text is based on is necessary before citing any page number.
- **Book size versus conversation budget:** every fetched text draws on the "context window" available in that conversation, and this differs by subscription plan. A single volume of a mid-length critically-edited Arabic text (600–700 printed pages) can run into the hundreds of thousands of tokens — on the free plan this alone can use up a large share of the usage limit in one fetch, while paid plans offer a wider window that usually accommodates a full volume. Practical effect: on the free plan especially, requesting a specific chapter or page range rather than the whole book at once is often a practical necessity, not just an option.
- **Print pagination is present unevenly.** What is constant across all texts is the system's own segmentation ("milestones," every ~300 words), meant for computational analysis and unrelated to the printed edition. Print page markers (in the form `PageV00P015`) appear in some texts and not others, depending on what the text inherited from its source. Where present they are useful for reference, but they do not replace consulting the printed edition when citing.
- **Very large books** may have their text truncated in a single fetch, requiring it in batches.
- **Book size versus conversation budget:** every fetched text draws on the "context window" available in that conversation, and this differs by subscription plan. A single volume of a mid-length critically-edited Arabic text (600–700 printed pages) can run into the hundreds of thousands of tokens — on the free plan this alone can use up a large share of the usage limit in one fetch, while paid plans offer a wider window that usually accommodates a full volume. Practical effect: on the free plan especially, requesting a specific chapter or page range rather than the whole book at once is often a practical necessity, not just an option.
- **A link beats uploading the file.** When a book has a text layer, fetching it by link is lighter, faster, and cleaner than downloading and uploading it, because processing pages as images costs several times what plain text costs. Manual upload is reserved for the one case where no text layer exists at all. Likewise, if the assistant is left to search for the book itself, it may spend several successive searches and still fail, so pasting the link saves the researcher's budget, not the tool's.
- **Check the edition before fetching, not after.** Where several digital versions of one book exist, establish which printed edition the text rests on before fetching it. The metadata at the head of the file usually names the editor and the edition year, and the version tagged `PRI` is the adopted one when there is more than one.
- **Zero fabrication:** if the link can't be accessed, or no identifier is found, this is stated explicitly — no identifier or link is invented.

---

## After the fetch

The text is now present, and this is where the work begins rather than ends. What you do with it is not a reading that replaces your own, but an exploration that guides it: you ask about a meaning rather than a word, and it points you to passages you did not know were there. You ask whether this book holds anything bearing on your question, and it spares you leafing through what does not concern you. You ask it to order scattered material into a table, or to collate a passage across two editions to surface omissions and additions.

And each time, tell it: show me the text you built this on. But know that seeing the quotation is not the end of verification. The fetched text is evidence of a location; the judgment is not made until you return to the edition itself. The tool shortens the path to the source; it does not lift from you the duty of examining it.

---

## A worked example (tested and working)

General pattern: paste the link with your request, e.g.:
> "Fetch the text of this book from the following link: [link]"

**Real example:** "al-Du'afa' al-Saghir" ("The Minor Book of Weak Narrators") by Imam al-Bukhari (d. 256 AH), edited by Mahmoud Ibrahim Zayed.

Link:
```
https://raw.githubusercontent.com/openiti/release/master/data/0256Bukhari/0256Bukhari.DucafaSaghir/0256Bukhari.DucafaSaghir.Shia003070-ara1.mARkdown
```

Fetching this link returns the full text along with structured metadata at the top of the file (author name, death year, book title, editor, edition year), followed by the body text divided by `###` markers separating entries. This particular file also carries print-edition page markers (e.g. `PageV00P015`), inherited from its source and not present across the whole corpus; useful for reference, but not a substitute for citing the printed edition itself.

This example was actually tested while writing this protocol, not assumed.

---

A gift to researchers and students, from Dr. Mahmoud Zaki
