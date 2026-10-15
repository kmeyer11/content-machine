# content-machine

Laver Instagram-grafik til BVB-kontoen `@bvb.immer.weiter` (på tysk): kampresultater, plus skabeloner til breaking news, klubudtalelser, transfers og skadesopdateringer. Se `CONTEXT.md` for hvordan det hele hænger sammen.

## Kom i gang

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt && .venv/bin/playwright install chromium
cp .env.example .env    # indsæt API_FOOTBALL_KEY og FOOTBALL_DATA_KEY
.venv/bin/python -m content_machine.cli crests --competition BL1   # hent alle Bundesliga-logoer til media/crests/
```

## Brug

```bash
.venv/bin/python -m content_machine.cli match --club bvb                        # seneste kamp: bekræft at det er den rigtige → post.png
.venv/bin/python -m content_machine.cli new transfer --club bvb --slug x         # start et opslag fra skabelon
.venv/bin/python -m content_machine.cli render posts/…/post.yaml                 # tegn igen efter rettelser
```

## Licens

MIT, se `LICENSE`.
