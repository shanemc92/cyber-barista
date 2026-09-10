# cyber-barista &#9749;

A small, offline text encoding/decoding tool. Order the operations, paste the beans, brew. One HTML file, no build step, no dependencies, no network calls. Open index.html and it works, including from file://.

Every step takes bytes and returns bytes, so binary output survives into the next step.

![cyber-barista](docs/screenshot.png)

## Operations

| Group | Operations |
|---|---|
| Encoding | Base64, Base32, Base58, Ascii85, Hex, Binary, Decimal, URL, HTML entities, Unicode escapes, Compression (gzip, zlib, raw deflate) |
| Classical | Caesar, Caesar brute force, Atbash, Affine, Vigenere, Substitution, Rail fence, A1Z26, Morse code, Bacon cipher, Polybius square |
| Hashing | Hash (MD5, SHA-1, SHA-256, SHA-384, SHA-512, CRC-32), HMAC |
| Crypto | XOR, XOR brute force, RC4, ROT13 / ROT-N / ROT47 |
| Parsing | JWT decode with optional HS256 verification, JSON beautify or minify, Timestamp conversion, Defang and refang |
| Analysis | Entropy report with byte distribution, Extract (IPv4, IPv6, domains, URLs, emails, hashes), Strings |
| Text | Find and replace with regex, Line tools, Change case, Reverse, Strip |

Dual operations show an encode / decode switch on the step. Single operations do not.

## Using it

- Paste into the input pane. Output brews automatically, or turn Auto brew off and use the Brew button on large inputs.
- Steps can be reordered, disabled without removing them, and removed. A failing step stops the chain and shows the reason on the step.
- Output can be viewed as text or as a hexdump, copied, or downloaded.
- **Save recipe** writes the steps, their arguments and the current input to JSON. **Load recipe** reads it back.
- Ctrl+S saves, Ctrl+O loads, Ctrl+Enter brews.

## Notes

- Hashing uses the browser's WebCrypto, which needs `https`, `localhost` or `file://`. MD5 and CRC-32 are implemented in the file because WebCrypto does not offer them, and they still work anywhere.
- Compression uses the browser's own `CompressionStream` and `DecompressionStream`.
- MD5, CRC-32, RC4, ROT, XOR and everything under Classical are here for CTFs, analysis and legacy formats. None of them protect anything.
- The classical ciphers take an alphabet: `a-z` keeps case and leaves everything else alone, the wider ones (`A-Z a-z`, alphanumeric, ASCII printable) treat every character in them as part of the ring, and Custom takes your own. Affine rejects a multiplier that shares a factor with the alphabet length, since it would not be reversible.
- The recipe and the current input are kept in this browser's local storage so a reload does not lose work, and a saved recipe file contains any key or secret typed into a step. Reset clears both.
- Loaded recipes are treated as untrusted: unknown operations and argument keys are dropped and every value is coerced to the type its argument expects.
- Base64 and hex decoding are lenient, so whitespace and stray characters are dropped rather than rejected. Base58 is capped, since it is bignum maths and gets slow quickly.
- A pathological regex in Find / replace can hang the tab, the same as it would anywhere else. Reload if that happens, the recipe is restored from local storage.

## Deployment

`_headers` (Netlify, Cloudflare Pages) and `.htaccess` (Apache) carry the same CSP and hardening headers. Neither is needed to run the file locally.

## Licence

MIT. See `LICENSE`.
