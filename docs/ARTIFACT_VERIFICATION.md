# Checkpoint artifact verification — 12 November 2026

The workflow and companion artifact repositories represent one five-person course project.

A read-only fetch from Compact downloaded all 388 referenced LFS objects
(1,968,528,560 bytes). Every object matched its pointer's SHA-256 and byte count.
Safetensors headers, dtype/shape lengths, contiguous data offsets and payload
lengths passed static inspection. Both model-only checkpoints contain 194 BF16
shards and 492,110,848 serialized tensor elements each. Full and Compact have
identical relative checkpoint pointer paths, object IDs and sizes. Checkpoint
metadata and the retained SFT model configuration agree across the two copies.

The workflow repository's own LFS fetch failed with missing-object messages. Use the companion repository's linked artifact access.

This verifies byte identity and serialization structure. It does not verify
finite tensor values, training provenance beyond retained metadata, conversion,
inference or benchmark scores. The Stage-3
metadata records step 12,000 and 3,145,728,000 tokens for that stage; SFT records
step 5,000 and 81,920,000 tokens. These stage-local counters do not independently
prove the report's full curriculum token total.

The exact external custom SFT tokenizer is absent. Pretraining references the
Meta Llama 3 tokenizer without a pinned revision. The historical exporter uses
RoPE/BOS/EOS settings that differ from retained configuration; keep these as
unresolved equivalence questions rather than silently changing historical code.
Recover and fingerprint the actual tokenizer and qualify conversion/inference in
a suitable numerical environment.
