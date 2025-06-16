FROM python:3.13-slim

# Set default working directory (important for Dev Containers)
WORKDIR /workspace

# Copy the requirements file
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Default command (optional)
CMD ["bash"]