FROM python:3.12-slim

ENV PYTHONUNBUFFERED=1

# Create a non-privileged user
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/home/appuser" \
    --shell "/sbin/nologin" \
    --uid "${UID}" \
    appuser

# Install system build dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        gcc \
        python3-dev \
        build-essential \
        libffi-dev \
    && rm -rf /var/lib/apt/lists/*

USER appuser

RUN mkdir -p /home/appuser/.cache
RUN chown -R appuser /home/appuser/.cache

WORKDIR /home/appuser

# Install Python dependencies
# NOTE: Do NOT install standalone "bson" — pymongo bundles the correct one.
RUN python -m pip install --user --no-cache-dir \
    "livekit-agents[codecs,images,turn-detector]" \
    livekit-plugins-google \
    livekit-plugins-silero \
    livekit-plugins-noise-cancellation \
    livekit-plugins-bey \
    motor \
    "pymongo[srv]" \
    httpx \
    python-dotenv \
    google-generativeai \
    langchain-google-genai \
    langchain-core \
    langgraph

# Copy all agent code
COPY . .

# Default: run the interviewer agent. Override via AGENT_ENTRY env var.
# Options: AIInterviewer/agent.py, AITutor/agent.py, CareerCounselor/agent.py
ENV AGENT_ENTRY=AIInterviewer/agent.py

CMD python ${AGENT_ENTRY} start
