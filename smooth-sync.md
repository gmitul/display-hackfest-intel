# Tear Reduction/Smooth (A)Sync

[← 2026](2026)

## Overview

- introduced to smoothly transition between old and new frame during an async flip.
- It uses both blending and dithering to achieve this.
- Can be exposed as a CRTC property.

![](images/Picture1.png)

## Different levels of smoothening
- Smooth Sync disabled
![](images/enable_tr_0_frame-kms_chamelium_frames-hdmi-frame-dump-(null)-4.png)
- Level 1
![](images/enable_tr_1_frame-kms_chamelium_frames-hdmi-frame-dump-(null)-2.png)
- Level 2
![](images/enable_tr_2_frame-kms_chamelium_frames-hdmi-frame-dump-(null)-1.png)
- Level 3
![](images/enable_tr_3_frame-kms_chamelium_frames-hdmi-frame-dump-(null)-4.png)
- Level 4
![](images/enable_tr_4_frame-kms_chamelium_frames-hdmi-frame-dump-(null)-3.png)
- Level 5
![](images/enable_tr_5_frame-kms_chamelium_frames-hdmi-frame-dump-(null)-0.png)

## Performance penalty

Enabling Smooth Sync reduces async flip throughput. The higher the level, the greater the performance cost.

Test: `kms_async_flips --r async-flip-speed-test` on Wildcat Lake (Linux 6.19.0-rc4) on an HD display

| Subtest | Baseline (flips/vblank) | Level 1 (flips/vblank) | Level 5 (flips/vblank)|
|---------|----------|---------|---------|
| pipe-A-eDP-1-4 | 6537 | 3109 | 713 |
| pipe-B-eDP-1-4 | 6600 | 2543 | 712 |
| pipe-C-eDP-1-4 | 6538 | 3177 | 713 |

At level 1 the flip count drops to ~50% and at level 5 it drops to ~10% of the baseline.
