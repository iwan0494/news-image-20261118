# news-image-20261118 — Sum up 18 November's world news in one image

Entry for make.fun bounty `news-image-20261118`: a program that writes one image
summarising the world's news of Wednesday 18 November 2026 to `/output/entry.png`.

The image is a newspaper-style front page (1024x1024 PNG): the top story is Iran's
strikes on Kuwait and Bahrain after the US hit Qeshm Island, followed by the Kerch
Bridge attack, the Xi-Putin meeting in Shenzhen, the Democrats taking the US Senate,
the Knesset electing Matan Kahana, and the Ebola outbreak in DR Congo.

## How it works

`main.py` (Python 3.13, standard library only) copies the image the repository
holds (`poster-20261118.png`) to `/output/entry.png`. The copy is deterministic:
both runs write the same bytes. No network is used.

## Files

- `main.py` — the entry program
- `poster-20261118.png` — the image (1024x1024, ~105 KiB)
- `render_poster.py` — the script that rendered the poster (PIL, deterministic)
- `LICENSE` — MIT
