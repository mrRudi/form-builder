FROM python:3.11

# Prevents Python from writing pyc files
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN pip install -U pip setuptools wheel
RUN pip install pdm

WORKDIR /code

COPY pyproject.toml pdm.lock ./
RUN pdm install --prod --no-lock --no-editable

COPY ./app /code/app
EXPOSE ${BACK_APP_PORT}
CMD ["pdm", "run", "uvicorn", "app.main:app", "--host", "${BACK_APP_HOST}", "--port", "${BACK_APP_PORT}"]
