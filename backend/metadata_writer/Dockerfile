FROM python:3.11-slim
RUN apt-get update \
    && apt-get install -y --no-install-recommends libcap2 \
    && rm -rf /var/lib/apt/lists/*
RUN python3 -m pip install --no-cache-dir 'wheel>=0.46.2'
COPY backend/metadata_writer/requirements.txt /kfp/metadata_writer/
ARG TARGETARCH
RUN if [ "$TARGETARCH" = "ppc64le" ]; then \
      /usr/local/bin/pip3 install --no-cache-dir \
        --trusted-host 10.20.186.132 \
        http://10.20.186.132:8000/ml_metadata/ml_metadata-1.21.0-cp311-cp311-linux_ppc64le.whl \
        --extra-index-url https://wheels.developerfirst.ibm.com/ppc64le/linux \
        -r /kfp/metadata_writer/requirements.txt; \
    else \
      /usr/local/bin/pip3 install --no-cache-dir \
        -r /kfp/metadata_writer/requirements.txt; \
    fi

COPY backend/metadata_writer/src/* /kfp/metadata_writer/
CMD python3 -u /kfp/metadata_writer/metadata_writer.py
