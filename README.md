# Repit: AI Rep Counting and Workout Tracking

Repit watches a workout video, tracks your body with pose estimation, and counts every rep automatically. It flags partial reps, measures depth and range of motion for each rep, and speeds up rest periods so you can review your form in seconds.

> Source code is private while Repit is in development as a startup. I'm happy to walk through the code and design in an interview.

## How it works

1. **Pose estimation:** MediaPipe tracks body keypoints in every frame.
2. **Joint-angle signal:** The keypoints become a joint angle over time (knee angle for squats).
3. **Calibration:** Repit learns *your* standing position instead of assuming a fixed angle. In the demo it calibrated to about 142°, compared with a 160° default.
4. **Rep counting:** A rep counts only when you go from standing to depth (below 100° for squats) and back. Smoothing keeps jitter from producing false counts.
5. **Partial reps:** Dips that never reach depth are flagged as partial reps and are not counted.
6. **Rest trimming:** Rest periods play at 8x speed and reps at 1x, so the form-review clip covers only the work.

## Sample output

Each set is saved as structured data:

```json
{
  "exercise": "squat",
  "rep_count": 8,
  "partial_reps_at_s": [21.07],
  "extended_threshold_deg": 141.9,
  "reps": [
    { "index": 2, "bottom_deg": 86.3, "rom_deg": 57.2, "duration_s": 1.601 },
    { "index": 3, "bottom_deg": 86.6, "rom_deg": 57.4, "duration_s": 1.634 }
  ]
}
```

For every rep, Repit records the depth at the bottom, the range of motion and the duration. Slowing rep speed across the set serves as a fatigue indicator. A speed map marks each segment of the video as rep or rest for playback.

## Status

| Feature | Status |
| --- | --- |
| Squat rep counting from pose estimation | Working |
| Per-user standing calibration | Working |
| Partial-rep detection | Working |
| Per-rep depth, range of motion and duration | Working |
| Rest-period speed-up for form review | Working |
| More exercises | In progress |
| Mobile app with on-device processing | In progress |
| Progress analytics (volume, PRs, estimated 1RM) | In progress |
| Local-first sync and automatic video compression | In progress |
| Exercise classifier trained on pose keypoint sequences | Planned |

## Tech

Python · MediaPipe

## Contact

Tajrean Ahmed · tajreanahmed@icloud.com
