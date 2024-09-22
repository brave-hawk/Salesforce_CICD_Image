# Use salesforce/cli:latest-full as the base image
FROM salesforce/cli:latest-full
 
# Set the maintainer label
LABEL maintainer="brave-hawk"
 
# Install sfdx-git-delta
RUN mkdir -p $HOME/.config/sf
RUN jq -n '["sfdx-templates", "sfdx-git-delta"]' > $HOME/.config/sf/unsignedPluginAllowList.json
RUN sf plugins install sfdx-git-delta
 
# Install SFDX Scanner
RUN sf plugins install @salesforce/sfdx-scanner
 
# Download get-pip.py and install pip
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python get-pip.py && \
    rm get-pip.py
 
# Install yq using pip
RUN pip install yq
 
# Add the entrypoint script
COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh
 
# Set the entrypoint to the script
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
 
# Set the default command to keep the container running with an interactive shell
CMD ["bash"]
