# Safety Gear Detector

An end-to-end computer vision application for automated workplace safety compliance monitoring. It utilizes a custom-trained **YOLOv8** model to detect personal protective equipment (PPE)—specifically hard hats and safety vests—in video streams, maintains stable worker IDs via **ByteTrack**, eliminates transient false positives using a **per-person temporal majority-voting rule**, and filters out non-worker bystanders using spatial work-zone boundaries.

## 🚀 Key Features
- **Custom Object Detection:** Trained on construction site environments to detect Hardhats, Safety Vests, and corresponding non-compliance violation classes (`NO-Hardhat`, `NO-Safety Vest`).
- **Robust Video Tracking & Temporal Voting:** Combines ByteTrack with a sliding-window majority vote per tracked worker ID to filter out false alarms caused by temporary motion blur, loose clothing, or partial frame occlusions.
- **Spatial Filtering & Zone Control:** Implements bounding-box height thresholds and pixel coordinate work-zone masks to isolate active workers from distant passers-by and temporary bystanders.
- **Automated Violation Logging:** Captures compliance breaches with snapshot evidence and metadata for auditing through an interactive Streamlit dashboard.

---

## 📊 Model Performance & Metrics
The model was evaluated on custom validation splits, yielding strong metrics for workplace safety compliance:

| Metric / Output | Path / Reference |
| :--- | :--- |
| **Validation Predictions** | [`sample_outputs/val_batch0_pred.jpg`](sample_outputs/val_batch0_pred.jpg) |
| **Confusion Matrix** | [`sample_outputs/confusion_matrix_normalized.png`](sample_outputs/confusion_matrix_normalized.png) |
| **Precision-Recall Curve** | [`sample_outputs/BoxPR_curve.png`](sample_outputs/BoxPR_curve.png) |
| **Threshold Sweep Analysis** | [`sample_outputs/threshold_sweep.png`](sample_outputs/threshold_sweep.png) |

---

## 🛠️ Tech Stack
- **Deep Learning:** Python, Ultralytics YOLOv8, PyTorch
- **Computer Vision & Tracking:** OpenCV, ByteTrack
- **Database & Logging:** SQLite
- **Frontend / UI:** Streamlit

---

## ⚙️ Pipeline Workflow
1. **Inference & Tracking:** Video frames stream through YOLOv8 and ByteTrack to assign and preserve persistent IDs for every individual.
2. **Zone & Size Filtering:** Feet coordinates and minimum box dimensions isolate active workers inside defined workplace zones while ignoring background pedestrians.
3. **Per-Person Temporal Majority Voting:** Rather than triggering flags on isolated single-frame detections, violations require sustained negative evidence (`MIN_VOTES >= 8` out of `MIN_VISIBLE >= 15` frames) before logging an incident.
4. **Database & Dashboard Auditing:** Confirmed compliance breaches are written to SQLite and visualized on the dashboard for a safety officer's review.

---

## ⚠️ Real-World Limitations & Design Considerations
- **Bystanders & Passers-by:** The base detector flags any person in frame. To prevent false violations on non-workers walking past the site, explicit spatial work-zone filtering and minimum-height constraints are enforced.
- **Occlusion Handling:** Loose or unbuttoned safety vests can momentarily trigger single-frame false positives; this is mitigated using temporal majority voting across multi-frame tracking histories.

---

## 📈 Progress Status
- [x] Chunk 1: YOLOv8 basics & environment setup
- [x] Chunk 2: Custom dataset preparation + model training
- [x] Chunk 3: Metrics evaluation & threshold tuning
- [x] Chunk 4: Video inference, tracking, and temporal majority-voting post-processing
- [ ] Chunk 5: Spatial work-zone and fragment filtering
- [ ] Chunk 6: SQLite incident logging & Streamlit dashboard implementation
