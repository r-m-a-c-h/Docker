FROM nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1
ENV PIP_ROOT_USER_ACTION=ignore

WORKDIR /app

RUN apt-get update && apt-get install -y \
    python3.10 \
    python3.10-distutils \
    python3-pip \
    python3-venv \
    git \
    curl \
    wget \
    build-essential \
    cmake \
    ninja-build \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10
RUN ln -sf /usr/bin/python3.10 /usr/local/bin/python && \
    ln -sf /usr/bin/python3.10 /usr/local/bin/python3

RUN git clone https://github.com/comfyanonymous/ComfyUI.git /app/ComfyUI

RUN sed -i '/torchaudio/d' /app/ComfyUI/requirements.txt && \
    python3.10 -m pip install -r /app/ComfyUI/requirements.txt

RUN python3.10 -m pip install \
    torch torchvision \
    --index-url https://download.pytorch.org/whl/cu124

RUN mkdir -p /usr/local/lib/python3.10/dist-packages/torchaudio && \
    echo "def __getattr__(name): return None" > /usr/local/lib/python3.10/dist-packages/torchaudio/__init__.py

RUN git clone https://github.com/ltdrdata/ComfyUI-Manager.git \
    /app/ComfyUI/custom_nodes/ComfyUI-Manager

RUN git clone https://github.com/PozzettiAndrea/ComfyUI-TRELLIS2.git \
    /app/ComfyUI/custom_nodes/ComfyUI-TRELLIS2
#RUN python3.10 -m pip install -r /app/ComfyUI/custom_nodes/ComfyUI-TRELLIS2/requirements.txt
#RUN python3.10 -m pip install triton==3.2.0
#RUN python3.10 -m pip install xatlas
#RUN python3.10 install.py
RUN python3.10 -m pip isntall -r requirements.txt --upgrade
RUN python3.10 install.py

RUN git clone https://github.com/PozzettiAndrea/ComfyUI-GeometryPack.git \
    /app/ComfyUI/custom_nodes/ComfyUI-GeometryPack
RUN python3.10 -m pip install -r /app/ComfyUI/custom_nodes/ComfyUI-GeometryPack/requirements.txt

EXPOSE 8188

CMD ["python3.10", "/app/ComfyUI/main.py", "--listen", "0.0.0.0", "--port", "8188"]
