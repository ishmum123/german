# TODO (v2 candidates)

Residuals from the v1 QA rounds. The rules already in place are described in
`engine/tools/packbuilder/langs/de.py` and summarised in the README.

## Levels
- Levels are frequency bands. Subtitle frequency pulls some B1 exam words
  early: obwohl, Erfahrung, Meinung, Zukunft and Entscheidung sit in A2, and
  trotzdem sits in A1. Bewerbung, Umwelt, Führerschein and Miete fall
  outside the 2000 words. A written-register frequency list, or a larger
  pack, would fix this.

## Sentence links
- Genitive and dative pronoun forms used as possessive-looking words
  ("statt meiner") link the possessive mein.
- Sentence-initial words that the truecaser lowered and the small model
  tagged as nouns stay nouns. Most are right (Essen, Leben), but a few
  imperatives slip through when nothing after them marks the imperative.
- A particle whose verb Wiktionary lacks as a separable verb stays unjoined.
  Examples are "da sein" and "los sein", which are not verbs. REPORT.md
  counts them.
- A possessive the small model tags as a verb after a formal imperative
  ("beschuldigen Sie meinen Sohn": meinen linked as the verb meinen) stays
  wrong. It was 2 of 25 in the second QA round. Routing mein- forms before a
  noun to the possessive would fix it.
- inner has only "im Inneren" as an example, which shows the nominalised
  form rather than the adjective (die innere Stadt).
- Swiss spellings with ss (weisst, heiss) resolve through Wiktionary's
  alternative-spelling entries. Rare Swiss-only words are left unlinked.

## Words and glosses
- Glosses beyond the reviewed ones come straight from the Wiktionary sense
  ranking. About 180 hand overrides fix the high-frequency ones in
  `tools/gloss_overrides.json`. Lower B1 glosses can still lead with a
  secondary or overlong sense.
- Technical plurals are hidden by frequency ("rarely pl."). Plurals that
  are both rare and homographs of a common form are judged from the corpus,
  so a few borderline nouns (Häute, Sichten) show "rarely pl.".
- 23 words, mostly attributive-only adjectives (heutig, ehemalig), have no
  example sentence showing the bare form, because none exists in the corpus.
- Pronoun case forms fold into the nominative lemma (mich, mir to ich). The
  pack has no separate entries for dir, dich, ihm and so on.
- The -in feminine forms (Lehrerin) fold into the masculine noun, as in
  Wiktionary's form-of entries. They are not separate words.

## Engine (vocab-engine repo)
- Lenient typing folds ä to a but does not accept ae/oe/ue/ss. The pack
  adds those spellings as `alt`s on A1/A2 words. An engine-level German fold
  (ae = ä, ss = ß), limited to lenient levels, would be cleaner.
- The id map (`tools/id_map_v1.json`) is frozen. It is keyed by the
  builder's internal lowercase lemma (`haus|noun`, `beamter|noun`), not the
  displayed words.json lemma. `.cache/derived/de_idkeys.json` holds the
  current build's keys.
