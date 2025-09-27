# uv + Python 3.12 (multi-arch)
FROM ghcr.io/astral-sh/uv:python3.12-bookworm

# Optional extras selector (e.g., "gpu")
ARG UV_PROJECT_EXTRAS=""

# Build context for deps
WORKDIR /app

# Copy lock + manifest first for caching
COPY pyproject.toml uv.lock ./

# Create a minimal dummy package so editable install succeeds
# (We don't need your notebooks here; those are mounted at runtime.)
RUN mkdir -p /app/src/cs_8001_odl_public \
 && printf '__all__ = []\n__version__ = "0.0.0"\n' > /app/src/cs_8001_odl_public/__init__.py

# Install exactly what the lock says; include extras if requested
RUN set -eux; \
    uv sync --frozen --no-cache ${UV_PROJECT_EXTRAS:+--extra "$UV_PROJECT_EXTRAS"}; \
    echo "VIRTUAL_ENV=/app/.venv" >> /etc/environment

# Use the venv by default
ENV VIRTUAL_ENV=/app/.venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# Runtime workdir is your mounted project root
WORKDIR /workspace

EXPOSE 8888