# nanlm-cpu-6

Disposable **NaNLMind head runner** slot 6. Holds no model weights and no
persistent data.

Dispatched by `NaNLMind/.github/workflows/ring.yml`. The job:

1. checks out `NaNLMind` and `deeprepo`;
2. builds the NaNLMind core;
3. runs `runner/nanlm_worker.py --op headshift --slot 6 --epoch <epoch>`;
4. writes one packet set for heads `h % 6 == 5` to `nanlm-ram`;
5. releases the lease; the next slot is dispatched only after this run finishes.

## Required setup (once)

Add secret `DEEPREPO_PAT` to this repo. The same token must have:

- Contents R on `NaNLMind` and `deeprepo`
- Contents RW on `nanlm-ram` and `nanlm-vram`
- Actions RW on all `nanlm-cpu-*` repos

Optional variables: `NANLM_MAIN` (default `OWNER/NaNLMind`),
`NANLM_RAM` (default `OWNER/nanlm-ram`), `NANLM_VRAM`
(default `OWNER/nanlm-vram`).

The Qwen demo stays on `deeprepo-cpu-1..5`; this ring is separate.
