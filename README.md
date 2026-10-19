# FUTURE_CS_02: Phishing Email Detection & Awareness Report

**Programme:** Future Interns, Cyber Security Track, Task 2 (2026)
**Prepared by:** Sudip Bhalla
**Date:** 10 October 2026

## Objective

Analyse phishing emails the way a security analyst would, classify each one as **Safe / Suspicious / Phishing**, explain the attacks in plain language, and turn the findings into an awareness guide that non-technical employees can use.

This is security education and analysis. No system was attacked, and no link or attachment was opened.

## Results at a glance

Six emails were reviewed: one template from the task brief, four real unsolicited emails from a public phishing collection, and one genuine Google security alert from my own inbox (used as the safe comparison).

| # | Email | SPF / DKIM / DMARC | Score | Classification |
|---|---|---|---|---|
| 01 | Account-lock template (task brief) | No headers | 6 | Phishing (content only) |
| 02 | Fake Visa World Cup prize | none / none / none | 8 | Phishing |
| 03 | Fake energy tariff refund | fail / fail / none | 9 | Phishing |
| 04 | Fake reward | none / fail / none | 8 | Phishing |
| 05 | Roof repair advert | pass / pass / pass | 5 | Suspicious |
| 06 | Google security alert (genuine) | pass / pass / pass | 0 | Safe |

## Key findings

- **Names are easy to fake.** Three of the four phishing emails put a trusted-sounding name ("Visa", "Klantenservice Energie", "Emily @ Rewards") on an address that had nothing to do with it.
- **Links gave the strongest evidence.** The two link domains I could check were registered 9 and 41 days before the emails were sent, and 11 and 14 of 93 VirusTotal vendors flag them.
- **Passing technical checks is not enough.** The roof advert passed SPF, DKIM and DMARC but was still misleading, because it used three unrelated domains.
- **A genuine email behaves differently.** The Google alert passed every check and all links stayed on Google's own domains.

## Repository contents

```
FUTURE_CS_02/
├── README.md
├── report/
│   ├── Phishing_Awareness_Report.docx
│   └── Phishing_Awareness_Report.pdf
├── samples/
│   ├── 01-account-lock-template.txt
│   ├── 02-fake-visa-prize.txt
│   ├── 03-fake-energy-tariff.txt
│   ├── 04-fake-reward.txt
│   ├── 05-roof-ad-suspicious.txt
│   └── 06-legitimate-google-alert.txt
└── screenshots/
    └── (analyser, WHOIS and VirusTotal screenshots, named by email number and tool)
```

- **`report/`** is the full Phishing Detection & Awareness Report: indicators, risk classification, how the attack works, prevention guidelines, Do's and Don'ts, and what to do if you clicked.
- **`samples/`** has sanitised copies of each email (headers, readable text and links). Links and addresses are defanged, and hidden padding text and tracking blobs were removed. The genuine sample contains headers only, with personal data redacted.
- **`screenshots/`** holds the tool output used as evidence.

## Tools used

| Tool | Purpose |
|---|---|
| Google Admin Toolbox: Messageheader | SPF, DKIM and DMARC results and sender details from raw headers |
| MXToolbox: Email Headers | Second opinion on alignment and the relay path |
| ICANN Lookup (RDAP) | Domain creation dates and nameservers |
| VirusTotal | Checking link domains against security vendors (searched only, never visited) |
| Plain text editor | Reading raw `.eml` files safely, without a mail program |

## Analysis approach

Every email went through the same five checks, in the same order:

1. **Headers:** are SPF, DKIM and DMARC present and passing, and does the delivery path make sense?
2. **Sender:** does the display name match the real address and domain?
3. **Links:** where does each button really go, how old is the domain, and do security vendors flag it?
4. **Language:** urgency, threats, too-good rewards, generic greeting?
5. **Request:** what does the email want me to do (click, log in, pay, share a code)?

### Scoring rule

Each email earns points for red flags: failed or missing authentication (2), display name mismatch (2), new or flagged link domain (2), urgency or too-good reward (1), generic greeting (1), asks for credentials, money or a click (1), unidentifiable sender (1).

- **Safe:** 0 to 1 points and every authentication check passes
- **Suspicious:** 2 to 5 points, or the checks pass but the sender or content is deceptive
- **Phishing:** 6 points or more, with a failed or missing authentication check or a deceptive, flagged link

## Safe handling

- No link and no attachment was opened. Emails were read as plain text.
- Domains were checked only through lookup sites (ICANN, VirusTotal).
- All links and addresses in the evidence are defanged (for example `hxxp://example[.]com`).
- My own email address was removed from the genuine sample before it was saved.

## Limitations

- Six emails is a small sample. Four similar reward and casino emails were reviewed and left out because they repeat the pattern of Emails 03 and 04.
- Authentication results can differ between tools and over time. Email 05 passed at delivery but fails when its sender's records are re-checked now.
- Domain reputation and age were checked on 8 to 9 October 2026. VirusTotal results change over time.
- I searched the front page of each link domain, not the exact tracking links in the emails.
- Email 01 is a template with no headers, so its classification rests on content alone.
- None of the samples carried an attachment.

## References (study only, not copied)

- [phishing_pot](https://github.com/rf-peixoto/phishing_pot) by rf-peixoto
- [phishing-mail-examples](https://github.com/autinerd/phishing-mail-examples) by autinerd

## Disclaimer

This work was done for education as part of the Future Interns Cyber Security internship. The sample emails are real unsolicited messages from a public collection. Do not click links or open attachments from unknown emails.
