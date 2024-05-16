# Define global args
ARG FUNCTION_DIR="/home/app/"
ARG RUNTIME_VERSION="3.8"

FROM python:${RUNTIME_VERSION}-slim AS python-slim

#RUN apt-get update \
RUN python${RUNTIME_VERSION} -m pip install --upgrade pip
RUN pip install --no-cache-dir torch torchvision --index-url https://download.pytorch.org/whl/cpu 
#&& pip install --no-dependencies --no-cache-dir facenet_pytorch --target ${FUNCTION_DIR}


FROM python-slim AS build-image

# Include global args in this stage of the build
ARG FUNCTION_DIR
ARG RUNTIME_VERSION
# Create function directory
RUN mkdir -p ${FUNCTION_DIR}

# Stage 3 - final runtime image
# Grab a fresh copy of the Python image
FROM python-slim
ARG FUNCTION_DIR
# Set working directory to function root directory
WORKDIR ${FUNCTION_DIR}
COPY --from=build-image ${FUNCTION_DIR} ${FUNCTION_DIR}

RUN python${RUNTIME_VERSION} -m pip install awslambdaric --target ${FUNCTION_DIR}
ADD https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie /usr/bin/aws-lambda-rie
RUN chmod 755 /usr/bin/aws-lambda-rie

# Copy handler function
COPY requirements.txt ${FUNCTION_DIR}

RUN python${RUNTIME_VERSION} -m pip install --no-cache-dir -r requirements.txt


# Copy function code
COPY entry.sh /
COPY handler.py ${FUNCTION_DIR}
COPY data.pt ${FUNCTION_DIR}
COPY models ${FUNCTION_DIR}models
COPY data ${FUNCTION_DIR}data
COPY face_recognition_code.py ${FUNCTION_DIR}

RUN chmod 777 /entry.sh
RUN chmod 777 handler.py

ENTRYPOINT [ "/entry.sh" ]
CMD ["handler.lambda_func"] 