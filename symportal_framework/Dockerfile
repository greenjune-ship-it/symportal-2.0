# Base image
FROM continuumio/miniconda3

# Set working directory
WORKDIR /app

# Copy environment.yml to the working directory
COPY environment.yml .
# Copy settings_blank.py to settings.py
COPY settings_blank.py settings.py
# Copy sp_config_blank.py to sp_config.py
COPY sp_config_blank.py sp_config.py

# Install Conda dependencies in the base environment
RUN conda env update -n base -f environment.yml && \
    conda init && \
    eval "$(conda shell.bash hook)"

# Generate a secret key
RUN SECRET_KEY=$(base64 /dev/urandom | head -c50) && \
    sed -i "s|SECRET_KEY = ''|SECRET_KEY = '$SECRET_KEY'|g" settings.py

# Modify sp_config.py
RUN sed -i "s/user_name = \"undefined\"/user_name = \"bcchume\"/g" sp_config.py && \
    sed -i "s/user_email = \"undefined\"/user_email = \"bcchume@symportal.org\"/g" sp_config.py


# Copy the rest of the application code
COPY . .

# Run migrations and populate the database
#RUN python manage.py migrate && \
#    python populate_db_ref_seqs.py

# Run your application
CMD tail -f /dev/null