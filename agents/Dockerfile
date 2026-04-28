FROM python:3.12-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        libffi-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies (LiveKit agents + shared deps)
RUN pip install --no-cache-dir \
    livekit-agents[codecs,images,turn-detector] \
    livekit-plugins-google \
    livekit-plugins-silero \
    livekit-plugins-noise-cancellation \
    livekit-plugins-bey \
    motor \
    pymongo \
    httpx \
    python-dotenv \
    bson \
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
