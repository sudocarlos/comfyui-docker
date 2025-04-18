# FROM pytorch/pytorch:2.3.0-cuda12.1-cudnn8-runtime
# FROM pytorch/pytorch:2.6.0-cuda12.4-cudnn9-runtime
FROM pytorch/pytorch:2.6.0-cuda12.6-cudnn9-runtime

ENV DEBIAN_FRONTEND=noninteractive
ENV PIP_PREFER_BINARY=1
ENV ROOT=/stable-diffusion

RUN apt-get update && \
  apt-get install -y git && \
  apt-get clean

RUN --mount=type=cache,target=/root/.cache/pip \
  git clone https://github.com/comfyanonymous/ComfyUI.git ${ROOT} && \
  cd ${ROOT} && \
  git checkout v0.3.29 && \
  pip install -r requirements.txt

WORKDIR ${ROOT}
COPY . /docker/
RUN chmod u+x /docker/entrypoint.sh && \
  cp /docker/extra_model_paths.yaml ${ROOT}

ENV NVIDIA_VISIBLE_DEVICES=all
ENV PYTHONPATH="${PYTHONPATH}:${PWD}"
ENV CLI_ARGS=""

EXPOSE 7860
ENTRYPOINT ["/docker/entrypoint.sh"]
CMD python -u main.py --listen --port 7860 ${CLI_ARGS}
