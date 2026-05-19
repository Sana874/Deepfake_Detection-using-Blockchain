# Deepfake Detection using Blockchain

> Upload an image. Get a deepfake prediction, a Grad-CAM explanation of *why* the model decided that, and an **immutable record of the result logged on the Ethereum blockchain** — all in one pipeline.

Built with **Xception (deep learning)** + **Web3/Solidity (blockchain)** + **Grad-CAM (explainable AI)**.

---

## Why This Project Stands Out

Most deepfake detectors are a black box — they give you a label with no explanation and no audit trail. This system solves both problems:

- **Explainability** — Grad-CAM heatmaps highlight *which regions* of the image triggered the deepfake classification
- **Provenance** — Every detection result is hashed (SHA-256) and logged immutably on Ethereum, so results can't be tampered with after the fact
- **Production-ready pipeline** — CLI, web interface, and programmatic API all supported

---

## Pipeline Architecture

```
Image Input
     ↓
Preprocessing (resize to 299×299, normalize)
     ↓
Xception Model Inference → Real / Fake + Confidence Score
     ↓
SHA-256 Hash of Input Image
     ↓
Grad-CAM Heatmap Generation (XAI)
     ↓
Blockchain Logging → Immutable record on Ethereum (Web3)
     ↓
IPFS Storage (optional, decentralized)
```

---

## Features

- **Xception-based deepfake detection** — state-of-the-art CNN architecture trained on GAN-augmented data
- **Grad-CAM visualizations** — explainable AI heatmaps showing which facial regions influenced the prediction
- **Blockchain provenance** — Ethereum smart contract logs every detection result immutably
- **SHA-256 integrity hashing** — ensures the input image hasn't been tampered with
- **IPFS integration** — decentralized storage for detection outputs (optional)
- **Streamlit web app** — clean UI for uploading images and viewing results
- **CLI + programmatic API** — flexible usage for research or integration

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Deep Learning | PyTorch, Xception (299×299) |
| Explainable AI | Grad-CAM |
| Blockchain | Solidity ^0.8.20, Web3.py, Ethereum |
| Smart Contract | `DeepfakeProvenance.sol` |
| Hashing | SHA-256 |
| Storage | IPFS (optional) |
| Web Interface | Streamlit |
| Language | Python 3.8+ |

---

## Project Structure

```
Deepfake_Detection-using-Blockchain/
│
├── app.py                        # Streamlit web interface
├── inference_pipeline.py         # End-to-end pipeline (main entry point)
├── model_inference.py            # Xception model inference
├── xai_gradcam.py                # Grad-CAM heatmap generation
├── blockchain_client.py          # Web3 blockchain client
├── hash_utils.py                 # SHA-256 image hashing
│
├── contracts/
│   └── DeepfakeProvenance.sol    # Ethereum smart contract
│
├── outputs/
│   └── xai/                      # Saved Grad-CAM visualizations
│
├── requirements.txt
└── .env.example
```

---

## Quick Start

**1. Install dependencies**
```bash
pip install -r requirements.txt
```

**2. Configure environment**
```bash
cp .env.example .env
# Add your blockchain credentials to .env:
# CONTRACT_ADDRESS=your_deployed_contract_address
# CONTRACT_ABI=your_contract_abi
# PRIVATE_KEY=your_wallet_private_key  ← never commit this!
```

**3. Deploy the smart contract**

Deploy `contracts/DeepfakeProvenance.sol` to your Ethereum network (local Hardhat/Ganache or testnet), then update `CONTRACT_ADDRESS` and `CONTRACT_ABI` in `.env`.

**4. Run**

Web interface:
```bash
streamlit run app.py
```

Command line:
```bash
python inference_pipeline.py path/to/image.jpg
```

Programmatic:
```python
from inference_pipeline import run_inference_pipeline

result = run_inference_pipeline("image.jpg")
print(f"Label:         {result['label']}")
print(f"Confidence:    {result['confidence']}")
print(f"Blockchain TX: {result['tx_hash']}")
```

---

## Smart Contract Functions

| Function | Description |
|----------|-------------|
| `registerModel(name, weightsHash)` | Register a model version on-chain |
| `logDetection(modelId, mediaHash, label, confidence, mediaCid, xaiCid)` | Log a detection result immutably |
| `getDetection(detectionId)` | Retrieve a past detection record |
| `getModel(modelId)` | Retrieve registered model info |

---

## Security Notes

- Never commit private keys — use `.env` and ensure it's in `.gitignore`
- Validate all image inputs before passing to the blockchain pipeline
- Account for Ethereum gas costs when deploying at scale
- Private keys should ideally use a hardware wallet or secrets manager in production

---

## Requirements

- Python 3.8+
- PyTorch 1.9+
- Ethereum node (local Ganache/Hardhat or remote via Infura/Alchemy)
- Solidity ^0.8.20

---

## Tech Used
`Python` · `PyTorch` · `Xception` · `Grad-CAM` · `Solidity` · `Web3.py` · `Ethereum` · `SHA-256` · `IPFS` · `Streamlit`
