# VP Shunt Valve Detector – YOLOv8‑Large

A fine‑tuned **YOLOv8‑large** model for automatic detection and classification of 11 ventricular‑peritoneal (VP) shunt valves on skull radiographs.

---

## 🌟 Key Features

- **Ready‑to‑use weights** (`weights/vpshunt_yolov8l.pt`) – just load and run.
- **Gradio demo** – launch an interactive web UI or Jupyter notebook in seconds.
- **YOLO‑style dataset & config** – standard Ultralytics layout with `data/vpshunt.yaml` bundled.
- **Extendable** – retrain or add classes with only a few lines of code.

---

## 📂 Repository Layout

```
VP‑Shunt‑Detector/
├── data/
│   └── vpshunt.yaml              # dataset config (shown below)
├── weights/
│   └── vpshunt_yolov8l.pt        # fine‑tuned model (≈ 90 MB)
├── notebooks/
│   └── inference_gradio.ipynb    # launchable demo
└── README.md                     # you are here
```

> **Note:** `vpshunt.yaml` is packaged so that Ultralytics can locate it automatically for both training **and** inference:
>
> ```python
> from ultralytics import YOLO
> model = YOLO('weights/vpshunt_yolov8l.pt')     # names baked into weights
> # ...but keep yaml alongside weights for reproducibility & re‑training
> ```

---

## 🚀 Quick‑start

### 1. Install dependencies

Follow the official **Ultralytics Quickstart** instructions: [https://docs.ultralytics.com/quickstart/](https://docs.ultralytics.com/quickstart/)

For a minimal environment (Linux/macOS):

```bash
python -m venv .venv && source .venv/bin/activate
pip install --upgrade pip
pip install ultralytics gradio opencv-python
```

### 2. Launch the Gradio demo

```bash
python - <<'PY'
from ultralytics import YOLO
import gradio as gr
model = YOLO('weights/vpshunt_yolov8l.pt')  # weights autoload class names

def predict(img):
    results = model(img, conf=0.25)
    return results[0].plot()

gr.Interface(fn=predict,
             inputs=gr.Image(type='pil', label='Skull Radiograph'),
             outputs=gr.Image(label='Detections')).launch()
PY
```

Or open `notebook/inference_gradio.ipynb` and run the cells.

---

## 🔧 Fine‑tuning / Adding Classes

1. **Prepare your dataset** in YOLO format (`images/` + `.txt` labels).
2. **Update ** `data/vpshunt.yaml` or create a new YAML including all classes (existing + new).
3. **Train** starting from these weights:

```bash
# example: continue training 100 epochs at 640 px
yolo train model=weights/vpshunt_yolov8l.pt \
           data=data/vpshunt.yaml \
           epochs=100 imgsz=640 \
           project=runs_vpshunt name=exp_finetune
```

YOLO automatically expands the detection head for new classes when the YAML specifies a larger `names` list.

---

## 📑 Dataset Config

```yaml
path: yolo # adapt to your directory
train: images/train
val: images/val
test: images/test

names:
  0: Codman Certas
  1: Medtronic Strata
  2: Codman Hakim
  3: Medtronic Delta
  4: Aesculap proGAV
  5: Sophysa Polaris
  6: Sophysa Sophy
  7: Aesculap proGAV 2
  8: Integra DP
  9: Spitz Holter
  10: Codman Fixed
```

Adapt `path:` if your dataset lives elsewhere.

---

## ⚠️ Disclaimer

This repository is provided **as‑is for research and educational purposes**. The model has **not been evaluated or cleared by the U.S. Food & Drug Administration (FDA)** and is **NOT intended for clinical decision‑making or patient care**. Use at your own risk.

---

## 📝 License

- **Ultralytics YOLOv8 framework:** AGPL‑3.0 – see [https://github.com/ultralytics/ultralytics/blob/main/LICENSE](https://github.com/ultralytics/ultralytics/blob/main/LICENSE)