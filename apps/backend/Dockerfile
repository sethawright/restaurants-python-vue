FROM python:3.11-alpine AS poetry

ENV POETRY_NO_INTERACTION=1 \
  POETRY_VIRTUALENVS_IN_PROJECT=1 \
  POETRY_VIRTUALENVS_CREATE=1 \
  POETRY_CACHE_DIR=/tmp/poetry_cache
RUN apk update && apk upgrade && \
  apk add --no-cache gcc g++ musl-dev curl libffi-dev postgresql-dev zlib-dev jpeg-dev freetype-dev
RUN pip install poetry

WORKDIR /app
COPY pyproject.toml poetry.lock ./
COPY backend ./backend/
RUN poetry export -f requirements.txt --output requirements.txt

FROM python:3.11-alpine AS python-base
WORKDIR /app
COPY --from=poetry /app/requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["./manage.py", "runserver", "0.0.0.0:8000"]