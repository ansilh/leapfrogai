FROM cgr.dev/chainguard/python:3.11-dev

# Set the working directory to /app
WORKDIR /app

# Copy the requirements file into the container at /app
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

ADD models.toml .

ADD simple_ai ./simple_ai

EXPOSE 8080

# Set the command to run the application
ENTRYPOINT [ "python" ]
CMD [ "-m", "simple_ai", "serve",  "--host", "0.0.0.0" ]


