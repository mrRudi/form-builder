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
EXPOSE 8000
CMD ["pdm", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
