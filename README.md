# Google Colab CLI

A command-line tool for interacting with Google Colab runtimes directly from your terminal.

## Installation

Install via [`uv`](https://github.com/astral-sh/uv):

```bash
uv tool install google-colab-cli
```

> **Note:** Supported on macOS and Linux.

---

## Quick Start

### 1. Manage Sessions

List active sessions:

```bash
colab sessions
```

Provision a runtime (defaults to CPU):

```bash
# Create a CPU session with a custom name
colab new -s my-session

# Create without name (generates a random 6-character ID)
colab new
```

Provision with a GPU or TPU accelerator:

```bash
# Named GPU session (T4, L4, A100, H100)
colab new -s my-gpu-session --gpu T4

# Named TPU session (e.g., v5e1)
colab new -s my-tpu-session --tpu v5e1
```

> **Tip:** Always pass `-s <name>` (or `--session <name>`) to give your session a memorable name. This makes it easy to target in commands like `colab exec -s <name>`, `colab ssh -s <name>`, and `colab stop -s <name>`. If omitted, Colab assigns an auto-generated hex ID.

### 2. Run Code

Execute a script on the remote runtime:

```bash
colab exec -f script.py
```

Or execute inline code:

```bash
echo "import torch; print(torch.cuda.is_available())" | colab exec
```

### 3. Interactive Shell & Bash Access

Connect directly to a remote bash shell on a specific session:

#### Option A: Interactive Console (Recommended)
Attach to a live TTY / tmux terminal on the VM:

```bash
colab console -s <session_name>
```

#### Option B: SSH Connection & IDE Remote Development
Open an SSH terminal session or attach your IDE (VS Code / Cursor):

```bash
# Connect directly via SSH terminal
ssh colab

# Or via the CLI
colab ssh -s <session_name>
```

> **IDE Setup (VS Code / Cursor):**
> 1. Press `Cmd + Shift + P` $\rightarrow$ **Remote-SSH: Connect to Host...**
> 2. Select `colab`
> 3. Open `/content` to view and edit files directly on the GPU VM.

#### Run Single Bash Commands
Execute one-off shell commands using the `!` prefix:

```bash
echo "!nvidia-smi" | colab exec -s <session_name>
echo "!df -h" | colab exec -s <session_name>
```

### 4. Stop the Session

Release remote resources when finished:

```bash
colab stop
# Or stop by specific name:
colab stop -s <session_name>

# If an orphaned session appears as '[?]' in `colab sessions`:
./scripts/clean_sessions.sh
```

---

## Documentation

* [YOLO Model Training Guide](docs/yolo_training.md) — Step-by-step instructions for provisioning remote GPUs, syncing datasets, executing training, and retrieving checkpoints.
* [Managing Packages & Files](docs/packages_and_files.md) — Installing Python packages (`uv`/`pip`), uploading files, zip dataset transfers, and Google Drive mounting.

---

## Ephemeral Run

Run a script and automatically terminate the VM when finished:

```bash
colab run -f script.py --gpu T4
```

---

## Help

To see all available commands and flags:

```bash
colab --help
```
