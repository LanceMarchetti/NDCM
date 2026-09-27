# Non-Destructive Coordinate Mapping Encoder (NDCM)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen.svg)
![Platform](https://img.shields.io/badge/platform-Browser%20%7C%20Client--Side-orange.svg)

A lightweight, zero-dependency client-side application that implements **Non-Destructive Coordinate Mapping (NDCM)**. This tool maps arbitrary payloads (text, URLs, binary streams) to character positions inside unmodified public host text—such as timestamped YouTube comments—exporting the result as a compact, binary `.key` file.

Try It Out Here: [https://lancemarchetti.github.io/NDCM/](https://lancemarchetti.github.io/NDCM/)
---

## 💡 What is Non-Destructive Coordinate Mapping?

Traditional steganography modifies host media—such as altering pixel LSBs in images or tweaking audio frequencies—which leaves detectable artifacts and risks platform re-compression.

**NDCM takes a non-destructive approach:**
1. It treats standard, unmodified public text as a **static reference lookup array**.
2. A single, natural-looking YouTube timestamp comment containing standard Base-16 characters (`0–9` and `a–f`) acts as the matrix.
3. The payload is converted to hexadecimal character coordinates relative to the host comment string.
4. The host comment lives on public servers unchanged, while the coordinate logic resides inside a detached, binary `.key` file.

---

## 🔥 Key Features

* **Zero Footprint / Non-Destructive:** The public host comment is posted once and **never modified or updated**. No large text dumps or unnatural edits to raise flags.
* **Infinite Host Reusability:** Because the host comment is purely a reference matrix, **a single posted comment can serve as the host for thousands of unique `.key` files**.
* **Zero-Password / Self-Sealing Keys:** You never need to define a password, manage a passphrase, or set an initialization vector. The encoder automatically constructs a self-contained binary key structure on every run.
* **Polymorphic Output:** Generating a `.key` file for the *exact same payload* against the *exact same host comment* yields a completely different binary byte signature every time.
* **Decoupled Security:** The host comment alone looks like a harmless list of video timestamps. The `.key` file looks like arbitrary binary noise. Neither reveals anything without the other.
* **Resilient Infrastructure:** Platform updates, UI redesigns, or re-renders do not break the payload mapping as long as the host plaintext remains readable.
* **Compact Raw Binary Export:** Serialized index streams are packed directly into a `Uint8Array` binary blob, reducing the output file size by 50% compared to raw string storage.

---

## 🛠️ How It Works

```
[Target Payload] ──► [Hex Stream] ──► [Host Coordinate Lookup] ──► [Polymorphic Binary .key]
                                                ▲
                                                │
                                    [Unmodified Host String]
                                  (e.g., YouTube Comment)
```

1. **Host Text Validation:** The app verifies that the host comment contains full Base-16 character coverage (`0–9` and `a–f`). Any timestamp list on a video longer than 9 seconds naturally fulfills the `0–9` requirement.
2. **Coordinate Indexing:** The target payload is split into hex nibbles. The encoder locates the positional offset (index) of each required character in the host comment.
3. **Serialization & Dynamic Packing:** The index numbers are serialized using dynamic, randomized delimiters to obscure index values into a standard hex stream, which is then written to disk as a binary `comment.key` file.

---

## 🚀 Quick Start

Since this application is a single, self-contained HTML file with zero external dependencies or build steps:

1. Download or clone `index.html`.
2. Open `index.html` in any modern web browser (Firefox, Chrome, Edge).
3. Paste your public host string into **Panel 1**.
4. Enter your target secret message or URL into **Panel 2**.
5. Click **Download Key (comment.key)**.

---

## 📋 Host String Requirements

To ensure complete coverage for all possible byte combinations, the host string must contain at least one occurrence of each Base-16 character (`0123456789abcdef`).

### Example Valid Host Comment:
> `"fav best are covered at: 0:12 2:36 5:44 6:58 7:49"`

* **Digits `0–9`:** Supplied naturally by video timestamps spanning past 9 seconds.
* **Characters `a–f`:** Supplied by short introductory or descriptive text (e.g., words containing `a`, `b`, `c`, `d`, `e`, `f`).

---

## 🔒 Security & Privacy Considerations

* **Client-Side Execution:** All encoding and binary packaging happen entirely within your local browser runtime. No data is sent to external servers.
* **Decoupled Data:** Without access to the specific host text string, the binary `.key` file cannot be decoded or mapped back to its source bytes.

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
