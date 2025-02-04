FROM nvidia/cuda:11.7.1-devel-ubuntu22.04

# Update, install
RUN apt update && \
    apt install -y ffmpeg curl

# Create user instead of using root
ENV USER='user'
RUN groupadd -r user && useradd -r -g $USER $USER
RUN mkdir -p /home/$USER
RUN chown -R $USER /home/$USER
RUN chmod a+rwx -R  /home/$USER
USER $USER


# Define workdir
RUN mkdir /home/$USER/app
WORKDIR /home/$USER/app

# Install Rye

RUN curl -sSf https://rye-up.com/get | RYE_VERSION="0.6.0" RYE_INSTALL_OPTION="--yes" bash


# Install project

COPY --chown=user:user main.py .
COPY --chown=user:user get_model.py .
COPY --chown=user:user .python-version .
COPY --chown=user:user requirements.lock .
COPY --chown=user:user pyproject.toml .
COPY --chown=user:user README.md .
RUN /home/$USER/.rye/shims/rye sync --no-dev --no-lock
RUN /home/$USER/.rye/shims/rye run python get_model.py

# Publish port
ENV UVICORN_HOST=0.0.0.0
ENV UVICORN_PORT=8000
EXPOSE 8000:8000

ENV PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:1024

# Enjoy
ENTRYPOINT ["/home/user/.rye/shims/rye", "run"]
CMD ["uvicorn", "main:app"]
