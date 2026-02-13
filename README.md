# Encrypted LLM (Split Learning Demo)

This project demonstrates a **Split Learning** approach for Large Language Models (LLMs). The model execution is divided between a local client and a remote server. The client processes the initial layers locally to keep raw input data private, sending only intermediate hidden states to the server for the heavy lifting.

## Architecture

- **Model**: [TinyLlama/TinyLlama-1.1B-Chat-v1.0](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)
- **Split Point**: Layer 4 (Configurable via `SPLIT_LAYER`)
- **Client**: Handles embedding and initial 4 layers.
- **Server**: Handles remaining layers and language modeling head.

## Technology Stack

- **Python 3.10+**
- **PyTorch**
- **FastAPI** (Server)
- **Transformers** (Hugging Face)

## Setup

1.  **Clone the repository**:
    ```bash
    git clone <repository_url>
    cd Encrypted_LLM
    ```

2.  **Create a virtual environment** (Recommended):
    ```bash
    python -m venv venv
    # Windows
    .\venv\Scripts\activate
    # Linux/Mac
    source venv/bin/activate
    ```

3.  **Install dependencies**:
    ```bash
    pip install torch transformers fastapi uvicorn requests
    ```

## Usage

### 1. Start the Server

The server loads the model and listens for hidden states on port 8000.

```bash
uvicorn server:app --reload --port 8000
```

### 2. Run the Client

The client processes a prompt locally and sends the intermediate representation to the server.

```bash
python client.py
```

## Security Note

This implementation currently uses **Split Learning** without additional encryption (like HE or MPC) on the transmitted hidden states. While the raw input text is not sent to the server, the intermediate representations (activations) are sent in plain text.

## Directory Structure

- `server.py`: FastAPI server handling the upper layers of the LLM.
- `client.py`: Client script handling tokenization and lower layers.
- `.gitignore`: Configured to exclude heavy model weights and virtual environments.
