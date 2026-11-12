# Checkpoint artifact verification — 12 November 2026

Full is the proposed canonical source and portfolio entry. Compact remains the
companion artifact repository; these packages represent one five-person course
project, rather than two achievements. Its existing artifact access is preserved.

A read-only fetch from Compact downloaded all 388 referenced LFS objects
(1,968,528,560 bytes). Every object matched its pointer's SHA-256 and byte count.
Safetensors headers, dtype/shape lengths, contiguous data offsets and payload
lengths passed static inspection. Both model-only checkpoints contain 194 BF16
shards and 492,110,848 serialized tensor elements each. Full and Compact have
identical relative checkpoint pointer paths, object IDs and sizes. Checkpoint
metadata and the retained SFT model configuration agree across the two copies.

Full's own LFS fetch failed with missing-object messages. Use Compact's linked
artifact access; the source review did not upload, replace, remove or smudge any
tracked pointers. The downloaded cache remains outside committed source.

This verifies byte identity and serialization structure. It does not verify
finite tensor values, training provenance beyond retained metadata, conversion,
inference, benchmark scores or reported individual contributions. The Stage-3
metadata records step 12,000 and 3,145,728,000 tokens for that stage; SFT records
step 5,000 and 81,920,000 tokens. These stage-local counters do not independently
prove the report's full curriculum token total.

The exact external custom SFT tokenizer is absent. Pretraining references the
Meta Llama 3 tokenizer without a pinned revision. The historical exporter uses
RoPE/BOS/EOS settings that differ from retained configuration; keep these as
unresolved equivalence questions rather than silently changing historical code.
Recover and fingerprint the actual tokenizer and qualify conversion/inference in
a suitable numerical environment before publishing reproducibility claims.

The framework's Apache-2.0 notice and project-specific attribution remain
separate from tokenizer, dataset and weight redistribution terms. No new license
acceptance or independent training/evaluation was performed. Public release also
requires credential revocation, history handling and the applicable rights review.

Framework licensing is confirmed against the [official Nanotron license](https://github.com/huggingface/nanotron/blob/main/LICENSE). The configured tokenizer’s supplied material has separate [Meta Llama 3 terms](https://huggingface.co/meta-llama/Meta-Llama-3-8B/blob/main/LICENSE); the actual recovered files and their distribution scope must be established before making a combined licensing claim. The project MIT notice is not a replacement for those terms.
