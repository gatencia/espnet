# ALFFA Swahili Broadcast News ASR Recipe (OpenSLR SLR25)

## Corpus

This recipe trains an ASR model on the Swahili broadcast-news portion of the
ALFFA (African Languages in the Field: speech Fundamentals and Automation)
corpus, distributed via OpenSLR: https://www.openslr.org/25/

- License: MIT
- ~9.5 hours train audio (10,180 utterances, minus the 634 held out as `sw_dev`)
- ~1.8 hours test audio (1,991 utterances)
- `sw_dev` (634 utterances) is carved out of the shipped train set, since the
  corpus ships only train/test splits. Two whole recording sessions are held
  out (never individual utterances split from a session), so no audio leaks
  between train and dev. See `local/data_prep.py` for details.

Citation:

```
@InProceedings{gelas:hal-00954048,
  author    = {Gelas, Hadrien and Besacier, Laurent and Pellegrino, Francois},
  title     = {Developments of {S}wahili resources for an automatic speech recognition system},
  booktitle = {SLTU - Workshop on Spoken Language Technologies for Under-Resourced Languages},
  year      = {2012},
  address   = {Cape Town, South Africa},
}
```

## Usage

```bash
./run.sh
```

This recipe defaults to `--ngpu 0` (CPU) since it was developed without cluster
access. Once you have a GPU (e.g. via a PSC/Bridges-2 or NCSA Delta account),
re-run with a GPU-enabled config, e.g.:

```bash
./run.sh --ngpu 1 --nj 32 --inference_nj 32
```

## Notes on this recipe

- The corpus's own `wav.scp` ships with placeholder audio paths (e.g.
  `/my_dir/wav/...`), not real ones. `local/data_prep.py` does not assume a
  fixed internal directory layout for the downloaded tarball; instead it
  indexes every `.wav` file under the download directory by filename and
  matches utterances to audio by basename. Any utterance whose audio can't be
  located is dropped with a warning (see stderr from `local/data.sh` stage 1).
- No language model is used (`--use_lm false`) to keep the recipe's scope
  minimal; this is a reasonable target for a first contribution and can be
  added later.
- `local/metadata/` bundles the small (a few MB total) canonical
  train/test `text`/`utt2spk`/`wav.scp` files, sourced from
  https://github.com/besacier/ALFFA_PUBLIC (the versioned companion repo to
  the OpenSLR release), since those don't reliably ship inside the OpenSLR
  tarball itself.

## Results

_TODO: fill in after training completes -- WER on `sw_test`, and a comparison
against the Gelas et al. (2012) baseline._
