# Rebuttal update review

This change reconciles the public evaluation code with the revised Speech-DRAME
manuscript. The original public branch predates the rebuttal analyses.

## Required corrections

- Mask human N/A annotations per dimension instead of treating them as score 1.
- Keep missing or malformed model scores separate from human annotation masking.
- Report the mean of dimension correlations, with counts and per-language results.
- Validate authentic-speech evaluation against explicit training and held-out IDs:
  the 2,164-record authentic pool includes 1,080 adaptation records and 1,084
  clean test records. The entire pool is not a held-out test set.
- Correct the documented benchmark-distribution results to archetype 0.629 and
  realism 0.602. Authentic-speech results are -0.142 pooled and +0.029 within
  speaker; the earlier positive claim used an overlapping evaluation list.
- Document the controls, uncertainty analyses, exact checkpoint identities, and
  outstanding release artifacts without implying that unavailable data are public.

## Review status

Initial inspection confirms that baseline scorers currently convert human N/A
ratings to 1 and silently substitute missing model scores. The README reports
pre-rebuttal values. Further code review and validated implementation are in
progress in this draft PR.
