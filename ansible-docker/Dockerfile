# Choose the appropriate base image for ServiceNow MidServer
FROM servicenow/midserver:latest

# Set the working directory
WORKDIR /opt/midserver

# Copy the MidServer application files into the container
COPY ./app /opt/midserver

# Set environment variables for MidServer configuration
ENV MID_INSTANCE_URL=${MID_INSTANCE_URL}
ENV MID_INSTANCE_USERNAME=${MID_INSTANCE_USERNAME}
ENV MID_INSTANCE_PASSWORD=${MID_INSTANCE_PASSWORD}
ENV MID_SERVER_NAME=${MID_SERVER_NAME}

# Expose any necessary ports (if needed)
EXPOSE 8080

# Set the startup command for the MidServer
CMD ["./start_midserver.sh"]
