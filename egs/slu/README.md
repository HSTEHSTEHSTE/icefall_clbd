# SLU recipe for Fluent Speech Commands

This contains the recipe for performing the SLU task for the Fluent Speech Commands (<https://fluent.ai/fluent-speech-commands-a-dataset-for-spoken-language-understanding-research/>) dataset.

Data preparation: 

```
./prepare.sh [data_dir] [target_root_dir]
```

Model training:

```
python transducer/train.py --exp-dir [exp_dir] --lang-dir [data_dir]/lm/frames --seed [seed] --feature-dir [data_dir]/fbanks
```

## Clean Label Poisoning Attacks (CLBD) against Icefall SLU

A number of prelimary data generations scripts that you need to run before starting CLBD experiments:

- Generate targeted PGD-attacked data dump (attacks all eligible samples): 
```
python transducer/pgd_attack.py
```
- Generate untargeted PGD-attacked data dump (attacks all eligible samples): 
```
python transducer/pgd_attack_untargeted.py
```
- Rank the adversarially attacked samples (requires running transducer/pgd_attack.py first)
```
python transducer/pgd_rank.py
```

CLBD data generation: 
- Perform Dirty Label Poisoning Attack (no PGD required):
```
python transducer/generate_poison_wav_dump_norm.py
```
- Perform Clean Label Poisoning Attack:
```
python transducer/generate_poison_wav_dump_norm_adv.py
```
- Perform Ranked Clean Label Poisoning Attack (requires transducer/pgd_rank.py):
```
python transducer/generate_poison_wav_dump_norm_rank.py
```
